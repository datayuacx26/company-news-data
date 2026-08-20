---
schema_version: "1.0.0"
document_id: "3d19bb76f3ef8bf32bac5137a7bb4dd811cd80b90478792813c614fd7b73091a"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/5-sre-tips-for-rails-workloads-in-kubernetes"
published_at: "2022-12-02T13:00:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:29bf0e4f2f6a4d309ca0681740b8f99b6214aef42aa7a2cd535aa7ab37139fdf"
---

# 5 SRE tips for Rails workloads in Kubernetes

In a[previous post](https://technology.doximity.com/articles/containerizing-ruby-on-rails-applications) we shared some suggestions for how to adjust a Rails application to make it run better in a containerized setting. In this post I'd like to explore the opposite relationship: five changes we have made to our Kubernetes clusters to better accommodate Rails workloads and workflows. All of these ideas can be relevant to non-Rails apps too!


## Selecting the right deployment strategies


The two deployment strategies Kubernetes offers out of the box are[RollingUpdate or Recreate.](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) Initially we selected RollingUpdate for all of our workloads; with this strategy some new pods are created before old ones are terminated. The RollingUpdate strategy is ideal for our web traffic because if Kubernetes shuts down all of the existing pods before it starts up any new pods, users will see 5XX errors until the new web pods are healthy, not good. That said, the RollingUpdate strategy created issues with our Sidekiq and Kafka workloads so we switched those to use the Recreate strategy.


Looking at Kafka first, consumers are basically just a group of processes working together to handle messages. Kafka consumers follow a rebalancing protocol when a new member wants to join a consumer group. During deployments we encountered bugs and performance issues while using RollingUpdate approach because new pods kept trickling-in and wanting to join the consumer group therefore triggering multiple rebalances. Switching our Kafka consumers to deploy using the Recreate strategy means all the existing Pods get killed before any new pods get created. This results in much more predictable deployment behavior, and now we see only a single rebalance with all new consumers joining the group at basically the same time.


Next, looking at Sidekiq, it does not stop workers from processing jobs when a new worker process joins, so we didn't have quite as many issues with using the RollingUpdate approach. However, it did occasionally cause subtle bugs on some deployments. For example, if a team created a new Sidekiq job and deployed the code that queues the job with the code that processes it in one PR that could cause issues. The issue is that with the RollingUpdate deployment strategy not all of the existing Sidekiq workers will be killed immediately. New web servers will start to come online while some of the old Sidekiq workers are still running. Suddenly jobs will be enqueued by these new web servers that the old Sidekiq workers don't know how to handle. Teams can of course design their deployments in a way to avoid this type of issue, but by switching to the Recreate strategy they don't need to think about it. This gives teams a simpler deployment mental model at the cost of having small windows of time during deployments when no Sidekiq jobs will be processed. We decided that was a good tradeoff to make.


## Managing interactive pods


Our engineers like to use the Rails console and they like to trigger ad-hoc rake tasks. We have CLI tooling that helps them launch an interactive pod to facilitate both of these workflows. This creates a container that is based on the currently deployed image. A few of the decisions we made about this type of pod are:


1. The Pod has a TTL by default to ensure it automatically gets removed after some amount of time, but can be launched without a TTL for long running tasks like big database migrations.
2. Our service mesh sidecar, Istio, runs in the pod ensuring networking parity with other types of workloads.
3. Prometheus does not collect metrics from the interactive pod.
4. Standard output is automatically discarded because it's easy to leak secrets when running interactive commands, but some logging we use for auditing purposes is forwarded through a proxy to our log aggregation vendor.
5. Environment variables are automatically added to the pod with information about who is accessing it that our Ruby tooling can use for auditing.


## Designing node groups


We contemplated but decided against creating different node groups for different workload types. The motivation for doing that would be to create isolation so that Sidekiq and Kafka workloads couldn't impact web Pods. Thus far though we haven't seen a compelling reason to make that switch, and keeping all workloads in the same node group gives the scheduler more flexibility. That said, we do use node groups for another reason. Some of our products touch Protected Health Information (PHI). These applications require extra layers of security and auditing, so we run all applications that touch PHI on a dedicated node group.


## Dealing with memory limits


Ruby apps have a tendency to increase memory usage over time. There are some settings like` MALLOC_ARENA_MAX=2` which we apply to all our Ruby applications to help minimize this issue. We even[upstreamed this change to the Ruby Paketo buildpack](https://github.com/paketo-buildpacks/mri/pull/433) which builds our Ruby images[(read more about why we chose Paketo here)](https://technology.doximity.com/articles/buildpacks-vs-dockerfiles) . Ultimately though we know that containers can exceed their memory limit for a lot of reasons, and when that happens, Kubernetes will issue an OOMKill to the container which the application has no chance to address. To allow our application processes to more gracefully respond to their imminent termination, we leverage a tool called[soft pod memory evicter](https://github.com/maxlaverse/soft-pod-memory-evicter) which will attempt to terminate the pods when they are at 90% of their memory limit in a way that allows them to gracefully drain their traffic first.


## Capacity planning with minimum replicas


Some of our traffic patterns come from emails our users subscribe to like: "What's new in Cardiology" When those emails hit inboxes user engagement generates a lot of traffic very quickly. Scaling up new pods in response to the traffic takes time. Therefore, our baseline capacity for our services needs to be high enough to tolerate sudden traffic bursts without the application performance degrading outside our established SLOs. We scale up the number of pods from this baseline capacity to continue operating at peak performance as traffic increases. Our alert suite and cost analysis tools like Kubecost help us make smart decisions about where to set these capacity baselines for each service and what the cost of that decision will be.


## Conclusion


At Doximity our products heavily utilize Rails applications for web traffic, Sidekiq workers, and Kafka consumers. There are a lot of knobs within Kubernetes to tweak how workloads behave in a cluster; we plan to continue optimizing our Kubernetes configuration based on what we observe from our Rails workloads.
