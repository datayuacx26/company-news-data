---
schema_version: "1.0.0"
document_id: "b94a393d91df1084dd7c532bc4041527740bf84b2d341c5301a3226b97a4be8e"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/tracking-the-ripple-effect-how-adverse-drug-events-move-pharma-markets"
published_at: null
first_seen_at: "2026-07-26T08:11:39.657869+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:031cec4a0a2b7381a03be690d4cb649558a96106bd5062660c27bcff499a2c17"
---

# Tracking the ripple effect: how adverse drug events move pharma markets

[← Back to all posts](https://deepnote.com/blog)


Event‑study walkthrough showing how adverse drug event signals map to pharma stock moves, combining price data with FAERS counts to measure market reactions. A practical framework for analysis, with a clear focus on method and interpretation.


Academic event-studies find that “drug-development setbacks trigger significant negative returns” on biopharma stocks. A recent[machine-learning analysis](https://www.nature.com/articles/s41598-023-39301-4?error=cookies_not_supported&code=bafbcb2f-69f8-4c68-81fe-278fef7100ba#:/~:text=necessarily%20imply%20a%20positive%20impact,g) also shows *negative trial* news tends to produce *larger* stock price drops. Real-world examples: e.g. Neurogene’s share price[plunged](https://www.biospace.com/drug-development/neurogene-details-gene-therapy-complication-sending-shares-plummeting-again#:/~:text=Neurogene%E2%80%99s%20shares%20fell%20by%2036,401) ~36% after a severe trial adverse event was disclosed, and Intellia saw its[stock fall](https://www.biopharmadive.com/news/intellia-crispr-liver-enzyme-adverse-event-ttr-cardiomyopathy/749231/#:/~:text=Intellia%20shares%20lost%20more%20than,Permission%20granted%20by%20Intellia%20Therapeutics) ~25% on word of a Grade 4 liver injury in a Phase 3 study.


An interesting wrinkle:[research from Portsmouth](https://www.newswise.com/articles/dangerous-infectious-diseases-bad-news-for-main-street-good-news-for-wall-street) showed that broad disease headlines; WHO pandemic alerts, for instance, can actually *lift* pharma names. Disease news is bad for Main Street but good for Wall Street, the authors put it. So the direction depends on whether the news is about a company's own drug problem or the market opportunity a health crisis creates.


> This blog post demonstrates an event-study workflow using stock prices and adverse-event signals. We identify days with elevated adverse event reports and measure stock returns in surrounding windows.
> This notebook is intended as a *methodology framework* for data ingestion and event-study analysis. It is not a reproduction of the specific results found in the academic papers, nor is it financial advice. It demonstrates how to fuse disparate data sources (yfinance, openFDA) in a single *analytical* view.


## Loading prices


We pull market data through the connectors already scoped: yfinance and openFDA FAERS for adverse-event counts.


> This notebook is configured for JAZZ (Jazz Pharmaceuticals) and XYREM. The markdown explanations, literature references, and
> results are specific to this ticker/drug combination. If you run this with a different ticker or drug, the outputs will change but the explanatory text won't automatically update. Treat this as a methodology template rather than a one-click report generator.


> I set up the inputs and displays using Deepnote’s native UI blocks, so the notebook behaves like a small app. The controls live up top, the logic runs quietly in code blocks, and the charts and metrics update as you tweak values (try the[web-app](https://deepnote.com/app/deepnote/Tracking-the-ripple-effect-how-adverse-drug-events-move-pharma-markets-d3ea42a1-f265-4910-aba7-74d79b7faec6?utm_content=d3ea42a1-f265-4910-aba7-74d79b7faec6&ticker_name=JAZZ&PRICE_FROM_input=2005-01-01T00%3A00%3A00.000Z&PRICE_TO_input=2025-12-31T00%3A00%3A00.000Z&DRUG_NAME_input=XYREM&START_DATE_input=2005-01-01T00%3A00%3A00.000Z&END_DATE_input=2025-12-31T00%3A00%3A00.000Z&__run=true) ).


The table above shows the baseline price series; open, high, low, close, volume; over the window you selected. The PLOS studies estimated what a stock should have returned given market movements, then compared actual returns in a short window around news dates. We pulled nearly ~19 years of trading data for Jazz Pharmaceuticals. The stock shows typical biotech volatility with a *4.85%* daily standard deviation and a long-term upward trend (mean return 0.137% per day). The 209% max daily return likely reflects clinical trial successes, while the -40% drop points to adverse news events we'll investigate.


## Pulling adverse-event reports


The drug name and FAERS date range are also wired to Deepnote inputs, so you can update them and see the counts refresh (try the[web-app](https://deepnote.com/app/deepnote/Tracking-the-ripple-effect-how-adverse-drug-events-move-pharma-markets-d3ea42a1-f265-4910-aba7-74d79b7faec6?utm_content=d3ea42a1-f265-4910-aba7-74d79b7faec6&ticker_name=JAZZ&PRICE_FROM_input=2005-01-01T00%3A00%3A00.000Z&PRICE_TO_input=2025-12-31T00%3A00%3A00.000Z&DRUG_NAME_input=XYREM&START_DATE_input=2005-01-01T00%3A00%3A00.000Z&END_DATE_input=2025-12-31T00%3A00%3A00.000Z&__run=true) ).


*Note: The*[FAERS database](https://open.fda.gov/data/faers/) *is voluntary; not every adverse event makes it in, and reports can lag by weeks or months. Still, spikes in FAERS counts often align with the kind of safety signals that move stocks; label updates, black-box warnings, publicized deaths. The Nature ML work leaned on exactly these negative cues when building sentiment features* .


## Joining prices and FAERS


## Identifying discrete adverse event dates


Rather than using daily FAERS counts as "events," we identify specific dates when adverse drug news was publicly disclosed. This approach matches the event-study methodology from academic literature, where researchers measure stock reactions to discrete announcements (FDA decisions, trial results, safety alerts) rather than continuous report volumes.


Using the FAERS database, we identified days when adverse event reports spiked above normal levels. The threshold (mean + 3 standard deviations) flags days with~366 reports as outliers. This statistical approach surfaces 7 potential safety signal dates between 2014-2018, a period that aligns with known controversies around XYREM's sodium content and off-label use concerns.


The FAERS spike dates tell us *when* adverse events were reported to the FDA, but they don't tell us *how* the market learned about them. This is where news coverage comes in. Stock prices react to information that reaches investors. A FAERS report sitting in a database doesn't move markets, headlines do. Research from the[Nature 2023 ML paper](https://www.nature.com/articles/s41598-023-39301-4) showed that negative news sentiment amplified adverse event impacts. Articles with terms like "halt," "death," "FDA warning," or "trial failure" correlated with larger stock price drops. By fetching news articles around each FAERS spike date and analyzing sentiment, we can test whether:


1. Media coverage clustered around the same dates as FAERS spikes (corroboration)


2. More negative sentiment → larger abnormal returns (information intensity)


We use the Apify Google News Scraper to fetch articles in a ±3 day window around each event, then apply keyword-based sentiment analysis to classify articles as negative, neutral, or positive. Academic papers like the Nature study used BERT classifiers, but keyword matching captures most obvious adverse event terms. Historical news archives have *limitations* . Google News coverage is comprehensive for recent years but sparse for 2014-2018 pharmaceutical news. The methodology demonstrated here works (we validated with 2024 articles), but historical event coverage may be limited. For production use, consider APIs with deeper archives (Bing News Search, LexisNexis) or real-time monitoring.


## Price chart with adverse-event markers


The chart above overlays high-FAERS days (dark blue triangles) on the price series. Look for whether drops cluster near markers. The[PLOS 2013 study](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0071966) documented cumulative abnormal returns of several percentage points after negative trial news; the[NIH study](https://pmc.ncbi.nlm.nih.gov/articles/PMC9439234/) found early-stage biotechs swung hardest.


## Event study: Using FAERS spikes as events


Rather than *hard-coding* specific dates, we use the high-FAERS days (mean + 3σ) identified earlier as our "adverse event" dates. This approach works for any ticker/drug combination.


To measure the true impact of adverse events, we need to separate normal market movements from event-specific reactions. We estimate each stock's expected return using the market model:


## Calculating abnomal returns


Across 7 adverse event spikes, Jazz Pharmaceuticals stock declined an average of **5.24%** relative to market expectations. The consistency is striking; median CAR is -5.13%, showing this isn't driven by a single outlier. The t-test confirms statistical significance (p = 0.017), meaning we can reject the null hypothesis that adverse events have no impact on stock prices.


This aligns with academic findings from the PLOS 2013 study, which documented similar percentage-point drops following FDA safety announcements. The waterfall chart below shows how CAR compounds across sequential events, reaching cumulative losses near -40% by early 2017.


## Visualizing cumulative abnormal returns


This chart shows the cumulative abnormal return (CAR) for each adverse event. Academic event studies typically present CAR plots to illustrate the magnitude and timing of market reactions.


Each step down represents a new adverse event spike. Notice the accelerating decline from 2015-2017, coinciding with Jazz's handling of XYREM safety controversies (unreported deaths, sodium content warnings). The market appears to update beliefs sequentially rather than pricing in all risk at once.


#### FAERS distribution


Days exceeding the threshold line are classified as *high-FAERS* events.


If the bar at offset 0 (event day) or +1 dips negative while surrounding days are flat or positive, that matches the asymmetric reaction pattern documented in the literature. TheNIH study found Phase 2/3 failures hit hardest; for a large-cap with many drugs, individual safety signals may not move the needle. If bars are flat, the ticker may be too diversified; or FAERS spikes don't correspond to market-moving disclosures.


The[Nature ML paper](https://www.nature.com/articles/s41598-023-39301-4) trained a BERT classifier on clinical trial announcements and found that negative sentiment terms - "fail," "halt," "adverse," "terminate"; dominated the price signal. A keyword filter is a blunt approximation, but it catches most obvious bad-news days. Compare the dates flagged here to the FAERS spikes above; if they cluster together, FAERS and headlines are telling the same story. If they diverge, headlines may surface events FAERS hasn't caught yet (or vice versa; FAERS reports can lag by weeks).


## A more rigorous model


HAC market model regression We only have a *handful* of high-FAERS event days, so a large (multivariate) time-series model would be unstable and hard to interpret. A market model with Newey-West (HAC) standard errors is a standard finance approach that accounts for autocorrelation and volatility clustering in returns. The coefficient on the event flag answers the key question directly: how much the stock moves on *high-FAERS* days after controlling for the market.


## What this model says for JAZZ / XYREM


The market beta comes out at about 1.11, which means JAZZ tends to move a bit more than the market on average. The event-day coefficient is about -0.22 percentage points with a large standard error and a high p-value, so the same-day effect is not statistically distinguishable from zero in this regression. That does not contradict the event-study result; the event study aggregates a *short window* and isolates abnormal returns around those dates, while this regression is a *same-day* test with only seven event days to learn from.


The day 0 coefficient stays near zero. The day +1 and +2 coefficients are more negative, with day +2 around -0.79 percentage points and marginal at the 10% level. This is consistent with a delayed reaction pattern rather than a clean same-day hit. Adding up day 0 through day +2 gives an estimated three-day impact around -1.78 percentage points.


## Dashboard metrics


Event days underperform by 0.08 percentage points on average, but this aggregates small negative moves across all event days. The CAR analysis is more powerful because it isolates the *abnormal* component (what the stock did vs what the market model predicted it would do). That's where we see the true -5.24% impact.


## Key findings and interpretation


Ticker / Drug **JAZZ / XYREM**


Analysis Period 2005-2024 (20 years)


Total FAERS Reports 60,586


Trading Days Analyzed 4,676


High-FAERS Event Days **7** (2014-2018)


Mean CAR (Day -5 to +5) **-5.24%**


Median CAR **-5.13%**


Statistical Significance **p = 0.017** (t = -3.27)


Event Period 2014-2018 XYREM safety controversies


The analysis demonstrates that adverse event spikes in the FAERS database serve as reliable proxies for market-moving safety signals:


1.


**Statistically significant negative returns:** Jazz Pharmaceuticals experienced a mean cumulative abnormal return of -5.24% across 7 adverse event spikes (p = 0.017), controlling for overall market movements via the S&P 500.


2.


**Consistency across events:** The median CAR of -5.13% closely matches the mean, indicating this isn't driven by a single outlier. The waterfall chart shows sequential negative impacts compounding from 2014-2017.


3.


**Replicates academic findings:** The -5.24% result aligns with the[PLOS 2013 study](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0071966) , which documented "several percentage point" cumulative abnormal returns following FDA safety announcements for pharmaceutical stocks.


4.


**News sentiment integration:** While we successfully demonstrated the news fetching and sentiment analysis workflow, historical news archives (2014-2018) had sparse coverage (1-2 articles across 7 events). The methodology is sound; we validated it with 2024 data; but production use would require deeper archives or real-time collection.


FAERS spikes correlate with significant abnormal negative returns even without explicit news sentiment data. The statistically significant CAR confirms that elevated adverse event reporting periods are material events that impact equity valuations.


Srihari Thyagarajan


Technical Writer


Follow Srihari on[Twitter](https://x.com/hari_leo03) ,


[LinkedIn](https://www.linkedin.com/in/srihari-thyagarajan/)


and[GitHub](https://github.com/Haleshot)
