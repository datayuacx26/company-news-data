---
schema_version: "1.0.0"
document_id: "2cea97eb47013d1ce910738babd6ef6af5a963d5f19df64298eae3bf11fd4c8d"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/built-once-serves-many-how-we-rebuilt-experiment-analysis-platform-at-roku"
published_at: "2026-07-23T14:27:15+00:00"
first_seen_at: "2026-07-23T15:12:43.499298+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:e663196454f700792764c37aaa74543a8d898fd558dd8fbf36e28fdae8ea9ab8"
---

# Built Once, Serves Many: How We Rebuilt Experiment Analysis Platform at Roku

By Abhilash Mittapalli and Milind Nandankar


At Roku, majority of the product decisions are validated through A/B testing. At any given time, hundreds of concurrent experiments run across our platform, each requiring statistical analysis before a go/no-go call can be made. On any given day we generate hundreds of experiment analysis reports consumed by PMs, ML engineers, and data scientists across domains like Recommendations, Search, Merchandizing, etc. As the experimentation volume grew, our analytics infrastructure became the bottleneck — not the experiment itself.


The cost was direct: product teams waited weeks for new KPIs to be added for analysis that should have taken hours, while data scientists spent the majority of their time on pipeline plumbing rather than focusing on business logic.


Read on to learn how we cut metric onboarding time from two weeks to days, stopped drifts in analytical calculations, and ensured metrics have clear ownership and lineage for business logic


## **Background**


Experiment analysis at Roku is not a single operation. We support 9 distinct analysis methods, each with its own parameter space and ETL requirements. User segmentation analyses require demographic parameters; breakdown analyses require event-level identifiers. This diversity is intentional — different product hypotheses require different statistical approaches.


Our legacy system evolved organically over years alongside our business. What started as a focused set of scripts became a tightly coupled monolith with diffuse ownership. Three failure modes defined it:


1. **Onboarding and maintenance overhead** . Even when the underlying compute logic was identical, each new metric required teams to repeat the same plumbing work. Typical onboarding took 2–3 weeks. Adding a single metric required updates across 3–4 table definition files(DDLs), and over time this led to hundreds of redundant tables across environments.
2. **No source of truth for metric definitions** . Metric definitions and ownership were scattered across MRs and internal documentation — answering “what does this metric actually measure?” required manual archaeology.
3. **Silent ETL drift** . Core aggregation logic was duplicated across places, diverging silently over time. A concrete example: some pipelines computed a plain average while others applied a normalized average scoped to identifiers meeting a specific enrollment criterion — with no mechanism to detect the discrepancy or determine which implementation was authoritative.


The design constraint we committed to was that data scientists declare business logic, not infrastructure. Physical DDLs, statistical computation, and experiment allocation and enrollment logic should be invisible to the metric author.


## **Architecture**


We decomposed metric development into two artifacts:


1. A **SQL file** containing the metric’s business logic — the grain, event filters, breakdowns, and base aggregation
2. A **YAML config** file specifying how to aggregate that metric, along with metadata: owner, applicable breakdowns, aggregation type, etc.


example_metric_config.yaml example_streaming_engagement.sql


In the example above, the metric definition and metadata tagging are simplified. Data scientist defines SQL with metrics like sessions and bounced sessions used for experiment evaluation. Binarization of streaming hours calculates the average number of users with nonzero streaming hours. Bounce rate is the ratio of bounced sessions to total sessions, averaged at the variant (test vs control) level. None of these files handle experiment metadata like variant or enrollment, nor do they create new tables, manage intermediate data, or write to the final results table; everything is handled behind the scenes and abstracted from the developer.


At runtime, analysis requests arrive via the Airtable API. Each request carries an experiment ID, analysis type (power analysis, lift analysis, etc.), analysis window, and segmentation parameters. A Python enrichment layer then:


- Joins request parameters against experiment metadata: enrollment dates, variant assignments, and allocation windows.
- Normalizes all metrics into a long format by pivoting each metric column into a row, eliminating per-metric-type schema maintenance.
- Injects breakdown computation logic for the requested segmentations.
- Emits Airflow tasks with the correct dependency graph for the requested analysis type.


The orchestration layer parses the runtime config to select the correct ETL path, then routes execution to either Trino or Spark based on cluster utilization and estimated data volume configured based on task profiling. High-volume, computationally complex analyses run on Spark; lower-volume analyses run on Trino, where query latency is lower. We also have a fallback to run tasks in Spark if our attempt to run ETL in Trino fails due to memory constraints.


Results land in our GCS data lake and are served through Looker or our experimentation platform UI.


Statistical methods are pluggable and can be extended. For instance, if certain metrics are better evaluated with delta test rather than a t-test we have the option to configure it. Because every analysis type consumes the same standardized long-format output, a new statistical method (such as Bayesian interpretation) can be added without touching the ETL layer.


## **Implementation Challenges**


**Migrating at Scale**
Migrating hundreds of KPIs across dozens of teams was equal parts org change and engineering. The new framework changed metric semantics (no raw-total comparisons; only averages/ratios). Some teams lost legacy features, so we paired the rollout with deliberate stakeholder management and migration support.


**Enrollment-Dependent Metrics**
Some metrics depended on the user’s enrollment day — at odds with our goal of hiding enrollment logic from authors. We added a controlled escape hatch: metric owners can supply fully custom SQL that takes over context injection and bypasses standard enrichment when needed.


**Non-Additive Unique Counts**
Distinct-user metrics aren’t additive across time; they require global deduplication over the experiment window. We added built-in support for non-additive uniques so authors don’t have to hand-roll dedupe logic.


**High-Cardinality Breakdowns**
High-cardinality dimensions made breakdowns expensive. We made cardinality a first-class config: developers cap values (e.g., top 20/30) to trade theoretical completeness for predictable performance.


**Validation and Migration Fidelity**
Migration was a multi-quarter effort. Each metric was validated against historical results, often surfacing bugs in the original logic that we fixed before certification. We optimized for correctness over speed by design.


## **Results**


Refactoring from a monolith to a declarative, pluggable framework produced measurable improvements:


- **KPI development time dropped from weeks to under 2 days** . Metric authors now write SQL and YAML; the platform handles ETL, statistical computation, and orchestration.
- **Metric definitions are versioned and owned** . Every metric has a declared owner and a single authoritative implementation, auditable via git history. Answering “what does this metric measure and who owns it?” is now a lookup, not an investigation.
- **Aggregation logic is implemented once** . Statistical computation and enrollment logic live in a single place — silent drift between metrics is structurally impossible.


## **Looking Ahead**


Two investments are in progress.


**Scalability** : We are migrating to a more robust infra with dedicated compute to handle peak seasonal load like holiday traffic spikes without the complexities involved with using a shared infrastructure. Our target is to cut down analysis request-to-delivery time by 50%


**LLM-powered analysis summaries** : We are integrating automated, plain-language interpretation of experiment results into the platform. The goal is to reduce the time between statistical output and a decision-maker understanding what it means. Engineers get per-metric detail; leaders get a one-line read on experiment status — without opening a dashboard. Our pilot rollout has driven strong engagement and acceptance and will expand to broader teams in the next phase.


## **Acknowledgements**


This milestone was made possible through the invaluable guidance of our leadership team—Andrew Moskowitz, Rupa Kommineni, Kajal Poply, and Olga Natkovich—alongside our PM, Gbenga Awodokun. Special thanks also go to the incredible project team: Balaji K Ganesh, Simon Tsai, Abhilash Mittapalli, and Milind Nandankar


The post[Built Once, Serves Many: How We Rebuilt Experiment Analysis Platform at Roku](https://engineering.roku.com/built-once-serves-many-how-we-rebuilt-experiment-analysis-platform-at-roku) appeared first on[Engineering Blog](https://engineering.roku.com/) .
