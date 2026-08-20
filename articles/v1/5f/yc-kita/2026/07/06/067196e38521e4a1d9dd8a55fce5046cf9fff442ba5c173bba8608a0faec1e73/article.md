---
schema_version: "1.0.0"
document_id: "067196e38521e4a1d9dd8a55fce5046cf9fff442ba5c173bba8608a0faec1e73"
company_key: "yc-kita"
company: "Kita"
source_id: "yc-kita-news-import-29b97ff66fe4"
canonical_url: "https://www.kita.ai/blog/documents-alternative-data-microlending"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-24T09:24:05.566790+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:07456cb02998da04b37bd9fb493378dfc1ea76197736f0c19d3c45421342bc68"
---

# The Untapped Data Source: +7.0 Gini From Borrower Documents in Microloan Underwriting

Across emerging markets, from the Philippines and Indonesia to Mexico, South Africa, and Nigeria, microlenders and MSME lenders serve borrowers the formal system barely sees. Their applicants are informal businesses, micro-retailers, self-employed workers, and gig workers with thin or empty bureau files and financial histories scattered across cash, mobile money, and paper. That is the market’s founding problem and its hardest: a **thin-file mass market** behind a $5.7 trillion financing gap. Serving these borrowers means assessing risk from what they can actually provide, which is most often **borrower-submitted documents** , in whatever form they arrive.


Collecting those documents carries a real cost. Every upload step adds application friction and risks borrower drop-off, and every file collected adds manual review burden, which is why many lenders keep document collection minimal. That tradeoff is the reason borrower-submitted documents stay underused, not evidence that they lack value.


Our ground truth is **DPD30 (Days Past Due 30)** : a borrower is in **default** if a payment runs more than 30 days late — the standard binary outcome in credit risk modeling.


The result that surprised us most was the baseline: existing microlenders split risk only slightly better than a coin flip. And the most predictive signal was not device metadata or psychometrics — it was the documents borrowers already upload, which nearly every lender collects and almost none scores.


In brief


-


8,000 documents from a globally diverse pool of MSME and micro lenders, backtested against bureau scores and DPD30 repayment outcomes. 500+ signals extracted blind from borrower-submitted documents.


-


Document signals added +2.4 Gini over the bureau score alone — +7.0 Gini (+0.036 AUC) on applications with bank statements or payslips.


-


Scores split risk monotonically: default rates fell near-linearly from riskiest to safest decile — sharper pricing, higher limits, wider approvals.


01


## The gap in the global alternative data stack


The core constraint in MSME and microlending is that the borrowers who most need credit are close to invisible to bureaus: informal businesses, fragmented financial histories, and little or no formal repayment record.


1.3B


adults worldwide remain outside the formal financial system


~70%


of micro, small, and medium-sized enterprises in emerging markets lack the financing they need to grow


$5.7T


the estimated unmet demand for credit


Lenders have responded by layering on alternative data, and each layer earns its keep, up to a point:


The alternative data stack


Credit bureau data


Strength


The industry standard where formal credit history exists


Limit


Thin or empty for the borrowers who most need credit


Device & behavioral signals


Strength


Strong for identity and fraud


Limit


A phone model says little about cash flow


Transaction & partner data


Strength


Feeds like telco, transaction data, and lender networks sit closer to repayment capacity


Limit


Access depends on partnerships; each feed is a partial view, so coverage is fragmentary


Borrower documents The untapped layer


Strength


Direct, first-party evidence of income and cash flow


Access


The borrower hands it to you at application, with consent, inside your own funnel


Documents are the overlooked layer, and the least abstract one. A bank or e-wallet statement is effectively the borrower’s income statement; payslips, utility bills, permits, invoices, and business records carry direct evidence of cash flow, business activity, stability, repayment capacity, and fraud risk. There usually aren’t many, often just one or two pages, but the surprise is how much data a single one holds. They arrive self-reported, as phone photos, screenshots, or forwarded emails, which is part of why they have gone unscored. Even when an upload is a low-quality picture or the wrong document, the signals pulled from it still split risk at scale. This is not alternative data in the abstract; it is underused credit data already sitting inside the application flow.


02


## The backtest: 8,000 documents and repayment outcomes


A diverse global dataset of microloans, with both optional and required document uploads.


1.


1


Documents


8,000 self-reported borrower uploads — bank statements, payslips, bills, IDs, etc.


2.


2


Computer vision


500+ signals extracted blind from each application.


3.


3


Regression


Every signal regressed against train outcome set.


4. 4


Real outcomes


Repayment ground truth of 30 days past due.


To rank signals we use **Information Value (IV)** , a standard measure of how strongly one variable separates repayers from defaulters (weight-of-evidence binning against DPD30). Above 0.1 is a medium predictor; above 0.3 is strong.


Information Value (IV) by signal category


Bank-statement income and cash-flow signals are the most predictive category — but even non-financial uploads carry measurable signal.


0.1 Medium


0.3 Strong


Bank statement income


0.44


Cash flow & balance patterns


0.36


Payslip & employer signals


0.30


Document fidelity


0.19


Bill & utility regularity


0.14


ID & biometric metadata


0.07


Default rate by score decile · monotonic risk splitting


Capture-fidelity score


linear R² 0.97


1 of 13 scores


The default rate falls with every step from the riskiest decile to the safest — no reversals. That monotonic ordering is what makes a score predictively useful: a higher score always means lower risk, so a lender can set an approval cutoff, price a tier, or raise a credit limit at any point on the curve and trust the risk ordering on both sides. All 13 document scores behaved this way (R² above 0.95).


Measured against the bureau score alone:


-


On this thin-file population, incumbent scores split risk only slightly better than a coin flip.


-


Portfolio-wide, document signals added +2.4 Gini over the bureau score.


-


With a verified financial document, the lift more than doubled: +7.0 Gini (+0.036 AUC).


-


Top document scores were monotonic — default rates fell steadily from riskiest to safest decile, directly usable for cutoffs, pricing, and limits.


Gini and AUC both measure how well a score separates repayers from defaulters — Gini from 0 (random) to 100 (perfect), AUC as a probability from 0.5 to 1.0. A few extra points mean fewer defaults at the same approval rate, or more approvals at the same risk.


How Gini is measured · cumulative gains (Lorenz) curve


Rank borrowers by score, riskiest first. The Lorenz curve shows the share of actual defaults captured as you step through the population — a random model follows the diagonal; a perfect one captures every default first. Gini is twice the area between the curve and the diagonal.


Random


Gini 0


coin flip


Bureau alone


Gini ~0.46


baseline


With documents


Gini ~0.53


with a data-rich document


The shaded region is the lift from document signals. Illustrative; applications with a data-rich document.


Risk-ranking power · relative to the bureau score alone


-


Bureau score alone


· the incumbent baseline


baseline


-


+ Document signals


· full portfolio · +2.4 Gini


+15%


-


+ Document signals


· financial documents · +7.0 Gini


+57%


03


## The A/B test


Self-reported uploads are messy — applicants submit everything from selfies to photos of their credit cards. Even so, **non-financial uploads** carried risk-splitting signal, and the flow itself steers borrowers toward richer documents. A **fine-tuned vision model** pulled structured data from bank statements and payslips even in skewed or blurry photos.


A/B test · document upload vs no document upload


−18%


defaults among approved borrowers vs the control arm


+12%


more valid approvals with no added portfolio risk


Same product, population, and cutoff policy — the only variable was the document-upload step.


The approval lift comes from repayment capacity that traditional underwriting overlooks. It is also the other side of the friction tradeoff: the upload step costs some drop-off, and the risk separation it surfaces is what that cost buys.


04


## What this means for MSME lenders


Scoring the documents already moving through the application flow gives a lender:


**Sharper risk ranking.** 15% better than the bureau score portfolio-wide, 57% where financial documents are present.


**Wider, safer coverage.** Documents score the thin-file borrowers bureaus cannot see: 12% more valid approvals, no added risk.


**Competitive rates and credit-limit increases.** Monotonic scores can be acted on directly: price safe deciles competitively, raise limits for proven repayers, tighten cutoffs where the risk sits.


For a lender, the question is no longer whether borrower documents carry signal; the backtest settles that. The practical question is where and when that signal is worth collecting, given the friction it adds, and how much of it is already sitting inside the application flow they run today.


## See it on your own loan book


Share a sample of the files you already collect, with outcomes, and we’ll return a ranked report of which signals predict repayment on your book.


[Talk to our team](https://www.kita.ai/contact)


Kita turns the documents borrowers already submit into measurable lift for risk models in thin-file markets.


## Frequently asked questions


### How much predictive power do documents add to a credit model?


In our backtest of 8,000 MSME and microloan applications, borrower-submitted document signals added +2.4 Gini over the bureau score, and +7.0 Gini (+0.036 AUC) on applications with a bank statement or payslip, a 57% improvement in risk ranking.


### What is a Gini coefficient in credit scoring?


How well a score separates repayers from defaulters, from 0 (random) to 100 (perfect). A few extra Gini points mean fewer defaults at the same approval rate, or more approvals at the same risk.


### What is AUC in credit scoring?


The same idea as Gini, expressed as a probability from 0.5 (random) to 1.0 (perfect) that a defaulter is ranked riskier than a good borrower. The scales convert directly: Gini = 2 × AUC − 1.


### Why does it matter that a credit score is monotonic?


Monotonic means default rates fall steadily from the riskiest band to the safest, no reversals — so a lender can set a cutoff, price a tier, or raise a limit anywhere on the curve and trust the ordering. All 13 document scores in our backtest were monotonic (R² above 0.95).


### What is Information Value (IV) in credit modeling?


A standard measure of how strongly one variable separates repayers from defaulters, computed with weight-of-evidence binning. Above 0.1 is a medium predictor; above 0.3 is strong. Bank-statement income and cash-flow signals scored highest in our dataset.


### Does document underwriting work when borrowers upload non-financial documents?


Yes — even messy uploads like selfies carried risk-splitting signal, though the core signal lives in payslips and bank statements. In a live A/B test, adding a document-upload step cut defaults 18% and lifted valid approvals 12% with no added risk.


### Can Vision AI really read poor-quality financial documents?


Yes. Vision-language models parse photographed, skewed, and degraded documents at accuracy legacy OCR never reached — the breakthrough that made document underwriting viable in emerging markets.


### How should a lender decide which document signals to put into production?


Backtest before building: extract signals from historical documents, measure each against real repayment outcomes, and keep only what proves itself on your own loan book.


Related reading


[Bank Statement Extraction, Benchmarked: 9 AI Systems on Real Lending Documents](https://www.kita.ai/blog/bank-statement-extraction-benchmark)[Reconstructing Financial Evidence from Degraded Documents](https://www.kita.ai/blog/reconstructing-financial-evidence-from-degraded-documents)


Kita · Backtest report · July 2026
