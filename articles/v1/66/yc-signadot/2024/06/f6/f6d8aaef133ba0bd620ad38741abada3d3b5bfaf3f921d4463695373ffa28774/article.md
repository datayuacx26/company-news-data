---
schema_version: "1.0.0"
document_id: "f6d8aaef133ba0bd620ad38741abada3d3b5bfaf3f921d4463695373ffa28774"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/shifting-testing-left-the-request-isolation-solution/"
published_at: "2024-06-21T20:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:b01b4cebb71b60e120e2cd326ff2c0f528266b146f7f0a0f72645c7ef4e66d74"
---

# Shifting Testing Left: The Request Isolation Solution

*Originally posted on*[The New Stack](https://thenewstack.io/shifting-testing-left-the-request-isolation-solution/) *. This is part of an ongoing series. Read previous parts:*


- [Why We Shift Testing Left: A Software Dev Cycle That Doesn’t Scale](https://www.signadot.com/blog/why-we-shift-testing-left-a-software-dev-cycle-that-doesnt-scale/)
- [Why Shift Testing Left Part 2: QA Does More After Devs Run Tests](https://www.signadot.com/blog/why-shift-testing-left-part-2-qa-does-more-after-devs-run-tests/)
- [How to Shift Testing Left: 4 Tactical Models](https://www.signadot.com/blog/how-to-shift-testing-left-4-tactical-models/)


In previous parts of this series we’ve discussed how QA has a role even as testing shifts left, and how there are multiple models for putting more accurate testing back in the hands of developers and software development engineers in test (SDETs). Now let’s consider a model that’s effective as teams scale beyond a single two-pizza team. Conceptually, testing in a complex microservice environment should include a highly accurate shared environment where developers’ tests and experiments won’t interfere with others’ updates.


In the paragraph above I say “back” in the hands of developers because, before microservice architecture and huge clusters of containers, developers had the power to run tests on their code that very closely resembled how their code would run in production.


Let’s discuss how such an approach works on an architectural level.


## **How To Leverage a Shared Cluster To Shift Left Testing?**


In a small team, a shared testing environment can put real testing in the hands of developers. With test versions of third-party dependencies and copies of the production versions of all internal services, it’s a great way for developers to do pre-release testing. In larger teams, the essential problem is that too many developers will want to test at once. When the test code is pushed to service A, the team that works with service B isn’t able to test, lest their tests fail or their changes interfere with service A’s tests.


The solution is to establish a highly reliable cluster for testing, and then let teams deploy test versions of services that don’t affect the cluster as a whole.


*Source: Signadot*


A few concepts that we’ll use throughout this explanation:


- **Baseline** — The baseline version of the cluster should include services and resources that are extremely close to the deployed production environment. Changes need to be merged to baseline before deployment, ensuring that there aren’t large gaps between this cluster and prod. This baseline is typically kept up to date with the main/trunk branch using CI/CD pipelines.
- **Sandbox** — The service or group of services on which developers are testing. In general this will involve a new version of an existing service, but a sandbox could also contain new services.


The core idea is that, with each request between services, we need to intelligently decide if the request should go to the baseline service or to the sandbox. This solution is generally referred to as request-level isolation of testing/development services.


## What We Need to Make Request-Level Isolation Work


The technical lift for request isolation isn’t zero, and it’s important to identify which components need to be in place to make the system work:


- **Request routing** — We also require some system to intelligently route requests. Every service-to-service request could be routed to a different target service based on the value of certain request headers.


At Signadot, request routing for sandboxes uses either a service mesh such as Istio or without a service mesh, using Signadot’s *DevMesh* . If using a service mesh, Signadot configures the mesh to perform request routing on its behalf without needing additional in-cluster components specific to routing. The DevMesh sidecar is a lightweight Envoy-based proxy that can be added to any workload through a Kubernetes pod annotation.


- **Context Propagation** — Finally, we must have a system to keep track of whether each requests is a “test” request from a developer working with a sandbox. As such, a system for context propagation with consistent headers is required.


At Signadot, we harness the power of OpenTelemetry to do this context propagation. OpenTelemetry’s “baggage” component is perfect for this need.


## Success Stories With Request Isolation


Many teams from mid-level to enterprise use request isolation in some form to put the power of highly accurate testing in the hands of developers. Here are a couple of examples.


### Uber’s Short-Lived Test Environments: Sandboxes in a Shared Cluster


At Uber, the frustrations mentioned in my previous articles with per-developer environments (out-of-date requirements, slow updates), lead to the development of Short-Lived Application Test Environments (SLATE) that created sandboxes within a cluster with up-to-date versions of dependent services.


*Source: Uber*


Uber goes one step further than the model I describe above: Rather than having everything work within an up-to-date staging cluster, SLATEs can also call production services as needed. The team describes the benefits in[their blog post](https://www.uber.com/en-MX/blog/simplifying-developer-testing-through-slate/) :


“SLATE significantly improved the experience and velocity of E2E testing \[end-to-end\] for developers. It allowed them to test their changes spread across multiple services and against production dependencies. Multiple clients like mobile, test suites and scripts can be used for testing services deployed in SLATE. A SLATE environment can be created on demand and can be reclaimed when not in reuse, resulting in efficient uses of infrastructure. While providing all this, it enforces data isolation and compliance requirements.”


### **DoorDash Gets Feedback 10 Times Faster**


Before implementing request isolation on a shared cluster, developers at DoorDash were doing final testing in a shared staging environment. Developers were forced to use mocks and contract tests to simulate how things would work on staging, and this imperfect replication caused staging to often break while testing new features.


With request isolation, developers are still using staging to test their work, but those tests are isolated from others meaning they won’t affect staging’s stability for others.


Using[Signadot’s local sandbox feature](https://doordash.engineering/2022/06/23/fast-feedback-loop-for-kubernetes-product-development-in-a-production-environment/) , the services a dev is experimenting with can run on their local workstation, while all requests go to the shared cluster. This allows much faster testing.


Developer experience specialists at DoorDash estimates that it’s 10 times faster to get feedback now compared to using a shared staging environment for final testing.


## **Conclusions: Back to Developer Testing**


When discussing[part 1](https://www.signadot.com/blog/why-we-shift-testing-left-a-software-dev-cycle-that-doesnt-scale/) of this series on Hacker News, one user wrote back with a[basic observation](https://news.ycombinator.com/item?id=40466868) :


“Any time the code you’re developing can’t be *run* without some special magic computer (that you don’t have right now), special magic database (that isn’t available right now), special magic workload (production load, yeah…), I predict the outcome is going to be bad one way or another.”


Ironically, this is the state of modern software development on Kubernetes clusters. The cluster can’t be fully replicated locally so we’re doomed to imperfect replication and mocks standing in for big chunks of our stack. Ever since the birth of agile methodologies, we’ve expected developers to be able to try out their code almost instantly.


This “shift left” then, is truly a return to form. With request isolation we can let developers do what they’ve always done: experiment, try things out, and discover what works.


## **Join the Signadot Community**


We’d love to show you how request isolation has worked for our users and explore how Signadot can help you. Check out[signadot.com](https://www.signadot.com/) for more user stories, tutorials and best practices.


‍
