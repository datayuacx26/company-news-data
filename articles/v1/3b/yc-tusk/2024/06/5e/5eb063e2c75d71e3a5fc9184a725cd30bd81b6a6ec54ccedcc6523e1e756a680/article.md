---
schema_version: "1.0.0"
document_id: "5eb063e2c75d71e3a5fc9184a725cd30bd81b6a6ec54ccedcc6523e1e756a680"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/release-of-semantic-graph-and-memory"
published_at: "2024-06-13T15:32:14+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:e256a83fa35df7a28830b60a17333a3af1b000470f21831ba3ffa204cb9c549c"
---

# Release of Semantic Graph and Memory

# Introduction


We've improved Tusk's codebase navigation and given it the ability to learn autonomously from past merged PRs.


Check out the video below showing how these improvements assisted in solving a user-reported bug with our marketing website -- all from a Loom video.


Your browser does not support the video tag.


## Why This Matters


Reliability is still one of the main issues plaguing AI agents in the wild. Our team knows this better than anyone because Tusk pushes code to prod for customers with mature, complex codebases.


As such, we're always looking for step-level improvements in code generation quality. Proud to show off some new agent improvements that have achieved this leap for our AI agent.


## What to Expect


🧑‍💻 Tusk now refers to an **abstract semantic graph** when navigating your codebase for context. This semi-programmatic approach allows Tusk to more reliably find files and symbols that are relevant to your engineering task.


🧠 Tusk now uses past merged PRs as **long term memory** to solve future tasks. This replicates how humans recall completing similar tasks in the past when given a new task. This is especially useful for recipe-based tasks that are repeatedly requested in your app.
