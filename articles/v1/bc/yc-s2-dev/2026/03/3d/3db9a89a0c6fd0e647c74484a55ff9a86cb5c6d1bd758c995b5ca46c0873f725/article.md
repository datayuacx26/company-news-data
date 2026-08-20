---
schema_version: "1.0.0"
document_id: "3db9a89a0c6fd0e647c74484a55ff9a86cb5c6d1bd758c995b5ca46c0873f725"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/video-conferencing"
published_at: "2026-03-18T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:bf094b122f50374de1a3bd075b46bfd2699a3e858382fdacd86a9ef8b8306123"
---

# Video Conferencing with Durable Streams

[SpacetimeDB](https://spacetimedb.com/) recently shared what they called,["the world's first video call over a database"](https://github.com/Lethalchip/SpaceChatDB) 😅: capturing camera and mic in the browser, encoding frames as JPEG + PCM, and routing them through its real-time subscriptions.[PlanetScale](https://planetscale.com/blog/video-conferencing-with-postgres) later followed with Postgres: encoding frames in` BYTEA` columns, delivery via WAL logical replication, and a cleanup job pruning frames after 5 seconds.


Both are very impressive! And I wanted to take a stab at it by using the infrastructure *designed* for ordered, real-time, durable data streams, so I built a full video conferencing app on[S2](https://s2.dev/) streams.


You can try it **[here](https://s2-video.fly.dev/)** . Open it in two tabs or share the link with someone! The source code is on[GitHub](https://github.com/s2-streamstore/s2-video) .


## Architecture


S2 turns the humble log, the stream, into a first-class cloud storage primitive. Instead of storing entire objects, applications append and read records on named streams using its focused API.


Every record is durably sequenced at the stream tail. Consumers can read streams live as new records arrive or replay history from any earlier position. This allows a stream to act as both durable storage and reliable transport for ordered data.


Each room uses a small set of named streams:


```text
rooms/{room}/media/{user}   -> video + audio + screen, interleaved
rooms/{room}/chat           -> persistent chat history
rooms/{room}/meta           -> join/leave + control events (like hand raises)
```


Audio and video are sent over a WebSocket connection to a Go server which makes them durable in S2 using an[AppendSession](https://s2.dev/docs/sdk/appending#append-session) and fans out to multiple readers over a[ReadSession](https://s2.dev/docs/sdk/reading#read-session) .


The key simplification here is that:


- live media is a stream read
- recording is a no-op, the stream is durable by design
- replay is another stream read
- MP4 export is another stream read


There is no separate recording pipeline, replay database, or post-processing step to assemble files!


## Reading live media


The Go server writes media using an[AppendSession](https://s2.dev/docs/sdk/appending#append-session) , batching records in 5ms windows for low latency and high throughput. Each record body is the raw media payload, with the media type stored as an S2[record header](https://s2.dev/docs/concepts/records) .


For live viewing, each participant reads each remote media stream from the current tail, following new records as they arrive.


So the live path looks like this:


The same pattern is used for other features too:


- ` chat` starts from` SeqNum: 0` , so new users get old messages first and then new ones
- ` meta` tails live control events, so "hand raises" work like any other record
- replay finds past participants by reading` join` events from` meta`


## Replay


Replay is not a special file format. The server just reads the room streams again!


```text
/api/rooms/{room}/timeline
├─ read meta stream for participant history + join/leave events
├─ read first media record for start timestamp
└─ CheckTail() for end timestamp


/ws?room=...&replay=true&from=T
├─ replay media/{alice} from T
├─ replay media/{bob}   from T
├─ replay meta          from T
└─ replay chat          from T
```


Playback speed is derived from record timestamps, so the replay UI is mostly a thin layer over stream reads:


For MP4 export, the server reads each participant's media stream, pipes audio and video directly into ffmpeg for compositing, and streams the result to the browser with no intermediate files.


If you don't care about saving the video, you can just set the retention policy on the streams to be short, e.g. 5 seconds, instead of having a[background job](https://planetscale.com/blog/video-conferencing-with-postgres#accumulating-rows) . Or to save it forever, you can set it to be` infinite` .


## Thoughts


This might just become our go to meeting spot given how smooth it was. Every feature that I thought of could be simply mapped as a read or write on S2's durable streams. It is an unusual architecture for a video conferencing app, but it points to a broader idea that when streams are treated as a storage primitive rather than merely a messaging layer, many real-time applications become far simpler to build and operate.
