---
schema_version: "1.0.0"
document_id: "086170b6c92bba0f732267e5a0ffd1b102e5631b45fba725837090b77785183f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/enforce-schema-consistency-across-federated-graphs-with-graphos"
published_at: "2023-08-24T15:37:06+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:4151b81cc9e00713315fcb539372c23d6d10c1ad9ae045bebd4b8ea1c054e4bf"
---

# Enforce schema consistency across federated graphs with GraphOS

When you’re building a federated GraphQL API that spans multiple backend teams and services, it can be difficult to ensure that the schema for your graph stays consistent from subgraph to subgraph. For example, how do you ensure that a type defined as an entity in one subgraph is also defined as an entity in all other subgraphs?


Today, we’re making it easier for API platform teams to enforce schema consistency across multiple subgraphs by adding composition rules to the[Apollo GraphOS linter](https://www.apollographql.com/blog/announcement/platform/standardize-graphql-schema-linting-policies-with-graphos/) . Now, you can configure the GraphOS checks workflow to flag inconsistent elements across subgraphs, overridden or unused elements, and issues with Federation directives.


## Configure composition rule severity


Like other linter rules in GraphOS, you can enable or disable each composition rule independently and set its severity to` Warn` or` Error` . Violations of rules set to` Error` will cause the checks workflow to fail outright (useful for changes pushed to production graphs) while` Warn` violations will allow the checks workflow to pass but output a warning.


## Integrate with development workflows


Composition rules are enforced by GraphOS linter checks, which you can integrate into your existing delivery processes:


1. **CI/CD pipelines** — Centrally enforce composition rules in your existing CI/CD pipelines by[installing Rover in your CI/CD environment](https://www.apollographql.com/docs/rover/ci-cd/) .
2. **The GraphOS registry** — Enforce composition rules for all schema changes pushed to the GraphOS registry — the final checkpoint before your schema is used in production.


## Get started


Composition rules for the GraphOS schema linter are available starting today for all GraphOS users. To get started, head to the Checks Configuration page for your graph in GraphOS Studio or[read the docs](https://www.apollographql.com/docs/graphos/delivery/linter-rules/#composition-rules) for a step-by-step setup guide.


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
