---
schema_version: "1.0.0"
document_id: "781370c8c17e5ac3c935c6671f77787673124957b41ee0c817ef2178ecd9df80"
company_key: "yc-relace"
company: "Relace"
source_id: "yc-relace-news-import-8821aa3570c8"
canonical_url: "https://relace.ai/blog/relace-repos"
published_at: "2025-10-16T00:00:00+00:00"
first_seen_at: "2026-07-24T11:35:43.760091+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:88b7a9ec4f4e4fa14731c21e31979335f4c3984da293307773981dabc87d88b5"
---

# Introducing Repos

Relace Repos is a source control system designed with AI agents as the intended user. It's fully git compatible, supports high frequency read/write operations, and deeply integrates with Relace models.


As a bigger proportion of code gets written by autonomous coding agents, a new category of developer tools will have to be built to support them. We're tackling source control first.


## GitHub for Agents


AI agents code in fundamentally different ways from humans. They require orders of magnitude higher throughput for filesystem operations and don't interact with visual user interfaces at all.


To securely execute tool calls, agents also run in short-lived sandboxes protected from code injection attacks. Proper user segmentation requires equipping each sandbox with a set of fine-grained permissions for what repositories it can edit.


Traditional source control systems like GitHub aren't built with these usage patterns in mind. GitHub is built to support a rich UI, enforces an enterprise limit of 100k repos per organization, and allows no more than 15k requests/hour for clone/push/pull operations.


We built Repos to be convenient for agentic workflows:


-


**Native git operations.** Standard commands like` git clone` ,` git commit` , and` git push` are supported. Spin up a sandbox, clone the repo, and use git commands to keep your source synced as the agent makes changes.


-


**Permissive rate limits.** Support for high frequency git operations across dozens of concurrent agent runs. Focus on maintaining low latency for bottleneck operations like clone, push, and pull.


-


**Sandbox-optimized security.** Fine-grained repository permissions with scoped API keys designed for ephemeral containers. Each sandbox gets exactly the access it needs.


## Codebase Retrieval


More and more, context engineering is becoming the key for boosting performance of agents. People have developed techniques like to-do lists, selective context compaction, and recursive language models to name a few.


At the core, however, every coding agent must still traverse its underlying codebase to locate the files most relevant to a user’s query. In our experiments, this step alone accounted for over 50% of token usage in an agentic traces.


###### **Figure 1:** Distribution of percentage of tokens spent on search across 1200 coding tasks produced from real vibe coding requests.


You can equip your agent with a[semantic search tool](https://relace.ai/docs/code-reranker/agent) as a first pass to seed the subsequent agentic search with the most relevant leaves in the file tree. Combining this with the usual file view and grep tools significantly cuts down on extraneous files viewed by the agent.


Fast, accurate semantic search that remains synchronized as the agent edits code requires a lot of infrastructure. Repos abstracts away that complexity with built-in two-stage retrieval.


### Auto Indexing with Relace Models


Precomputing codebase embeddings and doing vector search against a user query is fast, but low accuracy. Rerankers, on the other hand, deliver better accuracy but are computationally expensive, since retrieval cost scales with codebase size.


To run the reranker on an *entire codebase* means you need to aggresively limit the number of parameters for the model, which affects performance.


The best approach is a hybrid system, where a vector search is used to provide a reduced set of candidate files for a reranker.


Building this kind of system offers several challenges:


-


Embedding and reranker models have a hard limit on the size of input files, which means large files need to be chunked


-


Embeddings need to be stored in a vector database, and kept in sync with your source code


-


Computing embeddings is slow; this means it must be done asynchronously, exposing potential race conditions


Relace Repos handles all of this complexity for you and makes it viable for us to use 10x bigger embedding and rerank models at the same cost and latency.


###### **Figure 2:** Relace Retrieval Bench consists of medium-sized vibe-coded apps (50–500 files) with ground truth taken from Claude 4 Sonnet. GitHub Issues are large open-source repositories (500+ files) with ground truth taken from committed changes.


## What's Next?


Repos sets the foundation for our ability to co-optimize our suite of small, specialized coding models with low overhead infrastructure. So far, our models have been focused on core, non-agentic capabilities.


Over the next few months we'll be releasing our suite of Relace agents trained for end-to-end tasks like quickly traversing codebases, automatically merging conflicts, and intelligently refactoring codebases.


###### **Figure 3:** Upcoming specialized subagents for merging, searching, and refactoring codebases.


With Repos, these subagents will be as easy to integrate into agentic flows with commands like repo.search(), repo.merge(), and repo.refactor(). We believe models need to be co-optimized with the best infrastructure to take vibe coding to the next level.


If you're interested in getting started with Repos, read more in our[docs](https://docs.relace.ai/docs/repos/overview) or reach out tosupport@relace.ai .
