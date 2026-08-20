---
schema_version: "1.0.0"
document_id: "81bb286110290e628ad904ec7ba79186f1b5b3cc8ffbb98011e28de495fcde6a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-contract-launches-see-the-lifecycle-of-contract-schemas-in-apollo-studio"
published_at: "2022-02-17T08:56:17+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:e1eaaadfc00515294aeb94a774488e6fb924858a42bcda8b210fb955b1bf150c"
---

# Introducing Contract Launches: see the lifecycle of contract schemas in Apollo Studio

Since releasing[contracts](https://www.apollographql.com/blog/announcement/platform/introducing-contracts-serve-many-audiences-with-a-unified-graph/) as a way to serve many audiences with a unified graph, we’ve received great feedback from developers adopting contracts and championing the graph. However, one question we frequently hear is: “how can I reason about contracts with respect to my publishing pipeline?”


As of today, we are thrilled to introduce Contract Launches in Apollo Studio to answer this very question. With Contract Launches, you can use the Launches page in Studio that provides better visibility into the lifecycle of your contract schemas and helps you understand which contracts launched from a given source variant.


## Lifecycle of a Contract Launch


Every contract is built from an existing source variant. Publishing updates to your source variant triggers downstream launches for any contract built from that source.


Let’s look in more detail what this looks like:


1. You publish an update to your source variant
2. A launch initiates for the source variant
3. If the launch was successful a core schema is the output artifact
4. The core schema is then approved and made available to gateways
5. A launch for each contract is initiated


1. The core schema is used as the build input to contract launches


6. The schema is then filtered into a contract schema
7. If the launch was successful a contract schema is the output artifact
8. The contract schema is then approved and made available to gateways


The lifecycle of a contract launch


## Launch status


For any launch that triggers a contract launch, you can see a summary of how many downstream launches succeeded or failed:


You can also take a look at the Launch Sequence to see the status of each contract launch individually. To debug a failed launch, you can navigate to the Launches page for a contract by clicking the desired contract launch item.


A contract launch that completes successfully is made available to running gateways via Uplink.


⚠️ **Note:** Checks for contracts are not currently supported but are in active research and development.


## Reviewing changes


Any update to your graph will also update your contracts. To help you keep track of these changes, there’s a convenient link to the contracts changelog. The changelog page shows you any types or fields from your contract schema that have been added, removed, or modified.


## What’s next


Apollo Contracts is currently in public preview on Apollo’s Enterprise plan. Feedback from our community and customers is crucial to the success of Apollo, and the feedback we’ve received from pilot users so far has been instrumental in ensuring that contracts can serve many use cases while still giving teams full visibility and control over changes.


If you’re interested in learning more about Contracts, visit the[documentation](https://www.apollographql.com/docs/studio/contracts/) . We’re excited to see what you’ll build!


Written by


David Castaneda


[Read more by David Castaneda](https://www.apollographql.com/blog/author/davidcastaneda)
