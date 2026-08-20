---
schema_version: "1.0.0"
document_id: "cf67a5011db34ba53c3d37c56df8b23ecccc2fd90e06d2869577444a33f1c9b5"
company_key: "yc-writesonic"
company: "Writesonic"
source_id: "yc-writesonic-news-import-0d409124f8fc"
canonical_url: "https://writesonic.com/blog/ai-search-brand-displacement-study"
published_at: "2026-07-23T05:48:47.301+00:00"
first_seen_at: "2026-07-23T20:47:38.558322+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:faddf1e6aac1da547acfb24d23d6a33bdcc6de4cf57153ca56a80ab1b9bf03a5"
---

# Who Loses When You Win in AI Search? A Study of 10.7 Million Answer Changes

In AI search, winning usually means someone else loses.


Across 10.7 million changes between repeated AI answers, **90% of the time a brand entered, at least one other brand left.**


Usually, the answer did not expand. It rotated.


We call this **brand displacement in AI search** : one brand appears while another disappears.


And more than one in four swaps were not between competitors at all. They involved a brand trading places with its own alias, product, or parent company.


So the real question is not just, **“How often do we appear?”**


It is, **“Who do we replace, and who keeps replacing us?”**


This study answers that.


## What we found


- **90% of brand entries happened alongside at least one brand exit.**
- **About 3.4 brands entered and 3.4 brands left per answer change.**
- **3.77 brands left when a new brand entered, compared with 1.44 when no new brand entered.** That is a 2.6x difference.
- **At least 26% of swaps involved a brand and its own alias, product, or parent company.**
- **The strongest rivalries worked in both directions.** The same pairs repeatedly replaced one another across answer reruns.


## How we tracked brand displacement


We repeatedly asked the same prompts and compared the brands named in one answer with the brands named in the next.


A simple example:


- The first answer names Brand A, Brand B, and Brand C.
- The next answer names Brand A, Brand C, and Brand D.
- Brand D entered. Brand B left.


We compared repeated answers to the same prompt and recorded which brands entered and left. This simplified example shows Brand D replacing Brand B.


Repeating this at scale reveals which brands most often trade the same place.


We used these additions and removals to build an add/drop matrix. Instead of starting with an assumed competitor list, the matrix allowed the answers themselves to reveal which brands were repeatedly treated as alternatives.


The study tracked citations and mentions separately.


A **citation** is a source link attached to an AI answer. A **mention** is a brand name included directly in the answer text.


This analysis focuses on changes in brand mentions.


The orange arrows mark brand mentions in the AI answer. The highlighted Guideflow source card is a citation.


## AI answers behave like a fixed-size shelf


Across the answer changes we studied, approximately **3.4 brands entered and 3.4 brands left.**


Approximately 3.4 brands entered and 3.4 brands left per answer change, suggesting that AI answers rotate a nearly fixed number of brand positions.


That near-equal exchange is the structural finding behind the study.


AI answers did not keep expanding to include every qualified brand. They rotated a limited set of names in and out.


This builds on our[AI search ranking stability study](https://writesonic.com/blog/ai-search-ranking-stability-study?utm_source=chatgpt.com) , which examined how answers change when the same questions are asked repeatedly.


A position in the answer may remain available. The brand occupying it may not.


The 90% result makes this competitive effect clear. When a brand entered an answer, at least one other brand left in nine out of ten observed cases.


But the difference becomes even clearer when we compare the average number of removals:


- **When at least one new brand entered:** 3.77 brands left on average.
- **When no new brand entered:** 1.44 brands left on average.


Brands were removed **2.6 times more frequently** when a new brand entered the answer.


When at least one new brand entered an AI answer, an average of 3.77 brands disappeared, compared with 1.44 when no new brand entered. That is a 2.6x difference.


This does not prove that every entering brand directly caused every specific removal. The data is observational.


What it does show is that brand entry and brand exit happen together far more often than in answer changes where no new brand appears.


That means an AI visibility report should answer more than:


How often does our brand appear?


It should also show:


- Which brands disappear when you enter
- Which brands enter when you disappear
- Which prompts produce the most frequent swaps
- Whether those changes happen across every platform or only one
- Whether the brand replacing you is a competitor or another name from your own company


A point-in-time visibility score cannot answer those questions. The[GEO KPIs every brand should track](https://writesonic.com/blog/geo-kpis-every-brand-should-track?utm_source=chatgpt.com) explains how prompt-level visibility, citations, and share of voice fit into a broader measurement framework.


## More than one in four swaps happen inside the same brand family


At least **26% of all add/drop events** involved a brand trading places with its own alias, product, or parent company.


At least 26% of observed add/drop events involved a brand trading places with its own alias, product, or parent company.


This can happen when one business is represented by several names:


- A company and its flagship product
- A parent company and an acquired brand
- A full company name and a common abbreviation
- A current brand name and a former name
- A company and a separately branded AI product


An AI system may treat each of these names as a separate candidate.


One answer may recommend the product. The next may replace it with the parent company. A standard competitor report records a gain and a loss, even though both names belong to the same organization.


That creates two problems.


First, raw visibility numbers can make the brand look more volatile than it really is.


Second, the wrong entity may receive the valuable mention. The company may technically be visible, but the name you want customers to remember may not be the one appearing.


Before treating every loss as a competitor win, audit the identity layer:


1. List every company name, product, alias, abbreviation, and former name that may appear.
2. Decide which entity should lead for each important topic or prompt category.
3. Track total brand-family visibility alongside visibility for each individual entity.
4. Review where related names repeatedly replace one another.
5. Keep naming consistent across owned content, profiles, structured data, and third-party descriptions.


The study does not prove that naming changes alone will eliminate self-displacement. It shows that internal identity swaps are common enough to investigate before building an external competitive response.


## Your real AI competitors are the brands that repeatedly replace you


Most competitor lists come from traditional sources: SEO rankings, paid search data, analyst reports, product categories, and sales conversations.


Those lists are useful. But they do not necessarily show which brands an AI system treats as substitutes.


The add/drop matrix does.


The strongest signal is a **two-way rivalry** :


- Brand A frequently replaces Brand B.
- Brand B frequently replaces Brand A.
- The same swap happens repeatedly across answer reruns.


When two brands keep replacing each other in both directions, they are competing for the same answer space.


In the AI assistant category, the most active pair in our data was **OpenAI and Gemini** , which traded places 4,184 times during the 30-day period.


Other frequent pairs included:


- ChatGPT and Anthropic: 3,342 swaps
- Claude and OpenAI: 2,806 swaps
- Anthropic and Gemini: 2,424 swaps
- Anthropic and Perplexity: 2,410 swaps


In the AI assistant category, these brand pairs most frequently replaced each other across repeated answers during the 30-day study. Frequent two-way swaps reveal which names AI systems repeatedly treat as alternatives for the same answer space.


These results do not mean the pairs shown are the closest traditional business competitors in every context.


They show that, within the answers observed, AI systems repeatedly treated those names as alternatives.


The same pattern can reveal unexpected competitors in any category.


Your most active AI rival may be a familiar market leader. It may also be:


- A smaller specialist
- An adjacent product
- A marketplace or directory
- A company that targets a different customer segment
- A brand that barely appears in your traditional SEO reports


The competitive set may also change by platform. Our[AI citation source overlap study](https://writesonic.com/blog/ai-citation-source-overlap-study?utm_source=chatgpt.com) found that AI platforms can rely on different source pools. The evidence and brands competing in one platform may not transfer cleanly to another.


Your real AI competitor is not simply the company that looks most similar to you.


It is the brand that repeatedly takes your place.


## What brands should do with this data


### Track repeated answers, not isolated screenshots


A screenshot tells you who appeared once.


It does not show whether the result is stable, whether it changes on the next run, or whether your brand is locked in a repeated swap with the same competitor.


Track the same high-value prompts over time. Measure which competitors repeatedly appear instead of you and which brands disappear when you enter.


### Build your AI competitor set from observed swaps


Start with the brands that most often:


- Enter when your brand disappears
- Disappear when your brand enters
- Swap with you in both directions
- Replace you across multiple related prompts


Then segment those swaps by platform, market, topic, and search intent.


Once you identify the brands that repeatedly replace you, add them to your tracking set. In Writesonic, you can organize competitors by market and tags, then compare their position and AI visibility over time.


Add the brands revealed by your displacement data to your Writesonic competitor set, then organize and compare them by market, tags, position, and AI visibility.


This creates an AI competitor set based on observed answer behavior, not assumptions.


### Separate external losses from internal identity swaps


Group related names for an executive-level view, but keep separate reporting for each product, alias, and parent company.


This helps answer two different questions:


1. Is the overall brand family visible?
2. Is the right entity receiving the mention?


Without that separation, a gain for a product and a loss for the parent company may look like external competitive volatility.


### Investigate why the other brand was selected


Once you identify a repeated rival, study what surrounds its appearances.


Compare:


- The sources cited when the competitor appears
- Third-party articles and communities mentioning the competitor
- The claims, statistics, and language associated with it
- The content formats supporting its inclusion
- Whether the same sources appear across several AI platforms
- Whether the competitor is tied more closely to a specific use case or category


Our[study of what drives AI visibility growth](https://writesonic.com/blog/what-drives-ai-visibility-growth-study) provides a broader view of the factors associated with sustained improvements in AI visibility.


Our research on[which sources influence brand visibility in AI answers](https://writesonic.com/blog/sources-ai-models-cite-brand-visibility?utm_source=chatgpt.com) can then help prioritize the publications, communities, and platforms most closely associated with brand inclusion.


## Find the prompts where competitors replace you


Knowing who replaces your brand is useful only if you can see where the swap happens and what may be driving it.


Writesonic’s[AI Visibility Tracker](https://writesonic.com/ai-visibility-tracker?utm_source=chatgpt.com) shows:


- Where your brand appears across major AI platforms
- Which competitors are being recommended instead
- The prompts behind each visibility gap
- The sources influencing those answers
- How your performance changes over time


You can compare visibility by platform, market, topic, and intent, then inspect the individual answers behind the numbers.


The Prompts view in Writesonic shows the exact queries where a brand appears, along with AI Visibility, rank, topic, market, and changes between tracking periods.


[Start a free trial](https://app.writesonic.com/signup) or[book a demo](https://writesonic.com/?demo=open&utm_source=chatgpt.com) to find the AI answers where competitors are taking the places your brand could own.


## Methodology


The study examined AI answer composition across ChatGPT, Gemini, Perplexity, Google AI Overviews, Google AI Mode, Copilot, Grok, and Claude during a recent 30-day window.


The broader dataset covered hundreds of millions of AI-answer citations across thousands of brands.


We repeatedly ran the same prompts, compared the brands named from one answer to the next, and recorded each addition and removal.


This produced 10.7 million observed brand-entry and brand-exit changes used to measure:


- How often brand entry happened alongside brand exit
- The average number of brands gained and lost
- The difference in removals when a new brand entered
- Swaps between related brand entities
- Brand pairs that repeatedly replaced one another


Customer-specific rivalry data was omitted for confidentiality.


The analysis is observational. It measures changes in answer composition, not clicks, conversions, or revenue. It also does not prove that every entering brand directly caused a specific brand to leave.


## Winning in AI search means knowing who you replace


A visibility score tells you whether your brand appeared.


Displacement data shows whose place you took and who takes yours.


Across 10.7 million answer changes, AI answers usually made room for a new brand by removing another. The strongest competitive relationships appeared in the pairs that repeatedly exchanged the same space.


But before looking outside the company, brands should check whether their own products, aliases, and parent names are competing with one another.


Once you know who takes your place, where it happens, and which version of your brand is being selected, AI visibility becomes a much more concrete competitive problem to solve.


###


###


###


###


###


[Samanyou Garg](https://writesonic.com/blog/author/samanyou-garg)


Founder @ Writesonic


Samanyou is the founder of Writesonic, a platform that helps you track & boost your brand’s visibility in AI search. Two years before the launch of ChatGPT, Writesonic was already at the forefront, helping organizations automate their entire marketing workflow through specialized AI agents for SEO and content. Samanyou is a Forbes 30 Under 30 awardee and a winner of the 2019 Global Undergraduate Awards, often referred to as the junior Nobel Prize.


[Ramish Jamal](https://writesonic.com/blog/author/ramish-jamal)


Engineering @ Writesonic


Ramish builds the infrastructure behind Writesonic's AI visibility platform, including the data systems that power the AI Ads Index. His work focuses on real-time tracking of paid and organic placements across AI answer surfaces.
