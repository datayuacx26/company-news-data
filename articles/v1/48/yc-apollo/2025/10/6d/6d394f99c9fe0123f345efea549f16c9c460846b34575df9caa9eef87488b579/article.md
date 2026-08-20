---
schema_version: "1.0.0"
document_id: "6d394f99c9fe0123f345efea549f16c9c460846b34575df9caa9eef87488b579"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/subgraph-api-keys-and-safer-deployment-pipelines"
published_at: "2025-10-07T07:00:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:90d6bb6583cd006b5fc78bc770f20f959d52a62ef452eab522bd2fa45e1db797"
---

# Subgraph API Keys and safer deployment pipelines

We’re thrilled to announce the release of[subgraph API keys](https://apollographql.com/docs/graphos/platform/access-management/api-keys#subgraph-api-keys) !


Subgraph API keys are designed to scope permissions to a specific subgraph, or set of subgraphs, within your organization’s graph, and are ideal for CI/CD workflows. Until now, individual development teams have used Graph API keys in their deployment pipelines, giving them full access to a federated graph. In contrast, Subgraph keys provide more narrowly scoped permissions to only a select set of subgraphs, improving security and reducing risk.


## What customer problems do subgraph API keys solve?


API keys allow GraphOS to identify actors and the permissions those actors have in the system.


Until now, Apollo customers have typically used a graph API key to interact with GraphOS in their build and deployment pipeline automation. However, graph API keys are scoped to an entire federated supergraph. This means that in the worst case, Team A could push a change to their pipeline and accidentally make changes to Team B’s subgraph. As the number of individual teams contributing to a federated graph grows, the risk of a team making changes outside its area of ownership grows as well.


For many customers, graph API keys grant too many permissions at too high a level. This means they don’t follow the “principle of least permissions”, a security best practice for mature organizations.


## What are subgraph API keys?


Subgraph API keys are designed to scope permissions to a specific subgraph, or set of subgraphs, within your organization’s graph. When you create a subgraph key, you specify the desired subgraphs (and their variants) that you want to be able to change. Using a subgraph key therefore ensures that only the specified subgraphs can be modified when this key is used.


Subgraph keys are ideal for use in CI/CD pipelines and deployment workflows, especially in cases where the ownership of multiple subgraphs is spread across multiple autonomous teams. These keys have the specific permissions needed for CI/CD workflow management, allowing you to run checks against your supergraph, and to publish subgraph schema changes.


Subgraph API keys are available in[GraphOS Standard and Enterprise plans](https://www.apollographql.com/pricing) . Users who have the Org Admin or Graph Admin role can create and manage subgraph keys.


Figure: Subgraph API keys grant permissions at the Subgraph level


## How do I use a subgraph key?


1. Create a subgraph key using Rover or the Platform API. (Stay tuned for key management in Studio in the near future!) Note that subgraph keys are immutable. You must list all subgraphs and variants you want the key to work against when you create the key.
2. Store the key token in your secret manager.
3. Create a pipeline, or update your existing pipeline, to use your new key.


Figure 2: List subgraphs and variants for the API key


You’re all set! You can now test your pipeline workflow with confidence, knowing that each deploy is scoped to only your specified services.


Subgraph API keys are a meaningful improvement in letting teams right-size their permissions and the first stepping stone on our path towards fine-grained permissions! Get started today by[updating an existing pipeline](https://www.apollographql.com/docs/graphos/platform/schema-management/delivery/publishing) to use a subgraph key, or by[creating your subgraph pipeline from scratch](https://www.apollographql.com/docs/graphos/platform/schema-management/delivery/publishing/guide-github) .


Written by


Susannah Kirby


[Read more by Susannah Kirby](https://www.apollographql.com/blog/author/skirby)
