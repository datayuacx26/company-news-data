---
schema_version: "1.0.0"
document_id: "e6ea3f13ab574390781d61c305d0209b2df9af79e0db004dcce301657f143a01"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-overmind-got-10x-faster-dev-container-builds"
published_at: "2023-07-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:10d5e8fc34739a6bd5c7926022d624541770bf0d077d212dc3801b56eaa1c9e7"
---

# Overmind uses Depot to build Dev Containers 10x faster

This post was written by **[David Schmitt](https://www.linkedin.com/in/davidschmitt/)** , Staff Software Engineer at **[Overmind](https://overmind.tech/)** .


[Overmind](https://overmind.tech/) is a SaaS Terraform impact analysis tool. It discovers your AWS infrastructure to calculate the blast radius of a Terraform change, even if Terraform doesn't manage the affected resources.


The blast radius report can identify the causes of outages by showing you which affected resources caused which problems. While also helping you to deploy changes faster and more confidently as you understand what resources the change impacts before you make it. If a change is too risky, you can hold it back, helping prevent outages in the first place.


Overmind leverages Dev Containers to manage and roll out a standard set of development tools and configurations to their engineering team.


“As new tools were added to the IDE, versions were updated, or baked-in configuration was modified, the container would need to be rebuilt. Local build times became a regular morning nuisance for the entire team,” said David Schmitt, Staff Software Engineer at Overmind.


“We started pre-building the image in GitHub Actions. We needed a multi-platform image that could run on Intel and Arm, so we were stuck using emulation for the Arm image,” said David. “Our build times were over 20 minutes because of the slow emulation.”


“We tried running our own BuildKit instances in Kubernetes so we could leverage native Arm instances,” said David. “But, this caused us to 10x our cluster utilization and often exhausted our reserved capacity, requiring manual intervention.”


## Faster multi-platform Dev Container builds


When Overmind moved their Docker image builds to Depot, they were able to quickly switch that portion of their GitHub Action workflow because of the drop-in integration with` docker buildx build` .


The fact that Depot integrated with` buildx` rather than replacing the whole Github runner was perfect for us. It was a drop-in replacement for our Kubernetes approach and meant we only used the super-beefy build hardware in the section of the pipeline that actually needs it; the Docker build.


— David Schmitt, Staff Software Engineer, Overmind


As a result, Overmind took their multi-platform Dev Container builds from over 20 minutes to 2 minutes when a complete rebuild is needed and mere seconds for minor configuration changes. As a bonus, they switched their regular service builds to Depot and saw those builds drop from 10 minutes to 50 seconds.


“Depot provided good documentation and an easy onboarding process, allowing us to switch from the rather intricate hand-crafted pipelines to a simple GitHub Action that was faster and better,” said David. “When we started evaluating Docker build services, Depot quickly rose to the top both in speed and reliability.”


## Less infrastructure and faster iterations


Now that Overmind is using Depot to build their Dev Containers and their regular service container images, they've been able to scale their Kubernetes cluster back down.


“Depot allowed us to save money on our Kubernetes cluster and unblock our team waiting for slow builds. It allows us to only pay for what we need at a much better performance and reliability point.”


David Schmitt


Software Engineer at Overmind
