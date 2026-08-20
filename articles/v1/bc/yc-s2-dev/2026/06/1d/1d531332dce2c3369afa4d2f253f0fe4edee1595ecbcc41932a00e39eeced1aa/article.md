---
schema_version: "1.0.0"
document_id: "1d531332dce2c3369afa4d2f253f0fe4edee1595ecbcc41932a00e39eeced1aa"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/local-ai"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:c3519434a6372711f2db76f5ec6f0a49d284455965209010e3ea359775a34d3a"
---

# Serving Local AI on my Jetson through Durable Streams

With local AI feeling more and more practical, I wanted to self-host my own models and run my workloads independently without any third-party provider in the mix, and also look into serving my local model to some users reliably. The Jetson series by NVIDIA is a great starting point, and I went with the[Jetson Orin Nano Super kit](https://developer.nvidia.com/embedded/jetson-developer-kits) , aka “The most affordable generative AI supercomputer”! It has` 1024 CUDA cores` and` 32 tensor cores` and is rated at` 67 TOPS` (trillion operations per second), which should be good enough for my little experiment which is a small text-to-speech app powered by[Kokoro-82M](https://github.com/hexgrad/kokoro) , a neural text-to-speech model.


It is mostly inspired out of need that I don't want to always read a lot of text, but would rather hear it. So I want something where I select some text, pick a voice, and get a link which I can come back to later or share with people. For now that means pasting text into a page, but I'd want something even more lazy-proof eventually which would be a nicer frontend on top of the same core app. Beyond the app itself, I want to land on a small reference architecture for local inference: a self-contained serving layer that exposes a clean API, so the same setup can back a web app, a CLI, or another service without rework.


Try it out at[streamtts.dev](https://streamtts.dev/) (It is self-hosted on my Jetson! 😉):


## Not a normal Request/Response API


The simplest way to architect this would be:


```text
POST /generate
wait
return audio.mp3
```


Inference is slower than a normal web request. Kokoro on this Jetson can produce speech faster than realtime, but it is still a GPU job. A minute of audio can take many seconds of compute. A cold first sentence can be slower while the model stack warms up. If multiple users submit at once, a blocking request turns into a line of sockets waiting on the GPU.


The output is also naturally incremental. TTS does not need to finish the entire paragraph before the listener hears anything. The model can generate one sentence, encode that sentence to MP3, append it somewhere, and move on. If I force the whole thing into a single response body, I throw away the best property of the workload.


And I want the result to be shareable. The user should be directed to a link immediately where they can "await" the model to produce all the bytes. If they open it while the Jetson is still working, they should hear the prefix and then follow the live edge.


If we start with request-response, we end up adding a pile of infrastructure like:


- queue
- database for job bookkeeping
- object storage for the finished file
- retry logic
- dedupe logic
- cleanup process


All of this is reasonable. But together, it is a lot for one basic promise:


```text
accept work now
produce output later
let readers follow along
```


The request feels like the wrong lifetime for this. I want the inference job to work seamlessly across network disruptions. I also do not want a dropped browser tab to kill a running generation. Thus the output should have an identity before it is complete, and readers should be able to start at the beginning, catch up to the tail, or come back later and replay the same bytes!


In summary, I want:


```text
submit work
get an output stream immediately
worker appends model output
client awaits the stream
```


All of this can be cleanly abstracted over durable streams. A stream is an ordered sequence of records, where a record is just some bytes (here, a chunk of audio plus a little metadata). Durable means every record is persisted, so nothing is lost and a reader can come back later and replay the exact same bytes. Putting the two together, we get a simple but powerful building block.


Append records to the tail, and readers can start at the head, seek to a known sequence number, or sit at the tail and wait for the next record to arrive. A stream store gives you named timelines:


```text
APPEND record
READ from seq_num=N
TAIL for live records
```


Each record is the unit of progress. A record has a sequence number, timestamp, headers, and a body. StreamTTS does not need much more structure than that. We represent records like so:


```text
headers:
e: audio
i: 3
d: 4210
t: "sentence text"
body:
<raw mp3 bytes>
# e = event type
# i = index
# d = duration (ms)
# t = sentence text
```


And the output will be shaped like:


```text
pub/casts/4LwnHZDl_vFC
seq 0  meta
seq 1  start
seq 2  audio sentence 0
seq 3  audio sentence 1
seq 4  audio sentence 2
seq 5  eos              # end of stream
```


That stream is the audio file, the live feed, the replay log, and the progress indicator. It is also the contract between the web server, the GPU worker, and every browser that opens the link. The writer does not need to know who is listening. The reader does not need to know whether the writer is still alive. Both sides just agree on one named sequence of records.


Connection-only SSE or WebSockets are great for live delivery, but they do not give you durable replay by themselves. They move bytes to clients that are currently connected. They do not, on their own, remember the bytes for clients that arrive late, disconnect, or refresh the page. So if nobody is connected, there is nowhere durable for a websocket message to go. If a client drops, the server needs some other store to remember what that client missed. If a second listener opens the same link while generation is still running, the websocket connection does not tell the server how to replay the beginning and then follow the live edge. You can absolutely solve this by putting a database or object store next to SSE/WebSockets. But now live delivery and replay are two separate pieces that have to agree.


With a durable stream, that split can be unified! The worker appends output once and a live listener tails the stream. A late listener can read from` seq_num=0` and then tails the same stream. Replay and live playback are the same read path, just starting from different offsets.


## S2-Lite


S2 Lite is an open source self-hosted, single-binary implementation of the S2 durable streams API. In this setup, it runs on localhost with local disk for durable storage and gives me streams with append, read, tail, and long-polling semantics.


```text


s2   lite   --local-root   var/s2lite-data   --port   4002   --no-cors


```


We start by creating a basin, which acts as a namespace, and model the whole service as a handful of named streams. The arrows below show which component appends to each stream and which reads from it:


A few streams are shared across all casts:


- ` jobs` is the intake log: one record per inference request
- ` jobs/_cursor` holds the worker's committed read offset into` jobs`
- ` jobs/dead` collects jobs that failed past retries
- ` progress/done` gets one receipt per completed cast


And each cast adds two streams of its own:


- ` catalog/<id>` is the private recipe: full text, voice, title, created time
- ` pub/casts/<id>` is the public output stream: meta, start, audio..., eos


```text
s2   =   S2(
os.environ.get(  "S2_ACCESS_TOKEN"  ,   "local-token"  ),
endpoints  =  Endpoints(
account  =  lite_url,
basin  =  lite_url,
),
)


config   =   BasinConfig(
default_stream_config  =  StreamConfig(
storage_class  =  StorageClass.  EXPRESS  ,
retention_policy  =  RETENTION_SECS  ,
)
)
await   s2.ensure_basin(basin,   config  =  config)
```


Each` audio` record carries the sentence text and duration in milliseconds in its headers, and the raw MP3 bytes in its body. The text gives the browser captions and seek points. The duration lets the player schedule chunks. The browser always starts tailing at` seq_num=0` .


If the stream is complete, the browser reads through` eos` and stops. If the worker is still appending, the browser reads the existing prefix, reaches the tail, and waits for the next record. The browser player is also built around the stream shape. It does not use Media Source Extensions or build one growing MP3 file. Each` audio` record is a complete sentence-sized MP3 chunk. The browser receives each sentence-sized MP3 chunk, decodes it with the[Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) , and places it on a virtual timeline.


## Fair Scheduling


A single Jetson can’t behave like an elastic inference cluster 😅. If lets say three people submit text, I do not want the first long paragraph to finish completely while everyone else waits. The worker keeps several casts active and tracks how far ahead each stream is relative to wall-clock playback:


```text
def   lead  (self) ->   float  :
return   self  .total_ms   /   1000.0   -   (time.monotonic()   -   self  .started)
```


Positive lead means the stream has generated audio buffered ahead of playback. Negative lead means the listener is catching up to the live tail.


The scheduling loop is:


```text
admit jobs up to the concurrency cap
pick the active stream with the lowest lead
generate exactly one sentence for it
append that sentence
recompute lead
repeat
```


When every active stream is comfortably ahead, the worker sleeps for a tiny bit instead of sprinting one stream to completion creating live-output scheduling. The goal is to keep multiple public streams playable. The unit of fairness is not a request, but one appended sentence.


## Submitting Work


When a request comes in, the web process does not load the model. It validates the text and voice, computes a deterministic id, and creates a place where audio will appear.


The id is content-addressed:


```text
def   content_id  (text:   str  , voice:   str  ) ->   str  :
h   =   hashlib.sha256(  f  "  {  voice  }\x00{  text.strip()  }  "  .encode()).digest()
return   base64.urlsafe_b64encode(h).decode().rstrip(  "="  )[:  12  ]
```


Identical text with the same voice maps to the same stream. That turns repeated submissions into cache hits.


The write path is:


1. claim` catalog/<id>` with the full recipe
2. claim` pub/casts/<id>` with a meta record
3. append one job to the` jobs` stream
4. return` /c/<id>`


The important operation is the claim. S2 supports conditional append with` match_seq_num` . StreamTTS uses` match_seq_num=0` , which means "only append if this stream is empty."


```text
payload   =   {
"records"  : [{  "body"  : json.dumps(body,   separators  =  (  ","  ,   ":"  ))}],
"match_seq_num"  :   0  ,
}
```


If two people submit the same text at the same time, exactly one request wins the claim and enqueues the job. The other gets the same link and tails the same output stream.


That one append replaces a lock table, a uniqueness constraint, and a dedupe cache.


## The Worker is a Durable Consumer


The worker is the only process that owns the model and touches the GPU. It reads from the` jobs` stream, runs Kokoro-82M, and appends audio records to the cast stream.


On startup, the worker reads the last committed offset from` jobs/_cursor` :


```text
jobs/_cursor
{"offset": 123}
```


Then it reads` jobs` starting from that offset. If there is nothing new, it long-polls at the tail.


The subtle part is committing the cursor. StreamTTS can have several active casts at once, and they do not necessarily finish in job order. A short job 10 can finish before a long job 7. The cursor can only move forward when every job up to that point has finished.


The worker uses a contiguous-done watermark:


```text
def   advance_watermark  ():
nonlocal   committed
moved   =   False
while   committed   in   done_above:
done_above.discard(committed)
committed   +=   1
moved   =   True
if   moved:
self  ._commit_offset(committed)
```


If the process crashes, there is no special recovery protocol. On restart, the worker resumes from the last committed offset. Jobs after that offset are read again. Already-complete casts are skipped by checking whether their output stream ends in` eos` . Incomplete casts run again.


That is at-least-once delivery with idempotent output. It behaves like exactly-once for completed casts because` eos` is the durable completion marker. We could also use a fencing token with the token being a terminal marker to mark a cast as done.


Retries can leave partial audio in the stream. The start record is therefore an attempt boundary:


```text
seq 0  meta
seq 1  start attempt 1
seq 2  audio sentence 0
# worker crashes
seq 3  start attempt 2
seq 4  audio sentence 0
seq 5  audio sentence 1
seq 6  eos
```


The player can treat the latest start as the beginning of the playable attempt and ignore earlier partial audio.


## Serving Readers


The public read path is intentionally narrower than the internal S2 API. S2 Lite can write, delete, and read any stream but authentication/authorization is left opinionated to the user.


Thus, browsers read through a StreamTTS gateway that only allows public cast streams:


```text
GET /s2/records?stream=pub/casts/<id>&seq_num=0
```


The gateway rejects internal streams like` jobs` and` catalog/*` . It also gives the app a place to rate-limit reads.


For live playback, the same gateway serves SSE. S2 Lite shares a single upstream tail across many readers internally (one broadcast sender feeds every tailing reader), so the gateway just relays that tail to browsers.


Slow clients still do not get to backpressure the system: each subscriber has a bounded queue, and if it fills, the gateway drops that client rather than stalling the stream.


## Some insights


During a warm generation,[tegrastats](https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/AT/JetsonLinuxDevelopmentTools/TegrastatsUtility.html) looks roughly like this:


```text
GR3D_FREQ  0%   VDD_IN 3295mW   idle
GR3D_FREQ 99%   VDD_IN 9911mW   generating, gpu@45.9C
GR3D_FREQ  0%   VDD_IN 6955mW   done
```


` GR3D_FREQ` is GPU utilization. The model briefly pins the GPU while generating, but the full board stays under about` 10 W` for this workload, and thermals never get past roughly 46°C.


The more useful performance numbers come from the` progress/done` receipts. Each receipt includes` sentences` ,` audio_ms` , and` gen_ms` , which lets me compute` xRT` : seconds of audio produced per second of compute.


```text
sentences  audio_ms   gen_ms    xRT   voice
3     11221     2567   4.37   am_michael
2      5205     1670   3.11   af_heart
3     28224    11735   2.40   af_heart
1       917     2481   0.36   af_heart   cold first sentence
```


Once warm, generation lands around` 2.4x–4.4x` realtime. The first sentence after the box has been idle can fall below realtime while the model warms back up; that cold-start behavior is exactly what the scheduler buffer is meant to hide.


At roughly 3x realtime, three simultaneous live casts is a reasonable mental model, which is more than enough for this use case.


A self-hosted AI radio? 📻


We could put a durable stream on the *input* too: an LLM emits tokens into a stream, and the TTS worker tails that stream and generates speech for sentences as they come in at its own pace, and have a fun radio channel going!


## Logmaxxing


The conventional version of this app would use several separate pieces like a queue for background jobs, a database for status and retries, object storage for finished MP3s, WebSockets or SSE for live playback, and cleanup logic for retention and splits one simple flow across multiple systems. With durable streams, most of that collapses into named logs:` jobs` is the queue, stream tails give status,` catalog/<id>` stores the recipe, and` pub/casts/<id>` is both the output and the replay log.


A listener who arrives early tails the stream. A listener who arrives late reads from the beginning and then tails the same stream. I think designing around logs not only simplifies the architecture but also bakes natural reliability into the system. Such patterns around durable logs or streams are applicable across different use-cases and architectures!


Checkout the source for streamtts[here](https://github.com/s2-streamstore/streamtts) .
