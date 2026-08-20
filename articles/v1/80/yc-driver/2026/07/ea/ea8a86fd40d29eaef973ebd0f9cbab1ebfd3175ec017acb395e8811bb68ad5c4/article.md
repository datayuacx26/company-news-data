---
schema_version: "1.0.0"
document_id: "ea8a86fd40d29eaef973ebd0f9cbab1ebfd3175ec017acb395e8811bb68ad5c4"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/intelligence-efficacy-why-we-built-compiler-not-search-engine/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:c7177056adc72969e01bcd16c5e3bcbe7e53fbc6fe84ea6b9f4d3d4208027792"
---

# Intelligence ≠ Efficacy: Why We Built a Compiler, Not a Search Engine

# Intelligence ≠ Efficacy: Why We Built a Compiler, Not a Search Engine


The Data Processing Inequality proves that an AI agent's output is bounded by its input. We built a compiler architecture because information theory told us search would never be enough.


Feb 26, 2026 — 12 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


Daniel and I both come from signal processing and information theory backgrounds. We’ve spent a lot of time thinking about systems, transfer functions, signal-to-noise ratios, filter chains. When we started building Driver, we had an intuition about the architecture that turned out to be load-bearing. This post is about where that intuition came from and why it led us to build a compiler instead of a search engine.


## The Bound


The Data Processing Inequality is a theorem in information theory. For any Markov chain X → Y → Z, the mutual information satisfies I(X; Z) ≤ I(X; Y). In plain language: if Z depends on X only through Y, then processing Y to produce Z can only preserve or lose information about X. It can never create information that wasn’t there.


This isn’t a heuristic. It isn’t a best practice. It’s a mathematical proof that follows from the definition of mutual information. It’s as fundamental to information processing as conservation of energy is to physics.


Applied to LLMs, the mapping is straightforward. X is the ground truth about your codebase: what the software does, how it’s structured, why decisions were made. Y is the context you provide to the model: the prompt, the files, the retrieved fragments. Z is the model’s output.


I(X; Z) ≤ I(X; Y). The output cannot contain more information about your codebase than the input does. A smarter model gets I(X; Z) closer to I(X; Y). It does not raise I(X; Y). The ceiling is set by the input, not the model.


When I say it seems like it breaks information theory to expect a model to take an entire codebase and in one inference call figure it all out, I mean that literally. The math says you can’t do it.


## Where the Bound Bites


For proprietary codebases, the Markov chain assumption holds cleanly. The LLM has zero pre-training knowledge of your specific system. The only information channel about your codebase is what you put in the prompt. X → Y → Z is an accurate model, and the DPI applies directly.


If the billing service’s dependency on the payments service isn’t in the context, the model cannot know about it. Any correct guess would be coincidental pattern-matching from training data, not knowledge of the actual system. The model doesn’t know what it doesn’t know, and it can’t flag what’s missing because the missing information literally isn’t there. Errors of omission are the hardest to detect and the most dangerous at scale.


There’s an interesting corollary here. A more capable model with incomplete input doesn’t make fewer mistakes. It makes mistakes that are harder to catch. Higher I(X; Z) relative to a flawed I(X; Y) means the model is more efficiently extracting information from an incomplete picture. The output sounds more confident, more professional, more plausible. But the gaps are the same gaps. It’s like arguing with an expert who happens to be wrong. They can cover up the holes in their reasoning with sophisticated-sounding jargon that is hard to challenge if you’re not already an expert.


This is the intelligence-versus-efficacy distinction that we keep coming back to. Intelligence without availability of truth has a low ceiling. The model is smart. It just doesn’t have the information.


## The Two Goals That Fight Each Other


Shannon said it well: “Almost every problem that you come across is befuddled with all kinds of extraneous data of one sort or another; and if you can bring this problem down into the main issues, you can see more clearly what you’re trying to do.”


But bringing the problem down to the main issues isn’t enough. The main issues need to exhaustively cover what matters. Einstein’s version: make it as simple as possible, but no simpler.


So you need two properties simultaneously. Exhaustiveness: covering everything relevant to the task. And high signal-to-noise ratio: containing as little irrelevant information as possible.


These two goals fight each other.


The easy way to be exhaustive is to give the model everything. Massive context windows, dump in the whole codebase. You’ll definitely include the relevant information. You’ll also include an enormous amount of noise, and the model has to simultaneously pick the signal from the noise and ascertain the relationships within the signal. That’s a deconvolution problem, and it degrades performance fast.


The easy way to have high signal-to-noise is to focus on something very small. One file. One function. You’ll have clean, relevant context. But you’ll miss the cross-service dependencies, the architectural conventions, the historical decisions that govern how the system actually works.


Doing both together is the optimization challenge. And this is where the architectural decision becomes load-bearing.


## Why Search Is the Wrong Architecture


The natural approach is retrieval. Chunk the codebase, embed it, search for relevant fragments at query time. RAG.


RAG does some things well. For simple, localized questions it can surface the right fragment. But code has structure that text doesn’t. A function call isn’t a textual reference. It’s a binding. A type hierarchy isn’t a keyword match. It’s a structural relationship. A codebase with n symbols doesn’t have n pieces of information. It has n² potential relationships. Chunking destroys that combinatorial structure.


You could try graph RAG to preserve some relationships. But now you’re forcing a search paradigm onto a structural problem. You’re asking a system designed for “find me something similar” to answer “show me everything connected.” It’s the wrong tool.


The deeper issue: search operates at query time. Every time the agent needs context, it searches. Every search is probabilistic. Sometimes it finds the right fragments. Sometimes it doesn’t. The quality of context varies from query to query, and you can’t guarantee exhaustiveness. I(X; Y) is a random variable, and you’ve introduced variance into the ceiling itself.


## Why a Compiler Is the Right Architecture


From our signal processing backgrounds, Daniel and I knew that you can’t build an optimal system as one monolithic filter. A chain of specialized filters, each tuned for a specific operation, outperforms any single-stage system. This is well-established in signal processing. It’s why complex imaging systems like MRI machines have mandatory analog front-end conditioning layers. You cannot build an MRI machine without this layer. It doesn’t matter how good your digital processing is. Without the front-end, the signal is too noisy to be useful. This is a physical constraint.


The same principle applies here. What you want is a multi-pass processing chain where each stage increases the signal-to-noise ratio for the next stage. Raw codebase → language-specific parsing → symbol resolution → relationship mapping → structured context artifacts. Each stage is deterministic. Each stage operates on the output of the previous stage. And the DPI works in your favor at every step, because each stage is designed to preserve the information that matters and discard the noise.


This is the compiler architecture. Parse every file. Build the complete syntax tree. Resolve every symbol. Trace the call chains. Map the dependencies across services. Produce structured output that captures the relationships, the architecture, the intent. Do all of this ahead of time, once, and keep it in sync.


The output is symbol-complete: because we build the full syntax tree and abstract syntax tree of each language, we can exhaustively detect every unique symbol in the codebase. The context is deterministic. Given the same codebase state, it produces the same context. I(X; Y) is maximized for the representation, and there’s no query-time variance.


## The Iterative Exploration Objection


The most substantive counterargument is tool use. When an agent uses tools, it’s not a simple X → Y → Z chain. It’s iterative:


X → Y₁ → Z₁ → Y₂ → Z₂ → … → Yₙ → Zₙ


Each tool call acquires new information. The DPI still applies at each step, but the agent can progressively build up I(X; Yₙ) through exploration. In theory, enough iteration could approach the full information content.


This is correct. And some teams do exactly this, letting the agent grep for references, read files, trace call graphs over dozens of tool calls.


The problem isn’t that it can’t work. The problem is economics. You’re burning tens of minutes of compute and context window on an exploration that produces the same result every time for the same codebase. Tomorrow, a different engineer does the same exploration. Next week, you do it again after a few commits have landed. You’re re-deriving context that could have been computed once, kept up-to-date, and reused.


There’s also a subtler problem. The exploration itself introduces noise. Every search result, every file read, every dead end consumes the context window. By the time the agent has built up enough context, it’s also filled the window with irrelevant information from the exploration process. You end up having to emit the context to an artifact, clear the window, and reload, just to get back to a clean starting point.


Pre-computation changes the economics entirely. Compute the context once, ahead of time, through a structured pipeline that makes millions of targeted model calls to decompose the problem. Then every engineer on the team starts every session from a position of excellent context. No exploration tax. No context window pollution. No re-deriving what was already known.


## The Equation


Here’s the simplest way I’ve found to express all of this:


**Availability of truth + Intelligence = Efficacy.**


In information theory terms: availability of truth is I(X; Y), the mutual information between the ground truth and the input. Intelligence is how efficiently the model converts I(X; Y) into I(X; Z). Efficacy is the actual output quality.


Intelligence without availability of truth has a low ceiling. You have a brilliant model with nothing to work from. Availability of truth without intelligence wastes potential. You have all the information but nobody’s doing anything useful with it. You need both. But the ceiling is set by one side only.


We started with codebases because source code has attributes that make this a soluble problem. It’s discrete. It’s knowable. It’s written in structured syntax. And it’s what the software actually does. The code is executed. If you’re describing what systems do based on what the code says they do, you can rely on that being accurate. It becomes less a challenge of judgment and more a question of scale: can you accurately describe what software does, exhaustively, with high signal-to-noise ratio?


That’s the problem we’ve been solving. Not with search. With compilation.
