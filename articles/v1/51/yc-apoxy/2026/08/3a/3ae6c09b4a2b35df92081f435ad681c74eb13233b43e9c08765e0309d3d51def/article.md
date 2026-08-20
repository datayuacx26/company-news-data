---
schema_version: "1.0.0"
document_id: "3ae6c09b4a2b35df92081f435ad681c74eb13233b43e9c08765e0309d3d51def"
company_key: "yc-apoxy"
company: "Apoxy"
source_id: "yc-apoxy-rss-ae7e2d86e063"
canonical_url: "https://apoxy.dev/blog/oghttp2-vs-nghttp2"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T23:50:30.734690+00:00"
fetched_at: "2026-08-05T03:48:27.623827+00:00"
content_hash: "sha256:d0a802be51545deb831c230e178b94fe39ed64fa12f9320a23365638767fb169"
---

# A Tale of Two HTTP/2 Codecs

A while back we rolled a routine Envoy upgrade to one of our customers' dedicated proxies. Same config, same traffic, and the CPU graphs stepped up by roughly a fifth. This is the kind of chart that earns you a calendar invite with no agenda, so we decided to get ahead of it and started digging in.


Bisecting versions pointed at Envoy v1.34, which is where Envoy switched its default HTTP/2 codec from nghttp2 to Google's oghttp2. We weren't the first to notice: users had been reporting[15–25% latency regressions](https://github.com/envoyproxy/envoy/issues/40070) since that release, and v1.37.0 eventually[flipped the default back](https://github.com/envoyproxy/envoy/pull/42319) , with a comment in the source promising to try again "once performance aligns with nghttp2." That fixed our customer. It did not fix our curiosity. Why doesn't it align? What does an HTTP/2 codec even spend its time on?


This post is the investigation. There's a sequel coming where we make the fast codec faster still, but first things first.


## The best of codecs, the worst of codecs


Envoy has two implementations of HTTP/2 behind a runtime flag, though nobody could say which was which without a flamegraph:


- **[nghttp2](https://github.com/nghttp2/nghttp2)** - Tatsuhiro Tsujikawa's C library, the HTTP/2 workhorse since 2013 and Envoy's original codec. Lean C structs, caller-provided buffers.
- **oghttp2** - Google's C++ codec from the[QUICHE](https://github.com/google/quiche) family, which shares code with the HTTP/3 stack. It became Envoy's default in v1.34, and that's when the regression reports started.


We benchmarked both on four microarchitectures (Intel Sapphire Rapids, AMD Zen 4, AWS Graviton4, Google Axion). Setup, briefly: same-host loopback (h2load → Envoy → Go h2c backend, or Envoy serving` direct_response` ), every process pinned to disjoint physical cores, Envoy at` --concurrency 1` , so RPS/core is a pure CPU-efficiency number.


The user reports reproduce almost exactly: **nghttp2 beats oghttp2 by 15–25% RPS/core on header-heavy proxied traffic, on every host** , and by 7–19% under heavy connection churn. So the regression is real, portable, and lives somewhere in the codec. Time to open the profiles.


PSA


**If you build Envoy yourself, check your optimization flags.** Envoy's` .bazelrc` does *not* default to` -c opt` - a plain` bazel build //source/exe:envoy-static` produces a fastbuild (debug) binary that looks completely functional and benchmarks **15–32× slower** per core than the same source at` -c opt` . Every number in this post is from` -c opt` builds. We found this out the embarrassing way.


## Where an HTTP/2 codec spends its day


The flamegraphs for both codecs are dominated by the same job: header decompression. Every HTTP/2 request that crosses a proxy arrives with its headers compressed, and the proxy has to undo that - twice per hop, decode and re-encode - before it can route anything. It's a few hundred bytes of work per request, which sounds like nothing until you multiply it out: tens of thousands of requests per second per core, a dozen-plus header fields each, so the per-field decode path runs a few hundred thousand times a second.


The compression scheme is[HPACK (RFC 7541)](https://datatracker.ietf.org/doc/html/rfc7541) : two mechanisms working together. The first is indexing: a fixed 61-entry static table covers the universal headers (` :method: GET` is a single byte), and both sides maintain a synchronized *dynamic table* of recently seen header fields - so the second time you send` cookie: session=abc...` it costs one or two bytes instead of the kilobyte it costs literally. The second mechanism handles everything that *can't* be an index hit: literal strings are Huffman-coded. First-time headers, unique values (request IDs, tokens, the cookie your ad vendor rotates on every request), and most of what crosses a fresh connection goes through the Huffman path. A proxy at the edge sees a lot of fresh connections and a lot of unique values, so this path is hot.


## A two-minute tour of Huffman coding


David A. Huffman


1925–1999


In 1951,[Robert Fano](https://en.wikipedia.org/wiki/Robert_Fano) offered the students in his MIT information theory course a choice: sit the final exam, or write a term paper on a problem he supplied. One of the problems sounded almost too plain to be worth a grade - given a set of symbols and how often each one occurs, find the most efficient binary code for them. Fano didn't mention that he and Claude Shannon (yes, the[Claude Shannon](https://en.wikipedia.org/wiki/Claude_Shannon) ) had both attacked this exact problem and gotten only approximations. A graduate student named[David Huffman](https://en.wikipedia.org/wiki/David_A._Huffman) picked the paper, struggled with it for months, and by his own account had just thrown his notes in the trash to start studying for the exam when the solution surfaced. Everyone before him had built codes from the top down, splitting the symbol set into halves. Huffman inverted it: start from the *bottom* - take the two rarest symbols, merge them into one, repeat until a single tree remains. Rare symbols end up deep in the tree with long codes; frequent symbols end up shallow with short ones. That term-paper construction is provably optimal.1 Seventy-five years later it sits inside JPEG, gzip, MP3 - and every HTTP/2 header on the internet.


Huffman's bottom-up construction - A Tale of Two Cities


FIG. 01


Each node shows its weight: what share of the novel's letters it covers. The merges happen in weight order - first m with i, then that pair with t, then finally e. The 0/1 bits along a leaf's path become its code, shown in bold.


Huffman's construction on a four-letter alphabet, weighted by each letter's real frequency in A Tale of Two Cities.


Frequent letters end up shallow with short codes; rare ones hang deep. And because letters live only at the leaves, no code can be the prefix of another - hover a letter to trace its path from the root. The leaves, top to bottom, spell what a decoder does when it reaches one.


HPACK ships one fixed Huffman code for header text, its frequencies baked into the RFC from real header corpora captured a decade ago, so both ends of a connection always agree on it. Lowercase letters and digits sit near the top of the tree at 5–7 bits; rare bytes hang thirty levels deep. Typical header text comes out around 6 bits per byte. Now notice the property the tree structure gives you for free: no codeword can be the beginning of another, because symbols only live at the leaves. So the bit stream needs no delimiters at all. Read bits until they trace a path to a leaf - that's your symbol. Start again from the root. The stream carries its own boundaries, provided you start at the right place.


It also tells you where the cost lives. Encoding is table lookups and bit-appending. Decoding is that walk: left on 0, right on 1, emit at a leaf, jump back to the root - for HPACK's code, an average of ~6 dependent branch-and-load steps *per output byte* , which is a lot of machinery for one character of a cookie.


Here's the encoding half on a real header value - step through it character by character and watch the codes pack into bytes with no regard for byte boundaries:


Huffman, unpacked - “application/json”


FIG. 02


` accept: “application/json”` 16 chars · 88 bits → 11 B · −31%


1 · Each character maps to its fixed RFC 7541 code - frequent characters get the short ones.


a


00011


5 bits


p


101011


6 bits


p


101011


6 bits


l


101000


6 bits


i


00110


5 bits


c


00100


5 bits


a


00011


5 bits


t


01001


5 bits


i


00110


5 bits


o


00111


5 bits


n


101010


6 bits


/


011000


6 bits


j


1110100


7 bits


s


01000


5 bits


o


00111


5 bits


n


101010


6 bits


2 · Codes are concatenated into one bit stream and cut into bytes - 88 bits is exactly 11 bytes; this string needs no EOS padding.


0


0


0


1


1


1


0


1


0x1d


0


1


1


1


0


1


0


1


0x75


1


1


0


1


0


0


0


0


0xd0


0


1


1


0


0


0


1


0


0x62


0


0


0


0


1


1


0


1


0x0d


0


0


1


0


0


1


1


0


0x26


0


0


1


1


1


1


0


1


0x3d


0


1


0


0


1


1


0


0


0x4c


0


1


1


1


0


1


0


0


0x74


0


1


0


0


0


0


0


1


0x41


1


1


1


0


1


0


1


0


0xea


char 1 / 16


“a”


→


00011


5 bits


One header value under the fixed RFC 7541 code: 16 characters, 88 bits, 11 bytes on the wire.


Step with the arrows (keyboard works too) or hover anything - codes ignore byte boundaries, so one byte often carries pieces of two or three characters. Which is exactly what makes decoding the interesting half of the problem.


nghttp2 does something much better, and has since 2014: it decodes with a precomputed finite-state machine. A decoder mid-stream can only be in finitely many states - one per internal node of the Huffman tree, and HPACK's canonical tree has exactly 256 of them, so the state fits in one byte. You can precompute, for every (state, next-n-bits) pair, where you end up and which symbols (if any) you emit along the way. Decoding becomes: take the next n bits, index a table, maybe write a byte, repeat. No bit-buffer, no branching on code lengths.


The machine below decodes a real header value under the real RFC 7541 code, reading two bits per lookup so the whole state table fits on screen. Step through it - the table and the tree are the same thing wearing different clothes:


Fast prefix decoding - “application/json” at n = 2


FIG. 03


The same 88 bits, read 2 at a time - 44 lookups instead of 88 branch decisions. Click a chunk or step with ←/→ (keyboard works too). Chunks ignore character boundaries; the state carries the overlap. Wider gaps mark byte boundaries.


lookup 1 / 44


⟨start⟩


+


00


→


ε · mid-code


→


⟨00⟩


out: -


The precomputed table - T\[state\]\[chunk\] ⇒ emit + next state · 20 states × 4 chunk values


state \\ bits


00


01


10


11


⟨start⟩


ε


→00


ε


→01


ε


→10


ε


→11


⟨00⟩


∅


ε


→0001


ε


→0010


ε


→0011


⟨0001⟩


∅


∅


a


→0


a


→1


⟨1⟩


∅


ε


→101


∅


ε


→111


⟨101⟩


ε


→10100


ε


→10101


∅


∅


⟨10101⟩


n


→0


n


→1


p


→0


p


→1


⟨10100⟩


l


→0


l


→1


∅


∅


⟨0⟩


ε


→000


ε


→001


ε


→010


ε


→011


⟨001⟩


c


→start


∅


i


→start


o


→start


⟨0010⟩


c


→0


c


→1


∅


∅


⟨000⟩


∅


∅


∅


a


→start


⟨01⟩


ε


→0100


∅


ε


→0110


∅


⟨0100⟩


s


→0


s


→1


t


→0


t


→1


⟨0011⟩


i


→0


i


→1


o


→0


o


→1


⟨011⟩


ε


→01100


∅


∅


∅


⟨01100⟩


/


→0


/


→1


∅


∅


⟨111⟩


∅


ε


→11101


∅


∅


⟨11101⟩


j


→start


∅


∅


∅


⟨10⟩


∅


∅


ε


→1010


∅


⟨1010⟩


l


→start


∅


n


→start


p


→start


States on the tree - squares are this stream's chunk-boundary states; blue is the current lookup's walk


The fast-prefix FSM at n = 2, decoding the header value from FIG. 02. nghttp2 ships the same machine at n = 4.


Table rows cover the states this string actually visits; ∅ marks chunks whose path leaves this string's pruned alphabet. A production HPACK decoder tables the full 257-symbol code the same way - 256 states × 16 nibble values at n = 4, one lookup per half-byte; this is how nghttp2-family decoders do it. Technique after Pajarola, “Fast Prefix Code Processing”.


On the tree, when the walk reaches a leaf mid-chunk it emits that character and hops back to ⟨start⟩ - the dashed arrow - then keeps consuming the remaining bits. Small dots are tree positions a chunk boundary never lands on. Hover a table row to locate its state.


nghttp2 runs exactly this machine with 4-bit chunks: two lookups per input byte into a table of 256 states × 16 nibble values - 16 KiB, which sits comfortably in L1 cache. Validity checking (HPACK requires padding to be all-ones, and an embedded end-of-string symbol is an error) folds into the same lookup. It's a genuinely nice piece of engineering - one reason nghttp2's HPACK path profiles so flat.


## So oghttp2's Huffman decoder must be the problem, right?


That was our assumption going in, and the profiles said no. QUICHE's Huffman kernel is slower than nghttp2's FSM, but the bulk of the difference sits *above* the decoder:` HttpHeaderBlock` and the layers of representation and bookkeeping each decoded header passes through cost a consistent **20–30 µs per request** more than nghttp2's path, on every host we measured. One line item stood out: the decoder emits symbols through` std::string::push_back` , one byte at a time, and that call alone burns **13–20% of all of Envoy's CPU** on the x86 hosts in header-saturated runs.


You can see the shape of it in the source. The decoder's hot loop hands every decoded character to` std::string` individually:


$


quiche/http2/hpack/huffman/hpack_huffman_decoder.cc (condensed)


CPP


` bool


HpackHuffmanDecoder


::


Decode


(


absl


::


string_view


input,


std


::


string


*


output) {


input.


remove_prefix


(bit_buffer_.


AppendBytes


(input));


while


(true) {


if


(bit_buffer_.


count


()


>=


7


) {


// Top 7 bits may hold a complete 5-, 6- or 7-bit code.


uint8_t


short_code


=


bit_buffer_.


value


()


>>


(kHuffmanAccumulatorBitCount


-


7


);


if


(short_code


<


kShortCodeTableSize) {


ShortCodeInfo info


=


kShortCodeTable\[short_code\];


bit_buffer_.


ConsumeBits


(info.length);


output->


push_back


(


static_cast<char>


(info.symbol));


// one byte per call


continue


;


}


}


// ... longer codes take a second path (PrefixToInfo + a canonical-order


// table) that ends in another output->push_back(c), then the loop refills


// bit_buffer_ from input or returns.


}


}


`


Each` push_back` is a capacity check and a possible reallocation per byte of header text. Then, once a value is fully decoded, every header goes through a hash-map lookup plus an arena copy before Envoy ever sees it:


$


quiche/common/http/http_header_block.cc


CPP


` void


HttpHeaderBlock


::


AppendValueOrAddHeader


(


const


absl


::


string_view


key,


const


absl


::


string_view


value) {


value_size_


+=


value.


size


();


auto


iter


=


map_.


find


(key);


if


(iter


==


map_.


end


()) {


AppendHeader


(key, value);


// copies key and value into storage_


return


;


}


value_size_


+=


SeparatorForKey


(key).


size


();


iter->second.


Append


(storage_.


Write


(value));


// another copy, kept as a fragment


}


`


Both are perfectly reasonable C++ in isolation. On a path that runs once per header field - a few hundred thousand times per second per core - the reallocation checks, map lookups, and copies add up to the 20–30 µs the profiles show - while nghttp2's decoder writes into a caller-provided buffer and moves on.


CPU flame graph - oghttp2 decode path


FIG. 04


Stack


all


wrk:worker_0


Event::DispatcherImpl::run


ConnectionImpl::onReadReady \[downstream\]


OgHttp2Adapter::ProcessBytes


HpackDecoderAdapter::HandleControlFrameHeadersData


HpackDecoder::DecodeFragment


HpackHuffmanDecoder::Decode


std::string::push_back


HpackDecoderState::OnHeader


HttpHeaderBlock::AppendValueOrAddHeader


ConnectionManagerImpl::decodeHeaders


Router::Filter::decodeHeaders


UpstreamRequest::encodeHeaders


HpackEncoder::EncodeHeaderBlock


HuffmanEncode


tcp_sendmsg


ConnectionImpl::onReadReady \[upstream\]


OgHttp2Adapter::ProcessBytes


HpackDecoderAdapter::HandleControlFrameHeadersData


HpackDecoder::DecodeFragment


HpackHuffmanDecoder::Decode


std::string::push_back


HpackDecoderState::OnHeader


HttpHeaderBlock::AppendValueOrAddHeader


ConnectionManagerImpl::encodeHeaders


HpackEncoder::EncodeHeaderBlock


HuffmanEncode


tcp_sendmsg


tcp_recvmsg


oghttp2's decode path. Try searching “push_back” - the plumbing above and below the Huffman kernel is where the time goes.


Frame width is total µs/req; darker is hotter. Click a frame to zoom in, click the stack trail to zoom back out, hover for detail, search to highlight matching frames.


To be fair, oghttp2's framing and session layer is *cheaper* than nghttp2's - in the one scenario where header literals stop dominating (realistic headers, direct response, no proxying), oghttp2 wins by 8–13%. The codec isn't slow everywhere. But a proxy's job description is header-heavy, and on header-heavy traffic the plumbing tax buries the framing win.


The fix upstream needs is a leaner path from decoded bytes to header block, which is a deeper refactor than any single hot loop. That's presumably why the Envoy default got flipped back instead of forward.


## The fix


For us and our customers, the fix was pleasantly boring:


- **Envoy 1.37+:** you're done; nghttp2 is the default again.
- **Envoy 1.34–1.36:** set` envoy.reloadable_features.http2_use_oghttp2: false` . It's one line of runtime config and it's worth 15–25% RPS/core on most proxied HTTP/2 traffic.


We rolled the flag across our fleet, the customer's graphs stepped back down, and the calendar invite never came. It is a far, far better thing to flip a runtime flag than to refactor a header pipeline.


## Conclusions


**Codec defaults are performance changes.** A minor-version Envoy bump changed our customer's CPU bill by 20% without touching a single byte on the wire. If you operate proxies, benchmark the versions you ship, not the versions you read about.


**Profile before you optimize.** We went in certain the Huffman decoder was the villain, and the flamegraphs pointed at string plumbing instead. The decoder was, if anything, the best part of both codecs.


**Header decompression is a real workload.** A few hundred bytes per request, a dozen header fields each, tens of thousands of requests per second per core, twice per hop - the arithmetic stops being funny quickly.


While we were in there, though, we kept staring at nghttp2's n=4 table and at a 2017 paper by the nghttp2 author and friends that says you can read a whole byte at a time - if you're willing to pay for a table sixteen times larger. Whether that trade survives contact with a real CPU cache is the next post. Stay tuned.


## References


1. **Minimum-redundancy codes** - David A. Huffman,["A Method for the Construction of Minimum-Redundancy Codes"](https://doi.org/10.1109/JRPROC.1952.273898) , *Proceedings of the IRE* , vol. 40, no. 9, pp. 1098-1101, September 1952. The term paper itself, optimality proof included.


Dmitry Ilyevsky


Co-founder & CTO


Dmitry is co-founder and CTO of Apoxy. He previously built and operated infrastructure at Google, Cruise, and Mux (where he and Matt met).


[GitHub ↗](https://github.com/dilyevsky)


[← Back to all posts](https://apoxy.dev/blog)
