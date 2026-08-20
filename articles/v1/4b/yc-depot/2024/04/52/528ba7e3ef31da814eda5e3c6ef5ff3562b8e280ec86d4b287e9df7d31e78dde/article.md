---
schema_version: "1.0.0"
document_id: "528ba7e3ef31da814eda5e3c6ef5ff3562b8e280ec86d4b287e9df7d31e78dde"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/build-context"
published_at: "2024-04-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:12ba84bc92209690800690fb54369fe00c1679e4c497beb620a14f07e42ef591"
---

# How to debug your Docker build context with Depot

As part of our new[Build Insights](https://depot.dev/blog/build-insights) feature, we have a new Docker build context tab that lets you visualize what was in your build context when the build ran. The first time you run a build with no cache, you can see the full context that is transferred to Depot. On subsequent builds, you can see the modifications to the context.


With the context tab, you can quickly identify the total size of your context, the size of each file inside, how long each file took to transfer to Depot, and what directories were included.


## What is Docker build context?


Docker build context is the collection of files and directories that your Docker image build process receives and can access. It's referenced as a positional argument after either a` docker build` command with the Docker CLI or` depot build` command with our drop-in replacement CLI. Traditionally, your build context is passed to the Docker daemon, which then sends it onward to build your image with BuildKit on your local machine.


With Depot, your Docker build context is sent directly to our remote container build service, backed by BuildKit, to build your image. This allows you to build images faster on native Intel and Arm CPUs with fast persistent layer caching and, as we will see, better insights into what your build is doing.


The most common Docker build context folks leverage is their current working directory:


```text
docker   build   .
```


In this case, the` .` is the Docker build context. The build will send all files and directories in the current working directory to the builder.


If you want to exclude files from your Docker build context, you can use a` .dockerignore` file. This file is similar to a` .gitignore` file, but instead of ignoring files from Git, it tells which files and directories should be ignored as part of a Docker image build. For example, we probably want to ignore the` .git` folder and our` README.md` as we don't need them in our actual Docker image build.


```text
.git
README.md
```


The first time you run an image build with no cache, the entire Docker build context is passed to Depot. But on subsequent builds, only changes to the build context, like your source code changes, are passed up. So, if you have a large build context but only a few files have changed, we only need to sync those files after your first build.


## Visualizing what is in your Docker build context with Depot


Your Docker build context can impact your image build performance and final Docker image size.


First, the larger your build context, the longer it takes to transfer to a remote build service, which slows down your build.


Second, a bloated Docker build context can increase image sizes because you can inadvertently` COPY` files into your image that you didn't expect to be in your context.


However, debugging these problems has always been challenging. Understanding what is in your build context usually requires using tools like` dive` or building out-of-band images that mount your build context to inspect it.


But now, with the new Docker build context tab in your Depot Build Insights, you can quickly see what is in your build context on every build.


The build context view inside your build details shows you the total size of the context that was passed up to Depot, the size of each file, and individual file transfer times. This allows you to quickly debug why your build is slow or why your final Docker image is larger than expected.


## Conclusion


Debugging image builds, particularly what is in your Docker build context, has always been challenging. Our goal is to make not only your builds faster but also your entire lifecycle around them faster. This includes making them easier to debug by surfacing the data you need to understand what is happening. With the new context tab, we are surfacing precisely what was transferred in your build context to Depot.


If you're looking for help in debugging your own Docker build context, you can[sign up](https://depot.dev/start) for Depot and get started today with a free trial. We're also available in our[Community Discord](https://discord.gg/MMPqYSgDCg) to help answer any questions.


## Related posts


- [The complete guide to getting started with building Docker images](https://depot.dev/blog/docker-build-image)
- [How to reduce your Docker image size](https://depot.dev/blog/how-to-reduce-your-docker-image-size)
- [How to optimize Docker image builds for Depot](https://depot.dev/blog/optimize-for-remote-docker-image-builds)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
