---
schema_version: "1.0.0"
document_id: "04c1a76f13b5f8ec5897f5e2ac59ebeebf391bbb1b8865daf0a1b7b6168e770c"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/how-to-use-depot-in-monorepos"
published_at: "2023-11-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:8ca35226a34d07607fd3dbd6efda552416eb94a04834b15f24b7aacc5d036e40"
---

# How to use Depot to build Docker images in your monorepo

A common question we see as folks want to get faster Docker image builds by leveraging Depot is how to approach monorepos. Should I have multiple projects? Can I use one project? What happens to caching? These are all common questions.


In this guide, we will dive into our recommendations for leveraging Depot in your monorepo workflows to get effective caching and fast builds.


## What is a Depot project?


We cover the basics of a project in our[container builds overview](https://depot.dev/docs/container-builds/overview#projects) .


A project is a cache namespace with builders. If you're building a single Docker image, you can think of a project as being the Docker layer cache for each architecture of that image stored on an SSD. A project also has a BuildKit builder for each architecture you're building (i.e., Intel & Arm).


## How to use Depot in your monorepo


There are two options for monorepos that you can use depending on what you're building.


### Option 1: One project for the entire monorepo


The most straightforward approach is to have a single project for the entire monorepo.


A single project allows you to have a single project token or OIDC trust relationship that you can use across all of your builds. Additionally, if you have similar dependencies across your different Docker images, you can get incremental cache hits across all your builds for things like base images, etc.


The downside to this approach is that, depending on your builds, you can overwhelm the builder. You have 16 CPUs and 32GB of memory for each builder machine. So if you have 5 Docker images in your monorepo and each uses 8GB of memory during the build, you can have memory problems if you're building all 5 simultaneously.


BuildKit is generally good about managing memory and deduplicating work, but if you know you have a particular Docker image that is more cumbersome to build, create a separate project for it.


### Option 2: One project for each Docker image


The second option is to create a project for each Docker image in your monorepo. This is a good option if you have a Docker image that is particularly large or cumbersome to build. This allows you to have a dedicated cache and builders for that image that aren't competing for resources with other builds. It also has the additional benefit of defining a cache storage policy on a per-image basis.


The downside to this option is that you will have to use multiple project tokens in your CI environment to authenticate your different Docker image builds to their respective projects.


## Conclusion


The best approach when building Docker images in a monorepo while using Depot is a single project. It's simpler from an authentication perspective and gives you performance advantages for images that share dependencies.


When that isn't an option because you have multiple concurrent builds that can hog resources, separate projects per Docker image build also work.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
