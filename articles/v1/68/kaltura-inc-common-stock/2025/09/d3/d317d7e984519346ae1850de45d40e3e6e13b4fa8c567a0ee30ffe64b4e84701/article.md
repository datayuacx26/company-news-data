---
schema_version: "1.0.0"
document_id: "d317d7e984519346ae1850de45d40e3e6e13b4fa8c567a0ee30ffe64b4e84701"
company_key: "kaltura-inc-common-stock"
company: "Kaltura Inc."
source_id: "kaltura-inc-common-stock-rss-8a80d100aa25"
canonical_url: "https://medium.com/kaltura-tech/still-managing-redis-in-kubernetes-for-argo-cd-we-stopped-96b9b825d987"
published_at: "2025-09-01T10:54:01+00:00"
first_seen_at: "2026-07-20T23:18:51.911067+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:f073d5cf44fce51ed73a1149e7e00d7ac805fec04b2767219cd7e490f620edae"
---

# Still Managing Redis in Kubernetes for Argo CD? We stopped.

# Still Managing Redis in Kubernetes for Argo CD? We stopped.


## ***Cut Redis. Cut Complexity. Cut Costs.***


[Netanel Kadosh](https://medium.com/@netanel.kadosh?source=post_page---byline--96b9b825d987---------------------------------------)


6 min read


·


Sep 1, 2025


--


Press enter or click to view image in full size


*At Kaltura, switching to* ***AWS ElastiCache Serverless for Valkey*** *helped us eliminate Redis ops, cut Redis costs by over 95%, and simplify GitOps at scale. This is the story of how we made that decision, how we made the switch, and what we’ve learned from it.*


*That’s how we turned ArgoCD into a* ***Lean, Mean, GitOps machine*** *.*


***Disclaimer:*** *While this post focuses on* ***AWS ElastiCache Serverless for Valkey*** *, teams using* ***Azure Cache for Redis*** *or* ***Google Cloud Memorystore*** *can take a similar approach. The core idea of offloading Redis to a managed service to reduce complexity and cost applies across clouds. Whether you’re on AWS, Azure, or GCP, simplifying your GitOps stack is within reach.*


## **Why We Rethought Argo CD’s Defaults**


Deploying Argo CD with its default manifests is easy. That’s part of the appeal. But defaults are built for getting started, not for running GitOps at scale.


The default High Availability (HA) configuration includes six extra pods: three Redis and three HAProxy. Each Argo CD instance brings its own set of these, increasing memory, CPU, and operational complexity. When you’re running multiple instances across various environments, the cost, both financial and cognitive, adds up quickly.


At Kaltura, our platform team manages **dozens of Argo CD instances** , powering GitOps across **hundreds of Kubernetes clusters** and **thousands of applications synced daily** . Over time, we found ourselves spending far too much time managing Redis. It wasn’t just the infrastructure; it was the attention it demanded.


Redis was slowing us down.


## **What We Were Up Against**


### **Redis Became a Cost Center**


On paper, Redis is lightweight. But in a real-world HA Argo CD setup, it became expensive. Six additional pods per instance quickly multiplied across environments.


Worse, Argo CD’s architecture involves Redis traffic flowing through HAProxy before hitting the actual Redis master. In many cases, that traffic often crossed availability zones. On AWS, that meant we were being charged for **every hop** **in both directions** .


Two hops per request. Four cents per gigabyte. Syncing thousands of apps added up.


At its peak, our Redis infrastructure cost us nearly **$2,000 a month** . That didn’t even include the time engineers spent maintaining it.


### **We Were Stuck in Redis Ops**


Redis isn’t set-it-and-forget-it. We found ourselves constantly monitoring pod health, tuning memory limits, updating connection thresholds, upgrading versions to mitigate security vulnerabilities like[CVE-2024–31989](https://cycode.com/blog/revealing-argo-cd-critical-vulnerability/) , and manually fixing breaking changes introduced by upstream updates. It wasn’t just a background service anymore; it had become a workload of its own.


And the frustrating part? We never set out to run Redis. We wanted to run GitOps.


### **Our Turning Point**


Eventually, we asked the obvious question: **Why are we managing Redis in the first place?**


We didn’t have a good answer. So we started looking for alternatives that aligned with how we already operate in the cloud, fully managed, scalable, and resilient by default.


That’s when we landed on **AWS ElastiCache Serverless for Valkey** . And for teams using **Azure** or **Google Cloud** , equivalent managed Redis offerings may work similarly well. The important thing is offloading the work, not the specific provider.


### **What We Switched To**


[Valkey](https://github.com/valkey-io/valkey) is a Redis-compatible engine that works seamlessly with Argo CD. It was created as a fork after Redis changed its licensing model, and AWS supports it as part of its ElastiCache offering.


The serverless variant was the real win. Instead of provisioning and managing Redis clusters ourselves, we could just point Argo CD to a managed endpoint. No patching. No scaling decisions. No custom monitoring. AWS would take care of everything.


Even better, ElastiCache lets us create logical databases, which means we can consolidate multiple Argo CD instances into a single backend while maintaining isolation.


This setup lets us eliminate Redis pods from our clusters, remove HAProxy entirely, and simplify our entire caching layer. We simply pointed Argo CD to the new endpoint and let AWS handle the rest.


## **Making the Move**


So, how did we make the switch?


We started by provisioning an **ElastiCache Serverless Valkey** instance and configuring it with a secure username and password. Logical DBs allowed us to support multiple Argo CD environments from a single backend.


Next, we updated the Argo CD config to use the external Redis endpoint. Since ElastiCache enforces TLS by default, we added the — redis-use-tls flag to all relevant components: the **application controller** , **repo server** , and **Argo CD server** .


Then, we disabled Argo CD’s built-in Redis and HAProxy pods in our Helm charts. That reduced pod count and immediately freed up compute resources in every cluster.


Here are the complete values.yaml we used for this


That was it. The migration required no downtime, and Argo CD rebuilt its cache automatically after restart.


Whether you’re on AWS or another provider, the steps are conceptually similar. Create the managed Redis backend, secure it, point Argo CD at it, and disable the in-cluster version.


## **What Changed for Us**


### **1. 95% Cost Reduction**


Our monthly Redis-related expenses dropped from around **$2,000** to **just over $100** . ElastiCache Serverless itself costs only **$30** , and the rest is offset by reduced network traffic.


Because AWS charges only for inbound traffic to ElastiCache and outbound traffic is free, we slashed our data transfer costs from **$0.04 to $0.01 per GB** .


The impact was immediately visible in our monthly AWS bill. The following graph from Cost Explorer shows the drop in Redis-related infrastructure spending after we migrated to ElastiCache Serverless:


Press enter or click to view image in full size


Monthly Redis-related costs before and after switching to ElastiCache Serverless.


With no Redis or HAProxy pods, we now have a leaner and faster GitOps.


### **2. No More Redis Maintenance**


We no longer patch Redis. We don’t scale it. We don’t worry about backups or failovers. AWS handles all of that for us.


What used to require engineering time now just works.


### **3. Improved Performance and Resilience**


ElastiCache Serverless scales automatically with real usage. When we trigger bulk syncs or onboard a new batch of applications, the Redis backend grows and shrinks without intervention.


The service runs across multiple availability zones with a **99.99% SLA** . That’s better resilience than we ever achieved with self-hosted Redis.


And in disaster recovery scenarios, we no longer wait for Redis pods to stabilize or for app state to rehydrate. Argo CD connects immediately to the external cache and continues as if nothing happened.


## **Lessons Learned**


This migration gave us more than savings; it gave us focus.


We no longer spend time troubleshooting Redis. We no longer lose sleep over memory pressure during high-sync windows. And we’ve stopped writing internal runbooks on how to upgrade Redis in place.


The move also forced us to ask deeper questions about other components we manage. If Redis can be offloaded, what else can?


This shift wasn’t about changing Redis. It was about changing how we think about platform architecture: leaner, simpler, cloud-native by default.


Redis was just the beginning.


## **Thinking About Making the Switch?**


If you’re currently running Redis in Kubernetes for Argo CD, start by taking a closer look at your actual costs. Look beyond compute, including network traffic and engineering time. Add up what you’re really spending.


If GitOps is about reducing manual work and focusing on the desired state, does running your own Redis align with that philosophy? In most cases, it won’t.


Try migrating a non-critical Argo CD instance to a managed backend like **ElastiCache** , **Memorystore** , or **Azure Cache** . The compatibility is high, and logical databases allow easy multi-tenant setups.


And when you’re ready, start with a single production cluster. The benefits show up immediately. You’ll spend less. You’ll ship faster. And you’ll finally stop babysitting Redis.


We stopped managing Redis and we’re not looking back. Now it’s your turn.


## **Let’s Talk**


Got questions? Running a similar setup? I’d love to hear how you’re scaling GitOps in your organization.


I’ve shared most of our journey here, but if you’re curious about our internal migration checklist, cost breakdown, or want to dig deeper, feel free to reach out.


You can drop a comment, connect with me on[LinkedIn](https://www.linkedin.com/in/netanel-kadosh) , or message me directly. Always happy to share notes and help others ship faster.
