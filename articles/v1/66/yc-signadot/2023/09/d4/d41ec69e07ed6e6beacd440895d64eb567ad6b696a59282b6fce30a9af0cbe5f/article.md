---
schema_version: "1.0.0"
document_id: "d41ec69e07ed6e6beacd440895d64eb567ad6b696a59282b6fce30a9af0cbe5f"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-to-reduce-pain-during-development-on-a-microservice-architecture/"
published_at: "2023-09-19T04:58:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:4fa28544b3e27b33d50ede86793a327eb4d19ac8f0e21ba87203192088429b7a"
---

# How to reduce pain during development on a microservice architecture

## Introduction


Microservices offer a range of benefits, from scalability to fault isolation. However, they also introduce complexities, especially when it comes to development and debugging. This post aims to address some of these challenges, inspired by a[Reddit discussion](https://www.reddit.com/r/golang/comments/168vso1/how_to_reduce_painwasting_time_during_development/?utm_source=share&utm_medium=web2x&context=3) on the topic.


## The Challenges


1. **Debugging** : Traditional debugging tools often don’t work with microservices, especially running in a cluster.
2. **Dependency Management** : Running dependent services locally can be resource-intensive. As the original poster puts it:


> The services have their own dependencies like hydra, postgresql and so on. So in this case dependencies of services consume much more memory on my local PC. I need something very small and simple.


1. **Deployment** : Frequent deployments can slow down the development cycle. This can be the biggest transition from the monolith before: previously we could write code and instantly go click ‘refresh’ to see our changes. Now the added minutes (whether it’s 3 or 50 minutes) can really start to cramp our style.


I have a theory that part of this pain is about the practices we’re used to: as someone who started with small local projects and monoliths that were easy to mock locally, I got used to jus ‘trying stuff’ with my code, making changes and seeing the effects. I’ve often thought, as I once again tapped my desk waiting for my code to compile and deploy, that if I was an old-school programmer who planned her program on a legal pad, I might be better suited to an environment where it took 10 minutes to see the results of code changes.Part of the ethos of platform engineering, though, is to make our process work well with the developers we currently have, rather than hoping we’ll all suddenly change the way we work.


1. **Mocking** : A solution mentioned on this reddit thread is ‘can’t the other teams write you good mocks for their services’ and while this is possible in some cases, it’s certainly not standard practice. Often the work of mocking falls on the other teams that rely on said service. Writing and maintaining mocks for dependent services can be cumbersome.


## Solutions


### Debugging


1. **Remote Debugging** : Tools like Delve allow you to attach a debugger to a running container. This can be particularly useful when working with Kubernetes.
2. **Logging and Tracing** : Implement comprehensive logging and tracing to capture the state and behavior of the service. Critically: it’s best to be using an automatic tool for instrumenting your application for traces (and logs if supported). If you’re adding log lines one at a time, the the pain of this process will be *intense* as you add a few print statements, wait to build/deploy, don’t get the info you need, and have to restart. OpenTelemetry is a good choice for this. If you’re too impatient to wait for OpenTelemetry to report data, or don’t want to stand up an observability backend, consider having OpenTelemetry report to a local collector and dashboard for testing. Setting up automatic instrumentation, a collector, and a local dashboard can feel like a fairly ‘heavy’ process to get started, but once its in place it’ll offer you something that feels close to debugger for a distributed system.


### Dependency Management


1. **Docker Compose** : Use Docker Compose to run dependent services. Docker can be much more memory efficient than trying to run a Kubernetes cluster locally. This allows you to manage dependencies in a more isolated manner. The workability of this solution will depend on your scale in two ways: whether dependent services and their resources can get under threshold to work on your local workstation, and whether you face serious issues keeping your local docker compose in sync. From some teams I’ve spoken with, this tool chain worked, but developers were waiting an hour two first thing in the morning to download, build, and start up new version of all their dependencies. This isn’t as bad as waiting half an hour to deploy each code change, but still can affect engineer’s workflow.
2. **Shared Development Cluster** : If your microservices are too numerous or complex, consider a shared development cluster. Tools like Signadot can help immensely here, as it helps developers experiment on a shared cluster without their changes impacting others’ workflows. Other strategies like namespaces in your cluster can also provide the isolation needed for different teams’ experiments.


### Deployment


1. Namespaces: Well encapsulated in a single comment by Anirudh:


> if the number of services is low, you could spin up a dedicated namespace in K8s to run the services you need. But you’ll need to spin up everything in this namespace which could be slow depending upon how many entities you need to spin up. In this approach, you can use tools like skaffold to run the services you are changing locally and “sync” that code into the cluster to get the “hot reload” functionality without having to rebuild docker images.


1. **CI/CD Automation** : Automate the deployment process as much as possible to reduce manual errors and speed up the cycle. One user on the reddit thread puts it perfectly: “All testing must be automated and run fast.”


### Mocking


Mocking is a part of the standard advice for developers trying to replicate their environments, this generally has two components. Contract Testing uses contract tests to ensure that your mocks are behaving as the real services would. Service Virtualization uses tools like WireMock to simulate the behavior of dependent services.


### **The Limitations of Mocking**


This is worth its own section since we often here ‘Just mock everything else’ as a solution from people who don’t have much experience doing platform engineering for a large team.


**Maintenance Overhead**


One of the key challenges with mocking is keeping the mocks up-to-date as microservices often evolve rapidly. As services grow more complex, the mocks can become almost as difficult to understand and maintain as the service they’re emulating.


**Incomplete Simulation**


Mocks might not fully replicate the behavior of a service, especially if that service is stateful or has complex interactions. They often use static or generated data that might not reflect the current state of a real service, leading to false positives or negatives during testing.


**Inter-Service Dependencies**


Mocks can’t simulate the network latency or failures that might occur in a real microservices environment. If multiple services are involved in a transaction, mocking one without the others can lead to an incomplete test scenario.


**Responsibility Sharing**


Notable in the discussion on reddit and in the Rands Slack where a similar discussion occurred recently: it’s not always clear who should be creating mocks. It might make sense for team A to make the mocks to be used by others relying on team A, but this isn’t the general practice, meaning that mocks often most accurately reflect ‘this is what we expect the other service to do’ rather than a real understanding of the way team A’s service fulfills its contract


## Best Practices


One theme that comes up repeatedly when trying to test with microservices is the realization that separations of services don’t reflect separation of workloads and domains, meaning that interdependence is so significant it makes separate testing impossible. As reddit user ghostsquad4 put it


Microservices should not just “be small”. In fact, maybe we need to start a name change, it should be called “single-domain-service”. It can be helpful to think of a domain in the same way as a team of people, specifically though an autonomous team (one who doesn’t need to coordinate with others to get their job done). Maybe an example of this is when your app needs to “get data” (from other teams/services), but does not rely on the behavior of how that data is compiled. (The difference between GET and POST/PUT).


## Request isolation instead of replication


In a recent talk from the Lyft engineering team, about the implementation of testing on a shared cluster by[Matthew Grossman](https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-3-extending-our-envoy-mesh-with-staging-fdaafafca82f) of the Lyft Platform Engineering group, we see a new path to a solution:


> **We fundamentally shifted our approach for the isolation model: instead of providing fully isolated environments, we isolated requests within a shared environment.** At its core, we enable users to override how their request flows through the staging environment to conditionally exercise their experimental code.


Such a solution has its own engineering overhead for setup, but offers a workable way to[test microservices in a shared environment](https://www.signadot.com/blog/leveraging-service-mesh-for-dynamic-traffic-routing-in-shared-kubernetes-environments/) , where the requests to and from this service under test are isolated, without needing to create a whole separate cluster or mocking a large number of services.


## Conclusion


While microservices introduce complexity, especially in debugging and dependency management, several strategies and tools can mitigate these challenges. By implementing new team practices, and doing the work of providing appropriate tools and tests to your organization, you can approach the ease that developers felt emulating a monolith within their laptop.


‍
