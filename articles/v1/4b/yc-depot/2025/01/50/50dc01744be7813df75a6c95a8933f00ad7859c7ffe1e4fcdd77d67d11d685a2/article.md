---
schema_version: "1.0.0"
document_id: "50dc01744be7813df75a6c95a8933f00ad7859c7ffe1e4fcdd77d67d11d685a2"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/slow-builds-are-costing-you-money"
published_at: "2025-01-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:6cd76cb34a833467a6d5c20679b3344cb264a49f6d9d6c2d925b9a25a7a12404"
---

# How slow builds are costing you money

It might sound counterintuitive, but adding services to your CI setup can actually help reduce your costs.


When it comes to cutting expenses, most people instinctively think of trimming logos off their expense sheet, but let’s break down how optimizing your builds with Depot will ultimately cut your CI spend in half.


[Build faster. Waste less time. Depot accelerates Docker image builds and GitHub Actions workflows. Easily integrate with your existing CI provider and dev workflows to save hours of build time. Get Started →](https://depot.dev/start)


## Components of cost


Before we can really compare and contrast costs, it’s important to break down the cost accrued in a continuous integration (CI) environment such as GitHub Actions (GHA).


### Direct costs: Build minutes, cache storage, and data transfer


Many cloud platforms nowadays use a consumption-based billing model. GitHub Actions follows this trend by charging for build minutes. Runners are ephemeral; thus, cache results or things you want to reuse across builds must be persisted externally. Let’s run through a hypothetical example using a GitHub Actions 32-core Linux runner for some calculations.


#### Compute


Suppose you have a project that has a workflow with three jobs that run about eight minutes each. If this project gets built 100 times a day (say you’re working with a team of twenty devs), that’s 2,400 build minutes used up per day. If you average 22.5 working days per month and a single build minute costs $0.128, that gives you a total compute cost of over $6,900 per month.


We have recently introduced a[GitHub Actions cost calculator](https://depot.dev/github-actions-price-calculator) that you can use to estimate the total cost for your setup based on detailed usage for each GitHub Actions runner type.


#### Storage


While storage is generally much cheaper than compute, it’s still a tangible cost. Our example project and dev team use around 3TB for the month, because some of the builds produce large artifacts, and it’s a pain to clean up storage. At a unit cost of $0.008 per GB, that’s another $700 to your monthly bill for GitHub Actions.


#### Data transfer (billed outside of GitHub Actions)


GitHub’s managed runners are hosted in Azure. If your infrastructure is located outside of Azure — for example, in AWS — you may incur additional costs if you move a lot of data between CI and your infrastructure. On AWS, inbound traffic is free, so you won’t pay anything when uploading artifacts from GitHub Actions to AWS. However, outbound traffic is priced at $0.05–$0.09 per GB, so if you’re fetching anything from your S3 bucket inside your GitHub Actions workflows, you’ll be charged for that.


$0.05 per GB may seem small, but if you end up fetching, say, 3TB worth of packages and assets from S3 in a month (only around 1GB per build, if you’re building 100 times per day), the extra data transfer charge on AWS will cost around $280.


If your use case involves downloading large volumes of data stored in AWS into your CI workflows, such as with ML datasets, the data transfer costs can quickly become exorbitant.


### Indirect costs: Bottlenecks and unoptimized builds


There are also indirect costs that make bills higher than they could be. These tend to be inefficiencies in tooling, workflows, or infrastructure that inflate the amount of build minutes used. As we’ve covered how relatively expensive build minutes can be, these indirect costs really add up over months and years.


#### Build slowness due to low network transfer speed


If a build command takes longer to complete due to slow network transfer speeds, you’re still going to pay for the extra time.


For example, if you’re transferring large volumes of data in your CI jobs to and from S3, the transfer will likely take longer on GitHub Actions hosted runners, as they are located in Azure, and the traffic needs to cross multiple networks.


#### GitHub Actions per-minute billing


GitHub Actions bills you for jobs by[rounding up](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions#included-storage-and-minutes) your usage to the nearest minute. While this may sound like a small detail, it can massively increase the cost of running many small jobs. For example, if you run three jobs of twenty seconds each, despite the total run time being only one minute, GitHub Actions will bill you for one full minute for each job (three minutes in total), increasing the cost by 300%.


#### Caching


Caching is meant to improve build speed and thus lower build minute accrual. Regardless of cache hit rates and compression percentages, caching takes time. If our example project from above uses 1 build minute per job to download and extract the cache, that’s an extra 300 minutes per day or another $800 per month.


The caching example may be considered a minor indirect cost. If you consider the time saved by caching, it might even be considered a wash, or better yet, a savings. What happens when GitHub Actions 10 GB cache storage[limit](https://github.blog/changelog/2021-11-23-github-actions-cache-size-is-now-increased-to-10gb-per-repository/) is reached? Those build minute savings can go out the window.


#### Hardware: General performance


The hardware used, and for that matter, *not* used, is a big factor in indirect costs. The[clock speed of the CPU](https://depot.dev/blog/comparing-github-actions-and-depot-runners-for-2x-faster-builds#not-all-cores-are-created-equal) , the speed and throughput of the disk, networking bandwidth, and latency all play a role in how many build minutes may be used for the exact same workloads. Older, less optimized hardware will be slower to complete the exact same work as newer, more modern hardware attached to better storage systems and networks.


#### Hardware: Native platform support


[Docker multi-platform images](https://depot.dev/blog/multi-platform-docker-images-in-github-actions) are popular; however, building for multiple platforms comes with challenges. Building a Docker image for a non-native architecture can be up to 40x slower than on a native architecture. Simple math shows an up to 40x increase in build minute usage.


GitHub Actions offers runners for amd64 and arm64, so it is theoretically possible to avoid this massive slowdown by building two images on their respective native architectures and then merging them into one image. However, this approach is cumbersome (and thus costly in time, if not in dollars) to set up and maintain.


## Depot runners are faster, resulting in lower build minute usage


Now that we know how costs are calculated and what drives up the price of the final bill, we can dive into how Depot uses a combination of software and hardware (our own runners) to drastically reduce the amount of build minutes used per month.


Let’s start with the **runner specs** . Both GitHub Actions’s and Depot’s standard Linux runners come with two vCPUs. While Depot’s runner comes with faster CPUs, faster disks, more disk space, and optimized cache storage, the highlight here is the pricing: Depot’s runner is $0.004/minute, half the price of the GitHub Actions runner. Considering that Depot builds tend to complete faster than GitHub Actions’s (due to better hardware), there’s at least a 50% savings to be had.


For the cost of this GitHub Actions runner, you can also choose to use a Depot runner that’s one size larger with four vCPUs, resulting in more build performance at the same price as GitHub Actions. This in turn, shortens build time and generates savings (which can be up to 50% or more, depending on build specifics). You should verify that your job can make use of additional CPU cores as some tools are single-threaded.


Most recently, we introduced[Ultra Runners](https://depot.dev/blog/introducing-github-actions-ultra-runners) , which come pre-configured with a RAM disk. Most CI workloads are[disk-constrained](https://depot.dev/blog/uncovering-disk-io-bottlenecks-github-actions-ci) , so they will complete faster when a RAM disk is available. In our testing, the Ultra runners were up to 3x faster than the default GitHub Actions runners, corresponding to saving up to 66% cost on build minutes needed.


*Interested in a direct GitHub Actions Runner vs Depot Runner number comparison?[Check out this table](https://depot.dev/blog/comparing-github-actions-and-depot-runners-for-2x-faster-builds#real-world-ci-time-and-cost-comparison) .*


Depot’s builders **run in AWS** rather than Azure. If your infrastructure is already in AWS, having your CI runners in the same networks will result in faster transfer speeds, which will reduce the number of build minutes required and result in a lower data transfer bill, especially if you are transferring large assets in your CI flows.


Depot’s **[cache storage](https://depot.dev/blog/cache-v2-faster-builds)** is purpose-built to make saving state between builds fast. Depot’s cache[maximizes throughput](https://depot.dev/blog/uncovering-disk-io-bottlenecks-github-actions-ci#maximum-disk-throughput) between build machines and storage. The result is fast cache-save and cache-load operations that significantly reduce build time spent on cache operations. For larger files, the time savings are particularly noteworthy: Writing a 50GB cache takes 3m58s on average when using the standard GitHub Actions runner, and only 1m27s when using Depot’s Ultra Runner.


Depot offers[multi-platform Docker builds](https://depot.dev/blog/multi-platform-docker-images-in-github-actions) out of the box, letting you both take advantage of **native-speed Docker builds on Intel and ARM platforms** , which are up to 40x faster than emulation, and you do not have to configure those yourself.


## Depot vs GitHub Actions cost comparison example


Returning to our example project above, let’s tally up the numbers.


### Cost Comparison: GitHub Actions vs. Depot


**Category** **GitHub Actions** **Depot** **Explanation**


**Build minutes** $6,900/mo $3,450/mo Depot’s faster builds reduce the total time and cost, depending on build configuration.


**Cache storage** $700/mo $550/mo Depot’s optimized caching minimizes storage costs while improving efficiency.


**Data transfer (<-> AWS)** $280/mo $0 Depot runners are hosted in AWS; keeping data in the same region eliminates transfer costs.


**Indirect cost (slow cache)** $800/mo $0 Depot’s cache eliminates bottlenecks and delivers at ultra-fast speeds.


**Depot subscription** — $200/mo Try Depot's *Startup* plan with a[7-day free trial](https://depot.dev/start)


**Total Cost** **$8,680/mo** **$4,200/mo** **Savings of $4,480/mo**


Put another way, *not* using Depot in such a scenario would be *costing* you $4,480 every single month.


This number is purely based on the hard costs of compute, cache, and data transfer. It excludes developer productivity improvements or additional value created through faster build times.


*If you’d like more details, check out our[open-source Next.js example](https://depot.dev/blog/comparing-github-actions-and-depot-runners-for-2x-faster-builds) for a side-by-side runner comparison based on actual bills and run times.*


The numbers will ultimately vary depending on which GitHub and Depot plans you’re using and the workloads you are running. But as we’ve seen above, slow networks, storage, and compute ultimately result in slower builds. And slower builds are costing you money.


When you add into that the time developers are stuck waiting for slow builds and the frustration they’re experiencing, it is safe to say that slow builds are impacting your ability to innovate. Add to that the fact that “the amount of time we save your company in developer productivity means you’re actually saving money!” and really, not using Depot is *costing* you money.


If you’re fed up, like we were when we started building Depot, try out[Depot runners for GitHub Actions](https://depot.dev/products/github-actions) . It’s free for 7 days. Or at the very least, come vent about slow builds in our[Discord Community](https://discord.gg/MMPqYSgDCg) . 🙂


## Related posts


- [How to build Arm and multi-architecture containers today](https://depot.dev/blog/building-arm-containers)
- [Comparing GitHub Actions and Depot runners for 2x faster builds](https://depot.dev/blog/comparing-github-actions-and-depot-runners-for-2x-faster-builds)
- [Uncovering Disk I/O Bottlenecks in GitHub Actions](https://depot.dev/blog/uncovering-disk-io-bottlenecks-github-actions-ci)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
