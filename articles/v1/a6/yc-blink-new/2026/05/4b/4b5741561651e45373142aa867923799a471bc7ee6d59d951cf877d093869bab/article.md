---
schema_version: "1.0.0"
document_id: "4b5741561651e45373142aa867923799a471bc7ee6d59d951cf877d093869bab"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-opus-4-8"
published_at: "2026-05-29T00:23:56+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:50d743b97b8101fb30fb064bcd1c4ff9babc2d659e8013b3014429bcea760dfb"
---

# Claude Opus 4.8: What's New and Why Developers Should Care

## Fast mode: 3× cheaper


Fast mode for Opus 4.8 runs at 2.5× the speed of the standard model. The new pricing:


- **Fast mode input** : $10 per million tokens
- **Fast mode output** : $50 per million tokens


That's a **3× reduction** from previous fast mode pricing. For high-throughput agentic pipelines where speed matters more than maximum depth, fast mode is now significantly more viable.


Standard pricing remains unchanged from Opus 4.7: **$5 per million input tokens, $25 per million output tokens** . Prompt caching delivers up to 90% savings. Batch processing gives 50% off.


To use Opus 4.8 via the API, the model ID is` claude-opus-4-8` .


## Dynamic workflows: hundreds of parallel agents


Dynamic workflows in Claude Code let Opus 4.8 tackle tasks that are too large for a single agent pass. Claude writes an orchestration script, fans work across tens to hundreds of parallel subagents, and verifies outputs before surfacing a final answer.


Jarred Sumner, Bun's creator, ran this in practice. He used dynamic workflows to **port Bun from Zig to Rust** — roughly 750,000 lines of Rust — with **99.8% of the existing test suite passing** and **11 days from first commit to merge** . One workflow mapped Rust lifetimes for every Zig struct field. The next ran hundreds of parallel agents writing Rust files while two reviewers checked each one. A fix loop drove build and test until both ran clean.


That kind of task previously took months. It took days.


Dynamic workflows are available in research preview in Claude Code CLI, Desktop, and VS Code for Max, Team, and Enterprise plans. To enable ultracode (which activates automatic workflow orchestration), run` /effort ultracode` in Claude Code.


Read more about[how dynamic workflows work →](https://blink.new/blog/claude-code-dynamic-workflows)


## Messages API: system entries mid-task


The Messages API now accepts **system entries inside the messages array** . This means developers can update Claude's instructions during a task without breaking the prompt cache or routing the update through a user turn.


Practical use cases:


- Update permissions as the agent moves through workflow stages
- Adjust token budgets based on task complexity at runtime
- Pass updated environment context without resetting the conversation


This matters most for long-running agentic workflows where the environment changes — database connection state, available tool permissions, or remaining budget — without needing to restart the task context.


## What's coming next


Anthropic flagged two things in the announcement:


1.


**Models at Opus pricing with lower cost** — they're working on Sonnet-tier models that deliver Opus-level performance for specific use cases.


2.


**Claude Mythos** — a new class of model with higher intelligence than Opus. A small number of organizations are currently using Mythos Preview (codenamed "Project Glasswing") for cybersecurity work. Anthropic expects to bring Mythos-class models to all customers "in the coming weeks."


## Availability and pricing summary


Standard Fast Mode


Input $5 / M tokens $10 / M tokens


Output $25 / M tokens $50 / M tokens


Prompt caching Up to 90% savings —


Batch processing 50% savings —


Available today on Claude.ai (Pro, Max, Team, Enterprise) and via the Claude API (` claude-opus-4-8` ). Also available on Amazon Web Services, Google Cloud (Vertex AI), and Microsoft Foundry.


For US-only inference, pricing is 1.1× standard input and output rates.


## Build Opus 4.8 Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a multi-agent pipeline using Claude Opus 4.8 and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Use` claude-opus-4-8` when calling the API. It's available on the Anthropic API directly, and through Amazon Bedrock, Google Cloud Vertex AI, and Microsoft Foundry.


Higher effort levels cause Claude to think more deeply and use more tokens. Low and normal are fast and economical. High (the default) balances quality and speed. Extra is recommended for difficult tasks and long async workflows. Max is for critical work where you want Claude to reason as deeply as possible.


Yes. Fast mode runs at 2.5× the standard speed. Pricing is $10 per million input tokens and $50 per million output tokens — a 3× reduction from previous Opus fast mode pricing. Use it for high-throughput pipelines where latency matters more than maximum reasoning depth.


Dynamic workflows let Claude write orchestration scripts that run tens to hundreds of parallel subagents, verify outputs, and surface a single answer. They work with Claude Code and are available for Max, Team, and Enterprise plans. They work best with Opus 4.8 for extended, complex tasks. See the[full dynamic workflows guide →](https://blink.new/blog/claude-code-dynamic-workflows)


The API now accepts` system` role entries inside the` messages` array. This lets developers update Claude's instructions mid-task without breaking prompt caching or routing the update through a user turn. Useful for updating permissions, token budgets, or environment context as an agentic workflow progresses.


Anthropic described Mythos as a "new class of model with even higher intelligence than Opus." A small group of organizations are currently using Mythos Preview for cybersecurity work under Project Glasswing. Anthropic expects to bring Mythos-class models to all customers "in the coming weeks."


On the Super-Agent benchmark, Opus 4.8 is the only model to complete every case end-to-end at cost parity with GPT-5.5. On Online-Mind2Web (computer use), Opus 4.8 scored 84% — beating both Opus 4.7 and GPT-5.5. For coding specifically, see[Cursor vs Claude Code →](https://blink.new/blog/cursor-vs-claude-code) for a broader tool comparison.
