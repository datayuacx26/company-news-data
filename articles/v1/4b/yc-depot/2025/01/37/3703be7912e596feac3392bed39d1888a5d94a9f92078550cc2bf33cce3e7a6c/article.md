---
schema_version: "1.0.0"
document_id: "3703be7912e596feac3392bed39d1888a5d94a9f92078550cc2bf33cce3e7a6c"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-with-dagger"
published_at: "2025-01-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:4edfa8f59e69df130ca6faa063ab646dfb47263d09f850734f11edf14b82654d"
---

# Now available: Use Dagger with Depot

Continuing with our informal Drop Week, we're excited to announce our latest partner integration with[Dagger](https://dagger.io/) .


Starting today, you can run all your Dagger workloads on Depot through our GitHub Actions Runners. We're bringing the power of Depot's accelerated builds, faster GitHub Actions runners, and instant cache sharing directly to your Dagger workloads, right out of the box. No additional configuration needed.


We're happy to be the first partner in the new Dagger Powered program and excited to accelerate another type of build with the performance of Depot.


## How to use it


To get started, first connect Depot to your Dagger Cloud account for telemetry and visibility into your pipelines. You can configure that connection from the[Dagger tab inside Depot](https://depot.dev/docs/github-actions/integrations/dagger) .


Once you've configured your connection to Dagger Cloud, you can leverage Dagger with Depot GitHub Actions runners by appending an additional value to your runs-on label in your GitHub Actions workflows:


```text
runs-on  :   depot-ubuntu-latest,dagger=0.15.2
```


Specifying the dagger version in your runs-on label signals to our orchestration system that you want the runner to be pre-configured to leverage a Dagger engine running next door.


We launch a Dagger engine with a persistent cache for your specified version and configure everything inside the GitHub Actions runner to use it. When your job is done, we kill the Dagger engine and persist the cache to be reused automatically in subsequent builds.


This gets you all the benefits of Dagger without having to think about how to configure it inside GitHub Actions, and all the power of Depot's accelerated caching for Dagger pipelines.


## Pricing


Dagger invocations will incur the same **$0.04/minute pricing** as our Docker builds and will draw down your included Docker build usage.


## Conclusion


We're excited to bring Depot's performance to another workload and are excited to be partnering with Dagger on a new perspective for continuous integration. If you have any questions or need help getting started,[hop into our Discord Community](https://discord.gg/MMPqYSgDCg) and let us know.


If you're new to Dagger, you can also join the[Dagger Discord Community](https://discord.gg/dagger-io) to learn more!


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
