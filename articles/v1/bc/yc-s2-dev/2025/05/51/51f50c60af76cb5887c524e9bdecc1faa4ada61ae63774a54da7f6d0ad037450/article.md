---
schema_version: "1.0.0"
document_id: "51f50c60af76cb5887c524e9bdecc1faa4ada61ae63774a54da7f6d0ad037450"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/timestamping"
published_at: "2025-05-14T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:6edc27f1f66d08566f98b75573d137647da2abcee5bca58749d8a5f0f1d2db8e"
---

# Keeping time on a stream

An S2 stream is a log, so logical time with sequence numbers is a given.


But what about good old physical time? How long it's been since[Jan 1, 1970](https://en.wikipedia.org/wiki/Unix_time) ? ( *Hundreds of years from now, I reckon that will still be a pretty important reference point.)*


In our internal tooling, we had been rolling with inserting a header to propagate timestamps, which felt like a reasonable pattern to recommend. But real-world applications care about real-world time, and it became clear we should make timestamping first-class.


Storing a timestamp on each record wouldn’t be too useful if you can’t also consume by it — for example, if you want to use an S2 stream as a long-term source of truth on location data like vehicle movements. With cheap enough stream storage and a querying pattern of linear scans from a point in time, it is appealing to not involve another indexed data store.


If we assume that we can count on monotonicity — time marching inexorably forwards with each record — S2 wouldn’t even need a separate secondary index!


Searching for the right place to start reading by timestamp can be framed very similarly to reading from a sequence number: the backend is essentially navigating a distributed[skip list](https://en.wikipedia.org/wiki/Skip_list) over metadata storage, object storage, and cache of recent writes.


However, in a distributed system, time depends on who you ask. Are we going to go with what the client says or the service? They have different clocks, and network delays between them.


Further, what if you are primarily concerned not with the time of writing to the stream, but as an attribute of when events *actually* happened — like a fitness tracker squirting data points when it comes back online?


## Other systems


Let’s take a look at what some other systems do:


- Kinesis assigns an` ApproximateArrivalTimestamp` to records, and you can use this as a starting position for reads. It does not have any special knowledge of client-specified timestamps – those can be made part of the record if needed.
- Pulsar tracks broker-assigned` publishTime` and optionally client-specified` eventTime` . You can only 'seek' by the former – event time does not get indexed.
- Kafka allows for either a client-specified (` CreateTime` ) or broker-assigned (` LogAppendTime` ) timestamp per record. If the client does not provide a timestamp when using` CreateTime` , it gets silently replaced with the` LogAppendTime` . Record timestamps are also used in retention, so certain knobs to clamp the timestamp can be a good idea if using` CreateTime` .


## Time in S2


Coming back to what we have landed on for S2, in the simple case, you just let the service assign monotonic record timestamps of the arrival time in milliseconds since our favorite epoch.


How does the backend ensure monotonicity? We force it! It is tracking the highest timestamp per stream, and uses that if the new timestamp is *somehow* ( *cough* distributed systems *cough* ) smaller.


```text
inputs:         42,   44,   42,   50,   50,   48,   55,   ..
adjusted:       42,   44,   44,   50,   50,   50,   55,   ..
```


Unlike the sequence numbers assigned by S2, timestamps are allowed to be identical between consecutive records.


A key decision point was whether to allow client-specified timestamps. We decided some essential complexity here was worthwhile, because event time is far too useful when you consider scenarios like offline devices or backfills.


A` timestamping.mode` knob made sense to introduce in stream configuration, so users can know with confidence whether client or arrival time is used:


- ` client-prefer` – (the default) use client-specified timestamp if present, otherwise the arrival time
- ` client-require` – require clients to specify a timestamp that will be used
- ` arrival` – use arrival time regardless of whether or not the client specifies a timestamp


By default the arrival time acts as a cap to prevent out-of-whack values messing with the stream’s notion of time. You can enable` timestamping.uncapped` and get full fidelity within the 64-bit range of possibilities, just remember that there is no going back: the automatic adjustment to ensure monotonicity applies to client-specified timestamps too!


Append acknowledgments contain the first and last timestamps for the batch, so you can know if an adjustment was made. *If a use case demands, we can add a way to opt-out of this behavior and reject such an append instead.*


Given the monotonicity constraint, we end up with not-quite-an-arbitrary index — but whether you stick with arrival time or propagate your own timestamps, the service stays cost-effective and lets you efficiently read records along this additional time axis.


```text
$   s2   read   s2://perso-ingest-mnbk/user/foo   --ago   1h   --format   json   --count   3
{  "seq_num"  :8,  "timestamp"  :1747225273458,  "body"  :  "search   \"  hats  \"  "  }
{  "seq_num"  :9,  "timestamp"  :1747225343334,  "body"  :  "view-listing 42"  }
{  "seq_num"  :10,  "timestamp"  :1747225380872,  "body"  :  "add-to-cart 42"  }


$   s2   read   s2://perso-ingest-mnbk/user/foo   --timestamp   1747225340000   --format   json   --count   3
{  "seq_num"  :9,  "timestamp"  :1747225343334,  "body"  :  "view-listing 42"  }
{  "seq_num"  :10,  "timestamp"  :1747225380872,  "body"  :  "add-to-cart 42"  }
```


We hope our approach will serve users well!Feedback very welcome.
