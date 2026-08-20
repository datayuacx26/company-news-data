---
schema_version: "1.0.0"
document_id: "73631108fddb1fd1badfc0b57af1e86a996de675652a60694c959fe11b26b6a9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-router-our-graphql-federation-runtime-in-rust"
published_at: "2021-11-10T11:15:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:35.155530+00:00"
content_hash: "sha256:b6b32d7ac146f670a9884e69e24f7de65efe465335301c1088db5eb191f3ac0d"
---

# Apollo Router: our GraphQL Federation runtime in Rust

At Apollo, we help developers power the world’s most important applications with a graph. Whether you’re[shopping online](https://medium.com/walmartglobaltech/federated-graphql-walmart-bfc85c2553de) ,[booking travel](https://medium.com/expedia-group-tech/apollo-federation-in-a-graphql-kotlin-server-115cea51607a) , or[binge-watching your favorite movies](https://netflixtechblog.com/how-netflix-scales-its-api-with-graphql-federation-part-2-bbe71aaec44a) , you’re using the graph every day.


These experiences are all built on[Apollo Federation](https://www.apollographql.com/blog/announcement/backend/announcing-federation-2/) , an architecture for declaratively composing GraphQL APIs into a unified graph. Scaling a graph to serve billions of users is no small task: the runtime that processes incoming requests and fetches data from underlying services needs to be responsive, efficient, and reliable.


To meet those needs, today we’re excited to unveil our next-generation GraphQL Federation runtime: **the Apollo Router** . The Apollo Router is written in Rust, and it is *fast* . ⚡️ Early benchmarks show that the Router adds less than 10ms of latency to each operation, and it can process **8x** the load of the JavaScript Apollo Gateway. Packaged as a standalone, multi-threaded binary, the Router can use all available CPU cores *without* needing to run multiple instances on a single machine.[Try the alpha release today](https://www.apollographql.com/docs/router/quickstart/) , or read on to learn how we’re measuring and continuously improving the Apollo Router’s performance.


## Why write the Apollo Router in Rust?


**Graph routing** is the critical component of Federation that accepts client requests, plans and executes those requests across subgraphs, and returns responses back to the client. The faster this happens, the more invisible this layer of infrastructure becomes.


The Apollo Gateway, built on Node.js, is stateless and horizontally scalable. Capable of serving demanding workloads, it powers some of the largest graphs in the world. The Apollo Gateway isn’t going away, and we recommend using it if you need production-ready graph routing software today. However, critical infrastructure software such as a graph router is most often written in a lower-level systems language that offers a smaller operational footprint and direct control over the underlying runtime mechanics.


When deciding on a systems language for the Apollo Router, Rust felt like the perfect choice. Developing a heavily multithreaded, asynchronous piece of critical infrastructure can present security concerns, which Rust helps to mitigate with strong memory and type correctness guarantees.


Using Rust to build the Apollo Router has many advantages over Node.js:


- **90% less variance in latency** than the Apollo Gateway thanks to Rust’s memory management techniques
- **A reduced attack surface area** due to fewer dependencies
- **Fewer memory bugs** thanks to the powerful Rust type checker
- **Over**[4x more energy efficiency](https://haslab.github.io/SAFER/scp21.pdf) for the same workload on average


Migrating from the Gateway to the Router will be straightforward. The schemas you use in the Gateway are compatible with the Router. It will natively support much of what requires Gateway customizations today, and a well-defined extensibility model will support bespoke integrations.


## Performance: Latency, throughput, and predictability


API performance has a direct impact on customer experience and overall cost-of-ownership, both in terms of operational overhead and financial cost. We want to give you a glimpse into how we’re watching performance and show some scenarios that demonstrate just how fast the Router is.


As the Router is built out, we’ve been profiling and maintaining:


- **Low-latency:** *The overhead of the Router on the request pipeline.* Federation adoption shouldn’t require sacrificing existing, hard-earned performance. In many organizations, these metrics are attached to service-level agreements (SLAs) and scrutinized carefully. A fast API should remain fast when the Router is introduced.
- **High-throughput:** *The number of requests per second.* The Router can be horizontally scaled to add capacity, but we think it should also maximize utilization of the underlying hardware. The more efficient an instance is, the more operational value it brings to your stack. The Router should handle the throughput of all applications.
- **Predictability:** *The variances in latency.* Keeping these percentiles (i.e., p50, p95, p99) more closely aligned means more consistent performance for more users. The Router’s latency and throughput may vary, but it should be consistent.


### Experiment setup


We’re making benchmarking a part of our CI/CD pipeline, and the early experiments we’ll walk through below already tell a promising story. We want to understand regressions as soon as they happen and long before they end up in users’ deployments.


Benchmarks should mimic real-world scenarios as much as possible, including common deployment patterns and typical network topography. We’re deploying our tests on dedicated Kubernetes clusters so we can more accurately represent configurations that users have in their own production environments.


Within each test Kubernetes cluster we never co-locate our concerns. There are three dedicated *nodes* ([E2-family](https://cloud.google.com/compute/docs/general-purpose-machines#e2_machine_types) instances, running on Google Kubernetes Engine) as container hosts:


1. A node for the Router deployments — an e2-standard-8
2. A node for the subgraph deployments — an e2-standard-8
3. A node for the virtual users (i.e., clients) — an e2-standard-16; double that of other nodes to avoid bottlenecking


For load generation, we use Gatling, configured in the open workload model and Telegraf reporting to Grafana Cloud to measure the test results and overall cluster metrics.


As we’ll demonstrate below, the performance of the subgraph has a material impact on throughput, so we’ve also implemented those subgraphs using Rust to improve their overall capability. We’ll use the Rust subgraphs even when testing the Apollo Gateway.


Because the Apollo Gateway is a single threaded Node.js application, we run *six instances* to reliably saturate an 8 vCPU Kubernetes node; conversely, a single Apollo Router instance can fully saturate all of the resources on a node.


### Apollo Router vs. Apollo Gateway benchmarks


Let’s walk through four benchmark scenarios and look at how the Apollo Router compares to the Apollo Gateway in various tests:


- A query against a single subgraph
- A query against a single subgraph with delays
- A larger query against a single subgraph with delays
- A larger query against a number of subgraphs with delays


In each of these tests, we’ll compare the performance of:


- Querying the Apollo Router (Rust)
- Querying six Apollo Gateway instances (JavaScript)
- Querying a federated subgraph *directly* , when possible


#### A small query against a single subgraph


Often when an organization first adopts Federation, they start by putting the Router in front of their existing monolithic GraphQL API. Here’s how performance varies for this “base case” federated graph when executing a query for a single field:


The results here are astonishing: up to about 3,000 requests per second (RPS), the Apollo Router (the pink line) barely adds *any* *latency* when compared to querying the monolithic graph directly (the black line). Even up to 19,000 RPS, the Router never adds more than 5ms. Meanwhile, the JavaScript Apollo Gateway *starts* at more than 5ms of latency and can’t handle more than 3,000 RPS *at all* . At least in this basic test, the Router shows massive performance gains. Now, let’s look at the results of some more complex tests.


#### A small query against a single subgraph with delays


Usually a subgraph needs to perform additional computational work (such as reading from a database) to resolve an operation. To simulate the delay introduced by this work, we ran the same test as above but added a flat 40ms to our subgraph’s execution time. Let’s see how this affects performance:


Once again, the Apollo Router introduces minimal latency (less than 5ms) even up to 19,000 RPS. This is all the more impressive when we see that this time, the Apollo Gateway’s performance becomes erratic even earlier, at around 1,330 RPS. (Note that the latencies on the Y axis are all 40ms higher than the previous chart, reflecting the additional synthetic latency.)


Upstream subgraph latency adds pressure to the overall pipeline. As with any intermediary proxy, the longer upstreams take to respond, the more concurrent requests need to be tracked and maintained. Since we’re using the open-workload model, the virtual users continue to arrive!


#### A large query against a single subgraph with delays


The number of fields in a request has an impact as well. While queries come in many sizes, we see immediately that requesting 100 fields affects the overall throughput:


We see the Router serving 12,000 RPS with its P95 latency overhead still measuring at about 1 to 3 milliseconds. On the other hand, the Gateway latencies start increasing immediately, and it achieves about 1000 RPS before starting to spike sharply.


Both the Router and the six Gateway instances are challenged with additional data to process and serialize/deserialize. This is compounded by subgraph latencies forcing the retention of data structures in memory. However, the Router can manage 12x the throughput before becoming unstable.


#### A large query against multiple subgraphs with delays


Federation is all about combining data from multiple subgraphs. For this test, we execute a contrived 100-field query that performs 6 sequential subgraph fetches, each with a 40ms delay. The six Gateways still don’t handle this well and the single instance of the Router shines:


Because we’re now querying multiple subgraphs on each request, we’re no longer able to accurately measure the overhead when compared to fetching directly against a single subgraph, which is why the “direct” line doesn’t appear on the graph. And because we’re sequentially fetching from six subgraphs with 40ms of latency apiece, we can’t achieve a latency lower than 240ms.


The single Apollo Router achieves almost 9x the throughput of the six Gateways.


## Apollo Router performance recap


The Apollo Router is extremely fast in all of these experiments, often adding only 1-2 milliseconds of latency. It’s also far more reliable and consistent when compared to the Apollo Gateway. Much of this is thanks to Rust’s multi-threaded nature, better memory management, lack of a runtime/garbage-collection, and libraries that enable high-performance network I/O. The performance results above highlight how subgraph latency, number of fields, concurrent requests, and subgraph fetches all influence the overall response time.


Both the Router and the Gateway can be horizontally scaled to infinite workloads. It’s only through running tests in your own environment, with your own specific use case, that you can confidently know how things will operate. OpenTelemetry and Apollo Studio can help you identify where time is being spent for each request so you can tune performance.


The Apollo Router team is committed to benchmarking, observing, and learning from conditions like these, and we’re thrilled to be working in a language like Rust that provides fine-grained OS-level control over the Router’s behavior.


As we move closer to maturity, we’re excited to see customers’ own comparisons of the Router’s performance and how it compares to the Gateways they’ve run previously. We look forward to enabling you to make these comparisons by offering perspective into our future benchmarks.


## Getting started


- [Try out the Apollo Router](https://www.apollographql.com/docs/router/quickstart/)
- [Let us know what you think!](https://community.apollographql.com/)
- [Get involved on GitHub](https://github.com/apollographql/router)


## We’re hiring! 🦀


Finally — it’s incredibly exciting to see what the Apollo Router team has been able to do building this incredible product in Rust. A huge thanks goes out to that team: Bryn Cooke, Cecile Tonglet, Jeremy Lempereur and Geoffroy Couprie.


Does building highly-performant, fault-tolerant, Rust infrastructure software sound interesting to you?[We’re hiring!](https://jobs.lever.co/apollographql/d7c1d4f1-f2ad-41b0-a43b-b9f70d934ec3)


Written by


Jesse Rosenberger


[Read more by Jesse Rosenberger](https://www.apollographql.com/blog/author/jesse)
