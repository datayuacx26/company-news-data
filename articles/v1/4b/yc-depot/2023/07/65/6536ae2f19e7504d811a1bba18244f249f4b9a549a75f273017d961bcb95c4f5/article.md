---
schema_version: "1.0.0"
document_id: "6536ae2f19e7504d811a1bba18244f249f4b9a549a75f273017d961bcb95c4f5"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-ai"
published_at: "2023-07-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:6b98878a5ad62131a3e74c86e68c14ffdb2ac4eb06b7afb3aaf403c05468d672"
---

# Depot AI: A free open-source Docker registry for public AI models

We are excited to announce[Depot AI](https://depot.ai/) , a free, open-source Docker registry for public machine learning models that makes it easy to add any of the Top 100 models on Hugging Face to your own` Dockerfile` with a single` COPY` statement now in public beta.


[Following Cache v2, this is the second of three announcements for Drop Week #01](https://depot.dev/drop-week)


## Why Depot AI?


Packaging machine-learning models into Docker images is cumbersome. Many of our users are using Depot to package generative AI models like[Stable Diffusion](https://huggingface.co/runwayml/stable-diffusion-v1-5) ,[CLIP](https://huggingface.co/openai/clip-vit-base-patch32) , or[DALL-E](https://huggingface.co/openai/DALL-E) , into Docker images to serve them in production.


One common way to add a model to a Dockerfile is to clone it from Hugging Face via Git LFS:


```text
FROM   python:3.10


RUN   apt-get update && apt-get install -y git-lfs
RUN   git lfs install
RUN   git lfs clone https://huggingface.co/runwayml/stable-diffusion-v1-5
```


This build takes several minutes to complete, as the` stable-diffusion-v1-5` model repository is over 75 GB in size. But there are a few problems with this approach:


1. The model content must be downloaded with` git` , then transformed into a Docker image layer, and simply due to the size of the model, this takes a long time.
2. If any Dockerfile steps above the` git lfs clone` step change, the entire repository must be cloned again, even if the model itself hasn't changed. This is slow, and consumes another 75 GB of disk space.
3. The` git lfs clone` clones all the repository files, but you may only need a few files from the repository. Much of the time and disk space spent cloning the repository is wasted.


All this results in very slow Docker image builds that are especially painful as you iterate on your code and rebuild.


"Building Docker images for serving AI models is time-consuming. One approach to speed up the process is to store the model in the image instead of downloading it at runtime, reducing variability in cold start times caused by network connections.


But, packaging models during the build stage requires more compute power and results in long-running builds even when the model itself hasn't changed. Depot's solution addresses these issues by caching the model, enabling faster image builds and iterations, reduced cold-starts, and lower costs."


[— Aman Raghuvanshi, Co-Founder at Pyq](https://www.pyqai.com/)


## What is Depot AI?


[depot.ai](https://depot.ai/) is a free,[open-source](https://github.com/depot/depot.ai) Docker registry for the[top 100 models](https://huggingface.co/models?sort=downloads) on Hugging Face, with each model available as a Docker image. Both the registry and the images themselves are highly optimized to address the problems above.


Using[depot.ai](https://depot.ai/) , the` stable-diffusion-v1-5` model at` depot.ai/runwayml/stable-diffusion-v1-5` can be added to your image with a single` COPY` command:


```text
FROM   python:3.10


# Copy all files from the model repo to the current WORKDIR
COPY   --link --from=depot.ai/runwayml/stable-diffusion-v1-5 / .
```


Additionally, you can copy spcific files from the model repository into your image, rather than copying the entire repository:


```text
FROM   python:3.10


# COPY specific files from the model repo to the current WORKDIR
COPY   --link --from=depot.ai/runwayml/stable-diffusion-v1-5 /v1-inference.yaml .
COPY   --link --from=depot.ai/runwayml/stable-diffusion-v1-5 /v1-5-pruned.ckpt .
COPY   --link --from=depot.ai/runwayml/stable-diffusion-v1-5 /feature_extractor .
```


## How it works


We use a Depot project to build an image for each model, store the image contents in a[Cloudflare R2](https://www.cloudflare.com/products/r2/) bucket, and serve the images as a Docker registry using[Cloudflare Workers](https://www.cloudflare.com/products/workers/) . All of this is open-source, and you can[view the source code](https://github.com/depot/depot.ai) and can even[open a PR](https://github.com/depot/depot.ai#add-a-model) to add another model to the registry.


### Stable model content layers


The model content layer in the image does not change unless the model itself changes.[depot.ai](https://depot.ai/) builds each image using a variety of[reproducible build](https://github.com/moby/buildkit/blob/master/docs/build-repro.md) techniques that create an image where the model content layer is stable, only depending on the model content. The benefits of this are:


-


Once the build machine downloads the image layer once, it is cached. The layer that` COPY` creates in your own image is also stable, so it too is cached across builds.


-


When uploading your image to a registry, the model content layer is pushed once, then re-used for all subsequent pushes.


-


When your image is pulled from the registry in production, the model content layer is pulled once. If multiple containers run on the same host, that single layer pull is shared across all containers.


This makes a big difference when your Dockerfile or code changes between builds. For instance, with a Dockerfile like this:


```text
FROM   python:3.10


COPY   src/ .
COPY   --link --from=depot.ai/runwayml/stable-diffusion-v1-5 / .
```


When source code changes, the` COPY src/ .` step would be a cache miss, so all following steps would need to re-build.


Previously with` RUN git lfs clone ...` , that` RUN` would re-build, re-download the model, and make another copy of that model on disk and in your registry.


But with` COPY` , the logic is different. BuildKit will check to see if the layer already exists in the cache, and if it's unchanged, it will be re-used.


### Optimized for Docker image builds


Each model image contains a file index, using[eStargz](https://github.com/containerd/stargz-snapshotter/blob/main/docs/estargz.md) , that tells BuildKit what model files are included in the image and where they are stored in the layer.


Then, if eStargz support is enabled in your BuildKit builder and you request a specific file in the` COPY` , BuildKit can intelligently download only the files you request, saving time, cache disk space, registry storage, and bandwidth:


```text
FROM   python:3.10


# Only downloads the v1-inference.yaml file (1.87 kB)
```


If you build images with Depot, **we have enabled automatic support for eStargz for all projects.** You don't need to do anything to efficiently build images with only the files you need.


If you're not using Depot, you can read more about[enabling eStargz](https://github.com/containerd/stargz-snapshotter/tree/v0.13.0#building-estargz-images-using-buildkit) with a buildx instance.


## Conclusion


We're excited about making it easier and faster to build Docker images that leverage AI models.


If you're interested in learning more about Depot AI, check out our[GitHub repository](https://github.com/depot/depot.ai) and hop into our[Community Discord](https://discord.gg/MMPqYSgDCg) to ask us any questions. We're excited to develop this project in the open with the community during our public beta phase.


If you're building Docker images that need to add generative AI models,[sign up for a Depot account](https://depot.dev/start) to try things out.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
