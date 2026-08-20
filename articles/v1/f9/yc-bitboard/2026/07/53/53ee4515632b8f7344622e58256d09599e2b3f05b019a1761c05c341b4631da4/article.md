---
schema_version: "1.0.0"
document_id: "53ee4515632b8f7344622e58256d09599e2b3f05b019a1761c05c341b4631da4"
company_key: "yc-bitboard"
company: "BitBoard"
source_id: "yc-bitboard-news-import-cd518ead63b3"
canonical_url: "https://bitboard.work/blog/agent-ergonomics/"
published_at: null
first_seen_at: "2026-07-21T10:23:59.081525+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:b06b64de15bee1f2941b5cfcd2de00353039a2e3b316b7cfcb188aea0614597b"
---

# Data Agent Ergonomics

Coding agents like Claude Code and Codex are getting good at analysis. They handle the syntax, do the transforms, explain what they find, and anyone on the team can run them without waiting on someone else. The problem is the output is hard for the team to trust.


Someone runs an analysis in chat, gets a number, and tries to share it - but the code goes in git, the number goes in Slack, and the reasoning gets lost in between. They ask for a change and the agent starts over. They come back a day later to check the logic and the chat is gone. The answer might be right, but nobody can tell.


And they can be inefficient. The agent re-runs pipelines that didn’t change. It drags data through the context window when a pointer would do. It probes schemas one tool call at a time before it can write a single query. The cost adds up, and the budget left for reasoning shrinks.


When the work comes back, the team ends up asking two questions: can we trust this (accuracy), and was it worth the cost to get here (efficiency).


We think of accuracy as two halves: what the agent does, and what the team can verify. The agent needs to trace its claims back to the data and write transformations the dashboard can re-run. The team needs to follow the reasoning, add data, and redirect when the agent is off. Without both halves, you have an answer nobody can check.


Efficiency is about getting to a known answer with the minimum effort and time. Bare agents waste a lot. They re-run pipelines that didn’t change, drag data through the context window, and burn tool calls probing schemas one at a time. Good tooling does most of that work for them, and the savings compound into more budget for reasoning.


The next two posts go deeper on each:


- **Accuracy.** How we make agent work verifiable and extendable by the team.
- **Efficiency.** How a data graph, pointers instead of payloads, and composable typed tools cut agent waste.


We’re building BitBoard to solve these problems. We expect to learn a lot about making agents useful along the way. If this interests you, BitBoard’s tool reference, schemas, and example flows are live at[docs.bitboard.work](https://docs.bitboard.work/) .
