---
schema_version: "1.0.0"
document_id: "8215194897104dddc4bf2ca3b6ccf12aec935051e1c2423d44b84c7df53d28cb"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/build-docker-images-in-circleci"
published_at: "2023-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:64a9d16ec71fb1200743f62c9d213e85a5863c737c206445214cfb3de2e0495e"
---

# Building Docker Images in CircleCI with Depot

There are at least two options for building Docker images in CircleCI.


1.


We can use the` docker` executor and specify the` setup_remote_docker` key. With that key, we can run Docker commands in the job via the underlying VM that launched the job container. We get a container to run our job and the VM to execute our` docker build` and` docker run` commands.


2.


We can use the` machine` executor, which runs the entire job on the VM. Docker is installed by default, so we can run Docker commands on it.


Either of these options works, but they are limited. We still need to configure different` buildx` drivers. We still have to manage the cache across builds. And, we have to use emulation when building multi-platform or multi-architecture images.


## Building Docker images in CircleCI with Depot


With Depot, we can unlock faster Docker image builds in CircleCI. We install the` depot` CLI and swap out our` docker build` commands for` depot build` . With Depot, we get all the following benefits out of the box:


1. 16 CPUs, 32 GB memory, and a persistent 50 GB NVMe cache for Docker layer caching instantly available across builds
2. Native Intel & Arm CPUs to build multi-platform Docker images with zero emulation
3. Instant cache sharing across our entire team and in CI


### Using the` docker` executor in CircleCI with Depot


This example shows how to use the Docker executor. We use one of the CircleCI convenience images,` cimg/node:lts` , as the container image for the job.


If we needed a custom Docker image for the remote Docker environment, we could use that instead. Both options work with Depot.


```text
version  :   2.1


jobs  :
build  :
docker  :
-   image  :   cimg/node:lts
resource_class  :   small
steps  :
-   checkout
-   setup_remote_docker
-   run  :
name  :   Install Depot
command  :   |
curl -L https://depot.dev/install-cli.sh | sudo env DEPOT_INSTALL_DIR=/usr/local/bin sh
-   run  :
name  :   Build with Depot
command  :   depot build .


workflows  :
run_build  :
jobs  :
-   build
```


We set the resource class to the smallest size the Docker execution environment allows,` small` . We don't need lots of resources for the job because the actual build will happen on Depot builders. The default resource class for CircleCI and the Docker executor at the time of this writing is the` large` resource class. The difference between the two is 20 credits/minute versus 5 credits/minute.


We still use the` setup_remote_docker` key to route Docker commands, like` docker run` or` docker login` , to the underlying VM.


#### Pushing to private Docker registries with the` --push` flag


We can push our Docker image to a private registry, like Docker Hub, by adding the` --push` flag to our build command.


We can use the` docker login` command to authenticate to the registry as we would do before a` docker build` .


```text
version  :   2.1


jobs  :
build  :
docker  :
-   image  :   cimg/node:lts
resource_class  :   small
steps  :
-   checkout
-   setup_remote_docker
-   run  :
name  :   Install Depot
command  :   |
-   run  :
name  :   Build and push to DockerHub with Depot
command  :   |
echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin
depot build -t my-dockerhub-registry --push .


workflows  :
run_build  :
jobs  :
-   build
```


We use the credentials passed via environment variables to authenticate to Docker Hub. Then we use` depot build` to build our image and push it to our registry from the Depot builder. The build context sent to Depot contains the authentication context to push to our registry, the default behavior of BuildKit.


#### What about using the image after the build?


We can specify the` --load` flag on our build command to load it into the CI workflow we're running. This is helpful when we want to build the image and then run integration tests with it.


We can even specify` --load` and` --push` simultaneously to push to our registry and load the image back.


### Using the` machine` executor in CircleCI with Depot


Using the` machine` executor is very similar. We use the smallest resource class to execute our job but route the image build to Depot. We remove the` setup_remote_docker` key as the Docker CLI is already on the machine:


```text
version  :   2.1


jobs  :
build  :
machine  :   true
resource_class  :   medium
steps  :
-   checkout
-   run  :
name  :   Install Depot
command  :   |
-   run  :
name  :   Build and push to DockerHub with Depot
command  :   |
echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin
depot build -t my-dockerhub-registry --push .


workflows  :
run_build  :
jobs  :
-   build
```


The machine executor uses the smallest resource class,` medium` , at 10 credits/minute—half the cost of the default` large` resource class. We then install our CLI and run` depot build` to build our image.


## Advantages of building Docker images in CircleCI with Depot


With Depot, we can build our Docker images in CircleCI faster, saving time and credits. In addition, we also unlock a lot of other benefits that we don't get out of the box:


1. [Turbo Builders](https://depot.dev/blog/turbo-builders) with 16 CPUs, 32 GB of memory, and a 50 GB persistent NVMe cache
2. Native multi-platform Docker image builds on native Intel & Arm CPUs with a single build
3. [Instant cache sharing](https://depot.dev/blog/local-builds) across our entire team and CI


#### Build a multi-platform image in CircleCi


With this CircleCI config, we can build a container image with zero emulation that can run on both Intel & Arm. Check out[our guide on building Arm containers](https://depot.dev/docs/container-builds/how-to-guides/arm-containers) for more details.


```text
version  :   2.1


jobs  :
build  :
machine  :   true
resource_class  :   medium
steps  :
-   checkout
-   run  :
name  :   Install Depot
command  :   |
-   run  :
name  :   Build and push to DockerHub with Depot
command  :   |
echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin
depot build -t my-dockerhub-registry --push --platform linux/amd64,linux/arm64  .


workflows  :
run_build  :
jobs  :
-   build
```


## Conclusion


To see more examples of how to use Depot in CircleCI, check out our[CircleCI integration guide](https://depot.dev/docs/container-builds/integrations/circleci) .


If you want faster Docker image builds in CircleCI,[sign up for an account](https://depot.dev/sign-up) and try things out with our 7-day free trial. If you have questions about how to make Docker image builds faster, in general, hop into our[Community Discord](https://discord.gg/MMPqYSgDCg) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
