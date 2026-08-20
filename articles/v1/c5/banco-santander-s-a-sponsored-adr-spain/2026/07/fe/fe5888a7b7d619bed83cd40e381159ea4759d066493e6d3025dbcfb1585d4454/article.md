---
schema_version: "1.0.0"
document_id: "fe5888a7b7d619bed83cd40e381159ea4759d066493e6d3025dbcfb1585d4454"
company_key: "banco-santander-s-a-sponsored-adr-spain"
company: "Banco Santander S.A. Sponsored ADR (Spain)"
source_id: "banco-santander-s-a-sponsored-adr-spain-rss-14b90327b079"
canonical_url: "https://www.santander.com/en/stories/foundation-models-come-to-tables-and-time-series"
published_at: "2026-07-28T15:30:00+00:00"
first_seen_at: "2026-07-28T16:05:39.594197+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:4e279920a554fa21cfa719a8ed914bbc4140cd76c78cc6bdb484a4eff068efe8"
---

# Foundation models come to tables and time series

[Communications Santander](https://www.santander.com/en/press-room/topics-of-interest)


CEST


[Linkedin](https://www.linkedin.com/company/banco-santander)


[Instagram](https://www.instagram.com/santander.global/)


[X](https://x.com/bancosantander)


Most of the conversations about foundation models are related to language and images. Less discussed, but more consequential for a bank, is that the same idea is now being applied to one of the most widely used forms of data: rows in a table and numbers over time. Over the past year, pretrained models for this data have matured to the point where they are worth taking seriously in production, and Santander teams are already putting them to the test.


Instead of building a forecasting model from scratch for a specific series, Santander took a model that had never seen the data before and asked it to forecast what would happen next (no training step, just a prediction). Pedro Martin, Santander AI Scientist, explains that “time-series foundation models reduce the need to build and maintain separate models, such as ARIMA, SARIMAX or Prophet, for every forecasting use case. By reusing patterns learned from large and diverse datasets, they can produce strong forecasts without fine-tuning, reducing development costs, accelerating deployment and enabling deployment across thousands of client (or product) level series. Potential applications include forecasting balances, transaction volumes and liquidity”.


##


Train once, reapply on demand


A traditional forecasting model is trained on one series, such as daily card transactions for a given product, and learns that series alone. A time-series foundation model is trained once, on an enormous and varied collection of series, and can then forecast a series it has never seen before, with no additional training. This is called zero-shot forecasting, and a year ago it sounded implausible.


The current generation makes it credible. Google's TimesFM was pretrained on roughly one hundred billion time points. Amazon's Chronos-2 (late 2025) tokenises a series and passes it through a transformer, handling not just one series but many related ones and external drivers at once. TabPFN-TS, from Prior Labs, reaches state-of-the-art forecasts with only about eleven million parameters and was pretrained entirely on synthetic data. Using one of these looks less like machine learning and more like calling a function:


from chronos import ChronosPipeline


pipe = ChronosPipeline.from_pretrained("amazon/chronos-2")


forecast = pipe.predict(context=recent_history, horizon=30) # no training step


##


Raising the bar


In a bank, there are thousands of series to forecast (by product, segment, branch and currency) and many are short, sparse or new: precisely the kind of cold-start cases in which a model built from scratch struggles and a pretrained one shines. The same models give strong out-of-the-box baselines for anomaly detection, and a fast baseline is valuable because it tells you whether a bespoke model is actually earning its complexity. On the tabular side, foundation models such as TabPFN are beginning to do the same for classification on the structured data that makes up most of our world.


None of this makes classical methods obsolete. A well-specified statistical model, or a gradient-boosted tree tuned to a problem we understand, still often wins, especially where the series has strong, known structure. The honest posture is to treat a foundation model as a strong, cheap starting point and a benchmark to beat, not as an automatic upgrade.


In a regulated institution, a model that produces a number that feeds a decision is not merely a convenience. It is something that must be validated, explained and governed. Foundation models raise the bar here, because their internals are harder to interpret, and because using a pretrained model is not, by itself, an account of why a forecast can be trusted. Evolving model-risk expectations across the industry apply to these systems just as they do to any other model. The approach, therefore, is both practical and cautious: they can compress weeks of work into an afternoon, and that speed only creates value if the results are measured against a meaningful baseline, the model’s failure modes are understood and a human remains accountable for the decision it informs. Used that way, the arrival of foundation models in tables and time series is not a threat to careful analytics. It is a faster way to produce an initial analytical baseline. Title: Foundation models come to tables and time series.
