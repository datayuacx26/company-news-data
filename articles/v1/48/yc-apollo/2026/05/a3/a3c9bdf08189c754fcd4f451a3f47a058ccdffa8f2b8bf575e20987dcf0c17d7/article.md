---
schema_version: "1.0.0"
document_id: "a3c9bdf08189c754fcd4f451a3f47a058ccdffa8f2b8bf575e20987dcf0c17d7"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-federation-goes-full-rust"
published_at: "2026-05-07T17:50:09+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:7bd4a29c0645c3e3f83838d0cd541704732643590362a9870495b72ae6a51c69"
---

# Apollo Federation Goes Full Rust

Today we’re releasing a preview of Rust-native composition for Apollo Federation. We are close to the finish line of a multi-year effort to replatform Apollo Federation, both the query planner and the composition engine, from JavaScript/TypeScript to Rust. You can try it now by enabling the **Federation Next** build track in[GraphOS Studio](https://studio.apollographql.com/) .


## Why Rust


When we shipped the[Rust-native query planner](https://www.apollographql.com/docs/graphos/routing/query-planning/native-query-planner) over a year ago, the performance and reliability improvements were immediate.[Composition](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/composition) was the obvious next target, and the last major Federation component still running in JavaScript.


The JS composition engine relied on an embedded Deno runtime and V8 to execute. That meant garbage collection pauses and high memory consumption for large graphs. Rust eliminates that entire class of overhead, and its predictable memory model handles graphs with hundreds of subgraphs comfortably.


Beyond performance, this unifies our codebase. The query planner and composition engine now live in a single Rust monorepo. We no longer need to duplicate bug fixes across two languages, synchronize spec changes between JS and Rust implementations, or manage complex cross-language dependency chains. That simplification lets us iterate faster on Federation features going forward.


## Performance


We benchmarked the Rust implementation against the JS implementation across tens of thousands of real-world production graphs. The results are significant. At the median, Rust composition is roughly **27x faster** . At p95, it’s about **8.5x faster** . The gap narrows at higher percentiles because the largest graphs stress both implementations, but Rust still delivers a **6x improvement** at p99.


These numbers are pre-GA and measure composition in isolation. Your experience in Studio may differ since composition is one step in a larger build pipeline. That said, these results are with minimal dedicated optimization work on the Rust side, so there’s room to go further.


## How We Built It


Composition is a multi-stage pipeline. Rewriting all of it at once would have been risky. A single bug in any stage could produce incorrect supergraph schemas. Instead, we built a hybrid pipeline that let each stage run independently in either JavaScript or Rust.


This allowed us to develop and validate one stage at a time, comparing JS and Rust output at every boundary to catch regressions before they compounded. In production, we ran the Rust implementation in shadow alongside the existing JS pipeline, processing the same graphs in parallel so we could validate correctness and performance against real-world workloads without any risk.


## How We Validated It


Correctness was the bar we had to clear before anything else. A composition engine that produces fast but wrong supergraph schemas is worse than useless. It’s dangerous.


We tested against tens of thousands of real-world production graphs from our corpus. For every graph, we ran composition through both the JS and Rust implementations and compared their outputs at each stage of the pipeline. This wasn’t just “does it produce a schema.” We compared the full output including the supergraph schema, composition hints, and error messages.


To make these comparisons meaningful, we normalized for known cosmetic differences between the implementations. The JS and Rust compilers handle whitespace in schema descriptions differently, and JS relies on automatic type coercion for default values where Rust is explicit. We accounted for these so we could focus on semantic correctness rather than surface-level formatting noise.


At the time of this preview release, the Rust implementation produces identical results to JS on **the vast majority** of the corpus with the majority of differences appearing in hints or error messages rather than in the composed schemas themselves. A meaningful number of schema differences are cases where the Rust implementation is actually more correct. Any remaining differences are actively categorized and will be resolved before the GA release.


## Try It


You can test the Rust composition preview today by enabling the **Federation Next** build track in[GraphOS Studio](https://studio.apollographql.com/) . This will run your graph’s compositions through the Rust pipeline.


We want to hear from you. If you encounter any differences in composition behavior, please report them through your usual support channel or file an issue on the[Apollo Router GitHub repo](https://github.com/apollographql/router) .


## What’s Next


We’re working toward general availability, which means resolving the remaining equivalence gaps, completing our production shadow validation, and ultimately removing the JS fallback path entirely.


We’re also porting contracts, the last piece of Federation tooling still in JavaScript, to Rust. Once that’s complete, the entire Federation stack will run natively.


## Acknowledgments


This was a multi-year effort done right. Rewriting, validating and shipping a system that processes schemas for thousands of production graphs demands that kind of rigor.


We’re grateful to everyone across the Apollo team who contributed, and to the community members whose large, complex real-world graphs continue to push us to make Federation better. Those graphs are both our hardest test cases and our strongest motivation.


Written by


David Walter


[Read more by David Walter](https://www.apollographql.com/blog/author/dwalter)
