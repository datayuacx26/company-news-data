---
schema_version: "1.0.0"
document_id: "88ec60b246f1fb1d54b6d6e9a11d0be634b29542b0bf902421a9394b06dbfd9a"
company_key: "yc-writesonic"
company: "Writesonic"
source_id: "yc-writesonic-news-import-0d409124f8fc"
canonical_url: "https://writesonic.com/blog/gpt-5-6-sol-citation-study-vs-gpt-5-5"
published_at: "2026-07-16T13:35:09.371+00:00"
first_seen_at: "2026-07-22T20:38:42.288608+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:aa80d293863087b562bc414e52ae1b07f05e77d7bb6c35f2ef0513dc998b4a80"
---

# GPT-5.6 Sol Runs site: Queries on 71% of Its Searches. GPT-5.5 Did 11%.

Three months ago, our[GPT-5.5 citation study](https://writesonic.com/blog/gpt-5-5-citation-study-vs-gpt-5-4) documented the death of the site: operator. Domain-scoped searches fell from 40.5% of ChatGPT's fan-out queries to 12.6% in a single version bump, and brand pages lost their most direct route into AI answers.


GPT-5.6 Sol reverses that. Completely.


When ChatGPT swapped in the new model and replaced the Thinking toggle with a Medium/High effort picker, we reran our 50-prompt study against both tiers. Same prompts, same extraction method, effort held constant so every delta we report is a version effect, not a dial effect. On Medium, site: usage jumped from 12.6% to 59.3% of all searches. On High, it hit 71.2%. GPT-5.4, the previous record holder, peaked at 40.5%. No ChatGPT model has ever searched this way.


Brand citations, answer length, and content freshness all moved with it. This study covers all four shifts, plus a measurement trap in the new effort picker that will produce false headlines across the industry this quarter. Our data shows exactly where it hides.


## TLDR


- **site: is back, harder than ever.** GPT-5.6 Sol scopes 59.3% of its searches to specific domains at Medium effort (4.7x GPT-5.5's rate) and 71.2% at High (6.7x).
- **Brand citations rose 11 points at Medium.** First-party citation rate went from 47.2% to 58.1% on ChatGPT's paid default.
- **At High effort, the gain disappears.** 59.7% to 61.4%, within noise on a 50-prompt sample. GPT-5.5's High tier already cited brands at that rate. Any comparison that doesn't hold effort constant will manufacture a version gain that doesn't exist.
- **Cited pages are twice as fresh.** Median cited-page age fell from 88 to 52 days at Medium, and from 139 to 71 days at High.
- **Answers are longer.** +14% at Medium, +32% at High. Both tiers now converge around 670 words.
- **Google matters even less.** 89% of the domains GPT-5.6 Sol cites at Medium don't rank in Google's top 10 for the same query.


## How we did this


We ran the same 50 prompts through four configurations: GPT-5.5 at Medium, GPT-5.5 at High, GPT-5.6 Sol at Medium, GPT-5.6 Sol at High. The prompts cover 16 categories, from B2B SaaS and finance to food, legal, travel, and fitness.


Effort is the control variable, and controlling it is what separates this study from the comparisons you'll see elsewhere. Both Medium runs used` thinking_effort=standard` , both High runs used` thinking_effort=extended` , verified on every conversation via` model_slug` . When we say GPT-5.6 Sol changed something, the model changed it. Not the dial.


After each response, we pulled the full conversation payload from ChatGPT's` /backend-api/conversation/<id>` endpoint: every fan-out query the model ran, every web result it read, every URL it cited in the final answer. It's the same fan-out and citation chain our[AI visibility tracker](https://writesonic.com/ai-visibility-tracker) captures at scale, extracted manually here so every number is auditable. Claude Haiku 4.5 classified each cited URL as first-party (the brand being asked about) or third-party (review sites, blogs, retailers, media), with 50+ examples in the prompt. The classifier reproduced our published GPT-5.4 measurement within a point.


We also cross-referenced 30 of the prompts against Google US and Bing US top-10 results via SerpAPI.


One run per prompt per configuration, one ChatGPT Plus account, US. Directional by design. Treat any single-run study, including this one, as a map rather than a measurement to the third decimal.


## The site: explosion is the defining change of this release


GPT-5.6 Sol crossed GPT-5.4's old peak at both effort tiers. GPT-5.5 never got close.


GPT-5.5 used the site: operator on roughly 1 in 8 searches, whichever tier you picked. GPT-5.6 Sol uses it on 6 in 10 at Medium and 7 in 10 at High.


The effort dial explains none of this. Both 5.5 tiers sit at 10-13%. Both 5.6 tiers sit at 59-71%. This is a version-level rewrite of how the model constructs queries, and it's the largest single change we've measured across four studies of this model family.


Watch what it does with a real brand. When a prompt touched accounting software, GPT-5.6 Sol at High effort ran twelve separate queries scoped to quickbooks.intuit.com alone: pricing, plan comparisons, feature pages, support docs, one keyword at a time. Roam Research drew nine. Shopify drew eight. The model isn't browsing the web for answers about these brands. It's interrogating their websites, page by page.


That flips the operative question for every brand. It's no longer "does ChatGPT like my content?" It's "when ChatGPT runs` site:mybrand.com pricing` , does anything come back?"


**What to do:** Run that exact search on Google today, with your own domain and your three most commercial keywords. Empty, thin, or wrong-page results mean you're invisible to the majority of GPT-5.6 Sol's search activity on both paid tiers. The fix is technical SEO: indexation, crawlability, and real content in plain HTML on your pricing and product pages.


*Dig deeper:[GPT-5.4's site: operator and what it changed for AEO](https://writesonic.com/blog/chatgpt-citation-study-gpt-5-4-vs-gpt-5-3)*


## On the default tier, brand citations jumped 11 points


GPT-5.6 Sol at Medium effort: same search volume, 17% more reading, fewer but more brand-heavy citations.


All those site: queries land somewhere. On Medium, the current paid default, 58 of every 100 cited URLs now point at a brand-owned page, up from 47 three months ago.


The flat numbers tell you as much as the moving ones. Fan-out count didn't change; the model isn't searching more. It reads 17% more results and cites slightly fewer of them, 6.8 per answer instead of 7.2. Same effort, more reading, tighter selection, and a much larger share of the winners are brand pages.


The gains cluster where GPT-5.5 was weakest. Services prompts went from 22% first-party to 79%. Legal jumped 50 points, Home 49, Food 35, Productivity 33, Healthcare 26, Marketing 23. Categories that were already near ceiling gave some ground: Comparison dropped 26 points, Trends 22, Fitness 18.


**What to do:** If you're in services, legal, home, food, productivity, healthcare, or marketing, your odds of a direct citation on ChatGPT's default just improved more in one release than in the previous three combined. Audit your core prompts against the current model this month. Last quarter's visibility numbers describe a model that no longer exists.


## The effort picker is a measurement trap, and it will produce false headlines this quarter


Same 50 prompts, effort held constant. The version gain is real at Medium and noise at High.


Here's the comparison most teams will get wrong, and why our numbers won't match the ones you'll see elsewhere.


GPT-5.6 Sol at High effort cites brands on 61.4% of URLs. Set that against the 47.2% from the old default and you get a 14-point version gain, ready-made for a headline. It's wrong. The old default maps to today's Medium tier, and GPT-5.5 at High effort was already citing brands at 59.7%. Hold effort constant and the High-tier version gain is 1.7 points, which on 50 prompts is noise. The 14-point story measures the dial, not the model.


This is exactly why we run both effort tiers on both models. Effort was already doing most of the brand-citation work on GPT-5.5; the version change adds real gains at Medium and almost nothing at High.


The category data at High confirms it. GPT-5.5 High beats the new model in Fitness (by 29 points), Services, Education, Legal, and Marketing. GPT-5.6 Sol wins Productivity, Home, Food, Ecommerce, and Comparison. B2B SaaS sits at 91% on both. At High effort the two models aren't better or worse for brands. They're differently shaped.


**What to do:** Every model comparison you read this year, apply one filter first: was the reasoning tier held constant? If the methodology doesn't say, the finding is measuring a toggle. Benchmark your own visibility per tier, not on a blended average.


*Dig deeper:[how GPT-5.5's free Instant tier runs a completely different citation economy](https://writesonic.com/blog/gpt-5-5-instant-citation-study)*


## The pages it cites are half as old


Median cited-page age dropped 41% at Medium and 49% at High in one version bump.


The median page GPT-5.6 Sol cites at Medium effort is 52 days old. On GPT-5.5 it was 88. At High effort the drop is steeper still, 139 days down to 71.


The shift happens at the fresh end of the distribution, not the stale end. Citations under 30 days old went from 28% to 42% at Medium while the over-a-year bucket barely moved. The model isn't purging old pages. It's actively seeking new ones.


Full transparency on the data: publication dates were extractable for 14% of cited URLs on 5.6 Sol Medium and 5-9% on the High tiers, so these figures are directional, drawn from pages that expose a date. The direction is identical on both tiers, and it isn't subtle.


**What to do:** Put visible, machine-readable dates on your pages, then earn them. A pricing page last touched in 2024 now competes against a measurable freshness preference on both paid ChatGPT tiers. Set a real update cadence for your core commercial pages, with changes substantive enough to justify the new date.


Want to see which of your pages ChatGPT can actually reach, and how old they look to it?[Book a demo](https://writesonic.com/?demo=open) and we'll run your domain through the same citation analysis this study is built on.


## Answers converged at 670 words, and that changes how citations work


GPT-5.5's odd trade-off, more effort but shorter answers, is gone. Both new tiers write ~670 words.


GPT-5.5 carried a quirk we flagged at the time: High effort produced shorter answers than Medium. The extra compute went into searching, with fan-outs nearly doubling, while the writing got tighter.


GPT-5.6 Sol ends that trade-off. Both tiers now land around 660-670 words, 14% longer than 5.5 Medium and 32% longer than 5.5 High. Citation counts held at 7-8 per answer, so each citation now lives inside more prose.


For brands, that's a framing shift. Your mention is less likely to be a line item in a short list and more likely to be a clause inside a long explanation. Getting cited is step one. What the surrounding 670 words say about you is the metric to watch next, and it's why we track sentiment alongside citations rather than citations alone.


## And it drifted even further from Google


The share of cited domains absent from Google and Bing's top 10 rose from 84% to 89% at Medium effort, and from 86% to 88% at High.


This is a direct consequence of the site: behavior. When the model forces` site:hubspot.com pricing` , it lands on a page that will never rank in Google's top 10 for "best CRM for B2B SaaS." It doesn't need to. It matched the forced query exactly.


So the overlap keeps shrinking. Only 10-11% of the domains GPT-5.6 Sol cites rank in Google's top 10 for the same prompt. A rank tracker sees a coincidence, not a cause. Every version since GPT-5.3 has widened this gap, and this release widened it the most.


*Dig deeper:[which sources AI models actually cite, across every major engine](https://writesonic.com/blog/sources-ai-models-cite-brand-visibility)*


## What this means for your next quarter


**Make site: accessibility your first AEO investment.** With 59-71% of ChatGPT's paid-tier searches scoped to specific domains, whether your pages surface under` site:yourbrand.com <keyword>` now decides most of your citation odds. Indexation, crawlability, and plain-HTML content on pricing, product, and support pages come before any new content play.


**Judge every model shift per category.** This one release handed +57 points to Services and took 26 from Comparison prompts. An aggregate number tells you almost nothing about your prompts.


**Treat freshness as a ranking input, because it is one now.** Median cited-page age dropped by half in a single version bump. Dated, genuinely updated pages beat stale ones on both paid tiers.


**And hold every model comparison to the effort-constant standard.** The 14-point false gain documented above is sitting in plain sight for anyone who compares across tiers. Expect to see it published as fact. Now you'll know what you're looking at.


## Questions we're still investigating


1. Does the site: behavior hold on Enterprise and Team accounts, or is it a Plus pattern?
2. What does GPT-5.6 do on the free Instant tier? Our[Instant study](https://writesonic.com/blog/gpt-5-5-instant-citation-study) found a completely different citation economy there, and it's unmeasured on the new model.
3. Is the freshness preference a retrieval change or a re-ranking change? Low publication-date coverage makes this hard to isolate.
4. How stable are the per-category swings across repeat runs? Single-run cells are small, and we're re-running the biggest movers now.
5. Do 670-word answers change how often users click any citation at all?


## Methodology


50 prompts across 16 categories, run once per configuration between April and July 2026 from a single ChatGPT Plus account in the US. Four configurations: GPT-5.5 and GPT-5.6 Sol, each at Medium (` thinking_effort=standard` ) and High (` thinking_effort=extended` ), verified via` model_slug` on every conversation. Conversation payloads pulled from ChatGPT's` /backend-api/conversation/<id>` endpoint with browser-session auth. Each (prompt, URL) pair classified independently as first-party or third-party by Claude Haiku 4.5 with 50+ in-prompt examples, calibrated against our previously published GPT-5.4 first-party rate. SerpAPI cross-reference against Google US and Bing US on 30 of the 50 prompts.


Limitations: single user, single run per prompt, single point in time. ChatGPT is non-deterministic, and repeat runs would shift individual numbers. Freshness figures draw from the 5-14% of citations exposing publication dates and should be read as directional.


Four studies in, one pattern holds: every model swap redraws the citation map, and the redraw takes weeks, not quarters. The brands that caught the GPT-5.4 site: era early built an advantage the 5.5 release erased overnight. This release just handed out the same head start again, and it will expire the same way. Start tracking your citation data at[https://app.writesonic.com/signup](https://app.writesonic.com/signup) , or[book a demo](https://writesonic.com/?demo=open) and we'll show you your brand's site: accessibility gap before the next version bump resets the board.


###


###


###


###


[Samanyou Garg](https://writesonic.com/blog/author/samanyou-garg)


Founder @ Writesonic


Samanyou is the founder of Writesonic, a platform that helps you track & boost your brand’s visibility in AI search. Two years before the launch of ChatGPT, Writesonic was already at the forefront, helping organizations automate their entire marketing workflow through specialized AI agents for SEO and content. Samanyou is a Forbes 30 Under 30 awardee and a winner of the 2019 Global Undergraduate Awards, often referred to as the junior Nobel Prize.
