---
schema_version: "1.0.0"
document_id: "c9ced887ad5d49bf89e0046370d4e1e795e0546069498d5ffa96e356c009cb29"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-new-in-rover-docker-image-schema-search-client-checks-and-more"
published_at: "2026-07-17T11:24:07+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:0e1c326b5bd1106368908637efa4b31bf876b48d54f834281e391d74d043827c"
---

# What’s New in Rover: Docker Image, Schema Search, Client Checks, and More

Rover is Apollo’s CLI for managing GraphQL schemas, running checks, and working with the GraphOS registry. It is the primary tool most teams use to integrate graph operations into their development workflows and CI/CD pipelines.


This quarter we shipped a set of improvements that have been on the roadmap for a while. Some close long-standing gaps in Rover’s CI/CD story. One completes the feature parity work that has blocked teams from fully migrating off the deprecated Apollo CLI. And two new commands expand what Rover can do for teams building AI agents on their graphs.


Here is what shipped.


## **Official Docker image**


Starting with Rover v0.39.1, Apollo publishes an official Docker image with every release. If you are still installing Rover via curl script or maintaining your own container build in CI, we recommend switching to the official image. Each version tag is immutable, so you can pin to a specific version with a guarantee that the artifact will not change.


```text
docker pull ghcr.io/apollographql/rover:0.40.0
docker run ghcr.io/apollographql/rover:0.40.0 subgraph check my-graph@prod \
--name products \
--schema ./schema.graphql
```


The image is available on GitHub Container Registry (` ghcr.io/apollographql/rover` ). All CI platforms that support running steps inside a container image (CircleCI, Bitbucket Pipelines, GitLab CI/CD, Jenkins with a Docker agent) can reference it directly.


## **Atomic check and publish with` --check`**


Rover has always supported running schema checks and publishing schemas as separate commands. Most teams who wanted a safety gate in their pipeline had to wire those steps together manually: run the check, inspect the exit code, conditionally run the publish, handle failures.


A` --check` flag on` rover graph publish` collapses that into one step. Pass it and Rover runs all configured checks against the registry first. If checks pass, the publish proceeds. If checks fail, the publish is aborted and the command exits non-zero.


```text
rover graph publish my-graph@production \
--schema schema.graphql \
--check
```


## **` rover client extract` and` rover client check` : native client operation checks**


This is the update that completes the Apollo CLI migration story.


The Apollo CLI has been deprecated for some time, but one workflow kept teams on it:` apollo client:check` . That command validates GraphQL operation files against the GraphOS registry to catch breaking changes before they reach production. Rover’s client commands now cover that workflow.


` rover client extract` scans your source files for embedded GraphQL documents (it detects` gql` and` graphql` tagged templates in TypeScript, and triple-quoted strings in Swift and Kotlin) and writes each one to a standalone` .graphql` file.` rover client check` then validates those files against your graph’s published schema in GraphOS.


Run them in sequence:


```text
rover client extract --out-dir graphql
rover client check my-graph@staging --include "graphql/**/*.graphql"
```


` client check` supports` --include` and` --exclude` patterns, so you can point it at exactly the extracted output you want validated, and it reports any operation that references a field that’s been removed or renamed before that change reaches production. If you have been holding onto the Apollo CLI for` apollo client:check,` this is your migration path.


## **` rover schema describe` and` rover schema search` : schema intelligence for agents**


These two commands are different in character from the others. They are not about CI/CD. They are about making the schema queryable by agents and developers from the command line, with no additional infrastructure.


As teams start building AI agents on top of their graphs, a practical problem has emerged. The schema is in the registry, but there was no CLI-native way to explore it. Agents working on graph-related tasks had to either parse raw SDL or depend on a running MCP server to access schema intelligence.


` rover schema describe` solves exploration. You give it a schema coordinate and get back everything you need to understand that part of the schema: the type or field definition, paths from query and mutation roots, and the context needed to write a query against it.


```text
# Schema overview
rover schema describe schema.graphql


# A specific type
rover schema describe schema.graphql:Order


# A specific field, with paths from query roots
rover schema describe schema.graphql:Order.status
```


` rover schema search` solves discovery. You give it a keyword and it returns complete root-to-field paths for every match, including all the intermediate types needed to write a query.


```text
# Search a local schema file
rover schema search schema.graphql "payment status"


# Search against a registry graph by piping
rover graph fetch my-graph@current | rover schema search - "payment status"
```


The output of both commands is designed to pipe cleanly. When used in an automated workflow, the results give an agent enough context to go from a vague user request to an executable GraphQL query without human guidance and without a running MCP server. Fetch the schema, search for the concept, describe the field, write the query.


## **What’s next**


These updates reflect where Rover is headed: more reliable in production pipelines and more useful beyond schema management for the agentic workflows teams are building.


A few things to do now:


- Switch your CI/CD pipelines to the[official Docker image](https://www.apollographql.com/docs/rover/getting-started) if you are still using a curl install or a custom container.
- Add` --check` to your` rover graph publish` commands to get atomic check-and-publish in a single step.
- If you are still running` apollo client:check` , test` rover client extract` and` rover client check` against your operation files and start planning the migration.
- If you are building agents on your graph, try` rover schema describe` and` rover schema search` and share feedback in the[Apollo Community](https://community.apollographql.com/) .


The full changelog is available in the[Rover release notes](https://github.com/apollographql/rover/releases) .


Written by


Kaitlyn Barnard


[Read more by Kaitlyn Barnard](https://www.apollographql.com/blog/author/kbarnard)
