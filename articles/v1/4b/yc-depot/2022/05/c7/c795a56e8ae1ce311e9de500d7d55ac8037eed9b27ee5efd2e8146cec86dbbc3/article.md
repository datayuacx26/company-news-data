---
schema_version: "1.0.0"
document_id: "c795a56e8ae1ce311e9de500d7d55ac8037eed9b27ee5efd2e8146cec86dbbc3"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/building-arm-containers"
published_at: "2022-05-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:0f8b787fb04038fbe7a66054808569b1cb0c4b823936388ac0bd049a0b4bd6d6"
---

# How to build Arm and multi-architecture containers today

This blog post dives into three different options for building Docker images for Arm. Each option comes with a set of tradeoffs that can increase build times and complexity. We recently wrote an updated post that dives into the pain points with[emulation during a Docker build for Arm](https://depot.dev/blog/docker-arm) .


Developers using Docker locally or in production face new challenges with the transition to Arm. They want to build images that will run natively on their M1 machine. Meanwhile, companies would like to provide images that can be built once and run anywhere (i.e., multi-architecture/multi-platform).


Existing methods for building Arm or multi-architecture images today have cumbersome challenges and trade-offs.


In this post, we will explore the existing options for building Arm and multi-architecture containers, as well as how we have simplified these options with Depot.


[Depot launches native BuildKit cloud builders for both Intel and Arm — we build Docker images on native CPUs, avoiding emulation entirely. If you're looking to build Docker images for Arm and want faster builds that don't rely on emulation. Try it out →](https://depot.dev/start)


## Current state of affairs


The modern engine backing Docker, BuildKit, can build an image for the architecture that matches your machine. If you run` docker buildx build` locally, BuildKit builds you an image that runs natively on your machine.


If you want to build for an architecture that does not match your machine's, you can leverage the` —-platform` flag to choose the target architecture:


```text
# build for a single architecture
docker   buildx   build   --platform=linux/arm64   .
```


By default, BuildKit chooses emulation to build the Docker image if the target architecture is different from your machine. BuildKit uses the QEMU project to emulate various target architectures when necessary.


BuildKit also supports multi-platform builds, where you can build a single image manifest that contains two or more images inside, targeting different architectures. This is specified on the CLI similar to the above, but with multiple architectures in a list:


```text
# build for multiple architectures
docker   buildx   build   --platform=linux/amd64,linux/arm64   .
```


Behind the scenes, the image is built once for each architecture, and the results are merged into a single image manifest. Again, by default BuildKit builds the native platform without emulation and the other with it.


Emulation can be *very* slow. So BuildKit also supports offloading the build to remote builder instances via an SSH connection to another Docker instance or connecting to a pod running in Kubernetes.


*Note: BuildKit also supports launching a containerized version of itself on the local host, though not entirely useful for our cross-platform use case.*


By passing a CLI flag, you can build images for Intel or Arm CPUs, or both, regardless of your current machine's native architecture. But each approach comes with tradeoffs around build performance and infrastructure complexity.


Let's look at those approaches.


## Option #1: Slow emulation


If a build asks for an architecture different from the native host's architecture, BuildKit uses its copy of QEMU to emulate the target CPU. This option requires no changes to the Dockerfile.


Let's look at what is happening with our multi-architecture example from above.


```text
docker   build   --platform=linux/arm64,linux/amd64   .
```


If you're running this build on a common CI provider, you're likely building on an x86 CPU. We also know from earlier that the image is built for each platform and then the result is merged. In this scenario, the` linux/amd64` image is built natively, and the` linux/arm64` image is built using emulation.


This is functional but can be painfully slow. Emulation slows down builds greatly depending on what's inside the Dockerfile, sometimes even by an order of magnitude or more. A reasonable 4-minute build could take 40+ minutes with emulation!


This speed reduction is expected and unavoidable considering what the emulator must do. All machine instructions from the target architecture to the host architecture must be translated back and forth, introducing latency to every machine instruction. So, it works, but it will never be fast.


### Alternative: Cross-compilation


One way to avoid emulation entirely is by using one of the more advanced Dockerfile features: cross-compilation. It is mostly relevant for packaging compiled languages like Go, Rust, C/C++, etc.


Cross-compilation builds on top of[multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/) and works by one stage compiling your code for the target architecture, and the other configures the runtime to be exported to the final image.


This approach requires modifying your Dockerfile to use directives like` FROM --platform=$BUILDPLATFORM` and` FROM --platform=$TARGETPLATFORM` . The former tells BuildKit to run on the native host architecture, and the latter is emulated using the build target architecture.


With this, you can minimize the amount of emulation needed for the build to just the final stage. Which, hopefully, just has to copy artifacts into the image by cross-compiling your code in a previous stage.


For example, here is what a Rust cross-compilation` Dockerfile` may look like:


```text
# Base builder ---------------------------


FROM   --platform=$BUILDPLATFORM rust:1.61   AS   rust-builder
RUN   apt-get update && apt-get install -y \
g++-x86-64-linux-gnu libc6-dev-amd64-cross \
g++-aarch64-linux-gnu libc6-dev-arm64-cross && \
rm -rf /var/lib/apt/lists/*
RUN   rustup target add \
x86_64-unknown-linux-gnu aarch64-unknown-linux-gnu
RUN   rustup toolchain install \
stable-x86_64-unknown-linux-gnu stable-aarch64-unknown-linux-gnu
RUN   rustup component add rustfmt
ENV   CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_LINKER=x86_64-linux-gnu-gcc \
CC_x86_64_unknown_linux_gnu=x86_64-linux-gnu-gcc \
CXX_x86_64_unknown_linux_gnu=x86_64-linux-gnu-g++ \
CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_LINKER=aarch64-linux-gnu-gcc \
CC_aarch64_unknown_linux_gnu=aarch64-linux-gnu-gcc \
CXX_aarch64_unknown_linux_gnu=aarch64-linux-gnu-g++ \
CARGO_INCREMENTAL=0


# amd64 build ----------------------------


FROM   --platform=$BUILDPLATFORM rust-builder   AS   build-amd64
WORKDIR   /app
COPY   . .
RUN   cargo install --target x86_64-unknown-linux-gnu --path .
RUN   mv ./target/x86_64-unknown-linux-gnu/release/example-app /usr/bin/example-app


# arm64 build ----------------------------


FROM   --platform=$BUILDPLATFORM rust-builder   AS   build-arm64
WORKDIR   /app
COPY   . .
RUN   cargo install --target aarch64-unknown-linux-gnu --path .
RUN   mv ./target/aarch64-unknown-linux-gnu/release/example-app /usr/bin/example-app


# Final arch images ----------------------


FROM   --platform=amd64 debian:bullseye   AS   final-amd64
COPY   --from=build-amd64 /usr/bin/example-app /usr/bin/example-app


FROM   --platform=arm64 debian:bullseye   AS   final-arm64
COPY   --from=build-arm64 /usr/bin/example-app /usr/bin/example-app


# Final image ----------------------------


FROM   final-${TARGETARCH}
```


By building the application using the` x86_64` and` aarch64` cross-compilation toolchains, we can ensure that the actual compile step (the slowest) executes using the host's native architecture, then the result is packaged into a container for the target architecture.


Maintaining cross-compilation toolchains can be tedious. This approach requires carefully modifying your Dockerfile to support multiple architectures. It also doesn't help if part of the build *needs* to run in the final stage. For instance, if you need to` apt-get install` some operating system packages, those steps will always require emulation.


Cross-compiling isn't necessary, as we explore below.


## Option #2: CI providers with native Arm support


Images built on the same host architecture as the target architecture are fast; when they differ, slow emulation is necessary. So, you might turn to a hosted CI provider to supply access to machines of varying architectures, so all builds can be native and fast.


**Unfortunately, most common CI providers don't offer Arm runners** , and the ones that do, don't integrate with multi-platform builds.


At the time we're writing this, only CircleCI offers the ability to run jobs on Arm. GitHub Actions, Google Cloud Build, and GitLab hosted CI do not offer hosted Arm machines. Travis CI is trialing beta Arm runners for open-source projects.


Even if you do happen to use CircleCI and have the ability to route your jobs to Arm instances, multi-platform builds are still a challenge. Even if the build is on Arm, the Intel image would need to be built with emulation.


What you really need for multi-platform builds is an active connection to two builder instances. One Arm and one Intel, so you can take advantage of` docker buildx` 's ability to route build jobs to the correct host for the given target.


## Option #3: Running your own builder instances


Emulation is slow but can be avoided by routing builds to builder instances that match the target architecture. While common CI providers don't offer this ability, it's possible to do it yourself.


Docker Buildx supports connecting to remote instances over SSH or via Kubernetes. So, you could launch two EC2 instances in AWS, one with Intel CPUs and one with Arm. You would install Docker on those two instances and set up SSH access from your local workstation. Then register both VMs with a named Buildx build instance, using the` --append` flag to merge the two builders into a single multi-platform builder instance:


```text
docker   buildx   create   --name   multi   \
--driver   docker-container
--platform   linux/arm64
ssh://something@arm-instance


docker   buildx   create   --append   --name   multi   \
--driver   docker-container
--platform   linux/amd64
ssh://something@intel-instance


docker   buildx   use   multi
```


Now that your two VMs are registered with BuildKit and your local machine is configured to use the named builder, you can build a multi-platform image without emulation!


```text
docker   build   --platform=linux/arm64,linux/amd64   .
```


This configuration is the fastest in raw build speed but comes with additional complexities:


- You need to run and pay for your own cloud VMs
- You'll have to maintain those VMs, ensuring proper access controls and firewalls are in place, OS packages are updated, etc.
- This works for you and your machine. But if you want to share the builders with multiple teammates or with your existing CI provider, you will need to manage or automate SSH keys or SSH certificate infrastructure


## Fully managed native builders with Depot


With Depot, we wanted access to native build instances for both Arm and Linux, and we wanted to avoid all the complexity associated with running your own builder VMs.


Depot is a remote Docker build service that manages a fleet of builder machines for both architectures. Machines have higher specs than traditional CI providers and launch with a persistent SSD cache.


Our CLI,` depot build` , functions exactly like` docker buildx build` but is integrated with the hosted service. Builds are executed remotely on the remote builder instances, choosing an instance to match the target build platform. If you request a multi-platform build, two instances perform the build in parallel. All builds are native all the time!


```text
# replacing `docker buildx build` with `depot build`:


depot   build   -t   repo/image:tag   .   \
--platform   linux/arm64,linux/amd64
```


Depot offers a fully managed version of option 3 above. We manage all access control, automation, caching, maintenance, etc. of the builder instances. We run the latest version of BuildKit on each builder instance and` depot build` embeds the relevant BuildKit libraries to communicate with the builder instances using your Depot access token.


You can use` depot build` on your local machine — by default you get a build matching your host architecture — but you can also run it from your existing CI provider. Since our builder instances also manage a persistent SSD cache for build context, builds are often many times faster in CI than a generic provider can deliver natively.


Depot's builders require zero configuration on your part! Depot is the only hosted CI provider that offers native Docker builds for both Linux and Arm, with multi-architecture support.


See our[quickstart guide](https://depot.dev/docs/container-builds/quickstart) for more information on getting started.


Our philosophy with Depot is to remove as many complexities as possible from the container image build process. We built Depot's hosted builders and multi-platform support as the service we wished existed and are excited to share it with you!


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
