---
schema_version: "1.0.0"
document_id: "af05d594ec47ee28fee4f8233921aa5ad4d6d5a6e7091173ce2064c3a635dbbc"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/github-actions-cache"
published_at: "2024-05-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:4b78fbaab31f33b492dec05b3235cfa7663efa9badb07b02f154675a81818373"
---

# We reverse-engineered the GitHub Actions cache so you don't have to

We recently announced our new product, Depot-hosted GitHub Actions runners. Our runners bring an extra improvement in cache speed that's no longer limited to our accelerated Docker builds. We're excited to be bringing faster caching to all kinds of GitHub Actions workloads.


As we were building our runners, we learned a lot about the undocumented inner workings of the GitHub Actions cache. In this post, we share what we learned, how we incorporated this knowledge into our new product,[Depot GitHub Actions runners](https://depot.dev/docs/github-actions/overview) , and how you can use it to make your workflows more efficient.


[Use Depot GitHub Actions runners to speed up your builds. Fast machines hosted in AWS, with up to 10x faster caching performance. Try free for 7 days. To get started, create a Depot account.](https://depot.dev/start)


## The GitHub Actions cache challenge


In order for builds in general, and especially Docker builds, to be efficient within CI, they need to rely heavily on caching, so that the compute- and time-intensive work of building code and dependencies gets reused as much as possible between builds.


The problem with caching in hosted CI platforms is that all runners are ephemeral, so the cache needs to be stored remotely. Then, when the runner needs to use the cache, it has to be transferred over the network in every build before it can be used. Networks are often slow and flaky, which can negate the speed improvements from caching when you have to use them to save and load the cache.


GitHub's own hosted runners suffer from this limitation due to capped network speed. GitHub's runners have access to around 1 Gbps of network throughput, equivalent to 125 MB/s, which greatly limits how quickly cache can be saved or restored. GitHub also limits their cache to 10 GB per repository, which can quickly become exhausted as the cache becomes larger. On top of this, the cache API itself can be flaky.


There are alternatives that seek to address these limitations, but they don't solve them completely, or they create developer experience hurdles with their solutions. For example, most other hosted GitHub Actions runner providers have created their own separate caching implementation and forked all GitHub Actions cache actions into their own namespace, in order to point the cache action to their own cache implementation. So, to take advantage of the faster caching that these providers offer, you would need to change all your workflows away from` actions/cache@v3` to something like` hosting-provider/cache@v1` . While the config change might seem small, this involves duplicating work for everyone and maintaining multiple versions of what should effectively be the same GitHub Actions cache actions.


Another issue that alternative GitHub Actions runner providers can introduce is latency and reduced bandwidth due to having the compute located in European data centers such as Hetzner. While these compute providers are inexpensive, many internet services and infrastructure providers (including GitHub) are hosted in the US, and the added latency and lower bandwidth when moving data between Europe and the US can make some workflows much slower. We believe cache actions should “just work” out of the box and be fast, even on different runners than those hosted by GitHub, and developers shouldn't have to think about which action they should use based on where their build is going to be run.


## How Depot went about solving the GitHub Actions cache challenge


To address the caching challenge when building hosted GitHub Actions runners, we asked: how can we provide GitHub Actions cache without forking the default action and maintaining the entire ecosystem, while also making it significantly faster? The GitHub Actions runner code is[freely available](https://github.com/actions/runner) under the MIT license on GitHub, but the repo doesn't offer much guidance on how to customize it. So we had to get creative.


Infrastructure-wise, we went all in on AWS, using EC2 instances for the runners and S3 for the cache storage. We believe this is the right combination to get the best performance and access the best compute options, as well as to maximize bandwidth and minimize latency for anything our customers might need to do in AWS as part of their CI builds. AWS is also the stack we have the most experience with, as we use AWS extensively for the[Depot Docker Builds product](https://depot.dev/) .


Software-wise, we decided to try to “point” the GitHub Actions runner to our own cache instead of the default GitHub Actions cache. The method for doing this was not documented in the runner repo, so we had to do some observation and a bit of reverse engineering.


As the first step, we intercepted the cache interactions on a GitHub Actions runner and realized that the GitHub Cache API responded with blob URLs hosted on Azure Blob storage. This makes sense, given that GitHub Actions runners are hosted in Azure.


Second, we determined that we could point the runner to our cache storage by running a small Go proxy on every runner and routing all GitHub Cache API calls through it, and it could respond with blob URLs hosted on S3 instead. Thankfully, there is a variable that can be changed in the GitHub Actions runner code to facilitate this. Because of the very high throughput possible from EC2 to S3, we had confidence that using EC2 for the runners and S3 for cache storage would already yield a cache speed improvement.


Third, after setting up the proxy for the cache, we saw that the uploads to and downloads from the cache were happening via only two parallel streams. While this could have been optimal on Azure, on EC2 and S3 we were confident we could use more parallelism to get higher performance. So we experimented with a higher number of parallel streams until we found the best-performing settings for both upload (4) and download (8) for our usage patterns.


Effectively, we're running an on-machine implementation of GitHub's Cache API that stores all cache content to S3, is optimized to save and restore that content using many parallel streams, and is fully integrated with Depot's authorization system. With this architecture, we're able to save and restore cache at the rate of over 1 GB/s, a 10x improvement over GitHub's default cache!


## Depot's GitHub Actions Runners architecture


While a faster cache helps to improve the overall build performance, we also needed to figure out how to make runners available on demand with the right configuration without spending a crazy amount of money running instances 24/7.


At the infrastructure level, the runners are hosted in a way that's very similar to our[Docker builders](https://depot.dev/blog/depot-magic-explained) . Specifically, we provision a new ephemeral EC2 instance for every build, and never reuse instances between builds. We use a standby pool of machines to make sure builds start instantly.


The Runners are located close to the Docker container builders, so if you use both products you can benefit from even more speedup if you are building a container than if you're pulling it back into a CI workflow for testing, for example.


More detail on how we're provisioning VMs in EC2 fast and cost-effectively is coming up in a separate post.


## Results of our GitHub Actions cache optimization


With this infrastructure and cache logic, we are able to achieve up to 10x higher cache performance compared to GitHub Actions default runners.


**Aspect** **GitHub Actions default** **Depot**


Cache download throughput 100-150 MB/s 1 GB/s


Cache upload throughput 100-150 MB/s 1 GB/s


Cache size limit 10 GB Unlimited


Retention 7 days 30 days


For Docker builds, the Depot Runners can also be integrated seamlessly with Depot Docker builders. They live right next to each other, so customers using both benefit from data locality and faster network transfers over private network links rather than over the public internet.


For example, it's easy to load Docker images built on Depot back into a GitHub Actions workflow that uses Depot's runners to run` docker compose up` , for situations like running integration tests against a newly built Docker image.


## Where to next with GitHub Actions Runners and caching


With these speed improvements, which allow your Docker caching to be far faster than with the default GitHub Actions cache, we are hoping to unlock a new level of build acceleration with very little effort required.


We're planning to add additional functionality to Depot Runners. For example, we recently announced the public beta of ARM runners, which can drastically speed up builds that target ARM platforms. We've also released upgrades to disk space so that larger runners get access to larger disk sizes right out of the box.


If you'd like to give Depot Runners a try,[create a Depot account](https://depot.dev/sign-up) (it's free for 7 days), and then change the` runs-on` parameter from` ubuntu-22.04` to` depot-ubuntu-22.04` to use a Depot GitHub Actions runner in a GitHub Actions workflow:


```text
jobs:
build:
name: Build
-    runs-on: ubuntu-22.04
+    runs-on: depot-ubuntu-22.04
steps:
...
```


You can also[check out our docs](https://depot.dev/docs/github-actions/overview) for more details.


## Related posts


- [GitHub Actions Runners | Depot](https://depot.dev/products/github-actions)
- [Running GitHub Actions jobs in a container built with Depot](https://depot.dev/blog/github-actions-jobs-in-a-container)
- [How to leverage GitHub Actions matrix strategy](https://depot.dev/blog/github-actions-matrix-strategy)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
