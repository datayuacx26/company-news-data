---
schema_version: "1.0.0"
document_id: "f0e79e3be57fa864a14da2cf61ec60164ae9f45e08d4e141e0d4a704b193441a"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-magic-explained"
published_at: "2024-01-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:26:19.240569+00:00"
content_hash: "sha256:7a89c83d52587a057d3121dee8dbe16c09649a68a4df9d194a1bcb4d0194b544"
---

# How Depot speeds up Docker builds

We go behind the scenes into Depot's architecture to explore exactly how it delivers fast, secure, multiplatform Docker builds locally and through CI platforms.


[Depot is a remote Docker image build service that runs cloud builders on nativel Intel & Arm with fast persistent layer caching on NVMe cache SSD. Making Docker image builds 2-40x faster. Try it out today with our 7-day free trial →](https://depot.dev/start)


Docker is the go-to method of working with and deploying containers. It allows you to package up applications and services with all of their dependencies and deploy them anywhere as a single, fully self-contained entity, quickly.


However, one aspect of using Docker can significantly slow down your software development process: building Docker images. Rebuilding Docker containers on a developer's laptop is fast due to caching, but doing the same in a continuous integration (CI) environment like GitHub Actions, CircleCI, or GitLab can be very slow, because the cache isn't available in every build or takes a long time to load and save layers.


We created Depot to solve the problem of slow Docker builds. Depot is a remote container build service that can build Docker images up to 40x faster locally and in CI environments. We offer the Depot CLI as a drop-in replacement for` docker build` anywhere you're building your Docker images today.


20x is a big number, and it sometimes raises eyebrows. That's why, in this article, we share the three major techniques that Depot uses to speed up Docker builds. If you want to understand exactly how Depot works under the hood (and why it's not magic!), then read on.


## Technique 1: Reducing time-to-start of builds with a standby pool


When trying to make builds faster, a good starting point is to optimize the time it takes for the build to start. You want your builds to start almost immediately, and that means avoiding any time penalties from having to wait to provision additional infrastructure.


### Problem


Depot uses AWS EC2 instances to run Docker image builds. However, it's too expensive to have machines waiting idly, so these instances need to be launched on demand, only when builds are triggered by Depot users.


The problem is that AWS EC2 instances can take anywhere from forty seconds to five minutes to boot, which is an unacceptable time-to-start for a build. This is an inherent feature of on-demand computing: the entire idea behind on-demand is that compute is spun up **only when needed** . But the spin-up takes time.


### Solution


There's no way of getting around this bootup time for an individual machine, so Depot maintains a standby pool of warm machines that have been configured to run Docker image builds. We maintain a fleet of optimized builders in a *stopped* state so that builds can immediately start without having to go through the[full lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html) to launch an on-demand instance.


When a new build is triggered, one of these warm machines is started. Transitioning from the *stopped* to *running* state only takes two to three seconds, as opposed to the alternative of forty seconds to five minutes for launching a new on-demand instance. This additional two to three seconds is trivial compared to the massive time savings from caching between builds.


We also recognize that if you just built an image, you are likely to build another one soon after. We optimize for this by keeping your machine active for a two-minute window after every build. Consider the case during development when you're constantly adjusting your code and running a new build to test your changes. In these situations, your build starts immediately as the machine stays online for that additional time window.


For a deeper dive into how we attacked the problem of reducing time-to-start and to see how our solution has evolved over time, check out our[Reducing time-to-start](https://depot.dev/blog/infrastructure-provisioner-v3) post.


## Technique 2: Persistent cache, shared between local and CI builds


After efficiently and automatically provisioning infrastructure so that the compute is there when you want to run your build, the next major problem to solve is to make the Docker build itself faster through caching.


### Problem


Docker builds are structured in[layers](https://docs.docker.com/build/guide/layers/) and the results of each layer are stored in the build cache. When a new Docker build runs, it first checks the build cache to see if a certain layer has already been built. If it has (cache hit), then Docker uses the pre-built layer, saving time. If a layer has changed (cache miss), Docker rebuilds the layer. Making the Docker build faster is almost entirely a function of maximizing cache hits for these layers between builds.


Since[BuildKit](https://docs.docker.com/build/buildkit/) became the default build engine for Docker, the inner workings of Docker builds have become even more sophisticated and efficient, bringing in deduplication and parallelization across builds. We cover these inner workings in our BuildKit in depth article, but for now we're just focusing on maximizing cache hits for as many layers as possible between builds.


Leveraging a persistent cache shared between builds and users allows subsequent builds to avoid duplication of work as much as possible, drastically increasing the speed of Docker builds.


Even though Docker natively supports caching in builds, there is one major problem with most real-world use cases for Docker: in order to fully take advantage of Docker's sophisticated caching mechanism, **the same cache must be available across builds** ! This includes supporting the same cache across as many users on your team as possible, your CI/CD pipelines, and the different architectures that you use. This is why we introduced the idea of sharing cache across builds via a centralized remote machine.


There are many added benefits of sharing cache between users and CI/CD pipelines. For example, if you have already built the image in your local testing environment before opening a pull request, your first CI build can take advantage of the cached artifacts from local development, dramatically speeding up CI builds.


Other coworkers can also build your branch much more quickly by leveraging the shared cache. In fact, their builds could be almost instantaneous if Docker skips every step in the build process. In Docker, if something has recently been built by someone on the team, nobody should have to build it from scratch again. Your team should only have to pay the cost of the cache miss once.


While there are clear benefits to this approach, provisioning infrastructure to make sure a persistent cache is always available throughout your organization without major network overheads is easier said than done.


### Solution


Depot's solution is to persist the cache on fast NVMe SSDs outside the AWS EC2 build instances. We run a[Ceph](https://ceph.io/en/) cluster — an open-source, distributed storage system — to persist Docker layer cache during a build and then reattach that cache across builds.


In Depot, there is the notion of a *project* , which is a combination of compute and cache for each supported architecture. Each *project* has two caches (one for x86 and one for ARM) and a maximum of two EC2 instances running at a time (one for each architecture). When a build is started, the persistent Ceph volume tied to a project is attached to that newly-started EC2 instance and the Docker build begins, taking advantage of the cache persisted from previous builds. Each instance is able to run multiple builds in parallel due to BuildKit's out-of-the-box capabilities.


We limit the storage space for each project through a BuildKit cache storage policy. In Depot, volumes are given 50 GB but you can configure them on a per project basis. You could have ten or twenty engineers sharing a single project. Just recognize that they would be sharing the same cache, which would likely increase cache misses if the storage isn't high enough. For this reason, we also allow you to add extra storage as needed for $0.20/GB/mo.


Ceph offers a lot of advantages like being able to persist cache across fast NVMe local storage (i.e. instance storage) while maintaining minimal network latency due to keeping the volume and builder in the same[Availability Zone](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/) .


We originally started Depot using EBS for volume storage and had a typical write throughput of around 140 MB/s. Then, after managing our own storage on AWS with Ceph, our throughput went all the way up to 900 MB/s, a more than 6x increase in write throughput from our original architecture. To see how our caching mechanism has evolved over time, check out our[caching architecture](https://depot.dev/blog/cache-v2-faster-builds) post.


## Technique 3: Eliminating emulation for multi-platform or ARM Docker images


Building Docker images for multiple architectures is becoming a standard practice with the introduction of new ARM machines like the latest generation of MacBooks. It's possible to build Docker images for different architectures, but you often have to leverage emulation to do it.


### Problem


A Docker build natively detects the architecture it is running on and will by default produce a Docker image for the host architecture it is running on. But often you want to build for multiple architectures or a different architecture than your host. So you can use the` --platform` flag to tell Docker that you'd like to build an image for a set of architectures (i.e.` --platform linux/amd64,linux/arm64` ).


When you ask for a Docker build that differs from the host architecture, like building an ARM image on an Intel host, the build must emulate the ARM architecture to build the image. However, the problem is that building with emulation is often **painfully slow** . This often causes builds that would take a few minutes on the native architecture to take **an hour or more** with emulation.


### Solution


Depot solves this problem by building Docker images on native CPUs for Intel and ARM, no emulation required. We do this by maintaining two different caches (one for x86 and one for ARM) and allowing two EC2 instances (one for x86 and one for ARM) to be spun up at the same time. If a multi-platform build is requested, the two builds can run concurrently — and natively — on two separate EC2 instances without the massive costs of emulation.


Depot automatically detects the architecture you trigger your build on and routes it to the appropriate EC2 instance behind the scenes. It also orchestrates and manages the concurrent builds. After the image build is finished, Docker then adds a tag to the image that identifies which architecture it is for. Later, if that image is pulled, Docker is smart enough to provide the correct image based on the architecture of the machine issuing the pull operation.


## An extra layer of security for your Docker builds


Although our main focus has been speed, Depot has strong security baked in as well. We have gone to great lengths to completely isolate each user's instance so that their environment is completely their own.


Because a Docker build needs root permissions, we draw the build isolation level at the EC2 instance. This means that your build is running in your own project's EC2 instance with no other Depot projects sharing that machine. When your build is finished, the machine is destroyed and never reused again. We also avoid noisy neighbor builds where another customer's build could hog compute resources, starving other builds.


This is the major benefit of single-tenant systems. For the reasons mentioned above, we made this explicit security decision to isolate at the level of EC2 and Ceph rather than use a Kubernetes cluster. We use a single Ceph cluster for all customers because it is far more cost-effective, but it is namespaced by customer and project, so there is no possibility of reading a cache for a different project.


## More Depot magic to come


So far, we've gone into exactly how Depot's architecture works to provide fast, secure cloud-based Docker builds. However, there are still many improvements we want to make.


We want to build on tools like our[Dockerfile Explorer](https://depot.dev/blog/dockerfile-explorer) to help users write Dockerfiles by automatically providing smart assistance based on the tips we covered in our[Docker Build Fundamentals](https://depot.dev/blog/docker-build-fundamentals) article. This will help cut down the learning curve of building Docker images and avoid the often excessive speed penalties from subtle mistakes when crafting Dockerfiles. For example, we could give suggestions to add more build steps (so there is more opportunity for caching) or re-order steps (if one step is forcing another to always be rebuilt).


## Use Depot to speed up your Docker builds


The Depot command line tool is a drop-in replacement for` docker build` . This means that you can replace` docker build` with` depot build` in all of your workflows and scripts, while still relying on the same functionality and intuition that you expect from Docker. Depot is compatible with all of the major CI/CD providers including GitHub Actions, CircleCI, GitLab CI and[many others](https://depot.dev/integrations) .


Depot makes your Docker image builds up to 40x faster by automatically persisting cache across builds on fast NVMe SSDs and building images on native CPUs. Our builders have 16 CPUs, 32 GB of memory and a persistent 50 GB NVMe cache disk (with options to increase storage up to 500 GB if needed).


To use Depot, you can[sign up for a 7-day free trial](https://depot.dev/pricing) on any plan that works for you.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
