---
schema_version: "1.0.0"
document_id: "ad7148edca0fe45d44c49d83e9a6b4238f0a5334432616ecba8c454762ded360"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/vapi-review-2026-is-this-voice-ai-platform-right-for-your-project/"
published_at: null
first_seen_at: "2026-07-21T15:11:00.592145+00:00"
fetched_at: "2026-07-28T21:46:32.935029+00:00"
content_hash: "sha256:2a4d7480df217f34155b79e24582e3ef4dc20eaeb64d63110bbcf952fb7b53e0"
---

# Vapi Review 2026: Composer, Evals & Vapi Voices

Vapi closed a[$50M Series B at a ~$500M valuation in May 2026](https://vapi.ai/blog/series-b) , with Amazon Ring picking it over 40 other voice AI vendors after a 2025 holiday-season bake-off. In the four months before that, Vapi shipped Composer (a natural-language agent builder), Monitoring (production observability), Evals + Simulations (native testing), Vapi Voices Beta (proprietary TTS at $0.0025/min), and integrations with GPT-5, GPT-5.1, and OpenAI Realtime. If your last review of Vapi was from mid-2025, almost every load-bearing surface has changed.


This guide from Coval covers what Vapi actually delivers in 2026: the product surface (Assistants, Squads, Composer, Workflows, Chat), provider integrations across LLM/STT/TTS, pricing post-Series B, the observability trio Vapi now ships natively, and the trade-offs that decide whether it’s the right platform for your project. Where helpful, comparisons reference other 2026[voice AI platforms](https://www.coval.ai/blog/voice-ai-platforms-in-2025-a-conversation-with-vapi-founder-jordan-dearsley) ,[TTS providers](https://www.coval.ai/blog/best-text-to-speech-providers-in-2026-how-to-choose-(and-why-vendor-benchmarks-lie)) , and[STT providers](https://www.coval.ai/blog/best-speech-to-text-providers-in-2026-independent-benchmarks-and-how-to-choose) . It ends with how teams evaluate Vapi (and any competitor) in production using vendor-agnostic test infrastructure.


> **Key takeaways**
>
>
> - **Series B closed at ~$500M valuation** (May 12, 2026, Peak XV-led). The big proof point: Amazon Ring evaluated 40+ vendors during the 2025 holiday spike and chose Vapi, zero-to-production in two weeks. 1B+ calls, 1M+ developers, 10x enterprise ARR YoY.
> - **Composer** (Alpha, Feb 2026) is a natural-language copilot inside the dashboard that builds assistants, configures phone numbers, sets up tools and integrations. It can’t delete resources or auto-deploy. Materially shifts the “who is Vapi for” answer beyond engineers.
> - **Vapi Voices Beta** ships proprietary TTS at $0.0025/min. Old per-minute cost estimates quoting premium ElevenLabs voices are now misleading by 30–40% at the high end.
> - **Native observability** is now a real story: Evals (JSON-defined tests, 3 judge types, CLI for CI/CD), Simulations (AI-caller-driven end-to-end tests), Monitoring (production observability across effectiveness, compliance, technical, infrastructure). Honest treatment matters because this is where Vapi overlaps with vendor-agnostic eval platforms like Coval.
> - **Workflows is being de-emphasized** in docs in favor of Assistants. Any review that leads with “Vapi has a visual workflow builder” is misframing the current product.
> - **OpenAI Realtime-2 support** (May 7, 2026 launch — GPT-5-class reasoning baked into the voice model, with exclusive Cedar and Marin voices). Supersedes the earlier` gpt-realtime-2025-08-28` model. Squads + Silent Handoffs (Nov 2025) is the recommended pattern for complex multi-step agents; Fleetworks runs 240,000 calls/day on Squads.
> - **Pricing** stays usage-based on the Build plan ($0.05/min platform fee, provider passthrough at cost, $0 with BYO API keys). Enterprise / Scale is committed-volume with SLA, SOC 2 + HIPAA, SSO, RBAC. HIPAA and Zero Data Retention are paid add-ons on Build.


## What shipped in 2026


The Vapi most teams remember from 2024 was a developer-first orchestration layer with Twilio integration and a few model providers. The Vapi of 2026 is a different surface. The shipped-in-2026 list, with sources:


- **[$50M Series B at ~$500M valuation](https://vapi.ai/blog/series-b)** (May 12). Peak XV-led, with M12 (Microsoft), Kleiner Perkins, Bessemer, and YC participating. Total funding $72M. Amazon Ring is the flagship case study: chose Vapi over 40+ rivals, zero-to-production in two weeks per Ring’s VP of Software, customer-satisfaction scores improved post-deployment.
- **[Composer Alpha](https://vapi.ai/blog/composer)** (Feb 11). A conversational copilot inside the dashboard that builds, configures, and troubleshoots voice agents from natural-language descriptions. Materially expands who can build on Vapi.
- **[Monitoring](https://vapi.ai/blog/monitoring)** (Apr 15). Production observability across Effectiveness (intent fulfillment, early hang-ups), Compliance (prompt adherence), Technical (tool/STT/LLM/TTS failures), and Infrastructure (latency, dropped calls, concurrency). Effectiveness + Compliance are Enterprise-gated.
- **[Evals](https://vapi.ai/blog/evals)** (Dec 3, 2025). JSON-defined mock-conversation tests with three judge types: exact match / regex, LLM-as-judge (Claude, GPT-4, etc.), and tool-call verification. Runs via CLI in CI/CD.
- **Simulations.** AI-powered “tester” callers that actually call the assistant with configurable personas and scenarios. Produces full call recordings. Pass/fail via structured-output comparators.
- **[Squads + Silent Handoffs](https://vapi.ai/blog/introducing-squads)** (Nov 13, 2025). Multi-assistant orchestration with controlled context transfer (none / last N / full history). “Traffic controller” assistant pattern is the recommended architecture. Silent handoffs mean callers don’t hear the routing transition.
- **[Vapi Voices Beta](https://vapi.ai/blog/vapi-voices-beta)** (Dec 17, 2025). Proprietary TTS at $0.0025/min for base-case use cases (appointment reminders, verification, routing menus, status updates). Premium ElevenLabs and Cartesia voices remain available for naturalness-critical work.
- **[OpenAI Realtime](https://vapi.ai/blog/gpt-5-realtime-in-vapi)** — Realtime-2 (May 7, 2026 launch) supersedes the earlier` gpt-realtime-2025-08-28` . Native speech-to-speech, no separate STT/TTS, GPT-5-class reasoning, with Cedar and Marin as exclusive voices. Trade-off: knowledge bases not supported with Realtime, custom voice cloning unavailable.
- **GPT-5 (Aug 2025) and GPT-5.1 (Nov 2025).** GPT-5.1 brings adaptive reasoning and OpenAI’s 8 personality presets.
- **[Claude Skills + MCP connector](https://vapi.ai/blog/claude-skills)** (Feb 25, 2026).` npx skills add VapiAI/skills` lets Claude Code, Cursor, and other coding agents build Vapi voice agents without hallucinating APIs.


If your prior mental model of Vapi was “good for dev hacking, weak on observability, no native testing,” that hasn’t held for at least six months.


## Composer: natural-language agent building


Composer is the biggest “who is this for” shift since Vapi launched. The product is a conversational copilot inside the Vapi dashboard. The user describes what they want; Composer asks clarifying questions, then builds the assistant, configures phone numbers, sets up tools and integrations, and recommends fixes when something doesn’t work.


What Composer can do:


- Create and configure assistants from a natural-language brief
- Provision and configure phone numbers
- Set up custom tools and webhooks
- Adjust prompts, voice, model choice, and conversation flow
- Recommend fixes for issues it diagnoses


What Composer cannot do (Alpha as of mid-2026):


- Delete resources
- Deploy autonomously without user confirmation
- Make business-logic decisions on behalf of the user
- Replace a full engineering review for production deployments


The practical effect: PMs, ops leads, and customer support owners can build first-pass voice agents without engineering involvement. Engineering still owns the production hardening — Composer doesn’t replace evaluation, regression testing, or load testing.


## Assistants vs Squads vs Workflows


Vapi ships three composition primitives, and the docs are now explicit about which one to start with.


Primitive When to use Status


**Assistant** Single agent handling one task or domain. Default starting point. Recommended for most new projects.


**Squad** Multi-agent orchestration where specialists (qualifier, booker, biller) hand off control. Silent handoffs hide the routing from callers. Recommended for complex flows. Fleetworks does 240k calls/day on Squads.


**Workflow** Visual flow builder with branching logic, transfer-call, end-call, API-request nodes. Maintained but no longer the recommended path; docs now point most users to Assistants first.


**Practical guidance:** start with an Assistant, escalate to a Squad once you need explicit specialization between agents. Workflows is appropriate when you genuinely need deterministic branching (regulated scripts, multi-step compliance) but is the wrong default for most teams.


## Provider integrations in 2026


The “open stack” thesis is now Vapi’s defining positioning argument. The current integration list:


**LLM providers.** OpenAI (including GPT-5, GPT-5.1, and Realtime-2, the May 7, 2026 successor to` gpt-realtime-2025-08-28` ), Azure OpenAI, Anthropic (via Bedrock), Google Gemini, Groq, DeepInfra, Perplexity, TogetherAI, OpenRouter, plus custom-LLM-server support.


**STT / transcription.** Deepgram (including Flux with audio-text mode), Google, Gladia, Speechmatics, Talkscriber, AssemblyAI (with end-of-turn detection), LiveKit (text-based), Krisp (audio-based), and a Vapi-native text-based transcriber for non-English. Fallback chains supported.


**TTS / voice.** Vapi Voices Beta (proprietary, $0.0025/min), ElevenLabs, PlayHT, Azure, OpenAI, Cartesia, LMNT, Minimax, RimeAI. Custom voice cloning supported on most providers but not on OpenAI Realtime.


**Telephony.** Twilio, Telnyx, Plivo, Zadarma, Amazon Chime SDK, native Vapi phone numbers. SIP integration supported.


**Cloud artifacts.** AWS S3, GCP Cloud Storage, Cloudflare R2, Supabase S3.


The positioning argument: “the best STT, LLM, and TTS in May 2026 probably won’t be the best in November 2026, so architecture that lets you swap matters more than locking in to any one vendor’s stack.” That argument lands better when paired with vendor-agnostic evaluation infrastructure that can measure provider swaps apples-to-apples, which is the gap that platforms like Coval fill on top.


## Pricing in 2026


**Build plan (usage-based, self-serve):**


- Platform fee: **$0.05/min** of Vapi hosting
- 60+ minutes included
- 10 concurrent lines included; +$10/line/month for more
- SMS / chat: $0.005/msg
- Provider passthrough (STT/LLM/TTS): **at cost** , or **$0 if BYO API key**
- Data retention: calls 14 days, chat 30 days
- Support: Community Discord + email


**Scale plan (annual contract, Enterprise):**


- Fixed platform fee + committed-volume per-minute rate (negotiated)
- 99.9% uptime SLA
- Custom retention
- Dedicated account team, SOC 2, HIPAA, PCI
- SSO / OAuth / RBAC, AI guardrails, dedicated deployment support


**Add-ons (both plans):** HIPAA $2,000/mo, Zero Data Retention $1,000/mo.


**Typical per-minute cost example (Build plan, no BYO):** ~$0.05 (Vapi) + Deepgram STT pass-through + LLM pass-through + TTS pass-through. Vapi Voices Beta at $0.0025/min collapses the TTS component to near-free. Telephony (Twilio, Telnyx) is billed separately by the carrier; Vapi-native numbers have their own per-minute cost (not surfaced on the public pricing page).


For unit-economics modeling, the meaningful variables are: which LLM you choose (largest cost contributor in most stacks), whether you use Vapi Voices Beta or a premium TTS, and how much of the traffic is short-duration vs long-form.


## Vapi’s native observability: Evals, Simulations, Monitoring


Vapi shipped a three-product observability suite in late 2025 / early 2026. For a comparison article, this is the area that requires careful treatment because it overlaps with what teams previously used Coval and similar platforms for.


**Evals (Dec 3, 2025).** JSON-defined mock-conversation tests. Three judge types: exact match / regex, LLM-as-judge (Claude, GPT-4, etc.), and tool-call verification. CLI for CI/CD integration. Dashboard supports one-click “convert this production call into a regression test.” Strong for: per-deploy gating, simple conversational tests, fast feedback in dev.


**Simulations.** AI-caller-driven end-to-end tests. Configurable personas and scenarios; voice or chat mode; produces full call recordings; pass/fail via structured-output comparators. Strong for: testing how the agent handles realistic callers (interruptions, ambiguous goals, audio variations).


**Monitoring (Apr 15, 2026).** Production observability across four monitor classes: Effectiveness (intent fulfillment, early hang-ups — Enterprise only), Compliance (prompt adherence — Enterprise only), Technical (tool/STT/LLM/TTS failures), Infrastructure (latency, dropped calls, concurrency violations). Surfaces occurrence volume, callers affected, trend direction, and recommended fixes.


The honest assessment: Vapi’s native observability is real and useful for teams committed to Vapi specifically. The trade-offs:


- **Vendor lock-in.** Vapi Evals and Monitoring only measure Vapi. If you want to A/B Vapi against Retell, Bland, or an in-house orchestration, you need a vendor-agnostic eval layer.
- **Scenario library depth.** Vapi’s eval framework is designed for relatively short test definitions. Production-grade test suites typically grow to hundreds or thousands of scenarios, with statistical thresholds, regression-aware diffing, and a hub-and-spoke structure that some teams find easier to manage in a dedicated eval platform.
- **Effectiveness / Compliance gating.** The two most consequential Monitoring tiers are Enterprise-only. Teams on the Build plan see Technical and Infrastructure metrics only.


For teams running multiple voice AI vendors, building or buying a dedicated eval platform tends to outperform any one platform’s native observability. For teams committed to Vapi as the single vendor, Vapi’s native suite is enough to cover most of the regression-detection use cases that previously forced teams to integrate Coval or similar from day one.


## Latency and architecture choices


Vapi’s public latency claims:


- Homepage: **“<500ms average latency”** and “sub-500ms latency” at enterprise scale
- Architecture page: voice-to-voice cycle target **500–700ms** , with 50–100ms sensitivity per stage (STT → LLM → TTS)
- OpenAI Realtime path: marketed as “ultra-low latency” via native speech-to-speech (no specific ms target published)
- Vapi Voices Beta: “optimized for conversational responsiveness”; P90 benchmark not yet published


What this means for architecture choice:


- The **cascaded STT → LLM → TTS** stack is what Vapi runs by default. Each component contributes ~50–100ms; total voice-to-voice budget is ~500–700ms.
- The **OpenAI Realtime** option collapses STT and TTS into one speech-to-speech model. Lower latency in theory; trade-off is you lose knowledge-base support and custom voice cloning.
- The right architecture depends on whether your use case tolerates the Realtime trade-offs (knowledge base, voice control) and whether you need vendor-swap flexibility (Realtime locks you to OpenAI).


Vapi’s own positioning leans toward the cascaded “open stack” architecture for production-grade use cases. The case being made: models rotate faster than architectures, and swap-ability beats integration. For an outside view on this trade-off, see the[speech-to-speech vs cascaded architecture comparison](https://www.coval.ai/blog/speech-to-speech-vs-cascaded-voice-ai-which-architecture-should-you-deploy) .


## Vapi review verdict: when it’s the right choice


**Strong fit when:**


- You want an open, swap-friendly architecture across LLM, STT, and TTS providers
- You’re comfortable with cascaded latency (~500–700ms voice-to-voice) and prefer flexibility over speech-to-speech latency
- Telephony is in scope (Twilio / Telnyx / Plivo or BYO SIP) and you don’t want to build the voice-PSTN bridge yourself
- You want a viable native eval and monitoring story without adding a separate platform (at least until vendor breadth becomes a constraint)
- You’re building B2B-grade voice agents (contact center, sales outbound, support, healthcare, fintech) where the customer expects telephony, compliance options, and reliable scale


**Likely better elsewhere when:**


- You need the absolute lowest possible latency and accept OpenAI Realtime’s trade-offs (consider Realtime directly, or an integrated platform like ElevenAgents)
- You’re shipping a fully managed CX product where opinionated workflows beat orchestration flexibility (Sierra, Decagon, Cresta, Parloa, Replicant — see the[voice AI agent architecture guide](https://www.coval.ai/blog/voice-ai-agents-architecture-deployment-evaluation) )
- You’re running multiple voice AI vendors in parallel and need vendor-agnostic evaluation across all of them (this is where a separate eval layer like Coval becomes necessary, not optional)
- You want a fully visual, no-code agent builder for non-technical teams (Composer Alpha helps, but isn’t yet a complete replacement for purpose-built no-code platforms)


## How teams evaluate Vapi in production


Vapi publishes its own latency, accuracy, and effectiveness benchmarks. So does every other voice AI platform. Vendor-reported benchmarks pick conditions that flatter the system being measured. Production traffic looks nothing like the benchmark conditions.


The teams running voice AI at scale build evaluation infrastructure that’s vendor-agnostic and use-case-specific. The standard pattern, in three layers:


1. **A test set drawn from actual production audio.** Speakerphone, accents, frustrated tones, hold-music bleed-through, multi-intent requests, edge-case business logic. Not the demo calls Vapi (or any vendor) uses for their own benchmarks.
2. **Behavioral graders, not just exact-match.** Language-model graders that score whether the agent resolved the call, handled the tone appropriately, stayed on-policy, and called the right tools with the right parameters. STT WER and TTS naturalness are inputs; “did the conversation resolve” is the outcome metric.
3. **Continuous regression testing.** Every prompt change, model swap, provider update, or backend integration tweak runs against the same suite before it ships. Vendor model updates that don’t change the version string (silent drift) are caught by the same suite.


Coval is the evaluation infrastructure layer for that pattern. We’re vendor-agnostic by design: the test set you build for a Vapi-based agent runs unchanged against the same agent rebuilt on Retell, ElevenAgents, or an in-house orchestration, and produces apples-to-apples scoring. Public TTS and STT benchmarks live at[benchmarks.coval.ai/tts](https://benchmarks.coval.ai/tts) and[benchmarks.coval.ai/stt](https://benchmarks.coval.ai/stt) ; the methodology is documented in our[voice observability guide](https://www.coval.ai/blog/voice-observability-guide) . For teams already on Vapi, our[Vapi + Coval integration writeup](https://www.coval.ai/blog/vapi-and-coval-powering-end-to-end-voice-ai-reliability-at-scale) covers how the two stacks fit together.


The bigger point is that any honest 2026 Vapi review depends on what your production traffic actually looks like, not what Vapi’s marketing page measures.


## Frequently asked questions


**Is Vapi a good choice in 2026 after the Series B?**


Yes, with caveats. Post-Series B Vapi is on stronger financial and customer footing: $500M valuation, 10x enterprise ARR YoY, 1B+ calls, 1M+ developers, Amazon Ring as the flagship enterprise win. For teams that want an open, swap-friendly voice AI stack with native eval and monitoring, Vapi is one of the strongest options. For teams that need a fully integrated speech-to-speech architecture or vertical-specific CX workflows, other platforms may fit better.


**How much does Vapi cost per minute in 2026?**


On the Build plan, the platform fee is $0.05/min, plus provider pass-through (STT + LLM + TTS) at cost, or $0 on provider cost if you BYO API keys. Vapi Voices Beta at $0.0025/min effectively zeroes the TTS line item for base-case use cases. Total per-minute cost on a typical cascaded stack lands in the $0.07–$0.15 range depending on LLM and TTS choices. Enterprise / Scale contracts negotiate committed-volume per-minute rates.


**What is Vapi Composer and who is it for?**


In practice: a product manager describes “I want a callback agent that confirms appointment times with our top-100 healthcare customers and escalates anything outside the next 7 days” to Composer, and gets a working draft assistant they can refine without filing an engineering ticket. The same workflow on Retell, ElevenAgents, or a custom orchestration would either require an engineer to wire it up or sit untouched until the next sprint. Composer’s hard limits are deletion and auto-deployment — both intentional, so prototypes don’t accidentally turn into shipping production agents. Best fit for teams where the prototyping bottleneck is engineering capacity rather than ideation; less useful if you already have a no-code voice-agent builder integrated into an existing CX tool.


**Does Vapi have native evals and monitoring?**


Yes. Evals (Dec 2025) provide JSON-defined tests with three judge types (exact match / regex, LLM-as-judge, tool-call verification) and CLI integration for CI/CD. Simulations layer AI-caller-driven end-to-end testing on top. Monitoring (Apr 2026) covers Effectiveness, Compliance, Technical, and Infrastructure metrics — Effectiveness and Compliance are Enterprise-only. Teams running multiple voice AI vendors generally still benefit from a vendor-agnostic eval layer on top.


**Does Vapi support OpenAI Realtime and GPT-5?**


Yes. Realtime-2 (May 7, 2026 launch — GPT-5-class reasoning, Cedar and Marin voices) is now the current Realtime model and supersedes the earlier` gpt-realtime-2025-08-28` . GPT-5 has been live in Vapi since Aug 2025; GPT-5.1 since Nov 2025. The Realtime path collapses STT + TTS into native speech-to-speech (lower latency, no knowledge-base support, no custom voice cloning). The cascaded path remains the default for use cases that need provider swap-ability or knowledge bases.


**Should I use Assistants or Workflows in 2026?**


Assistants. Vapi’s docs now recommend Assistants as the default starting point; Workflows is maintained but no longer the recommended path. For multi-step or specialized flows, escalate to Squads with Silent Handoffs rather than building a visual Workflow. Fleetworks runs 240k calls/day on Squads.


**Is Vapi HIPAA-compliant?**


HIPAA support is available as a $2,000/mo add-on on the Build plan and is included in the Scale plan. SOC 2 and PCI are also in place; Zero Data Retention is a separate $1,000/mo add-on. Production deployments in regulated industries should pair Vapi’s compliance posture with the customer’s own DPA, BAA, and audit-log requirements during procurement.


**Can I use Vapi without managing the telephony layer?**


Yes. Vapi-native phone numbers are provisioned directly through the platform. Alternatively, BYO via Twilio, Telnyx, Plivo, Zadarma, or Amazon Chime SDK is supported, with SIP integration for custom telephony.


## Where to go from here


If you’re early in evaluating voice AI platforms, the[voice AI agent architecture guide](https://www.coval.ai/blog/voice-ai-agents-architecture-deployment-evaluation) covers the platform landscape and which fit which use cases. For provider-level depth, the[TTS providers guide](https://www.coval.ai/blog/best-text-to-speech-providers-in-2026-how-to-choose-(and-why-vendor-benchmarks-lie)) and[STT providers guide](https://www.coval.ai/blog/best-speech-to-text-providers-in-2026-independent-benchmarks-and-how-to-choose) cover the layers Vapi orchestrates. For the comparison piece on cascaded vs. speech-to-speech architectures, see[speech-to-speech vs cascaded](https://www.coval.ai/blog/speech-to-speech-vs-cascaded-voice-ai-which-architecture-should-you-deploy) .


If you want to talk through how to measure your specific Vapi deployment,[book a call with the Coval team](https://cal.com/forms/b7126bad-a5c3-4ed0-af4b-fdfd4b268381) .
