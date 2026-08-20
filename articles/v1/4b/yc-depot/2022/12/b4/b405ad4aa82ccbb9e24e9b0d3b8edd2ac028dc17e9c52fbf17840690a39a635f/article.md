---
schema_version: "1.0.0"
document_id: "b405ad4aa82ccbb9e24e9b0d3b8edd2ac028dc17e9c52fbf17840690a39a635f"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/project-region-selection"
published_at: "2022-12-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:eed97be503c2886d8e3c10d361a3df2bb82288acf168de10bf3619b6906aeecf"
---

# New feature: Select the region for your Depot projects

Today, we are excited to announce the ability to choose the region for your Depot project when using our Premium Usage plan. This feature allows you to launch your image builders in the region closest to your registry by selecting your desired region when creating a new project.


For Depot-hosted projects, you can select between our` US East` and` EU Central` regions. Then, when a build is routed to that project via either our[depot build](https://depot.dev/docs/cli/reference/container-builds#depot-build) CLI or[depot/build-push-action](https://github.com/depot/build-push-action) in GitHub Actions, we will route the build to a builder in that region.


```text
$   depot   build   -t   12345678910.dkr.ecr.eu-central-1.amazonaws.com   --push   .
```


The` build` command above would have seen a bottleneck at the final step of the image build when the layers & manifest got pushed to the registry. This bottleneck was because, by default, the project was in` us-east-1` and had to push across the network over to` eu-central-1` .


Now, when the builder is in a region closest to your registry, pushing your resulting image becomes significantly quicker because there is far less network latency between our remote builder and your registry.


We currently allow you to choose between our` US East` and` EU Central` regions when creating a project or editing an existing project when you are on the Premium Usage plan. However, if you need a region outside the current ones we offer, pleasedrop us an email , and we will be happy to launch the one you need.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
