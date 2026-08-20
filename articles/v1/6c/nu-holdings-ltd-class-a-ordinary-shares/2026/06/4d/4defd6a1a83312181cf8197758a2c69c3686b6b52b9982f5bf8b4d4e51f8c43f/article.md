---
schema_version: "1.0.0"
document_id: "4defd6a1a83312181cf8197758a2c69c3686b6b52b9982f5bf8b4d4e51f8c43f"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-b653f977f25a"
canonical_url: "https://building.nubank.com/feature-stores-for-real-time-ml-part-2-lessons-learned-from-production/"
published_at: "2026-06-18T12:09:16+00:00"
first_seen_at: "2026-07-20T04:36:16.841262+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:18944dd0e0fd9e2c495249a0b6868683ff467dd7530b11015d5809ef8a3f4308"
---

# Feature stores for real-time ML: Part 2 – Lessons learned from production

Written by:[Felipe Almeida](https://www.linkedin.com/in/felipeqbalmeida/?locale=pt) , with contributions from[Luiz Felix](https://www.linkedin.com/in/lzfelix/)


*This is* ***part 2*** *of a 3-part series on Feature Stores for Real-time ML. In* ***part 1*** *we talk about Introduction and also about Pros & Cons. In* ***part 3*** *we conclude with end-to-end real-world architecture setups from Nubank.*


## **A quick recap**


In[part 1](https://building.nubank.com/feature-stores-for-real-time-ml-why-and-when-to-centralize-feature-logic/) , we explored what feature stores are and the role they play in real-time machine learning systems. We discussed how they help mitigate crucial problems such as training-serving skew and why, despite the adoption costs, they are a valuable investment for organizations operating ML at scale.


Over the past few years, we’ve used feature stores across a wide range of real-time ML applications at Nubank, including credit underwriting, fraud detection and customer service. Along the way, we’ve seen recurring patterns emerge, encountered unexpected challenges and gathered practical lessons that helped us avoid common pitfalls while maximizing the value delivered by the platform.


**In this article** , we’ll cover some we consider especially important.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## **Lesson: Build vs buy is a spectrum**


It’s common to refer to the “build-vs-buy” trade-off when discussing complex platforms such as feature stores. Some organizations may want to build everything inhouse, while others may want to shift some of the complexity away to more “complete” feature stores such as[Chalk](https://chalk.ai/) or[Feast](https://feast.dev/) .


In our opinion, you don’t have to *choose* between building *everything* or buying *everything* . You can instead have a *hybrid* feature store: you build some parts, use open-source tools in other parts and delegate to third-party service providers where needed.[This is similar to the approach we currently follow at Nubank](https://building.nubank.com/mastering-streaming-data/) .


> *Build vs buy is not a binary choice: you may choose to “buy” some components and build others yourself.*


Figure 1 below shows a high-level view of a real-time feature store and some of the components that may exist in each layer. Each of these components may be “built” or “bought”.


**Figure 1:** A real-time feature store has many components and many of them could theoretically be built inhouse, or bought as an “off-the-shelf” product. Interestingly, many open-source projects have actually given rise to companies that provide managed versions thereof (e.g. Kafka/Confluent, Spark/Databricks, Cassandra/Datastax).


At Nubank, we use open-source tools such as Kubernetes, Kafka, Pinot, Spark and Flink for core parts of the architecture, where our use-cases don’t depart *too much* from what the tools cater to. We also use fully-managed services from our cloud providers, while other components (such as our monitoring infrastructure) are built inhouse.


The key to deciding which parts are “built” vs “bought” is about the level of control you have, and where your organization’s workflow patterns are too different from the default use-cases the tools cater to: in these cases you may have no option but to build.


## **Lesson: Feature stores are** ***products*** **and should be managed as such**


In addition to being *systems* , feature stores are also *products* . And products are living things that need to be *managed* , ideally by a dedicated Product Manager (PM). Everything related to internal product management applies here.


Many of the practices commonly associated with internal product management apply directly to feature stores, such as:


- **Roadmap and task prioritization:** Which tasks and enhancements should be built first? Which requests fall outside the platform’s scope and why?
- **Support channels:** How will users get help when they encounter issues or have questions? Will there be a rotation of engineers to provide support? How will the team track the time it takes to answer questions?
- **User interviews/surveys:** How will the FS team gauge how satisfied users are with the product?
- **Stakeholder management:** How will key stakeholders stay informed about product enhancements? Who should be involved in defining the roadmap so that the company’s priorities are addressed?


Note that feature stores differ from other internal products in that they naturally span multiple disciplines. Troubleshooting a training-serving skew issue, for example, often requires collaboration between software engineers, data engineers, data scientists and domain specialists. As a result, operating a feature store successfully involves coordinating expertise across several areas of the organization.


## **Lesson: Define a naming convention for features**


Set a naming convention for features in the feature store. Naming conventions have very little downside (especially when they are lightweight enough to accommodate many use-cases).


> *Use a naming convention for features: it helps humans and AI tools navigate the codebase.*


Naming conventions have several advantages (as does *standardization* more generally):


- **Reduced chance of human error:** Standardized names make tasks such as code reviews, copy-pasting and bulk edits safer and easier.
- **Lower cognitive load:** Users can infer what a feature represents without needing documentation.
- **Enhanced feature discoverability:** Users are naturally exposed to other items in the same group when exploring features.
- **Simpler monitoring and debugging:** Most feature problems (e.g. drift, skew) affect entire groups of features, making issues easier to identify when those relationships are visible.
- **Easier refactoring and organization:** Feature group names serve as natural namespaces that can be reused across folders, dashboards and other monitoring assets.


It doesn’t really matter *which* naming convention you choose, as long as it’s versatile and allows for some customization. If the rules become too restrictive, users will have to *bend* and *hack* the naming convention until it no longer serves a purpose.


Examples include:


- customer_cc_transactions_24h_count
- customer_app_behavior_2h_logins
- savings_account_5d_balance_daily_avg
- savings_account__current_balance (the TIME_FRAME block is omitted, as it’s the savings-account balance at a given point in time).


Some additional recommendations:


- Avoid using m to represent months, or it can be confused with *minutes* . If a feature covers the last 2 months, prefer 60d.
- Use future-proof names that do not depend on team structure, but on business concepts. Team names reflect organizational structure and change very often, while business concepts tend to remain stable over time.


## **Lesson: Versioning features is hard**


Suppose a team needs a small variation of an existing real-time feature in the Feature Store. The FS team can’t simply change the feature implementation, as that would cause training-serving skew for models trained on the current feature version. (They may not want to re-train their models with the newer version).


So one either needs to create a whole new feature or a new *version* of an existing feature. Most feature stores support feature *versioning* but, in practice, this means that clients must provide the *version* of the feature they want, in addition to its name.


It’s not so easy, however, to know what changes are better described as a new version of an existing feature or as a whole new feature on its own. Judgement must be applied to decide what action to take in each case.


For example: you have a feature called customer_cc_purchases__5d__count. Intuitively, it’s the number of CC purchases made by a given customer in the last 5 days. Now let’s see some scenarios of what can happen in a real-world situation, in Figure 2:


***Figure 2:*** *Examples of situations where teams may need to create a new version of an existing feature, create a different one altogether (depending on the semantics of the change) or, in some cases, do nothing at all.*


The challenge is that versioning can quickly become difficult to manage. It’s easy to end up with dozens of versions of a feature, which means a complex and messy set of features (different teams have different retraining schedules and there are always models that are still bound to earlier feature versions and cannot be retrained right now).


One possible way out of this “versioning hell” is for the feature store team to *take it upon themselves* to migrate models using old feature versions into using new ones. The situation is similar to the work of updating applications to drop older, unsupported libraries and dependencies in favor of newer versions.


**Data contracts** can also be used to mitigate versioning problems: feature producers set the expectations and consumers are made aware when schema and semantic changes happen.


## **Lesson: Use a streaming architecture when you can and direct calls when you must**


As discussed in Part 1, there are two primary ways to retrieve features for real-time inference. The first relies on direct calls to the systems that own the source data, while the second relies on an intermediary streaming platform that continuously receives and processes events.


These approaches can be summarized as:


### **“Direct-call” features**


Features are retrieved via HTTP calls to the microservices that own the transactional “source-of-truth” databases. Depending on the use case, this may include multiple calls and joins.


### **Streaming-based features**


Features are retrieved from an intermediary store instead: usually a short-term database that listens to events from an event bus such as Kafka.


As a rule-of-thumb, **we recommend favoring streaming-based features whenever possible.** There are several reasons for this: ****


- **Better scaling** : Streaming features scale better, because data ingestion is decoupled from data consumption. An increase in the rate of event consumption does not require the producers to also scale up.
- **Better fault isolation and smaller blast radius in case of crashes:** Asynchronous architectures provide natural “environment isolation” that reduces cascading failures. If either the producer or the consumer crashes, it doesn’t cause a failure on the other end.
- **Reduced load on transactional services** : In a streaming architecture, the feature store doesn’t hit the “source” services to fetch information. These services usually have other responsibilities than providing data for ML workloads, so one doesn’t want to overload them causing them to crash, potentially causing business disruption in unrelated flows.


It’s true that using streaming features requires a streaming “platform”, which is the lower-level infrastructure allowing source services to stream events to an asynchronous event bus (usually Apache Kafka). This is a complex and expensive platform in and of itself, and it’s a prerequisite for streaming features.


## **Lesson: Direct-call and streaming-based feature retrieval have different failure modes**


Feature retrieval will fail from time to time, either when using “direct call” or streaming-based strategies. Some failure modes are “explicit” (i.e. exceptions and flow interruptions), while others are “silent”. This happens because they just change the feature distribution, but don’t trigger explicit exceptions. (Silent failures are very dangerous because you risk making bad decisions for days or weeks before anyone detects there’s a problem.)


Each strategy fails in different ways, and the asynchronous nature of the streaming strategy introduces some different failure modes one may not be used to. You must understand how each fails and what the implications are for your business.


- **“Direct-call” features** **fail explicitly if the owner microservice is unavailable for any reason:** This includes timeouts, service crashes and even service overloading caused by the feature fetching itself *.*
- **“Direct-call” features fail silently if some logic is changed in the source service:** Very often, engineers that own services do not know that some endpoints are used to “feed” real-time models. Sometimes, they make changes to support some business case and, inadvertently, change the distribution of data being requested, which causes a training-serving skew problem.
- **Streaming features fail silently if delays become too great:** While some level of delay is to be expected in streaming features, sometimes this delay reaches unacceptable levels due to a number of structural and temporary factors. This means that the real-time model will consume stale information, leading to training-serving skew and a performance decay.
- **Streaming features fail silently if events arrive out of order:** Depending on the event bus used to process ingested features, some events may arrive out of order, which will cause training-serving skew and performance decay in models.
- **Streaming features fail silently if there’s no strict parity between batch and streaming ingestion:** Very often, engineers make changes to the batch ingestion logic but forget to replicate the changes to the streaming ingestion logic. In these cases, the streaming data will “fall out of sync” with the batch data, which will cause training-serving skew. There should be integration tests to guarantee that whatever is ingested in batch is also ingested in the streaming layer.


## **Lesson: Training-serving skew monitoring is still necessary**


Training-serving skew refers to undesired differences between how features are generated during *training* -time and how they are retrieved at inference- or *serving* -time.


In part 1, we argued that feature stores help mitigate this problem, because both data “paths” (batch and real-time) are defined in a single place. This reduces the chance of such deviations going unnoticed.


Feature stores do not, however, **eliminate** the problem. One still needs to **monitor** training-serving skew, as there are still many ways for batch and real-time data to fall out of sync even when both paths are defined within the feature store platform. Two examples: (1) Upstream services may change a field’s semantics in the real-time path without updating the batch equivalent. (2) There may be some systematic delays in the event bus, causing the real-time streaming features to *always* be late when compared with the instant they should have been available.


But what does monitoring training-serving skew actually look like in practice?


One can detect training-serving skew by programmatically generating the training-time feature extraction logic (minus the target) for those examples that were scored in real-time in the recent past,and then comparing those with the actual values used at inference-time (obtained from production logs). A visual representation can be seen in Figure 3 below:


***Figure 3:*** *One possible strategy to monitor training-serving skew is to extract features using batch-time logic for instances scored in real-time and then compare the values obtained in each “path” and see whether they match. This can be run in an ad-hoc manner or as a recurring daily job.*


One important caveat applies here: This monitoring strategy assumes that teams are able to *programmatically* generate training datasets for models, to be used as the comparison benchmark versus real-time logs. Not only does this unlock training-serving skew monitoring, but it’s **crucial** to guarantee reproducibility.


## **Lesson: Pilot projects are one of the most effective ways to drive adoption**


When introducing a feature store to an organization, not all teams may immediately *want* to use it, as they *might* lose some autonomy and control over features, lose relative power in the organization, have to deprioritize other projects, etc. The usual factors that contribute to resisting platformization efforts.


> *The best argument for adopting a feature store is a clear view of the benefits. But if there’s still resistance from client teams, pilot projects owned by the FS team are a good way forward.*


Feature store adoption should be encouraged via carrots, not sticks (e.g. top-down *mandates* ). At the end of the day, the benefits of using a feature store (increased impact, more robustness, faster TTM, etc.) must be clear to encourage adoption. Figure 4 summarizes several common reasons why teams hesitate to adopt feature stores, along with potential mitigation strategies.


***Figure 4:*** *There are many reasons why teams resist adopting feature stores. Most of them can be mitigated if the Feature Store team shares some of the integration burden (i.e. pilot projects) and if there’s clear documentation and good support channels.*


In Figure 4, we see that a great overall mitigation strategy is for the FS team to create the first few integrations for client teams, as **pilot projects** .


These projects greatly reduce the cost of adoption (from the PoV of client teams) and it’s a “win-win” strategy: the client team gets features built “for free” and the FS team has a pilot project to stress-test the platform, and to serve as an example for other teams to learn from.


# **Conclusion**


As we claimed in part 1, feature stores require significant investment in platform engineering, operational maturity and organizational alignment. But for companies running real-time ML systems at scale, that investment unlocks substantial long-term benefits.


Our experience at Nubank has shown that successful feature store adoption is not only about technology choices. It also depends on product thinking, strong operational practices and a deep understanding of how real-world ML systems evolve over time.


Along the way, a few lessons consistently stand out:


- It’s possible to mix and match several types of components (open-source, vendors, homebuilt) to adapt your setup to your specific needs.
- Feature stores are products and must be managed as such.
- Defining and *enforcing* a naming convention reduces cognitive load and makes versioning easier.
- Streaming-based features scale better. Feature stores should also support direct-call for edge cases.
- Synchronous and asynchronous workflows are very different and fail in different ways. Understand how each fails and how they affect your business.
- Using feature stores mitigates, but never fully eliminates training-serving skew. Monitoring is still needed.
- Pilot projects are a great all-around tactic to help *prove* the benefit of platform-like initiatives. Feature stores are no different.


This is Part 2 of our series on Feature Stores for Real-Time ML at Nubank. In Part 3, we’ll bring these concepts together and explore real-world architectures we use in practice.
