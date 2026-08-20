---
schema_version: "1.0.0"
document_id: "e64ddd1f6980fb46589f0119c193ec0e302213d4825d35658c0ecc7e4b2a0677"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/introducing-the-mem0-plugin-for-pi-code"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-23T17:29:03.310285+00:00"
fetched_at: "2026-07-28T21:23:16.325685+00:00"
content_hash: "sha256:f9bab29d14a6a8330741a26322577a105c6a5212e4d6a66bcc91145735cebe1a"
---

# Introducing the Mem0 Plugin for Pi Code

Give Pi Code persistent, scoped, semantic memory across sessions and projects.


Pi Code is a terminal coding agent that can read your repo, make changes, run commands, and help you move through development tasks.


But like most coding agents, it should not have to start from zero every session. Project structure, preferences, prior decisions, migration patterns, and mistakes to avoid should carry forward.


Today, we are launching the Mem0 plugin for Pi Code: a Pi Agent extension that gives your coding agent long-term memory powered by Mem0.


Install it with:


```text


```


```text


```


```text


```


## What the plugin does


The Mem0 plugin gives Pi persistent semantic memory that works across sessions, projects, and devices. It can capture useful context from conversations, retrieve relevant memories by meaning, and help manage memory directly from the terminal.


The plugin includes:


-


Automatic memory capture from user and assistant messages


-


Semantic search, so memories can be found by meaning instead of exact keywords


-


Project, session, and global memory scopes


-


Monorepo-aware project detection using the git root


-


Dream consolidation to merge duplicates, resolve contradictions, and prune stale entries


-


Eight slash commands for memory management


-


An agent-accessible` mem0_memory` tool for search, add, update, and delete actions


The goal is simple: Pi should not have to relearn your working context every time you start a new session.


## Setup


First, sign up for Mem0 and get an API key from the dashboard:[https://app.mem0.ai/](https://app.mem0.ai/)


Then install the plugin:


```text


```


```text


```


```text


```


Set your API key:


```text


```


```text


```


```text


```


You can also configure the plugin with a local config file at:


```text


```


```text


```


```text


```


Example config:


```text


```


```text


```


```text


```


Environment variables such as` MEM0_API_KEY` and` MEM0_USER_ID` override the config file.


## Memory scopes


The plugin supports three scopes:


Scope


Filters


Use case


` project`


user + app_id


Default. Project-specific knowledge


` session`


user + app_id + run_id


Session-only context


` global`


user only


Memories across projects


Project scope is the default. The plugin detects the project using:


```text


```


```text


```


```text


```


That means subdirectories inside the same monorepo share the same project memory pool. If you work in multiple repos, their memories stay separated by default.


## Slash commands


The plugin gives you direct memory controls inside Pi:


Command


What it does


` /mem0-remember <text>`


Store a memory verbatim


` /mem0-search <query>`


Search memories semantically


` /mem0-forget <query>`


Search and delete memories, with confirmation


` /mem0-tour \[scope\]`


Browse memories grouped by category


` /mem0-dream`


Consolidate memories


` /mem0-pin <query>`


Protect a memory from dream pruning


` /mem0-scope <scope>`


Change the default scope for the session


` /mem0-status`


Check connection health, identity, project, and memory count


For example:


```text


```


```text


```


```text


```


The plugin also registers a` mem0_memory` tool that Pi can use directly. The tool supports actions including` search` ,` add` ,` get_all` ,` update` ,` delete` , and` delete_all` .


## What Pi can remember


Memories are classified into ten general-purpose categories: identity, preferences, goals, projects, decisions, technical, relationships, routines, lessons, and work.


For coding workflows, the most useful memories are often technical decisions, project context, preferences, and lessons learned:


```text


```


```text


```


```text


```


Once saved, those memories can be searched later by meaning, not just by exact wording.


## Dream consolidation


Long-term memory is only useful if it stays clean.


The plugin includes a dream consolidation workflow that reviews memories and helps merge duplicates, resolve contradictions, and prune stale entries. You can trigger it manually:


```text


```


```text


```


```text


```


The plugin can also run dream consolidation automatically when its configured gates are met. By default, the config enables automatic dream consolidation after minimum thresholds for time, sessions, and memory count.


If a memory is important and should be preserved, pin it:


```text


```


```text


```


```text


```


Pinned memories are protected from dream pruning.


## Why this matters


The best coding-agent workflows are not only about larger context windows. They are about carrying forward the right context.


Pi can already help inside your repo. With Mem0, it can also remember the parts of your workflow that should survive beyond one session:


-


how a project is structured


-


which conventions the repo follows


-


what decisions were already made


-


what the user prefers


-


what mistakes should not be repeated


-


what technical context matters across sessions


That makes every new session less like starting over and more like continuing the same working relationship.


## Get started


To use the plugin, create a Mem0 account, get your API key, install the package, and export the key in your shell:


```text


```


```text


```


```text


```


Then start using memory directly in Pi:


```text


```


```text


```


```text


```


Package:` @mem0/pi-agent-plugin`
