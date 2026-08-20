---
schema_version: "1.0.0"
document_id: "1e4eb96281c460532ca313a0936038d6515b288b8d222f90f0f89746711c0304"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-rss-4486f0fc66d1"
canonical_url: "https://jfrog.com/blog/why-model-routing-backfires/"
published_at: "2026-07-23T17:16:20+00:00"
first_seen_at: "2026-07-23T19:14:14.507084+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:ee6007a7c58c78cd794eafdb37ec5f9ab1e2f345279b3e2f736152adfdaf2c9f"
---

# Why Model Routing Backfires and How to Build Agents That Don’t Burn Your Budget

Model routing promises to cut your AI agent spend by offloading routine tasks to cheaper models like Claude Haiku while reserving frontier models like Claude Sonnet for complex reasoning. In the right configuration, routing strategies can reduce inference costs by[40–85%](https://proceedings.iclr.cc/paper_files/paper/2025/hash/5503a7c69d48a2f86fc00b3dc09de686-Abstract-Conference.html) . But if you implement routing incorrectly in a multi-turn agent, you can end up paying more than if you’d never routed at all. Here’s why and how to fix it.


## How Token Costs Accumulate Across a Session


Understanding why routing backfires requires understanding how agentic sessions accumulate costs. Each turn in a conversation compounds the token count, and that compounding is the engine behind both your savings and your risk.


Every turn in an agent session carries a growing token payload. The table below shows how the distribution shifts across a typical three-turn session:


**Turn** **New Input Tokens** **Cached Tokens**


Turn 1 High (baseline message) None


Turn 2 Low (follow-up message) High (all of Turn 1)


Turn 3+ Minimal Very high (compounding)


By Turn 3, cached tokens make up the vast majority of your token payload. This is the efficiency you’re trying to protect. Switching models mid-session destroys that protection entirely.


## Why Does Switching Models Mid-Session Increase Your Costs?


Switching LLM models mid-session wipes out accumulated prompt cache savings because every provider’s cache is model-specific — the new model has no access to the previous model’s stored history and must re-read the entire conversation from scratch at standard input token rates.


Prompt caches operate on prefix matching. The LLM provider stores a hash of your message prefix. As long as the prefix stays stable and the model stays the same, every subsequent turn benefits from heavily discounted cached token pricing.[Anthropic](https://www.anthropic.com/claude/sonnet) , for example, offers up to a 90% discount on cached input tokens. The moment you switch models, the new model starts with a cold cache. It processes the entire conversation history at full input token cost, as if Turn 1 never happened. For short sessions, this is a minor annoyance. For agentic coding workflows where context windows can stretch to 50,000 tokens or more, a single mid-session model switch can spike costs enough to eliminate all the savings routing was supposed to deliver.


## What You Should Not Do


Several common routing patterns look like optimizations but actively hurt your cost profile. These are the most frequent causes of unexpected cost increases in production agent systems:


- **Switching models in the middle of a long conversation without summarization** — This forces the new model to reprocess the entire conversation history at full input token cost. The cache miss penalty for a 20,000-token session can cost more than the savings the switch was intended to generate.
- **Enabling auto-routing mid-session in LLM gateways** — LLM gateway tools like LiteLLM, OpenRouter, and Portkey support automatic routing based on query complexity. This is powerful for single-turn queries, but enabling it mid-session in an agentic loop invalidates your KV cache at every routing boundary.
- **Modifying tools or system prompts mid-session** — Tool definitions sit inside the cached prefix. Adding or removing a tool during a session invalidates the entire cache downstream from that change. Treat session tool configuration as immutable once started.
- **Using compression calls with a separate model** — Many agents compress long history by spawning a summarization call with a cheap model. If that call uses a different model or a different system prompt, it generates its own cache miss and resets the main session cache on return.


Avoiding these patterns requires intentional session design, not just correct routing logic. Your routing strategy is only as good as your session architecture.


## What to Do Instead


Effective model routing in agentic systems is a session-level concern, not a query-level one. The following practices let you capture the cost benefits of routing without sacrificing the prompt cache savings that make agentic AI economically sustainable:


1. **Minimize turns per task and delegate early** — Keep individual agent sessions as short as possible. When you know a task requires extensive context (e.g., a complex multi-file refactor, a long-running compliance check) delegate it to a subagent at the start, before the session accumulates history. Short sessions mean small caches, which means a model switch costs less.
2. **Use the subagent pattern for model diversity** — Instead of switching models within a single session, build an orchestrating agent that delegates subtasks to purpose-built subagents. Each subagent starts fresh with only the context it needs and runs on whatever model is best for its specific task. No cache invalidation. No accumulated history penalty. This pattern (sometimes called the orchestrator-subagent architecture) is the cleanest way to get cost-based model diversity without paying the mid-session switch penalty.
3. **Summarize deliberately before any required model switch** — If a model switch mid-session is unavoidable, create a structured handoff: explicitly summarize the active context, the current task state, and any open decisions into a compact message. Start a fresh session with the new model using only that summary as input. You pay standard input costs on a small summary instead of cached costs on a 20,000-token history replay.
4. **Lock your session configuration** — System prompts, tool definitions, and model selection should be fixed at session start. Treat them like a schema — changing them mid-session is a migration event, not a configuration tweak. If your agent framework allows dynamic tool loading, audit where and when those loads happen relative to your cache prefix boundary.


## How JFrog Boost Protects Your Cache Efficiency Automatically


While managing model routing logic is critical for agent sustainability, context bloat acts as a massive force multiplier for token waste. Every time an agent runs a build, executes a test suite, or pulls logs, hundreds of lines of repetitive terminal noise flood the conversation history.


In a multi-turn session, this noise compounds rapidly. It turns what should have been a lean history into a 50,000-token payload within a few turns. This directly breaks your cost engineering: if your session is bloated with terminal noise, the “cache miss penalty” we discussed earlier becomes exponentially more expensive the moment a mid-session model switch occurs.


When utilizing coding agents, developers shouldn’t have to manually curate terminal output just to prevent a noisy build step from inflating their LLM bill.


**[JFrog Boost](https://boost.jfrog.com/) is designed to solve this context bloat directly within the developer workflow.** Boost is a lightweight CLI tool that intelligently compacts terminal output during agentic loops, stripping out repetitive boilerplate while perfectly preserving the critical signal, like exact error stack traces. Instead of sending thousands of tokens of raw build logs to your model, Boost trims the payload before it ever hits the agent.


By keeping your session history lean, Boost drastically reduces the baseline tokens processed per turn. This minimizes the financial penalty of unavoidable model routing switches, extends the life of your context window, and maximizes the economic efficiency of your prompt caches.


## Designing for Sustainable Agent Economics


Model routing delivers real cost savings, but only when it respects the underlying mechanics of provider prompt caches. To build sustainable agent workflows, treat your conversation history as a highly optimized asset: Keep your primary sessions lean, delegate heavy context to short-lived subagents, and ensure any necessary model transition is handled through a structured state summary rather than a raw history replay.


Ultimately, engineering sustainable agentic systems requires treating token efficiency as a core constraint alongside speed and accuracy.


To implement these session-level optimizations automatically and eliminate context waste in your development workflows, you can[install Boost](http://boost.jfrog.com/) and start saving tokens today.


### Connect with us


We would love to hear about your experiences, architectural patterns, and challenges with model routing. Feel free to reach out or follow our ongoing research into coding agents:


→ **X:**[https://x.com/ShayFrektman](https://x.com/ShayFrektman) ,[https://x.com/yahav_ohana](https://x.com/yahav_ohana)


→ **LinkedIn:**[https://www.linkedin.com/in/shay-dahan](https://www.linkedin.com/in/shay-dahan) ,[https://www.linkedin.com/in/yahav-ohana/](https://www.linkedin.com/in/yahav-ohana/)


→ **Substack:**[https://substack.com/@yahavohana](https://substack.com/@yahavohana)
