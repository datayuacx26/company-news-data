---
schema_version: "1.0.0"
document_id: "57b3ac186afdd5cd0303dcacf1d3a9ac14f3381b3bf06df5facc6efaa82b3249"
company_key: "yc-warpbuild"
company: "WarpBuild"
source_id: "yc-warpbuild-news-import-6421ae0a6624"
canonical_url: "https://warpbuild.com/blog/docker-builders-arm64"
published_at: "2025-03-19T00:00:00+00:00"
first_seen_at: "2026-07-22T19:23:06.881853+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:c32d0cfd2d63d0db634abca59c4c0af6d54be54987fcd7541894c33554c2cc60"
---

# Cached Docker Builders - ARM64 support

WarpBuild recently announced support for new docker container builders combine high performance processors with directly attached SSDs to deliver the fastest docker builds in the world. Now, we are excited to announce ARM64 support for these builders to enable multi-arch builds.


## What does this mean?


This means that you can now build and push your docker images for` linux/amd64` and` linux/arm64` architectures from a single build.


## Caching


Similar to the x86_64 builders, the ARM64 builders support caching of the build context, base image, and dependencies. This means that you can now build and push your docker images for` linux/amd64` and` linux/arm64` architectures from a single build.


Caches are automatically managed by the builder and no additional configuration is required. Remove the` cache-from` and` cache-to` steps from your` build-push-action` step as the builder will handle it for you.


This leads to build times that are 2-3x faster than the x86_64 builders with emulation for` linux/arm64` builds.


## 🚀 Try it out


Create a builder profile in the WarpBuild dashboard and add the following to your GitHub actions workflow before your` build-push-action` step:


```text
-   name  :   Configure WarpBuild Docker Builders
uses  :   Warpbuilds/docker-configure@v1
with  :
profile-name  :   "super-fast-builder"


-   name  :   Build and push
uses  :   docker/build-push-action@v6
with  :
context  :   .
file  :   Dockerfile
platforms  :   linux/amd64,linux/arm64   # Provide the platforms you want to build for
push  :   true
tags  :   ${{ steps.docker_build.outputs.image }}
```


Try out WarpBuild's new docker builders


- [Documentation](https://www.warpbuild.com/docs/ci/docker-builders) .
- [Get started](https://app.warpbuild.com/) .


---
