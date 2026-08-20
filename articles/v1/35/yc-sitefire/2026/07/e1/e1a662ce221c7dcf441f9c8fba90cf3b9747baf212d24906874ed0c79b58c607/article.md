---
schema_version: "1.0.0"
document_id: "e1a662ce221c7dcf441f9c8fba90cf3b9747baf212d24906874ed0c79b58c607"
company_key: "yc-sitefire"
company: "sitefire"
source_id: "yc-sitefire-news-import-97a7181f264f"
canonical_url: "https://sitefire.ai/blog/ai-engines-cite-different-sources"
published_at: "2026-07-21T22:00:00+00:00"
first_seen_at: "2026-07-24T00:58:46.205658+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:6127e1f90f10b686ad2513aedf60215f6f3b65cb5e39691800d84cb873327657"
---

# Why AI Models Recommend the Same Brands but Cite Different Sources

Ask ChatGPT, Gemini and Google AI Mode the same commercial prompt on the same day, and they mention about a third of the same brands. On the web pages they cite to get there, they agree on fewer than one in ten.


We measured this across 650,545 AI answers, then looked at what kinds of websites those citations came from. The models are not reading separate corners of the web. They read the same web and weight it differently. Where they agree least is the one type of site you control: your own.


This matters because most AI visibility budgets are still spent as if AI search were one channel. It is three, and a win on one buys you about a third of a win on the next.


## Is AI visibility one channel or three?


AI visibility is not one channel. For the same prompt on the same day, the three largest AI models share roughly 32% of the brands they mention and 8% of the web pages they cite.


We took every prompt that ran on ChatGPT, Gemini and Google AI Mode in the same window and paired the answers by prompt and by day. Then we measured two overlaps: the brands both models mentioned, and the web pages both models cited. Each is a share of everything either model named.


Brand agreement is 30% for ChatGPT and Gemini, 31% for ChatGPT and AI Mode, and 36% for Gemini and AI Mode. Source agreement is 8%, 6% and 10%.


Two things stand out. Gemini and AI Mode agree most with each other, which makes sense because both are Google and share the same search infrastructure. So the better framing is *ChatGPT versus Google's models* rather than three independent systems. Second, and more usefully: brand mentions agree about four times more than cited web pages, on every pair.


### The average hides how often they share nothing


The 8% is an average of two very different situations. **Most of the time, two models answering the same prompt on the same day cite no web page in common at all.** That happens 60% of the time for ChatGPT and Gemini, 52% for ChatGPT and AI Mode, and 32% for Gemini and AI Mode.


One prompt makes it concrete. Asked *"what do teams actually use to convert handwritten notes to searchable text"* , ChatGPT cited reddit.com, techradar.com, support.microsoft.com and notion.com. Gemini cited guideflow.com, preocr.io, lytewriter.com and suparse.com. Not one page in common, on the same day.


The counter-example: an ultra-niche prompt about tennis-court dome costs returned the same five vendor sites in both models. **When the web offers only a handful of relevant pages, the models land on the same ones.** That is not a footnote, it is the mechanism behind every number in this article.


## Are the AI models reading different parts of the web?


No. All three models take 46-57% of their citations from company websites. The differences sit in everything else, and they are consistent.


Sitefire classifies every cited domain by source type, so we grouped them into six categories and worked out each model's mix. Each bar is 100% of that model's own citations, so the comparison is not distorted by how much each model cites.


AI model Company sites Editorial Institutional Forums Reference Other


ChatGPT 46% 14% 12% 6% 7% 17%


Gemini 57% 10% 5% 7% 2% 19%


Google AI Mode 55% 10% 4% 11% 2% 18%


ChatGPT cites institutional sources roughly 2.5x more often than either Google model, and leads on editorial and reference too. Google AI Mode cites forums about twice as often as ChatGPT. Gemini relies most of all on company websites.


Before you ask whether this is a volume effect: it is not, and measuring each AI model against its own total is what rules it out. AI Mode cites **10.5** web pages per answer, Gemini 3.1 and ChatGPT 2.7. Over the window AI Mode drew on roughly 118,000 different websites, Gemini 44,000 and ChatGPT 34,000. Counting raw citations would show AI Mode dominating every category regardless of what it actually favours.


## Does the source type explain the disagreement?


No. Source type predicts agreement but does not explain it. Within company websites, agreement between two AI models is still only 10.6%.


The obvious inference from the section above is that different category weights are what produce different sources. It is wrong. If the models simply favoured different categories, agreement *inside* a category would be high. So we ran the same overlap measure within each category.


Source category Agreement within the category Share of all citations


Reference 44.4% 2-7%


Institutional 28.0% 4-12%


Forums 27.1% 6-11%


Editorial 15.2% 10-14%


Company sites **10.6%** **46-57%**


Other 5.2% 17-19%


All categories together 7.6%


Agreement runs cleanly from 44% down to 11%, so the type of source does predict how much two models will converge. But the category that supplies roughly half of all citations, company websites, is also one of the two they agree on least. Inside it, agreement is 10.6% against 7.6% across all categories.


> Publishing across more source categories will not make the models agree with each other. They disagree most on company websites, which is also the one place brands can control.


## Why the gradient exists


Supply, not preference. There is one Wikipedia, one Reddit and a handful of standards bodies. There are tens of thousands of company websites that could reasonably answer the same prompt.


On reference and institutional sources the models have almost no room to differ. If an answer needs an authoritative definition, there are two or three places to get it, and every model goes to the same one. That is why reference agreement is 44%.


On company websites they have almost unlimited room, and they use it. AI Mode alone cited more than 118,000 different websites in seven weeks. When a model has that many reasonable options and cites about ten per answer, the odds that a second model picks the same ten are small.


This is what the tennis-court example was showing. Agreement is not a sign that the models think alike. It is a sign that the web ran out of options.


So brands compete in the half of the web where the models disagree most. That is how the category works, not a failure of your content.


## Is any of this stable enough to plan against?


Partly. Google AI Mode's source mix moved less than 1.5 percentage points on any category across seven weeks. ChatGPT's moved up to 8.5.


Everything above is an average over seven weeks, so the fair next question is whether these are lasting habits or a moment in time. We recalculated the mix week by week and plotted each category against that model's own average, so the panels compare movement rather than level.


The difference is stark. AI Mode's six categories sit flat for the entire window. ChatGPT's fan out: company websites fell from 48.3% to 39.8%, institutional sources rose from 8.4% to 16.9%, and editorial rose alongside them. Gemini sits between the two.


One note on timing. ChatGPT's shift falls in the same weeks OpenAI shipped GPT-5.6. We cannot see which model version served any given answer, so we cannot connect the two, and we are not going to imply it.


What holds regardless is the ranking. Every comparison in this article is true in all seven weeks on its own: ChatGPT is lowest on company sites and highest on institutional every week, AI Mode highest on forums every week. The levels move; the ranking does not.


The practical read: work aimed at Google AI Mode has a longer shelf life than work aimed at ChatGPT. Plan to revisit the ChatGPT side sooner.


## Where this holds, and where it does not


Research is only useful if you know its limits, so here are ours.


**It holds across industries.** Every comparison above is true within each industry we measured, not just in the overall average. The gaps vary in size, never in direction, so this is not an artefact of one dominant industry.


**It holds across languages.** We found no meaningful difference between English and German prompts. Worth stating plainly, because the "AI behaves differently in my market" assumption is common and we did not find support for it.


**It is sharpest exactly where discovery happens.** Agreement is lowest on early-stage *explore* prompts and highest on *buy* and *evaluate* prompts. The models diverge most at the moment a buyer is building a shortlist, so per-model tracking matters most on the prompts furthest from a purchase.


**Comparisons are firmer than the individual percentages.** Tightening how strictly we classify a domain moves every level by around 5 percentage points, but moves all three models almost identically. Every comparison in this article survives that test, which is why we have written it in ratios rather than decimals.


**This is not a sample of the web.** These are commercial buying-intent prompts across several industries. It describes what AI models cite when a buyer is actively shopping, not AI usage in general.


**"Other" is real and we show it.** Between 17% and 19% of citations fall outside the five named categories, sites that could not be classified with confidence. We show it in every chart rather than hiding it, and we make no claim about what is inside it.


## What to do differently


Three moves, each tied to a number above.


1. **Track each AI model separately.** At 32% brand overlap, a single blended "AI visibility" number averages away the thing you need to act on. If you only see one figure, it is hiding two thirds of the story.
2. **Fix your own site first.** Company websites are 46-57% of citations on every model. It is the one lever that pays out everywhere, and in our experience it is where most teams still have the most room.
3. **Then split the rest deliberately.** Institutional and reference coverage is how you reach ChatGPT. Presence in communities and forums is how you reach Google AI Mode. Editorial coverage helps on both. Plan to revisit the ChatGPT side sooner, because it moves.


Sitefire runs this measurement continuously: your prompts across every major AI model, every cited page classified by source type, plus the AI bot traffic and AI referral traffic each model sends you. The per-model breakdown in this article is the view teams use to decide where a quarter of effort goes. Sitefire then turns that into the specific work: the pages to fix, the articles to publish, and the third-party coverage to earn.


## What we are measuring next


Everything here is about which pages AI models cite. The next question is the one every GEO budget rests on: does earning a citation actually make a brand more likely to be recommended?


It is worth measuring rather than assuming. A model might cite a brand's page because it had already decided to recommend that brand, in which case the citation followed the decision rather than caused it. Looking at the two side by side cannot tell them apart.


Sitefire can, because it records the pages a model reads as well as the ones it links. If simply being read raises the odds of being recommended, then getting into what the models read is upstream of getting recommended, and that is where the work belongs. That measurement is running now, and we will publish it.


## Frequently asked questions


### Do ChatGPT, Gemini and Google AI Mode cite the same websites?


Rarely. For the same prompt on the same day they share about 8% of cited web pages, and most pairs share none at all. Brand mentions overlap about four times more than cited pages do.


### Which AI model cites the most sources per answer?


Google AI Mode, by a wide margin. It cites about 10.5 web pages per answer against Gemini's 3.1 and ChatGPT's 2.7, and drew on roughly 118,000 different websites over seven weeks.


### What kind of content do AI models cite most?


Company websites, on every model. They account for 46% to 57% of citations across ChatGPT, Gemini and Google AI Mode. Editorial, institutional, forum and reference sources split the rest differently by model.


### Does optimizing for ChatGPT help my Gemini visibility?


Partly, and less than most teams assume. Brand mentions overlap about 30%, so roughly a third of a ChatGPT win carries over. The pages you earn transfer far less, at around 8%.


### Are AI models' source preferences stable over time?


It depends on the model. Google AI Mode's mix moved under 1.5 percentage points across seven weeks. ChatGPT's moved up to 8.5 points and in one direction, so work aimed at it needs revisiting sooner.


## Key takeaways


- For the same prompt on the same day, ChatGPT, Gemini and Google AI Mode agree on about 32% of the brands they mention and 8% of the web pages they cite.
- Most pairs of answers share no cited page at all. The 8% average is carried by a minority of niche prompts where the web offers few options.
- All three models take 46-57% of their citations from company websites, so your own site is the one lever that pays out everywhere.
- ChatGPT cites institutional sources about 2.5x more than either Google model; Google AI Mode cites forums about twice as often as ChatGPT.
- Source type predicts agreement but does not explain it: within company websites, agreement is still only 10.6% against an overall 7.6%.
- Supply drives the pattern. Models agree where the web offers few sources and diverge where it offers many, which is exactly where brands compete.
- Google AI Mode's source mix barely moved across seven weeks while ChatGPT's moved up to 8.5 percentage points, so the shelf life of your work differs by model.


## Sources


- Sitefire proprietary analysis, 1 June to 19 July 2026. Brand and citation overlap measured across 650,545 AI answers on ChatGPT, Gemini and Google AI Mode; source-type composition measured across 556,444 AI answers on the same models over the same window, with every cited domain classified into six categories. Commercial buying-intent prompts across several industries.
- Answers are paired by prompt and by calendar day, and figures are calculated per prompt and then averaged, so heavily sampled prompts do not dominate.
- The window was checked against AI model release dates and contains no change of default model on any of the three. GPT-5.6 shipped on 9 July 2026; ChatGPT's default remained GPT-5.5 Instant throughout.
