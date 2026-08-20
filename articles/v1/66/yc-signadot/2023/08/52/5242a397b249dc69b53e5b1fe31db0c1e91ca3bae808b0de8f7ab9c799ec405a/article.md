---
schema_version: "1.0.0"
document_id: "5242a397b249dc69b53e5b1fe31db0c1e91ca3bae808b0de8f7ab9c799ec405a"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/kubernetes-sidecar-containers-a-new-dawn-for-container-orchestration/"
published_at: "2023-08-03T08:29:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:7cc8323f87179bb323f383516bf5bae404a6bb41ed709a7c1d598cd2e768e04d"
---

# Kubernetes Sidecar Containers: A New Dawn for Container Orchestration

**Update:** This post was written when native sidecar containers were alpha in Kubernetes 1.28. The feature has since graduated to beta in Kubernetes 1.29 and became stable (GA) in Kubernetes 1.33, released in April 2025.


Kubernetes, the de facto standard for container orchestration, has always been about managing complex, multi-container workloads. However, one area that has been a challenge for Kubernetes users is the management of sidecar containers. This blog post aims to shed light on the challenges of sidecar containers, the newly introduced feature to address these challenges, and the use-cases that can now be enabled with this feature.


## The Challenges of Sidecar Containers


Sidecar containers are helper containers that run alongside the main container in a pod, providing additional functionality like logging, monitoring, or networking services. While they are an essential part of many Kubernetes deployments, managing them has been a challenge.


The main issue has been the lifecycle management of sidecar containers. Until now, sidecar containers did not have a special status; they were just regular containers in your pod. This posed problems because they weren’t available for the full lifecycle of the pod, notably during the initialization process. If your init containers needed services provided by a sidecar container, you were going to have a hard time.


Moreover, the shutdown process of a pod could be blocked by a sidecar container. If the main container completed its task and exited, the sidecar container could keep the pod in a running state, which could lead to unexpected behavior and resource wastage.


## Introducing the Sidecar Feature


To address these challenges, Kubernetes has introduced a new feature in its 1.28 release: Sidecar Containers. This feature introduces sidecar containers as a new type of init container that starts before other containers but remains running for the full duration of the pod’s lifecycle. Importantly, sidecar containers will not block pod termination.


The new feature is currently in Alpha and can be enabled with a feature gate on the API server. It’s worth noting that it’s unlikely to be available on managed Kubernetes services until it gets to General Availability (GA).


## How to Use Sidecar Containers


The use of sidecar containers in Kubernetes involves defining them in your pod’s configuration file. Here’s a step-by-step guide on how to do it:


1. **Define Your Sidecar Container**


In your pod’s configuration file, define your sidecar container just like you would define any other container. The sidecar container should be listed in the ***initContainers*** field of the pod specification. Here’s an example of a sidecar container that acts as a proxy:


In this example, ***istio-proxy*** is the sidecar container, and it’s marked as a sidecar by setting ***restartPolicy*** : Always. This tells Kubernetes to treat this container as a sidecar and manage its lifecycle accordingly.


1. ‍ **Configure Lifecycle Handlers and Probes**


Sidecar containers can have PostStart and PreStop lifecycle handlers, as well as all types of probes (startup, readiness, liveness). The readiness probes of sidecars will contribute to determining the whole Pod readiness.


1. ‍ **Understand the Restart Behavior**


During the sidecar startup stage, the restart behavior will be similar to init containers. If the Pod’s ***restartPolicy*** is ***Never*** , a sidecar container that fails during startup will not be restarted, and the whole Pod will fail. If the Pod’s ***restartPolicy*** is ***Always*** or ***OnFailure*** , the sidecar container will be restarted.


Once the sidecar container is started (postStart completed and startup probe succeeded), this container will be restarted even when the Pod’s ***restartPolicy*** is ***Never*** or ***OnFailure*** . Furthermore, sidecar containers will be restarted even during Pod termination.


The sidecar feature launched in Alpha in Kubernetes 1.28 and is stable (GA) as of Kubernetes 1.33, so it is now safe to rely on in production clusters running 1.33 or later.


## Use Cases Enabled by the Sidecar Feature


The introduction of sidecar containers opens up a range of new possibilities for Kubernetes users. Here are a few examples:


1. **Service Meshes:** Service meshes like Istio or Linkerd inject an envoy sidecar into each pod to manage networking services. With the new sidecar feature, these service meshes can now ensure that the sidecar is available for the full lifecycle of the pod.
2. **Logging and Monitoring:** Sidecar containers are often used to collect and ship logs from the main container. With the new feature, these sidecar containers can now be assured of running for the full duration of the pod’s lifecycle, ensuring that no logs are missed.
3. **Secret Management:** Sidecar containers can be used to fetch secrets for the main container. With the new feature, these sidecar containers can start before the main container, ensuring that the secrets are available when the main container starts.


## Conclusion


The introduction of sidecar containers in Kubernetes is a significant step forward in managing complex, multi-container workloads. While it’s currently in Alpha, the feature promises to address some of the long-standing challenges with sidecar containers and enable new use-cases.


For more information, you can refer to the[Kubernetes Enhancement Proposal (KEP)](https://github.com/kubernetes/enhancements/tree/master/keps/sig-node/753-sidecar-containers) and join the discussion on[Hacker News](https://news.ycombinator.com/item?id=36663060) . As always, it’s recommended to thoroughly test new features in a non-production environment before rolling them out in production.


The feature reached General Availability in Kubernetes 1.33. Happy Kubernetes-ing!


‍
