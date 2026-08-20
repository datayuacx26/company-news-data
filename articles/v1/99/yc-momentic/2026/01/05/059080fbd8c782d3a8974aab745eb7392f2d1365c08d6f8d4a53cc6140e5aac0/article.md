---
schema_version: "1.0.0"
document_id: "059080fbd8c782d3a8974aab745eb7392f2d1365c08d6f8d4a53cc6140e5aac0"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/the-slop-is-out-of-the-bottle"
published_at: "2026-01-19T00:00:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-08-19T18:58:31.052183+00:00"
content_hash: "sha256:c564f5a0c34f6510ad952fa4cd15c1f66f1d62f2ade9a9fedcbc1301a4f4d00b"
---

# The slop is out of the bottle

> “Of course, this code is complete AI slop.”


LaurieWired


And so it will be, forever. The slop is out of the bottle. And just like any stand-in-for-the-genie in this metaphor, we can’t put it back in.


Two options: (1) pure, unrelenting Darwinism, and (2) actually doing some work.


## Option 1: The environment as the teacher


There is something fun about letting the slop detectives tear people to shreds. Authentication silently dropped? Headers not passed? Flags that do nothing? You’re publicly torn apart, and millions watch like they are enjoying an evening out at Tyburn.


The consequences are immediate and public.


We can think about this as reinforcement learning in its purest form. Not the version where you train a model in a sandbox. The real version, where the environment provides rewards and punishments, and products learn or die. Ship code that works, survive. Ship slop, get selected out. The environment doesn't care about your intentions or your process or how many fuzz harnesses you claim to have.


Is this a good option? Yes, but mostly no.


Yes, because it is bringing the problem to the forefront. Now, you generate an entire feature and ship it while concentrating on Factorio. Generation got fast. Verification didn't. Thus, verification gets skipped, so you can debug the bottleneck that actually matters: your iron smelting array.


If you didn't verify it properly, you find out by Thursday. The loop has compressed from months to days.[The Cortex 2026 Benchmark Report](https://www.cortex.io/post/ai-is-making-engineering-faster-but-not-better-state-of-ai-benchmark-2026) found that change failure rates increased 30% as teams ship more AI-generated code. More outages, more rollbacks. The environment is grading harshly, and most teams will learn quickly because this teacher is impossible to ignore.


Pure Darwinism will teach these lessons. Expensively.


But mostly no. For it to come to the forefront, there has to be a problem. AI slop is starting to drag down OSS projects. Both[tldraw](https://github.com/tldraw/tldraw/issues/7695) and[Ghostty](https://news.ycombinator.com/item?id=46460319) have limited issue creation due to the number of poor, AI-generated contributions they have received. Slop doesn’t just describe the quality of code; it also represents the quantity. There is now too much bad code, and it is screwing everything up.


But this isn’t really a slop problem. In many ways, it isn’t even an AI problem. It is a human problem that needs to be solved with option two.


## Option 2: Actually doing some work


The curl replacement wasn't bad because AI wrote it. It was bad because someone shipped unverified code into a domain that required decades of maturity and thought that was fine. The AI is incidental. The failure is human judgment.


As[Simon Willison puts it](https://simonwillison.net/2025/Dec/18/code-proven-to-work/) , *"A computer can never be held accountable. That's your job as the human in the loop."* Claude isn't going to get fired for the curl replacement. The AI has no reputation to protect, no career at stake, no skin in the game. Someone decided to ship that code. That person made a verification decision, or failed to make one.


This is freeing once you accept it. You don't have to solve "how do we stop people from generating slop." That's not a solvable problem. The economics are too compelling. Code generation is cheap and fast, and people will use cheap and fast tools. The people building these tools understand this. Claude Code writes 80% of Claude Code. Claude Code writes 100% of Claude Cowork. And still: "[Every line of code should be reviewed by an engineer.](https://www.npr.org/2025/10/21/nx-s1-5506141/ai-code-software-productivity-claims) " Anthropic doesn't ship unverified AI output, even when the AI is building itself.


The question is: **how do you build systems that catch slop before it reaches users?**


Verification. Testing. Proof that code does what it claims to do. Grokking that[the test is the truth](https://momentic.ai/blog/the-test-is-the-truth) . This isn't a new idea. It's the oldest idea in software engineering, finally getting the attention it deserves because the consequences of ignoring it are now immediate and visible.


But it requires a shift. Verification can't be downstream. It can't be "QA will catch it" or "we'll add tests later." When code generation is instant, verification has to be instant too, and that’s as much an organizational shift as a technical one. It means treating specification as production work, and accepting that some features won’t ship because correctness was never defined. “We don’t know what right looks like yet” has to be an acceptable answer.


The test comes first. The behavior contract comes first. Code is generated to satisfy the contract, not the other way around.


This is where human judgment actually matters now. Not in writing the implementation. In defining what must be true. In specifying behavior precisely enough that correctness is verifiable. The differentiator isn't "can you write the code?" It's "Can you define what the code must do, clearly and completely?"


What does this look like?


- Behavior tests that define outcomes, not implementation details
- Contracts at system boundaries, not just unit coverage
- Explicit ownership of what AI generates vs. what humans verify
- Failure modes that are enumerable, not emergent
- Observability designed in from the start, not bolted on after an incident


Not every system can do all of this at once. Legacy code rarely has clear contract boundaries or well-defined enumerable failure modes. In practice, verification starts at the edges and moves inward.


Verification is the first discipline, not the only one. Testing won’t save you if the architecture is chaos or the organization rewards speed over correctness. But you can’t fix incentives or culture while continuing to ship unverified code. AI just makes the cost of that failure arrive faster.


The slop is out of the bottle, and we can’t put it back in. So let's work on decontamination.
