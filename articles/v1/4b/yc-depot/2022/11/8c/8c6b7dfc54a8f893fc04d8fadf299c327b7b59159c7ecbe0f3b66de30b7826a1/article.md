---
schema_version: "1.0.0"
document_id: "8c6b7dfc54a8f893fc04d8fadf299c327b7b59159c7ecbe0f3b66de30b7826a1"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/docker-layer-caching-in-google-cloud-build"
published_at: "2022-11-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:8dda061ec43f4d67f27791a268ba405fa53cf4a77b70c9933e2c2bea4cbb3662"
---

# Faster Docker image builds in Cloud Build with layer caching

The key to building Docker images quickly across CI providers like Google Cloud Build is to make use of the previous build's layer cache. There is the[theory and best practice](https://depot.dev/blog/fast-dockerfiles-theory-and-practice) for creating a` Dockerfile` that takes advantage of layer caching by trying to get as many cache hits as possible during a build. But, in a CI environment, you need to have the layer cache available to the build for that work to pay off.


In this post, we will focus on how to build a Docker image as quickly as possible in Cloud Build by leveraging layer caching. We will benchmark build performance with caching using the` docker` executor,` kaniko` executor, and our own` depot` service.


[Depot launches cloud builders for both Intel and Arm — if you'd rather not manage Docker layer caching yourself, you can use Depot's builders in your existing Google Cloud Build configuration for a 2-20x speedup! Get Started →](https://depot.dev/start)


## Building Docker images in Cloud Build


Getting an image built inside Cloud Build can be done with a single step inside a` cloudbuild.yml` file. Here is an example where we are building a Node application that has the following` Dockerfile` :


```text
FROM   node:16   AS   build


WORKDIR   /app
COPY   package.json yarn.lock tsconfig.json ./
COPY   src/ ./src/
RUN   yarn install --immutable
RUN   yarn build


FROM   node:16
WORKDIR   /app
COPY   --from=build /app/node_modules /app/node_modules
COPY   --from=build /app/dist /app/dist
ENV   NODE_ENV production
CMD   [  "node"  ,   "--enable-source-maps"  ,   "./dist/index.js"  ]
```


To build this image we add a` cloudbuild.yml` file to the root of the repository with the following contents:


```text
steps  :
-   name  :   gcr.io/cloud-builders/docker
args  :
-   build
-   -t
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:$COMMIT_SHA
-   -t
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:latest
-   .


images  :
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:$COMMIT_SHA
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:latest
```


This configuration tells Cloud Build to build an image using the` docker` builder image with two different tags, one for the commit SHA and the other for` latest` . Then the` images` block tells the build to push those resulting images to Artifact Registry. Running the build we see the total build takes **1 minute and 40 seconds** , with the image build portion taking **~78 seconds** .


If you run the build a second time, you will notice that the image build is again, approximately 78 seconds. Why? Because we aren't doing anything to use the previous builds' cache. We can add that by updating our` cloudbuild.yml` file to the following:


```text
steps  :
-   name  :   gcr.io/cloud-builders/docker
entrypoint  :   bash
args  :
-   -c
-   docker pull us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:latest || exit 0


-   name  :   gcr.io/cloud-builders/docker
args  :
-   build
-   -t
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:$COMMIT_SHA
-   -t
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:latest
-   --cache-from
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:latest
-   .


images  :
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:$COMMIT_SHA
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:latest
```


This is the easiest way to leverage the build cache of a previous build in Cloud Build. Here we are pulling down the` latest` tag for the image we are building in the first step. We pull that tag down so that we can make use of it in the` --cache-from` flag in the second step. This allows us to utilize the build cache from the previous build because we tag all new images with the` latest` tag.


So, what are the results now? The first build took about 78 seconds to build the image and the second build takes **~38 seconds** . A nice improvement, but if you look closely, something doesn't look right.


Did you spot it? We got the` docker build` portion down to 38 seconds, but the entire build still took **~78 seconds** . Why is that? Well, it's because pulling the` latest` tag to use in the` --cache-from` takes time to transfer that image from the registry to your build so it can be used for caching. In this case, that took 25 seconds and has negated any benefit we could have seen from using the layer cache in total build time.


## Building Docker images in Cloud Build with Kaniko


[kaniko](https://github.com/GoogleContainerTools/kaniko) is a tool that allows you to build container images inside Kubernetes without needing the Docker daemon. Effectively, it allows you to build Docker images without` docker build` .


We can actually change our` cloudbuild.yml` file to use` kaniko` instead of the` docker` builder image. With the Kaniko executor in Cloud Build, we can specify a` --cache` flag that allows us to store our Docker layer cache in Container Registry. Here is the updated` cloudbuild.yml` file:


```text
steps  :
-   name  :   gcr.io/kaniko-project/executor:latest
args  :
-   --destination=gcr.io/$PROJECT_ID/depot-demo/demo
-   --cache=true
-   --cache-ttl=24h
```


If we run a build with this configuration, we see the following results:


The entire build took **2 minutes and 30 seconds** , and the image build portion took **2 minutes and 19 seconds** . That's not ideal, but maybe build performance will be better for the next build because we can use the layer cache via Kaniko. Let's run the build again and see what happens:


On the second run, the image build is now **~69 seconds** and the entire build is **79 seconds** . An improvement over the previous run because we get to make use of caching, but we aren't seeing any improvement over our Docker builder approach. In fact, the total time is effectively the same, and the image build is slower. To recap, here are the results we have seen so far:


total time image build time


with` docker` builder (no cache) 100s 78s


with` docker` builder (with cache) 78s 38s


with` kaniko` builder (no cache) 150s 139s


with` kaniko` builder (with cache) 79s 69s


## Faster Docker image builds in Cloud Build with Depot


We've observed that using the Docker layer cache across builds speeds up build times significantly. But, as we saw, the current approach for doing that in Cloud Build can negate any performance gains because of network latency. The image build might take 38 seconds, but the entire build still takes a total of 78 seconds to complete because it takes another 25 seconds to pull down the` latest` tag to use for caching.


What if we could use the layer cache without the network penalty? That is where Depot comes in. Depot provides remote container builders on cloud VMs. They come with more resources, 16 CPUs and 32 GB memory, and a 50 GB persistent SSD cache. A large, fast, and persistent disk allows us to share layer cache across builds automatically without spending any time transferring the cache for the build.


We can use Depot to build our image in Cloud Build by using the` depot` builder image. Here is the updated` cloudbuild.yml` file:


```text
steps  :
-   id  :   Build with Depot
name  :   ghcr.io/depot/cli:latest
args  :
-   build
-   --project
-   <your-depot-project-id>
-   -t
-   us-west1-docker.pkg.dev/$PROJECT_ID/depot-demo/demo:$COMMIT_SHA
-   .
-   --push
env  :
-   DEPOT_TOKEN=${_DEPOT_TOKEN}
```


This configuration uses the` depot` builder image to build the image. The` --project` flag routes your build to your Depot project and the remote builders that back it, using the` DEPOT_TOKEN` environment variable to authenticate the build to your project. Note the token used here is a project token that can be[created under your project settings](https://depot.dev/docs/cli/authentication#project-tokens) .


If we run our first build using this configuration, we see an output like the one seen below:


We can see that the first build is uncached and takes a total of **73 seconds** to complete, with **64 seconds** of that being the image build. Things are already faster when looking at total build time than other previous options. Let's run a second build that leverages the persistent SSD cache on the remote builders. We don't have to make any changes to leverage the layer cache; it's already on the remote builder.


The entire build took **28 seconds** , and the image build portion was **15 seconds** . Depot is over 2x faster at building Docker images inside Google Cloud Build than any of the previous options we looked at.


total time image build time


with` docker` builder (no cache) 100s 78s


with` docker` builder (with cache) 78s 38s


with` kaniko` builder (no cache) 150s 139s


with` kaniko` builder (with cache) 79s 69s


with` depot` builder (no cache) 73s 64s


with` depot` builder (with cache)


28s


15s


## Conclusion


In this post, we have seen the variety of different ways that you can build Docker images in Google Cloud Build. A fundamental component to making image builds fast is using the layer cache from previous builds.


As we saw, with` docker` , we can use the layer cache by pulling down the previous image and using it as a cache source. This produced an image build twice as fast, but the total build time remained roughly the same because of the network penalty of pulling down the previous image.


With` kaniko` , we can use the layer cache by persisting it to Container Registry. But the image build, for cached and uncached, wasn't any faster than using the` docker` builder image. Moreover, Kaniko caching is slower as it snapshots the filesystem after each layer, and a network penalty is still being paid to transfer the layer cache from Container Registry.


With` depot` , we can use the native Docker layer cache without the network penalty. The image build is over 2x faster than the` docker` approach and almost 3x faster than the` kaniko` approach. The layer cache is persisted to a fast SSD on the remote builders, allowing subsequent builds to be faster by using it automatically.


For a complete walk-through on leveraging Depot inside your existing Google Cloud Build workflows, check out our[Google Cloud Build integration guide](https://depot.dev/docs/container-builds/integrations/google-cloud-build) .


## Related posts


- [How to use Docker layer caching in GitHub Actions](https://depot.dev/blog/docker-layer-caching-in-github-actions)
- [The ultimate guide to Docker build cache](https://depot.dev/blog/ultimate-guide-to-docker-build-cache)
- [How to use BuildKit cache mounts in CI providers](https://depot.dev/blog/how-to-use-buildkit-cache-mounts-in-ci)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
