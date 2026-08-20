---
schema_version: "1.0.0"
document_id: "3ced133ff79461de1279463e3f7f86532540179e3ca7e782c6f29f1c494b17ac"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/why-you-should-avoid-copy-link-in-your-dockerfile"
published_at: "2025-03-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:2686085de517f6a626a83d26fa66d48dfe0111d31fd5f75411827808e58144f6"
---

# Why you should avoid COPY --link in your Dockerfile

At this point, we've run tens of millions of container image builds with Depot, and we've seen a lot of ways folks have tried to optimize their builds.


One of the most common things we see is folks using` COPY --link` in their Dockerfiles to try to speed up their builds.


The idea is that you can get better performance when your parent layer invalidates, because the` COPY --link` creates an independent layer that isn't dependent on the parent layer.


But, in our experience and observations,` COPY --link` is more likely to *increase* your build times than decrease them. Here's why.


## Hidden inefficiencies with` COPY --link`


To fully understand what` --link` is doing, you first have to take a step back and look at how Docker layers are produced & consumed with normal` COPY` . Let's look at a trivial example of a Dockerfile that copies a file from one directory to another:


```text
FROM   alpine
COPY   /dir1 /dir1
```


This example creates a relatively straightforward build graph for BuildKit to operate on. You can see the live LLB output in our[Dockerfile Explorer](https://depot.dev/dockerfile-explorer) :


We essentially get a basic build graph DAG where there are two nodes at the top: one for the build context, and one for the base image. The` COPY` instruction creates a new node that is dependent on the base image and the build context. This is a simple and efficient way to build a Docker image.


In this basic example, the` COPY` layer will need to be recomputed if anything in the build context that exists in the` dir1` directory changes, or if the base image changes. This is because in classic Docker layer caching, the` COPY` layer is dependent on both the build context and the base image.


Now, let's look at the same example but with` COPY --link` :


```text
FROM   alpine
COPY   --link /dir1 /dir1
```


If we look at the BuildKit LLB output for this Dockerfile, we see a different graph:


If you look closely, you'll see that the` COPY --link` layer is now dependent on the` dir1` directory in the build context, but not on the base image. This breaks the paradigm of classic Docker layer caching, where the` COPY` instruction is dependent on both the build context and the base image.


In this example, the layer that's created from the` --link` flag is not dependent on the base image. This means that if the base image changes, the` COPY --link` layer will not need to be recomputed. This is because the` COPY --link` instruction actually takes that command and builds it in isolation on top of` scratch` .


So this build graph is actually two build graphs that are computed in parallel. One for the base image:


```text
FROM   alpine
```


And one for the` COPY --link` layer:


```text
FROM   scratch
COPY   /dir1 /dir1
```


This is a clever way to try to optimize your Docker image builds, but it's actually more likely to increase your build times than decrease them.


Why? Because now *two* build graphs have to be created, a virtual stage from` scratch` has to be executed, and the two graphs have to then be merged back together at the end of the build process. This merging itself takes time, and taken all together, this can be less efficient than the simple` COPY` .


There are certainly edge cases where` COPY --link` can be useful, but in our experience, it's more likely to increase your build times than decrease them, because of performance issues with the way BuildKit handles` COPY --link` .


We've even seen bugs in the cache garbage collection algorithm inside of BuildKit, where it doesn't properly invalidate the cache when it should, believing that some of the cache artifacts from` COPY --link` are 0-byte in size, rather than their actual size on disk.


## Conclusion


The rationale for using` --link` isn't illogical: It makes sense to create a separate layer that is *not* dependent on the parent layer, so that if your parent layer invalidates, the` COPY --link` layer doesn't need to be recomputed. It's often talked about in terms of rebasing your images when base images are updated, because BuildKit has logic that can skip pushes and pulls of layers that are already present and reorder the layers so that the image manifest contains the new layers and old layers in the correct order.


But in practice, we've found this flag to be loaded with inefficiencies and bugs that can actually increase your build times. Even in the base image use case, we tend to see just doing full rebuilds with your traditional` COPY` are often faster than the` --link` equivalent for most things.


## Related posts


- [Docker multi-stage builds explained](https://depot.dev/blog/docker-multi-stage-builds)
- [Buildx bake deep dive: Bake all your images with one command](https://depot.dev/blog/buildx-bake-deep-dive)
- [How to speed up your Docker builds](https://depot.dev/blog/speed-up-docker-builds)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
