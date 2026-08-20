---
schema_version: "1.0.0"
document_id: "1c162a9e61b74c621a3bf432ae36d57f22f506d5dd1ef1f9d733f5fc09b0d54f"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-api"
published_at: "2023-06-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:42a2fcbc3d0ed8b975e37641cb4af4145399452d0cbff6eb810ac108bc86e9c4"
---

# Now available: Depot API

We're excited to announce that our public API for building Docker images from your own code is now generally available to all Depot customers 🎉


The API powers all of our builds via the[depot CLI](https://depot.dev/docs/cli/reference) , and it's now available to all organizations to use in their own applications & services. With it, you can manage namespaces (i.e., projects), launch builds, and get access to our lower-level BuildKit connections for advanced use cases.


To get started with the API, you can generate an[Organization API Token](https://depot.dev/docs/api/authentication#generating-an-api-token) and start issuing requests from our[Node SDK](https://github.com/depot/sdk-node) .


## Why did we build an API?


Since our launch, many folks have asked us if they can use Depot to build images on behalf of their users. For example, a SaaS service that takes machine learning models and packages them into Docker images to deploy them to Kubernetes clusters. They want the fastest way to build those container images without running all the internal infrastructure to do so.


So, we built an API to make it easy to build Docker images from your code. You can use it to build images for your applications or on behalf of your users.


## What can you do with the API?


The gRPC API consists of three primary services:


1.


**Namespace** : Create, update, and delete namespaces (i.e., projects) in Depot. For folks building images on behalf of their users, you can create a namespace for each user and then launch builds on their behalf. These are isolated from other projects and get their own dedicated cache.


2.


**Build** : Launch a single image build within a given namespace. You can use the build ID & build token to shell out to[depot build](https://depot.dev/docs/cli/reference/container-builds#depot-build) or get a lower-level BuildKit endpoint.


3.


**BuildKit** : Get a lower-level BuildKit connection for advanced use cases that don't require the Depot CLI. This allows you to create a build for a given architecture, get the endpoint, and feed it directly into[buildx](https://pkg.go.dev/github.com/docker/buildx) yourself.


These three services allow you to build images for your applications or on behalf of your users. You can create and destroy namespaces, launch builds, and run them via the` depot` CLI, or get a BuildKit connection to use in your applications with` buildx` .


You can find more details about the API in our[overview documentation](https://depot.dev/docs/api/overview) .


We are excited to see what use cases this unlocks and to iterate on new features & enhancements to our API to make your life easier. If you have any feedback or suggestions, pleasereach out to us and let us know.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
