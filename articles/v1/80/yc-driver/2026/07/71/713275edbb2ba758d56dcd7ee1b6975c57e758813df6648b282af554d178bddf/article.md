---
schema_version: "1.0.0"
document_id: "713275edbb2ba758d56dcd7ee1b6975c57e758813df6648b282af554d178bddf"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/claude-and-cursor-smart-not-enough-codebase-context/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:38c1502e0ba31067859d1499b6ac51b65d0688d9ad166c180e56c942df628fa7"
---

# Claude and Cursor Are Smart, But Not Enough to Solve Your Codebase Context Problem

# Claude and Cursor Are Smart, But Not Enough to Solve Your Codebase Context Problem


Information theory proves that model intelligence has a ceiling — and it's set by the context you provide, not the model you choose.


Mar 13, 2026 — 15 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


*Part 2 of 5 · Previous:[We Spent a Year Building Codebase Documentation Nobody Read](https://www.driver.ai/blog/we-spent-a-year-building-codebase-documentation-nobody-read) · Next:[Why are You Still Putting Codebase Context in Markdown Files?](https://www.driver.ai/blog/why-codebase-context-in-markdown-files)*


Claude Shannon


We keep meeting teams at the same point in the same story arc. They’ve hit the wall with AI coding agent failures — not because the agents are dumb, but because they lack codebase context.


They’ve adopted Claude Code or Cursor or one of the other agentic tools. The first few weeks are electric. Small tasks, clean results, obvious productivity gains. Then they try something big — an architectural change across millions of lines of code — and they hit the wall.


The agent doesn’t know which patterns to follow. It doesn’t know why the system was designed this way. It doesn’t know that the authentication module was refactored last quarter or that the payment service has a different error-handling convention than the rest of the codebase. So the engineer has to stop and tell it. They spend tens of minutes, sometimes hours, just building up context inside the agent’s context window before they can start the actual work.


And here’s the thing that surprises people: upgrading the model doesn’t fix this. We’ve watched it happen. Teams move from Sonnet to Opus, from GPT-4 to whatever’s next. The agent gets more articulate. The code it produces looks more professional. But the fundamental problem doesn’t change — the agent still doesn’t know what it doesn’t know.


I’ve started describing this as the difference between intelligence and efficacy.


**Codebase context for AI agents** is the structured, pre-computed understanding of a software system — its architecture, conventions, dependencies, and design decisions — delivered to an AI coding agent so it can operate effectively across large codebases without re-deriving that understanding from scratch on every task. Without it, even the most capable models hallucinate functions, miss cross-service dependencies, and produce code that ignores the patterns your team actually follows.


## A Law of Nature


Daniel and I both come from signal processing and information theory backgrounds. We’ve spent a lot of time thinking about systems — transfer functions, signal-to-noise ratios, filter chains. When we started building Driver, we had an intuition about why model upgrades would not solve the context problem.


The Data Processing Inequality is a theorem in information theory — not a heuristic, not a best practice, a mathematical proof. It says: for any processing chain, information about the input can only be preserved or lost, never created. No matter how sophisticated the processor is, it cannot recover information that wasn’t in the input.


Applied to LLMs, this means something specific and important. The ceiling on output quality is set by the input, not the model. A smarter model gets closer to that ceiling. It doesn’t raise it.


This isn’t a claim about current limitations that future models will overcome. This is information theory. It’s a law of nature, the same way the speed of light is a law of nature. LLMs are governed by it regardless of weight count, architecture, or how many billions you spend on training.


When I say it seems like it breaks information theory to expect a model to take an entire codebase — with all the necessary information buried inside a mountain of irrelevant information — and in one inference call figure it all out, I mean that literally. It breaks information theory. The math says you can’t do it.


## The Hard Problem


Shannon’s insight points to the challenge, but the real difficulty is more specific than just “get better input.”


Einstein said it well: make it as simple as possible, but no simpler. The context you give a model needs two properties simultaneously. It needs to be exhaustive — covering everything relevant to the task. And it needs high signal-to-noise ratio — containing as little irrelevant information as possible.


These two goals fight each other.


The easy way to be exhaustive is to give the model everything. Massive context windows, dump in the whole codebase. You’ll definitely include the relevant information. You’ll also include an enormous amount of noise, and the model has to simultaneously pick the signal from the noise and ascertain the relationships within the signal. That’s a deconvolution problem, and it degrades performance fast.


The easy way to have high signal-to-noise ratio is to focus on something very small. One file. One function. You’ll have clean, relevant context. But you’ll miss the cross-service dependencies, the architectural conventions, the historical decisions that govern how the system actually works. Errors of omission.


Doing both together — that’s the optimization challenge.


Now, you could theoretically achieve both at inference time. Let the agent run an iterative loop — read files, grep for references, trace call graphs, build up a picture of the codebase over dozens of tool calls. Some teams do exactly this. And it can work, for a single session. But think about what you’re doing: you’re burning tens of minutes of compute and context window on an exploration that produces the same result every time for the same codebase. Tomorrow, a different engineer on the same team does the same exploration. Next week, you do it again after a few commits have landed. You’re re-deriving context that could have been computed once, kept up-to-date, and reused.


This is where pre-computation changes the economics entirely. If you can build up that exhaustive, high-SNR context ahead of time — through a structured pipeline that makes millions of targeted model calls to decompose the problem into its essence — then every engineer on the team starts every session from a position of excellent context. No exploration tax. No context window pollution. No re-deriving what was already known.


That’s what we’ve been solving at Driver.


## How This Actually Fails


The failure modes we hear about from customers are mundane and insidious.


The first is what I’d call context window pollution. An engineer is trying to give their agent the context it needs, so they do a bunch of exploratory work — “look at this file, now look at that service, now check how this API works.” By the time they’ve built up enough context, they’ve also filled the context window with a bunch of irrelevant information from the exploration. The process of building up context introduces noise. They end up having to emit the context to an artifact, clear the window, and reload — just to get back to a clean starting point. There’s no way around this if you’re assembling context at inference time.


The second is errors of omission, and this one connects directly to the Data Processing Inequality. If something important isn’t in the context, the model can’t flag that it’s missing — because the missing information literally isn’t there. The model explains something to you, you read it and think “that sounds right.” But if a critical dependency or a cross-service interaction wasn’t included, you’re operating on incomplete context and you don’t even know it. It won’t be apparent until you’re several steps down the road and things have already gone off the rails.


We’ve heard specific examples from customers. They’re trying to resolve a bug and, by the nature of it being a bug, they don’t know why it’s happening. So they’re relying on their agentic tools to go figure it out. But it’s actually a multi-codebase problem — they don’t even know which codebase to look in. They’ll burn days, sometimes weeks. The model isn’t dumb. The model doesn’t have the information.


## ”Just Wait for Better Models”


This is the pushback we hear most often. “GPT-12 will fix this.” “The next Claude will be smart enough to figure it out.”


We’ve been building Driver since GPT-3.5. The models have progressed enormously since then. And our solution just keeps getting more valuable — not less. As models get better, their desire and hunger for context goes up. What they can do with accurate, robust context goes up too. We’re seeing the opposite of what the “just wait” crowd predicts.


Think about it this way. You could hire the smartest person in the world to consult for your business. But if they don’t know anything about how your business works — the product, the market, the codebase, the architectural decisions, the design patterns — how are they going to be effective? In any large company, it’s always the same 10 people who can actually get things done, no matter how big the org gets. Not because they’re the smartest. Because they have the context. They’ve built up that mental map over years.


And you can’t bake this into the model through fine-tuning. Software doesn’t sit still. It’s not a single point moving through time — it’s a parallel system. Developers are working on feature branches that are eventually getting merged into the main branch. The actual dynamic process by which code evolves is extremely complex. Are you going to fine-tune and retrain a model after every commit on every feature branch? It’s not even a practical thing to attempt.


More intelligence without context just produces more articulate wrong answers. It’s like arguing with an expert who happens to be wrong — they can cover up the gaps in their reasoning with sophisticated-sounding jargon that’s hard to weed through. A smarter model with bad context doesn’t make fewer mistakes. It makes mistakes that are harder to catch.


## The Equation: Why Context Infrastructure Matters More Than Model Intelligence


Here’s the simplest way I’ve found to express this:


**Availability of truth + Intelligence = Efficacy.**


In information theory terms: the availability of truth sets the ceiling. Intelligence determines how close you get to it. Efficacy is the outcome.


Intelligence without availability of truth has a low ceiling — no matter how capable the model. Availability of truth without intelligence wastes potential — you have the information but no one’s doing anything useful with it. You need both. But the ceiling is set by one side only.


What do I mean by “availability of truth”? We started with codebases because source code has attributes that make this a soluble problem. It’s discrete. It’s knowable. It’s written in structured syntax. And it’s what the software actually does — the code is executed. If you’re describing what systems do based on what the code says they do, you can rely on that being accurate. It becomes less of a challenge of judgment and more a question of scale: can you accurately describe what software does, exhaustively, with high signal-to-noise ratio?


That’s what Driver does. We’ve built a system that provides thin but broad high-level information — an architecture overview, a codebase map you can navigate — alongside detailed tools that let the LLM dig into specifics. Exhaustiveness at the top, surgical precision at the bottom. Both properties, simultaneously, without forcing the engineer to assemble it by hand.


## Why This Matters: The Context Bottleneck Is the Real Bottleneck


I’ve been thinking about whether we’ve crossed over from a model-bottleneck era to a context-bottleneck era, and honestly, I think the context bottleneck has always been there. The whole prompt engineering movement was about this — the prompt is the context for one LLM call, so of course spending time getting it right produces better outcomes.


What’s different now is scale. We’re in the age of scalable context. Organizations want agents that operate at the scale of their business. But as they try to scale up, they discover that these agentic systems don’t have what they need to contain the power of the model. The model can make an enormous amount of changes to your software faster than ever. But you still have to maintain the context. What is this product for? Why are we building it? What are the design patterns we use? What are the critical parts of the software that need more rigorous testing if changed?


You can’t rewrite that every time you sit down to work. You want to capture it once, keep it reliable and up to date, and make it available to everybody on the team. You want your organization to have a shared, accurate point of view on what you’re building and how it works.


What we’ve seen from customers who’ve adopted Driver at scale is a binary outcome. They don’t come back and say it made things 20% better. They come back and say things that weren’t working before are now working — reliably, and at scale.


The models are going to keep getting smarter. That’s a good thing. But the Data Processing Inequality isn’t going anywhere either. The ceiling is still set by the input. If you want the next model upgrade to actually deliver — if you want to stop coding agent hallucinations and AI coding agent failures at their source — invest in codebase context. It’s the input side of the equation, and it’s the only side that raises the ceiling.
