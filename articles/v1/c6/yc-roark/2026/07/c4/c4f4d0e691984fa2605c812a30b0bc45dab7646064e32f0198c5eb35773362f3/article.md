---
schema_version: "1.0.0"
document_id: "c4f4d0e691984fa2605c812a30b0bc45dab7646064e32f0198c5eb35773362f3"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/voice-agent-ai-disclosure-regression-testing"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-24T00:07:13.352567+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:32f8fcd2fd91043b4fe38b22eefae54d61945e78c640d7d61bb59c7129ed0b4c"
---

# Your voice agent's AI disclosure is a regression test, not a prompt line

When[Pie stepped out of stealth with a $19.5M Series A on June 30](https://www.techtimes.com/articles/319494/20260701/ai-receptionist-startup-pie-gets-195m-state-laws-may-require-bot-disclosure.htm) and launched Front Desk, its AI receptionist for nail salons, auto shops, and fitness studios, the coverage came with a subplot most voice-AI teams have been quietly worrying about for months. State laws in California, Texas, Utah, and a growing list of others now[require the AI to identify itself to callers](https://www.techtimes.com/articles/319494/20260701/ai-receptionist-startup-pie-gets-195m-state-laws-may-require-bot-disclosure.htm) , and the small business owner, not the vendor, is usually the one on the hook when it doesn't.


If you're building the agent, that has a practical consequence. "Put the disclosure in the system prompt" is not a plan. Disclosure is a behavior that has to fire correctly on a specific set of triggers, in a specific window, in the right language, even when the caller barges over the greeting or the model swaps mid-quarter. That is not a prompt-engineering problem. It is a regression-testing problem.


## The disclosure matrix is now a real matrix


A US voice agent operating across state lines has to satisfy at least four different flavors of disclosure rule at once, and the flavors don't overlap cleanly.


- **California** :[AB 2905 has required AI phone hosts to disclose their AI identity since January 1, 2025](https://www.techtimes.com/articles/319494/20260701/ai-receptionist-startup-pie-gets-195m-state-laws-may-require-bot-disclosure.htm) , and California's earlier BOTS Act (SB 1001)[makes it unlawful to use a bot to mislead a person about its artificial identity](https://ainora.lt/blog/ai-caller-disclosure-laws-by-country-2026) in commercial or political interactions. The newer SB 243 companion-chatbot law[carries a private right of action at $1,000 per violation plus attorney fees](https://thelyonfirm.com/blog/undisclosed-chatbot-consumer-rights-ai-disclosure-law/) .
- **Texas** :[SB 140 requires AI voice technology to be disclosed within the first 30 seconds of a call](https://www.retellai.com/blog/tcpa-compliance-playbook-voice-ai-outbound) , and the Responsible AI Governance Act (HB 149)[took effect January 1, 2026](https://stackcyber.com/posts/ai-state-laws) .
- **Utah** : the amended AI Policy Act only requires disclosure[when a consumer asks directly or during "high-risk" interactions](https://www.cooley.com/news/insight/2025/2025-10-21-ai-chatbots-at-the-crossroads-navigating-new-laws-and-compliance-risks) , with proactive disclosure required at the start of regulated-occupation interactions like healthcare, law, and finance.
- **Washington** : the March 2026 companion-chatbot law[requires recurring disclosure notifications throughout an interaction](https://thelyonfirm.com/blog/undisclosed-chatbot-consumer-rights-ai-disclosure-law/) , with hourly disclosures when minors are involved.
- **New York** :[S-3008C requires disclosure at the start of each interaction and at least once every three hours during ongoing conversations](https://fpf.org/blog/understanding-the-new-wave-of-chatbot-legislation-california-sb-243-and-beyond/) .
- **Colorado** : the CAIA is[set to take effect June 30, 2026](https://www.cooley.com/news/insight/2025/2025-10-21-ai-chatbots-at-the-crossroads-navigating-new-laws-and-compliance-risks) after being pushed back from February, and its disclosure obligations apply regardless of whether the deployment is "high risk."


That is six different triggers, three different windows, and one private right of action, all pointing at the same audio stream. The[FCC has an NPRM pending that would likely make in-call AI disclosure federally mandatory within 12 to 24 months](https://www.retellai.com/blog/tcpa-compliance-playbook-voice-ai-outbound) , which will simplify the top of the matrix but not the bottom. State-specific triggers, especially direct-ask handling and regulated-occupation flows, aren't going anywhere.


## Why prompt-only disclosure breaks in production


If you've shipped a voice agent, you know the shape of this problem already. The disclosure line lives in the system prompt. It works on the happy path. And then it doesn't.


Four failure modes we see over and over:


**Prompt drift on hot paths.** The greeting says "This is an AI assistant for Acme." A new engineer adds a voicemail-detection branch, and on the voicemail path the agent goes straight into leaving a message with no disclosure. Or the human-transfer flow was rewritten to be terser and the disclosure got trimmed. The main-path disclosure still fires, so a manual smoke test looks fine.


**Model swaps.** You move from one STT provider to another to shave 80ms of latency, or you upgrade the LLM version. Turn-taking behavior shifts subtly, and now the caller's "hello" arrives during your disclosure phrase. Endpointing cuts the response short. The transcript still contains the disclosure, but the caller never heard the "AI" part. On[Pipecat's frame-based pipeline where interruption cancels pending tasks when the user starts speaking](https://www.arunbaby.com/ai-agents/0018-voice-agent-frameworks/) , this is very easy to trigger without noticing.


**Barge-over on the direct-ask handler.** Caller says "wait, am I talking to a real person?" three seconds into the call, mid-turn. Your intent classifier catches it. Your disclosure response fires. The caller, agitated, talks over the response and the model yields. Did the caller actually hear "I'm an AI"? In Utah, that's the entire ballgame, because[the disclosure obligation attaches specifically to a clear and unambiguous consumer request](https://www.wiley.law/alert-AI-Chatbots-How-to-Address-Five-Key-Legal-Risks) .


**Non-English flows.** Your Spanish greeting is a separate prompt written by a different engineer. It has the company name but not the AI disclosure. Nobody notices because your QA team runs their samples in English.


None of these show up in transcript-level review. They show up when you listen to the audio, from the caller's perspective, on the states and triggers you actually operate in.


Where disclosure logic breaks between the caller and the transcript


## What actually needs to be tested


Treat disclosure as a set of behaviors, each with its own trigger and its own pass condition. At minimum:


1. **Opening-of-call disclosure fires within the required window.** Texas is 30 seconds. Some state variants read as "at the start." Measure the timestamp of the AI-identification phrase against call start, on the caller-audible audio, not on the log line.
2. **Direct-ask handler responds correctly, mid-conversation.** "Am I speaking to a person?" "Is this a real human?" "Are you a bot?" "Am I talking to AI?" The response has to affirmatively identify as AI, and the caller has to be able to hear it uninterrupted long enough for the affirmation to land.
3. **Barge-over-disclosure recovery.** When the caller talks over the disclosure phrase, does the agent repeat it? Does it detect that the disclosure was interrupted before the "AI" token was rendered? Nothing in most stacks tracks this by default.
4. **Non-happy-path disclosure.** Voicemail detection, human-transfer, callback scheduling, hot-transfer to a live rep, and after-hours flows each need their own coverage.
5. **Locale-conditioned disclosure.** If the agent runs in Spanish, French, or any of the[25+ languages Grok Voice ships with](https://x.ai/news/grok-voice-agent-builder) , each locale needs its own disclosure phrase and its own tests.
6. **Regulated-occupation branch.** If the call is about a healthcare, legal, or financial matter and the caller is or may be in Utah,[the agent must proactively disclose at the beginning of the interaction](https://thelyonfirm.com/blog/undisclosed-chatbot-consumer-rights-ai-disclosure-law/) , not on request. That is a routing test.
7. **Recurring-disclosure timers.** For flows in Washington or New York, or any product marketed to minors,[recurring disclosures at fixed intervals are the compliance floor](https://thelyonfirm.com/blog/undisclosed-chatbot-consumer-rights-ai-disclosure-law/) . Test that a 3-hour conversation still fires the second and third disclosure, not just the first.


Each of these is a test case, not a prompt review. And each has to be re-run on every prompt change, model change, and provider change, because any of them can silently regress the others.


Prompt-only disclosure vs. simulation-tested disclosure


## Real calls, real audio, before it ships


The reason this can't be solved with transcript-level evals is that the caller doesn't experience a transcript. They experience audio, at some latency, with some emotion in the voice, over a PSTN codec that clips sibilants and drops packets. The disclosure either lands, uninterrupted, at a moment the caller can perceive, or it doesn't.


That is what simulation testing is for.[Roark](https://roark.ai/) dials your agent over real PSTN and WebRTC with configurable personas, so you can build a disclosure suite that runs the same seven scenarios above from a California persona, a Utah persona in a regulated-occupation flow, a Spanish-speaking persona from Texas, and a Washington persona whose call runs past the recurring-disclosure interval. Every call is scored against audio-native metrics (pronunciation, pace, interruption handling) plus whatever custom metrics you define for the disclosure check itself. Failures become replayable regression tests, so the next time someone tweaks the voicemail branch and drops the disclosure, the[CI-triggered simulation run over HTTP](https://docs.roark.ai/) catches it before merge, not after a state AG letter.


Illustrative disclosure suite run before merge


## Wiring it into CI


The move that actually changes team behavior is making the disclosure suite a required check on the same PRs that change prompts, tools, or model configuration. A minimal setup that works with any of the major agent frameworks (Vapi, Retell, LiveKit, Pipecat, Bland, ElevenLabs) looks like:


```text
# .github/workflows/voice-agent-ci.yml
on:
pull_request:
paths:
- "prompts/**"
- "agent-config/**"
- "tools/**"


jobs:
disclosure-suite:
runs-on: ubuntu-latest
steps:
- name: Deploy candidate agent to staging
run: ./scripts/deploy-staging.sh


- name: Trigger Roark disclosure suite
run: |
curl -X POST https://api.roark.ai/v1/simulations/runs \
-H "Authorization: Bearer $ROARK_API_KEY" \
-d '{
"suite": "disclosure_v3",
"target": { "phoneNumber": "'"$STAGING_NUMBER"'" },
"waitForCompletion": true,
"failOn": { "anyMetricBelow": 0.9 }
}'
```


The suite name matters less than the discipline of gating merges on it. The moment a disclosure regression is a red check on the PR instead of a Slack thread post-launch, teams stop shipping the regression.


For agents in regulated verticals, the same simulation harness is what you use to prove disclosure coverage to auditors after the fact. Every scored call is an audio recording plus a metric result plus a filed issue on any failure. That is the evidence chain the[Colorado AG will expect once the CAIA takes effect at the end of this month](https://www.cooley.com/news/insight/2025/2025-10-21-ai-chatbots-at-the-crossroads-navigating-new-laws-and-compliance-risks) , and it's the same chain that will keep small-business operators using products like Front Desk out of court.


## The point


Voice AI disclosure has crossed the line from "policy question" to "engineering behavior." It has triggers, windows, locales, and edge cases, and it can be broken by any change to the prompt, the model, the STT provider, the TTS voice, or the turn-taking logic. If your team is treating it as one line in a system prompt, you already have regressions in production that you have not seen yet, because they don't show up in the transcript.


The path forward is the same path that made the rest of software work: define the behavior as a testable spec, run it as a suite on real calls, gate merges on it, and store the recordings so you can prove it later. The state laws are going to keep multiplying. The disclosure suite is what stays constant.
