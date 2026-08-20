---
schema_version: "1.0.0"
document_id: "b76b7be75e5c561433791d945ca7e48d330ec38255a4ec343c0a86ae1f5a290f"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/how-to-test-vapi-voice-agents"
published_at: "2026-07-05T00:00:00+00:00"
first_seen_at: "2026-07-22T12:05:46.538010+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:c5531220f2a726959cda6d9778477868bc65d5334d460e87565395023124318f"
---

# How to test Vapi voice agents

A Vapi agent that passes every scripted test can still fail its first real call. That's not a knock on Vapi — it's the nature of the stack. A voice agent is a pipeline of transcriber, model, voice, endpointing, and telephony, and a text transcript only shows you one slice of it.


This guide is written for engineers shipping Vapi agents to production. It covers the failure modes that actually show up in the wild (sourced from Vapi's own docs and community forum), what Vapi's built-in testing tools — Test Suites, Evals, and Simulations — cover and where they stop, a testing methodology that works, and how[Roark](https://roark.ai/) connects to Vapi if you want simulation and production evals on the full audio path.


## Why Vapi agents break in production


Vapi's own[debugging docs](https://docs.vapi.ai/debugging) bucket failures into three categories: Speech & Language (transcription accuracy, intent recognition), Tool & Workflow (tool execution errors, variable extraction, node routing), and System-Level (call drops, audio problems, integration failures). What's notable is how few of the production incidents you'll actually hit are prompt problems. Most live in the plumbing.


### The custom tool contract is stricter than it looks


Vapi's[custom tools troubleshooting guide](https://docs.vapi.ai/tools/custom-tools-troubleshooting) documents a contract that trips up a lot of teams:


- Your tool server must **always return HTTP 200** , even for errors — other status codes are ignored, and the agent behaves as if the tool never answered.
- The response must be a` results` array whose` toolCallId` exactly matches the request. A mismatch means the result silently doesn't land.
- ` result` and` error` must be **single-line strings** — line breaks cause parsing errors.
- The default` maxTokens` of **100** truncates complex tool responses (Vapi recommends 500).


Every one of these fails quietly. The agent doesn't crash — it hallucinates around the missing data or stalls. If your tests only assert on the happy path with a mocked backend, you will not see any of it.


### Endpointing failures sound like a dead agent


One documented community case: an assistant **stopped speaking after successful tool calls** , with no configuration change by the user. The fix Vapi support recommended was switching from transcription-based endpointing to` smartEndpointingPlan` with the LiveKit provider and its Aggressive Function setting. Two details worth knowing before you hit this yourself: the` livekit` smart-endpointing provider is **English-only** (non-English agents use the` vapi` provider), and when both are configured,` smartEndpointingPlan` takes precedence over transcription-based endpointing. To a caller, all of this is indistinguishable from a dead line — and none of it is visible in a text-only test.


### Latency is real, and the dashboard understates it


Two community threads make the same point from different angles. In one, the Vapi dashboard reported **950ms** latency while callers experienced **2–3 second** delays — roughly 2–3x the reported figure. In another, a team saw ~5 seconds per turn; Vapi's diagnosis was that Groq was over capacity, with LLM latency averaging **1,862ms of a 5,023ms** total turn — and because the agent spoke Polish, the LiveKit endpointing optimization wasn't available to claw any of it back. The recommendation was to switch LLM providers.


> The lesson: component latency measured inside the platform is not the latency your caller experiences. The only number that matters is measured from the caller's side of the call — which means testing over a real call, and monitoring production audio, not just logs.


### Webhooks that never arrive


If you build billing, concurrency tracking, or CRM sync on Vapi's[server messages](https://docs.vapi.ai/server-url/events) , two documented behaviors matter. First,` end-of-call-report` and` status-update` webhooks are intentionally **not sent for calls that never connected** at the telephony level (e.g.` customer-did-not-answer` ) — one team reported this breaking their concurrency tracking, and Vapi acknowledged the gap. Second, a thread about webhooks only firing for completed calls was resolved by explicitly enabling` end-of-call-report` and` status-update` in the assistant's` serverMessages` configuration — the events you don't opt into simply don't arrive.


And sometimes the platform itself has a bad week: starting June 20, 2025, a Vapi incident caused webhook tool calls in *Workflows* to silently fail — the agent idled at the tool node with no request sent — while identical tools in Assistants kept working. Web calls were affected; phone calls were fine. It was resolved within days, but it's a class of failure no pre-launch test catches. Only production monitoring notices that tool-call success rates fell off a cliff on Tuesday.


Finally, learn the[endedReason taxonomy](https://docs.vapi.ai/calls/call-ended-reason) . Values like` silence-timed-out` ,` exceeded-max-duration` ,` pipeline-error-*-llm-failed` , and` call.in-progress.error-vapifault-worker-died` are the first place a production problem shows up in aggregate. Alert on distribution shifts, not individual calls.


## What Vapi's built-in testing gives you (and where it stops)


Vapi has shipped three generations of native testing. All three are worth understanding, because the newest two are genuinely useful — and because most third-party guides still describe the deprecated one.


### Test Suites (deprecated)


[Test Suites](https://docs.vapi.ai/test/test-suites) launched on February 20, 2025 and now carry a deprecation notice: "Test Suites is being deprecated. It will be replaced by Simulations, a more powerful way to test your voice agents." The mechanics: up to **50 test cases** per suite, 1–5 attempts per case.[Voice tests](https://docs.vapi.ai/test/voice-testing) connect two AI agents — yours and Vapi's testing agent — on a real phone call, capped at **15 minutes** , with an LLM scoring the transcript against your rubric; per the docs, test calls "cost you the same as regular calls". Chat tests are text-only and "much faster than voice testing" — which also means they can't test speech recognition, voice quality, endpointing, or audio latency. If you're starting today, don't build on Test Suites.


### Evals


[Evals](https://vapi.ai/blog/evals) (announced December 3, 2025) are Vapi's unit-testing layer — tests that, in Vapi's words, "run the same way your unit tests do": JSON mock conversations where each assistant turn can carry a` judgePlan` — exact match, regex, or an AI judge — plus tool-call validation on function names and arguments (exact or partial). They support batch runs, a CLI, and CI/CD integration, and you can convert production calls into evals from the dashboard. They run[entirely as text](https://docs.vapi.ai/observability/evals-quickstart) — a successful run ends with` endedReason: mockConversation.done` , no audio involved. That makes them fast and cheap, and it means they cannot catch any STT, TTS, endpointing, or latency failure by design. Use them for what they are: turn-level regression checks on model behavior and tool-call correctness.


### Simulations


[Simulations](https://docs.vapi.ai/observability/simulations-advanced) are the current-generation system: Scenarios (instructions + evaluation criteria) bundled into Simulation Suites, executed as Simulation Runs, with persona-based testing ("decisive, confused, impatient, detail-oriented, non-native speakers"), tool mocks for deterministic error-path testing, and reusable Structured Outputs as evaluation schemas. There are two transports: chat mode (` vapi.webchat` , text-only, faster and cheaper) and voice mode (` vapi.websocket` , real audio through actual STT/TTS). Hooks fire in voice mode only, and the docs' own troubleshooting entry is telling: "No audio in recording? Using chat mode. Switch to vapi.websocket transport." The docs also describe GitHub Actions integration — PR-triggered smoke tests, pre-deploy regression suites, nightly runs, pass-rate gates (e.g. 95%) — and a sensible taxonomy of smoke, regression (named "Regression: \[Issue #\]"), and edge-case tests.


Tool Audio path tested? Best for


Test Suites Voice tests: yes (15-min cap, billed as real calls). Chat tests: no. Nothing new — deprecated in favor of Simulations.


Evals No — text-only mock conversations. Fast per-turn assertions on model output and tool calls in CI.


Simulations Voice mode: yes (WebSocket). Chat mode: no. Scenario/persona coverage with pass-rate gates in CI.


Where the native tooling stops, fairly stated: everything is judged on Vapi's own view of the call — the transcript produced by the same pipeline under test. Voice-mode runs go over Vapi's WebSocket transport rather than the public phone network, so the telephony leg your real callers traverse isn't in the loop. And the scoring is transcript-rubric scoring: it can tell you the agent said the wrong thing, but not that it mispronounced a drug name, left four seconds of dead air, or talked over the caller. If your agent answers real phone calls, you want at least part of your test coverage to arrive the way your callers do.


## A methodology that catches these failures


The pattern that works is the same one that works for conventional software, adapted for audio: **simulate before launch, evaluate everything after.**


### Before launch: simulation coverage that matches your traffic


- **Scenarios from real intents.** Enumerate the ten things callers actually do — book, cancel, reschedule, ask for a price, escalate — and write each as a scenario with explicit pass criteria. Vagueness in the success criterion is where flaky tests come from.
- **Personas, not just scripts.** The same scenario should run as the impatient caller who interrupts, the confused caller who answers a different question than was asked, and the rambling caller who buries the intent in a story. Vapi's own persona list — decisive, confused, impatient, detail-oriented, non-native speakers — is a good starting taxonomy.
- **Accents and languages.** If your callers aren't all native English speakers with studio microphones, your test callers shouldn't be either. This is where transcription-layer failures hide.
- **Background noise.** Callers phone from cars, kitchens, and coffee shops. An agent that only passes in silence hasn't been tested.
- **Error paths.** What does the agent say when a tool times out or returns an error? Vapi's tool mocks are genuinely useful here; so is deliberately breaking your staging backend.
- **A real phone leg for at least a subset.** Endpointing behavior, perceived latency, and telephony-level failures only exist on a real call. Run your core scenarios over PSTN before every significant prompt, model, or voice change.


And borrow the regression discipline from Vapi's Simulations docs: every production failure becomes a named regression scenario. The same bug should never reach production twice.


### After launch: score every call, alert on drift


Pre-launch coverage is finite; production traffic isn't. The post-launch half of the methodology is evals on production calls: score every call against the metrics you care about, watch the` endedReason` distribution for shifts, track latency as callers experience it rather than as the dashboard reports it, and feed anything that fails back into the simulation suite. The June 2025 Workflows incident is the argument in one line: a platform regression that your own deploys didn't cause, that no pre-launch test could have caught, and that only shows up as a production metric falling.


## How Roark connects to Vapi


[Roark](https://roark.ai/) is a simulation-testing and evals/observability platform that plugs into Vapi directly. The[integration](https://docs.roark.ai/documentation/integrations/vapi) is a five-step, API-key flow — no code changes to your agent:


1. In Roark, go to **Settings → Integrations** and select Vapi.
2. Enter a name and your Vapi API key — it's validated automatically and your assistants are fetched.
3. Choose a historical import window (default: last 90 days; custom range or skip).
4. Select all assistants or specific ones.
5. Choose the evaluators to run on synced calls, then create the integration.


platform.roark.ai/agents/new


Connect Vapi


Paste your Vapi API key — Roark syncs your agents and every new call.


Credentials


Connection name Production Vapi


API key vapi_sk_••••••••••••••••


Agents synced from Vapi


support_v2


Synced


booking_flow


Synced


after_hours_ivr


Synced


Connected — historical calls imported, new calls sync automatically


From there Roark syncs your assistant configurations, phone numbers, system prompts, transcripts with speaker labels, and tool/function invocations. The historical import runs once at setup; after that, new calls sync automatically. Your synced assistants also show up as dialable targets in Roark's simulation setup.


The simulation side is the part that closes the gap in Vapi's native tooling: **Roark places real phone calls.** For each simulation it leases a real phone number and dials your Vapi assistant's number over PSTN — so the call traverses the same telephony, transcription, endpointing, and TTS path your customers hit, and the recording Roark scores is the audio a caller would actually hear. Test callers are built from[personas](https://docs.roark.ai/documentation/simulation-testing/overview) — distinct voices, languages and accents (45 supported), speech pace, emotional register, and background-noise environments like driving, a coffee shop, or an office — crossed with scenarios into run plans, with schedules for recurring automated runs.


Outbound agents are covered too. If your Vapi agent *makes* calls rather than answering them, Roark[provisions a number for your agent to dial](https://docs.roark.ai/documentation/simulation-testing/inbound-vs-outbound) and auto-triggers your outbound flow via an HTTP POST using a` {{phoneNumberToDial}}` template variable — Vapi is one of the documented provider examples.


On the production side, Roark scores every live call with the same evaluators, files what breaks as issues, and exposes dashboards and OTEL traces — so the simulate-before, evaluate-after loop runs on one platform. If you run voice agents in a regulated vertical, there are dedicated overviews for[healthcare](https://roark.ai/industries/healthcare) and[customer support](https://roark.ai/industries/customer-support) teams.


### What to measure


The metrics that matter for Vapi agents are the ones a transcript can't carry. Roark ships **64+ built-in metrics** plus unlimited custom ones, and the audio-native set maps directly onto the failure modes above:


- **Dead air and response time** — catches the silent-agent endpointing failure and the caller-side latency the dashboard understates.
- **Interruptions** — is the agent talking over callers, or getting steamrolled by barge-in?
- **Pronunciation and accent clarity** — the agent can say the right words badly; only the audio shows it.
- **Emotion, vocal stress, and pace & pauses** — measured on the recording, these surface calls going wrong even when the transcript reads clean.


## FAQ


### Should I use Roark or Vapi's built-in Evals and Simulations?


Both, for different layers. Vapi's Evals are fast text-level unit tests — great for asserting per-turn model output and tool-call arguments on every PR. Vapi's Simulations add scenario and persona coverage inside the platform. Roark adds the layers Vapi can't see from inside its own pipeline: real PSTN calls through the full telephony path, audio-native scoring (dead air, interruptions, pronunciation), cross-platform coverage if you run more than one voice stack, and production evals on every live call.


### Do Roark's simulation calls to Vapi use real phone calls?


Yes. Roark leases real phone numbers and dials your assistant's number over the public phone network. That's deliberate — endpointing, perceived latency, and telephony failures don't exist on a text or WebSocket transport, so testing over PSTN is the only way to cover them.


### My Vapi agent makes outbound calls. Can I still test it?


Yes. Roark provisions a phone number for your agent to call and triggers your outbound flow automatically via an HTTP POST with a` {{phoneNumberToDial}}` template variable. Vapi is one of the documented provider examples for this trigger.


### Is Roark suitable for regulated industries?


Roark is SOC 2 Type II certified, offers a HIPAA BAA, and undergoes annual penetration testing; ISO 27001 certification is in progress. If you're running Vapi agents in[healthcare](https://roark.ai/industries/healthcare) or another regulated space and have specific compliance questions, contactsupport@roark.ai .
