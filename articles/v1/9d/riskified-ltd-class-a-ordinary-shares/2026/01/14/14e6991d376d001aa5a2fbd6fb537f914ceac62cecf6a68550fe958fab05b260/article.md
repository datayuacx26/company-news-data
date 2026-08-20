---
schema_version: "1.0.0"
document_id: "14e6991d376d001aa5a2fbd6fb537f914ceac62cecf6a68550fe958fab05b260"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-rss-dd7d0cc56e2d"
canonical_url: "https://medium.com/riskified-technology/unblocking-the-node-js-event-loop-practical-troubleshooting-of-a-real-world-bottleneck-27aa5a3d2022"
published_at: "2026-01-11T19:38:33+00:00"
first_seen_at: "2026-07-20T23:18:31.853064+00:00"
fetched_at: "2026-08-20T02:06:38.087890+00:00"
content_hash: "sha256:64379362a7b6a892b56f6a9a68417cb2a3189955ce6c4f47835025e5cf8689e1"
---

# Unblocking the Node.js Event Loop: Practical Troubleshooting of a Real-World Bottleneck

#### When your Node.js service stops responding, Kubernetes kills pods, and your metrics show gaps — you know it’s time to dig in.


I work in a fullstack team at **Riskified** that builds an identity-resolution engine clustering E-commerce accounts into underlying identities, one of the company’s few high-load, near-realtime Node.js systems. The engine is a chain of Kafka-driven **producer-consumer** services, using **Neo4j** as the graph database.


One normally reliable Node.js consumer began freezing when processing huge identities — sometimes hundreds of thousands of graph nodes — causing pods to hang, fail health checks, and get restarted.


This post covers how we tackled the incident, from first symptoms to debugging and optimization, eventually tracing the issue to an unexpected bottleneck in the Neo4j driver, and the techniques we used to understand and prevent **event-loop blocking** .


### First Signs of Trouble


The issue surfaced in production shortly after we onboarded a new data set. We noticed that once certain large identities entered the pipeline, **consumer pods stopped responding** .


At first, it seemed random — the pods would just stop responding to health checks and get killed by ArgoCD and Kubernetes’ liveness probes. But a look at our Grafana **** dashboards told a clearer story.


Our Node.js application exposes Prometheus metrics (via *prom-client* ), scraped every 15 seconds. Normally, the graphs are smooth and continuous — but when processing those big identities, we saw **45-second gaps** in the metric timeline.


The metric used in the below time series visualization was nodejs_eventloop_lag_seconds, which can be configured when integrating Node.js with *prom-client* :


In other words: the app stopped responding to */healthcheck* and */metrics* endpoints. It wasn’t dead — it was **busy** . And in Node.js, “busy” usually means **event loop blocking** . Being single-threaded, Node.js executes JavaScript on one thread at a time, so even I/O-heavy systems stall if a synchronous operation takes too long.


### Reproducing the Problem


The first step was to **recreate the issue** in a controlled test environment.


I dug through logs to find a sample account ID from one of the massive identities, then asked our Data team to **restore Neo4j from a backup** containing that identity. Using the same Kafka message format as production, I published a single input message with it, and observed the consumer’s behavior.


I quickly confirmed the event loop was indeed blocking — the consumer stopped writing logs mid-processing, and eventually crashed or got killed by K8s:


But tracing where it got stuck was tricky. We lacked sufficient debug logs, so I added new ones to every major processing step (callbacks, success/error handlers, etc.) to pinpoint where execution halted.
Eventually, ArgoCD logs revealed an ominous message:


```text
FATAL ERROR: Reached heap limit allocation failed — JavaScript heap out of memory
```


Increasing pod memory limits eliminated the crash, but **the blocking remained** . The consumer was still freezing under heavy loads, even without running out of memory.


Publishing a batch of 25 nearly identical messages reliably reproduced the stall — a useful test setup for deeper investigation.


### Investigation & Optimizations


#### The Usual Suspects: Iterations & Performance Optimizations


Large datasets often tempt developers to blame inefficient loops — and I was no exception.


I suspected our code’s **multiple iterations** over the identity.accounts array (which could contain hundreds of thousands of elements) might be the culprit. I went through each occurrence, merging duplicate loops, optimizing comparisons, and removing redundant computations.


I also parallelized certain promise chains — invoking success/error handlers concurrently instead of sequentially awaiting each one, and moved code outside of the Neo4j transaction if it didn’t have to be there.


Those optimizations were made after I found those small areas in the code where we did not put enough performance-oriented thought in advance.
And while these changes improved micro-performance, they **did not eliminate the blocking** . Even with cleaner code, the event loop still significantly froze during large identities’ processing.


#### Partitioning Work into Non-Blocking Batches


Next, I tried **Partitioning** — a common optimization technique where a large dataset or task is **divided into smaller, independent parts** that can be processed separately.
If processing ~800k items in one go blocked the loop, maybe chunking it into smaller async batches would help.


Here are 2 examples for partitioned computations I implemented to yield back to the event loop between chunks:


```text
import * as util from 'node:util';   export async function findMinimumInNonBlockingBatches<T, V>(    array: T[],    compareFieldExtractionFn: (item: T) => V,    initialMinValue: V,    batchSize: number,  ): Promise<V> {    return new Promise((resolve) => {      let index = 0;      let currentMin: V = initialMinValue;     function processChunk() {        const end = Math.min(index + batchSize, array.length)        for (let i = index; i < end; i++) {          const currentValueToCompare = compareFieldExtractionFn(array[i]);          if (currentValueToCompare < currentMin) currentMin = currentValueToCompare;        }        index += batchSize;        if (index < array.length) setImmediate(processChunk);        else resolve(currentMin);      }      processChunk();    })  }   export async function processInNonBlockingBatches<T>(    array: T[],    callbackPerBatchItem: (item: T) => void,    batchSize: number,  ): Promise<void> {    for (let i = 0; i < array.length; i += batchSize) {      const batchItems = array.slice(i, i + batchSize);      for (const item of batchItems) callbackPerBatchItem(item);      await util.promisify(setImmediate)();    }  }
```


I deployed and tested this partitioning logic in our Node.js consumer with various batch sizes (from thousands to hundreds of thousands).


Yet even with these changes, **blocking persisted** . This meant that the partitioning code seemed to either be implemented wrong or indeed have no effect on the blocking.


#### Building a Side Project for Controlled Testing


To isolate the issue and validate my partitioning implementation, I built a small **side project** replicating the partitioning logic under simpler conditions, unrelated to our Node.js consumer code.


It exposed 3 endpoints:


- */healthcheck* — pinged periodically
- */find-minimum/blocking* — runs the non-partitioned loop
- */find-minimum/non-blocking* — runs the partitioned version


By periodically calling the */healthcheck* endpoint while invoking the blocking vs. non-blocking endpoints, I could observe event loop responsiveness.


The results were partially encouraging: the partitioned version **significantly reduced response delays** on healthchecks.
This validated that the partitioning logic was working correctly — so if this works, but does not resolve the issue in production — then the consumer’s blocking must originate **somewhere else** .


### Finding the Bottleneck — Profiling with Clinic.js


To pinpoint the actual bottleneck, I decided to use **Clinic.js** — a powerful Node.js profiling tool that diagnoses performance issues and provides resource usage metrics.


To allow it to run on all of our code — this time with no presumptions — I compiled our TypeScript consumer into JavaScript.


Then I triggered consumption of a single Kafka message containing the massive identity, by publishing a message using Conduktor, working against similar data in Neo4j as in prod to force querying the huge identity, and ran the following Clinic.js tools:


```text
clinic doctor - node main.js
```


This confirmed the significant event loop delays I was experiencing. I moved on to run the even more helpful *flame* tool:


```text
clinic flame — node main.js
```


The flamegraph revealed 3 **hottest frames** consuming around **80%** of total execution time, all pointing to the Neo4j driver’s internal buffers:


```text
BaseBuffer.readUInt8  …/node_modules/neo4j-driver-bolt-connection/lib/buf/base-buf.js line:201 column:47
```


```text
streamDecodeCombinedBuffer  …/node_modules/neo4j-driver-bolt-connection/lib/channel/utf8.js line:50 column:36
```


This stacktrace led us to the Neo4j javascript driver’s readTransaction (executeRead) function, which serializes the returned aggregated records into memory, to be further processed in our JavaScript code.


The problem wasn’t our JavaScript loops. It was **Neo4j’s driver synchronously serializing huge result sets into memory** , blocking the event loop while doing so.


### Implementing the Solution — Streaming Query Results from Neo4j


By default, our read query aggregated all nodes via collect(…), forcing Neo4j to serialize and return a **single giant array** .


The problematic query was:


```text
…  RETURN collect({nodeId: resultNode.value, nodeParentId: node.parentId}) AS accountsInEntity
```


I refactored it to stream results, using Neo4j’s Streaming API:


```text
…  RETURN {nodeId: resultNode.value, nodeParentId: node.parentId} AS accountInEntity
```


Meaning the wrapping collect(…) statement was removed, so instead of returning all aggregated accountsInEntity at once, we stream and serialize each accountInEntity one by one.


Apart from the query refactoring, I also had to change the read transaction function implementation and return type to match Neo4j’s Streaming API, and implemented stream reading with:


```text
protected async runQuery(     query: Neo4jQuery,     transaction: ManagedTransaction,   ): Promise<FindEntityQueryResult> {     const identityAccounts = [];      const queryStream = this.streamQueryTransaction(query, transaction);      for await (const record of queryStream) {       const currentStreamedAccount = record.get('accountInEntity');       identityAccounts.push(currentStreamedAccount);     }      /* rest of the code */  }   private streamQueryTransaction = (     neo4jQuery: Neo4jQuery,     transaction: ManagedTransaction,   ): Result<RecordShape> => {     return transaction.run(neo4jQuery.query, neo4jQuery.parameters);  }
```


I implemented this under a feature flag, allowing us to:


- Continue to support the non-streamed read query for some of the consumer’s pods, in order to not increase its latency there, since **streaming takes slightly longer** to complete compared to the regular read query.
- Easily toggle between the old and new queries in specific pods, to carefully **monitor streaming performance** for the next few days in production.


With streaming in place, the event loop could finally “ **breathe** ”.
The consumer pods stopped crashing, Grafana metrics were reported continuously with no gaps, and we were able to see the large identities finally being fully ingested! 🎉


### Key Takeaways


- **Use performance analysis tools — don’t search “under the lamppost”** : False assumptions about where blocking occurs waste time. Profiling first gives correct focus and saves “side quests”. Running Clinic.js locally against real data was key to pinpointing the actual bottleneck.
- **Maintain a proper test environment** : Stable test setup with dashboards and the ability to easily clone production data into it — lets you reliably reproduce and benchmark issues, and test your solutions.
- **Add verbose debug logs** : When pods die mid-processing, logs may cut off — your logs give clues about where execution stops, and which parts of the code are unrelated to the blocking.
- **Consider partitioning or worker threads offloading for heavy work** : Libraries like[Piscina](https://github.com/piscinajs/piscina) can offload CPU-heavy tasks from the main event loop, which can be combined with or replace partitioning techniques.
However, it might require deeper knowledge to ensure all relevant contextual data is passed between the worker threads, and might not suit all use cases.
- **Rethink Node.js for CPU-intensive workloads when choosing your tech stack** : Node shines for I/O, not large in-memory serializations or heavy synchronous processing. While we were eventually able to successfully process those huge arrays — if it weren’t for Neo4j’s Streaming API, we might’ve had to consider refactoring our consumer into a more suitable language/framework.


#### Tools Used


Our stack:


- **Node.js + TypeScript** — consumer implementation
- **Kafka** — message broker
- **Neo4j** — graph database storing node clusters


The tools I used:


- **Clinic.js 🔥** — profiling and flamegraphs ( *doctor* + *flame* )
- **Grafana + Prometheus** — metrics dashboards and monitoring (reported with[prom-client](https://github.com/siimon/prom-client) )
- **Kubernetes + ArgoCD** — deployment and health checks infras
- [Conduktor](https://conduktor.io/) **—** message publishing tool for local debugging


#### Final Thoughts


Debugging event loop blocking can be cumbersome and require patience and careful profiling.
By systematically reproducing, logging, and using recommended profiling tools, we uncovered a deep driver-level serialization bottleneck that simple code optimizations couldn’t fix.


The database streaming solution worked great for us, and I hope this article can encourage you to dive into your own Node.js bottleneck, and maybe even encounter a similar root cause.


The lesson: **profile before you guess and refactor** , and **keep the event loop as non-blocked as possible** .


---


[Unblocking the Node.js Event Loop: Practical Troubleshooting of a Real-World Bottleneck](https://medium.com/riskified-technology/unblocking-the-node-js-event-loop-practical-troubleshooting-of-a-real-world-bottleneck-27aa5a3d2022) was originally published in[Riskified Tech](https://medium.com/riskified-technology) on Medium, where people are continuing the conversation by highlighting and responding to this story.
