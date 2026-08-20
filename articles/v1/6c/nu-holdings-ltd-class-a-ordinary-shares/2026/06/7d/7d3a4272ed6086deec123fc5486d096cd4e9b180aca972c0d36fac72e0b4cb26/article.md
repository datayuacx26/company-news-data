---
schema_version: "1.0.0"
document_id: "7d3a4272ed6086deec123fc5486d096cd4e9b180aca972c0d36fac72e0b4cb26"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-b653f977f25a"
canonical_url: "https://building.nubank.com/feature-stores-for-real-time-ml-why-and-when-to-centralize-feature-logic/"
published_at: "2026-06-10T19:26:27+00:00"
first_seen_at: "2026-07-20T04:36:16.841262+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:b6f2a13c199b5d2e0385dd09a5dda6feee0a58a77f776d24341ac1a18ab46cee"
---

# Feature stores for real-time ML: Why and when to centralize feature logic

Written by:[Felipe Almeida](https://www.linkedin.com/in/felipeqbalmeida/?locale=pt) , with contributions from[Luiz Felix](https://www.linkedin.com/in/lzfelix)


*This is the first part of a three-part series about feature stores for real-time ML at Nubank. In part 2, we’ll explore the lessons we learned from using feature stores in production. In part 3, we’ll walk through end-to-end architectures used in real-world scenarios.*


## **Introduction to feature stores**


One of the few constants in software development is that systems tend to accumulate entropy and grow more complex over time. It’s possible, however, to slow down this decay by extracting common functionality into libraries and platforms. This simplifies systems and keeps complexity at manageable levels.


As a data-driven organization grows and matures, ML code *also* tends to become increasingly complex. For example, when models start being used in multiple teams, it’s common to see feature logic duplicated in several places. This makes it much harder to manage and maintain these models, and prevents code reuse across the organization.


> *“All problems in computer science can be solved by \[adding\] another level of indirection”*
> – *David Wheeler*


A common strategy to address this problem is to centralize all feature logic in a library so that teams can reuse each other’s features instead of having to rewrite and duplicate them whenever they train a new model.


When models are used for real-time inference, there is *yet* *another* layer of code involved, namely the code used to retrieve features at inference-time. This “inference-time” code usually sits in microservices in the production environment.


**Feature stores** are platforms that centralize feature logic for all models in an organization. In real-time inference scenarios, the feature store also provides centralized endpoints for feature retrieval.


> *A feature store is a platform that centralizes feature management (at training-time and at inference-time)* .


Let’s look at two examples of what ML code looks like with and without a feature store, both at training-time and inference-time.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## **Training-time**


At training-time, using a feature store means that each team will *import* feature definitions from a standard library, rather than defining their own team-specific ones. In Figure 1 below, we show examples of what training-time code would look like with and without a feature store:


***Figure 1: From feature_store import:*** *At training-time, data science teams usually take a base dataset containing entity IDs and timestamps, then iteratively “apply” features on top of it. With a feature store, this “feature logic” is extracted into a reusable shared library.*


## **Inference-time**


At inference-time, features need to be retrieved by some kind of orchestrator. In microservice-based architectures, this orchestrator will be a service (called “Upstream service” in Figure 2 below) that will build the payload, send it to the real-time ML model and handle the model’s predictions:


***Figure 2:*** *A feature store abstracts away all the complexity involved in retrieving, calculating, and transforming features used in real time.*


## **“Direct-call” vs streaming-based feature serving**


To provide a feature to an upstream service, the feature store needs to *first* retrieve this information somehow. The two most common strategies are:


- **(1) Direct call:** In this model, the feature store makes HTTP calls directly to the microservices that own the required data whenever a feature is requested.
- **(2) Streaming** : Here, microservices publish events to a streaming layer, such as Kafka, as events happen. The feature store then consumes these events and retrieves features from the streaming layer when needed. In some cases, these features are also stored in local databases for faster access.


These two strategies are illustrated below:


***Figure 3:*** *Features can be retrieved directly from source services or through streaming platforms like Kafka. Streaming-based features are often called* ***near-real-time*** *because they may have small update delays, typically on the order of milliseconds or seconds.*


A single feature store may support *both* strategies simultaneously. It’s very common for *some* features to be retrieved directly from primary sources (i.e. case (1)), while others are retrieved from streaming platforms.


Each strategy comes with different advantages, drawbacks, and failure modes. We’ll discuss those in the next part of this series.


**In the next sections** , we’ll list the advantages and disadvantages of using feature stores, in our experience at Nubank.


## **Pros**


## **Pro: Feature stores mitigate training-serving skew**


[Training-serving skew](https://building.nubank.com/dealing-with-train-serve-skew-in-real-time-ml-models-a-short-guide/) refers to differences in how features are defined at training-time and inference-time.


These mismatches break the assumption that inference-time data will follow a distribution similar to the one used during training. When that assumption fails, performance metrics estimated during training stop reflecting the model’s real-world behavior.


Training-serving skew happens because feature definition at training-time is done in the **analytical** **environment** , by data scientists, while at inference-time it’s often in the **production** **environment** , by engineers. Since it’s done by different people, in different environments (often using different stacks), differences in implementations are very common and hard to detect. See a summary in Figure 4 below:


***Figure 4: Differences between training-time and inference-time:*** *Model work is done not only by different people, but also in different environments and often using different stacks. These differences are the main cause of training-serving skew.*


If the feature store can handle both the training-time data and the inference-time retrieval, it can eliminate, or at least mitigate, training-serving skew, because both definitions now live in the “same place” so to speak.


> *Feature stores reduce the risk of train-serve skew because feature logic—at training-time and inference-time—is done “in the same place”.*


Training-serving skew monitoring is also made easier and more efficient. Since all needed data is centralized in a single place, monitoring can be handled by the feature store team itself.


## **Pro: Feature stores enable feature discovery and reuse among teams**


Feature stores usually have a *registry* or *catalog* where individual model features are defined, described and versioned.


This could be as simple as a folder inside a common repository, a library or a fully-fledged internal website where users can enter information (see Figure 5 for inspiration).


***Figure 5: Sample feature registry:*** *Sample web UI for an imaginary feature registry. Users can search for existing features and find information about them. Here we see that the reported_income feature is on version 3, is being used by 4 models, and is owned by the Onboarding BU.*


These registries are the perfect place for potential users (i.e. ML practitioners) to *browse* existing features and find out about features that they didn’t know existed. Users can easily choose existing features to add to a model, reducing Time-to-Market (TTM).


Two scenarios where feature registries are useful:


- **Creating a low-cost baseline (v0) model using existing features:** You can just look up a few common features and quickly ship a simple baseline model to test a hypothesis.
- **Augmenting an existing model:** Adding new features to existing models is one of the most common tasks data scientists engage in. Feature registries enable you to quickly see what features are already available and the effort involved in adding them to your model.


## **Pro: Feature stores increase the incentive for feature creation**


Companies often base employee career progression on **objective** criteria, to align incentives and promote an overall fairer workplace dynamic. Examples of metrics used include financial impact generated by a project, number of people using a given tool or platform, NPS or user feedback, etc.


Creating features is an important task, which usually has financial impacts in an organization. So ML practitioners are incentivized to do it, since it leads to greater financial impact which will, in turn, lead to better career progression. This impact is made even larger if other teams are able to use features created by individuals, therefore the **incentive** for the practitioners to create features is also increased. See a summary in Figure 6 below:


***Figure 6: Incentives matter:*** *Creating features in a feature store usually incurs extra work (need to create metadata, get reviews from other teams, etc), but this disproportionately increases the impact of those features, which in turn helps the career progression of practitioners. Win-win.*


Incentives are important: Feature stores increase the impact of features which, in turn, increases the organizational impact of ML practitioners and their incentives to create them.


## **Pro: Feature stores centralize feature management in a single platform, increasing efficiency**


Retrieving real-time model features from a feature store (as opposed to the source services for each feature individually) means that many responsibilities can now be “abstracted away” in the platform.


**Observability** is a good example. A feature store can easily instrument feature retrieval with logs and metrics every time they are requested by an upstream service. It can also generate generic monitoring dashboards so that interested parties have visibility into feature distributions and monitor for problems. See a visual representation below.


***Figure 7: Centralized logs and metrics*** *allow client services to get observability “for free,” without implementing repetitive functionality for every integration.*


Many other responsibilities can also be extracted into a feature store platform so client teams do not have to deal with them:


- **Documentation:** All feature documentation can be centralized in a feature registry (as shown in Figure 5), which is part of the feature store.
- **Maintenance:** Updating feature code to fix bugs, improve efficiency, and manage versioning. All of this becomes much more efficient when handled by a dedicated, specialized team.
- **Debugging/troubleshooting capabilities** : The platform can create centralized tools for log visualization and processing (e.g. distributed tracing visualization, SQL queries on logs, and more).
- **Caching:** It’s very common for feature values to be cached to reduce costs and processing times. Providing such a caching layer at the platform entry-points can be easily done by the feature store team.
- **Governance/Auditing:** Organizations operating in heavily-regulated environments (e.g. banking, healthcare, defense) often need to keep precise audit trails of how models are used. Some examples:


- What were the exact feature values and output score for each individual instance scored? (or at least a sample thereof)
- Which exact model version was being used at a specific moment in time?


## **Cons**


## **Con: Feature stores can become bottlenecks for teams that need to iterate quickly**


From the point of view of client teams, retrieving a feature from a feature store platform may be more difficult than doing it “the usual way” (i.e. directly querying its primary source).


As a result, teams with very short experimentation cycles may see the platform as a source of friction. This usually happens in scenarios such as:


#### Fast Proofs-of-Concept (PoCs)


A team wants to create a simple feature to test a new data source, but having to add it to the FS first adds too much overhead and defeats the purpose of a simple PoC.


#### Capabilities not yet available in the feature store


A team needs a custom functionality (such as custom pre-processing or post-processing) that does not yet exist in the platform. However, the FS team has its own roadmap and objectives and it can’t prioritize enabling this functionality now.


Of course, the FS team can and *should* make the platform as simple as possible to use. Ease of use reduces the overhead, and therefore reduces these problems. We learned several lessons on this topic, as we’ll cover in part 2 of this series.


## **Con: Naïve architectures can introduce additional latency**


The most “naïve” implementation of a real-time feature store is as an “intermediate layer” between models and the upstream services that own the features.


However, this new layer *will* add some latency overhead when compared to accessing those sources directly (simply because extra network or database calls are being added to the flow).


It may be an issue in scenarios where milliseconds make a difference (e.g. High-frequency Trading), but these are few and far between. Still, there are several strategies to reduce this impact:


- **Prefetching:** Refers to fetching and caching the *next* features (before they are even requested). In other words, if we know feature C is always requested after features A and B, we can compute C ahead of time so it’s already available when it *is* requested.
- **Local caching:** Data used to build critical features can be stored locally in the FS platform for fast retrieval (instead of being fetched over the network). For example, if transfer-related features are very important for you, all transfers could be stored in a local key-value store like Redis, such that retrieval becomes instantaneous when/if features are requested.
- **Parallel fetching:** Independent features can be retrieved in parallel through multithreading.


It’s worth noting, however, that not all feature store implementations follow this naïve approach. In fact, many architecture choices can in fact *reduce* feature fetching time instead of increasing it.


## **Con: Feature stores may make observability and debugging harder for client teams**


Introducing a feature store to retrieve real-time features usually means that **observability** (viewing logs and metrics) and **debugging** (tracing the root cause of a problem) will change. That happens because retrieval logic is now abstracted away, and logs may no longer look like what client teams are used to.


This often creates initial resistance to adoption. It’s important to make sure client teams do not lose the ability to observe and debug feature retrieval on their own, because nobody likes losing autonomy when solving production issues.


In practice, this resistance can be significantly reduced when the feature store follows the same conventions used by the organization’s other services. Important practices include:


#### Following existing logging conventions


Logs and metrics emitted by the feature store platform must be as similar as possible to those used by other (non-ML) services: using the same technologies, same patterns, same structure, following the same distributed tracing patterns where applicable.


#### Making sure basic user needs are met


The “bare minimum” use cases should remain simple and easy. All client teams should be able to:


- Inspect raw input features for individual instances scored by a model.
- View latency metrics (averages and percentiles) for individual features over time.
- See exception messages and stack traces whenever errors occur during feature retrieval.


#### Including all necessary dimensions in logs and metrics


Logs and metrics must include the caller model name and any other dimensions required to distinguish different call flows (e.g. shadow-mode vs non-shadow-mode calls).


This “con” is only an issue during the initial adoption of a feature store. In the long run, a feature store may provide even better observability and debugging capabilities than standard services.


## **Conclusion**


Feature stores carry a real implementation and adoption cost, especially for organizations that are still beginning to structure their real-time ML systems. Still, as the number of models, teams, and use cases grows, the benefits of standardization, reuse, governance, and operational efficiency tend to outweigh that initial investment.


By centralizing feature logic in a single platform, organizations can reduce issues such as training-serving skew, simplify ML pipeline maintenance, accelerate experimentation, and create more scalable real-time inference systems. Moreover, second-order effects such as cross-team feature discovery, centralized observability, and component reuse compound as adoption grows.


**In part 2 of this series** , we’ll explore the main lessons learned from using feature stores in production at Nubank. **Stay tuned!**
