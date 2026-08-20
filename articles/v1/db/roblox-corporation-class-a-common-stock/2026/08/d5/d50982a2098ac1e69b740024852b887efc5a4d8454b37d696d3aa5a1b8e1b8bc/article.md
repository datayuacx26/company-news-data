---
schema_version: "1.0.0"
document_id: "d50982a2098ac1e69b740024852b887efc5a4d8454b37d696d3aa5a1b8e1b8bc"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-13cc9c892e31"
canonical_url: "https://about.roblox.com/newsroom/2026/08/beyond-selfie-how-roblox-age-assurance-system-helps-keep-age-checks-current-"
published_at: "2026-08-04T12:00:00+00:00"
first_seen_at: "2026-08-05T10:22:21.162203+00:00"
fetched_at: "2026-08-05T10:22:23.429305+00:00"
content_hash: "sha256:a767b394a1533ab363a7d4cf550c2ec453772b004f3f93774a298e7ff8a1e435"
---

# Beyond the Selfie: How Roblox’s Age-Assurance System Helps Keep Age Checks Current

Share


[Engineering](https://about.roblox.com/newsroom?filter=engineering)[Safety + Civility](https://about.roblox.com/newsroom?filter=safety%20%2B%20civility)


# Beyond the Selfie: How Roblox’s Age-Assurance System Helps Keep Age Checks Current


A Modular, Multimodal Behavioral ML System That Prioritizes Safety


By


Xixi Wang


Published


Aug 4, 2026


Roblox’s[Facial Age Estimation](https://about.roblox.com/newsroom/2025/11/roblox-requires-age-checks-limits-minor-and-adult-chat) system has processed 338 million age-check records to help Roblox users unlock age-appropriate chat and access age-appropriate games on the platform. The initial age check is necessary, but age assurance doesn’t end at the selfie. Accounts could be shared or handed off to a younger sibling. Bad actors could attempt to probe our defenses. To move age assurance beyond a point-in-time signal, we built a high-throughput ML layer that evaluates whether an account’s ongoing behavior still matches its estimated age over time. Our feature-level modular design enables efficient model updates and rapid adaptation to shifting behavioral patterns.


Reliably operating our end-to-end age-assurance workflow is as much a distributed systems challenge as an ML challenge. Age estimation itself is a difficult ML problem because age boundaries are not behavioral cliffs. Parents may enjoy the same experiences as their children, and language varies across culture, context, and community as well as age. No single behavioral signal can reliably capture nuanced behavioral patterns that shift over time and under adversarial pressure. Even our strongest ground truth labels come from a selected population and can carry bias.


Operating at Roblox scale adds additional complexity. In each daily inference cycle, we assemble behavioral signals from across the platform, run submodels on the compute stack best suited to each workload, and combine their outputs into one calibrated estimate. Our models range from traditional ML classifiers to multilingual transformers with over 300 million parameters. The system processes close to two billion account-level records each day, while training draws on nearly 30 million high-confidence age labels.


Building the entire system in-house gives us the transparency and control we need to review all features and signals for privacy, which is a central design requirement. We can also use Roblox-specific signals that capture how an account is used over time—longitudinal context that a standalone external estimator would not have on its own. It also gives us control over the full ML life cycle. We can combine modalities as they become available, and calibrate outputs for Roblox safety workflows while monitoring, gating and updating individual components as behavior changes.


###### Choosing the Right Signals to Predict Age


Different age groups navigate Roblox differently across play, communication, and platform interaction patterns. To capture behavior diversity, we break our inputs down into distinct signal families:


- **Social context:** Aggregate patterns in a user’s social circle, including who they connect with and how often they interact.
- **Experience engagement:** Aggregate patterns that go beyond what users play, capturing when they play, how long they stay, and how their engagement changes over time.
- **Account and platform activity:** Aggregate platform usage and life cycle signals, such as account tenure, activity frequency, and session-level behavior.
- **Communication patterns:** Model-derived summaries of age-correlated language usage, including how communication patterns vary across context and time.
- **Learned user representations:** Higher-level representations that capture broader usage patterns. We work closely with our Discovery and Personalization ML team; the same rich representations that predict what a user may engage with next can be post-trained to become predictive of broad age bands.


Each signal family gets a model that fits the shape of its data. Sparse, high-dimensional aggregated signals are handled by gradient-boosted decision trees (e.g., XGBoost classifiers); text is handled by a fine-tuned multilingual transformer encoder (mmBERT); and user representations feed some compact neural networks (Multilayer Perceptron).


Each submodel predicts the same target: a probability distribution over a set of age bands. A gradient-boosted meta-learner then combines those probability vectors into the final calibrated estimate.


We deliberately chose this decision-level fusion approach over one monolithic feature-level model for modularity, velocity, and explainability. Making the system modular allows us to add, retrain, or retire a submodel without rebuilding the whole system. It also allows us to respond more quickly as behavior patterns shift due to natural evolution or adversarial pressure. The fusion layer reads each submodel’s latest promoted output so it can use the best available signals and degrade gracefully when one is absent or delayed. Because each modality enters as a distinct input, we can trace which signal families are driving decision changes, and apply calibration and quality gates before outputs are consumed downstream.


We enforce calibration at the fusion layer because in a safety system, predictability beats pure accuracy. We calibrate the models so that they give us confidence scores we can trust. That way, if a downstream action is going to heavily impact a user, we can require stronger evidence and higher confidence before consequencing.


###### Selection Bias in Labels


Our highest-confidence labels come from users who have completed a strong age check. These labels are useful, but they are not a random sample of Roblox users. For a system that can affect user access, representativeness is a core part of the training objective.


The labeled population is not the target population. Users who complete stronger age checks differ systematically from the broader population. A naive model trained on that group risks learning who chooses to complete the process as much as it learns age. To reduce that selection bias, we reweight training and evaluation data so the labeled sample better reflects the population we intend to score.


We work cross-functionally with user research, using aggregated survey results to inform the population targets used for weighting. From there, we use techniques such as inverse propensity weighting and iterative proportional fitting—or raking—to align the weighted sample across multiple dimensions at once instead of correcting for one factor in isolation.


Not every high-confidence source produces a usable training example. A label is only as reliable as the verification events behind it. Some events could carry signals that make them a poor fit for model training, such as unusual verification patterns or account-integrity concerns.


We strive to clean labels with integrity-aware label filtering or label error identification approaches like[Confident Learning](https://arxiv.org/abs/1911.00068) before training to remove examples that look out-of-distribution for behavioral modeling. Early experiments show that this kind of cleanup helps keep the model from overfitting and provides consistent performance gains.


###### Training, Inference, and Avoiding Skew at Roblox Scale


Inference needs to run frequently enough to keep signals fresh, but training cadence can vary by signal family. We choose retraining schedules based on feature drift, model stability, and model cost. A signal that changes or degrades quickly can retrain more often; a heavier model with slower-moving performance can retrain less frequently or on demand.


The compute stack follows the shape of the workload. We use[Apache Spark](https://spark.apache.org/) for large-scale structured transformations,[Ray framework](https://docs.ray.io/en/latest/index.html) for GPU-backed workloads, and production workers for real-time signal processing.


At Roblox’s scale, rebuilding every account’s complete history each day would also not be practical. Our in-house data-processing framework maintains bounded, rolling histories for high-volume signals and materializes expensive transformations in stages. This lets us process and aggregate millions of account-level records each day while scaling large expansions, joins, and feature aggregations independently. This hybrid production design gives us direct integration with our data warehouse, reproducible execution across model components, and access to accelerated compute where it matters.


To help maintain stability at our scale, we enforce two strict design decisions across our feature pipelines:


- **Store derived features:** We store derived features rather than raw activity streams wherever possible. We keep the signal we need for modeling, not the full history behind it.
- **Unified data contracts:** The same data contracts feed both training and inference. That reduces one of the most common failure modes in ML systems: skew between what was trained and what is served.


We use gating at two points in the production life cycle. First, model promotion is separate from model training. A candidate model must pass guardrail checks before it becomes eligible for production inference. If a candidate fails our guardrail checks, the pipeline blocks the promotion and we keep serving the latest promoted good version.


Second, inference output is gated before it is published downstream. Automated checks look for quality regressions and large prediction-distribution shifts. If a check fails, the publish step is blocked. In an ML system attached to user-facing safety workflows, “do nothing today” is often a safer failure mode than shipping an unexplained distribution shift.


###### Measuring the System


For a system like this, a single accuracy number is not very meaningful. We evaluate model quality across ranking quality, calibration, stability over time, and behavior near important age-band boundaries. We also treat representativeness of the evaluation data as part of the measurement problem, because an aggregate improvement is only useful if it reflects the population that the system is meant to serve.


The fused model reaches a mean average precision of over 0.9 on this weighted, held-out evaluation data. Its high-confidence estimates are also consistent from day to day. More than 99% remain in the same broad age band on consecutive days.


We also compare the model against independent ground truth produced by trained human reviewers through our Trust and Safety workflows. At high confidence, the model’s age-band estimate agrees with the human consensus label for adults more than 80% of the time.


Offline evaluation is only one layer. In production, user behavior changes with seasonality, product launches, and adversarial pressure. That is why we pair model-evaluation metrics with ongoing monitoring of prediction distributions, drift, and stability before outputs are used downstream. We also monitor outcomes from follow-up age checks, so that production feedback can inform the reliability of the model and indicate where thresholds or review paths should be adjusted.


###### What’s Next


Owning this system in-house lets us leverage Roblox-specific signals safely, operate with extreme rigor, and continuously iterate.


Moving forward, we’re focused on optimizing our submodels, inference, and measurement. We push each layer further while focusing on our operating principles of modularity, calibration, privacy, and production guardrails.
