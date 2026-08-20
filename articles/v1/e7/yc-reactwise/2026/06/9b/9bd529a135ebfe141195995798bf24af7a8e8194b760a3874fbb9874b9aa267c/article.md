---
schema_version: "1.0.0"
document_id: "9bd529a135ebfe141195995798bf24af7a8e8194b760a3874fbb9874b9aa267c"
company_key: "yc-reactwise"
company: "ReactWise"
source_id: "yc-reactwise-news-import-144b6e87acf7"
canonical_url: "https://www.reactwise.com/news/a-model-trained-only-on-what-worked-has-an-incomplete-view-of-the-reaction"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-22T10:59:50.628827+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:355752763ec74e9743a1617505aa79071a4d33e642857e3d19ac2fe1a272ea4f"
---

# A model trained only on what worked has an incomplete view of the reaction.

A model trained only on what worked has an incomplete view of the reaction.


In most labs, low-yield reactions are treated as dead ends. The conditions that went nowhere are recorded inconsistently, or never written down at all. But a model doesn't only learn from success - Bayesian optimization builds a picture of the entire response surface, and negative results are what define its edges. By balancing exploration and exploitation in this way, the model suggestions can appear odd to a trained chemist but are crucial; they show where performance falls away and which regions aren't worth revisiting.


This is why we treat negative results as data, not waste. In our HTE lab, we deliberately generate and retain the full range of outcomes across the reaction classes most relevant to process chemistry - Buchwald-Hartwig, Suzuki, and amide couplings. The 25,000+ data points we've generated include the conditions that didn't work, because those are part of what makes the resulting models reliable.


It's also what makes MemoryBO effective at warm-starting new campaigns - carrying forward the dead ends so teams don't spend early experiments rediscovering them.


And with HTE Data on Demand, teams can request these datasets directly rather than building that foundation from scratch.


The reactions that don't make it into the final report are just as important. The question is whether your workflow is set up to remember it.


‍
