---
schema_version: "1.0.0"
document_id: "fb71fc6024d7b7fd39d839e33f17b5c03bf743934ea0e217375db1e3adb3afae"
company_key: "yc-event-horizon-labs"
company: "Event Horizon Labs"
source_id: "yc-event-horizon-labs-news-import-36ee4ccaf0e5"
canonical_url: "https://www.ehl.markets/posts/noisy.html"
published_at: null
first_seen_at: "2026-07-25T03:47:22.258094+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:fe9a6acdedf1bdc6a8470c22cf7ea2fda6734b5b935c0d584e35dbf0a9041c9c"
---

# Can agentic loops optimize noisy rewards? — Event Horizon Labs

[← Back to research](https://www.ehl.markets/research.html) 2026 · arXiv:2605.23007 · q-fin.TR


## Can agentic loops optimize noisy rewards?


FunSearch and AlphaEvolve showed that an LLM in an evolutionary loop can rewrite code toward better solutions on deterministic targets — matrix multiplication, bin packing, compiler heuristics. Markets are the opposite: the reward is stochastic, non-stationary, and trivially easy to overfit. We put an LLM-driven evolutionary loop to that harder test — rewriting the code of a trading strategy, scoring each candidate against a held-out backtest, and keeping and recombining what survives — to ask whether an agentic loop is **doing research or just p-hacking.**


HOW IT WORKS


An LLM proposes a diff or a full rewrite to the parts of the strategy marked mutable; everything else — simulation, data, PnL accounting — stays fixed, so gains reflect real algorithmic change rather than evaluation artifacts. A structured population with quality-diversity selection and island migration keeps the search broad while it compounds on its best solutions, and mutation queries route across several frontier and efficient-tier models rather than betting on one.


WHAT WE FOUND


**Gains that hold out of sample.** Out-of-sample Sharpe improves by 0.6 to 1.8 points across runs; the best run reaches a test Sharpe of 5.65.


**Not p-hacking.** Against a conservative best-of-K null, the selected strategy sits roughly 45 standard deviations into the tail on held-out data — and the out-of-sample curve rises rather than decays, the opposite signature of overfitting.


**Better forecasts, too.** In the forecasting-only run, an evolved feature set roughly doubles out-of-sample R² over a three-feature baseline — though higher accuracy only becomes PnL once execution is re-tuned.


**Model-agnostic.** Best results trace their lineage through three to five distinct LLMs, so the engine compounds with each new model release rather than betting on one.


OPEN PROBLEMS


**Metrics aren't PnL.** Higher forecasting accuracy underperforms out-of-sample until execution hyperparameters are re-tuned.


**Bigger search, more overfitting.** The largest search space gives the best risk-adjusted result but the widest validation-to-test retention gap.


**How far can it go?** The objective isn't gradient-friendly, so there's no inner tuning loop yet — and the deeper question stands: how far can agentic loops push as reward functions get noisier?


SCOPE


Conditional on the backtest's market model — exchange-aggregated data that isn't directly tradable, full fill at the limit price, a specific fee and impact model. Results do not transfer out-of-the-box to live trading; live assessment is future work.


Kvasiuk, Li, Colegrove, Münchmeyer
University of Wisconsin–Madison & Event Horizon Labs


[Read the paper →](https://arxiv.org/abs/2605.23007)
