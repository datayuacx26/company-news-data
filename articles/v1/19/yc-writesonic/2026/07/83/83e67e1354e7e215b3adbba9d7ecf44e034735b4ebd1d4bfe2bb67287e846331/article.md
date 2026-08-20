---
schema_version: "1.0.0"
document_id: "83e67e1354e7e215b3adbba9d7ecf44e034735b4ebd1d4bfe2bb67287e846331"
company_key: "yc-writesonic"
company: "Writesonic"
source_id: "yc-writesonic-news-import-0d409124f8fc"
canonical_url: "https://writesonic.com/blog/ai-search-ranking-stability-study"
published_at: "2026-07-22T14:03:48.260+00:00"
first_seen_at: "2026-07-22T20:38:42.288608+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:74a1d32d07a516912ce29cbace7f177a44d72bc31d6916f130a0a253a69a30a9"
---

# The Brand at #1 in AI Search Changes 52% of the Time. We Ran 631,999 Prompts to Measure It.

Run the same prompt on ChatGPT 10 times. Watch which brand shows up at #1.


It changes. More than half the time.


We ran **631,999 prompt-model pairs across 7 AI platforms** from March through June 2026, every prompt repeated a minimum of 10 times. On ChatGPT: 52% volatile. Only 8% locked. The brand holding #1 today is 1.18x more likely to hold it tomorrow than random chance. That is a 6-percentage-point edge on a 39% baseline.


That is all of the incumbency advantage AI search currently offers.


If your GEO strategy is built around capturing and defending a #1 AI ranking, you are optimizing for a position that rotates among 7 to 9 brands with almost no structural lock-in.


## Summary of findings


- **52% of ChatGPT #1 rankings rotate** across reruns of the same prompt. Only 8% are locked. The remaining ~40% are contested.
- **Incumbency lift is 1.08–1.18x above chance** across all 7 platforms. No platform delivers true lock-in.
- **Grok is the stickiest platform** (0.655 mean persistence, 13.1% locked, 5.6 distinct leaders per query). **ChatGPT is the most volatile** (0.519 mean persistence, 8% locked, 9.2 distinct leaders).
- **Slot 1 is 3.4–4.8x more stable than slot 5** positionally, but the brand filling it still rotates among 7 to 9 contenders.
- **Brand-mention prevalence (r ≈ 0.66) predicts AI ranking roughly 3x better than backlinks.** It is the strongest signal in this dataset.


## How we ran this


Every bar represents prompts run a minimum of 10 times per platform. ChatGPT and Grok sit at opposite ends of the volume range, which matters for interpreting their persistence figures in context.


Seven platforms: ChatGPT, Google AI Overviews, Gemini, Perplexity, AI Mode, Microsoft Copilot, and Grok. Claude, Meta AI, and Perplexity Pro did not meet the inclusion threshold and were excluded. Every prompt ran a minimum of 10 times per model; failed runs were excluded.


We measured which brand holds the #1 slot, run by run, on the same query. Then we measured how often it returned and how much better than chance that was. Three metrics drove the analysis: mean persistence (share of runs where the same brand holds #1), locked rate (prompts with a consistent winner), and incumbency lift (actual stay-rate divided by baseline probability for that platform's competitive field). The stickiness analysis used 21-day windows. All findings are correlational.


## Incumbency lift ranges from 1.08x to 1.18x above chance. Across every platform.


The tightness of the cluster near 1.00x is the finding: no platform delivers meaningful lock-in, regardless of how different raw retention rates look.


No platform produces meaningful lock-in. That is the headline finding. Incumbency lift tops out at 1.18x on ChatGPT and bottoms at 1.08x on Grok. Every platform sits in between. The range is narrow, the proximity to random chance is the point, and it holds regardless of how different the raw stay-rates look from platform to platform.


Grok posts a 57% raw retention rate. That looks like dominance compared to ChatGPT's 45.2%. But Grok also runs fewer competing brands per query by design, which mechanically inflates stay-rates without any genuine lock-in. Normalize for that and Grok's lift is 1.08x, the lowest of any platform tested. Its Q1 apparent stability traces to base-rate effects, not competitive moat.


ChatGPT goes the other way. The most volatile platform in raw terms, with 52% of queries showing the top brand rotating and 9.2 distinct brands trading the slot per query category, also produces the highest incumbency lift at 1.18x. More competition, narrower baseline, real but thin advantage.


Raw stay-rates mislead. Lift tells you what is real. And what is real, across all 7 platforms and 631,999 prompt-model pairs, is that the brand you think owns your category in AI search is probably one of seven to nine trading that slot around half the time.


**What to do:** Stop reading AI ranking screenshots as proof of position. Run the same prompt 10 times and count how often your brand comes back at #1. That distribution, not any single result, is your actual visibility.[Writesonic's AI visibility tracker](https://writesonic.com/ai-visibility-tracker) measures share of voice across 10 platforms using data from 2B+ real AI conversations, giving you a run distribution rather than a single-moment read.


*Dig deeper:[GEO KPIs Every Brand Should Track in 2026](https://writesonic.com/blog/geo-kpis-every-brand-should-track)*


## Slot 1 is structurally stable. The brand inside it rotates among 7 to 9 contenders.


Positional stability and brand identity stability are two different measurements. Conflating them produces bad strategy.


Slot 1 is architecturally privileged. Across all 7 platforms, the variance at slot 5 is 3.4–4.8x higher than at slot 1. ChatGPT, Gemini, Grok, Perplexity, and Copilot cluster between 4.0–4.8x. AI Overviews and AI Mode sit at 3.4–3.7x. Slots 1 through 5 are reliable for measurement. Slots 7 and 8 are not.


So AI reliably produces a top-ranked answer. That slot exists consistently. What is not consistent is which brand fills it. The throne is stable. The occupant rotates.


This matters because a lot of AI visibility strategy is implicitly built around positional stability: if slot 1 is reliable, defending it must be valuable. What this data shows is that slot 1's structural stability does not transfer to the brand inside it. You get the slot. You do not get the permanence.


**What to do:** If your AI visibility setup is showing you a single rank position per query, you are measuring the slot, not your brand's presence in it. The metric worth tracking is how frequently your brand occupies that slot across repeated runs, not whether slot 1 exists. Track share of voice across runs, not rank position on a given day.


*Dig deeper:[10 Common GEO Mistakes Preventing Your Brand From Appearing in AI Answers](https://writesonic.com/blog/common-geo-mistakes-ai-search)*


## Brand-mention prevalence predicts AI ranking 3x better than backlinks (r ≈ 0.66)


We tested four proposed drivers of top AI ranking. Brand-mention prevalence across AI-generated answers correlates with AI inclusion at r ≈ 0.66, roughly 3x stronger than the correlation with backlinks. It is the strongest signal in this dataset by a wide margin.


The logic is not complicated. The brands AI recommends most consistently are the ones most present in the content AI trains and retrieves on. Reddit threads, independent review sites, comparison articles, community discussions. The brands rotating through the top cluster are the ones people talk about in places those brands do not control. Third-party citations function as trust signals. Domain authority drives inclusion via expertise signals. Branded prompts, where a query names a specific brand, likely explain most of the 8% of locked positions on ChatGPT, since the model defaults toward the anchored brand.


None of these are confirmed causal. All are correlational. The r ≈ 0.66 comes from this dataset.


The budget implication is direct. A link-building campaign aimed at improving AI visibility is working on a signal that is roughly 3x weaker than brand-mention prevalence. The marginal hour spent building third-party mentions in Reddit threads, independent comparison articles, and community forums outpredicts the marginal hour spent on link building for AI ranking purposes.


**What to do:** Audit where your brand gets mentioned outside your own site. Count the active Reddit threads, the independent listicles, the community forum discussions where your brand shows up. That surface, not your backlink count, is the primary input to the signal that correlates most strongly with AI ranking. The[AEO checklist](https://writesonic.com/blog/aeo-checklist) covers the content structure side. Third-party mention building needs its own dedicated effort and budget line.


*Dig deeper:[Which Sources Influence AI Answers? 2,000+ Brand Study](https://writesonic.com/blog/sources-ai-models-cite-brand-visibility)*


## What this changes for GEO


The competitive window in AI search is more open than most rankings data suggests. Incumbency lift caps at 1.18x. No brand has locked in a moat. The brands leading your category in AI answers today are doing so while rotating through the slot with six to eight other brands. That is not dominance. That is a lead in a dynamic system.


Competing in that system means three things.


Get into the rotating top cluster and stay there. Consistent presence across reruns of the same query is more durable than occasional #1 spikes. A brand appearing at #1 on 40% of runs outperforms one that held it on a single scrape and disappeared. Tracking share of voice across repeated runs is what makes that visible and actionable.


Build brand-mention volume as a primary input. r ≈ 0.66 is strong enough to allocate budget around. Reddit threads, independent listicles, community discussions, high-DR comparison pages. These are the channels with the highest correlation to AI ranking in this dataset.


Apply lift, not raw stay-rates, to competitive analysis. Before assuming a competitor has locked in a durable position, find the baseline probability for your category and run the calculation. Grok's 57% retention looks like a moat. At 1.08x lift, it is not. The same arithmetic applies to every category.


## Track your brand's AI ranking, not just your competitors'


The data from 631,999 prompt-model pairs points in one direction: AI search rankings are a rotating system, not a fixed hierarchy. The brands that win over time are the ones with consistent presence in the top cluster, built on brand-mention prevalence and third-party citation signals, measured across repeated runs rather than single snapshots.


Writesonic tracks your brand's AI visibility across 10 platforms, measures run distributions rather than single-point ranks, and surfaces the citation and mention gaps worth closing.[Start free](https://app.writesonic.com/signup) or[book a demo](https://writesonic.com/get-a-demo) to see where you actually stand.


## Methodology


631,999 prompt-model pairs across 7 platforms: ChatGPT, Google AI Overviews, Gemini, Perplexity, AI Mode, Microsoft Copilot, and Grok. Data collected March through June 2026. Minimum 10 reruns per prompt per platform; failed and incomplete runs excluded. Claude, Meta AI, and Perplexity Pro excluded for not meeting the inclusion threshold.


Primary measurement: which brand holds the #1 slot, tracked run by run across repeated instances of the same prompt. Incumbency lift calculated as actual stay-rate divided by baseline probability for each platform's competitive field. Stickiness analysis used 21-day windows. All reported correlations are Pearson r. No causal claims.


###


###


###


###


###


###


###


[Samanyou Garg](https://writesonic.com/blog/author/samanyou-garg)


Founder @ Writesonic


Samanyou is the founder of Writesonic, a platform that helps you track & boost your brand’s visibility in AI search. Two years before the launch of ChatGPT, Writesonic was already at the forefront, helping organizations automate their entire marketing workflow through specialized AI agents for SEO and content. Samanyou is a Forbes 30 Under 30 awardee and a winner of the 2019 Global Undergraduate Awards, often referred to as the junior Nobel Prize.


[Tarsh Swarnkar](https://writesonic.com/blog/author/tarsh-swarnkar)


Data & Infrastructure @ Writesonic


Tarsh builds the data pipelines behind Writesonic's AI visibility research, including the AI Ads Index. His work focuses on real-time measurement of how AI platforms cite, mention, and rank brands.
