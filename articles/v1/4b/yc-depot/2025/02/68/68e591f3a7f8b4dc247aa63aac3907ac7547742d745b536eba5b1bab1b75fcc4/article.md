---
schema_version: "1.0.0"
document_id: "68e591f3a7f8b4dc247aa63aac3907ac7547742d745b536eba5b1bab1b75fcc4"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/faster-bazel-builds-with-remote-cache"
published_at: "2025-02-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:fb66dad0385818dd0cec51e8c6375170ed4558ff9a27bea9f5321c079381211e"
---

# Faster Bazel builds with remote cache

When it comes to modern day build tools,[Bazel](https://bazel.build/) is one that is mentioned often. It's powerful, can scale to very large projects, and is used by companies like Google, Uber, and Dropbox. One of Bazel's key features is its ability to cache build artifacts, which can significantly speed up your builds.


In this post, we're going to explore how to leverage a remote build cache for Bazel. We'll start with how caching works in Bazel before heading into enabling remote caching. Then, we'll dive into some limitations of Bazel remote cache. Finally, we'll discuss how to optimize your Bazel builds using a remote cache service like Depot Cache.


[Use Depot Cache with Bazel to unlock faster builds with distributed remote caching that's shared with your entire team and CI environment. Try it free for 7 days →](https://depot.dev/sign-up)


## Why remote caching is relevant


When it comes to remote caching in Bazel, it's all about sharing reproducible builds across machines. Using remote caching allows you to share build outputs with your entire team and across all environments, including CI, local development, and anywhere else you build your project, unlocking significantly faster builds.


## How Bazel's build cache works


When you start a build with Bazel, it creates a dependency graph of[actions](https://bazel.build/reference/glossary#action) that need to be executed to build your project. This graph of actions lays out the transformation of inputs (e.g.,` MyClass.java` ) to outputs (e.g.,` MyClass.class` ), with environment variables, command-line arguments, and other metadata included.


Each action is hashed into an[action key](https://bazel.build/reference/glossary#action-key) that is stored in the cache with a map of file locations.


When a subsequent build is performed, Bazel compares the action keys to the cache to determine which outputs can be reused. If any of the build inputs change, the cache key will also change, causing Bazel to rebuild that action and all dependent actions in the graph.


In simple terms, Bazel hashes the content of source code files and other inputs to determine if a build action needs to be executed. If the hash matches a previous build, Bazel will reuse the output from the cache. By default, Bazel caches everything in a local directory.


## Remote caching in Bazel


Bazel's remote cache is a feature that allows you to store build artifacts in a centralized location, making them accessible to all developers on your team. This can be especially useful in a CI/CD pipeline, where you can share build artifacts across all your builds.


To enable remote caching in Bazel, you need to configure Bazel to use a remote cache server. Bazel supports a number of remote cache backends, including[Google Cloud Storage](https://cloud.google.com/storage) ,[AWS S3](https://aws.amazon.com/s3/) , and[HTTP](https://bazel.build/remote/caching#http-caching) .


You can choose to implement your own remote cache server if you're OK with managing all of the underlying infrastructure and authentication. Or you can use Depot Cache, which is a fully managed remote cache service for Bazel.


Either way, you can run your Bazel build using a remote cache by setting the` --remote_cache` flag.


```text
bazel   build   --remote_cache=  <  your-cache-server-url  >   --remote_header=  <  your-cache-server-auth  >
```


Alternatively, you can set the` remote_cache` and` remote_header` flags in your` .bazelrc` file.


## Fully managed Bazel remote cache with Depot Cache


Integrating with remote cache servers can be complex and time-consuming.[Depot Cache](https://depot.dev/products/cache) is a fully managed remote cache service that is designed to work seamlessly with Bazel. With Depot Cache, you can start using a remote cache in minutes, without having to worry about managing the underlying infrastructure.


We've automatically integrated Depot Cache with our GitHub Actions Runners so that your Bazel builds automatically benefit from remote cache inside of GitHub runners out of the box. All of the cache is also instantly shared across your entire team as you can[configure your local Bazel builds to use the same Depot cache.](https://depot.dev/docs/cache/integrations/bazel) .


Just plug a Depot Cache url and token into the aforementioned CLI flags:


```text
bazel   build   --remote_cache=https://cache.depot.dev   --remote_header=authorization=YOUR_DEPOT_TOKEN
```


## Comparing GitHub Actions Cache and Depot Cache for Bazel builds


How much faster is using a managed remote cache for Bazel like Depot Cache? To demonstrate the improvement in build times, we benchmarked building[GRPC](https://github.com/grpc/grpc/) on GitHub Actions using GitHub's cache, and then compared that with building GRPC using Depot's runners, both with and without the remote cache for Bazel.


In this test specifically, we have an incremental build, where the cache is pre-populated from a build on a recent commit, then in our test we build against a slightly newer commit, giving us a realistic partial cache hit.


On the GitHub hosted runner, we are using the` actions/cache` action to load the previously built artifacts. In the Depot runners, Bazel's remote cache is enabled by default. To perform a test without the remote cache on Depot, we remove the` ~/.bazelrc` file before building.


Right away, we can see that Depot's runner, even without cache, is nearly **2.5x faster** than GitHub's runner with cache. Depot with the remote cache for Bazel enabled is **over 10x faster** than GitHub's runner with cache.


Depot's runners are[half the cost](https://depot.dev/products/github-actions) of GitHub's hosted runners,[more powerful](https://depot.dev/docs/github-actions/runner-types) , and tuned with a[number of optimizations](https://depot.dev/blog/introducing-github-actions-ultra-runners) to make your builds faster.


## Conclusion


Remote caching for Bazel is quite the powerful tool, making[repeated builds up to 50x faster](https://nicolovaligi.com/articles/faster-bazel-remote-caching-benchmark/) . By using a remote cache, you can share build artifacts across all environments, including across all of your developer machines, so you get perfectly incremental builds everywhere.


With Depot Cache, you get all of the benefits of remote caching in Bazel, but without having to manage your own cache server. We showed how using Depot Cache from a Depot GitHub Actions runner can make your Bazel builds over 10x faster. You get all of the benefits of shared caching built in, backed by a global CDN.


If you'd like to try out Depot Cache for your own Bazel builds,[you can sign up for a 7-day free trial](https://depot.dev/sign-up) and start making your builds faster today.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
