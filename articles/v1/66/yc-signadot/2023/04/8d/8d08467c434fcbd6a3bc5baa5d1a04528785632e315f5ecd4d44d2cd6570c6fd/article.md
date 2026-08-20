---
schema_version: "1.0.0"
document_id: "8d08467c434fcbd6a3bc5baa5d1a04528785632e315f5ecd4d44d2cd6570c6fd"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/cant-miss-kubecon-eu-sessions/"
published_at: "2023-04-12T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:77665d7289c79ab98197ae60e5d3608e1996152e942daedacb0a16320dbd2770"
---

# Can’t-Miss KubeCon EU Sessions

Next week, we’ll be in Amsterdam for KubeCon EU at Booth #SU8 (in the Lily Zone near the Mercedes Benz Picnic Lounge).


The sessions at KubeCon cover a wide range of topics, from beginner-level introductions to advanced technical discussions. With so many options to choose from, it can be challenging to decide which sessions to attend. However, there are always a few talks that we simply can’t miss.


### [A CI/CD Platform in the Palm of Your Hand - Claudia Beresford, Weaveworks](https://kccnceu2023.sched.com/event/1HyVW)


Many organizations that consider themselves “cloud native” tend to treat CI infrastructure as an afterthought. The providers available often use outdated infrastructure, which can create bottlenecks when teams need to gradually build, test, and release.


[Claudia Beresford](https://cbctl.dev/blog) does amazing things with Bare Metal, and is one of my favorite writers on GoLang. This talk is sure to be great.


### [The Power of Self-Managing Clusters - Sahithi Ayloo & Arun Krishnakumar, VMware](https://kccnceu2023.sched.com/event/1HyXS)


Managing the life cycle of a Kubernetes cluster can be difficult, particularly when you have to deal with thousands of clusters on your clouds. The Cluster API can aid in solving this problem by transferring the responsibility to “management clusters” that are in charge of managing their child workload clusters. Nevertheless, this approach has its own drawbacks.


Great quote from the talk description:


> *Can we get away with this overhead of “Management clusters” but still leverage all the richness of Cluster API? Yes, that is possible by transforming workload clusters into “Self Managing” clusters.*


[Sahithi Ayloo](https://github.com/sahithi) is the technical lead for Kubernetes-as-a-Service platform for a multi-tenant cloud provider platform at VMware, and I’m excited to see what she has to share on this topic.


### [Verifiable GitHub Actions with eBPF - Jose Donizetti, Aqua](https://kccnceu2023.sched.com/event/1HybH)


A few years back, there was a major hack of Codecov, a widely-used service in build pipelines. This caught the attention of the industry, and in response, a new solution was developed using Tracee, an open-source runtime security solution. This new solution introduced the concept of profiling with eBPF and verifying software builds, providing greater protection for build pipelines.


This talk is of particular interest to us at Signadot since the use of eBPF to verify builds kind of reminds me of our using OpenTelemetry to perform smart routing of packets for test: it’s using an existing robust system to achieve big improvements to the test/deploy process.


Jose Donizetti is a veteran of the intense Redis engineering at Shopify, and this is sure to be a treat to see.


### [Tips from the Trenches: GitOps at Adobe - Larisa Andreea Danaila & Ionut-Maxim Margelatu, Adobe](https://kccnceu2023.sched.com/event/1Hyca)


> *Larisa and Ionut have spent a big part of 2022 investing in GitOps, learning how to model a deployment system which encompasses stringent organizational CI/CD standards.*


As we talk about real operational excellence for releasing code more effectively, and managing infrastructure changes at scale, it’s useful to take a look at how some of the biggest operations around do it.


See some previous writing by this team here, on[Advanced Deployment Patterns With Argo](https://containerjournal.com/features/advanced-deployment-patterns-with-argo/) .


### [Understand Systems with OpenTelemetry - Ran Xu, Huawei & Xiaochun Yang](https://kccnceu2023.sched.com/event/1Hyb2)


> *OpenTelemetry can produce truly insightful system observability data. But if you don’t manage the flow of information, you’re just adding straw to the haystack.*


How to support efficient correlation query and real-time analysis in massive high-cardinality telemetry data and reduce the cost of telemetry data storage and computing is a challenge for the Huawei Cloud team, who will here offer some of the ways they managed all the data that OpenTelemetry can produce.


Xiaochun Yang of Northeastern University is one of most-respected voices on data management, this talk is sure to offer some major insights.


### [What Could Go Wrong with a GraphQL Query and Can OpenTelemetry Help? - Sonja Chevre & Ahmet Soormally, Tyk Technologies](https://kccnceu2023.sched.com/event/1HyVc)


Numerous developers are now turning to GraphQL, a query language and server-side runtime, to create a monolithic facade over their intricate microservice architecture. However, using GraphQL also poses new challenges when it comes to isolating failures and troubleshooting performance issues.


> *Can OpenTelemetry help? How good is OpenTelemetry support for GraphQL right now? What needs to be improved?*


While this use case for OpenTelemetry is more conventional than Signadot’s use of OpeneTelemetry baggage for smart routing, there is a connection: this points to a major frustration of modern development on microservices, since *when first developing locally, you won’t find problems like the one described above.*


That’s almost definitional: normally when developing locally, you’d be using a mock for GraphQL, so you can’t find any unexpected interactions with the service. What Signadot offers is a way to shift your testing so far left that even your first test environment can interact with a shared staging GraphQL service.


I really love seeing talks by[Sonja](https://twitter.com/SonjaChevre) , a really valuable member of the CNCF community, who wrote this great guide on[migrating from OpenTracing to Otel](https://tyk.io/blog/migrating-from-opentracing-to-opentelemetry/) .


### [The Next Episode in Workload Isolation: Confidential Containers - Jeremi Piotrowski, Microsoft](https://kccnceu2023.sched.com/event/1HyaJ)


Now THIS is an interesting idea: can you get more secure than straightforward containerization?


> *Kata-CC is an extension of Kata Containers that makes use of Trusted Execution Environment features present in modern CPUs to enhance security in a multi-tenant environment by combining workload attestation and memory encryption. An issue hindering wider adoption of this technology for some time has been hardware availability.*


I’ve never tried to control multi-tenancy to this degree, but it’s important to know where the bleeding edge is for container security


### See you all at Kubecon!


The theme of this Kubecon is very much that DevOps partnerships are achieving greater developer velocity and reliability despite environments of increasing complexity. We can’t wait to see you there.


Don’t forget to stop by Booth #SU8! We’d love to chat and hear about how you’re working with Kubernetes. We’ll also be giving away cool swag and raffling off prizes.


‍
