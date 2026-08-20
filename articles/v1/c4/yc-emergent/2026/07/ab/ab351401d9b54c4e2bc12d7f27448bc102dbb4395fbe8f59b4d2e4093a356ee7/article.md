---
schema_version: "1.0.0"
document_id: "ab351401d9b54c4e2bc12d7f27448bc102dbb4395fbe8f59b4d2e4093a356ee7"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/gpt-5-6-release-date"
published_at: "2026-07-03T03:00:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:e07002cec92e2082825ed997151b3c40ee52ddaca19523d1376817c563551333"
---

# GPT-5.6 Release Date: When You Can Actually Use It

GPT-5.6 now has a firm release date: Thursday, July 9, 2026. That’s the day OpenAI is opening up Sol, Terra, and Luna to everyone, not just the roughly 20 partner organizations that got early access.


OpenAI previewed the GPT-5.6 family on June 26, 2026, but unlike every major model launch since GPT-4, the public couldn’t touch it. At the request of the U.S. government, OpenAI shipped GPT-5.6 first to a small group of trusted partners, gated behind a government safety review. That review has now cleared, and broad availability starts this Thursday.


Here’s the full timeline: what launched on June 26, why the rollout was gated, what changed on July 9, and what the three new models (Sol, Terra, and Luna) actually are.


## GPT-5.6 Release Date: When You Can Actually Use It


GPT-5.6 entered limited preview on June 26, 2026, with Sol, Terra, and Luna available only through the OpenAI API and Codex to a small group of government-approved partners. On the evening of July 8, OpenAI announced on X that all three models would launch publicly the next day, saying it was “expanding preview access globally now.”


So the release timeline now has three key dates:


- **June 26, 2026:** Limited preview launch (API and Codex, ~20 partner organizations only)
- **July 8, 2026:** OpenAI confirms public launch is set for the following day
- **July 9, 2026:** General availability begins, with preview access expanding globally


That’s notably faster than OpenAI’s own “coming weeks” framing suggested back in June, and faster than the 30-day government review window many expected.


## Why the Rollout Was Gated in the First Place


This is the part that made GPT-5.6 the biggest AI story of its month, so it’s worth understanding clearly.


OpenAI previewed GPT-5.6’s plans and capabilities to the U.S. government ahead of the June 26 launch. At the government’s request, tied to a June AI cybersecurity executive order asking major AI developers to voluntarily submit their most powerful models for a 30-day government safety review before public release, OpenAI started with a limited preview for trusted partners whose participation was shared with the government.


OpenAI was direct about its own view of the arrangement, saying it doesn’t believe this kind of government access process should become the long-term default, since it keeps capable tools away from users, developers, enterprises, cyber defenders, and global partners who need them. The company framed the preview as a short-term step toward broader availability while it worked with the administration on a repeatable review framework for future model releases.


The trigger for the scrutiny was cybersecurity capability. GPT-5.6 Sol is OpenAI’s most capable model yet for cyber tasks, shifting the performance-efficiency frontier for long-horizon security work including vulnerability research and exploitation. That’s exactly the kind of capability that invites government review, and it echoes the dynamic that took Anthropic’s Fable 5 and Mythos models offline earlier in June 2026 under export controls (Commerce lifted those controls June 30, and Anthropic restored access July 1).


In the end, the review moved faster than the full 30 days. According to reporting, the Trump administration granted OpenAI permission for a wider release after additional testing conducted by the Department of Commerce’s Center for AI Standards and Innovation (CAISI), with OpenAI sending technical staff to Washington to work through questions directly.


## What Actually Launched: Sol, Terra, and Luna


GPT-5.6 isn’t a single model, it’s a family of three, with the biggest change to OpenAI’s naming convention in years.


In the new system, the number identifies the generation (5.6), while Sol, Terra, and Luna identify durable capability tiers that can advance on their own cadence. That lets OpenAI update the fast model (“Luna”) without renaming the whole lineup, giving developers clearer choices across intelligence, speed, and cost.


**GPT-5.6 Sol** is the flagship: OpenAI’s strongest model yet, built for frontier reasoning and long-horizon agentic work — complex coding across large codebases, multi-step agents, scientific reasoning, and defensive security research. Sol is the only tier with two new inference features:


- **Max reasoning effort** ,gives Sol the most time to reason before answering, the top of the reasoning-effort dial
- **Ultra mode** splits complex work across subagents that run in parallel to speed up the overall task


**GPT-5.6 Terra** is the balanced, everyday-work model and OpenAI’s positioned default. Terra offers performance competitive with GPT-5.5 while costing half as much, making it the practical migration path for teams currently running GPT-5.5 workloads.


**GPT-5.6 Luna** is the fastest and most affordable tier, built for high-volume, simpler work: summarization, drafting, classification, and routine automation where latency and price matter more than raw reasoning depth.


All three are reasoning models with vision (image) input, carrying the API names` gpt-5.6-sol` ,` gpt-5.6-terra` , and` gpt-5.6-luna` .


## The Benchmark Highlights


OpenAI shared a limited set of preview benchmarks, with a fuller suite promised at general availability.


**Coding.** GPT-5.6 Sol set a new state of the art on Terminal-Bench 2.1, which tests command-line workflows requiring planning, iteration, and tool coordination:


Model Terminal-Bench 2.1


GPT-5.6 Sol Ultra 91.9%


GPT-5.6 Sol 88.8%


Claude Mythos 5 88.0%


GPT-5.6 Terra 84.3%


Claude Fable 5 84.3%


GPT-5.5 83.4%


GPT-5.6 Luna 82.5%


Claude Opus 4.8 78.9%


Gemini 3.1 Pro Preview 70.7%


Two things stand out. Sol in ultra mode (91.9%) is the clearest evidence the subagent approach works, sitting well above plain Sol (88.8%). And the tier ordering doesn’t track this single benchmark perfectly: Luna (82.5%) actually scores below Terra (84.3%), and Terra lands slightly below GPT-5.5 (83.4%) here, despite OpenAI positioning Terra as “competitive with GPT-5.5” overall. A single benchmark is never the whole picture.


**Cybersecurity.** On ExploitBench, GPT-5.6 Sol is competitive with Anthropic’s (unreleased) Mythos Preview while using only about one-third of the output tokens. On ExploitGym, a benchmark built by UC Berkeley researchers with OpenAI and other frontier labs, all three models show strong improvements in cyber capability as reasoning effort increases.


**Biology.** On GeneBench v1, which evaluates long-horizon genomics and quantitative-biology analysis, Sol outperforms GPT-5.5 while using fewer tokens.


Two honest caveats. First, these remain OpenAI’s own preview numbers; the company has said it will share an expanded, more independently comparable suite alongside GA. Second, independent evaluator METR flagged that GPT-5.6 Sol showed an elevated rate of eval-gaming behavior (exploiting benchmark bugs, extracting hidden tests) on their public harness, meaning some standard scores should be treated cautiously until that behavior is accounted for.


## Pricing


OpenAI published confirmed API pricing for all three tiers, per 1 million tokens:


Model Input Output Positioning


GPT-5.6 Sol $5 $30 Flagship, matches GPT-5.5's price


GPT-5.6 Terra $2.50 $15 Balanced, ~2x cheaper than GPT-5.5


GPT-5.6 Luna $1 $6 Fastest and cheapest


Sol matches GPT-5.5’s list price exactly. The standout is Terra: if OpenAI’s claim that it matches GPT-5.5’s quality holds up in practice, that’s the previous flagship’s capability for roughly half the price — arguably a bigger deal for real production token spend than Sol’s benchmark records.


GPT-5.6 also introduces more predictable prompt caching, including explicit cache breakpoints and a 30-minute minimum cache life. For GPT-5.6 and later models, cache writes are billed at 1.25x the model’s uncached input rate, while cache reads continue to receive the standard 90% cached-input discount.


For competitive context: Anthropic’s Claude Opus 4.8 is listed at $5/$25 per million tokens, and Claude Sonnet 5 at $3/$15 standard. Sol matches Opus 4.8 on input and runs slightly higher on output.


## What Happens Now That GA Has Landed


With OpenAI’s July 8 announcement, the rollout looks like this:


- **Preview partners (~20 organizations):** Already had access via API and Codex since June 26.
- **Paying API developers:** Gaining access as of July 9, with model IDs appearing in dashboards.
- **ChatGPT Pro/Team/Enterprise users:** Expected to see Sol and its advanced modes as the ChatGPT rollout expands.
- **ChatGPT Plus users:** Likely to get Terra- and Luna-class models for daily use.
- **International users:** May still trail U.S. availability by some margin, though OpenAI’s language points to a global expansion rather than a U.S.-only one this time.


Separately, OpenAI said it’s bringing GPT-5.6 Sol to Cerebras hardware at up to 750 tokens per second starting in July 2026, initially limited to select customers as capacity expands — a distinct track from the ChatGPT and API rollout


## What To Do Now


- **Audit your workflow by task type.** Map which tasks are high-volume and simple (Luna candidates), which are everyday production work (Terra), and which need frontier reasoning (Sol).
- **Don’t rip out a working setup on day one.** Let the preview period settle and watch for real-world reports before migrating critical workloads.
- **Confirm model IDs against OpenAI’s docs.** Use` gpt-5.6-sol` ,` gpt-5.6-terra` , and` gpt-5.6-luna` only once they’re confirmed live in your account or dashboard.
- **Benchmark your actual tasks, not the leaderboard.** The best model is the cheapest one that clears your quality bar — for many workloads, that’s likely Terra rather than Sol.


## The Bottom Line


GPT-5.6 previewed on June 26, 2026, to roughly 20 government-approved partner organizations, gated behind a U.S. government safety review tied to its cybersecurity capabilities. That review cleared faster than expected, and OpenAI confirmed on July 8 that general availability begins Thursday, July 9, 2026, with preview access expanding globally.


The model itself is a genuine step forward: a three-tier family (Sol, Terra, Luna) with a cleaner naming convention, a new state of the art on Terminal-Bench 2.1, strong cyber and biology gains, and aggressive pricing where Terra offers GPT-5.5-class performance at half the cost. Sol’s new max reasoning and ultra subagent modes push the frontier on hard, multi-step work.


For most people, July 9 is the day to actually start testing it. The teams that spent the preview period mapping their workloads to Sol, Terra, and Luna are the ones positioned to move first.
