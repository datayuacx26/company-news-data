---
schema_version: "1.0.0"
document_id: "f027f1e2974904d33cd7bb24ad0ea1366f43dc82caad198e470a3c861f525c20"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/fast-dockerfiles-theory-and-practice"
published_at: "2022-06-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:afa27399c0ac49955e93b048f6faa0745cee9bc185f3542ddd7dd9b201b5b697"
---

# Fast Dockerfiles: theory and practice

The core theory behind fast Docker builds is conceptually straightforward: write your Dockerfile such that as many possible build steps can be reused between builds. By skipping build steps entirely, the build has less to do and completes faster!


## An example with Node.js


To illustrate Docker layer caching, let's take a look at an example Node.js application (the same principles apply to most all Dockerfiles though).


Say you have a Node.js application, and you want to package it into a container. Understanding the basic Dockerfile building blocks of` FROM` ,` RUN` ,` COPY` , etc, you might start with:


```text
FROM   node:16


WORKDIR   /app
COPY   . .
RUN   npm install
RUN   npm build
```


With those building blocks you're all set, you have a working container image!


This is not the fastest Dockerfile. The way Docker layer caching works is by reusing steps ("layers") from the previous build that have not changed. When it encounters a layer that has changed, it needs to rebuild that layer and all following layers.


With that in mind, you may have noticed an issue with our initial Dockerfile. By placing the` COPY . .` step that copies source code into the container before anything else, any time that source code changes all the following steps must re-run.


But we know that the` npm install` step only really depends on our` package.json` and` package-lock.json` files (in this example at least). However, currently, when *any* file in our project changes, the` npm install` step must re-run, which can be slow. So an iterative improvement would be to only copy the dependent files into the image first, then run` npm install` , then copy the rest of the files:


```text
FROM   node:16


WORKDIR   /app
COPY   package.json package-lock.json /app/
RUN   npm install
COPY   . .
RUN   npm build
```


Now we have improved the cache hits and reduced the build time! If files change, but the` package.json` and` package-lock.json` files don't, the` npm install` step is skipped.


What about the` npm build` step?


That's a bit more complicated, that builds your entire application, and it's not straightforward to only copy the files that it uses as that effectively means listing almost all files in your project.


But at the same time, there are clearly files that *aren't* needed for the build. Like perhaps the` README.md` file or editor configuration. Ideally, those files would not unnecessarily invalidate the build cache.


For that, we can use a` .dockerignore` file - this is a file placed at the root of your build context that describes files that Docker should skip copying into the build context, meaning they won't be considered as part of the build, won't be copied into the container, and won't be considered when checking build cache.


For our Node.js project, we might create a` .dockerignore` file like so:


```text
dist
node_modules
README.md
.editorconfig
```


That excludes the` dist` directory, the` node_modules` directory, the` README.md` file, and the` .editorconfig` file, so none of them will be copied into the container and none of them can invalidate the previous build layers if they change.


## Applying the theory


While the theory is simple, order build steps to maximize reusing previous layers, in practice it can be tricky to get working and maintain the dependencies between project files and` RUN` steps as project structures evolve. But by spending some time ordering the build steps and crafting` .dockerignore` files, you can get major speedups in your Docker builds!


Docker and Docker's modern build engine BuildKit also provide some advanced features that allow you to even further optimize using caches, including things like mounting directories directly into the build so that builds can be incremental. We will explore some of these options in a following post.


## Building images with Depot


Depot provides hosted Docker builder instances - a remote server that runs Docker builds.


**All the Docker caching theory and techniques work without Depot** . You can get all of the performance improvements discussed here using just your local machine. Many of the simple and advanced optimizations work in CI providers too (though with some limitations).


But Depot is designed specifically to work with and enhance the Docker layer cache, adding a few additional features and speedups:


1. In CI, Depot is often several times faster as it natively saves and loads your previous build cache on local SSDs. Rather than needing to download the previous build's cache from remote storage, instead it's instantly available for the next build to use.
2. Because Depot offers remote builders, the Docker layer cache is shared among everyone with access to the project, so if your coworker builds the same project, they can reuse the same cache. You might be able to skip *all* of the build steps, the theoretical maximum speedup!


If this sounds interesting, we'd love for you to[give Depot a try](https://depot.dev/sign-up) , and run some Docker builds using your local machine or existing CI provider.


Jacob Gillespie


CTO & Co-founder of Depot
