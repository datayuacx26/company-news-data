---
schema_version: "1.0.0"
document_id: "fac6376f0a4a429a90f9d25e5338bf7890e52c656dfb71fb74c5137457441aee"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/why-codebase-context-in-markdown-files/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:49f4a604330a1077dd98fea9e3cc76c2802a2da17affa6492fe1032e16eb2055"
---

# Why are You Still Putting Codebase Context in Markdown Files?

# Why are You Still Putting Codebase Context in Markdown Files?


Every team that adopts AI coding tools at scale ends up maintaining markdown files. It's the right problem and the wrong solution.


Mar 20, 2026 — 10 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


*Part 3 of 5 · Previous:[Claude and Cursor Are Smart, But Not Enough to Solve Your Codebase Context Problem](https://www.driver.ai/blog/claude-and-cursor-smart-not-enough-codebase-context) · Next:[The DIY Context Trap](https://www.driver.ai/blog/diy-context-trap-codebase-context-infrastructure)*


One of our customers — a developer at a fintech company with a large Ruby monorepo — described his workflow to us on an onboarding call. He opens a fresh Cursor or Claude Code session. He needs to make a change. But the agent doesn’t know how the codebase is organized, so he starts pointing it at directories manually: “the logic is in these paths,” “this framework handles that.” He runs a separate AI session just to extract how the current behavior works, outputting the results to a markdown file. Then he writes up the requirements for his change in another markdown file. Then he starts a third session, feeds both markdowns in, and begins the actual work.


Every time. Every task. We hear versions of this from customers constantly.


## The Markdown Phase


What I’ve found is that teams go through a remarkably consistent progression once they realize codebase context is the bottleneck.


It starts with markdown. A developer writes a document. Could be a codebase map that summarizes how the repository is organized, or a technology overview listing languages, frameworks, and third-party packages so the agent stops introducing new dependencies. Maybe it’s a style guide, or a description of a domain concept that lives in the code but needs business context to make sense. They check it into the repo. Other engineers start using it. It helps.


Then more documents. A` product.md` describing what the software is for, who uses it, what the core features are. A` CLAUDE.md` with rules for the agent. Skills that encode architectural patterns. Before long, you’ve got a constellation of purpose-built context artifacts scattered across your repository, each solving a different slice of the problem.


And then the maintenance wall hits.


Every time the codebase changes — a refactor, a new feature, a directory restructure — someone has to update these documents. But the documents are holistic: they describe the whole codebase. So any change to the codebase potentially invalidates any document. Multiply that across a team, and you get merge conflicts, stale information, and engineers spending meaningful time maintaining context artifacts instead of building the product.


Why would you want your teams spending a lot of time producing context? It’s not an end result in itself. It’s a “so that we can” kind of solution — you develop this context so that you can go do what you’re actually trying to do. It’s not the outcome you’re driving towards, so it really is wasted effort.


## Why Search Doesn’t Solve It


The natural next step is to try automating the retrieval. If maintaining context documents is too expensive, maybe you can search for what you need at query time. This is the intuition behind RAG — chunk the codebase, embed it in a vector database, retrieve relevant fragments when the agent needs context.


RAG does some things well. If the question is simple and localized — find a specific function, look up an API signature, pull the definition of a class — similarity search can surface the right fragment. For these cases, it genuinely helps.


The problem is that code is not text. Code has structure.


A function in your payments service calls a function in your billing service, which triggers an event consumed by the notifications service. RAG retrieves the payments function because you mentioned payments. It doesn’t retrieve the billing call chain. The billing service never mentions the word “payment.” It references an event ID. The notifications service is three hops away. The agent sees one node of a graph and makes decisions without understanding the graph.


RAG may retrieve Function A and Function C but miss the critical intermediate dependencies B and D entirely. And it gets worse at scale. A codebase with n symbols doesn’t have n pieces of information — it has n² potential relationships. Chunking destroys that combinatorial structure.


You could try graph RAG to maintain some of the relationships between modules, but now you’re abusing what search technology is designed to do. You’re trying to make it fit the structure of the problem, and it’s better to just understand that this problem is structural.


Codebase context isn’t a needle-in-a-haystack problem. It’s a comprehension problem — you need exhaustive understanding of how the system works, served at the right level of detail for the task at hand. Search is the wrong paradigm entirely.


## The Local Minimum


Every incremental improvement to a search-based approach is real. Better chunking helps. Re-ranking helps. Metadata filtering helps. Each improvement produces measurable gains and reinforces the conviction that you’re on the right track.


In optimization terms, this is a local minimum. You’re at a real optimum within the search paradigm. There’s a deeper valley, but it requires abandoning retrieval entirely and treating the problem as structural analysis. Compilation, not search.


The activation energy to make that shift is enormous. You have to commit to a fundamentally different architecture with zero payoff during the transition. Every hour spent optimizing a RAG pipeline makes the local minimum more comfortable and the escape more costly.


## What Codebase Context Actually Requires


Think about what a compiler does. It doesn’t figure out your code structure at runtime. It parses every file, resolves every symbol, builds a complete graph of dependencies, and produces structured artifacts that the rest of the system can consume. The understanding is computed ahead of time, exhaustively and deterministically.


Codebase context requires the same approach. A system that analyzes the code ahead of time — building the syntax tree, tracing call chains, mapping dependencies across services — and produces structured context that agents can consume directly. A complete, navigable representation of how the codebase actually works.


This is what we’ve been building at Driver. Daniel calls it a transpiler. It parses code the way a compiler would, but instead of emitting executable code, it emits context. The output is symbol-complete: we build the syntax tree and abstract syntax tree of each language, so we can exhaustively detect every unique symbol in the codebase. Every function, every type, every dependency.


What comes out is thin but broad high-level information — the architecture, the structure, how the system is organized — alongside detailed navigation tools that let the agent dive into specifics where it needs to. Exhaustiveness at the top, surgical precision at the bottom.


And it stays current. When you push to remote, the context updates automatically. No merge conflicts. No stale documents.


## Why This Matters Now


We keep meeting teams at the same point in the journey. They’ve adopted AI coding tools. They’ve hit the context wall. They’re maintaining markdown files, building custom tooling, optimizing search pipelines.


The tools are capable enough. The models are smart enough. What’s missing is context infrastructure underneath them, the same way you don’t build your own version control or your own CI/CD pipeline. Context should be reliable, automatic, and available to every engineer on the team from the moment they start a session.


If you’re spending meaningful time curating codebase context for your AI tools, you’re solving the right problem. But it’s not something you should be building by hand.
