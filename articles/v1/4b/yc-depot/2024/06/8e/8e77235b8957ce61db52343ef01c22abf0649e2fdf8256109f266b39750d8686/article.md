---
schema_version: "1.0.0"
document_id: "8e77235b8957ce61db52343ef01c22abf0649e2fdf8256109f266b39750d8686"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/best-ci-for-docker"
published_at: "2024-06-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:bded965987cd877258c499045d8dcb1007ab3f043dee138310a3d1989a63e4e3"
---

# The best CI provider for fast Docker builds

There are many continuous integration (CI) tools that can be used to build and deploy Docker containers, but which is the *best* CI provider for Docker builds? To answer this, we look at a few different factors, speed being the most obvious. Speed is affected by the Docker cache size limit, what kind of storage is backing the cache, and whether the provider is limiting any Docker features that would be required for fast builds. We also consider which CI tools offer support for multiple architectures such as Intel and ARM.


As Depot integrates with all CI providers, we have extensive experience with all of them, so we're intimately familiar with the strengths and limitations of each platform. We explain which CI providers are the best for fast Docker builds in our experience, and cover how to shave large amounts of your time off your Docker builds no matter which platform you're using.


[Use Depot with any CI provider to speed up your Docker builds by up to 40x. Depot solves CI caching issues by running your Docker builds on fast, low-latency machines, with instant caching and native builders for Intel and ARM. Try free for 7 days. To get started, create a Depot account →](https://depot.dev/start)


## Comparison of the top CI providers for Docker


While most CI providers are *able* to build Docker images, few can build them *at reasonable speed* . The time it takes to build a Docker image is a significant factor for an engineering team's productivity. If building an image for production takes 10 minutes, a developer can wait for the build to finish and then continue with their task. If building an image takes 1.5 hours, however, the developer has to go do something else during that time, and then remember to come back to the image once it's ready.


Let's discuss at what level each CI provider supports Docker and the functionality required to make Docker image builds fast.


### GitHub Actions


GitHub Actions fully[supports](https://github.com/marketplace/actions/build-and-push-docker-images) BuildKit, which is critical for fast and efficient Docker builds in CI.


Another key feature for fast Docker builds is cache speed and size. Here GitHub Actions performs less well. The fastest way to use GitHub Actions for Docker builds is to use the[GitHub Actions cache exporter](https://docs.docker.com/build/cache/backends/gha/) , so that the Docker layer cache can persist between builds, as otherwise all builders are ephemeral. However, the cache on GitHub Actions has quite a few limitations for Docker builds:


**Slow to transfer cache over network** : The cache is persisted via the[GitHub caching API](https://depot.dev/blog/github-actions-cache) , which is backed by Azure Blob Storage. The bandwidth between the runner's network and the cache storage is limited, so it's slow to save and load the cache over the network. This negates a lot of the performance benefits that exporting the cache promised.


**Cache size limited to 10 GB:** The GitHub cache storage is hard-limited to 10 GB per repo. Once the cache reaches 10 GB in size, less-recently-used cache items get removed in order to make space for the newer cache items. In production projects with multiple large layers, this means that GitHub might remove the layers that your other layers depend on in the cache, resulting in needing to rebuild all layers from scratch, negating all of the performance benefits of caching.


**Multi-platform builds use emulation:** GitHub Actions runners run on one architecture, such as Intel or ARM. Building a Docker image for the other architecture, such as during a multi-platform build, requires[emulation](https://depot.dev/blog/multi-platform-docker-images-in-github-actions) , which is very slow.


### CircleCI


CircleCI supports BuildKit on its[machine executor](https://circleci.com/docs/executor-intro/) , which provides ephemeral Linux VMs for your builds. For building Docker images, this is the executor that you would normally use on CircleCI.


(There is also an option to use the` docker` executor, which runs CI builds *inside* of a Docker container, but this is not a good option for building Docker images because the build doesn't get full access to the Docker daemon.)


As each CircleCI build runs in an ephemeral environment, you will need to persist the Docker layer cache between builds for optimal performance. The most performant option is to use CircleCI's[built-in Docker layer caching system](https://circleci.com/docs/docker-layer-caching/) , which automatically persists Docker volumes between builds. You can enable Docker layer caching using the` docker_layer_caching` key in your CircleCI config. Keep in mind that using Docker layer caching costs extra.


CircleCI's caching system does not use object storage but rather uses volumes, therefore it's faster than the systems on other providers that rely on object storage. CircleCI does not mention a cache size limit in the documentation, but this limit is larger than the 10 GB GitHub Actions offers and is likely around the 50 GB mark per project.


**Multi-platform builds require emulation** : CircleCI offers various[sizes](https://circleci.com/docs/configuration-reference/#linuxvm-execution-environment) of Linux VMs both on Intel and ARM, so it's possible to use a larger machine to give your build more resources. However, jobs run on one architecture at one time, so if you want to use multi-platform builds, you will need to rely on emulation to build for the other platform, which is slow. In addition, manual configuration is[required](https://circleci.com/docs/docker-layer-caching/#buildx-builder-instances) for buildx to work properly with Docker layer caching on CircleCI.


Overall, CircleCI offers more features that support faster Docker image build times, but multi-platform builds are slow due to emulation and require configuring buildx yourself. CircleCI's Docker layer caching feature also comes at an extra cost to the base build instances.


### Google Cloud Build


Google Cloud Build supports BuildKit, but unlike GitHub Actions or CircleCI it doesn't offer a persistent cache between builds, so to cache Docker layers you need to use a container registry.


You can make the caching work via registry on Cloud Build by pulling in the latest version of your Docker image on every build and then using the[cache-from parameter](https://depot.dev/blog/docker-layer-caching-in-google-cloud-build) . Also, remember to set the` DOCKER_BUILDKIT=1` environment variable, to ensure that BuildKit is being used.


Overall, Cloud Build may be slower than the alternatives for the following two reasons:


**Registry-based caching is unlimited in size but slow:** The upside of the registry-based caching solution is that its size is not limited. The downside is that it's slow, as you need to pull in the layers over the network every time.


**Emulation for multi-arch builds:** Google Cloud Build also doesn't support ARM compute, so if you need to build a multi-platform image, you have to rely on the slow emulation approach.


### Bitbucket Pipelines


Bitbucket Pipelines is the least efficient CI provider when it comes to Docker builds. The biggest issue is that it **doesn't fully support buildx or BuildKit** , which is a requirement for efficient builds. Because of this limitation, Bitbucket Pipelines can't handle multi-platform builds at all, as that requires buildx support.


Another issue with Bitbucket Pipelines is its tiny cache size limit (1 GB). There is a workaround for this issue: you can pull in the latest Docker image from the registry on every build and then use the` cache-from` parameter to load layers for the current build, although this approach will lead to slow network transfer speeds, like with Google Cloud Build:


```text
script:
- export DOCKER_BUILDKIT=1
+ - docker build --cache-from $IMAGE:latest .
```


If you only need to build for a single architecture and don't require much caching, it's possible to configure Bitbucket Pipelines to be somewhat effective, but if there's any chance you'll need to build to multiple platforms, or you'll have a Docker image of a considerable size, you'll need to look for an alternative solution.


### GitLab CI/CD


GitLab CI/CD offers both runners hosted by GitLab (as part of their SaaS offering) and self-hosted runners.


The **SaaS-hosted runners** support BuildKit and are configured to allow privileged access to the Docker daemon, which is helpful for faster Docker image builds. However, GitLab **doesn't offer a way to persist Docker layers between builds** other than the registry approach, which we have already discussed above.


On GitLab's **self-hosted runners** , you are responsible for hosting your own GitLab CI runners and maintaining the infrastructure that they run on. In theory, this means that you could persist the Docker layer cache between builds, as it would be located on the machines you manage.


In practice, however, each way of building Docker images on GitLab self-hosted runners is **prone to[security issues](https://depot.dev/blog/build-docker-images-in-gitlab-ci)** :


1. Using the shell executor on your own GitLab runners: requires configuring Docker Engine and granting the` gitlab-runner` user full root permissions to invoke Docker commands. So if any untrusted code ever makes it into these runners, your runner infrastructure could get compromised.
2. Using Docker in Docker (dind): requires giving each build job its own instance of Docker Engine, so jobs are slower because there is no layer caching between different Docker Engine instances.
3. Binding to the Docker socket by bind-mounting` /var/run/docker.sock` into the container where your build is running: requires you to bind-mount the Docker socket into your container, which exposes the underlying host and any other processes on that host to privilege escalation, creating a significant security risk.


An[alternative](https://docs.gitlab.com/ee/ci/docker/using_docker_build.html#docker-alternatives) to these options that GitLab suggests is to use kaniko or Buildah, or another Docker replacement, but this requires extra work and configuration.


In summary: using the GitLab SaaS runners for Docker image builds is slow due to limited caching support (requires the registry approach), and using self-hosted GitLab runners for Docker image builds tends to cause security risks.


### Jenkins


Jenkins is an open-source solution that's designed to be self-hosted, so in theory you can build the kind of environment that you need with it — including buildx and BuildKit support.


However, **self-hosting build infrastructure opens up the same security risks** that we discussed in the GitLab self-hosted section. So the options with Jenkins are either to accept security risks or to accept much slower build times.


And, of course, hosting your own environment requires time and effort, which not every team can allocate.


### Buildkite


Buildkite is a hybrid between self-hosted and a SaaS offering, similar to using GitHub Actions with self-hosted runners. The SaaS part is a control panel for visualizing and managing CI/CD pipelines, and the runners themselves are hosted by you.


Buildkite doesn't prevent you from using all Docker functionality, as it doesn't control the runners that you use with the service. So if you configure underlying hosts with BuildKit and buildx support, you will be able to use them with Buildkite.


Buildkite is another option where you **self-host the runners** , and while Buildkite offers a better deployment management functionality for self-hosted runners compared to Jenkins or GitLab self-hosted, the runners face **similar security risks** when building Docker images. Unless you're ready to invest significant time in building a dedicated environment for Docker images, with Buildkite you will need to accept either security risks or slower Docker image build times.


## The best CI provider for Docker image builds


In summary, the best CI options for Docker are:


- **CircleCI** , which offers all key Docker functionality as well as a built-in Docker layer caching solution. However, the caching still has limitations and comes at an extra cost.
- **GitHub Actions** , which supports all key Docker functionality, but has a limited built-in cache that's rather slow to use.


**None of the options we discussed offer native multi-platform image builds** without running the other architecture on your own BuildKit instance.


Google Cloud Build, Bitbucket Pipelines, and GitLab SaaS can only offer a registry-based caching solution, which is slow. Doing Docker image builds with Gitlab CI/CD, or other self-hosted options like Jenkins and Buildkite, has a high chance of introducing security risks.


**No matter which provider you use, however, you can sidestep all Docker build limitations by integrating Depot into your CI workflow.**


Depot is a drop-in` docker build` replacement — simply switch` docker build` for` depot build` , and your Docker images will be built on faster machines, specifically configured for fastest Docker build performance, with a Docker layer cache backed by physical SSDs on the build machines, and with support for native multi-architecture builds out of the box.


### Depot makes all CI providers equally fast


When you integrate Depot into your CI provider, this outsources any Docker build to Depot. This means the build will take the same amount of time no matter which CI tool you use. So once you're using Depot, it doesn't matter which CI provider you use: you can use whichever you have currently or find best for other reasons, and still get the fastest Docker builds possible.


Depot integrates with *all* CI providers and can be used locally by developers to instantly share cache between CI and local environments. This means you can also use it with other CI providers not mentioned above, such as Travis CI, AWS CodeBuild, Concourse, Drone and Tekton.


## How Depot makes any CI provider the best CI for Docker


Depot builds are run on much faster machines than CI providers typically use, with 16 vCPUs, 32 GB memory, 50+ GB NVMe cache SSD storage, and an optimized version of BuildKit.


A lot of the speed benefits of Depot come from cache improvements. For starters, the cache size is virtually unlimited, unlike the cache of many CI providers. The cache is also automatically persisted across builds, meaning no latency from transferring the cache over a network. Finally, as the cache isn't locked to one CI provider, it can be shared between CI providers if needed — and with local Docker builds as well, saving time for local development across your teams.


If you need to build Docker images to multiple architectures, Depot is easily the fastest choice, as it's capable of automatically running builds on multiple machine types (such as Intel and ARM) instead of running multiple slower builds on the same machine and having to use emulation.


If you want to try it out,[create a Depot account](https://depot.dev/sign-up) , which is free for 7 days. To integrate it into your CI provider, you can follow our[CI/CD integration guides](https://depot.dev/integrations) , which we have created for all the main CI providers.


## Related posts


- [Introducing Depot managed GitHub Actions Runners](https://depot.dev/blog/depot-github-actions-runners)
- [Depot Build API: build Docker images as a service](https://depot.dev/blog/docker-build-api)
- [How Depot speeds up Docker builds](https://depot.dev/blog/depot-magic-explained)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
