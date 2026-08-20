---
schema_version: "1.0.0"
document_id: "48ae9b6c25ab2ccb3d6fefe108a20973ab63004ca283157dd6c27744c519d775"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/autonomously-finding-7-ffmpeg-vulnerabilities-with-ai-2025"
published_at: "2025-12-02T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T22:25:06.703683+00:00"
content_hash: "sha256:5a5dc2cef0dc0d2a836a2e599f805a37f8f3d1b3dd3f1da8934b60f8af8abadc"
---

# Autonomously Finding 7 FFmpeg Vulnerabilities With AI

# Autonomously Finding 7 FFmpeg Vulnerabilities With AI


## Introduction


ZeroPath's AI-assisted SAST analyzed FFmpeg's protocol handlers, parsers, filters, and Android glue code, and reported seven distinct memory safety flaws. Each issue stems from a mismatch between what the developers intended to guarantee and what the code actually does. This is precisely the class of issues that traditional SASTs miss: subtle, logically-flawed code, that cannot be detected with simple pattern matching.


Below we document each case, and some guesses as to why conventional tools failed to pick them up. We also include the original ZeroPath report, to give an idea of how our tool explains the issues it finds.


Due to lack of space in this blog post, we will not be including our verbose, targeted source-to-sink tracker output, which also visually describes the pathway of how each bug occurs; nor will we be demonstrating our AI-based patch generation which autonomously creates patches for issues that are detected. If you're interested in checking those out, register for a free account at ZeroPath and try it out; otherwise, we'll have another post pretty soon showing how this works!


If you're interested in learning about how ZeroPath actually works, take a look at[our explainer](https://zeropath.com/blog/how-zeropath-works) , which outlines some of our architectural decisions in creating ZeroPath.


All of these bugs have been patched by the FFmpeg team, and we thank them for their hard work and dedication when it comes to their codebase and security.


---


## 1) Android MediaCodec audio: Buffer overflow due to a truncated sample count and full-size copy


> mediacodec_wrap_sw_audio_buffer() computes frame->nb_samples by dividing info->size by (sample_size * channels), allocates buffers via ff_get_buffer() based on that truncated nb_samples, then memcpy()s info->size bytes into the allocated buffer. If info->size is not an exact multiple of (sample_size * channels) the memcpy will write up to (unit - 1) bytes past the allocation, causing a heap buffer overflow. The overflow is reachable from attacker-provided media sent through Android MediaCodec APIs.


This buffer overflow exists in ffmpeg's Android MediaCodec player, in the` mediacodec_wrap_sw_audio_buffer` function, making it a juicy target for Android devices (and really, Android devices only):


```text
static     int     mediacodec_wrap_sw_audio_buffer  (  AVCodecContext   *  avctx  ,
MediaCodecDecContext   *  s  ,
uint8_t     *  data  ,
size_t   size  ,
ssize_t   index  ,
FFAMediaCodecBufferInfo   *  info  ,
AVFrame   *  frame  )
{
int   ret   =     0  ;
int   status   =     0  ;
const     int   sample_size   =     av_get_bytes_per_sample  (  avctx  ->  sample_fmt  )  ;
if     (  !  sample_size  )     {
av_log  (  avctx  ,   AV_LOG_ERROR  ,     "Could not get bytes per sample\n"  )  ;
ret   =     AVERROR  (  ENOSYS  )  ;
goto   done  ;
}


frame  ->  format   =   avctx  ->  sample_fmt  ;
frame  ->  sample_rate   =   avctx  ->  sample_rate  ;
frame  ->  nb_samples   =   info  ->  size   /     (  sample_size   *   avctx  ->  ch_layout  .  nb_channels  )  ;
[  .  .  ]
ret   =     ff_get_buffer  (  avctx  ,   frame  ,     0  )  ;     // allocates frame->data[0] based on frame->nb_samples
[  .  .  ]
memcpy  (  frame  ->  data  [  0  ]  ,   data  ,   info  ->  size  )  ;
```


The problem with the above code was that the allocation of memory for the` frame->data\[0\]` buffer was performed with the assumption that` frame->nb_samples` would directly correspond to` info->size` . However in reality, this calculation:


```text
info->size / (sample_size * avctx->ch_layout.nb_channels)


```


may produce a too-small number, if` info.size` was not a multiple of` sample_size * channels` , due to integer truncation. For example,` 25/10=2.5` , which would allocate only` 20` bytes of data, but the` memcpy()` call would pass` info->size = 25` , producing a heap buffer overflow.


**Why typical tools missed it**


- Reaching this wrapper requires Android MediaCodec plumbing and realistic buffer sizes.
- Division-based truncation does not look suspicious locally; the copy uses a variable that appears "validated” by earlier code.


**Why our AI SAST flagged it** The analyzer built the unit relation` frame_unit = sample_size * channels` , inferred the contract` alloc_bytes >= nb_samples * frame_unit` , and compared it to the later` copy_bytes = info.size` . It then searched for` info.size = k * frame_unit + r` with` r > 0` , which violates the equality that the code implicitly relies on.


The patch for this issue is[here](https://code.ffmpeg.org/FFmpeg/FFmpeg/pulls/20810) . The fix involves ensuring that no integer truncation has occured in the sample and frame allocation calculations.


---


## 2) RTMP client: Buffer overflow due to unbounded AMF serialization from attacker-controlled parameters


> The RTMP client code (gen_connect) allocates a fixed-size packet buffer and writes arbitrary AMF fields derived from the rt->conn string into that buffer without checking remaining capacity. An attacker-controlled rtmp_conn can be tokenized and serialized into the packet, overflowing the allocated buffer and causing heap corruption/crashes.


This bug exists in the Real-Time Messaging Protocol (RTMP) code of FFmpeg, This issue was quite basic, and a bit surprising that it wasn't found before. It mostly stemmed from a packet which was built with a local parameter, which simply overflows a heap buffer when constructing a network packet. This isn't the most exciting vulnerability due to the requirement to control a local parameter when ffmpeg is called, but it could be easily reproduced with the following:


```text
ffmpeg -v debug -nostdin -re -f lavfi -i   anullsrc  =  r  =  44100  :cl  =  mono   \
-f flv -rtmp_conn   $(  perl -e   'print "4" x 7000;'  )     "rtmp://127.0.0.1:1935/live/stream"
```


**Why typical tools missed it**


- Most fuzz harnesses treat RTMP as a network transport and do not drive the client-side packet builder.
- The writes are split across helpers, each of which seems locally correct; capacity is not rechecked at the call site.


**Why our AI SAST flagged it** The analyzer inferred an *implicit maximum encoded size* contract from neighboring builders and observed that the caller did not maintain a decrementing "remaining” counter. It then symbolically encoded a long` conn` list and demonstrated that the cumulative writes exceed the buffer.


The patch for this issue is[here](https://code.ffmpeg.org/FFmpeg/FFmpeg/pulls/20796) .


---


## 3) ICY metadata: Off-by-one NUL on the stack


> In store_icy(), the code reads ch16 bytes of metadata into a local buffer of size (25516 + 1) and then writes a NUL byte at data\[len + 1\] instead of at data\[len\]. For the maximum allowed ch (0..255) this results in a write one past the intended terminator. This off-by-one write can corrupt adjacent stack state, leading to crashes or memory corruption and potentially enabling exploit primitives (denial-of-service or code execution) when processing crafted remote ICY metadata.


Icecast (ICY) is an oldschool protocol that is built on top of HTTP to retrieve internet radio streams and play them. In this vulnerability's case, metadata retrieved from an external server could write past the stack buffer, due to a simple off-by-one bug. The bug is quite obvious, and it's again quite surprising it hasn't been found before:


```text
static     int     store_icy  (  URLContext   *  h  ,     int   size  )


[  .  .  ]
if     (  len   <     0  )
return   len  ;
if     (  ch   >     0  )     {
char   data  [  255     *     16     +     1  ]  ;
int   ret  ;
len   =   ch   *     16  ;
ret   =     http_read_stream_all  (  h  ,   data  ,   len  )  ;
if     (  ret   <     0  )
return   ret  ;
data  [  len  +  1  ]     =     0  ;
[  .  .  ]
```


While` http_read_stream_all()` rejected a too-large` len` of anything bigger than` 255 * 16 + 1` , it did not account for the fact that right on the edge-case of` len = 255 * 16 + 1` , the null-pointer may be written out-of-bounds later on.


**Why typical tools missed it**


- Many test inputs never reach the extreme value where` len` equals the maximum; fuzzers bias toward varied content rather than precise boundary multiples.
- The pattern resembles typical safe termination and passes superficial checks.


**Why our AI SAST flagged it** The analyzer tracked` len` as` ch * 16` and proved that for` ch = 255` the write index equals the array bound, violating the buffer's exact size by one.


The patch for this issue is[here](https://code.ffmpeg.org/FFmpeg/FFmpeg/pulls/20804) . The patch was to simply set` data\[len\] = 0;` instead of using` len+1` .


---


## 4) RTP RFC4175 uncompressed video: Buffer overflow due to integer overflow in offset arithmetic


> In rfc4175_handle_packet(), the code computes copy_offset from attacker-controlled line and offset values extracted from RTP packet headers, then checks if copy_offset + length > data->frame_size before calling memcpy. However, for large video dimensions, the intermediate calculation (line * data->width + offset) * data->pgroup / data->xinc can overflow a signed 32-bit integer, wrapping to a negative value. This causes the bounds check to pass incorrectly and allows a heap buffer overflow via crafted RTP packets, enabling remote code execution or denial of service.


This bug, in the RFC4175 Real-time Transport Protocol (RTP) raw video parser, is another classic buffer overflow caused by an integer overflow. This time, it's an externally-driven integer (RTP is used for delivering audio/video over IP networks), allowing a remote exploit. The bug was in the` rfc4175_handle_packet()` function, which did something along the lines of this:


```text
line   =     (  (  headers  [  2  ]     &     0x7f  )     <<     8  )     |   headers  [  3  ]  ;
offset   =     (  (  headers  [  4  ]     &     0x7f  )     <<     8  )     |   headers  [  5  ]  ;
cont   =   headers  [  4  ]     &     0x80  ;
headers   +=     6  ;
data  ->  field   =   field  ;


if     (  !  data  ->  pgroup   ||   length   %   data  ->  pgroup  )
return   AVERROR_INVALIDDATA  ;


if     (  length   >   payload_len  )
length   =   payload_len  ;


if     (  data  ->  interlaced  )
line   =     2     *   line   +   field  ;


/* prevent ill-formed packets to write after buffer's end */
copy_offset   =     (  line   *   data  ->  width   +   offset  )     *   data  ->  pgroup   /   data  ->  xinc  ;
if     (  copy_offset   +   length   >   data  ->  frame_size   ||     !  data  ->  frame  )
return   AVERROR_INVALIDDATA  ;


dest   =   data  ->  frame   +   copy_offset  ;
memcpy  (  dest  ,   payload  ,   length  )  ;
```


The issue stemmed from the fact that the calculation of the correct offset of the buffer (` data->frame + copy_offset` ) did not account for massive numerical values of the height of a stream's frame, which could result in the incorrect calculation of` line` , or` copy_offset` , which resulted in the inequality` copy_offset + length > data->frame_size` being false, due to` copy_offset + length` being false when the integer wrapped around to a negative due to truncation.


Even though there were protections in place to prevent ill-formed packets from causing a buffer overflow, the protection was insufficient, as our SAST calculated.


**Why typical tools missed it**


- Using RFC4175 requires network-driven streams and valid packet sequences.


**Why our AI SAST flagged it** The analyzer normalized the arithmetic to a 64-bit model, derived upper bounds from legal video parameters, and produced cases where intermediate additions exceed` INT_MAX` while remaining plausible for modern resolutions.


This patch for this issue is[here](https://code.ffmpeg.org/FFmpeg/FFmpeg/pulls/20803) . The patch was as simple as:


```text
if     (  line   >=   data  ->  height  )
return   AVERROR_INVALIDDATA  ;
```


---


## 5) drawtext with detection bboxes: Buffer overwrite due to separator budgeting error in string build


> The drawtext filter concatenates detection bbox labels into a heap-allocated buffer without accounting for separator overhead. When text_source=AV_FRAME_DATA_DETECTION_BBOXES, it allocates s->text to AV_DETECTION_BBOX_LABEL_NAME_MAX_SIZE*(AV_NUM_DETECTION_BBOX_CLASSIFY+1) (320 bytes). Later, it copies the detect_label and up to AV_NUM_DETECTION_BBOX_CLASSIFY classify_labels with ", " separators using strcpy/strcat, which can exceed the allocated size (e.g., 64 + 4*(2+64) = 328) and overflow s->text, leading to heap memory corruption.


FFmpeg's` drawtext` filter is designed to display text over video, and it can take input from a few different sources. When` text_source` is set to detection bounding boxes (which are used in object detection, like detecting and labeling things in a video frame), the filter works by concatenating labels that describe the detected objects into a single text string. To fit the text string into memory, the memory was allocated as such:


```text
s  ->  text   =     av_mallocz  (  AV_DETECTION_BBOX_LABEL_NAME_MAX_SIZE   *     (  AV_NUM_DETECTION_BBOX_CLASSIFY   +     1  )  )  ;
```


This, however, was insufficient for allocation, as the text string is a concatenation of the text and` ", "` , for each label.


**Why typical tools missed it**


- Most harnesses do not populate detection metadata in frames.
- String handling with` strcpy` and` strcat` appears benign for typical label sizes and silently becomes unsafe when all labels are at maximum length.


**Why AI SAST flagged it** The analyzer computed a worst-case label vector of length bounds and added separator overhead. If up to` k` labels can be concatenated, the allocation must include` sum(label_lengths) + 2 * (k - 1) + 1` for separators and terminator. The code budgeted only` k * max_label + 1` . It then compared the exact bound to the allocated size and marked the overflow when all labels hit their maxima.


This patch for this issue is[here](https://code.ffmpeg.org/FFmpeg/FFmpeg/pulls/20811) . The allocation simply changed to:


```text
s  ->  text   =     av_mallocz  (  (  AV_DETECTION_BBOX_LABEL_NAME_MAX_SIZE   +     1  )     *  (  AV_NUM_DETECTION_BBOX_CLASSIFY   +     1  )  )  ;
```


---


## 6) WHIP muxer: invalid free due to incorrect stream index and freeing an invalid pointer


> In create_rtp_muxer (libavformat/whip.c), for each input stream the code creates an rtp_ctx and a single stream inside it (index 0). Later it incorrectly accesses rtp_ctx->streams\[i\]->codecpar (using the loop index i) when it should access rtp_ctx->streams\[0\]. For s->nb_streams > 1 this references out-of-bounds memory and then calls av_freep() on that pointer, leading to memory corruption or a crash (Denial of Service) during WHIP initialization.


FFmpeg's WebRTC-HTTP Ingestion Protocol (WHIP) muxer had a simple logical flaw in its muxing code when using the H264 codec, resulting in an invalid free of an out-of-bounds pointer, during connection setup.


Previously, the muxer code did something like this:


```text
for     (  i   =     0  ;   i   <   s  ->  nb_streams  ;   i  ++  )     {
[  .  .  ]
/**
* For H.264, consistently utilize the annexb format through the Bitstream Filter (BSF);
* therefore, we deactivate the extradata detection for the RTP muxer.
*/
if     (  s  ->  streams  [  i  ]  ->  codecpar  ->  codec_id   ==   AV_CODEC_ID_H264  )     {
av_freep  (  &  rtp_ctx  ->  streams  [  i  ]  ->  codecpar  ->  extradata  )  ;
rtp_ctx  ->  streams  [  i  ]  ->  codecpar  ->  extradata_size   =     0  ;
}
```


To cut a long story short, in WHIP and H264, the *inner* RTP context (` rtp_ctx` ) *always* holds exactly one stream at index zero, regardless of how many streams the packet contains. If a packet contained multiple streams, the loop would attempt to read (and possibly free) the invalid pointer,` rtp_ctx->streams\[i\]` where` i >= 1` . This will simply result in a crash on most systems.


**Why typical tools missed it**


- Muxer inits are exercised far less than demuxers in typical fuzz targets.
- Many test pipelines only mux a single stream, which masks the out-of-bounds access.


**Why AI SAST flagged it**


The analyzer tracked cardinality from the constructor to the use site. It saw a single-element container on the producer path and an index that comes from an unrelated loop on the consumer path, then proved the index can exceed zero for multi-stream inputs.


The patch for this issue is[here](https://code.ffmpeg.org/FFmpeg/FFmpeg/pulls/20805) . It's nothing complicated at all:


```text
-              av_freep(&rtp_ctx->streams[i]->codecpar->extradata);
-              rtp_ctx->streams[i]->codecpar->extradata_size = 0;
+              av_freep(&rtp_ctx->streams[0]->codecpar->extradata);
+              rtp_ctx->streams[0]->codecpar->extradata_size = 0;
```


---


## 7) SCTP write: buffer underflow due to caller-controlled header consumption without length guard


> sctp_write forwards caller-controlled buffer and length directly to the socket. When s->max_streams is set there is no check that the provided size is >= 2 before reading the first two bytes (AV_RB16(buf)) and calling ff_sctp_send(buf + 2, size - 2, ...). This can result in out-of-bounds reads, integer-to-size_t conversion of negative lengths, crashes, and potentially disclosure of process memory to the network if an attacker can control buf and size. The stream id is taken directly from the buffer and only checked with a > comparison against max_streams, which may allow incorrect stream selection if semantics are mismatched.


In another logical flaw which depended on specific flags being set in the call-path, FFmpeg's Stream Control Transmission Protocol (SCTP) handler contained a logic flaw when writing a buffer to a socket, leading to a massive out-of-bounds reads due to the incorrect calculation of the size of a buffer and integer overflow.


The` sctp_write()` function did the following:


```text
if     (  s  ->  max_streams  )     {
/*StreamId is introduced as a 2byte code into the stream*/
struct     sctp_sndrcvinfo   info   =     {     0     }  ;
info  .  sinfo_stream             =     AV_RB16  (  buf  )  ;
if     (  info  .  sinfo_stream   >   s  ->  max_streams  )     {
av_log  (  h  ,   AV_LOG_ERROR  ,     "bad input data\n"  )  ;
return   AVERROR_BUG  ;
}
ret   =     ff_sctp_send  (  s  ->  fd  ,   buf   +     2  ,   size   -     2  ,     &  info  ,   MSG_EOR  )  ;
}     else
ret   =     send  (  s  ->  fd  ,   buf  ,   size  ,   MSG_NOSIGNAL  )  ;
```


This code reads a 16-bit stream id from the start of the buffer and forwards` buffer + 2` with` size - 2` . This is problematic, because there is no actual check requiring` size >= 2` , meaning, for example,` size - 2` as is passed to` ff_sctp_send()` , may be equal to` -2` .` ff_sctp_send()` 's second parameter is a` size_t` type, meaning` -2` underflows to a massive number, resulting in a massive amount of memory being sent and disclosed over the socket.


Other similar code correctly, before consuming an inline header, validated that the input length satisfied the header size and that the remaining payload length stayed nonnegative after subtraction.


**Why typical tools missed it**


- SCTP is rarely enabled in default builds and seldom exercised in fuzz setups.
- SCTP is network-orientated and this vulnerability relies on both receiving and sending packets.


**Why AI SAST flagged it** The analyzer treats "consume header then forward remainder” as a standard pattern. It generated a minimal counterexample` size = 0 or 1` , established that` size - 2` becomes negative, and followed the value through conversion sites and subsequent reads.


The patch for this issue is[here](https://code.ffmpeg.org/FFmpeg/FFmpeg/pulls/20809) , and was simple enough:


```text
+++ b/libavformat/sctp.c
@@ -332,6 +332,9 @@ static int sctp_write(URLContext *h, const uint8_t *buf, int size)
}


if (s->max_streams) {
+          if (size < 2)
+              return AVERROR(EINVAL);
[..]
```


---


## How ZeroPath's AI SAST reasons about intent


Across these seven cases the engine applied a small set of reusable intent models:


-


**Allocation and copy alignment.** If an allocation is computed from a derived unit, any later copy must be bounded by the same unit relation. The system symbolically searches for residues and rounding effects that violate the equality.


-


**Framing invariants.** When code constructs "header then payload” blocks, the header length must be less than or equal to the block size. The analyzer confirms this with linear constraints and examines both signed and unsigned interpretations.


-


**Packet builder capacities.** For encoders that write variable-length fields into fixed buffers, the engine checks that every write decrements a shared capacity variable or passes through a helper that enforces capacity. Missing decrements are treated as violations of an inferred contract.


-


**Cardinality propagation.** If a function creates exactly one stream or exactly N entries, downstream code must not index beyond that cardinality. The engine follows these facts interprocedurally and flags mismatches at use sites.


-


**Offset arithmetic integrity.** When writing into frame buffers, the analyzer lifts index math into a wide-integer domain, then proves whether offsets fit into the destination bounds for the legal domain of widths, heights, strides, and pixel sizes.


This blend of symbolic execution, unit reasoning, and contract inference is what distinguishes AI-powered SAST from rule-matching and pure coverage exploration. Rather than waiting for a random input to stumble into a failure, it proves the existence of violating inputs and gives you the exact shape of those inputs.


---


## Why the usual tools missed these vulnerabilities


### Fuzzers


Modern fuzzers excel at exercising decoders that accept single-file inputs. Several of these bugs require:


- environment-gated paths such as Android MediaCodec, WHIP initialization, or SCTP;
- protocol handshakes and multi-packet state (RTMP, RTP RFC4175);
- metadata that must be injected into` AVFrame` side data rather than the media bitstream (drawtext detection labels).


Fuzzing harnesses typically stop at demux or decoder entry points with file inputs and do not emulate network sessions, signaling, or platform frameworks. That leaves entire classes of packet builders, mux inits, and filter paths under-exercised.


### Traditional static analysis


Rule packs catch direct misuse patterns, but frequently miss cross-function resource contracts such as "derive allocation from A, then copy size B that was not derived from A,” or "read a header field that semantically bounds a later write, but never validate the relation.” These checks require reasoning about *intent* and *units* across call chains, not just local anti-patterns.


## ZeroPath's AI SAST


Our analyzer constructs *semantic contracts* from code structure, sibling functions, and common protocol idioms, then proves or refutes them along real call graphs. When a developer clearly intended two quantities to align — for example, a byte count used for allocation and the same count used for copy — the engine treats that as an invariant and searches for counterexamples, including integer truncation, alignment rounding, or unit mismatches.


If you've enjoyed taking a look at these vulnerabilities, how they were discovered by logical reasoning by an AI SAST, and their technical details, don't forget to check back soon, as we'll be documenting a bunch of similar vulnerabilities we've discovered in the Linux kernel, too.
