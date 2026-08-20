---
schema_version: "1.0.0"
document_id: "48e6bd07cd6de285209e85f6a4c61e6cae9697c6a1b0091be624dfdad3ac927f"
company_key: "yc-ledger-investing"
company: "Ledger Investing"
source_id: "yc-ledger-investing-news-import-76567589263d"
canonical_url: "https://www.ledgerinvesting.com/news/ledger-analytics-api"
published_at: "2025-04-08T00:00:00+00:00"
first_seen_at: "2026-07-22T02:06:53.692026+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:3a8a87bbb0e59fd231478ba273af5ba295bec28ced5020be3712d20cc9d24ab4"
---

# The Ledger Analytics API is now open for beta testing

The Ledger Analytics API is now open for beta testing!


We've talked to a lot of actuaries who are excited about fitting Bayesian models to insurance data at scale, but not so excited about wiring up all the infrastructure to make it happen. They want to reliably estimate full posterior distributions on ultimate loss (ratio) predictions, and they want priors that are supported by industry data.


That's why we are opening up our Ledger Analytics API along with a convenient Python interface that makes it easy to get full posterior distributions on ultimate loss ratios with minimal setup.


While there are existing tools for modeling insurance data, they lack a few key features that we need to appropriately model insurance-linked securities. We've spent years developing our API to meet these needs:


-


**Advanced Loss Development** : Fit loss development and tail development models directly using our API, ranging from Bayesian versions of traditional actuarial models like Chain Ladder to more advanced Bayesian models developed throughout our ongoing R&D process. Our models leverage informative priors derived from industry data.


-


**Time Series Forecasting** : Fit Bayesian autoregression and state-space time series models to forecast ultimate losses for future accident and policy periods. In insurance forecasting we often only have dozens of observations, and the ultimates for more recent periods are estimates as opposed to observed data. Our forecasting models use industry-informed priors and account for uncertainty in historical ultimate loss estimates, producing forecasts that are well-calibrated across a wide range of P&C product lines.


-


**Open-Source Integrations** : Ledger Analytics is heavily integrated with Bermuda, our open-source Python library for insurance loss triangle management and manipulation. Users can also perform model stacking using BayesBlend, our library for performing Bayesian model stacking.


-


**Comprehensive Model Documentation** : Our documentation includes in-depth tutorials of loss development and forecasting, mathematical specifications of all available models, and details on how each model can be configured.


Coming Soon: Keep an eye out for upcoming features, including loss payment cashflow modeling and ILS deal modeling capabilities!


Ready to get started?


-


Email analytics@ledgerinvesting.com to register and get your API key


-


Install the package:` pip install ledger-analytics`


-


Check out the documentation: https://ledger-investing-ledger-analytics.readthedocs-hosted.com/en/stable/index.html
