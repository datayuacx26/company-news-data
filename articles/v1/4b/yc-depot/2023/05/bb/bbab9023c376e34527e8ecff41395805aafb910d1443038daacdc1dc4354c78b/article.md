---
schema_version: "1.0.0"
document_id: "bbab9023c376e34527e8ecff41395805aafb910d1443038daacdc1dc4354c78b"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/local-builds"
published_at: "2023-05-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:5ed6ab434f3cb4855622465e96e834fbeb538fe69a0608f3e72e940992408b72"
---

# Accelerated local builds, with instant shared cache

We are excited to launch a new feature today: **accelerated local Docker image builds** , with instant cache sharing across your entire team! This is available now for all Depot users.


This means that when you` depot build` an image from your local machine, the build runs on[Depot's accelerated compute](https://depot.dev/blog/turbo-builders) , and the resulting image is downloaded to your local machine.


But even better, **your project's build cache is shared with your entire team** . Anyone who has access to your project can instantly reuse any layers that were already built, so if your coworker already built the image, you can just download the result! This unlocks perfectly incremental builds, with cache hits even if you've never built the image before.


## How it works


The new local build experience is powered by a few key components:


1. A new transport layer that is aware of what layers already exist on your local machine and only transfers the diff between what you have and what changed during the build.
2. An acclerated process for generating the images themselves, including parallel gzipping of image layers and SIMD-optimized hash calculations.
3. Full support for multi-platform images, including the ability to` --load` the correct architecture for your local machine after build.


## Why we built this


Builds are slow — both in CI and locally.


CI builds are slow because they're often running on ephemeral machines, need to save and load the Docker layer cache over slow networks, and don't offer the full range of CPU architectures for multi-platform images. Depot is especially suited to speed up these CI builds, and we've seen Depot make builds over 40x faster in CI:


But building Docker images from your local machine is also slow. You are limited by your device's CPU and memory, your network connection, and the fact that you have to build the entire image from scratch. You also can't efficiently build multi-platform images locally, because you must emulate other architectures on your local machine.


Until now, you *could* use Depot to build images from your local machine, but you needed to download the entire image back to your local machine, for every build. This was functional, but very inefficient, with lots of wasted time redownloading the same layers over and over again.


With the new launch of accelerated local builds, Depot is now specifically optimized for building images from your local machine as well as from CI!


## How to get started


If you have the[Depot CLI installed](https://depot.dev/docs/cli/installation) already, you can build your image with` depot build` . If you're familiar with an equivalent` docker build` command, the Depot CLI accepts the same arguments:


```text
- docker build -t my-image .
+ depot build -t my-image .
```


That will execute the build using your remote Depot project, making use of any previous build cache that exists, whether previously built by you, your teammates, or your CI pipeline.


From there, you can decide to do one of three things with the resulting image:


1.


**Build the image and load it** , so you can run it on your local machine:


```text
- docker build -t my-image .
+ depot build -t my-image --load .
```


2.


**Build the image and push it** to a remote registry:


```text
- docker build -t my-image .
- docker push my-image
+ depot build -t my-image --push .
```


3.


**Just build the image** , leaving it in your remote cache:


```text
- docker build -t my-image .
+ depot build -t my-image .
```


See[our full local development guide](https://depot.dev/docs/container-builds/how-to-guides/local-development) for more information.


## Bonus features


Solving acclerated local builds unlocked a few other improvements that we're excited about:


-


You can` --push` and` --load` at the same time — previously Docker and Depot would only allow one or the other, now you can do both in a single command! This also extends to CI, so it's possible to build an image, push it to a registry and load it locally for integration testing all at once:


```text
-   uses  :   depot/build-push-action@v1
with  :
tags  :   ghcr.io/depot/app:tag
push  :   true
load  :   true
```


-


You can` --load` a multi-platform image, and Depot will automatically load the correct architecture for your local machine:


```text
$   depot   build   --load   --platform   linux/amd64,linux/arm64   -t   my-image   .
```


-


Loading an image with` --load` no longer produces a legacy Docker tarball of the whole image — previously loading required creating all the image layers, *then* making one large tarball of all layers, then sending the whole thing to the client, and finally unpacking the image into Docker's local store. Now, even if you don't have any of the layers locally, for instance in a CI pipeline, Depot can skip the tarball creation and instead transfer each layer directly to the client. ⚡


-


Multiple parts of the build pipeline are now parallelized, including the gzipping of image layers and the hashing of layer contents. This makes builds faster, especially for large images with many layers or images with very large layers. Depot can additionally push to multiple registries in parallel, with a single build command:


```text
$   depot   build   --push   .   \
-t   ghcr.io/depot/app:tag   \
-t   us-east1.pkg.dev/depot/app:tag
```


We also made this really cool visualization of shared layer cache for the homepage 😎


Dockerfile


$


depot build -t ghcr.io/org/app:tag .


# Build stage


•


FROM node:18.12.1-bullseye-slim AS frontend-build


•


WORKDIR /code


•


COPY package.json pnpm-lock.yaml ./


•


RUN corepack enable && pnpm install --frozen-lockfile --prod


•


COPY frontend/ frontend/


•


RUN pnpm build


# Package stage


•


FROM python:3.10.10-slim-bullseye


•


WORKDIR /code


•


USER posthog


•


RUN apt-get update && apt-get install -y --no-install-recommends chromium chromium-driver libpq-dev libxmlsec1 libxmlsec1-dev libxml2


•


COPY ./bin ./bin/


•


COPY posthog posthog/


•


COPY --from=frontend-build /code/frontend/dist /code/frontend/dist


•


CMD \["./bin/docker"\]


## Accelerating your local builds with Depot


If you want to speed up your Docker image builds locally and in CI,[sign up for an account](https://depot.dev/start) and start running builds with our 7-day free trial.


- Powerful remote build machines with datacenter network speeds
- No more rebuilding layers that your team already built
- No more waiting for your team to push their changes to CI, build an entire image, push that image to a registry, and then pull that image down to be used locally
- No longer is your Docker layer cache isolated to each developer machine.


We've brought fine-grained distributed caching to container image builds, accessible from CI and your team's local machines. Builds are perfectly incremental. You get cache hits even with changed code, and you can reuse layers from your team. You never have to start a build from scratch again.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
