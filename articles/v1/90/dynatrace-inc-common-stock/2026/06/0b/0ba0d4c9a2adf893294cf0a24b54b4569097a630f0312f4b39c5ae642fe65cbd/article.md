---
schema_version: "1.0.0"
document_id: "0ba0d4c9a2adf893294cf0a24b54b4569097a630f0312f4b39c5ae642fe65cbd"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/building-in-the-open-how-dynatrace-invests-in-open-source-to-move-the-industry-forward/"
published_at: "2026-06-23T20:24:37+00:00"
first_seen_at: "2026-07-20T03:31:43.037247+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:e54ddea70075ed96c63f508849ffda47ed55cd82684d82a345cd59cae21a0277"
---

# Building in the open: How Dynatrace invests in open source to move the industry forward

At Dynatrace, we believe the future of observability and cloud-native operations is open. Not “open” as a slide-deck buzzword, but open as in showing up every day to write code, review PRs, chair working groups, and build tools the community can use, extend, and make their own.


We’re proud to be an active contributor to the cloud-native ecosystem. We build in the open because we’ve seen firsthand that open collaboration produces better software for our customers, the community, and the industry.


This post looks at some of our most significant open source contributions — why we built them, why we open sourced them, and how they’ve grown into thriving community projects. It’s not exhaustive; for the full set, explore our[projects on GitHub](https://github.com/dynatrace-oss) .


## The OpenTelemetry community, setting the standard for observability


### What is the OpenTelemetry ecosystem?


The[OpenTelemetry® (OTel)](https://www.dynatrace.com/news/blog/what-is-opentelemetry/) ecosystem provides an open source, vendor-neutral way to generate, collect, process, and export telemetry data, specifically traces, metrics, and logs. Born from the merger of the OpenTracing and OpenCensus projects in 2019, the OTel community is now the second most active Cloud Native Computing Foundation® (CNCF) community after Kubernetes and the de facto standard for instrumenting cloud-native applications.


Before OpenTelemetry standards, every observability vendor had its own proprietary agent, data format, and instrumentation libraries. Switching vendors meant ripping out instrumentation and starting over. The standards change that by providing a single set of APIs, SDKs, and a Collector that work with any backend. You instrument once, and you can send your telemetry data wherever you need it.


### How Dynatrace contributes to the OpenTelemetry ecosystem


Dynatrace has been involved in the OpenTelemetry ecosystem since the project’s earliest days, and our engagement goes well beyond code commits. Our engineers hold leadership positions across the project:


- **Governance Committee:** Daniel Dyla, a Senior Open Source Architect at Dynatrace, serves on the OpenTelemetry Governance Committee, helping shape the project’s strategic direction.
- **Technical Committee:** Armin Ruech, a Team Lead and Product Owner at Dynatrace, sits on the Technical Committee, guiding the cross-language specification that ensures consistency across all OTel implementations.
- **SIG Maintainers and Approvers:** Dynatrace engineers serve as maintainers or approvers across multiple Special Interest Groups. Contributors like Mario Jonke, Marc Pichler, Evan Bradley, Giovanni Liva, Joao Grassi, and many more are active across these SIGs.
- **Community Management:** Adriana Villela, a Principal Developer Advocate at Dynatrace,[recently took on the role](https://www.dynatrace.com/news/blog/adriana-villela-of-dynatrace-takes-on-opentelemetry-community-manager-role/) of OpenTelemetry Community Manager, working to grow the contributor and end-user community and raise the OTel community’s profile within the CNCF and the broader open source world.


Dynatrace also ships its own distribution of the[OpenTelemetry Collector](https://github.com/Dynatrace/dynatrace-otel-collector) , with enterprise-grade support and curated components so teams can confidently run OTel in production.[Our acquisition of Bindplane](https://www.dynatrace.com/news/blog/dynatrace-to-acquire-bindplane-telemetry-pipeline/) , a telemetry pipeline built on OpenTelemetry standards, extends that commitment.


### How the community benefits


The OpenTelemetry community has fundamentally changed how the industry approaches observability. Developers no longer commit to a single vendor’s instrumentation at the code level, SREs can standardize on one set of libraries across polyglot environments. Organizations can evolve their observability strategy, switching or combining backends without re-instrumenting their applications.


### Get involved


Check out[OpenTelemetry.io](https://opentelemetry.io/community/) and find an interest group that matches your interests. Also check out the[first episode of the Observation Deck](https://www.youtube.com/watch?v=vnii6zvfvO4) to learn more about the community and how to get involved.


## W3C® Trace Context: Making distributed tracing actually work


### What is W3C Trace Context?


[W3C Trace Context](https://www.dynatrace.com/knowledge-base/w3c-trace-context/) is a specification that defines a standardized set of HTTP headers for propagating distributed tracing context between services. Before it existed, every tracing tool—both commercial and open source—used its own proprietary headers, which resulted in a high volume of broken traces. We saw the problem firsthand every day across thousands of customer environments: Traces would vanish whenever a request crossed a boundary between two monitoring tools or passed through a cloud provider’s managed service. The root cause was always the same: no agreed-upon header format.


Trace Context solves this by defining a universal traceparent header that all compliant tools and middleware recognize and forward. It’s a deceptively simple idea with a massive practical impact on end-to-end tracing in production.


### How Dynatrace contributes


Dynatrace helped lead W3C Trace Context. Alois Reitbauer, now Dynatrace’s Chief Technology Strategist, chaired the W3C Distributed Tracing Working Group; colleagues including Christoph Neumüller and Daniel Khan worked alongside engineers from Google, Microsoft, and New Relic to iterate on the standard, and Daniel Dyla contributed as a co-author alongside his OpenTelemetry work. Dynatrace was a founding sponsor of the working group when it moved into the W3C in 2018, and the first vendor to ship production support for the standard.


### How the community benefits


W3C Trace Context has been adopted by the OpenTelemetry project (which uses it as its default propagation format), Microsoft Azure services, Google Cloud Platform™ services, Elastic® services, and many others. Middleware, service meshes, and cloud-managed services now forward traceparent headers automatically, making end-to-end visibility achievable in environments where it was previously impossible. It’s one of those standards that, once adopted, makes you wonder how anything worked without it.


## OpenFeature: An open standard for feature flags


### What is the OpenFeature standard?


The[OpenFeature project](https://www.dynatrace.com/knowledge-base/openfeature/) , a CNCF Incubating project, defines a vendor-neutral open standard for feature flag management. It provides a unified API and SDK developers can code against regardless of the backend — an in-house solution, a commercial platform, or an open source tool.


Feature flags are a core practice in modern software delivery: They let teams decouple deployments from releases, roll out features to targeted user groups, run A/B experiments, and instantly kill-switch problematic features — all without redeploying code. But the ecosystem has historically been fragmented: Every vendor and homegrown solution has its own API, so migrating between solutions requires rewriting application code.


The OpenFeature project remedies this with a standardized abstraction layer. You write your evaluations once against the OpenFeature API; to switch backends — say, from an in-house solution to a commercial vendor — you swap out the “provider” (a thin adapter), and your application code doesn’t change.


### How Dynatrace contributes


Dynatrace initiated and led the creation of the OpenFeature project. In 2022, we assembled an industry consortium of top feature flag management solutions and submitted the standard to the CNCF as a sandbox project. We remain one of the project’s key drivers, contributing to the specification, SDKs, and provider ecosystem.


Dynatrace engineers play central roles in the project’s governance and day-to-day development:


Governance Committee: Michael Beemer, a Product Manager at Dynatrace, serves on the OpenFeature Governance Committee and has been instrumental in driving the project from its earliest days. Alois Reitbauer, Dynatrace’s Chief Technology Strategist and a driving force behind many of our open source initiatives, has recently passed leadership to Jonathan Norris following[our acquisition of DevCycle](https://www.dynatrace.com/news/blog/dynatrace-acquires-devcycle-to-strengthen-feature-delivery/) .


Project Maintainer: Todd Baert, a Software Engineer at Dynatrace, works almost entirely as an OpenFeature project maintainer, focusing on SDK development, integration points, extensibility, and the OpenTelemetry hook that bridges feature flags with observability data.


Observability is built in by design: The OpenTelemetry API supports OpenFeature flags, so feature flag evaluations are automatically captured as part of distributed traces. Observability platforms like Dynatrace can show you not just what happened in a request, but which flags were evaluated, what values were returned, and how those flags affected performance and user experience.


In early 2026, Dynatrace acquired the DevCycle® platform, a feature management solution built natively on the OpenFeature standard, deepening our investment in open, standards-based progressive delivery.


### How the community benefits


The OpenFeature standard lets developers adopt feature flags without vendor lock-in. Outgrow an in-house solution? Move to a commercial provider without rewriting code. Vendor sunsets their product? Your application code is unaffected; just swap providers. And because feature flag evaluations are captured as part of distributed traces, teams can correlate flag states with performance data, error rates, and user experience metrics — making progressive delivery measurably safer.


### Get involved


Visit openfeature.dev to learn about the standard, try a provider, or contribute to an SDK. The community is active on the[CNCF Slack](https://communityinviter.com/apps/cloud-native/cncf) .


## Keptn toolkit: Cloud-native application lifecycle orchestration


### What is Keptn toolkit?


Keptn toolkit is a CNCF Incubating project that provides event-driven orchestration for cloud-native application delivery and operations. It connects observability to operations through a declarative approach to automation for deployment pipelines, quality gates, and auto-remediation workflows.


In practical terms, Keptn toolkit lets you define Service Level Objectives (SLOs) and automatically evaluate them at every stage of your delivery pipeline. Did that new deployment meet its performance targets? Keptn toolkit checks your SLIs (Service Level Indicators) against your SLOs and decides automatically whether to promote the release or roll it back. It’s often described as “SRE in a Box.”


Field teams at Dynatrace kept seeing the same pattern across customer engagements: every organization was writing the same Jenkins scripts, the same ServiceNow® integrations, the same dashboard-checking routines, just with slight variations.


Keptn toolkit is tool-agnostic by design: it integrates with whatever CI/CD, testing, monitoring, and notification tools you already use. You define the “what” — your delivery and operations strategy — and Keptn toolkit orchestrates the “how” using your existing toolchain.


Keptn toolkit’s unique value in the CNCF landscape is its focus on observability-driven operations. SLOs drive its orchestration decisions, bridging the gap between “we deployed it” and “it’s actually working well for users.” For SRE and platform engineering teams, it’s a structured, declarative way to codify operational best practices without being tied to any specific tool.


### How Dynatrace contributes


Dynatrace created Keptn toolkit internally in 2019 and donated it to the CNCF as a sandbox project in 2020. It was promoted to CNCF Incubating status in 2022 after significant community growth.


Key Dynatrace contributors include Alois Reitbauer (Keptn toolkit creator and Dynatrace CTS), Andreas Grabner (co-founder and DevOps Activist at Dynatrace), and Johannes Bräuer (co-founder). The project was born directly out of our work helping customers implement autonomous cloud operations.


### Get involved


[Head to keptn.sh](https://keptn.sh/) to learn about the project, try the quickstart, or build an integration for your favorite tool.


### How the community benefits


Since joining the CNCF, Keptn has grown tenfold in adoption, with more than 100 production instances and more than 40 companies using it in production including Citrix, T-Systems, and others. The project has attracted dozens of community-built integrations and inspired the CDEvents standard in the Continuous Delivery Foundation.


## OffOn.dev: Building the Open Source Community of tomorrow


Every project in this post depends on something that doesn’t show up in a GitHub® commit graph: the people who sustain them — maintainers, contributors, mentors, and the next generation of developers who haven’t written their first PR yet. They’re often overlooked. Most platforms, foundations, and content formats are built with enterprises, legal teams, or marketing goals in mind, not for individual maintainers or attracting new contributors. Yet these are the people who keep open source alive.


[OffOn.dev](https://offon.dev/) was created to address that gap. Too many community spaces are built to react to problems — they grow, but stay focused on troubleshooting rather than adoption, innovation, and exploration. The Open Ecosystem takes a different approach: a practical, vendor-agnostic space for builders working with open standards and cloud-native tools, focused on reproducible, hands-on knowledge — tutorials with real code and configs, Q&A with accepted solutions, technical challenges, and a Community Voices space for members’ projects and demos.


This is not a sales channel. Companies like Dynatrace may sponsor with resources or funding, but sponsors don’t influence the content or the mission. The purpose is to sustain open source momentum and help create the maintainers of tomorrow’s digital landscape — a need made more urgent by digital sovereignty and emerging regulations like the CRA and EU AI Acts.


Whether you’re an SRE wiring up an OpenTelemetry pipeline, a developer experimenting with OpenFeature flags, a student writing your first contribution, or a maintainer mentoring the next generation — the Open Ecosystem is built for you. Introduce yourself, jump into a challenge, share what you know.[Head to OffOn.dev](https://offon.dev/) and come build with us.
