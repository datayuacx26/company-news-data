---
schema_version: "1.0.0"
document_id: "628cc27bd7fa61f95102d8293e65207c1785507cf94167ff09727191d897710b"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/claude-fable-5-what-it-is-what-it-means-for-developers"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:0fde0aa94e312e884ed32037a99ed4f3ddab6394bef39c713ed91728159beebd"
---

# Claude Fable 5: Benchmarks, Pricing, and What Developers Need to Know (2026)

Anthropic launched Claude Fable 5 June 9, 2026. It is the most capable model Anthropic has ever made generally available, and it lands at a price point below half of what Claude Mythos Preview costs. If you build on Claude, or if you are evaluating which frontier model to put at the center of your stack, this release changes the calculus.


Here is everything you need to know, sourced directly from[Anthropic's announcement](https://www.anthropic.com/news/claude-fable-5-mythos-5) .


Fable 5 is the model now powering Claude Code's strongest agentic results. If you are comparing AI coding tools, see[Claude Code vs GitHub Copilot vs Cursor](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026) and our newer[Claude Code vs Codex vs Cursor breakdown](https://www.cosmicjs.com/blog/claude-code-vs-codex-vs-cursor) for practical guidance on where Fable 5 fits in your workflow.


---


## What Is Claude Fable 5?


Fable 5 is a Mythos-class model that Anthropic has made safe for general use. The naming tells you the positioning: Fable (from the Latin *fabula* , "that which is told") and Mythos share the same underlying model. The difference between them is a set of safety classifiers that govern which kinds of requests Fable 5 can fulfill. More on that below.


Mythos-class models sit above Opus in Anthropic's capability hierarchy. Claude Fable 5 is therefore more capable than any Opus model, including Opus 4.8, which until today was the top of the public stack.


> **TL;DR on model hierarchy (updated June 2026)**
> Fable 5 / Mythos 5 > Opus 4.8 > Sonnet 4.6 > Haiku


---


## Benchmarks: What the Numbers Say


Anthropic benchmarked Fable 5 and Mythos 5 against leading models across software engineering, knowledge work, vision, and science. Fable 5 leads or matches the state of the art on nearly every tested dimension:


- **SWE-bench and FrontierCode (software engineering):** Fable 5 scores highest among frontier models on Cognition's FrontierCode evaluation, which tests whether models can pass difficult coding tasks while meeting the standards of high-quality production codebases, even at medium effort.
- **Finance reasoning (Hebbia Finance Benchmark):** Highest score of any model, with major gains in document-based reasoning, chart and table interpretation, and problem solving.
- **Vision:** New state of the art. Fable 5 can extract precise numbers from scientific figures and rebuild a web app's source code from screenshots alone.
- **Computer use:** Fable 5 completed Pokémon FireRed with a minimal, vision-only harness. Earlier Claude models required a complex helper harness to get close to this result.
- **Long-context / memory:** In Slay the Spire testing, giving Fable 5 access to persistent file-based memory improved its performance three times more than the same upgrade did for Opus 4.8.


The longer and more complex the task, the larger Fable 5's lead over previous models. For agentic workloads specifically, Fable 5 represents a qualitative shift.


---


## The Stripe Story: 50 Million Lines in a Day


The benchmark number that has gotten the most attention is this one: during early testing, Stripe reported that Fable 5 **compressed months of engineering into days** .


Specifically: in a 50-million-line Ruby codebase, the model performed a codebase-wide migration in a single day that would otherwise have taken a whole team over two months to complete by hand.


This is from Stripe's own internal testing, cited directly in[Anthropic's announcement](https://www.anthropic.com/news/claude-fable-5-mythos-5) . It is also representative of the broader category of capability Fable 5 is designed for: long-horizon, autonomous engineering work at a scale that was previously only possible with large teams over extended timelines.


Other early-access customers reported:


- Fable 5 opened "a class of long-horizon problems that were out of reach for earlier models" (Cursor)
- "Apps that took a hundred prompts a year ago, it now one-shots" (an early Anthropic partner)
- Fable 5 broke 90% on one team's core analytics benchmark of complex, long-running analytical tasks, a 10-point jump over Opus
- On frontier physics research, Fable 5 got to results in 36 hours that GPT-5.5 reached after four days, using a third of the reasoning tokens


---


## Vision: Rebuild Apps from Screenshots


Fable 5 is the new state-of-the-art model for vision tasks. The headline capability: it can rebuild a web application's source code from screenshots alone. No access to the original codebase, no DOM inspection. Just visual input.


This opens a category of tasks that were previously out of reach for AI assistants:


- Generating accurate frontend implementations from design mockups
- Auditing UI changes without requiring code access
- Extracting structured data from charts, tables, and scientific figures
- Debugging visual regressions from screenshots


For teams building with Computer Use agents, the vision upgrade is significant. Fable 5 needs less scaffolding to navigate real interfaces and can handle vision-based tasks that required purpose-built tooling with earlier models.


---


Want to build AI-powered content workflows? Cosmic gives your agents a structured, versioned content store with a REST API, TypeScript SDK, and built-in analytics. See what your agents produce and whether it worked.[Start for free, no credit card required.](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=in-article-signup-cta)


---


## Long-Horizon Autonomy: What Changes


Fable 5 stays focused across millions of tokens in long-running tasks and improves its outputs using its own notes. The persistent memory advantage over Opus 4.8 showed up clearly in Anthropic's Slay the Spire testing: Fable 5 reached the game's final act three times more often than Opus 4.8 when given access to persistent memory.


The practical implications for development teams:


- Complex multi-file refactors can be handed off with less supervision
- Long-running analytical tasks maintain coherence further into execution
- Agentic pipelines that previously required frequent human check-ins can run more autonomously
- The model self-reviews its work before returning results, which is what makes highly autonomous operations practical


One early-access partner summarized it: "At the highest effort, Claude Fable 5 reflects on and validates its own work. For us, that's what makes highly autonomous operations possible, the extra thinking pays for itself."


For developers choosing which AI coding tool to run Fable 5 through, our[Claude Code vs GitHub Copilot vs Cursor comparison](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026) and[Claude Code vs Codex vs Cursor](https://www.cosmicjs.com/blog/claude-code-vs-codex-vs-cursor) cover the tooling options in detail.


---


## Pricing: Less Than Half of Mythos Preview


Fable 5 and Mythos 5 are priced at:


- **$10 per million input tokens**
- **$50 per million output tokens**


This is less than half the price of Claude Mythos Preview. For teams that have been running Mythos Preview workloads, the cost reduction is significant. For teams evaluating whether Mythos-class capability is worth the price, the barrier just dropped substantially.


For comparison, Opus 4.8 is priced at $5 input / $25 output per million tokens. Fable 5 costs more per token than Opus, which reflects the step up in capability. Whether that premium makes sense depends on your workload. For tasks where Fable 5's long-horizon capability avoids multiple Opus iterations, the math often works out in Fable 5's favor.


---


## Availability: Free Window Through June 22


Anthropic is rolling out access in stages due to expected high demand:


- **API and consumption-based Enterprise plans:** Fully available from today.
- **Pro, Max, Team, and seat-based Enterprise plans:** Fable 5 is included at no extra cost through **June 22** . On June 23, using it will require usage credits. Anthropic intends to restore it as a standard part of subscription plans once capacity allows.


If you are on a subscription plan, you have a limited free window to run real workloads on Fable 5 and evaluate whether it belongs in your stack. Use it.


Access via the Claude API uses the model identifier .


---


## Claude Mythos 5: The Restricted Counterpart


Mythos 5 is the same underlying model as Fable 5, but with safety classifiers lifted in specific areas. Today it is restricted to:


- Partners in Project Glasswing (cybersecurity, critical infrastructure)
- A forthcoming trusted access program for biology researchers


Mythos 5 has the strongest cybersecurity capabilities of any model currently available. For most development teams, Fable 5 is the right model. Mythos 5 is a specialized tool for specific, vetted use cases.


---


## Fable 5's Safety Classifiers: What You Need to Know


Fable 5 ships with a new set of safety classifiers covering three areas:


1. **Cybersecurity:** Requests related to offensive cyber tasks, exploitation, and vulnerability development are handled by Claude Opus 4.8 instead. Fable 5 never touches these requests directly.
2. **Biology and chemistry:** Broadly scoped (conservatively, by Anthropic's own admission). Benign biology and chemistry requests may occasionally fall back to Opus 4.8. Anthropic plans to narrow these safeguards as the program matures.
3. **Distillation:** Requests flagged as attempts to extract Fable 5's capabilities to train competing models fall back to Opus 4.8.


When a fallback triggers, users are notified. Anthropic's data shows that more than 95% of Fable 5 sessions involve no fallback at all. The false positive rate is real but low. Anthropic describes the safeguards as intentionally conservative and committed to reducing false positives as the model matures.


For most developer use cases, coding, content, analysis, vision, and long-horizon agent tasks, you will not encounter these classifiers.


---


## What This Means If You Build on Claude


Fable 5 does not change the APIs you use or the SDKs you integrate. The integration path is identical to any Anthropic model. What changes is the capability ceiling.


If your application involves any of these workloads, Fable 5 is worth evaluating:


- **Agentic coding pipelines** where multi-file, multi-step tasks previously required human checkpoints
- **Long-context document analysis** (finance, legal, research)
- **Vision-based workflows** that previously required dedicated tooling
- **Complex analytical tasks** where model confidence and self-review matter
- **Any task where earlier Claude models needed multiple retries** to reach acceptable output


If you are currently on Opus 4.8 for production agent workloads, running the same benchmark workloads on Fable 5 is a natural next step.


---


## Connecting Fable 5 to a Content Layer


For teams building AI-native applications, pairing Fable 5 with a headless CMS gives you a clean separation between your AI logic and your content data. Here is how to use Fable 5 alongside the[Cosmic TypeScript SDK](https://www.npmjs.com/package/@cosmicjs/sdk) :


```text


```


```text


```


The model identifier is . Everything else about the integration is the same as any other Anthropic model. For agentic workflows using the Cosmic MCP Server, Fable 5 connects via the same endpoint. See the[Cosmic MCP Server guide](https://www.cosmicjs.com/blog/hosted-mcp-cosmic-in-cursor-claude-and-codex-with-zero-install) for the full setup.


---


## Fable 5 vs Opus 4.8: The Practical Decision


Dimension Fable 5 Opus 4.8


Capability tier Mythos-class Opus-class


Input pricing $10/M tokens $5/M tokens


Output pricing $50/M tokens $25/M tokens


Long-horizon tasks Best available Strong


Vision State of the art Capable


Agentic coding Leads benchmarks Strong (leads SWE-Bench Pro)


Safeguards Yes (cyber/bio classifiers) Standard


General availability Yes Yes


For tasks where you are currently running Opus 4.8 and hitting quality ceilings, Fable 5 is the next step up. For cost-sensitive, high-volume tasks where Opus 4.8 quality is sufficient, Opus remains the better value. The right call depends on your specific workload.


---


## Related Reading


- [Claude Code vs GitHub Copilot vs Cursor (2026): Honest Comparison](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026)
- [Claude Code vs Codex vs Cursor: The Best AI Coding Tool in 2026](https://www.cosmicjs.com/blog/claude-code-vs-codex-vs-cursor)
- [Claude Opus 4.8 Is Out: What It Means for AI-Native Development Teams](https://www.cosmicjs.com/blog/claude-opus-4-8-ai-native-development)
- [Claude Sonnet 4.6 vs Sonnet 4.5: A Real-World Comparison](https://www.cosmicjs.com/blog/claude-sonnet-46-vs-sonnet-45-a-real-world-comparison)
- [Claude Sonnet vs Opus for Coding: Which Model Should You Choose?](https://www.cosmicjs.com/blog/claude-sonnet-vs-opus-for-coding)
- [Hosted MCP: Cosmic in Cursor, Claude, and Codex with Zero Install](https://www.cosmicjs.com/blog/hosted-mcp-cosmic-in-cursor-claude-and-codex-with-zero-install)


---


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)[Log in](https://app.cosmicjs.com/login?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-login)


---


*Source:[Claude Fable 5 and Claude Mythos 5 — Anthropic](https://www.anthropic.com/news/claude-fable-5-mythos-5) , published June 9, 2026.*


*Tony Spiro is the CEO of Cosmic (cosmicjs.com), the AI-powered headless CMS for modern development teams.*
