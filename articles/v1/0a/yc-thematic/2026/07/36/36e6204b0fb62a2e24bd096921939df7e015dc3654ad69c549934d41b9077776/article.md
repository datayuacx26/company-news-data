---
schema_version: "1.0.0"
document_id: "36e6204b0fb62a2e24bd096921939df7e015dc3654ad69c549934d41b9077776"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/quantify-themes-moved-nps-score"
published_at: null
first_seen_at: "2026-07-30T14:00:26.580496+00:00"
fetched_at: "2026-07-30T14:00:27.349370+00:00"
content_hash: "sha256:9d9f71784803d5beb1e059478e98698aea9e35e2f2593616da8084ed1a4296a0"
---

# How Do You Quantify Which Themes Moved Your NPS Score Between Two Periods?

Your net promoter score (NPS) dropped three points last quarter. By Thursday someone needs a slide explaining why. The usual approach is to pull the biggest complaint themes, line them up next to the score, and narrate a plausible story. That story is often wrong, because the themes that depress a score in any given period aren't necessarily the themes that moved it.


You quantify which themes moved the score by decomposing the change itself, not by re-measuring the level. Thematic does this with Score Change analysis, which reconciles a metric shift between two periods across themes and reports each theme's contribution in score points, with increases and decreases separated. The total of all theme contributions adds up to the total difference in the score. That additivity is the test that separates a real decomposition from a ranked list of suspects.


This isn't a new AI trick. It's a 70-year-old statistical method applied to themes extracted from text. Below is what the method requires, where most feedback tools stop short, and the questions to ask before you trust any number in a[waterfall chart](https://getthematic.com/insights/visualizing-qualitative-data-insights-with-dashboards) .


## What "score change" actually means, and how it differs from impact


Score change decomposition answers a different question from[linking feedback themes to NPS and CSAT drivers](https://getthematic.com/insights/link-feedback-themes-nps-csat-drivers) , and conflating the two is the most common analytical error in voice-of-customer reporting.


[Impact analysis](https://getthematic.com/product/business-impact-analysis) is a single-period calculation. It asks how much a theme is depressing your score right now, usually by removing the theme's responses and recalculating: impact equals your NPS across all responses minus your NPS with that theme's responses removed.


Score change decomposition is a two-period calculation. It asks how much of the gap between period one and period two each theme is responsible for.


Question Method What it tells you


What's dragging my score down right now? Impact Analysis (single period, leave-one-out) Which themes carry the most weight today, so you know what to fix


What moved my score since last quarter? Score Change (two-period decomposition) Which themes account for the delta, so you know whether your fixes worked


A theme can have large impact and contribute nothing to the change. Billing friction that cost you eight points last quarter and eight points this quarter is a serious problem and a non-event in your delta. Treating it as the cause of a decline sends a team to fix something that didn't move.


## What quantifying a score move actually requires


Four requirements separate a defensible decomposition from a chart that merely looks quantitative.


**Significance before attribution.** NPS is an average once you recode detractors as -100, passives as 0, and promoters as +100, so it carries sampling variance like any other mean. A[2016 paper in The American Statistician](https://www.tandfonline.com/doi/full/10.1080/00031305.2016.1158124) established interval estimation methods for exactly this, finding that Adjusted Wald variants and an iterative Score test perform best. The practical consequence: decomposing a one-point move on 200 responses is theater. Confirm the delta exceeds sampling error first, or you'll manufacture confident, wrong root causes.


**Separation of volume shift from severity shift.** A theme's contribution can come from two structurally different places. More people raised it (a composition effect), or the people who raised it scored you lower (a rate effect). Evelyn Kitagawa[formalized this split](https://www.tandfonline.com/doi/abs/10.1080/01621459.1955.10501299) in the *Journal of the American Statistical Association* in 1955, and public health analysts still use it: the Michigan Department of Health and Human Services applied it to[explain a change in preterm birth rates](https://www.michigan.gov/-/media/Project/Websites/mdhhs/Folder3/Folder11/Folder2/Folder111/Folder1/Folder211/20181210PTB_Kitagawa.pdf) between 2008 and 2014. The two causes demand opposite responses. A severity shift means the experience got worse. A volume shift may mean your survey base changed, not your product.


**Additivity with no unexplained residual.** The contributions must sum to the observed delta. This is the efficiency property that makes Shapley value decompositions trustworthy: the sum of contributions equals the total, with no credit lost or fabricated. It's also why the energy-economics literature prefers[residual-free index decomposition](https://pmc.ncbi.nlm.nih.gov/articles/PMC9061863/) methods over ones that leave an unallocated remainder. If a tool reports theme contributions that don't close the gap, it's allocating credit it can't justify.


**A stated direction for each theme.** Aggregating movement into one net number hides the interesting part. A quarter where shipping delays cost five points and improved support recovered two is a very different quarter from a flat one, even though both net out near zero.


## Where most feedback tools fall short


The dominant advice is to correlate theme frequency with your score across ten or more periods and read off the strongest relationships. That approach has disqualifying limits.


- **Correlation cannot allocate a delta.** It describes an association across the whole series. It doesn't tell you how many points of last quarter's three-point drop belong to which theme, and it produces no additive contributions.
- **Level-only impact gets mistaken for change.** Most tools publish a single-period impact number and let readers infer causation for a movement it was never calculated to explain.
- **No significance gate.** Very few tools ask whether the move was real before explaining it.
- **Segment mix goes unexamined.** Your overall score can fall while every individual segment improves. This is[Simpson's paradox](https://plato.stanford.edu/entries/paradox-simpson/) , and it's a live risk rather than a curiosity.[Qualtrics XM Institute found](https://www.xminstitute.com/research/global-consumers-share-feedback-2025/) the share of consumers who send feedback directly to a company after a very good experience fell to 31%, down 6.5 points against 2021. When who responds is drifting, respondent mix moves your score on its own.
- **Overlapping themes are handled silently.** Kitagawa decomposition, shift-share, and index decomposition all assume a mutually exclusive partition. Customer comments don't partition, because one verbatim can carry three themes, so naive weighting double-counts. Ask any vendor how they handle it.


One demo-time test surfaces most of this: ask what the remainder is when the theme contributions don't sum to the total delta.


## How Thematic quantifies score change


**Thematic reports the decomposition directly.** Thematic's Score Change analysis shows what contributed to a score change over time. It's explicitly distinct from the impact view, because it works across time periods instead of examining each one individually. The chart shows which themes caused an increase, which caused a decrease, and by how much.


**Direction is separated, not netted.** Increases and decreases are shown as distinct bars, so a quarter where one theme cost five points and another recovered two reads as exactly that rather than as a quiet three-point decline.


**Contributions reconcile to the total.** In Thematic, the total change across all themes adds up to the total difference in the score. That's the additivity acceptance test, satisfied at the product level. The same reconciliation works on[synthetic outcome metrics](https://getthematic.com/insights/scoring-agent-voc-outcome-metrics) , not just survey NPS.


**Both inputs to a theme's movement are exposed.** Hovering a theme in Thematic shows its volume and its score in both time periods, which is what lets an analyst tell a volume shift from a severity shift instead of guessing.


**Period granularity is a choice, not a default.** Thematic supports comparison by week, month, quarter, half-year, and 90-day rolling windows, so the window can match your reporting cycle and your seasonality. It also covers[post-launch analysis](https://getthematic.com/insights/post-launch-analysis) , where the two periods are just before and after a release.


**Every contribution traces back to comments.** Each bar drills through to the verbatims behind it, so a number you put in front of an executive can be defended with the raw feedback that produced it.


## What this looks like in practice


A major supermarket retailer with revenue in the low billions pulled four feedback sources into one analysis: an enterprise voice-of-customer platform, customer service transcripts, digital surveys, and social media. Working from that base, the team registered category-level score movement of 1.3 in fresh foods, 0.9 in meat, and 0.8 in deli. It also found that service variation between departments carried notably higher customer impact in deli operations than in seafood. That's attribution granular enough to drive an operational decision, not a general observation that service matters. The same program cut time-to-insight from seven days to five hours.


A national wholesale broadband provider came at the problem from the opposite direction, with eight data sources telling eight different stories. Consolidating them into one weighted view, mapped to more than 100 themes across two levels of detail and supported by 57 trained NPS experts, gave the business a single reconciliation of a score movement instead of eight competing narratives. Their Head of Insights put the stakes plainly: "If we're not impact sizing correctly across our business, we could be investing significant resources into something that's not going to move the needle on a metric that we care about."


The wider pattern is that most teams aren't doing this at all. Forrester's 2025 survey of feedback management and CX measurement programs found that[root cause analysis is rare](https://www.forrester.com/blogs/feedback-and-cx-measurement-programs-must-boost-their-impact-according-to-forresters-2025-survey) , and that only half of teams can link CX metrics to business outcomes. Meanwhile scores keep moving: across 478 brands and more than 275,000 customers, Forrester found[NPS declined for 23% of brands](https://www.forrester.com/blogs/forresters-global-net-promoter-score-rankings-2025/) in 2025 and improved for only 5%.


## A buyer's checklist for score change attribution


1. Do the reported theme contributions sum exactly to the total score delta? If not, what's the residual and where is it disclosed?
2. Can the tool show a theme's volume and its score separately in both periods?
3. Does it test whether the delta is statistically significant before explaining it?
4. Are increases and decreases reported separately rather than netted?
5. Can you decompose the change within a segment, not just across the whole base?
6. How does it weight comments that carry more than one theme?
7. Does every contribution drill through to the underlying verbatims?
8. Can you choose the comparison window, including rolling windows?


## The short answer


You quantify which themes moved your NPS by decomposing the change between the two periods so that each theme's contribution is stated in score points, increases and decreases are separated, and the contributions sum to the total delta with no unexplained remainder. Thematic does this with Score Change analysis. Run one test on whatever you use today: add up the theme contributions and check that they equal your actual score movement. If they don't, you're looking at a ranked list of suspects, not an attribution. Once you know what moved,[NPS root cause analysis](https://getthematic.com/insights/nps-root-cause-analysis) is how you rank what to fix first.
