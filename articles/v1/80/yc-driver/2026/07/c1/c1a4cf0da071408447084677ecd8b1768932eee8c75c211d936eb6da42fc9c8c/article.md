---
schema_version: "1.0.0"
document_id: "c1a4cf0da071408447084677ecd8b1768932eee8c75c211d936eb6da42fc9c8c"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/we-spent-a-year-building-codebase-documentation-nobody-read/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:5b10b17fcb213c33732894e67ad36d7cea8c342670e56e85fa9e93b37264c1e7"
---

# We Spent a Year Building Codebase Documentation Nobody Read. Then AI Coding Agents Started Reading it.

# We Spent a Year Building Codebase Documentation Nobody Read. Then AI Coding Agents Started Reading it.


Documentation was the solution for collaboration between humans. Context is the solution for collaboration between agents.


Mar 6, 2026 — 12 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


*Part 1 of 5 · Next:[Claude and Cursor Are Smart, But Not Enough to Solve Your Codebase Context Problem](https://www.driver.ai/blog/claude-and-cursor-smart-not-enough-codebase-context)*


## The Pivot We Didn’t Plan


Daniel and I were trying to automate a spectroscopy machine in a lab in Austin. The only documentation that came with it was a dense PDF describing something like 250 APIs, all implemented over Telnet. Start byte, message size, stop byte. Every API call was unique. No common pattern to any of them.


We needed to wrap all of this in a REST API so our lab control software could talk to it. In a normal scenario, we would’ve spent hours reading the PDF, marking it up with a highlighter, trying to understand each call, then hand-implementing each wrapper and testing along the way. I’d scoped it at three months.


Instead, we copied each API’s documentation from the PDF into ChatGPT (which at the time only had a 4K context window), along with the MATLAB reference code. Asked it to explain how the API worked. Once we had the explanation, we used Copilot to implement the Python wrapper. Repeated that for each of the 250 calls.


The whole project took about a week.


## Connecting the Dots


Daniel and I had both been at Nike. My job was being the glue between Nike’s innovation teams and global technology, about 7,000 engineers around the world, 30 years of software history.


Every project we ever ran at Nike was an archaeology project. You’d spend nine months just trying to understand: what systems do we have? What capabilities do those systems implement? How are those capabilities implemented? What’s the delta between where things are today and where we need them to be? And then: what are the engineering plans to close that delta?


We were able to connect the dots between this small experiment we’d just run in a narrow scope and the scale of large enterprises. If comprehensive documentation was the input that made an LLM effective on 250 Telnet APIs, what would happen if you could produce that kind of comprehensive context for an entire enterprise technology landscape — tens to hundreds of millions of lines of code?


That’s what motivated us to start Driver.


## What We Built


An LLM call is, at the end of the day, a transfer between a set of inputs and an output. The quality of the output is bounded by the quality of the input. Two things matter on the input side: making sure you’ve exhaustively included all the relevant information, and making sure you’re not including a bunch of information that’s irrelevant to the task, i.e., high relative signal-to-noise ratio.


We built a system that processes source code and produces structured context optimized for consumption by AI models. Daniel calls it a transpiler — it parses code the way any compiler would, but instead of emitting executable code, it emits context. The output is symbol-complete: every symbol is fully resolvable, every relationship is mapped, every dependency is traced.


What comes out isn’t a code listing. It’s what a senior engineer would write in their notes before making changes to a large system — except generated automatically, exhaustively, from the actual code. And it stays in sync with every commit.


We pointed it at enterprise codebases and called it documentation.


That turned out to be wrong.


## Why Engineers Wouldn’t Read It


The output was accurate. Exhaustive. Nobody questioned the accuracy. But engineers wouldn’t read it.


Looking at documentation takes you out of your workflow. If you’re inside an editor trying to understand what the code does, navigating away to read documentation requires energy. Engineers would rather struggle through the source code than context-switch to a document.


And documentation has a deeper problem: to write it perfectly, you need to understand who your audience is going to be and the questions they’re going to ask. But you don’t have that information at the time of writing. New questions come up, new use cases emerge. People try to do things with the code you didn’t expect. You can never really be done writing documentation.


Then there was the qualitative feedback. Engineers would read our output and come back with: “It’s not technically wrong, but that’s not how I would describe what it does.” What they meant was that our descriptions, while accurate, didn’t match their mental model. And the polar bounds on that were wide: somebody who’d never seen the codebase before was relieved to have something in plain English that told them what the software does. Somebody intimately familiar with the code was never really satisfied, because they had their own model and ours wasn’t going to match it perfectly.


There are a lot of different local minima for optimizing documentation. There is no perfect set of documentation. We tried. We aligned everything to the Simplified Technical English standards, which made the output terse, direct, and focused on technical facts rather than qualitative judgments. It helped. But the fundamental tension remained: exhaustive documentation includes things that some users consider irrelevant, and those users question why it’s there. Then on a different screen, different task, they’re glad it’s there. Balancing this in an automated system is genuinely hard.


We opted to maintain the exhaustiveness and let users self-select into what they wanted to read. This was the right call, but we didn’t know why yet.


## What We Discovered


What happened next is that customers started checking our output into their repos.


Optiver was the first to do this at scale. Matt Nassr, their Head of Global Data and AI Engineering, had actually spent months building a RAG-based solution to the same problem — manually exporting context to markdown, checking it into repositories, pointing agents at those directories. It hit a hard limit because RAG was never the right architecture for code. Chunking source code for vector retrieval destroyed the dependency relationships and call graphs that define how their systems actually work.


Then something we didn’t expect. The agents that were searching those repos for context found our output and started using it. Agentic performance went up, dramatically, when agents had well-structured, exhaustive documentation to work from.


What we discovered was that it’s helpful to have a compact description of a large amount of software, because the LLM can look at it and learn a lot about the system holistically. This allows the LLM to decide what parts of the software to look at in more detail, and not miss parts it should be looking at. The exhaustiveness that humans rejected was exactly what agents needed.


When we realized not just that customers were checking our output into their repos, but why they were doing it, to make their agents more effective, we knew we were onto something specific.


We built an MCP integration. Daniel and the engineering team shipped it, and it changed everything. The same system that produced documents nobody read was now serving real-time context that agents used continuously, and to great effect. But what we hadn’t realized was that the primary use case wouldn’t be end users reading about the software. It would be developers working with agents to modify the codebase.


Matt at Optiver was the first in-market evidence. Now we see it everywhere. Customers come to us saying: “Agentic performance has gone up when we’ve done a really good job documenting our systems, so we’re looking to solve documentation at scale.” But what they really need isn’t documentation. It’s context for their agents.


## The Simpler Idea


Documentation used to solve for collaboration between humans. Context now solves for collaboration between agents.


That’s really what this comes down to. You don’t need documentation, you just need context.


What Driver provides is reliable, scalable context for agents that you can turn on in an instant. Two weeks from pilot to full-scale deployment. Every engineer flips a switch to integrate with Driver and instantly has access to the context their agents need.


If you’ve been working with Claude Code and you’re hitting the problems that emerge at the limits — in large-scale systems where the agent can’t hold enough context, where it makes errors of omission because it hasn’t seen the whole codebase — you don’t have to struggle with that anymore. It’s easy to look at something that’s wrong and know it’s wrong. Hard to look at something that’s incomplete and know it is incomplete. Driver solves the incomplete problem.
