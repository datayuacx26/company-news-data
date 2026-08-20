---
schema_version: "1.0.0"
document_id: "f32998fcb017537409eef822a6d7b7baa28c85f05e6b8a4e622f00a865c1e942"
company_key: "yc-writesonic"
company: "Writesonic"
source_id: "yc-writesonic-news-import-0d409124f8fc"
canonical_url: "https://writesonic.com/blog/ai-citation-source-overlap-study"
published_at: "2026-07-22T19:31:11.215+00:00"
first_seen_at: "2026-07-23T08:39:11.879843+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:ef7cbb2237261b0111547e3228310124af04b9bcbe4643446c5a20c9aa09d317"
---

# Do AI Engines Cite the Same Sources? We Studied 161,286 Prompts Across 4 Platforms

3.8%. That's the share of sources that ChatGPT, Gemini, Perplexity, and Google AI Overviews all agree on for the same prompt.


The rest, 72 to 73% of every cited domain, appears on exactly one engine and nowhere else.


If your GEO strategy assumes that ranking well on one AI platform transfers to the others, that 3.8% is the ceiling on how right you are.


We ran 161,286 prompts across all four platforms and measured citation overlap using Jaccard similarity. Of those prompts, 70,879 returned citations from all four engines simultaneously. That's the core comparison set. Any two engines, same question, same prompt: they share roughly 17% of their cited sources.


## How We Measured This


The metric is Jaccard similarity. For each prompt where two engines both returned citations:


*Jaccard = (sources cited by both) ÷ (all unique sources cited by either)*


A score of 1.0 is perfect overlap. A score of 0.0 means nothing in common. Our range across all six engine pairs: 0.12 to 0.24.


Matching happens at domain level: URLs lowercased, "www." prefixes stripped. This is the lenient version. Exact-URL matching would push every score lower. One limitation worth flagging: engines cite different volumes of sources per answer, and a symmetric metric like Jaccard doesn't fully capture that. A directional cut ("of engine A's sources, what share also appear on B?") is in progress as a follow-up.


Across all six engine pairs in this study, scores range from 0.12 to 0.24, meaning even the closest pair shares less than a quarter of their sources.


## No Two Engines Agree on More Than 24% of Their Sources


Every engine pair falls between 0.119 and 0.237. The three ChatGPT pairs cluster at the bottom; the search-grounded engines sit higher but none clear 0.25.


Perplexity and Google AI Overviews are the closest pair at 0.237. For a given prompt, of every domain those two engines cite between them, only 23.7% gets cited by both. The other 76.3% belongs to one engine alone. That is the highest overlap in the study.


ChatGPT and Gemini sit at the other end: 0.119, across 88,969 prompts. 88% of their combined cited sources are exclusive to one engine.


The full breakdown:


- Perplexity and AI Overviews: 0.237 (24% shared, 126,299 prompts)
- Gemini and AI Overviews: 0.216 (22%, 87,558 prompts)
- Gemini and Perplexity: 0.174 (17%, 82,221 prompts)
- ChatGPT and Perplexity: 0.130 (13%, 119,513 prompts)
- ChatGPT and AI Overviews: 0.126 (13%, 128,623 prompts)
- ChatGPT and Gemini: 0.119 (12%, 88,969 prompts)


No pair clears 0.25. There is no engine combination where being cited on one reliably predicts being cited on the other.


**What to do:** Stop benchmarking AI visibility against a single engine. **Pull** citation data per engine and look at each separately. The gaps are in different places, and the fixes are too.


## 72 to 73% of Cited Sources Are Unique to One Engine


Of every domain cited across the 70,879 four-engine prompts, 72 to 73% are cited by exactly one engine. A small share appears on two or three. Just 3.8% are cited by all four.


That 3.8% is the consensus layer. Thin, but real. The 72 to 73% that's engine-exclusive is the part most GEO strategies walk straight past.


A brand earning a citation on Perplexity has not, statistically, earned anything on the other three. The content that gets you into one engine's answers is mostly not the same content moving the needle elsewhere. Our[study of citation influence across 2,000+ brands](https://writesonic.com/blog/sources-ai-models-cite-brand-visibility) showed this from a different angle: the sources that drive AI answer inclusion differ significantly by platform. The citation overlap data now confirms it at scale.


**What to do:** **Audit** your citation footprint per engine, not in aggregate. A citation on Perplexity and a citation on ChatGPT are different outcomes with different drivers. If your GEO work has focused on one platform, you likely have blind spots on the others you haven't measured yet. The[AEO checklist](https://writesonic.com/blog/aeo-checklist) maps the per-engine work in the order that tends to move citations.


## Track which engines are citing you and which aren't


Writesonic tracks citation sources across ChatGPT, Gemini, Perplexity, AI Overviews, and six other AI platforms in one dashboard. For every tracked prompt, you see which engines cite you, which cite your competitors, and where the per-engine gaps are.


[Start free](https://app.writesonic.com/signup) or[book a demo](https://writesonic.com/get-a-demo) and we'll run your brand through the per-engine citation breakdown.


## ChatGPT Is the Outlier. The Other Three Cluster Together.


Pull the six pairwise scores apart and two camps emerge.


ChatGPT's average Jaccard against the other three engines is approximately 0.13, the lowest of any engine in the study. Every ChatGPT pair sits at the bottom of the overlap range. Its citation pool is architecturally different from what the other three return.


This isn't an isolated pattern. Our[AI search ranking stability study across 631,999 prompts](https://writesonic.com/blog/ai-search-ranking-stability-study) found ChatGPT is also the most volatile platform for brand rankings: 52% of its number-one positions rotate on the same prompt. The citation data here adds a second dimension to that. ChatGPT doesn't just rotate brands more than the others. It draws from a fundamentally different source pool than the engines around it.


AI Overviews, Perplexity, and Gemini cluster higher. All three are search-grounded or search-influenced in their retrieval. The Perplexity/AI Overviews pair at 0.237 is the study's ceiling: both are live-retrieval engines pulling from overlapping web indexes.


But "cluster" does a lot of work in that sentence. Even within this trio, pairs share only 17 to 24% of sources. "More alike than ChatGPT" and "similar enough to treat as one strategy" are not the same claim.


What the data supports: there are two separate citation ecosystems. ChatGPT is one. AI Overviews, Perplexity, and Gemini form a related but distinct second. Optimizing for one does not reliably transfer to the other.


**What to do:** **Treat** ChatGPT separately from the search-grounded trio in your GEO strategy. Content and source-building that earns citations on Perplexity or AI Overviews will partially transfer within that cluster. It will not transfer to ChatGPT. If ChatGPT citations matter to your business, audit separately and build separately. The[common GEO mistakes guide](https://writesonic.com/blog/common-geo-mistakes-ai-search) covers the most frequent error here: building one strategy and assuming it generalizes across all four engines.


## What This Means for Your GEO Strategy


With 72 to 73% of citation sources engine-exclusive, a narrow citation footprint means a narrow AI visibility footprint. The brands most exposed are the ones that picked one engine's behavior as the model and assumed the others follow.


A few things worth taking seriously:


ChatGPT's source pool is structurally separate. A strategy built for live-retrieval engines like AI Overviews, Perplexity, and Gemini won't transfer to ChatGPT. The overlap is too low to treat that as an assumption.


Aggregate AI visibility scores lose the signal. If your reporting collapses all engines into one number, you're averaging over fundamentally different source ecosystems. The number obscures more than it reveals. The[GEO KPIs guide](https://writesonic.com/blog/geo-kpis-every-brand-should-track) covers how to structure per-engine measurement so the picture doesn't get flattened.


The 3.8% universal citation rate is not a practical target. Building toward the thin consensus layer sounds appealing. In practice, 72 to 73% of AI citation decisions happen in engine-exclusive territory. That's where the real coverage gaps are.


For B2B teams, the[AEO for B2B playbook](https://writesonic.com/blog/aeo-for-b2b) goes deeper on citation-building per platform when your audience is distributed across multiple AI engines with different retrieval logic.


## Open Questions


A directional follow-up is in progress: of engine A's sources, what share also appear on engine B? That cut accounts for volume asymmetry and will sharpen the picture.


Exact-URL comparison will produce lower overlap scores than the domain-level numbers here. Domain-level is the more generous baseline.


Citation behavior shifts as models update. The[GPT-5.6 Sol citation study](https://writesonic.com/blog/gpt-5-6-sol-citation-study-vs-gpt-5-5) documented a significant retrieval change in a single version bump. The two-camp structure we see here may look different as these platforms continue to evolve.


## Methodology


**Engines studied:** ChatGPT, Gemini, Perplexity, Google AI Overviews


**Total prompts with citations from at least one engine:** 161,286


**Prompts with citations from all four engines simultaneously:** 70,879


**Metric:** Jaccard similarity: (domains cited by both engines) ÷ (all unique domains cited by either engine), computed per prompt then aggregated across all prompts in each pair


**Matching:** Domain level. URLs lowercased, "www." stripped. Lenient variant. Exact-URL matching would produce lower scores.


**Pair scope:** Each pair counted only on prompts where both engines returned citations. Per-pair prompt counts vary as a result.


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
