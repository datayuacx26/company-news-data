---
schema_version: "1.0.0"
document_id: "69df84d6dd8eb2f261423bb4a3e36233f1870329b7fadc73f13376443161a8f8"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/vendor-simulations-arent-your-acceptance-test"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T17:46:33.195958+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:d2985749bb931704fd201db9a7fb39950cbe8d1ced5fb3f8378bfabd0d553a2f"
---

# Vendor-run simulations aren't your acceptance test

On July 22, OpenAI launched[Presence](https://openai.com/index/introducing-openai-presence/) , an enterprise platform for deploying voice and chat agents. The pitch, in OpenAI's own words, is a "battle-tested" product that bundles policies, guardrails, approved actions, simulations, evaluation tools, and a Codex-driven improvement loop into one managed offering. It ships with[BBVA, SoftBank, and Insurance Australia Group named as design partners](https://www.reworked.co/digital-workplace/openai-presence-pitches-trusted-ai-agents-to-enterprises-a-day-after-owning-the-hugging-face-hack/) , and it is delivered through OpenAI Forward Deployed Engineers rather than as a self-serve product.


That is a serious offering, and the shape of it matters: the era where "we bought a voice agent" meant "we bought a model API" is closing. What enterprises are buying now is a governed operational layer. But there is a quiet fact buried in the launch coverage that every PM about to sign a voice AI contract needs to notice.[Every named Presence customer is still in testing.](https://www.reworked.co/digital-workplace/openai-presence-pitches-trusted-ai-agents-to-enterprises-a-day-after-owning-the-hugging-face-hack/) The only production deployment OpenAI points to is its own English-language phone support line, where it claims[75% self-resolution](https://www.reworked.co/digital-workplace/openai-presence-pitches-trusted-ai-agents-to-enterprises-a-day-after-owning-the-hugging-face-hack/) . Those numbers are the vendor's numbers, measured on the vendor's workflows, in the vendor's language, against the vendor's own definition of a resolved call.


Which means the buyer still owes themselves an acceptance test. This is not a knock on Presence, and it applies equally to every managed voice AI platform you might evaluate. Vendor-bundled simulation is real value, but it is not, and cannot be, the test that decides whether an agent is ready to talk to *your* customers.


## What vendor-bundled testing actually does


Give Presence its due. Reading OpenAI's own[help documentation](https://help.openai.com/en/articles/20001405) , the testing controls include simulations and evaluations that exercise common workflows and edge cases before release, session records and quality signals to review after launch, human escalation paths with structured context, and controlled rollout with monitoring and rollback. That is a defensible pre-launch harness, and it is exactly the kind of thing that most in-house voice AI programs are still trying to build themselves.


Independent coverage confirms the shape.[Simulations and graders in Presence check whether the agent reached the intended outcome, followed policy, used tools correctly, and escalated when required](https://www.helpnetsecurity.com/2026/07/22/openai-presence-ai-agent-platform/) . The launch also introduces a formal split between routine, edge-case, and higher-risk scenarios, with Codex proposing changes based on production evidence that a human must approve before rollout.


You should absolutely lean on that. But now walk into your CFO's office and defend the contract. The questions you will get, and the questions the regulator will get if something goes wrong, are not about whether the vendor's graders passed. They are about whether you tested the agent against your actual callers, in your actual conditions, against your actual policies. Vendor-bundled simulation cannot answer those, structurally, for four reasons.


## Why vendor testing stops at your door


**1. Vendor personas are the vendor's, not yours.** Simulations only find failures the simulated caller creates. A generic support-agent test suite will not include the specific accents in your customer base, the emotional register of a policyholder who just totalled their car, the background noise of a job-site caller on a Bluetooth headset, or the code-switching between English and Spanish that shows up on every third call in some regions. If your callers speak with the range of accents that a national deployment actually sees, and the vendor's persona library was tuned on a different demographic, the pass rate is misleading.


**2. Vendor graders score what they can see.** Most vendor-side evaluation is transcript-first: did the agent say the right thing, call the right tool, follow the right branch. That is necessary, but voice agents fail on the sound of the call, not just its text. Pronunciation of a drug name or an address. Dead air after the caller finishes a question. Talking over the caller during an angry pause. A pace that is technically correct but wildly wrong for the emotional register of the call. These are the failure modes that show up in Trustpilot reviews and don't show up in transcript graders.


**3. Vendor rollouts are gated on vendor thresholds.** OpenAI's[own Presence page](https://openai.com/index/introducing-openai-presence/) is clear that companies set what remains consistent across deployments, but the default thresholds inside the graders, the specific edge cases the vendor thought to include, and the exact scoring rubric are the vendor's opinions. Your compliance officer's definition of an acceptable AI disclosure is not the same as OpenAI's. Your legal team's tolerance for an incorrect refund is not the same as a design partner's.


**4. Vendor pilots are not your pilot.**[BBVA is testing voice support for Mexico customers, SoftBank is trialing Japanese-language service, and IAG is piloting agents that support customers during severe weather events](https://www.reworked.co/digital-workplace/openai-presence-pitches-trusted-ai-agents-to-enterprises-a-day-after-owning-the-hugging-face-hack/) . None of those tell you how the agent will perform against home-services customers in the American South, or Medicare-eligible patients scheduling procedures, or logistics dispatchers in noisy warehouses. "Battle-tested" is domain-specific, and the domain isn't yours yet.


Vendor-bundled testing vs. buyer-run acceptance testing


## The five things your acceptance test has to cover


The acceptance test is not adversarial to the vendor. It is the thing that lets you sign the contract with confidence, defend the launch to your risk committee, and separate "the vendor missed something" from "we skipped a check." Whatever platform you're evaluating, these are the five layers a serious buyer runs on their own.


### 1. Personas that reflect your caller mix


Not "a professional English speaker" and "an angry customer." Build the persona set from your last 30 days of call recordings: the accents you actually get, the age skew, the emotional register that shows up when the caller reaches your line (calm, confused, angry, medical distress, in-vehicle), and the background environments (car, warehouse, hospital, home). This is where voice differs sharply from chat:[half the failure modes are audio conditions, not language logic](https://openai.com/index/introducing-openai-presence/) , and a persona catalogue that doesn't cover them is a test that doesn't apply to production.


### 2. Real telephony, not a text loopback


If the acceptance test doesn't actually dial the agent's PSTN or SIP number and evaluate what the caller hears, you are testing the wrong artifact. Codec compression, jitter, packet loss, and the specific carrier path in the regions you serve all change how a "passing" transcript sounds when a customer picks up. Vendor demos are usually on WebRTC. Your customers are usually on cellular. Any acceptance test that skips the phone network is theater.


### 3. Audio-native scoring, not just transcript checks


This is the layer most in-house harnesses skip because it is the hardest to build. Score every scenario on pronunciation accuracy of the terms your business actually uses (drug names, address components, product SKUs), interruption count, dead-air duration, pace stability under emotional load, and vocal-stress markers on the caller side. Transcript graders will pass calls that customers hate. Audio-native scoring catches the difference.


### 4. A regression suite built from your own calls


Sample-based QA doesn't work for voice AI, and neither does sample-based acceptance testing. Every real call that failed a business check, whether during the pilot or after go-live, needs to become a repeatable test. Replay it against every proposed agent update, whether that update is[proposed by Codex](https://www.helpnetsecurity.com/2026/07/22/openai-presence-ai-agent-platform/) , by a prompt engineer, or by a new base model the vendor rolled under you. If a change breaks a call you've already seen fail, you want to know before the second one happens, not after.


### 5. Cross-cutting checks the vendor doesn't own


Some of what you need to verify sits outside the vendor's remit entirely. TCPA disclosure timing. State-specific recording notices where the[FCC's proposed rules on in-call AI disclosure remain unfinalized but enforcement is active](https://callsphere.ai/blog/vw2d-fcc-ai-call-disclosure-rules-2026) . HIPAA handling where you have a BAA in place. Escalation precision measured against your own definition of an escalation-worthy call, not the vendor's. Cross-vendor comparability if you're evaluating Presence against a stack built on[Vapi](https://vapi.ai/) ,[Retell](https://retellai.com/) ,[LiveKit](https://livekit.io/) , or[Pipecat](https://www.pipecat.ai/) . The acceptance test has to be *portable* enough to compare candidates, which vendor-bundled testing by definition is not.


Illustrative buyer-side acceptance run across the five layers


## Buy or build the harness (be honest about which)


You can build all of this. A serious voice-AI acceptance harness is roughly six months of engineering plus an ongoing team: personas, telephony orchestration, audio-native scoring models, a scenario editor non-engineers can use, a way to convert production failures into regression tests, and dashboards that both your QA lead and your compliance officer can read. If you're a five-thousand-seat contact center, that math might work.


For everyone else, this is where[Roark](https://roark.ai/) fits. Roark's simulation testing dials your agent over real telephony, using personas built from real voices, languages, and accents, with configurable pace, emotional register, and background noise. It scores each call with 64+ built-in metrics plus unlimited custom ones, on audio-native models that read pronunciation, interruptions, and pace, not just the transcript. Every production call is scored automatically, and failures get filed as issues you can replay against the next version. It ships one-click integrations with the major builder platforms ([Vapi, Retell, LiveKit, Pipecat, Bland, and ElevenLabs](https://docs.roark.ai/) ), so the same suite works whether your agent runs on Presence, on a custom stack, or on a mix.


The point is not that Roark is the only way. The point is that acceptance testing for voice AI is now a distinct discipline, and treating it as something the model vendor throws in for free is how enterprises end up with pass rates on paper and complaints on Trustpilot.


## Sign the contract with your test suite already green


Vendors are getting genuinely better at governed deployments. Presence is a real product, and the shape of what it bundles reflects a healthy industry-wide move from demo-driven procurement to controlled rollout. That should make buyers braver, not lazier.


The right sequence, whether you're evaluating Presence, another managed platform, or a self-built stack, is: define your persona set from your own callers, stand up an acceptance harness that runs over real phones with audio-native scoring, seed it with regression tests from your own production data, and only then ask the vendor to hit it. Vendor-side simulations are useful evidence. They are not the verdict. The verdict is your suite, running on your calls, hitting your thresholds, before the first real customer picks up.
