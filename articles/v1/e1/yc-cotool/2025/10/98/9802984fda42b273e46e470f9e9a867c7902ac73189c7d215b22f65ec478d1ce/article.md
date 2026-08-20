---
schema_version: "1.0.0"
document_id: "9802984fda42b273e46e470f9e9a867c7902ac73189c7d215b22f65ec478d1ce"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-db39e53ee409"
canonical_url: "https://www.cotool.ai/blog/context-management"
published_at: "2025-10-20T00:00:00+00:00"
first_seen_at: "2026-07-21T15:09:13.517815+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:0ba8deb075b4a0f998d31465cef903ff1f35adbdbaa85debf0b464f6661a52f0"
---

# Context Management for Agentic Security

Context management is a core engineering problem for any reliable agent harness. At Cotool, our users rely on us to automatically solve this for their custom agentic security flows. These flows involve autonomously querying SIEMs, analyzing logs, and following investigation paths. However, users build the agents: they can choose any model, write custom system prompts, and connect to security tools through our in-house connector platform, and we need to be able to manage context throughout.


**The Problem** : the LLM decides what data to retrieve and when. The agents query SIEMs, retrieve logs, read detection-as-code, and explore threat intelligence feeds to bring that data into context dynamically.


We thought context management would be a solved problem. Traditional patterns like RAG, chunking, and sliding windows work well for document Q&A and code generation. They don't work for security investigations. In this post, we'll walk through why security operations tasks are fundamentally different, the solutions we tried, and what we eventually built.


## The Security Data Problem


Security tools are verbose. Extremely verbose.


Most security workflows center around log aggregators and SIEMs. These systems are designed to capture everything because security is fundamentally about anomaly detection: finding needles in haystacks. That suspicious PowerShell command buried in line 847 of a 5,000-line log dump? That could be your incident.


This creates a paradox for context management: you can't aggressively prune data before it enters the context window because you don't know what's anomalous yet. Any line, any field, any timestamp could be the critical piece of evidence. Traditional chunking and semantic search fall flat here; security logs are typically structured, and the anomaly is only meaningful in the context of surrounding logs.


We've had some success giving LLMs access to pruning tools: direct SQL-like SIEM queries, grep utilities, and filtering capabilities. But with long enough agent loops tackling complex investigations, you still eventually hit the context ceiling.


## Our Requirements


Based on our use case, we established three hard requirements:


**Speed** : Our agents need to respond quickly. We have interactive flows where high latency breaks the user experience. Any context management solution that adds seconds to every agent turn is a non-starter.


**Minimal Compression** : While context rot exists, our hypothesis is that models perform best on time-based correlation and anomaly detection when they can access as much tool output as possible. We only wanted to prune or compact when absolutely necessary.


**Zero Context Errors** : Running out of context mid-investigation is an unacceptable failure mode.


## Measuring Context: Harder Than It Looks


To manage context, you first need to measure it accurately. But here's the constraint: you need a measurement system that always overestimates (to avoid context errors) but doesn't overestimate by too much (to avoid unnecessary pruning). We set a target of no more than 110% overestimation.


Cotool supports all the major LLM providers, but this introduces a challenge because each model provider uses a different (and in some cases, black-box and proprietary) tokenizers. In practice, the token lengths vary dramatically between providers for the same messages, so your estimator needs to be provider-aware.


### What We Tried First


**Generic Character Estimation** : We tried` JSON.stringify(messages).length / 4` . This was either a massive overestimate or underestimate depending on the content mix (which we get into later); useless in practice.


**OpenAI + Tiktoken** : We started with Tiktoken, OpenAI's open-source encoder. On an M4 MacBook Pro, encoding a 673K token conversation takes ~368ms. Not heinous, but not particularly latency or resource-efficient either.


On our production GCP Cloud Run instances (CPU-only, serverless), the numbers were significantly worse: **3.3 seconds to encode 678K tokens** . This is significant additional latency for interactive UX’s on top of the providers’ time to first token.


More importantly, from first principles, this approach is heavy: do we need to fully tokenize the entire conversation repeatedly just to estimate token counts? And critically, this only solves the problem for OpenAI. We still needed solutions for Anthropic and Google, for example.


**Provider Token APIs** : Anthropic and Google both offer endpoints that return token counts. These were vulnerable to rate limits and still introduced unacceptable latency for multi-turn conversations. It also left us without a paddle for OpenAI


### What Actually Worked


We built a custom token estimator using three key insights:


1. **Encoders behave differently for different input types.** JSON payloads encode differently than files, which encode differently than plain text.
2. **Token counts are additive across messages.**` estimateTokens(m1, m2, m3) = estimateTokens(m1) + estimateTokens(m2) + estimateTokens(m3)`
3. **LLMs can "learn" estimators through test-driven iteration.**


Here's what we did:


1. Built a labeled dataset by generating diverse conversations across model providers and capturing actual token counts from API responses.
2. Initialized base weights for each message type within functions for each provider and applied
3. Created unit tests with real messages and their known token counts.
expect(estimateTokens(m1)).toBeGreaterThan(actualCount);
` expect(esimateTokens(m1)).toBeLessThanOrEqualTo(actualCount * 1.1);`
4. Used Claude Code to iteratively adjust the estimation function until tests passed within our threshold.


**The result:** a performant, interpretable function that mimics provider token counts on a distribution of data we care about within 110% accuracy, with latency bound to computing lengths of serialized JSON strings. We considered applying linear regression over the dataset to compute optimized weights as well, but we were interested in pushing the coding agent to optimize the entire estimation function.


## Windowing Techniques: What We Learned the Hard Way


### Attempt 1: Large Context Models


Initially, we just used large context window models and hoped for the best. This worked fine when use cases were simple. As investigations got more complex, requiring deeper log analysis and more tool calls, we started hitting limits even on million-token models like Gemini 2.5-pro.


### Attempt 2: Sliding Window


We implemented a sliding window that removed old messages when context exceeded limits. This somewhat worked, but Anthropic broke everything.


**Problem 1: Anthropic's Reasoning Requirements**


Anthropic requires that assistant messages include reasoning content (thinking blocks) when the model supports extended thinking. If you remove an old assistant message that had reasoning but keep a newer one without it, the API throws an error. You can’t insert fake reasoning messages because Anthropic signs their reasoning messages to prevent mutating the chain-of-thought.


**Problem 2: AI SDK Tool Call/Result Pairing**


The AI SDK enforces strict pairing between tool calls and tool results. You can't have an assistant message with a tool-call without the corresponding tool message containing the tool-result, and vice versa.


These two constraints do not prevent you from implementing a sliding window approach, but they require you to maintain model-specific state and potentially over-prune messages in order to be compliant with the Anthropic and AI SDK APIs. Rather than maintain a complex state-machine with these variables to achieve a sliding window, we thought there might be an easier way where we keep the entire conversation history in context.


### Attempt 3: Tool Result Pruning (Current Solution)


Our current approach keeps all messages in order but truncates their content strategically.


We score each message based on age and size, prioritizing tool results first. Starting with the oldest, largest tool results, we prune content until we're back under the context limit. If tool results aren't enough, we continue with other message classes.


This preserves conversation structure while intelligently removing the least critical data. It's not perfect, but it works reliably across providers and maintains the context integrity that Anthropic and the AI SDK demand.


**How It Works**


Instead of removing entire messages, we replace oversized content with placeholders while preserving message structure:


The pruning happens in phases:


1. **Tool result outputs** (oldest, largest first) - except the most recent
2. **Assistant tool-call inputs** - except the latest assistant message
3. **Last tool result** - if still over budget
4. **Assistant text content** - except latest assistant
5. **User content** - except first and last user messages


Each phase exits early if we're back under budget, ensuring minimal pruning.


## Our Learnings


**Context management is still unsolved for security AI.** The patterns that work for document Q&A and code generation don't translate to security workflows.


**Security logs are fundamentally different.** They're not semantic, they're anomaly-focused, and you need surrounding context to derive meaning. RAG and chunking don't help when the needle could be anywhere in the haystack.


**Generic agents are harder to manage.** Giving control to the LLM produces better investigations but creates arbitrary conversation lengths with unpredictable tool usage. You can't pre-optimize for specific flows.


**LLM-assisted optimization is powerful.** Using an LLM coding tool to hill-climb toward an optimal solution through test cases proved surprisingly effective for hairy estimation problems. We'll definitely be applying this pattern elsewhere.


If you're building AI agents for security... or any domain with verbose, anomaly-driven data... traditional context management won't cut it. You need to measure fast, prune conservatively, and design for the fact that the LLM is in control, not you.


*Cotool is a platform for building custom security AI agents. If you're dealing with alert fatigue and want to explore agentic automation, reach out.*


## Glossary


**AI SDK** : A software development kit (in this case, from Vercel) that provides standardized interfaces for working with different LLM providers and managing conversation flows, including tool calls and responses.


**Chain-of-Thought** : A reasoning technique where models explicitly show their thinking process step-by-step before arriving at conclusions, improving logical reasoning and transparency.


**Chunking** : The practice of breaking large documents or data into smaller pieces that fit within context limits, typically used in RAG systems.


**Context Window** : The maximum amount of text (measured in tokens) that an LLM can process in a single request, including both input and output. Different models have different limits (e.g., 128K, 1M tokens).


**Context Rot** : The degradation in model performance that occurs when context windows become too full, causing the model to lose track of earlier information or make errors.


**Extended Thinking / Reasoning Blocks** : Features in some models (like Anthropic's Claude) where the model performs internal reasoning that can be preserved in the conversation but may have special requirements for message structure.


**RAG (Retrieval-Augmented Generation)** : A technique where relevant information is retrieved from a knowledge base and injected into an LLM's context to answer questions, typically using semantic search.


**Semantic Search** : A search technique that finds information based on meaning and context rather than exact keyword matching, often using embeddings to compare similarity.


**Sliding Window** : A context management technique that maintains a fixed-size "window" of recent messages by removing older messages as new ones are added.


**Time to First Token (TTFT)** : The latency between sending a request to an LLM and receiving the first token of its response, a key metric for user experience.


**Token / Tokenization** : The process of breaking text into smaller units (tokens) that LLMs process. A token is roughly 4 characters or ¾ of a word. Token count determines context usage and API costs.


**Tokenizer** : The algorithm or tool that converts text into tokens. Different model providers use different tokenizers (e.g., Tiktoken for OpenAI), which can produce different token counts for the same text.


**Tool Call** : When an LLM requests to execute a specific function or API (like querying a database), including the function name and arguments. The result comes back as a "tool result."
