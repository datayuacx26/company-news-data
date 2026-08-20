---
schema_version: "1.0.0"
document_id: "c290481b50719b0f7c1cf944c627a7fa80ffe0f9531b1d70af7d84a6c8e7c0e1"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/optimize-for-remote-docker-image-builds"
published_at: "2023-11-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:5f37636f8f39eaf5263f651e0dd51fe156386fc9e39e885f5dafcd18ff3e26ca"
---

# How to optimize Docker image builds for Depot

When building Docker images with a remote container build service like Depot, there are a few things you can do to optimize your builds. In this post, we'll review tips and tricks to optimize your Docker image builds for Depot.


## What's different?


When using Depot, the build context for your Docker image build has to be transferred over the network to our builders. However, the entire build context is only transferred once, and only the changes are transferred on subsequent builds. So, if you have a large build context, it will take longer to transfer the first time, but following builds will be much faster.


Below are some ways to optimize Dockerfiles and your builds to decrease the impact of network latency and reap the full benefits of Depot's builder infrastructure with instant cache sharing.


## Use a` .dockerignore` file


Similar to how[using a .dockerignore to decrease image size](https://depot.dev/blog/how-to-reduce-your-docker-image-size#add-a-dockerignore-file) , a` .dockerignore` file can also help reduce the size of your build context. This will decrease the amount of data that needs to be transferred over the network to our builders.


A few things we often suggest adding to your` .dockerignore` file are:


- Directories like` node_modules` should often be excluded as you should typically reinstall your dependencies during a build via your Dockerfile
- Exclude directories where build output or artifacts would be generated again during a build; it's redundant to transfer them over the network when they will just be rebuilt again
- Ignore the` .git` directory unless you need to use your version control history during your build
- Ignore unnecessary files like` README.md` or` LICENSE` files and test files that you don't need in your final image


In short, the more unnecessary files you remove from your build context, the less data must be transferred over the network to our builders.


## Use smaller base images


Smaller base images directly impact the final image size.[Shaving down your base images](https://depot.dev/blog/how-to-reduce-your-docker-image-size#shave-lots-of-bytes-with-smaller-base-images) via things like` -alpine` images can help reduce the amount of data that needs to be transferred over the network when using` --load` or` --push` .


## Use multi-stage builds


A multi-stage Docker image build not only helps your[final image size](https://depot.dev/blog/how-to-reduce-your-docker-image-size#leverage-multi-stage-builds) , but it can also make your builds faster. Each stage in a multi-stage build can be built in parallel. Artifacts constructed in different stages can be copied into your final minimal image with` COPY --from` statements.


## Use remote files during your build


Rather than using a local file that gets uploaded to your Depot builder as part of the build context, it's better to fetch remote files during the build. If you have a file that may exist locally, it's typically better to fetch it during your build rather than uploading it as part of the build context. This will decrease the amount of data that needs to be transferred over the network and allow you to download the remote file faster via the fast networks attached to builders.


## Use multi-threaded tools


Each Depot builder runs an optimized version of[BuildKit](https://depot.dev/docs#how-does-it-work) on cloud VMs for Intel & Arm that have 16 CPUs and 32 GB of memory available to them. If you can use tools that can leverage multiple cores, you can take advantage of the additional CPU resources available.


## Conclusion


Optimizing your Docker image builds for building in the cloud is about keeping your build context and image size as small as possible. This helps reduce network transfer, making your builds faster. Additionally, a remote Depot builder has more resources available dedicated to your build than your local machine in most cases. So, using tools that can use those resources will further speed up your builds.


We're also working on additional features to alert you and get visibility into your build context so that we can surface suggestions about how to further optimize your build.


If you're interested in trying out Depot,[sign up for our 7-day free trial](https://depot.dev/start) with no credit card required.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
