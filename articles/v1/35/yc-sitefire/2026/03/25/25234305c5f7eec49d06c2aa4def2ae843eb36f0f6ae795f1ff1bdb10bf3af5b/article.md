---
schema_version: "1.0.0"
document_id: "25234305c5f7eec49d06c2aa4def2ae843eb36f0f6ae795f1ff1bdb10bf3af5b"
company_key: "yc-sitefire"
company: "sitefire"
source_id: "yc-sitefire-news-import-97a7181f264f"
canonical_url: "https://sitefire.ai/blog/ai-content-quality-shift"
published_at: "2026-03-19T23:00:00+00:00"
first_seen_at: "2026-07-24T00:58:46.205658+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:53c15ee9509ab6a6aa4bb331fde78102cce3fdd64a85006bf6680ba526a19eab"
---

# AI Content Is Good Enough - But Good Enough for What?

AI content can rank in Google. But 25M+ citation data reveals AI search engines reward originality, not production quality. Here's what actually gets cited.


Ahrefs' Director of Content Marketing, Ryan Law, recently argued that AI-generated content has[crossed the quality threshold](https://ahrefs.com/blog/ai-content-wasnt-good-enough-now-it-is/) . Writing, he says, is "simpler and more mechanical than most people assume." Content marketing is "intensely formulaic." And the tooling has matured to the point where there is "no trade-off" between AI and human output.


He is right. But he is asking the wrong question.


Law's argument rests on authority - his 13 years of content experience and a senior role at Ahrefs, but lacks proof. No traffic comparisons. No ranking experiments. No blind tests. For a company that sells an analytics platform, the absence of evidence is notable.


The question "is AI content good enough?" has been answered. The data is clear. But "good enough" for what, exactly? Good enough for Google rankings in 2024? Or good enough for the way people are actually discovering products in 2026?


That distinction matters. And the data tells a very different story.


## The quality debate is settled


AI content can rank in Google. This is no longer a debate - it is a measured fact.


[Semrush analyzed 20,000 URLs](https://www.semrush.com/content-hub/can-ai-content-rank-on-google/) ranking in Google's top 20 and found that **57% of AI-generated content appeared in the top 10** , compared to 58% for human-written content. The difference is statistically negligible.


Can humans tell the difference? Barely. A[CISPA/ACM study](https://cacm.acm.org/research/as-good-as-a-coin-toss-human-detection-of-ai-generated-content/) with 1,276 participants found that people detect AI-generated content with **51.2% accuracy** - statistically equivalent to flipping a coin. The bias against AI content is real, but it is driven by labeling, not by detectable quality differences. When content is labeled "AI-generated," preference drops by 30%. When the label is removed, the distinction vanishes.


And the volume is already enormous.[Ahrefs' own data](https://ahrefs.com/blog/what-percentage-of-new-content-is-ai-generated/) shows that **74.2% of new web pages** now contain AI-generated content. Only 2.5% are "pure AI" - most are AI-assisted with human editing. But the baseline has shifted. Production quality is no longer a differentiator. It is table stakes.


Ryan Law is right that AI content is good enough to rank. The question is whether ranking in Google is still the finish line.


## When everyone is "good enough," good enough gets you nowhere


The same month that Ahrefs was celebrating AI content quality,[Graphite published a study](https://graphite.io/five-percent/more-articles-are-now-created-by-ai-than-humans) of 65,000 CommonCrawl URLs showing that AI-generated articles surpassed human-written articles in volume for the first time in November 2024. The web reached a rough 50/50 split - and then plateaued.


But here is the part that Graphite's researchers noted and the content marketing industry has largely ignored: these AI articles **"largely do not appear in Google or ChatGPT search results."**


Half the web is now AI-generated. And most of it is invisible.


Google's response has been consistent and escalating. The evolution from "AI content is spam" (2022) to["we focus on quality, not how content is produced"](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content) (February 2023) was not a loosening of standards. It was a reframing. Google stopped asking *who* wrote the content and started asking *whether it adds value* .


The enforcement has been methodical:


Date Google Action


March 2024 "Scaled content abuse" policy introduced. 40% reduction target for low-quality content.


January 2025 16,000 quality raters now specifically assess AI-generated content.


December 2025 Core update: **87% negative impact rate** for mass-produced AI content without editorial oversight.


March 2026 Core update rolling out. Targeting scaled AI content and parasitic SEO.


The bar is not quality. It is value. Google does not penalize AI content for being AI content. It penalizes content that adds nothing new to the web - regardless of who or what produced it.


## AI search uses the same index - but a much wider surface area


This is where Ryan Law's framing breaks down. Not because AI search engines use different ranking signals. They don't. The web index is the same. The ranking signals - domain authority, relevance, freshness, backlinks - are the same. What changed is how queries are generated.


The mechanism is called[query fan-out](https://sitefire.ai/blog/query-fan-out) . When a user types "best CRM for small business" into ChatGPT or Google AI Mode, the AI does not run one search. It rewrites the query into 5 to 20 sub-queries - "CRM pricing for small teams under 50 employees," "HubSpot vs Salesforce vs Pipedrive small business," "CRM with best free tier for startups" - and searches each one independently against the same web index. The results are merged via Reciprocal Rank Fusion (RRF), and the AI synthesizes one answer from the combined pool.


One query in, many searches out. Same ranking signals, but a much wider surface area.


This explains a stat that seems paradoxical at first. According to[Search Engine Land's analysis of 8,000 AI citations](https://searchengineland.com/how-to-get-cited-by-ai-seo-insights-from-8000-ai-citations-455284) , only **8-12% of ChatGPT citations match pages in Google's top 10 results** for the head term. A[Surfer SEO analysis](https://surferseo.com/blog/query-fan-out-impact/) of 173,902 URLs found that **68% of pages cited in AI Overviews were not in the top 10 organic results** . Not because AI ranks pages differently - but because AI searches for different, more specific queries. Those pages rank well for the sub-queries, not the head term.


This is why keyword stuffing for the head term hurts. The[GEO paper](https://arxiv.org/abs/2311.09735) from Princeton, Georgia Tech, and the Allen Institute for AI (KDD 2024) tested nine optimization strategies across 10,000 queries:


Strategy Visibility Impact


Adding quotations from credible sources **+41%**


Adding statistics **+33%**


Fluency optimization +29%


Citing sources +28%


Keyword stuffing **-9% (harmful)**


Content stuffed with the head keyword doesn't match the specific sub-queries AI generates. Content with statistics, citations, and specific claims does - because it naturally covers the nuances that sub-queries target.


[Semrush confirmed this at scale](https://www.semrush.com/blog/content-optimization-ai-search-study/) . Analyzing 304,805 cited URLs against 921,614 non-cited URLs across ChatGPT, Google AI Mode, and Perplexity, they found that content cited by AI search engines scores significantly higher on:


-


**Clarity and summarization:** +32.83%


-


**E-E-A-T signals:** +30.64%


-


**Q&A format:** +25.45%


-


**Section structure:** +22.91%


The ranking game hasn't changed. The query surface has. And content that covers the fan-out space - the full cluster of sub-queries AI generates from a single prompt - wins.


## What 25 million citations tell us


At Sitefire, we track AI visibility across ChatGPT, Gemini, Perplexity, Google AI Mode, AI Overviews, and DeepSeek. We analyzed 1.7 million AI evaluations across these six models, covering more than 25 million citations and 300,000 unique domains.


### AI search is genuinely long-tail


The most surprising finding is how distributed AI citations are. **72% of all cited domains appear in five or fewer AI answers.** Only 1,302 domains (0.4% of the total) are cited in 1,000 or more answers.


Domain tier Domains % of total


Cited in 1 answer only 129,904 43.3%


Cited in 2-5 answers 86,571 28.9%


Cited in 6-20 answers 46,120 15.4%


Cited in 21-100 answers 25,885 8.6%


Cited in 101-1,000 answers 10,013 3.3%


Cited in 1,000+ answers 1,302 0.4%


This is not a winner-take-all game. **68% of all AI citations go to domains outside the top 100.** More than half (54.5%) go to domains outside the top 500. There is real room for brands that produce content with substance.


### Your own website is the primary citation source


Corporate content accounts for **63% of all citations** across AI models. Editorial sites contribute 20%, UGC (Reddit, forums) accounts for 7%, and institutional and reference sources make up the remainder.


But the mix shifts by model:


Source type ChatGPT Perplexity Gemini Google AI Mode AI Overviews


Corporate 56.8% 64.8% 60.1% 66.9% 65.1%


Editorial 24.6% 22.0% 21.3% 16.3% 18.8%


UGC 2.8% 4.4% 6.5% 7.9% 9.0%


Institutional 8.3% 5.7% 4.2% 4.8% 3.9%


Reference 7.2% 3.0% 5.5% 4.0% 3.2%


ChatGPT relies more on editorial and reference sources (Wikipedia, institutional pages). Google AI Mode and AI Overviews lean heavier on UGC and corporate content. Optimizing for one model means missing the majority of AI search.


### Each model cites a different number of sources


The depth of sourcing also varies across models. This is data we track continuously at Sitefire across all customer evaluations:


Model Avg. sources per answer


Google AI Mode 15.65


Google AI Overviews 10.70


Gemini 9.70


Perplexity 6.95


ChatGPT 4.13


Google AI Mode cites nearly **four times more sources per answer** than ChatGPT. Google's AI features create more opportunities for your content to be cited - but you are also competing against more sources for attention within each answer.


**Takeaway:** Each model has different source preferences, different citation depths, and different biases toward content types. A multi-model strategy is not optional - and that requires tracking AI visibility across models, not guessing based on one.


## The real distinction: derivative vs original


The data points in one direction. AI search engines do not care whether content was written by a human or an AI. They care whether it adds information to the web - or reshuffles what already exists.


This is the distinction Ryan Law's article misses entirely. His argument is about production quality. But production quality is not the bottleneck. The bottleneck is substance.


**Derivative content fails in AI search.** Syndicated press releases account for[0.04% of all AI citations](https://almcorp.com/blog/ai-search-press-release-citations/) across 4 million citations analyzed by ALM Corp. AI engines cross-reference multiple sources before citing. Content that merely rephrases existing information gets filtered naturally because the AI already has the original.


Original research and data tell a different story. The GEO paper found that lower-ranked websites benefit most from optimization - sites ranked 5th saw a **+115% visibility improvement** when adding citations and statistics. Not because lower-ranked sites have better SEO. Because they often have original data, niche expertise, or specific case studies that the AI cannot find elsewhere.


The format is secondary to this. Whether your content is a 3,000-word guide or a concise FAQ matters less than whether it contains data points or expert perspective that exist nowhere else on the web. AI engines are not impressed by word count. They reward novel, verifiable claims backed by evidence.


This creates a paradox that many content teams have not yet internalized. AI tools are excellent at producing text. They are incapable of producing original information. A brand's competitive advantage in AI search comes not from how its content is written, but from what the content contains - proprietary data, unique expertise, customer-validated insights, and specific positioning.


## What "providing the right context" means in practice


If the distinction is derivative vs original, what does that look like when you sit down to create content?


It starts with understanding the landscape. Which prompts matter in your space? What competitors are getting cited? What content structure do AI engines prefer for your category? This is the intelligence layer - the strategic foundation that determines what to create. It is what we build at Sitefire when onboarding a new customer: mapping the citation landscape before writing a single word.


Then it requires packaging real brand knowledge in formats that AI can extract and cite. Not asking ChatGPT to "write a blog post about X." That produces derivative content by definition - it can only reshape what it already knows.


Instead, it means grounding content in what makes a brand distinct: proprietary product data, real customer outcomes, specific methodology, earned expertise, and authentic positioning. It means maintaining a custom brand context - voice, tone, terminology, and competitive differentiation - so that every piece of content reflects the brand's actual authority rather than a generic synthesis of the topic.


Content created this way uses AI as a production tool, but the raw ingredients - the context - come from the brand. The AI structures, clarifies, and formats. The brand provides the substance worth citing.


This is what separates AI-assisted content that earns citations from AI-generated content that adds to the noise. The tool is the same. The input is the difference.


## Frequently Asked Questions


### Does Google penalize AI-generated content?


No. Google's official position since February 2023 is that they evaluate content quality regardless of origin. However, mass-produced AI content without editorial oversight faces increasing enforcement under "scaled content abuse" policies. The December 2025 core update had an 87% negative impact rate on such content.


### Can AI content rank in Google?


Yes. Semrush's study of 20,000 URLs found AI content appears in Google's top 10 at nearly the same rate as human content (57% vs 58%). The quality bar for ranking has been cleared. The question is whether ranking in Google alone is sufficient as AI search grows.


### How is AI search different from Google for content?


AI search engines use the same web index and ranking signals as Google, but they decompose each user query into 5-20 sub-queries via query fan-out. This means your content competes on a much wider surface of specific, long-tail searches. Only 8-12% of ChatGPT citations match Google's top 10 for the head term, because the cited pages rank for the sub-queries instead.


### What type of content gets cited by AI search engines?


Content with original data, statistics, credible citations, and clear structure. The GEO paper found that adding statistics improves AI visibility by 33% and adding quotations by 41%. Corporate content (brand-owned websites) accounts for 63% of all AI citations across six major models, based on Sitefire's analysis of 25 million citations.


### Is it worth creating content specifically for AI search?


Yes, but not by producing generic AI-written articles at scale. The winning strategy is creating content grounded in unique brand expertise and data - content that provides information AI models cannot find elsewhere. Sitefire's data shows 72% of cited domains appear in five or fewer AI answers, confirming there is real opportunity for brands with original substance.


## Key Takeaways


-


AI content quality has crossed the bar for Google rankings. Semrush data shows near-identical ranking rates for AI and human content. This debate is over.


-


Production quality is now table stakes. 74% of new web pages contain AI content, but most AI-generated articles do not appear in search results.


-


AI search engines use the same index and ranking signals as Google, but search a much wider surface area. Query fan-out generates 5-20 sub-queries per prompt, and 68% of AI-cited pages are not in the top 10 for the head term.


-


AI search is genuinely long-tail. 72% of cited domains appear in five or fewer answers, and the top 100 most-cited domains capture less than a third of all AI citations.


-


Each AI model plays by different rules. Google AI Mode cites 15.65 sources per answer; ChatGPT cites 4.13. Source type preferences vary across models.


-


The distinction that matters is not human vs AI - it is derivative vs original. Content that adds new information to the web earns citations. Content that reshuffles existing information does not.


-


Brands winning in AI search are providing the right context - proprietary data, real expertise, and authentic positioning - for AI to say something worth citing.


## Sources


-


Ryan Law. "AI content wasn't good enough, now it is." Ahrefs Blog, March 2026.[https://ahrefs.com/blog/ai-content-wasnt-good-enough-now-it-is/](https://ahrefs.com/blog/ai-content-wasnt-good-enough-now-it-is/)


-


Semrush. "Can AI Content Rank on Google? We Analyzed 20K Blog URLs." 2025.[https://www.semrush.com/content-hub/can-ai-content-rank-on-google/](https://www.semrush.com/content-hub/can-ai-content-rank-on-google/)


-


Di Cooke, Abigail Edwards, Sophia Barkoff, Kathryn Kelly. "As Good as a Coin Toss: Human Detection of AI-Generated Content." Communications of the ACM, September 2025.[https://cacm.acm.org/research/as-good-as-a-coin-toss-human-detection-of-ai-generated-content/](https://cacm.acm.org/research/as-good-as-a-coin-toss-human-detection-of-ai-generated-content/)


-


Ahrefs. "74% of New Webpages Include AI Content." April 2025.[https://ahrefs.com/blog/what-percentage-of-new-content-is-ai-generated/](https://ahrefs.com/blog/what-percentage-of-new-content-is-ai-generated/)


-


Graphite. "More Articles Are Now Created by AI Than Humans." 2025.[https://graphite.io/five-percent/more-articles-are-now-created-by-ai-than-humans](https://graphite.io/five-percent/more-articles-are-now-created-by-ai-than-humans)


-


Aggarwal, Murahari, Rajpurohit, Kalyan, Narasimhan, Deshpande. "GEO: Generative Engine Optimization." KDD 2024.[https://arxiv.org/abs/2311.09735](https://arxiv.org/abs/2311.09735)


-


Semrush. "Content Optimization for AI Search: An 11,882-Prompt Study." 2025.[https://www.semrush.com/blog/content-optimization-ai-search-study/](https://www.semrush.com/blog/content-optimization-ai-search-study/)


-


James Allen. "How to get cited by AI: SEO insights from 8,000 AI citations." Search Engine Land, May 2025.[https://searchengineland.com/how-to-get-cited-by-ai-seo-insights-from-8000-ai-citations-455284](https://searchengineland.com/how-to-get-cited-by-ai-seo-insights-from-8000-ai-citations-455284)


-


ALM Corp. "AI Search Press Release Citations." 2025.[https://almcorp.com/blog/ai-search-press-release-citations/](https://almcorp.com/blog/ai-search-press-release-citations/)


-


Google. "Google Search's guidance about AI-generated content." Updated December 2025.[https://developers.google.com/search/docs/fundamentals/using-gen-ai-content](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content)


-


Surfer SEO. "Query Fan-Out Impact Study." 2025.[https://surferseo.com/blog/query-fan-out-impact/](https://surferseo.com/blog/query-fan-out-impact/)


-


Jochen Madler. "Query Fan-Out: What AI Search Actually Does With Your Query." Sitefire Blog, March 2026.[https://sitefire.ai/blog/query-fan-out](https://sitefire.ai/blog/query-fan-out)
