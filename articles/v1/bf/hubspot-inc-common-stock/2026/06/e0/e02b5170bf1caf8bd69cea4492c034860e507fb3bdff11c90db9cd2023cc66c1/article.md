---
schema_version: "1.0.0"
document_id: "e02b5170bf1caf8bd69cea4492c034860e507fb3bdff11c90db9cd2023cc66c1"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-rss-7ea402701b4d"
canonical_url: "https://product.hubspot.com/blog/building-the-ai-retrieval-infrastructure-behind-20-billion-vectors-at-hubspot"
published_at: "2026-06-25T18:17:44+00:00"
first_seen_at: "2026-07-20T03:32:21.515438+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:19913127a9f51a5db38a210fe6270c983a49266f8b8e34c5cc296e00b7f45431"
---

# Building the AI Retrieval Infrastructure Behind 20 Billion+ Vectors at HubSpot

Semantic search plays a critical role in HubSpot operations and powers a wide variety of use cases, from agents and RAG (Retrieval Augmented Generation) to contact deduplication. Starting from the POC a few years back, it has grown to a scale of tens of billions of vectors, utilized by 38+ teams. With the growth of agent usage, semantic search capabilities have become even more important to ensure agents can quickly retrieve the information they need while maintaining retrieval quality. This article describes the journey of vector DB infrastructure, from a small POC to the tens of billions of entries powering hundreds of use cases across HubSpot.


### What is VaaS?


VaaS (Vector as a Service) is HubSpot's centralized vector storage and search platform built on top of a vector database. It provides an API layer and features such as:


1. Access control


2. Embeddings generation based on the collection setup


3. Data versioning


4. Feedback collection


It sits in front of the vectors database, in our case


[Qdrant](https://qdrant.tech/) , and is directly exposed to the consumers. It handles access control, basic validation, embeddings generation and then proxies requests to Qdrant. For read operations, requests are handled synchronously, whereas for write operations the flow is asynchronous with minimal consumer lag.


VaaS platform high-level architecture


### Why Qdrant?


HubSpot uses the


[Qdrant](https://qdrant.tech/) vector database for all semantic search operations. Qdrant is an open-source vector database that can be deployed on-premises and provides an extensive API covering most modern semantic search features, such as named vectors, hybrid search, multi-stage querying, and weighted reranking. It uses


[HNSW](https://en.wikipedia.org/wiki/Hierarchical_navigable_small_world) (Hierarchical Navigable Small World) as an


[ANN](https://en.wikipedia.org/wiki/Nearest_neighbor_search#Approximate_nearest_neighbor) (Approximate Nearest Neighbor) algorithm, which provides very low latency at the trade-off of higher memory consumption.


Qdrant also supports an extensive set of cost-efficiency optimizations, such as quantization, on-disk storage options for payloads and vectors, and configurable precision. This allows us to tune the setup to find a balance between providing the required level of retrieval quality and associated costs.


### Why we need to run Qdrant in-house


HubSpot has a lot of expertise in running self-managed open-source database systems in-house, and over the years our infrastructure team has built numerous solutions, rules, and best practices that we want to take advantage of. Having such an extensive infrastructure layer lets us seamlessly integrate with and benefit from all of HubSpot’s internal tooling, including tracing, cost tracking, rate limiting, and scaling. Running Qdrant in-house enables us to easily fine-tune the infrastructure to our needs and control costs by utilizing our corporate AWS rates. Finally, running in-house gives us full control over customer data and infrastructure, and ensures all of HubSpot’s extensive security best practices are applied to this data store.


### Current scale


The VaaS (Vector-as-a-Service) platform is currently used by


**38+ teams** and has more than


**200 indexes** that run on a fleet of


**140+ clusters** across


**5 regions** and


**2 environments.** We isolate clusters by product teams to minimize the blast radius from failures affecting the entire platform. Collectively, the underlying vector database stores


**20+ billion** vectors across different regions, with the largest index holding 9.5 billion vectors and an average index size of 95 million vectors.


The platform handles 5,000+ RPS for writes, with spikes up to 100,000 RPS, and serves 1,000+ RPS for reads across all regions. The traffic can also be spiky, specifically in cases when large backfills run in parallel with the live traffic; therefore, the underlying infrastructure should be scalable and reliable to provide the best customer experience while supporting engineering use cases.


Total reads across all regions


Total writes across all regions


---


## The Early Proof of Concept


Back in 2023, semantic search had just started to take off, and the internal POC was set up to run Qdrant alongside the required pipelines and an API layer on top of it. There was a limited number of consumers and a dozen Qdrant clusters that were relatively easy to operate. As a first solution, Helm was utilized to define per-cluster/environment setup and parameters such as number of replicas, disk space, RAM, CPU, etc., were adjusted manually. The cluster definition and related cluster artifacts (Kafka topics, workers, etc.) were defined in code and created manually. For a small fleet of clusters, it was a great solution that allowed us to iterate fast, utilize overprovisioning to handle traffic spikes, and keep the setup simple. All the clusters were defined as StatefulSets and shared the same namespace. At that time, Qdrant was already handling 2+ billion vectors across ~10 indexes.


Helm deployment shared the same k8s namespace across all clusters


---


## Scaling Up the Infrastructure


With more and more features relying on semantic search, and VaaS being at the core of multiple AI capabilities, powering functionalities from RAG (Retrieval Augmented Generation) to agentic experiences, it became clear that it was time to transition from the POC to full production scale to address the growing demand.


Alongside the rapid growth in demand for semantic search, our setup started to evolve in terms of complexity, with additional artifacts required for each cluster and a growing need for quick, consistent cluster setup. Helm is a powerful tool, but it’s purely a templating and deployment tool that cannot:


- Make API calls


- Auto-scale based on external metrics


- Run custom logic for complex state-aware lifecycle management


As a result, the decision was made to migrate from Helm to a Kubernetes Operator pattern to ensure reliability and scalability while reducing operational load on the team by automating standard operating procedures.


### Onboarding to Kube-operators framework


HubSpot has an internal framework called \`Kube-operators\` that implements the


[pattern](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/) . The main idea is to automate the lifecycle management of systems running on K8s by using operators to track and maintain the system’s defined state. It provides a robust toolkit for building operators designed to meet the needs of HubSpot and our infrastructure teams. It also enables seamless integration with multiple HubSpot infrastructure tools, which makes it possible to use them in the Kubernetes Operator’s logic.


The operator continuously drives actual state toward the CR's desired state


Running Qdrant on Kube-operators allows us to instantiate and decommission clusters together with all related artifacts and subsystems. It also automates multiple workflows to reduce operational load on the team.


Through translators, a Cluster CR propagates into namespaces, Qdrant StatefulSets, indexers, Kafka topics, and shard-management operations


The main components in the setup are Translator artifacts. Translators watch assigned CR, running in a loop, re-evaluating the state every 60s, applying the required updates, and ensuring that underlying resources are in sync with the defined state. This is an extensible architecture that allows adding/removing translators and therefore onboarding more features to the system. In addition, the translator contains all the logic required for rendering underlying artifacts; therefore, it can be extended with required automation.


Our current setup has 3 translators:


- **Cluster translator:** responsible for instantiating the individual namespaces for clusters and downstream artifacts.


- **Qdrant nodeset translator:** responsible for instantiating Qdrant clusters. It instantiates all required K8s artifacts with the specified number of pods, memory, disk storage, parameters, etc.


- **Indexer nodeset translator:** indexers are the workers that handle incoming write traffic and index it into Qdrant. There are multiple indexers per cluster handling different traffic priorities, etc. This translator is primarily responsible for instantiating the required indexers and creating necessary Kafka topics.


Migrating from the Helm setup, we also updated the K8s namespace strategy and moved towards a namespace-per-cluster approach instead of using a single namespace. This provides the following benefits:


- **Isolation:** provides higher reliability, since issues such as misconfigurations have a reduced blast radius.
- **Namespace-specific configuration** : we can set up namespace-specific resources, limits, etc.


- **Simplified cost tracking:** tracking costs per namespace is much easier, which is important for current scale.


Migrating from the Helm setup we moved towards a namespace-per-cluster approach


After the migration, the cluster configuration became simple and requires creating or modifying a single config that defines both the Qdrant cluster and the other infrastructure artifacts as Custom Resources (CRs):


Example cluster config


### Dynamic cluster definition


Together with onboarding to Kube-operators, we redefined the contract between VaaS (API layer + data pipelines) and the underlying infrastructure to ensure Qdrant clusters are self-discoverable and do not require any additional code changes when a new cluster is created. The important part is that cluster metadata is required for each search/ingest/delete request; therefore, cluster lookup should have minimal effect on end-to-end latency. As a solution, we decided to abstract cluster metadata into a K8s CRD (Custom Resource Definition) to ensure the latency overhead is minimal. Moreover, we had already successfully used the CRD approach for other metadata in VaaS.


The translator writes cluster-metadata CRs that VaaS watches to discover new clusters and route traffic


The end-to-end flow is shown in the diagram above:


- Cluster creation in Kube-operators waits until all pods are started, then triggers cluster metadata CR creation.


- VaaS automatically discovers cluster metadata CRs and starts routing traffic to that cluster for the corresponding collections


---


## Automated Cluster Maintenance (Operations & Scaling)


Before Kube-operators, scaling down a cluster took four steps


Before Kube-operators, cluster maintenance was very difficult. Open-source Qdrant does not support a lot of automated maintenance operations out of the box that other distributed data stores do. Qdrant provides APIs that serve as building blocks to perform actions like shard transfers, but leave it up to the user to implement the full workflow. In practice, this means that horizontal scaling can be very difficult or error-prone. To scale down a cluster horizontally prior to Kube-operators, we needed to:


1. Run an on-demand job to transfer shards off evicted peers and remove peers from consensus.


2. Update cluster Helm chart to lower pod count.


3. Apply the Helm chart.


4.


Delete the PVCs for the removed pods


With Kube-operators, we baked all of the above into our Translator. All we have to do is update the number of replicas in the cluster config; the watching Translator daemon will reconcile, detect that the replica count has been lowered, and automatically run the eviction process - consolidating 4 operations into 1. With our setup, the process is fully automated and requires a single config change on the cluster CR to kick off.


### Safe eviction of shards from Peers


Shards draining off evicted pods before they are removed from consensus


To scale down a cluster, we needed to transfer every shard off the pods being removed before we could safely remove them from consensus. It sounds simple, but there are many ways this can go wrong. If you remove a pod from consensus before its shards are fully transferred, you risk data loss. If the remaining pod count drops below the replication factor, you violate your durability guarantees.


So we baked a strict sequence into the Translator:


1. Evacuate all shards off the evicted pods.


2. Wait until every transfer is confirmed complete on the destination.


3. Remove the pods from consensus.


4. Delete PVCs.


The eviction process rejects the eviction upfront if the remaining pod count can't satisfy the replication factor, and validates that every shard on the evicted pods has a valid destination before issuing a single transfer. If a shard ends up in a Dead state during transfer, we abort immediately.


All of this runs automatically. Scaling down a cluster is a single field change on the CR.


### Automated shard distribution balancing


Some of the clusters in our fleet host collections with billions of points. For performance reasons, we have to utilize custom user-defined sharding. With custom sharding, requests only need to hit a subset of shards defined by a shard key, whereas with auto sharding, requests need to fan out to every single shard. However, using custom sharding may result in shards that are unevenly distributed across the cluster. Qdrant is incredibly fast and efficient because it pre-loads all vectors into memory by default. Consequently, uneven shard distribution leads to uneven memory usage, which is bad because this leads to premature scaling up to accommodate the pod with the heaviest skew. We designed an algorithm to greedily balance shards - by shard count and by point count - and baked detection of uneven distribution into our Kube-operators translators. Whenever the Translator runs its reconciliation loop every 60s, it’ll automatically balance shards using our algorithm.


Here are the results after our algorithm runs on an imbalanced cluster storing a BM42 sparse vectors collection with over 3 billion points:


Across the board, once shard count is evenly distributed per pod, we see cluster resources (memory, disk) flatten and skew is massively reduced. Long-term, this results in cost savings because we effectively gain more headroom before we need to scale up.


Our shard count balancer algorithm uses a greedy approach that runs until every pod holds either


*|N / P|* or


*|N / P|+1* shards, where


*N* is the total shard count and


*P* is the number of pods.


First, it calculates each pod’s target. The remainder shards are assigned to the pods already holding the most shards, minimizing unnecessary moves. Then it loops: find the pod furthest above its target, find the pod furthest below its target, transfer one shard between them, repeat. When choosing which shard to move, it picks the one whose point count best balances the cluster’s overall point distribution.


Below is a visual representation of the shard state before and after the shard count balancer algorithm runs on an example cluster with 6 shards and replication factor of 2:


Before and after shard count balancer algorithm


### Recovering replication factor for shards


When a pod is purged the replication factor is silently violated; the translator detects the under-replicated shards and restores them on a new peer


In production systems, we want a replication factor of 3 to tolerate AZ failures in the deployed region. However, when a shard gets dropped, Qdrant does not complain. Read requests will succeed if there is at least one replica, but write requests will fail if there are not enough replicas to respect


write_consistency_factor


. For high availability and automated recovery of shard replicas, we built this into our Kube-operators logic.


The Kube-operators translator scans every shard per reconciliation loop, compares its current replica count against the target replication factor, and for each under-replicated shard, picks the peer with the fewest shards that doesn’t already host that shard and adds a replica there.


---


## Results


As a result of the migration to Kube-operators:


- New cluster spin-up time dropped from hours to minutes, which let us retire the practice of keeping standby clusters for recovery.


- Clusters are now provisioned from a single config, with all required artifacts created together, instead of through a complex operation involving manual steps.


- Operational load no longer scales linearly with cluster count.


- Lifecycle-aware automation is now unlocked, supporting operations like horizontal scaling, replication factor recovery, and shard rebalancing, with room to add more capabilities in the future.


---


## Lessons Learned


Looking back at this migration, a few lessons stand out:


- **Manual operations don't survive growth.** The Helm-based setup was the right fit for the early stage, allowing us to iterate fast, handle traffic spikes through overprovisioning, and keep things simple. However, the same manual steps that worked well at POC scale became increasingly difficult to sustain as the fleet grew to 140+ clusters. For any rapidly growing fleet, it is worth investing in lifecycle automation before manual operations become a bottleneck.


- **Extensible automation beats one-off scripts.** Every capability described in this article, from horizontal scaling to shard rebalancing and replication factor recovery, is built on the same model: a Translator that reconciles observed state against desired state every 60 seconds. Rather than implementing each operation as a separate, bespoke workflow, we add its logic into the translators' reconciliation loop, where it runs automatically on every cycle. This is what lets a small team operate hundreds of clusters with minimal manual intervention.


- **Even distribution is cost savings.** Because Qdrant pre-loads all vectors into memory, the pod with the heaviest skew effectively determines the memory limit requirements of the entire cluster. Distributing shards evenly is therefore not only a matter of efficiency; it directly defers the next scale-up and the associated cost
