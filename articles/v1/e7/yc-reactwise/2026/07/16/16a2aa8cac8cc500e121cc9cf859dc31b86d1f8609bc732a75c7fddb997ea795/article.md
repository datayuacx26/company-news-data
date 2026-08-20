---
schema_version: "1.0.0"
document_id: "16a2aa8cac8cc500e121cc9cf859dc31b86d1f8609bc732a75c7fddb997ea795"
company_key: "yc-reactwise"
company: "ReactWise"
source_id: "yc-reactwise-news-import-144b6e87acf7"
canonical_url: "https://www.reactwise.com/news/a-model-that-doesnt-know-a-solvents-boiling-point-will-happily-recommend-conditions-above-it"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-22T10:59:50.628827+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:b66c941a17ebce99b3903fbea7282cde896b78b106d2e3c6b06867dbfd9dc917"
---

# A model that doesn't know a solvent's boiling point will happily recommend conditions above it

A model that doesn't know a solvent's boiling point will happily recommend conditions above it.


Most optimization discussions focus on the objective - yield, impurity, selectivity. But every real campaign is bounded by constraints: the accessible temperature range of a solvent, the values a reactor or pump can actually be set to, reagent compatibilities, and how experiments have to be grouped on a plate.


A model that ignores these will recommend conditions that look excellent in theory, but can't be executed at the bench - slowly eroding trust in the tool.


We build these constraints into the optimization itself, so every recommendation is one you can actually run.


Temperature is bounded by the solvent: the platform won't propose conditions outside a solvent's accessible range, so a screen across several solvents respects each solvent's limits.


Continuous inputs can be discretized to values you can actually dial in, and where parameters depend on one another, both linear constraints, such as stoichiometry or total volume, and conditional "if X, then Y" constraints, such as restricting the compatible solvents once a base is chosen, are honored throughout.


The feasible space is defined up front, and the optimizer searches within it - rather than proposing points just to be rejected.
