---
schema_version: "1.0.0"
document_id: "4a0dd366c72c576c37f2de36f387fda39a30c66e774d033e828249c7ec9fd363"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/how-to-test-retell-ai-voice-agents"
published_at: "2026-07-05T00:00:00+00:00"
first_seen_at: "2026-07-24T00:07:13.352567+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:ce3ee1fa3db9b5488fc1cb68e72ea5f7966e0b347821e7b1a337caa8b12c7b8e"
---

# How to test Retell AI voice agents

A Retell agent that passes every simulation in the dashboard can still drop call transfers, sit in dead air while a tool executes, and mispronounce your product name on a live call. That's not a knock on[Retell](https://www.retellai.com/) — it ships more native testing than most voice platforms. It's the nature of the problem: the failure modes that hurt you live in audio, timing, and telephony, and Retell's automated simulation runs as text.


This guide is for engineers who run Retell agents in production or are about to. It covers the failure modes Retell builders actually report, what Retell's native testing stack covers and exactly where it stops, a simulation-first methodology that matches the failure modes, and how Roark's Retell integration fits into that workflow.


## Where Retell agents actually fail


The best predictor of what to test is what breaks for other people. Retell's community forum and Retell's own reliability documentation give an unusually honest catalog.


**Call transfers.** Transfer is a first-class failure class on Retell — enough so that Retell maintains a dedicated["Debug call transfer failure"](https://docs.retellai.com/reliability/call-performance) reliability doc. Community threads report the` transfer_call` tool[failing repeatably right after the agent invokes it](https://community.retellai.com/t/single-prompt-agent-transfer-call-not-working/3184) , and separately,[multi-second dead air between transfer initiation and the parties being bridged](https://community.retellai.com/t/delay-between-call-transferred-and-getting-connected/1039) . When a platform writes a named runbook for a failure class, that class is common enough to deserve a test on every release.


**Platform-side behavior drift.** One builder reported their[agent became "more eager to respond" and started interrupting callers](https://community.retellai.com/t/agent-is-more-eager-to-respond-since-june-28-29th/3233) with no configuration changes on their side. Model routing and platform updates shift behavior underneath you. The only way to catch a regression you didn't cause is a test suite that runs on a schedule — not just when you deploy.


**Tool-call timing.** A subtle one:[latency fillers playing after the custom tool finishes executing instead of during it](https://community.retellai.com/t/latency-status-fillers-executing-after-custom-tool-completion-instead-of-during-execution/3162) . The caller sits through silence while the tool runs, then hears "one moment while I look that up" after the answer is already available. The transcript of that call looks perfect. This entire failure class is invisible to text-based testing.


**Tool and webhook HTTP failures.** Builders hit[errors when Retell calls the webhook or API specified in a tool](https://community.retellai.com/t/error-when-retell-is-calling-the-webhook-or-any-api-specified-in-the-tool/926) mid-call, and integration forums document data-flow failures on both the n8n and Make.com sides. Your agent's behavior when a tool errors or times out is a scenario in its own right.


**The test surface diverging from live behavior.** A high-priority[community bug report](https://community.retellai.com/t/high-priority-bug-report-manual-test-interface-agent-transfer-transition-failure/890) documents the dashboard's manual test interface failing on agent-transfer transitions where live calls behaved differently. Passing in the dashboard is necessary, not sufficient.


**TTS pronunciation.** Custom pronunciation[compatibility issues with the ElevenLabs V2.5 engine](https://community.retellai.com/t/custom-pronunciation-with-elevenlabs-v2-5-engine/2835) and mispronunciation reports with Cartesia TTS come up on the forum. If your agent says street names, drug names, or SKUs, pronunciation is a regression surface.


**Off-script generation.**[G2 reviewers](https://www.g2.com/products/retell-ai/reviews?qs=pros-and-cons) note that agents driven by open-ended prompts frequently wander off-script, fabricate details a caller then relies on, or get trapped in logical loops — a failure mode Retell mitigates with structured Conversation Flow agents as an alternative to prose prompts. Either way, you need adversarial test cases that try to pull the agent off script.


**Latency.** Retell's own[latency troubleshooting doc](https://docs.retellai.com/reliability/troubleshoot-latency) targets end-to-end latency under P90 3 seconds and "estimated latency" under 1.5 seconds, decomposes it into LLM, ASR, TTS, and network components, and flags prompts beyond roughly 8,000 tokens and network RTT above 300ms as common causes. Those numbers are usable pass/fail thresholds for your own suite — more on that below.


## What Retell gives you natively — and where it stops


Credit where due: Retell's[native testing stack](https://docs.retellai.com/test/test-overview) is one of the more complete among voice platforms.


- **LLM Playground** — interactive text chat against your agent with function-call visualization, billed per message. The right tool for fast prompt iteration.
- **LLM Simulation Testing** — automated[test cases](https://docs.retellai.com/test/llm-simulation-testing) defined by a simulated-user prompt (the docs recommend Identity, Goal, and Personality sections), your own LLM-judged evaluation metrics (e.g. "Verify that the customer successfully returned the package and received a refund"), and a choice of model to play the user. Supports dynamic variables and mocks for custom functions. Runs execute as text conversations labeled "AI Simulated Chat."
- **Batch testing** — run a test suite from the dashboard's Simulation tab or via the[Create Batch Test API](https://docs.retellai.com/api-references/create-batch-test) (1–1,000 test case IDs per job, returning pass/fail/error counts and a job status). This is genuinely CI-wirable.
- **Web and phone call testing** — live voice validation via[web calls](https://docs.retellai.com/deploy/web-call) (no phone number needed) or real phone calls, at standard call rates.
- **A/B testing** — percentage-split[traffic routing](https://docs.retellai.com/deploy/ab-testing) across agents, with analytics filterable by agent version to compare success rate, duration, and latency.


Now the limits, all from Retell's own docs:


Testing surface Automatable in batch? Exercises real audio?


LLM Playground No — interactive only No — text


LLM Simulation Testing Yes — dashboard + API No — text only


Web / phone call testing No — manual Yes


Audio Simulation Listed "Coming Soon" in Retell's docs


- **Simulation is text-only.** Retell's test overview states plainly that Playground and Simulation Testing cannot test background-noise handling or interruption management — those require actual voice calls.
- **Voice calls have no batch automation.** The only surfaces that exercise real audio are the manual ones. You cannot schedule fifty phone calls against your agent from inside Retell today.
- **The batch-test API excludes custom-LLM agents.** Its` response_engine` accepts only` retell-llm` and` conversation-flow` . If you run your own LLM behind Retell's WebSocket protocol, the automated testing path doesn't apply to you — and your WebSocket server is precisely the component most in need of regression coverage.
- **Non-determinism is acknowledged but unmanaged.** The[batch-testing docs](https://docs.retellai.com/test/batch-test-simulation) recommend running tests multiple times because LLMs "can sometimes produce inconsistent or unexpected results," but there's no built-in repeat count or statistical pass criterion.


The pattern: Retell covers conversational logic well and leaves a gap exactly where audio, timing, and telephony live — which is where the failure catalog in the previous section lives.


## A simulation-first methodology


Four stages, cheapest first. Each stage catches what the previous one structurally cannot.


**1. Iterate on logic in text.** Use the Playground and text simulations for prompt and flow iteration. They're fast and cheap, and most logic bugs — wrong branch taken, missing slot confirmation, policy violations — show up here. Don't skip this because it's "only" text; it's the highest-velocity loop you have.


**2. Validate in audio with automated voice calls before launch.** Everything in the failure catalog that involves timing, interruptions, pronunciation, or transfers needs a real call over the same audio path your customers use. A test that doesn't ride the phone network can't catch a transfer that fails to bridge or dead air during a tool call.


**3. Freeze a regression suite and run it on a schedule.** Not just when you change something. The "more eager to respond" incident is the argument: the builder changed nothing, and behavior shifted anyway. A nightly or weekly run of the same scenarios against an unchanged agent is your drift detector.


**4. Score production continuously.** Simulations cover the scenarios you thought of. Live calls contain the ones you didn't. Every production call should be scored against the same criteria as your simulations, so a metric that regresses in production becomes next week's simulation scenario.


Derive your scenario list directly from the documented failure classes:


- A transfer scenario for every routing branch, asserting the handoff actually bridges.
- A tool-call scenario where the simulated caller stays silent during tool execution — the condition that exposes dead air and filler mistiming.
- An interruption-heavy persona that talks over the agent mid-sentence.
- A script of your domain vocabulary — product names, addresses, dosages — read back by TTS.
- An adversarial caller who pushes for information the agent shouldn't invent ("just give me a ballpark price"), asserting the agent doesn't fabricate.
- A tool-failure scenario: what does the agent say when the endpoint 500s or times out?


## Testing Retell agents with Roark


[Roark](https://roark.ai/) does simulation testing plus evals and observability for voice agents, and Retell is a first-class integration. It fills the specific gaps above: automated voice-level calls, a scheduled regression suite, and production scoring — connected to your existing Retell agents rather than a re-implementation of them.


### Connecting takes five steps


The[Retell integration](https://docs.roark.ai/documentation/integrations/retell) is an API-key flow: name the integration and paste your Retell API key, choose a historical sync range (default: the last 90 days of calls), select all or specific agents, choose the evaluators to run on synced calls, and activate. Roark configures webhooks with Retell automatically — new calls sync via Retell's` call_started` and` call_ended` webhook events — and a scheduled background sync keeps agents, phone numbers, LLM prompts, and conversation flows current. The historical import runs once, at setup.


platform.roark.ai/agents/new


Connect Retell


Paste your Retell API key — Roark syncs your agents and every new call.


Credentials


Connection name Production Retell


API key key_••••••••••••••••


Agents synced from Retell


support_agent


Synced


appointment_booking


Synced


lead_qualifier


Synced


Connected — historical calls imported, new calls sync automatically


### Phone simulations against your actual agent


Simulations place real phone calls: Roark leases a phone number and dials your Retell agent's number over the public phone network — the same path your customers take, so ASR under real network conditions, TTS output, endpointing, transfer bridging, and tool-call timing are all in scope. Test runs are composed from[personas](https://docs.roark.ai/documentation/simulation-testing/overview) (virtual customers with distinct characteristics, voices, and behaviors), scenarios (the conversation blueprint), run plans (persona × scenario × agent), and schedules — the schedules being the drift detector from stage three. Simulated callers speak 45 languages and accents.


Two details worth calling out for Retell specifically. First,[outbound agents are supported](https://docs.roark.ai/documentation/simulation-testing/inbound-vs-outbound) : if your Retell agent places calls rather than receiving them, Roark provisions a number and triggers your system with an HTTP POST containing a` {{phoneNumberToDial}}` template variable — Retell is one of the documented provider examples — and your agent dials the simulated caller. Second, because simulation happens over a phone call rather than through Retell's batch API, the custom-LLM exclusion doesn't apply: whatever sits behind the phone number, including a custom LLM on Retell's WebSocket protocol, gets exercised end to end.


### What to measure


Roark ships 64+ built-in metrics plus unlimited custom metrics you define. The audio-native models among them measure things text evaluation can't: pronunciation, accent clarity, emotion, vocal stress, pace and pauses, and interruptions. Mapped onto Retell's failure classes:


Retell failure class What to measure


Transfer failures / dead air at handoff Pace & pauses on transfer scenarios, plus a custom metric asserting the call reached a human.


Fillers firing after tool completion Pace & pauses — long silences during the tool-execution window.


Interruption drift after platform changes Interruption metrics, tracked across scheduled runs of an unchanged agent.


TTS mispronunciation Pronunciation and accent-clarity scoring on your domain-vocabulary script.


Off-script / fabricated answers A custom metric asserting the agent only quotes documented facts and prices.


Latency Response-delay measurements held against Retell's own targets: P90 under 3s end to end.


The same evaluators run on synced production calls, which closes the loop: a live call that scores badly is evidence, with the recording and transcript attached, and its scenario goes into the regression suite. For regulated deployments — a common situation for Retell agents in[healthcare](https://roark.ai/industries/healthcare) and[support](https://roark.ai/industries/customer-support) — Roark is SOC 2 Type II, offers a HIPAA BAA, and is penetration-tested annually, with ISO 27001 in progress.


## What Roark doesn't do


Setting expectations honestly, because that's cheaper than a disappointed evaluation:


- **Roark doesn't fix your agent.** It finds failures and gives you the evidence — recordings, transcripts, metric scores, trends. The fix is still you, editing the prompt or conversation flow in Retell's dashboard.
- **Voice simulations are real phone calls.** They're slower and cost more per run than Retell's text simulations. The right split: Retell's Playground and text simulations for high-velocity logic iteration, voice simulations for the audio-and-timing layer text can't reach. Don't abandon one for the other.
- **Roark doesn't split production traffic.** Retell's built-in A/B testing handles percentage rollouts across agents; Roark's role there is scoring the calls each variant produces.
- **Historical import is one-shot.** The backfill (default 90 days) runs once at setup; ongoing coverage comes from webhooks and the background sync, not repeated re-imports.


For anything specific, asksupport@roark.ai .


## FAQ


### Can I use Retell's batch-test API with a custom-LLM agent?


No. The Create Batch Test API accepts only the` retell-llm` and` conversation-flow` response engines. For custom-LLM agents, your automated options are tests you build against your own WebSocket server, or phone-level simulation from outside the platform — which has the advantage of exercising the WebSocket integration itself.


### Do I need a phone number to test a Retell agent?


Not for Retell's own surfaces: web calls via` retell-client-js-sdk` need no number, and text simulations need no audio at all. Roark's simulations against Retell agents run over the phone, so the agent must be reachable at a number — one purchased through Retell, or your own via Retell's custom telephony (SIP trunking with Twilio, Telnyx, or Vonage). Outbound agents are the exception: they dial a number Roark provisions, so they need no inbound number.


### How do new Retell calls get into Roark after I connect?


Roark configures webhooks with Retell automatically when you activate the integration, and new calls sync via the` call_started` and` call_ended` events. A scheduled background sync also keeps agents, phone numbers, prompts, and conversation flows up to date. One Retell-side detail worth knowing: per[Retell's webhook docs](https://docs.retellai.com/features/webhook) , calls that never connect (` dial_failed` ,` dial_no_answer` ) skip` call_started` but still fire` call_ended` — those are calls your customers experienced as a no-answer, so don't filter them out of monitoring.


### How do I catch regressions I didn't cause?


Run a frozen scenario suite on a schedule against an agent you haven't touched. If scores move, the platform moved. This is the only test design that would have caught the documented June 28–29 interruption-behavior drift, because there was no deploy to hang a CI run on.


### What latency should a Retell agent target?


Use Retell's own published thresholds: end-to-end latency under P90 3 seconds, estimated latency under 1.5 seconds. Their troubleshooting doc attributes most excursions to oversized prompts (beyond ~8,000 tokens), slow tool calls, and network RTT above 300ms. Encode the thresholds as pass/fail criteria in your suite rather than eyeballing dashboards.


### Is Roark suitable for HIPAA-regulated Retell deployments?


Roark is SOC 2 Type II certified, offers a HIPAA BAA, and undergoes annual penetration testing; ISO 27001 certification is in progress. If you're running patient-facing agents, start with[the healthcare page](https://roark.ai/industries/healthcare) or emailsupport@roark.ai .
