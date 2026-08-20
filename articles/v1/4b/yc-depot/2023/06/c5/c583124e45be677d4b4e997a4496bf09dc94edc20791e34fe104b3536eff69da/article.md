---
schema_version: "1.0.0"
document_id: "c583124e45be677d4b4e997a4496bf09dc94edc20791e34fe104b3536eff69da"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-windmill-got-13x-faster-docker-builds"
published_at: "2023-06-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:ba1534e8ecf876151cafb509a7d1dabb66014524c42beb4ec139c1c021fa4aff"
---

# Windmill.dev builds multi-platform Docker images faster with Depot

This post was written by **[Ruben Fiszel](https://www.linkedin.com/in/rubenfiszel)** , founder of **[Windmill](https://www.windmill.dev/)** .


[Windmill.dev](https://www.windmill.dev/) is an open-source developer platform where companies can build endpoints, internal workflows, and UIs from scripts in Python, Typescript, Go, and bash. Over 300 companies, including enterprise clients such as PhotoRoom, use Windmill as a core part of their production infrastructure for various operations, such as ETL pipelines, stitching together internal and external APIs, and more.


Windmill embraces open-source by making it as easy as possible to have it run anywhere. Docker is one of the most common ways our users deploy Windmill to their own environments. Either via our default single-instance deployment with Compose or into their Kubernetes clusters via our helm charts.


Using Windmill is only one` docker compose up` or` helm install` away. We release four times daily and must target both Intel and Arm CPU architectures. So building Docker images quickly for both of those is critical to our CI.


## Self-hosted GitHub Action runners can be a pain


Before Depot, we managed a fleet of GitHub Action runners in a Nomad cluster. With that setup, we had to manage a local Docker registry on a different VM with a large disk attached to it to leverage caching.


We struggled with reliability issues with the Nomad cluster, and it was a large amount of infrastructure to maintain for our CI. Our caching story was brittle as we had to keep a separate VM that acted as a local Docker registry for persisting cache between builds. It was latency prone and would often require manual intervention to clear out the cache, taking down our CI workflows at the most inconvenient times.


Self-hosted GitHub action runners were also inconsistent at times and would die mid-job.


Additionally, our provider Hetzner didn't have any Arm VMs at the time. Because of that, we couldn't build multi-platform images for Windmill as we wanted to use native Intel & Arm CPUs because emulation was painfully slow.


The amount of time we had to dedicate to managing this complexity to support our CI was distracting and ultimately slowed down our ability to ship new features.


## Depot makes builds instantly faster


With Depot, we removed all this self-hosted infrastructure and moved back to traditional GitHub Actions runners to make our CI more consistent and faster. It was straightforward to switch to Depot, effectively a one-line code change to swap` docker build` for` depot build` inside of our workflows.


We don't have to worry about caching anymore. Depot handles caching for us, so we don't have to think about it, maintain a separate registry cache, or save & load layer cache in between builds. It's immediately available across builds. The combo of caching alone reduced our regular Intel build times from **17 minutes to 4 minutes** .


We also got the ability to build Arm containers natively, which has been massive for our build times; we went from **55 minutes to 4 minutes** for building Arm images.


---


Want to try out Depot for your own Docker image builds?[Sign up for our free tier](https://depot.dev/sign-up) and get started today.


Don't miss an update — Follow us on[Twitter](https://twitter.com/depotdev) or join our[Community Discord](https://discord.gg/MMPqYSgDCg) .


Ruben Fiszel


Founder at Windmill
