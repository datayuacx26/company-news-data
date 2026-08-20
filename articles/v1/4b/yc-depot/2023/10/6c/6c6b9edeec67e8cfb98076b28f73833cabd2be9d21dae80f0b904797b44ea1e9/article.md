---
schema_version: "1.0.0"
document_id: "6c6b9edeec67e8cfb98076b28f73833cabd2be9d21dae80f0b904797b44ea1e9"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/docker-arm"
published_at: "2023-10-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:ec0eadde5981396cb139a392184f09d0c9a340e997bef854146395689de4ed32"
---

# Faster Docker builds for Arm without emulation

Building a Docker Arm image is loaded with inefficiencies. With the adoption of Arm-based devices like M1 / M2 MacBooks, and the growing popularity of Arm-based servers like AWS Graviton, it is becoming more important to build Arm and multi-platform containers. It can be a challenge to build these containers efficiently.


[Depot launches native BuildKit cloud builders for both Intel and Arm — we build Docker images on native CPUs, avoiding emulation entirely. If you're looking to build Docker images for Arm and want faster builds that don't rely on emulation. Try it out →](https://depot.dev/start)


## Emulation is painfully slow


Today, most people build Arm images using emulation. Why? Because emulation is built into Docker and` buildx` out of the box. By passing the` --platform linux/arm64` flag to` docker buildx build` , Docker will use emulation to build the image for Arm if the host architecture is Intel.


```text
docker   buildx   build   --platform   linux/arm64   -t   org/repo:tag   .
```


Or, to build an image for multiple architectures, also know as a multi-platform image, you can pass multiple platforms. Here we tell it to build an image for both Intel & Arm in parallel:


```text
docker   buildx   build   --platform   linux/arm64,linux/amd64   -t   org/repo:tag   .
```


### Multi-arch images with Docker` --load` has gotchas


Another quirk to building multi-platform Docker images with` buildx` is that importing a multi-platform image into Docker with` --load` will fail with a cryptic error:


> docker exporter does not currently support exporting manifest lists


This fails because the Docker daemon doesn't understand how to load multi-platform images out of the box. It receives a manifest list back but doesn't know how to handle it to select the correct architecture to load into the clients' daemon.


With Depot, we eliminate this problem with our faster` --load` implementation that knows how to handle the manifests correctly and import the correct image architecture into the daemon. You can read more about the implementation of it in our[blog post on accelerated local builds](https://depot.dev/blog/local-builds) .


### Side note on multi-platform images


Building multi-architecture Docker images like the example above results in one half of the build happening on the native host platform and the other half happening in an emulated platform. But multiple container images aren't produced. It's one image that contains a image manifest that states which platforms this Docker container image can run on. You can use tools like` docker buildx imagetools` or` docker manifest` to actually inspect these manifest (note:` docker manifest` is still an experimental feature).


So if you were to` docker run --rm org/repo:tag` and you were on an Arm server, the daemon will ask the Docker registry for the image manifest and select the image with a matching platform to use for the launched container.


Emulation is a logical place to start as that is what Docker Desktop supports out of the box when installing Docker. But it's slow, really slow, and it gets exponentially worse for more complex applications:


1.


[Mastodon's emulated builds](https://depot.dev/benchmark/mastodon) take around 55 minutes to complete.


2.


[Temporal's emulated builds](https://depot.dev/benchmark/temporal) take as many as 80 minutes to complete!


The benchmarks shown above are happening in GitHub Actions with Intel runners and asking for multi-platform images. So, when we need to build the Arm image (` linux/arm64` ), we have to use emulation during the Docker build of that architecture.


## Building Docker Arm images natively


You can use other tricks like[cross-compilation](https://depot.dev/blog/building-arm-containers#alternative-cross-compilation) in your Dockerfile to try and work around the slowness of emulation. But it's not a great experience. You have to get crafty with multi-stage builds and maintain cross-compilation toolchains.


The better option is to build Docker images for Arm natively by running the builds on real Arm hardware.


Unfortunately, this isn't a great experience if you're trying to do it yourself. You must[run your own builder instances](https://depot.dev/blog/building-arm-containers#option-3-running-your-own-builder-instances) , maintain them, keep them up to date, and ensure they are always available.


## The fastest way to build Docker images for Arm


With[Depot](https://depot.dev/start) , you get native Intel & Arm builders right out of the box. No emulation, no complicated cross-compilation, and no running your own builders. Just fast builds on native hardware.


It's as simple as installing our[depot CLI](https://depot.dev/docs/cli/installation) and running our[configure-docker](https://depot.dev/docs/cli/reference/container-builds#depot-configure-docker) command:


```text
depot   configure-docker
docker   buildx   build   --platform   linux/amd64,linux/arm64   -t   org/repo:tag   .
[+] Building 0.9s (  32/32  ) FINISHED                                                                                                                                                     docker-container:depot_456
=  > [depot] build: https://depot.dev/orgs/123/projects/456/builds/dw0n0x4b4g                                                                                                          0.0s
=  > [depot] build: https://depot.dev/orgs/123/projects/456/builds/ttcb3q4ss5                                                                                                          0.0s
=  > [depot] launching arm64 machine                                                                                                                                                                                                 0.4s
=  > [depot] launching amd64 machine                                                                                                                                                                                                 0.3s
=  > [depot] connecting to arm64 machine                                                                                                                                                                                             0.1s
=  > [depot] connecting to amd64 machine
=  > [internal] load .dockerignore                                                                                                                                                                                                   0.1s
=  > =  >   transferring   context:   116B                                                                                                                                                                                                     0.1s
=  > [internal] load build definition from Dockerfile                                                                                                                                                                                0.1s
=  > =  >   transferring   dockerfile:   435B                                                                                                                                                                                                  0.1s
=  > [linux/amd64   internal]   load   metadata   for   docker.io/library/node:16-alpine                                                                                                                                                         0.5s
=  > [linux/arm64   internal]   load   metadata   for   docker.io/library/node:16-alpine
```


With a Depot project configured, you can now build native multi-platform Docker images for Arm without the pain of emulation, cross-compilation, or running your own builders.


The results of building Docker images for Arm with Depot speak for themselves:


1.


The[Mastodon benchmark](https://depot.dev/benchmark/mastodon) went from **55 minutes** with emulation, down to **3 minutes** with native CPUs.


2.


The[Temporal benchmark](https://depot.dev/benchmark/temporal) went from **80 minutes** with emulation, down to **2 minutes** with native CPUs.


## Try it out


Depot launches on-demand builders for both Intel & Arm with 16 CPUs, 32GB of memory, and up to 500GB of persistent cache storage that is shared across all your builds and teammates.


If you're looking for the fastest way to build Docker images for Arm,[sign up for Depot](https://depot.dev/start) and try it yourself.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
