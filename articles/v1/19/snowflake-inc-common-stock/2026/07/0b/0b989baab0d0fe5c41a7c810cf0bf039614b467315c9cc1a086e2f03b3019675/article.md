---
schema_version: "1.0.0"
document_id: "0b989baab0d0fe5c41a7c810cf0bf039614b467315c9cc1a086e2f03b3019675"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/ml-jobs-snowflake-data-clean-rooms"
published_at: "2026-07-08T20:43:00+00:00"
first_seen_at: "2026-07-24T01:53:21.113388+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:ccf1d5b3898eb4d31b956b9b2aa2c6bd4e6cf8145e02d5c58015ae32805eb374"
---

# ML Jobs in Snowflake Data Clean Rooms Now GA

Until now, running complex ML workloads inside a data clean room meant hitting a wall. Most clean room environments are limited to SQL queries or single-node Python, which runs into memory constraints quickly at enterprise data volumes. Teams ended up treating clean rooms as a compliance tool rather than a place to build models. ML Jobs changes that. ML Jobs in Snowflake Data Clean Rooms™ is now generally available.


Data scientists can now bring their standard Python ML stack with distributed training, hyperparameter optimization, custom packages and GPU compute directly into a multiparty collaboration. Models train on combined data from multiple organizations without raw records leaving anyone's account, and the pipeline runs automatically rather than requiring manual intervention each time.


Consider a concrete example from advertising. An advertiser builds audience and measurement models using ad log data from publishers, identity resolution data from identity providers and transaction signals from retail data partners. Each data source improves model quality. However, each provider has legitimate concerns about how their data is used and who can see it. At the same time, the advertiser's model logic and scoring algorithms are proprietary IP they have no interest in exposing to their data partners.


ML Jobs in Data Clean Rooms gives each party what they need. Data and identity providers know their data is governed and used only for workloads they have explicitly approved. The advertiser's model code stays inside the collaboration boundary, invisible to the data providers. As new data flows in, models score continuously on fresh signals without any party renegotiating access or re-exposing their assets.


The same infrastructure also points toward what's next: training AI agents whose effectiveness depends on signals distributed across multiple organizations. This includes a publisher's behavioral data, an advertiser's conversion history, a data provider's demographic enrichment, all combined in a single fine-tuning run inside a governed clean room. That's where multiparty ML is heading, and the foundation is available today.


*“Snowflake's ML Jobs release in DCR enabled us to bring VideoAmp's best-in-class lift methodology into a dramatically more scalable computing environment. We can now process lift reports faster, using more signals than ever before — unlocking greater value for our clients at scale.” —Katy Mitchell, Sr Director Ad Measurement Products, VideoAmp*


*"At Affinity Solutions, we're proud to use ML Jobs in Snowflake Data Clean Rooms. For advertisers, the ability to train and score models on combined data without raw records leaving anyone's environment solves a problem that has blocked serious measurement and targeting work for years. What excites us most is the foundation this creates for what's next: AI agents continuously trained on our consumer purchase data and our clients' data in a safe, secure and seamless way. ML Jobs makes data collaboration genuinely practical, and that's what the future of advertising will be built on.” —Kalyan Lanka, Chief Product Officer, Affinity Solutions*


*"With Snowflake's ML Jobs now available in Snowflake's Data Clean Room Collaboration API, Kroger Precision Marketing is able to connect our industry-leading media measurement sciences natively within Snowflake Data Clean Rooms. This innovation unlocks our best-in-class incrementality sciences, allowing KPM to deliver trusted, privacy-first insights at the speed and scale modern marketers demand." —Nathan Dall, Product, Data Collaboration & AI, Kroger Precision Marketing*


### What this enables


Some of the most valuable ML models in advertising, financial services and healthcare require data from multiple organizations that have generally not been able to share it because moving it across company boundaries creates privacy, or competitive problems. Clean room ML is designed to address that directly.


#### Multiparty model training and scoring


A machine learning model trained on a single data silo reflects only that silo's view of the world. A retail media network, a financial services provider and a brand each have distinct signals about consumer behavior such as purchase history, transaction patterns and engagement data that are far more predictive in combination than in isolation.


ML Jobs makes it practical to run a single training pipeline across feature sets from multiple parties. Each organization contributes to the model without exposing raw records to the others. The accuracy improvement from richer, multi-source features is one of the clearest arguments for collaborative ML.


Propensity scoring is a direct example of this. A publisher scores their full user population for purchase propensity on behalf of an advertiser, but a propensity model trained only on the publisher's behavioral signals is limited. Bring in the advertiser's first-party conversion history and a data provider's demographic and psychographic signals, and the same model trains on a far richer feature set: content affinity and session behavior from the publisher, income tier and life stage from the data provider, and actual purchase outcomes from the advertiser. The resulting scores are meaningfully more predictive because the model has seen the full picture of what drives conversion. Models that previously relied on a single publisher's behavioral signals can now be retrained with each new campaign using the full set of available features, keeping scores fresh rather than degrading as audience composition shifts.


#### Ease of development, deployment and scale


For ML engineers and data scientists, the experience inside a clean room collaboration is the same as outside it: Write standard Python, use any packages you need and work in whatever IDE or notebook environment you prefer. The only addition is a short YAML code spec that declares what to run and what it needs:


```text
ml_jobs:
- name: train_model
entrypoint: train.py
stage_code_dir: '@my_db.public.ml_stage/project'
pip_requirements:
- xgboost
- scikit-learn
- pandas
```


No Docker images to build, no container registries to configure, no infrastructure to provision manually. Scaling to multiple nodes or GPUs is a parameter change, not an architectural rewrite. Registering a new version of a job, updating requirements or adding a pipeline step is a spec change. Once a template is approved, it runs via a single SQL call from any orchestrator on whatever schedule you set.


The development workflow is also designed to avoid friction at the clean room boundary. You can build and test the full ML Job workflow outside Snowflake Data Clean Rooms (DCR) in a standard Snowflake environment using the same Python scripts and container runtime. Once you’ve built and tested, you can bring it into the collaboration using the code spec with a pinned image version to lock down the versions of dependencies you have developed and tested with. This means iteration happens in your normal development environment, and you can seamlessly deploy and operationalize the workloads in DCR.


ML Jobs workloads are designed to run as production pipelines, not one-off experiments. Once a template is approved, you can schedule it at any cadence, trigger it from upstream data events or orchestrate it from any automation tool such as Snowflake Tasks, Airflow or any system that can make a SQL call.


Operationally, activity history for all job runs is also queryable in Snowflake, giving you an audit trail of analysis runs. And when something goes wrong, you can access the standard container logs for debugging by allowing monitoring on the ML Jobs.


### Use cases


#### Incrementality measurement without a data intermediary


Measuring true sales lift from advertising requires joining ad exposure data with purchase outcomes across party lines. Historically, this required a neutral third party to hold the combined data or significant custom infrastructure — high-friction paths that most advertisers skip.


With ML Jobs, a brand and a retailer run an uplift model inside the collaboration: impression logs stay in the brand's account, transaction data stays in the retailer's account and the model trains on the join. Causal lift measurement, which previously required a dedicated data science project, becomes a repeatable per-campaign workflow. Campaign teams get a real signal on what's working, not a proxy metric.


#### Retail media attribution at transaction scale


Retailer transaction data is one of the most valuable signals for attribution, but has been practically inaccessible to media agencies and DSPs due to competitive sensitivities. ML Jobs lets the attribution model run where the data lives. The retailer's records never move. The model output including attribution weights and conversion lift estimates get shared downstream.


#### Identity crosswalk for higher match rates


Third-party cookie deprecation has reduced deterministic match rates significantly for many advertising programs. When hashed email or device ID coverage is partial, deterministic joins leave a large portion of the audience unmatched. Closing that gap requires joining first-party advertiser data such as customer relationship management (CRM) records and conversion history with third-party identity provider graphs that map device IDs, hashed emails and mobile ad IDs across publishers.


Probabilistic identity resolution, a model trained to match records across ID spaces using behavioral and contextual signals, can recover meaningful match rate lift, but it requires scale and dedicated compute. ML Jobs provides that inside the clean room: An advertiser's first-party CRM data and a third-party identity provider's graph train a resolution model without either party's records leaving their account. Where deterministic joins leave large portions of the audience unmatched, a well-trained resolution model closes that gap and because the model can be retrained as ID coverage shifts, match rates don't simply degrade over time and stay degraded.


### From lookalike scoring to campaign optimization agents


Lookalike modeling produces a ranked list of users who resemble a seed audience, a static score at a point in time. A campaign optimization agent goes further: Given a campaign brief, budget and objective, it reasons over combined signals from multiple parties to recommend which audience segments to target, in what context and at what bid level.


The difference in training data is what makes this possible in a clean room. A lookalike model trains on a publisher's user features against an advertiser's seed audience. A campaign optimization agent trains on publisher behavioral signals, advertiser conversion history across multiple campaigns and product categories, and data provider demographic enrichment simultaneously. It learns not just which users look like past converters, but which combinations of audience context, demographic profile and campaign type drive conversion and generalizes that learning to new campaigns the model has never seen before.


The three-party clean room provides the governance structure that makes all three parties willing to contribute: The publisher's behavioral data never leaves their account, the advertiser's conversion history and model logic stay within the collaboration boundary and the data provider's demographic graph is used only for the approved training workload. Two parties and a warehouse can build a lookalike model. A campaign optimization agent requires multiparty training data, GPU compute and the trust model that clean room ML provides.


### How it compares to other approaches


Certain clean room platforms design their ML APIs around tailored use cases, such as lookalike audience generation or specific custom model formats. Others utilize centralized, shared notebook environments, which are well-suited for exploratory data science and manual collaborative review.


ML Jobs in Snowflake Data Clean Rooms expands on these approaches by supporting standard, end-to-end Python ML workloads optimized for automated production pipelines. Data remains within each party's respective account, utilizing a hash-based approval system that enables seamless, automated re-runs without requiring manual intervention for every execution. Additionally, when both parties enable Cross-Cloud Auto-Fulfillment, workloads run seamlessly across different clouds and regions. To efficiently scale to the needs of each project, the analysis runner can select a compute pool directly within their account that matches the workload—ranging from standard or high-memory CPUs for large feature sets, to GPU instances for accelerated tasks like deep learning and LLM fine-tuning (in supported regions).


### Get started


ML Jobs in Data Clean Rooms is now available for all Snowflake accounts with the Data Clean Rooms environment installed. No account-level enablement required.


For ML engineers and data scientists: Two end-to-end examples are available, each with a sample data generator, SQL worksheets for both parties and Python training and scoring scripts you can run today:


-


[Lookalike audience modeling with ML Jobs](https://docs.snowflake.com/user-guide/cleanrooms/collab-lookalike-modeling#option-2-ml-jobs) : Advertiser trains a lookalike classifier on publisher user features and activates the scored audience.


-


[Multiparty incrementality measurement with automated hyperparameter optimization (HPO):](https://docs.snowflake.com/user-guide/cleanrooms/collab-mljobs-rmn-hpo) Brand and retail media network run per-campaign uplift modeling with Bayesian hyperparameter optimization.


For campaign managers and media buyers evaluating this for measurement or targeting workflows: The incrementality and lookalike examples above are the fastest way to understand what's possible. Share them with your data science or measurement team. The examples include everything needed to run end to end, and the workflow maps directly to standard campaign and activation patterns you're already using.


Full reference documentation:[ML Jobs in Data Clean Rooms](https://docs.snowflake.com/user-guide/cleanrooms/ml-jobs) .
