---
schema_version: "1.0.0"
document_id: "95690fe2986f32fb040029ee857b5e988692796522f655cb190e1afd1caf4225"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-voice-agents-that-sell-retail-sales-floor-qa"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T10:15:28.316611+00:00"
fetched_at: "2026-08-05T10:15:29.866635+00:00"
content_hash: "sha256:4b0721c02488daa663d399ffd47b779985ee71f2ba599c5d18b7c1838c361302"
---

# Testing voice agents that sell: a QA playbook for the retail sales floor

Something changed for retail voice AI this month. Japanese electronics chain Yamada Denki, working with avatarin, put a voice agent built on OpenAI's GPT-Realtime in front of shoppers on its online store. In two weeks,[about 30,000 people used the agent, and 92% of post-conversation survey responses were positive](https://openai.com/index/avatarin/) . Customers reportedly said it was easier to talk to than a human associate,[and asked follow-up questions without feeling stressed](https://www.pymnts.com/news/artificial-intelligence/2026/ai-voice-agents-take-over-the-retail-sales-floor/) .


That is a different job than the voice agents most teams have been shipping. For two years, retail voice AI mostly meant deflecting "where is my order," confirming store hours, and routing complex issues to humans. The new generation asks probing questions, recommends products, and closes. Which means your QA process, if it was built around support metrics, is now testing the wrong things.


## Sales agents fail in different places than support agents


A support agent's worst day is a caller who hangs up frustrated. A sales agent's worst day is a caller who buys the wrong thing because the agent recommended it, or who books a service the store cannot actually deliver at the price it just quoted. The stakes are asymmetric: a support failure costs a ticket, a sales failure costs a chargeback, a refund, or worse, a legal complaint about a misquoted price.


The failure taxonomy for a sales-facing voice agent is not the same list you inherited from your support QA process. It looks like this:


- **Pricing hallucinations.** The agent invents a discount, quotes an outdated price, or reads a bundle rate that no longer applies. This is[the most documented failure mode for LLM-driven sales agents](https://www.autointerviewai.com/blog/ai-voice-agent-hallucinations-sales-lies-2026) , and it is easy to trigger with a leading question ("I heard you have a summer promo, is that right?").
- **Inventory fabrication.** The agent confirms stock at a location that does not have it, or misses that a SKU has been discontinued. Practitioners have called this out as a specific retail failure mode where the AI "sits alone" without live systems:[if it cannot pull live inventory or confirm an order status, it defaults to scripted, unreliable answers](https://www.bland.ai/blog/ai-voice-agents-for-retail) .
- **Wrong recommendation.** The shopper describes what they need. The agent surfaces a product that technically matches keywords but is wrong on a dimension the shopper did not think to say aloud (voltage, size, subscription commitment, warranty scope).
- **Overpromising on service.** Same-day delivery it cannot honor, an installation window that isn't offered in that region, a return policy that has not applied for six months.
- **Missed upsell where required.** In some verticals, "did you offer the warranty" is a compliance-adjacent line item, not a nice-to-have.
- **Silent close failures.** The shopper is ready to buy and the agent keeps discovering. Or the shopper is not ready to buy and the agent forces a close and loses them.
- **Language / accent regressions.** The Yamada Denki agent is[explicitly multilingual](https://openai.com/index/avatarin/) , and GPT-Realtime is designed to[switch seamlessly between languages mid-sentence](https://openai.com/index/introducing-gpt-realtime/) . That flexibility is a strength on a demo and a testing headache in production: the agent's behavior on Japanese-English code-switching is not necessarily the same as its behavior on Spanish-only calls.


None of these show up in a "did the agent respond politely?" review. Half of them do not show up in a transcript-only review at all, because they hinge on what the shopper actually needed versus what the agent decided they needed.


Support QA and sales QA are not the same job


## The gap traditional QA leaves


Most retail voice-agent programs still rely on a mix of three things: manual review of a random sample of calls, keyword-based rules ("did the agent say the word 'price' outside these approved contexts?"), and a periodic mystery-shopper test run by a QA vendor. Each of these breaks against the failure taxonomy above.


Random sampling is[a known dead end at production volume](https://roark.ai/blog/sample-based-qa-doesnt-work-for-voice-ai-agents) . If you listen to 50 calls a week and the agent takes 5,000, a discount-fabrication failure happening once every 300 calls will hit you at scale before it hits your QA sample. Keyword rules catch some price hallucinations, but they cannot judge whether the recommended product actually matched the shopper's stated needs, or whether the agent talked over the caller during the close. And mystery shopping happens on a cadence, not on the deploy that shipped this morning.


The QA process a retail sales agent actually needs looks more like software testing than call-center QA:


1. A **pre-launch simulation suite** that dials the agent with realistic shopper personas across the failure modes you care about, before any real customer talks to it.
2. **Every live call scored** against those same metrics automatically, so a regression that ships on Tuesday is visible on Tuesday.
3. **Real production calls turned into replayable regression tests** , so the shopper who almost got mis-quoted last month becomes a scenario you never fail again.


This is where[Roark](https://roark.ai/) fits: simulation testing is the core capability, and the same metric definitions run against both the pre-launch simulations and every live production call.


## Pre-launch: simulate the shoppers, not the happy path


The Yamada Denki result is impressive because voice agents on the sales floor are exposed to something support agents rarely are, which is the full noisy distribution of real shopper behavior. Cooperative shoppers with a clean spec. Skeptical shoppers pushing on price. Confused shoppers who describe the product wrong. Bilingual shoppers who switch languages when they get frustrated. Elderly shoppers speaking slowly. Shoppers on a bad mobile connection. Kids in the background.


A sales simulation suite has to reflect that distribution, not just the happy path. In practice that means personas with distinct voices, accents, speech pace, emotional register, and background-noise environments, and scenarios that exercise the failure taxonomy directly. Roark's simulations[run over real PSTN and WebRTC calls, not a text loopback](https://docs.roark.ai/) , so the audio path you test is the audio path production traffic uses. Personas define caller voice, language and accent, pace, emotion, and background noise. Simulations are scored in[45 languages and accents](https://roark.ai/) , which matters the moment your agent is multilingual by design.


Illustrative pre-launch suite for a retail sales agent


The point of the suite is not to hit 100% pass. The point is that when you change the prompt or swap the model, the suite tells you which behaviors regressed and which improved. A retail team promoting a new summer sale should not learn from a customer that the agent is still quoting last quarter's discount.


## Post-launch: score every call, not a sample


The hard part of a sales agent is that failures often happen on calls that look fine. The shopper is polite. The agent is fluent. Everyone hangs up. Then, three days later, the order comes back because the product the shopper bought did not do the thing they said they needed it to do, and the transcript reveals the agent recommended it without asking a clarifying question that a human associate would have asked.


That failure is invisible to sentiment analysis and invisible to random sampling. It is visible to a metric suite that scores every call against explicit criteria: did the agent verify the constraint the shopper mentioned, did the recommended product satisfy that constraint, did the agent quote a price that matches the current price list, did the agent close in a way that matches policy.


Roark scores every production call against[64+ built-in metrics plus unlimited custom metrics](https://roark.ai/) , and its[audio-native models score the sound of the call, not just the transcript](https://roark.ai/) , covering pronunciation, emotion, vocal stress, pace and pauses, and interruptions. That last set matters more for a sales agent than it does for a support agent: a call where the agent talks over the shopper twice during the close is a call that lost trust even if the transcript reads clean. When a call fails a metric, Roark files it as an issue automatically, so the failure lands in a queue instead of vanishing into a dashboard.


A live call flagged by the metric suite


## Regression: yesterday's near-miss becomes today's test


The scenarios you actually want to test against are the ones your real callers already tried. A shopper who almost got mis-quoted, an accent the agent misheard, a barge-in the agent handled badly, a hand-off that did not happen when it should have. Roark supports[capturing real production calls and replaying them against updated agent logic](https://roark.ai/) , turning real failures into repeatable regression tests. Every prompt change, model swap, or tool update runs against that growing library before it ships.


For retail specifically, this is the compounding advantage. Season two of your sales agent has to be measurably better than season one, and the only credible way to prove that is to run season two against the exact call distribution season one struggled with. Not a synthetic benchmark, not a vendor's demo suite. Your callers.


## A minimum bar for launching a voice sales agent


If you are shipping or planning to ship a retail voice agent that participates in sales, not just support, treat this as a floor:


1. **A named failure taxonomy.** Write down the six to twelve failure modes that would embarrass your brand or trigger a refund. Score every simulation and every live call against those, by name.
2. **A simulation suite that covers the taxonomy.** Not just polite shoppers. Difficult shoppers, non-native speakers, background noise, callers pushing on price, callers describing the product wrong.
3. **Multilingual coverage that matches your customer base.** If your agent claims to handle Japanese-English code-switching or Spanish-only calls or accented English, your suite covers all three, not just the demo language.
4. **Every production call scored on the same metrics as your simulations.** Not sampled. Every call.
5. **Automatic issue creation.** When a call fails a metric, a human sees it. No exceptions, no daily digest of "43 anomalies detected."
6. **Regression coverage from real calls.** The failure that got through last week is a permanent test case, not a memory.
7. **A CI gate.** Prompt changes and model swaps[run the suite before they hit production, via HTTP-triggered runs](https://docs.roark.ai/) , and a red suite blocks the deploy.


The Yamada Denki story is the leading edge of something the rest of the industry will do next: voice agents that actively sell, in multiple languages, on real phone and web calls, at retail scale. The teams that ship those agents responsibly will be the teams whose QA process caught up with what the agent is now doing. Support-era QA is not going to be that process.
