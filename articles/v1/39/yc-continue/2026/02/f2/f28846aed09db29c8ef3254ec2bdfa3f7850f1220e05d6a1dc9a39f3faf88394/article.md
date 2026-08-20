---
schema_version: "1.0.0"
document_id: "f28846aed09db29c8ef3254ec2bdfa3f7850f1220e05d6a1dc9a39f3faf88394"
company_key: "yc-continue"
company: "Continue"
source_id: "yc-continue-rss-8ad3fe8c275c"
canonical_url: "https://blog.continue.dev/chiseling-the-art-of-polishing-vibe-code"
published_at: "2026-02-16T08:00:00+00:00"
first_seen_at: "2026-07-24T23:23:23.996128+00:00"
fetched_at: "2026-07-28T20:54:21.606053+00:00"
content_hash: "sha256:693f1f8a3469d089ef62a4c4a1fded265536c0871bc2ec0f0bb2448a1bad6417"
---

# Chiseling: The Art of Polishing Vibe Code

I recently read Mitchell Hashimoto's post on[Vibing a Non-Trivial Ghostty Feature](https://mitchellh.com/writing/non-trivial-vibing) , where he describes his "anti-slop sessions":


> Throughout these \[prompting sessions\], I am usually dropping in and making minor manual changes as well.
>
>
> The cleanup step is really important. To cleanup effectively you have to have a pretty good understanding of the code, so this forces me to not blindly accept AI-written code. Subsequently, better organized and documented code helps future agentic sessions perform better.
>
>
> I sometimes tongue-in-cheek refer to this as the "anti-slop session".


In the debate around vibe coding, the cleanup step seems to be left out of the conversation. Everyone can agree on the principle of "make it work, make it right, make it fast", and for experienced developers like Mitchell it has become clear that LLMs can rapidly accelerate the "make it work" phase.


But for some reason everyone believes that vibe coding ends there. Instead, at[Continue](https://www.continue.dev/) we like to think of the vibe coding process as more akin to sculpture work.


### The Jackhammer


The CLI is your jackhammer, breaking off huge chunks of possibility, letting you verify ideas and explore architectural patterns without getting bogged down in details. As Karpathy outlined in[the original vibe coding post](https://x.com/karpathy/status/1886192184808149383) , at this stage you "forget that the code even exists". For many engineers this is uncomfortable, but it's the quickest path to "making it work".


But working code isn't finished code. The kernel of what you actually wanted is buried underneath a mountain of AI-generated slop. Duplicated functionality, 1000 line methods, useless unit tests. Like a sculptor facing a rough-hewn marble block, you need to chisel away the excess to reveal the clean, logical core beneath.


### The Chisel


This is when you refine and polish, using whatever tools work best for your workflow. You refactor that sprawling function into three focused ones. You strip the over-engineered abstraction that handles one use case. You rename processDataAndTransformResultsIntoFinalFormat() to simply parse().


Chiseling isn't just cleanup, it's comprehension. Each line you refine forces you to understand what the code actually does, not just that it works. This is where "vibe coding" transforms into "vibe engineering." You're no longer just directing an AI, you're taking ownership of every decision, every abstraction, every line that ships.


The temptation is to stop at "working" - after all, the tests pass. But without chiseling, you're not building software, you're accumulating technical debt at a breakneck speed of thousands of tokens per hour. Vibe coding gives you velocity, but chiseling gives you quality. It's the critical human judgment that separates a prototype from production code.


### Chiseling at Scale


Here's the challenge: as teams adopt AI-assisted development, chiseling becomes a bottleneck. You can't rely on every developer to chisel perfectly every time. Standards that exist in documentation or in senior engineers' heads don't scale when the team is shipping at AI-accelerated velocity.


This is where organizations need a third phase: **the inspection** — automated checks that ensure what ships meets your standards before it reaches production. Cloud agents running in CI/CD can enforce the chiseling standards you care about: naming conventions, abstraction patterns, test requirements, security patterns, documentation quality.


Think of it as encoding your best chiseling practices into agents that review every PR. The standards you'd catch in code review get caught systematically. Organizations can define once what "well-chiseled" looks like — clear naming, appropriate abstractions, meaningful tests — and have those standards enforced automatically at the PR level.


This isn't about replacing human judgment. It's about scaling it. Your senior engineers focus on architecture and design decisions, while automated checks handle the systematic verification that code has been properly chiseled before it merges.


The CLI makes it work. Your chiseling makes it right. Mission Control makes it consistent.
