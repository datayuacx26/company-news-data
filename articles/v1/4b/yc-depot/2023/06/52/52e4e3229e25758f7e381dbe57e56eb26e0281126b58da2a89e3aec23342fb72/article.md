---
schema_version: "1.0.0"
document_id: "52e4e3229e25758f7e381dbe57e56eb26e0281126b58da2a89e3aec23342fb72"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/docker-build-no-cache"
published_at: "2023-06-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:5c3441cadaa748ef085b791866bc524365b44e77fd338aaf83d75883902de061"
---

# How to build an image without the Docker cache

Building Docker images as fast as possible is essential. The quicker you can build an image, the more quickly you can test it and deploy it to production. Docker's build cache is a great way to speed up builds by reusing layers from previous builds.


[Depot launches cloud builders for both native Intel and Arm with instant caching — if you'd rather not manage Docker layer caching yourself, you can use Depot's builders in your existing CI workflows or locally for a 2-20x speedup! Get Started →](https://depot.dev/start)


## How Docker caching works


The Docker build cache is best thought of as a stack going from the top of your Dockerfile to the bottom. Given a Dockerfile like this:


```text
FROM   node:16


RUN   apt-get update && apt-get install -y curl
WORKDIR   /app
COPY   package.json package-lock.json /app/
RUN   npm install
COPY   . .
RUN   npm build
```


Each line in the Dockerfile is a step in the Docker image build process that creates a layer in the image. These layers stack on top of each other from top to bottom to form your final Docker image.


This inheritance forms the backbone of Docker layer caching.


When you build an image with the` docker build` command, Docker executes each step from top to bottom. When executing a given step, it checks to see if it already has a layer for that step. If the step hasn't changed since the last build, the layer will exist in the cache and not get rebuilt.


If the step has changed (like you would see in the` COPY . .` because our source code changes), the layer cache won't have a match, and the step gets built again.


It's also possible for the step to not be present in the cache if you have cleaned your local Docker layer cache.


Using layer caching during` docker build` is why the order of your steps is essential. If you change a step, all the steps below get built again.


## Building an image without the cache using` --no-cache`


But, sometimes, you want to build an image without the cache. You may be debugging a build issue and want to start with a clean slate. Or you may want to force a dependency to get upgraded. Whatever the reason, you can build an image without the cache by using the` --no-cache` option.


```text
docker   build   --no-cache   .
```


This flag tells the Docker daemon to skip the cache during a` docker build` and run every step in the Dockerfile. It results in a slower build but will ensure you run every step. Specifying` --no-cache` is helpful for debugging build issues. You can also use it to force a dependency to upgrade, like` curl` in our` apt-get install` above.


### Hacking in a specific point to invalidate the cache


Sometimes you want to invalidate the cache at a specific point in the` Dockerfile` . For example, you might want to invalidate the cache after the` npm install` step so that you can debug the` npm build` step.


You can use a trick with the` ARG` instruction for that. We can set a` ARG STOP=1` step in our Dockerfile above the` npm build` . This causes the cache to invalidate at that line when we change its value.


```text
...
COPY   . .
ARG   STOP=1
RUN   npm build
```


Now when you run a` docker build` , you see that the cache gets invalidated before the build step runs. You can invalidate it again by changing the value. You can change it inside the Dockerfile or use a build argument to change it when invoking the docker build.


```text
docker   build   --build-arg   STOP=2   .
```


## Conclusion


The Docker build cache is a great way to speed up builds by reusing layers from previous builds. Optimizing for using the layer cache as much as possible ultimately speeds up a Docker build.


But sometimes, you want to build an image without the Docker cache. Using the` --no-cache` option will force the Docker daemon to run every step in the Dockerfile during a build. It helps debug build issues or force OS dependencies to get upgraded.


The Docker build cache is critical to building Docker images. There are other[fundamentals to building a Docker image](https://depot.dev/blog/docker-build-fundamentals) that can make your Docker builds even faster.


## Related posts


- [The ultimate guide to Docker build cache](https://depot.dev/blog/ultimate-guide-to-docker-build-cache)
- [The fundamentals of building a Docker image](https://depot.dev/blog/docker-build-fundamentals)
- [Build Docker images faster using build cache](https://depot.dev/blog/faster-builds-with-docker-caching)
- [How to clear Docker cache and free up space on your system](https://depot.dev/blog/docker-clear-cache)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
