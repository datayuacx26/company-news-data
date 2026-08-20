---
schema_version: "1.0.0"
document_id: "64a36c8c9df474de11499a54fce673f576537aa0519bf921f5bd79d6941b458e"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/anyshift-partnership"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:ca36c7b2f7749142547c0d8efb69970e8e3ac763b6bd479e13b310c8cd614cf4"
---

# Q&A: How Elastic and Anyshift are bringing AI-powered context to incident response

# Q&A: How Elastic and Anyshift are bringing AI-powered context to incident response


By


[Sunnie Weber](https://www.elastic.co/blog/author/sunnie-weber)


July 6, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


- Anyshift’s AI agent, Annie, can now read from Elasticsearch as a log source during incident investigations.


- SRE teams can ask Annie incident questions about ongoing incidents and receive responses grounded in log data stored in Elastic.


- The integration provides read-only access to Elasticsearch data, supports API key authentication, and can connect to multiple Elasticsearch instances across environments. Annie can also surface anomalous log spikes and other signals that may warrant investigation.


- The integration reflects a broader shift toward AI systems that leverage customer-owned operational data and context to improve incident response and operational decision-making.


Incident response often depends on connecting two kinds of context: what changed in the environment and what the logs say happened next. Through a new integration with Elastic, Anyshift’s AI agent, Annie, can read from a customer’s Elasticsearch deployment to search logs, surface error and warning spikes, and correlate log evidence with infrastructure change history.


We spoke with Louis Fradin, developer advocate and backend engineer at Anyshift, about how the integration works, why Elastic was a natural fit, and what it means for SRE and operations teams using AI to accelerate incident response. His work with cofounders, Roxane Fischer and


Stéphane Jourdan, allows them to build the joint story with Elastic while supporting developers through technical writing and DevRel work.


**Q: For readers who may be unfamiliar with Anyshift, what does the company do?**


Anyshift builds a versioned resource graph of live infrastructure — cloud accounts, Kubernetes, and infrastructure as code, plus the source control and workflow tools teams already run — and keeps the change history of every state transition.


Our AI agent, Annie, traverses that graph to answer the kinds of questions a graph is best at, like:


*What depends on this? What changed? What is the blast radius? How do you trace incident symptoms back to the change that introduced them?*


**Q: Why did Anyshift integrate with Elasticsearch for incident response?**


The driver was data access. Elasticsearch is the logging and observability back end in a lot of the customer stacks we work with. Any time an on-call SRE asked Annie about an incident, the most useful piece of evidence — the actual log lines — often lived inside Elasticsearch with no path for Annie to reach it.


Until the integration was in place, Annie’s SRE monitoring surface had a gap every time a customer’s logs lived in Elasticsearch. Closing that gap by giving Annie read access to customer Elasticsearch deployments through the Search API is what led to the integration.


**Q: Was this a competitive vendor selection?**


No, this wasn’t a traditional vendor bake-off. Anyshift is customer-pulled across logging and observability sources. We integrate with whatever stack a given customer runs.


Elastic moved up the priority list because the overlap between our customer base and active Elasticsearch deployments was especially strong, and Elastic’s REST Search API offered a clean path to read-only log access without a middleware layer in the way. The question wasn’t “which logging vendor scores best?” It was “where do customers most need us to land first?”


**Q: What made the partnership a strong fit beyond the technical integration?**


There’s a shared ethos. Customer observability data belongs to the customer and should stay open and queryable, not locked inside the platform that captures it.


Elastic exposes a clean REST Search API over customer logs, which made it possible for Anyshift to plug a customer’s Elasticsearch into Annie with nothing more than an API key. On the AI side, the thesis is similar: The value is in customer-owned, structured context, not in the collection format.


**Q: How does Anyshift use Elastic today?**


The shipped MVP gives Annie read access to a customer’s Elasticsearch deployment as a log source.


There are two access paths:


1.


A set of read-only tools that Annie calls on demand while answering an SRE’s incident question like searching documents, retrieving index information, checking cluster health, and reviewing cluster stats


2.


A proactive hourly worker that polls the customer’s index pattern for error, warning, and request statistics and surfaces spikes inside Annie’s SRE monitoring view


Authentication is API key-based, and the direction is one-way: Annie reads from Elasticsearch but never writes back.


**Q: How does the Elastic and Anyshift integration help SRE teams during incidents?**


For an Anyshift customer running Elastic, Annie can now reach into Elasticsearch and answer incident questions that depend on log evidence like error spikes, recent anomalies and message-pattern lookups without the on-call SRE leaving the Annie surface.


The hourly worker also means spikes can be surfaced proactively, not just on demand. For Elastic customers adopting Anyshift, their existing Elasticsearch deployment becomes another first-class data source for Annie. During an investigation, Annie can consult Elastic logs alongside Anyshift’s infrastructure graph and other integrations.


**Q: Were there any meaningful technical hurdles along the way?**


Yes, three areas required real iteration.


-


**Multi-instance Elasticsearch support:**


Real customers often run more than one Elasticsearch deployment, whether that’s production in different regions or separate production and staging environments. The initial single-instance MVP had to be reworked to support multiple deployments per project.


-


**Customer-side network access:**


Customer Elasticsearch clusters are often behind IP allowlists or VPCs, so onboarding includes a coordinated step where the customer allows Anyshift’s outbound IPs.


-


**Varied customer log schemas:**


Elasticsearch deployments differ in how fields are typed and structured, which affects the hourly worker’s aggregation logic. We addressed that with better service-field detection, validation before aggregation, and graceful fallback behavior.


**Q: Did the integration have any impact on how Anyshift builds products internally?**


It did. The multi-instance work for the Elasticsearch integration became the architectural pattern we now use more broadly across our integration layer. It helped shift us from a single-instance default to a multi-instance-by-default posture for the rest of the roadmap.


**Q: What can customers do now that they couldn’t do before?**


A few things stand out.


-


Annie can search a customer’s Elasticsearch logs on demand during an incident, so log-evidence answers are available without the SRE leaving the Annie surface.


-


Error and warning spikes in Elasticsearch can be surfaced proactively in Annie’s SRE monitoring view by the hourly worker.


-


Elasticsearch becomes one of the data sources Annie correlates against when answering an SRE question, alongside the rest of the infrastructure and workflow context Anyshift already brings together.


**Q: What does this partnership say about the future of AI in operations?**


The core thesis is that AI in production is bottlenecked on context, not on models.


The faster the versioned resource graph becomes the infrastructure context layer for AI-native observability, the more durable our position is. Elastic is a natural partner in that story because observability data is such a critical part of the context AI needs to be useful in production.


**Q: Are there any early indicators of impact?**


Anyshift customers running Elastic in production are using the integration today. Public attribution from those customers isn’t in place yet, so named joint case studies are still ahead.


From the broader Anyshift side, we’ve seen Annie consistently reduce root-cause time significantly on incidents recoverable from graph state. In one example, Hilary Brennan-Marquez, head of infrastructure at MotherDuck, described a process that would have taken a human anywhere from 20 minutes to two hours that Annie completed in 30 seconds. That same MTTR pattern is what we expect to see reflected in future joint customer stories.


**Q: What’s next for the Elastic and Anyshift collaboration?**


There are three near-term tracks.


One is the joint blog story around how to use Anyshift to explore infrastructure where Elastic products are running. Another is the first-named joint case study once a shared customer is ready to be referenced publicly. And the third is extending the integration into Elastic knowledge base and Streams, so customer infrastructure context and AI-partitioned significant events can flow into the same Annie surfaces alongside the log data we already consume.


**Q: What part of the roadmap are you personally most excited about?**


Honestly, Streams. The idea of an AI-parsed, AI-partitioned narrative of significant events from unstructured logs — alongside infrastructure change events on the same timeline — is one of the most compelling directions I’ve seen for real-time operational understanding. That changes what on-call can look like.


**Q: What advice would you give to other organizations interested in partnering with Elastic?**


Lead with the technical conversation, not the legal one. Once the engineering and product alignment are sharp, the commercial structure tends to follow.


Also, pick a first integration surface that creates a demo moment a non-technical buyer can immediately understand. For us, that was simple; the on-call SRE asks Annie about an error, and Annie pulls the matching log lines from the customer’s Elasticsearch deployment in the same conversation.


**Q: If you had to describe the Elastic and Anyshift partnership in three words, what would they be?**


Open. Complementary. AI-native.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, and associated marks are trademarks, logos or registered trademarks of elasticsearch B.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
