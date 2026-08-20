---
schema_version: "1.0.0"
document_id: "71ab1209569af0fb59692e20d236ae9175360ddee0c63d9d499f3ac949911dbb"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/kv-store"
published_at: "2025-01-10T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:caae7567bb1da8224d2ccf6886da38279a9b221502eaf43dfbe32a8ae4863227"
---

# One weird trick to durably replicate your KV store

Note


The KV-store described in this post was also touched upon in a talk at[HYTRADBOI 2025](https://www.hytradboi.com/2025) . Check out the[video](https://www.hytradboi.com/2025/f2cc03cb-14fc-42f4-ad38-b4b15a15815f-serverless-primitives-for-the-shared-log-architecture) !


## Overview


S2 is fundamentally a building block. We believe it is an excellent fit for a variety of use cases that require fast, scalable, and cost-effective access to sequenced data: change data capture, feature transformation, observability events, and click streams, to name just a few. A lot of the time, you really just want an object-storage-like interface for durable streams of records.


We are continuing to iterate on the core capabilities of S2, such as supporting record timestamps in addition to sequence numbers (on its way), and key-based compaction (roadmap). We also have our sights on offering higher-level layers for compatibility with protocols like Kafka that offer features like consumer groups and queue semantics, via software that runs *on top of* S2.


As we embark on building these additional layers, we are making the core S2 service available for **anyone** to use as a foundation for their own data-intensive systems. This post explores one concrete example of what that can look like, using a beloved "hello world" distributed data system: a **replicated key-value store** .


The S2 API is[deliberately simple](https://s2.dev/docs/concepts/records) ; however, it is by no means limited to basic use cases. S2 is designed to provide a first-class implementation of the **shared log** abstraction, a powerful tool for distributed systems engineers that has gained traction in recent years.


This post is structured in three parts:


1. [An intro to the concept of shared logs.](https://s2.dev/blog/kv-store#part-1-the-shared-log-abstraction)
2. [Exploring how S2's stream API can be used to design a multi-primary replicated KV store with strong consistency properties.](https://s2.dev/blog/kv-store#part-2-designing-a-replicated-kv-store) *(This part won't be language specific at all.)*
3. [Digging into a sample implementation of the system which uses S2's Rust SDK.](https://s2.dev/blog/kv-store#part-3-implementing-the-kv-store-using-the-rust-sdk)


Code example


The code example we'll be walking through is[on GitHub](https://github.com/s2-streamstore/s2-kv-demo) . If you want to jump right into that, head over to the[README](https://github.com/s2-streamstore/s2-kv-demo/blob/main/README.md) where you can find some quickstart materials for getting it up and running locally, as well as sample` curl` invocations to exercise it.


## Part 1: The shared log abstraction


[Write-ahead logs](https://en.wikipedia.org/wiki/Write-ahead_logging) (or "WAL"s) have been a cornerstone of database durability for decades. Traditionally, WALs are stored on non-volatile storage that is co-located with compute, and are used to provide crash recovery for database nodes. As soon as a mutation has been persisted in the WAL, it can be considered durable — its effects can be deterministically recovered after a crash — and the database is free to asynchronously complete more expensive actions, like rewriting a page in a B-tree.


In the distributed context, maintaining copies of a database for scalability, availability, and performance is notoriously tricky but well-researched. There are many types of consistency guarantees that a system may target, but an underlying challenge in a replicated setup is making nodes agree about the content of the database at a given moment. Strong[consistency models](https://jepsen.io/consistency/models) promise that even a highly distributed database appears as if we are communicating with one that were only running on a single machine.


This is where consensus protocols like Paxos or Raft enter the picture to power[state machine replication](https://en.wikipedia.org/wiki/State_machine_replication) (or "SMR"). Generally, the database implementor has to take on a lot of the heavy-lifting involved in SMR, which has a whole[spectrum of design possibilities](https://transactional.blog/blog/2024-data-replication-design-spectrum) .


Suppose that instead of residing on a block storage device attached to a single machine, the WAL were distributed — able to be written to and read from multiple nodes on different hosts concurrently, and independently durable across node failures.


If we had something like this, we could use the WAL itself,["disaggregated" from local compute](https://blog.schmizz.net/disaggregated-wal) , as the mechanism for distributing state across our replicas. Our core database code could then be much more focused on indexing and querying semantics.


This is the concept of the shared log, which Mahesh Balakrishnan explores in depth in his 2024 paper,[Taming Consensus in the Wild](https://maheshba.bitbucket.io/papers/osr2024.pdf) :


> As an analogy, think of the SMR platform as a filesystem and the shared log as a block device. In much the same way that a block device makes it easier to build a filesystem without worrying about hardware internals (e.g., HDD vs. SSD), a shared log helps us write an SMR layer without reasoning about the internals of the consensus protocol. The SMR layer is then free to focus on the complexity of materialization, snapshot management, query scalability, single-node failure atomicity, etc., in much the same way that a filesystem can focus on file-grain multiplexing, directories, crash consistency, etc.


This idea is at the heart of the[Delos](https://engineering.fb.com/2019/06/06/data-center-engineering/delos/) system at Meta, used for powering control plane services.


More recently, Amazon has shared[how they built MemoryDB](https://assets.amazon.science/e0/1b/ba6c28034babbc1b18f54aa8102e/amazon-memorydb-a-fast-and-durable-memory-first-cloud-database.pdf) , a strongly-consistent replicated Redis, using this same technique:


> MemoryDB offloads durability concerns to a separate low-latency, durable transaction log service, allowing us to scale performance, availability, and durability independently from the in-memory execution engine.


Access to a system that provides sufficient durability, performance, and a suitable API for using as a shared log service is, alas, pretty hard to come by unless you happen to find yourself at Meta or Amazon.


We want S2 to be the **public implementation** of exactly this type of service, which has to date been a super-power mostly just for super-scalers.


## Part 2: Designing a replicated KV store


Let's see how a key-value database can be implemented using S2 as a shared log. Our example will not have nearly as sophisticated of an API as Redis — to start with, we'll just support` get` /` put` /` delete` .


### Goals


Some things we would like to see in our KV store:


- HTTP interface over a` get` /` put` /` delete` REST API: it should be easy to interact with the system using` curl` .
- Durability: specifically, in multiple availability zones of a cloud region. Writes modifying the state of the store cannot be lost once acknowledged.
- Horizontal scalability: we aim to distribute our requests across an arbitrary number of replicas, for improved performance and availability.
- Multi-primary: each of our nodes should be capable of servicing both reads and writes. S2 does support concurrency control mechanisms1 which simplify the robust implementation of leader/follower schemes, but we will dive into that in a future post.
- Strongly consistent,[linearizable](https://jepsen.io/consistency/models/linearizable) reads and writes: we want every read or write to reflect all prior writes that have completed before it (and also *in the order* in which those writes occurred). For clients that don't require strong consistency, we can provide better performance with an opt-in "eventual consistency" mode.


### Non-goals


Some aspects we will consider acceptable limitations, but all of which make for good *"exercises left for the reader"* 😄:


- Returning prior values of a key from` put` and` delete` .
- Supporting values larger than 1 MiB, which is the limit on an S2 record's size.
- Snapshotting to allow for restoring state via a combination of a snapshot and, only for recent writes, the log.
- Sharding or any sort of data partitioning.


### Connecting to S2 concepts


To build this system, we need a shared log, stored durably, where each entry in that log provides some payload representing a change to our KV store. These log entries will need to be totally ordered, so that there is no ambiguity about the sequence of events, and we can deterministically reconstruct state from the log.


Maybe you can already see where this is going... this is exactly what we get with[an S2 stream](https://s2.dev/docs/concepts/records) ! We are going to use "log entry" and "record" interchangeably for the rest of this post.


### AppendSession for log writes


S2 streams can be written to via unary or streaming RPCs. Since our KV store is always going to need access to its log, each node can maintain a streaming` AppendSession` for its lifetime.


Only two of the routes in our KV store's API actually "write":` put` which supplies a new value, and` delete` which removes the existing value. Any time a node receives a request for one of these, it will also need to prepare a log entry corresponding to those actions, and send that as an append over the session.


The actual format of this entry can be virtually any lossless representation of the key it applies to and the value being supplied (in the case of` put` ).


Once we have encoded our entry and sent it to S2, we have to wait to receive a corresponding acknowledgement that it has become durably sequenced within the log before we can in turn send an acknowledgment to the KV store requestor.


The` AppendSession` RPC is full-duplex. We can send records to it and concurrently also receive acknowledgements (or an error) from it. The acknowledgements will always arrive in the same order as the inputs.


Sequence diagram for a` put` to the KV store


S2 append latency is in the critical path of` put` and` delete` calls on our store, so we can never return faster than the "round-trip time" to append a record and receive its acknowledgment. Accordingly, these write-triggering calls on our KV store will typically be in the low tens of milliseconds (50 ms at p99) — assuming our KV store is also running in the same cloud region as S2,2 and that we elect to use S2's[Express storage class](https://s2.dev/blog/intro#serverless--at-what-cost) . *(We do plan to offer a faster storage class in the future for single-digit millisecond latencies.)*


### ReadSession and CheckTail for log reads


We have not covered the actual "materialized state" of our KV store yet.


If we were not going for horizontal scalability, and could guarantee that there would only ever be one node running, then serving reads would simply be a matter of consulting with the node's internal state stored in an in-memory dictionary of some sort. Writes could update that state directly, after receiving an acknowledgment from S2.` Get` requests would also be incredibly fast, as there would be no external service to communicate with.3


In a multi-primary setup, however, any of the KV store replicas might be concurrently writing to the shared log, so updating the state directly in response to an append wouldn't work. We could be missing any writes that occurred on other nodes, and replicas would quickly get out of sync.


Instead, we can have each node keep a tailing` ReadSession` open for its lifetime. This is a half-duplex RPC that will let us read all entries on the shared log, in the same order in which they were appended, with minimal latency overhead. If all nodes then apply those entries to their local copies of the materialized state, they will end up as identical replicas of our KV store, and any of them can service` get` requests by consulting their local copy.


Almost, anyway.` ReadSession` s do tail updates to the log in real time, but to provide strong consistency we also have to ensure that the node which is servicing the` get` is fully caught up with the log.


Suppose a` get` request comes in to one of our nodes just after a` put` completed (either on the same node or a different one), but our read session hasn't quite caught up with it yet. If we didn't do any extra coordination, and simply served the` get` using the current materialized state, we could return a stale value, and our system wouldn't actually be linearizable.


This is where` CheckTail` comes in. This operation gives us the current sequence number representing the tail of the stream (i.e., the sequence number that would be assigned to the next record appended). When a` get` arrives, our KV store just needs to check the current tail of its log, and compare it to the last sequence number which has been reflected in the local state. If the applied local state lags from the tail, we simply need to wait until we've caught up to it over the` ReadSession` .


Sequence diagram for a strongly consistent` get` to the KV store


Similar to` put` and` delete` , which are bounded below by the time it takes to append and receive an S2 acknowlegement, any` get` on our KV store is bounded by the time it takes for` check_tail` to return. Unlike appends, this latency is not a function of the stream's storage class,[and should be quite fast](https://s2.dev/docs/api/records/check-tail) . Nevertheless, clients that can tolerate *eventual consistency* reads (potentially stale values) may elect to forego the` check_tail` operation for better latency.


#### Wrapping it up


If we do all of this, we will end up with a KV store that can satisfy our lofty[goals](https://s2.dev/blog/kv-store#goals) . We only have to deal with three fundamental operations on a stream:[append](https://s2.dev/docs/api/records/append) ,[read](https://s2.dev/docs/api/records/read) , and[check_tail](https://s2.dev/docs/api/records/check-tail) . This is the power of decoupled storage and compute!


## Part 3: Implementing the KV store using the Rust SDK


Now we'll walk through[an actual implementation](https://github.com/s2-streamstore/s2-kv-demo/tree/main) of this system that is built around the[S2 Rust SDK](https://github.com/s2-streamstore/s2-sdk-rust) .


If Rust is not your jam, SDKs for other languages are on their way — starting with[Go](https://github.com/s2-streamstore/s2-sdk-go) ,[Python](https://github.com/s2-streamstore/s2-sdk-python) , and[Java](https://github.com/s2-streamstore/s2-sdk-java) — as well as a REST API.


### Setup


#### Data formats


Keys in our system can simply be` String` s. For values, we'd like to support several common datatypes, for convenience.


A reasonable approach to this is to create a new` Value` enum which wraps other types, e.g.:


```text
enum   Value   {
Str  (  String  ),
UInt  (  u64  ),
List  (  Vec  <  Value  >)
// ...
}
```


Our materialized state can be stored in just about any map datastructure. In the demo, we use a` BTreeMap<String, Value>` .


Similarly, log entries will need to represent` Put` and` Delete` actions:


```text
#[derive(  Clone  ,   Debug  ,   Deserialize  ,   Serialize  )]
enum   LogEntry   {
Put   { key  :   String  , value  :   Value   },
Delete   { key  :   String   },
}
```


S2 streams expect to receive data as batches of[AppendRecord s](https://docs.rs/streamstore/latest/s2/types/struct.AppendRecord.html) . These consist of a binary body, as well an optional set of[headers](https://docs.rs/streamstore/latest/s2/types/struct.Header.html) . Since we're working with bytes for the body, we have complete control over how to encode our log entries as records. The only real constraint is that, since no record can exceed 1MiB on S2, our` Value` s must also be small enough to be serialized within that budget.


For convenience and debug-ability, we can simply encode our log entries as JSON bytes using the[serde](https://serde.rs/) library, so that we get a nice human-readable represetation when reading the log via the[CLI](https://s2.dev/docs/quickstart#get-started-with-the-cli) . In a production system, we would probably want to adopt an efficient binary format.


```text
s2   read   "s2://${  MY_BASIN  }/${  MY_STREAM  }"
```


... might for example produce something like:


```text
{  "Put"  :{  "key"  :  "hello"  ,  "value"  :{  "Str"  :  "world"  }}}
{  "Put"  :{  "key"  :  "s2"  ,  "value"  :{  "Set"  :[{  "Str"  :  "is really cool"  },{  "UInt"  :  1337  }]}}}
{  "Delete"  :{  "key"  :  "hello"  }}
{  "Put"  :{  "key"  :  "map-sample"  ,  "value"  :{  "Map"  :{  "k1"  :{  "Str"  :  "hello"  }}}}}
```


#### HTTP server


We can use the[axum library](https://github.com/tokio-rs/axum) for our actual HTTP server, and set up routes corresponding to our KV store's` get` ,` put` , and` delete` API by using the RESTful` GET` ,` PUT` , and` DELETE` HTTP methods respectively. The README in the[demo repo](https://github.com/s2-streamstore/s2-kv-demo) has some sample` curl` invocations for testing these routes out with actual values.


For additional debug context, we'll also have each route return the shared log range reflected by the corresponding action. Any consumer of this specified portion of the log would see the value that was returned in the response, or in the case of` put` , the value which was provided in the request. The range values are simply S2 stream sequence numbers.


For example, the acknowledgment from this` put` :


```text
$   curl   \
--silent   -H   'Content-Type: application/json'   \
-X   PUT   \
-d   '{"key": "hello", "value": {"Str": "world"}}'   \
"localhost:4001/api"
```


... might look like this:


```text
{   "Ok"  : {   "end"  :   272   } }
```


A subsequent` get` for that key might return this:


```text
{   "Ok"  : [{   "end"  :   272   }, {   "Str"  :   "world"   }] }
```


... which would signify that no additional` put` or` delete` occurred anywhere in the KV store in the meantime, as the reflected log region is still` (..272)` .


Let's dive into how the actual KV store functions will work.


### Main event loop


Earlier, we touched on the main components of our system, and generally how the KV store API will interact with S2's stream API.


There are many ways to structure an actual implementation of this system. In the demo, the main "engine" of our KV store is modelled as an event loop, which is a common pattern for I/O bound workloads.


The idea is that we can[spawn a Tokio task](https://docs.rs/tokio/latest/tokio/task/fn.spawn.html) which will be responsible for executing this loop, and therefore responding to our various input and output streams, and managing the internal materialized state. This task will run for the lifetime of our node. By giving that task ownership of the materialized state, as well as of any I/O streams, we can avoid use of locks entirely.


Internally, our` get` ,` put` , and` delete` functions will all rely on this task to accomplish what they need to.


We can model an event loop task like this as a regular async Rust function, which will get spawned during startup of our node. This is the[orchestrate function](https://github.com/s2-streamstore/s2-kv-demo/blob/7f32cbaca849110ca385c3094d0ba3b08fbce4cd/src/main.rs#L222) in the demo.


This function mostly consists of one large` loop` , the body of which will await a few different async functions. We can use the[tokio::select! macro](https://tokio.rs/tokio/tutorial/select) , which is a handy way to await multiple futures simultaneously. That way, we can handle whichever future[returns Ready](https://doc.rust-lang.org/std/task/enum.Poll.html#variant.Ready) first, and only run the code in the branch that was associated with it. The futures which we don't end up handling will get dropped, but we can re-await futures from the same underlying streams and channels on the next iteration of the` orchestrate` loop.


Here's what the skeleton of our event loop function will look like:


```text
async   fn   orchestrate  (
// ...
)   ->   Result  <(),   KVError  > {
// ...
loop   {
tokio  ::  select!   {
Some  (cmd)   =   command_rx  .  recv  ()   =>   {
// ... commands about new `put`/`delete`/`get` requests
}
Some  (ack)   =   append_acknowledgments  .  next  ()   =>   {
// ... S2 record batch acknowledgments
}
Some  (record)   =   tailing_reader  .  next  ()   =>   {
// ... S2 log entry records
}
else   =>   {   break  ; }
}
}
// ...
}
```


The only catch with using an event loop is that, since we only have a single task responsible for all I/O, we have to be careful to make sure that we correctly offload work. We want the loop to be spending most of its time in the` select!` await, where the task is able to respond to whichever type of message appears next. We don't want the actual event loop task to be stuck awaiting a single function call within one of the branches — it will need to stay open to quickly deal with whatever occurs next, dispatch it, and move on.


For instance, a` put` request will take some time for our KV store to service; each` put` will require a write to the S2 log, and will need to then wait to see if that write was acknowledged by S2. We can't hold up our event loop while we're waiting for that acknowledgment, or it would block other concurrent callers to our system. A typical way to deal with this is by using some sort of async "callback" mechanism ( *more on that in a bit* ).


### Defining command messages


What are these actual flows of information that` orchestrate` will be responsible for, and what actions it will take in response to each?


The first branch in our` select!` block handles commands. Since` orchestrate` is a task, not a struct, we can't have it perform actions by calling a function directly on it. Instead, we'll communicate with it via message passing. That way, commands are simply modelled as another I/O stream to react to in the main event loop.


The` command_rx` in the code snippet above is simply the receiving end of an[unbounded channel](https://docs.rs/tokio/latest/tokio/sync/mpsc/fn.unbounded_channel.html) . This is a "multi-producer / single-consumer", or MPSC, channel — so the receiver held by` orchestrate` is guaranteed to be the only one. We can have an unlimited amount of sender handles, on the other hand, which comes in handy. In other words, callers can easily` clone` and send new commands into our channel, and all of the messages will flow to the main loop.


What actually needs to be communicated to our task? Really, any action we want it to perform. In practice, this will be servicing requests that come from our KV store users directly — so` put` ,` delete` , and` get` . The commands themselves can be an enum, allowing us to pattern match on the different variants within the` select!` branch.


We won't quite just have different commands for` put` /` delete` /` get` respectively. Recall that both` put` and` delete` can be handled almost identically. We can consolidate both of them as a single` WriteLog` command, where the sender constructs a log representing either a` Put` or` Delete` action.


On the other hand,` get` commands actually end up being processed in two different ways. If the caller wants strong, linearizable consistency, we need to make sure that when we execute the` get` that the local materialized state has caught up with the tail of the S2 log. If the caller is fine with eventual consistency, we can execute that` get` immediately, regardless of what the tail is.


Our commands end up looking like this:


```text
type   SequenceNumber   =   u64  ;
type   SequencedValue   =   (  RangeTo  <  SequenceNumber  >,   Option  <  Value  >);
type   WriteSender   =   oneshot  ::  Sender  <  Result  <  RangeTo  <  SequenceNumber  >,   KVError  >>;
type   ReadSender   =   oneshot  ::  Sender  <  Result  <  SequencedValue  ,   KVError  >>;


enum   OrchestratorCommand   {
WriteLog   {
log  :   LogEntry  ,
response_tx  :   WriteSender  ,
},
ReadStrongConsistency   {
key  :   String  ,
reflect_applied_state  :   RangeTo  <  SequenceNumber  >,
response_tx  :   ReadSender  ,
},
ReadEventualConsistency   {
key  :   String  ,
response_tx  :   ReadSender  ,
},
}
```


You may be wondering what these` response_tx` fields are. Those are how we can call back to the original sender of the command (i.e., the actual` put` ,` get` , and` delete` methods on the HTTP server).


Since commands are being communicated to the` orchestrate` task via an MPSC channel, and not a function call, there is no obvious way for the senders of those commands to be notified when the requested asynchronous work is completed.


A common pattern in these situations is to send a[oneshot channel](https://docs.rs/tokio/latest/tokio/sync/oneshot/fn.channel.html) . These are very similar to MPSC channels, but used for receiving a single value — so particularly useful as an async notification mechanism. In our case, the commands contain a sender for the oneshot, which` orchestrate` will either use immediately or hold on to, and the receiver end is then retained by the original issuer of the command.


For instance, the` put` command (what actually ends up getting called by the HTTP server's PUT route) looks like this:


```text
/// Put a new key/value to the store.
async   fn   put  (
&  self  ,
key  :   String  ,
value  :   Value
)   ->   Result  <  RangeTo  <  SequenceNumber  >,   KVError  > {
// Create the oneshot for receiving a response.
let   (response_tx, response_rx)   =   oneshot  ::  channel  ();


// Send a `WriteLog` command corresponding to
// the `Put` key and value, and providing the
// sender end of the oneshot.
self  .  orchestrator_cmd_tx
.  send  (  OrchestratorCommand  ::  WriteLog   {
log  :   LogEntry  ::  Put   { key, value },
response_tx,
})
.  map_err  (  |  _  |   KVError  ::  OrchestratorTaskFailure  )  ?  ;


// Await a response on the receiver.
response_rx
.await
.  map_err  (  |  _  |   KVError  ::  OrchestratorTaskFailure  )  ?
}
```


### Executing commands


We've seen how commands flow into our` orchestrate` task, and how responses can be communicated back using` oneshot` s.


What does` orchestrate` actually do in response to the different commands?


#### Write commands


For` WriteLog` commands (from` put` or` delete` requests), it will simply package the log into a record batch, and emit it to the active` AppendSession` via a channel that supplies (using tokio's[ReceiverStream](https://docs.rs/tokio-stream/latest/tokio_stream/wrappers/struct.ReceiverStream.html) ) an async stream of[AppendInput](https://docs.rs/streamstore/latest/s2/types/struct.AppendInput.html) values.


```text
Some  (cmd)   =   command_rx  .  recv  ()   =>   {
match   cmd {
OrchestratorCommand  ::  WriteLog   { log, response_tx }   =>   {
write_queue  .  push_back  (response_tx);
append_tx  .  send  (  AppendInput   {
records  :   AppendRecordBatch  ::  try_from_iter  (
[  AppendRecord  ::  new  (  bytes  ::  Bytes  ::  from  (log))  ?  ]
)  .  map_err  (  |  _  |   KVError  ::  Weird  (  "unable to construct batch"  ))  ?  ,
..  Default  ::  default  ()
})  .  map_err  (  |  _  |   KVError  ::  Weird  (  "s2 append_session rx dropped"  ))  ?  ;
}
}
// ...
}
```


Notice that we don't just send the record — we also push the` oneshot` sender received as part of the command into a` write_queue` . We can't resolve the originating request until we've received an acknowledgment from S2 that the corresponding log is durable, so we need to hold on to the` oneshot` sender for now.


#### Eventually consistent read commands


Commands for performing eventually consistent reads are pretty easy! Since the caller has elected for non-linearizable reads — in other words, they can tolerate staleness, and don't care if the read doesn't necessarily reflect all prior writes on the shared log — we can simply check the materialized state and return the result immediately:


```text
Some  (cmd)   =   command_rx  .  recv  ()   =>   {
match   cmd {
// ...
OrchestratorCommand  ::  ReadEventualConsistency   {
key,
response_tx
}   =>   {
// Use the oneshot to communicate the read result immediately.
_   =   response_tx
.  send  (  Ok  ((local_state  .  applied_state, local_state  .  storage  .  get  (  &  key)  .  cloned  ())))
.  inspect_err  (  |  _  |   debug!  (  "read rx dropped"  ));
},
}
}
```


#### Strongly consistent read commands


Strong reads are only slightly more involved. These commands will specify a log prefix that must have been applied to the materialized state before we can return a value. (If you're wondering where that prefix is obtained, we'll get to that later, when we discuss where` check_tail` gets called.)


If the materialized state already happens to have caught up to that point, we can immediately return the value. If not, we need to use some sort of queue — similar to what we do with writes — and hold on to the` oneshot` (as well as the` key` ), until we catch up to the desired state.


```text
Some  (cmd)   =   command_rx  .  recv  ()   =>   {
match   cmd {
// ...
OrchestratorCommand  ::  ReadStrongConsistency   {
key,
reflect_applied_state,
response_tx
}   =>   {
if   reflect_applied_state  .  end   <=   local_state  .  applied_state  .  end {
// Applied state is already caught up with the tail.
_   =   response_tx
.  inspect_err  (  |  _  |   debug!  (  "read rx dropped"  ));
}   else   {
// Not yet caught up. Defer until we do.
pending_responses  .  submit  (  PendingResponse  ::  new  (
reflect_applied_state,   ResponseContext  ::  Read  { key, response_tx }
))
}
},
// ...
}
}


```


### Handling S2 I/O


At this point, we've seen how all of our different` OrchestratorCommand` s are reacted to. Some reads get handled immediately, others are pending on some future log entries being applied, and are stashed until then. Writes, similarly, are all stashed into a queue where they will await acknowledgments from S2.


In both cases, we are awaiting some additional information from our durability layer, S2. Let's see how that works.


#### S2 append acknowledgements


We send batches of records (individual log entries) to S2 via an MPSC channel (the` append_tx` sender, as discussed earlier), but also[receive acknowledgments back](https://docs.rs/streamstore/latest/s2/client/struct.StreamClient.html#method.append_session) from S2 as an async stream of[AppendOutput](https://docs.rs/streamstore/latest/s2/types/struct.AppendOutput.html) messages.


Within an` AppendSession` , acknowledgments are guaranteed to be received in the same order that we sent appends, which is why a FIFO queue is a good way to keep track of our pending, or "inflight", log writes.


Whenever a new acknowledgement arrives, we simply` pop_front` from our queue and obtain the` oneshot` for communicating back to the original` put` or` delete` call, and send a message indicating that the write has succeeded.


```text
Some  (ack)   =   append_acknowledgments  .  next  ()   =>   {
let   response_tx   =   write_queue  .  pop_front  ()  .  expect  (  "queue entry"  );
_   =   response_tx
.  send  (  Ok  (  ..  ack  ?.  end_seq_num))
.  inspect_err  (  |  _  |   debug!  (  "write ack rx dropped"  ));
}
```


#### S2 log entries


Our` orchestrate` task is also always listening for log entries[via a ReadSession](https://docs.rs/streamstore/latest/s2/client/struct.StreamClient.html#method.read_session) . These could correspond to writes that were proposed earlier by the same node — or they might have been written by one of the other nodes; it's a shared log after all!


Whenever a log entry is received,` orchestrate` simply has to apply it to the materialized state (the in-memory version of the map), and update the tracker of the currently applied contiguous prefix of log entries. Whenever it updates its state, it can also look for any pending strong` get` requests, and see if they can now be resolved (and the requested value can be returned from the now up-to-date state).


```text
Some  (record)   =   tailing_reader  .  next  ()   =>   {
let   record   =   record  ?  ;


// Deserialize the json log.
let   log_entry   =   serde_json  ::  from_slice  (record  .  body  .  as_ref  ())
.  map_err  (  |  e  |   KVError  ::  JsonError  (e  .  to_string  ()))  ?  ;


// Insert or delete the key from the local storage map.
match   log_entry {
LogEntry  ::  Put   { key, value }   =>   local_state  .  storage  .  insert  (key, value),
LogEntry  ::  Delete   { key }   =>   local_state  .  storage  .  remove  (  &  key),
};


// Update the applied state prefix. Our materialized state reflects
// this contiguous range of the log.
local_state  .  applied_state   =   ..  record  .  seq_num   +   1  ;


// Find any pending `get` requests that may have been waiting for
// the `applied_state` to catch up.
for   PendingResponse   {
end_seq_num  :   _,
response_context
}   in   pending_responses  .  drain_applied_responses  (local_state  .  applied_state) {
match   response_context {
ResponseContext  ::  Read  { key, response_tx }   =>   {
_   =   response_tx  .  send  (  Ok  ((local_state  .  applied_state, local_state  .  storage  .  get  (  &  key)  .  cloned  ())))
.  inspect_err  (  |  _  |   debug!  (  "read rx dropped"  ));
}
}
}
}
```


That's about it for` orchestrate` !


### CheckTail operations for strong reads


Earlier, when we were looking at[how OrchestratorCommand::ReadStrongConsistency commands get processed](https://s2.dev/blog/kv-store#strongly-consistent-read-commands) , we saw that all strong read requests need to specify a` reflect_applied_state` parameter. This value indicates the value of the S2 log's tail at the time at which the` get` was processed, and we[can only achieve linearizability](https://s2.dev/blog/kv-store#readsession-and-checktail-for-log-reads) if we ensure that the materialized state has reflected all logs up to that position when we retrieve and return a value.


It is expected, therefore, that the` get` handler in our KV store call` check_tail` itself, and provide the value it received in the command to` orchestrate` . In theory,` orchestrate` could also be responsible for performing the` check_tail` op — it's just one additional I/O task after all.


#### Bus-stand optimization


In practice, it's a bit neater to have a separate, dedicated event loop task for` check_tail` purposes. This is mainly to take advantage of what Mahesh Balakrishnan refers to as the "bus-stand" optimization (from §3.2 in[this paper](https://maheshba.bitbucket.io/papers/osr2024.pdf) mentioned earlier).


Since every single strong read serviced by our KV store needs to obtain the current tail, by default that means one[check_tail operation](https://docs.rs/streamstore/latest/s2/client/struct.StreamClient.html#method.check_tail) per read request. If we're willing to slightly delay reads, we can instead group these tail requests into batches, where — similar to catching a bus at a bus stand — passengers wait until the next "departure" to the underlying` check_tail` call, and a single response can satisfy that input for several read requests at once. This allows us to trade off additional latency for strong reads with a ceiling on the maximum number of` check_tail` operations performed per second.


Similar to` orchestrate` , we can communicate with a dedicated` check_tail` task over an MPSC command channel, and receive responses via a` oneshot` . The bus stand optimized version of` check_tail` looks like this:


```text
/// Get the current stream's tail.
///
/// Wait up to `max_wait` before the actual `check_tail` invocation occurs, allowing
/// other callers to also be served by the same underlying S2 tail op.
async   fn   bus_stand_check_tail  (
&  self  ,
max_wait  :   Duration  ,
)   ->   Result  <  RangeTo  <  SequenceNumber  >,   KVError  > {
let   (response_tx, response_rx)   =   oneshot  ::  channel  ();


self  .  bus_tx
.  send  (  BusRider   {
max_wait,
response_tx,
})
.  map_err  (  |  _  |   KVError  ::  BusStandTaskFailure  )  ?  ;


response_rx
.await
.  map_err  (  |  _  |   KVError  ::  BusStandTaskFailure  )  ?
}
```


*(The actual implementation of the bus stand task, which performs the batching, can be seen[here](https://github.com/s2-streamstore/s2-kv-demo/blob/7f32cbaca849110ca385c3094d0ba3b08fbce4cd/src/main.rs#L181) in the demo).*


Our new` bus_stand_check_tail` function can be called by` get` and used to obtain the tail before sending a read command to the` orchestrator` :


```text
/// Get the value of a key.
async   fn   get  (
&  self  ,
read_consistency  :   ReadConsistency  ,
key  :   String  ,
)   ->   Result  <(  RangeTo  <  SequenceNumber  >,   Option  <  Value  >),   KVError  > {
// Oneshot for receiving the value.
let   (response_tx, response_rx)   =   oneshot  ::  channel  ();


let   cmd   =   match   read_consistency {
ReadConsistency  ::  Eventual   =>   {
OrchestratorCommand  ::  ReadEventualConsistency   { key, response_tx }
}
ReadConsistency  ::  Strong   =>   OrchestratorCommand  ::  ReadStrongConsistency   {
key,
reflect_applied_state  :   self  .  bus_stand_check_tail  (  BUS_RIDER_MAX_WAIT  )  .await?  ,
response_tx,
},
};


self  .  orchestrator_cmd_tx
.  send  (cmd)
.  map_err  (  |  _  |   KVError  ::  OrchestratorTaskFailure  )  ?  ;


response_rx
.await
.  map_err  (  |  _  |   KVError  ::  OrchestratorTaskFailure  )  ?
}
```


## Conclusion


We covered a lot in this post, and managed to build a simple multi-primary, strongly consistent database! I hope it gave you a glimpse into some of the ways in which S2 can be used at the foundation of real distributed data systems. We can't wait to see what people end up building.


The actual[implementation](https://github.com/s2-streamstore/s2-kv-demo/blob/main/src/main.rs) of the system discussed here is only ~600 lines, and worth checking out if you made it this far!


## Footnotes


1.


Marc Brooker's[blog post about MemoryDB](https://brooker.co.za/blog/2024/04/25/memorydb.html) discusses how fencing support by a shared log service can safely support leader leases. MemoryDB itself requires only conditional append support, and uses the same log used for data to coordinate leader election and leases (see §4.1 in the[paper](https://www.amazon.science/publications/amazon-memorydb-a-fast-and-durable-memory-first-cloud-database) ). You are able to take[either approach](https://s2.dev/docs/api/records/append#concurrency-control) with S2!↩


2.


Currently, S2 is only hosted from AWS's` us-east-1` region, but we will expand access to other regions and public cloud providers.↩


3.


Actually guaranteeing that there is truly only one node capable of writing — either because we didn't want a replicated setup, or in some sort of leader/follower replication scheme — needs careful design. See note above.1↩
