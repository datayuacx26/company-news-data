---
schema_version: "1.0.0"
document_id: "6d94f73ef9f21b3115c748f9fed3643ec4a747a3d9253824ec39638ed66bc228"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/drop-week-01"
published_at: "2023-07-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:8557fb2d63d5d4e2ffe112cfc5d0fd91849b70193f4e9ff428df3cf120f61ea3"
---

# A recap of Depot Drop Week #01

Our first Depot Drop Week is in the books, and we're excited by the massive response we've had throughout the week! It's been fantastic to interact with the community and see how each new feature we announced will help you build your Docker images faster.


In case you missed our announcements throughout the week, here's a recap of what we announced:


## Cache Storage V2


We[announced our brand new cache architecture](https://depot.dev/blog/cache-v2-faster-builds) for persisting and making your Docker layer cache instantly available across builds in CI and locally on your machine.


With this new architecture, we move away from EBS volumes and instead leverage a[Ceph](https://docs.ceph.com/en/latest/rados/) storage cluster that allows us to scale storage to meet your project's needs. It unlocks many new features, such as the ability to configure your cache size at a project level, so you can use a larger cache size for those large images you're building.


As a bonus, our cache storage v2 offers 10x the write throughput and 20x the read throughput. The net result of that is the most expensive part of some customer builds is going from **8 minutes to 40 seconds** .


## Depot AI: An easier way to embed AI models into your Docker image builds


[depot.ai](https://depot.ai/) is a free open-source Docker registry for public AI models. We've made it easy to embed any of the top 100 Hugging Face models into your Dockerfile with a single` COPY` statement.


We're building highly optimized Docker images for each model and hosting a registry over the top of Cloudflare R2 storage to make it fast and easy to embed these models into your own Docker images. This is a massive upgrade for folks doing anything in the generative AI space, as this is a huge time saver across builds.


You can read all the details of how it works in our[announcement blog post](https://depot.dev/blog/depot-ai) .


## Depot builds for pull requests from open-source forks


We rolled out a new authentication mechanism for workflows that run for untrusted` pull_request` events from open-source forks in GitHub Actions. We're running an[OIDC issuer](https://actions-public-oidc.depot.dev/) that receives claim requests from these workflows, validates their identity, and issues short-lived OIDC tokens describing their details.


With our own OIDC issuer, we can authenticate these workflows and route them to ephemeral build infrastructure, separate from your main layer cache. Allowing us to offer access to Depot build infrastructure for these external pull request workflows. Allowing you to get the speed of Depot builds even for pull requests from open-source forks.


Read our[full announcement blog post](https://depot.dev/blog/github-actions-oss-fork-builds) for all the details on why this limitation exists in GitHub Actions, how we are solving it, and how you can enable it for your projects.


## What's next?


We're already hard at work on Depot Drop Week #02, which we are tentatively planning for September/October. You can follow our[Drop Week](https://depot.dev/drop-week) landing page and subscribe to get notified of future launches.


We have a few things already in motion regarding what we're working on next. We are looking at ways to make builds more insightful and easier to debug. We're also looking at ways to make going from source code to a Docker image easier.


If you have things you'd like to see in Depot or want to chat about problems you're facing with your Docker image builds or any CI/CD problems, we'd love to hear from you! You can reach out to us on[Twitter](https://twitter.com/depotdev) or join our[Community Discord](https://discord.gg/MMPqYSgDCg) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
