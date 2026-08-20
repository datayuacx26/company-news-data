---
schema_version: "1.0.0"
document_id: "390a41c77e22fa6f18b5bc483c783913ed99022774ab877140514f297667bea9"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/sampling-qa-voice-ai-agents"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-22T12:05:46.538010+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:7250b9b4906849104916c3a8c282e92efe2ee0c19a33d59b72e9b0bb7da8144d"
---

# Sample-based QA doesn't work for voice AI agents

Contact centers have run quality assurance on a random sample of calls for the better part of two decades. Analysts pull one or two out of every hundred, score them against a scorecard, and coach the agent on what came up. It was never a great methodology, but for human agents whose behavior changes slowly,[1 to 3 percent sampling](https://www.verint.com/guides/call-center-quality-assurance-best-practices/) produced useful signal for the coaching conversations that came next.


Voice AI agents break that model in ways most QA leaders haven't fully priced in. The agent under review this week isn't the same agent that ran last week. The 2,000 calls you sampled from aren't drawn from a stable population, they're drawn from whichever build was in production between prompt tweaks, model version bumps, and TTS swaps. Sampling assumes the underlying population is stable. In voice AI, it isn't.


## The 2% number, and why it's a ceiling


Manual QA at scale has always been operationally bounded. Traditional QA programs review[1 to 3 percent of interactions](https://www.verint.com/guides/call-center-quality-assurance-best-practices/) because human analysts working through 10 to 15 minute recordings cap out at maybe 20 calls a day. Reviewer scoring also drifts.[Manual scoring accuracy tops out around 70%](https://krisp.ai/blog/call-center-qa-sampling-to-100-percent/) because two reviewers applying the same rubric routinely reach different scores on the same call.


When callers, agents, and rubrics all changed slowly, 2% was defensible. You could argue you'd catch systemic issues in aggregate. The shift the enterprise QA world is already going through has nothing to do with voice AI at all: Fiserv moved from evaluating[1% of calls manually to 96% automatically](https://www.verint.com/guides/call-center-quality-assurance-best-practices/) with a human contact center, because 1% was hiding too much.


Voice AI makes the same trade sharper.


## Three ways sampling breaks harder for AI agents


Human-agent QA has a coverage problem. Voice-AI QA has that problem plus three specific ones.


**Change velocity.** xAI's[Voice Agent Builder](https://x.ai/news/grok-voice-agent-builder) , released July 1, promises a working phone agent in about two minutes. That's genuinely useful, and it also means the agent handling your calls tomorrow morning isn't the agent handling them tonight. If you sampled 300 calls last month, most of them describe a build you no longer ship. You aren't coaching an agent, you're autopsying a version.


**Low-frequency, high-impact failures.** In a human contact center, a random sample of 60 calls surfaces the most common patterns fairly well. Voice AI failures don't distribute that way. A hallucinated policy statement, an incorrect price quote, a missed AI disclosure: these live in the fraction of calls that pattern-match to some specific caller phrasing your test set never anticipated. As one operator put it,[one bad block in a conversation flow can drive 15% of drop-offs](https://dapta.ai/blog-posts/ai-voice-agent-call-evaluation/) , and you'll never find it in a sample of ten random calls, because random samples surface random problems, not the concentrated ones that are actually costing money.


**Audio-specific failures are invisible to transcripts.** This is the one that catches teams off guard. Most "AI QA" tools score the transcript. But the transcript can't tell you the agent stepped on the caller's next word, mispronounced a medication name, replied 3.8 seconds after the caller finished speaking, or delivered a legally required disclosure in a rushed monotone the caller didn't register. A voice agent isn't the text of its outputs. It's the audio the caller hears, and half the failure modes only exist there.


Sampling QA versus voice AI QA


## The population is shifting under you


Statistical sampling assumes the sampled distribution is representative of the underlying population. In voice AI, three things shift that population between the moment you sample and the moment you act on it.


- **Prompt and knowledge base changes.** Every tweak to the system prompt or retrieval index changes what the agent says. A sample from before the change tells you nothing about after it.
- **Model version drift.**[GPT-Live launched July 8](https://openai.com/index/introducing-gpt-live/) with a full-duplex architecture that listens and speaks at the same time and delegates hard questions to a frontier model behind the scenes. Providers upgrade underlying models on their own timelines, sometimes silently. The agent you evaluated at v1 is not the agent your caller reaches at v1.1.
- **Traffic drift.** Marketing pushes a new campaign, a partner integration goes live, a holiday shifts your call mix. Your Tuesday sample doesn't describe your Wednesday reality.


The fix is not "sample harder." The fix is to stop pretending the population is stable, and to treat every pre-launch change and every live call as an observation you need.


## The two-part fix


Voice AI quality doesn't collapse to a single line item. It has two shapes, and you need both.


### 1. Simulate before launch, on every change


The cheapest place to catch a regression is before your callers do. Simulation testing dials the agent over real telephony with synthetic callers whose personas, accents, pace, and background noise you control. You run the same suite before every change. When a prompt edit that "obviously can't affect billing calls" breaks the refund flow, you find out at your desk, not from a Trustpilot review three days later.


Simulation is also the only defensible way to keep pace with the change velocity described above. If your agent ships in two minutes, your regression pass has to run in minutes too, and it has to run on the actual phone stack, because that's where the failure modes live. A text loopback that never touches PSTN or WebRTC will not catch endpointing regressions, DTMF handling issues, or codec-driven ASR errors.


### 2. Score every live call in production, on audio-native metrics


Once the change is live, sampling isn't a QA methodology. It's a coverage confession. Every call needs to be scored against a metric set that includes the audio behaviors sampling would miss even at 100% coverage of transcripts.


The scoring rubric that matters for voice AI includes:


- **Interruption and turn-taking** — did the agent step on the caller
- **Latency and dead air** — how long did the caller wait between finishing and hearing a reply
- **Pronunciation** — proper nouns, drug names, city names, product SKUs
- **Emotional register and vocal stress** — did the tone match the situation
- **Pace and pauses** — was the disclosure delivered in a way a human could actually parse
- **Adherence to script or knowledge base** — factual claims that trace to a source
- **Tool-use correctness** — did the right tools fire with the right arguments


Transcript-only QA can score the last two. The first five require audio-native evaluation. If your tooling scores on transcripts alone, you have a 1970s scorecard applied to a 2020s stack.


Illustrative scored call showing audio-native metrics


## What good looks like operationally


The pattern the strongest voice AI teams have converged on isn't complicated:


1. Every change (prompt, knowledge base, model, TTS voice) runs a simulation suite before it merges. Suite failures gate the deploy.
2. Every production call gets scored against the same metric set the simulation used. Same rubric, dev to prod.
3. Failures in production become new simulation scenarios automatically, so the same regression can't happen twice.
4. Humans still review, but only the calls the system already flagged. Their time goes to interpretation and coaching, not listening.


That's the loop that lets a team ship a voice agent every week and still sleep at night. It also happens to be the loop the market is converging on.[Retell Assure monitors 100% of calls](https://www.assemblyai.com/blog/voice-ai-in-2026-series-1) automatically at Retell's scale, flagging failures and assigning scores rather than waiting on a sampling cycle, and Retell's newly-launched[Conductor](https://www.voiceaispace.com/news/voice-ai-news-2026-07-06) is a graph-native review interface aimed at the same problem. The direction is clear enough that the platforms shipping the agents are shipping the review layer too.


## What this means for your QA program


If you're a QA leader coming from a traditional contact center, three practical shifts:


- **Retire "target sample size" as a KPI.** It measured effort under a scarcity constraint that no longer applies. Replace it with coverage percentage, mean time to detect a regression, and the ratio of production failures already reproducible as a test.
- **Move the scorecard into the pre-launch loop.** The metric definitions your QA analysts use in review should be the same metrics that pass or fail a build. If they're not, you're grading two different agents.
- **Budget for audio-native evaluation.** If your current tooling can only score transcripts, half your rubric is unenforceable. Interruption and turn-taking, latency and dead air, pronunciation, and emotional register are all real failures, and none of them show up in text.


## Where Roark fits


[Roark](https://roark.ai/) is built for this loop. Simulation testing dials your agent over real PSTN and WebRTC calls, driven by personas with configurable voices, accents, pace, emotion, and background noise, across 45 languages and accents. Runs are triggered over HTTP so a simulation suite can gate CI the same way unit tests do. When production breaks in a way your suite didn't anticipate, production call replay captures the real call and replays it against updated agent logic, turning the failure into a repeatable regression test you keep forever.


Every live call is scored against 64+ built-in audio-native metrics, plus any custom metrics you add, covering pronunciation, emotion, vocal stress, pace and pauses, and interruptions: the audio behaviors that transcript-only QA can't see. When a call breaks a metric, Roark files it as an issue automatically. One-click integrations wire this up for Vapi, Retell, LiveKit, Pipecat, Bland, and ElevenLabs, and OpenTelemetry traces make everything queryable alongside the rest of your observability stack. The[SDKs and integration guides](https://docs.roark.ai/) cover the specifics.


Sampling was a decision to be surprised on 97% of your calls. In a stable human contact center, that was a tolerable trade. In voice AI, where the agent changes weekly and half the failure modes hide in audio, it's a decision you can't afford twice.
