---
schema_version: "1.0.0"
document_id: "e2dcc1fe2d66c86f58930589e48f15e7a35ecb7e166e78f0752b5d6ea259ae31"
company_key: "mcdonald-s-corporation-common-stock"
company: "McDonald's Corporation"
source_id: "mcdonald-s-corporation-common-stock-rss-e3f7e88d5cc9"
canonical_url: "https://medium.com/mcdonalds-technical-blog/when-a-missed-message-becomes-a-missed-meal-1a44cb0051f3"
published_at: "2026-08-12T14:03:07+00:00"
first_seen_at: "2026-08-12T14:18:39.956323+00:00"
fetched_at: "2026-08-12T14:18:40.773548+00:00"
content_hash: "sha256:1c38fb670ea4978d01c73efb981f86ce396f8b5bdf59c8c52994abae056cf0d6"
---

# When a Missed Message Becomes a Missed Meal

Kubernetes


Cloud Computing


Software Engineering


Distributed Systems


DevOps


# **When a Missed Message Becomes a Missed Meal**


[Global Technology](https://medium.com/@global_technology?source=post_page---byline--1a44cb0051f3---------------------------------------)


6 min read


·


Just now


--


Why graceful shutdowns matter when millions of orders are in flight.


Press enter or click to view image in full size


*by: Samuel Hapgood, Software Engineer III*


**Quick Bytes**


- At McDonald’s scale, rare failures in distributed systems can impact real customer and crew experiences
- Graceful shutdown patterns in Kubernetes — across application code and infrastructure — help protect in-flight work during scaling and deployments
- With the right shutdown strategies, teams can reduce message loss, improve reliability, and ensure orders make it to the kitchen


Imagine you’re placing a mobile order at McDonald’s during a busy lunch rush. You hit “submit,” and the kitchen never gets the order. Now you’re left waiting — no food, no updates, and growing frustration.


At first glance, it seems like an easy problem to avoid.


*How hard can it be to just keep track of the order?*


But in modern, large-scale distributed systems — especially those that must scale continuously — that’s not always straightforward. Behind the scenes, platforms like Kubernetes (which orchestrate containerized applications in the cloud) are continuously rescheduling pods as part of normal operations such as scaling applications, performing rolling updates, and maintaining cluster health.


During these transitions, in-flight requests or background messages can be interrupted in subtle ways. A pod might shut down before finishing a task, a request might be canceled mid-flight, or a message might be acknowledged by a receiving service, but never actually processed. Even with strong messaging platforms and best practices in place, application shutdowns can still interrupt in-flight work if they are not handled carefully.


These are edge cases — but at McDonald’s scale, edge cases happen often enough to matter. In the US alone, our platforms support roughly one million orders per hour on a dynamically scaled cluster of hundreds of nodes. At that volume, reliability becomes critical to ensuring a seamless experience for customers, crews, and restaurant operations.


Preventing these failures requires a deeper understanding of how Kubernetes handles shutdown signals, termination periods, and traffic routing. With the right patterns in both infrastructure and application code, teams can dramatically reduce lost work and ensure that critical operations — like your hypothetical lunch order — make it through reliably.


Let’s walk through a few simple examples and ideas that illustrate how these challenges show up in real distributed systems.


**Designing for graceful shutdowns in application code** When applications run in a containerized, orchestrated environment like Kubernetes, they’re constantly exposed to events outside their control: scaling operations, rolling updates, node drains, and infrastructure faults. Any of these can interrupt long-running requests or background work if the application doesn’t shut down cleanly.


Without deliberately handling in the application code, these interruptions can lead to inconsistent behavior, partial operations, and hard-to-diagnose bugs — issues that become more visible as systems scale.


Most modern frameworks provide built-in lifecycle hooks to help developers respond to shutdown signals predictably. These hooks are triggered when the underlying process receives termination signals (such as SIGTERM in Linux), giving the application a short window to finish inflight work, stop accepting new requests, and clean up resources.


For example, the .NET hosting model exposes several lifecycle events through dependency injection, including an ApplicationStopping hook. Developers can register callbacks that run automatically when the application begins a graceful shutdown:


Press enter or click to view image in full size


As described in Microsoft’s documentation, the[ApplicationStopping cancellation token](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.ihostapplicationlifetime.applicationstopping?view=net-10.0-pp) is:


“Triggered when the application host is performing a graceful shutdown. Requests might still be in-flight. Shutdown will block until all callbacks registered on this token have completed.”


In practice, this means the application has been asked to stop — but it still has a brief grace period to finish what it’s doing. To see it in action, you can run a sample .NET application with the above hook and send a termination signal such as:


kill <pid>


Press enter or click to view image in full size


You’ll see the callback fire before the process exits, demonstrating how the application receives and responds to graceful shutdown events. While this graceful shutdown period is occurring, Kubernetes stops routing new traffic to the pod so it can focus on completing requests and background work already in progress.


It’s also important to consider scoped requests, proper disposal of services in the request lifecycle, the use of transactions (begin, commit, rollback), and retry mechanisms. Together, these lifecycle hooks and conventions form the foundation for building resilient behavior during termination — such as draining connections, rejecting new requests, flushing in-memory buffers, committing final messages, or safely stopping background workers.


**Signaling readiness during shutdown** When your app is orchestrated by Kubernetes, health signals are how the platform decides whether to restart a container (liveness) or stop sending it new traffic (readiness). Getting this right is critical to graceful termination:


- [Liveness probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) **** determine if a container is running as expected. If a liveness probe fails, Kubernetes automatically restarts the container to attempt to recover it.
- [Readiness probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) determine if a container is ready to receive incoming traffic. If a readiness probe fails, Kubernetes removes the pod from the Service load balancers, preventing traffic from being directed to it if it’s not ready to handle it.


Expanding on our example, we can define a readiness probe that returns a non-ready HTTP status code when the application stopping event is fired. This distinction is important: it ensures the application does not accept additional traffic while it is shutting down.


During this grace period, while the application is shutting down, we can safely clean up long-running tasks and resources before shutdown — reducing the risk of partial work or lost messages. In practice, this might include finishing in-flight HTTP requests while refusing new ones, committing or rolling back in-process transactions, stopping polling operations, releasing distributed locks for another replica to take over, and logging essential details before the application exits.


The following is an example of /readiness and /liveness endpoint responses during termination.


Press enter or click to view image in full size


**Graceful shutdowns don’t stop at the application layer** Up to this point, we’ve discussed examples for handling shutdowns in application code. But graceful termination isn’t just an application concern — the infrastructure must participate as well.


Kubernetes and the surrounding platform can be tuned to optimize cost, performance, or elasticity, and those choices affect how aggressively scaling and rescheduling operations occur. Because of this, understanding how your cluster behaves during scaling and termination is essential for designing applications that fail gracefully and respond predictably to pod lifecycle events.


Kubernetes provides terminationGracePeriodSeconds setting to control how long a pod has to shut down before receiving a SIGKILL. According to the[Kubernetes documentation](https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/#hook-handler-execution) , this grace period applies to the total time it takes for both the[PreStop hook](https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/#container-hooks) to execute and for the Container to stop normally. This gives applications time to finish in-flight work before forced termination.


If you’re using a service mesh like Istio, similar constructs exist to coordinate shutdown behavior.[Istio’s terminationDrainDuration](https://istio.io/latest/docs/reference/config/istio.mesh.v1alpha1/#ProxyConfig) setting controls how long the sidecar proxy waits before stopping, ensuring that in-flight requests (like any remaining database or API calls) complete even as the pod terminates. This helps prevent active connections from being cut off prematurely.


**What this means at McDonald’s scale** The patterns presented here are not exhaustive — they’re a starting point for building resilient shutdown behavior in distributed systems.


At McDonald’s, graceful shutdowns aren’t just about clean deployments or infrastructure hygiene. They help ensure that in-flight work completes reliably, orders reach kitchens, and customer experiences remain consistent — even as platforms scale, evolve, and deploy continuously.


Understanding how Kubernetes, application runtimes, and supporting infrastructure coordinate during termination allows teams to reduce message loss and protect critical operations during everyday events like scaling and rolling updates.


Because at McDonald’s scale, reliability isn’t optional — and every message matters.
