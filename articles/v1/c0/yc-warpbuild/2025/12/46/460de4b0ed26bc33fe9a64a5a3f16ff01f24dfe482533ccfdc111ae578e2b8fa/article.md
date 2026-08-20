---
schema_version: "1.0.0"
document_id: "460de4b0ed26bc33fe9a64a5a3f16ff01f24dfe482533ccfdc111ae578e2b8fa"
company_key: "yc-warpbuild"
company: "WarpBuild"
source_id: "yc-warpbuild-news-import-6421ae0a6624"
canonical_url: "https://warpbuild.com/blog/github-actions-price-change"
published_at: "2025-12-15T00:00:00+00:00"
first_seen_at: "2026-07-22T19:23:06.881853+00:00"
fetched_at: "2026-07-28T22:24:58.378758+00:00"
content_hash: "sha256:a41e9cc34aee4ea1e692a1eb3c5e3fe56de085d3b2e9b8717f9699c438be380c"
---

# GitHub Actions Price Change

GitHub Actions reduces pricing by 15-39% for GitHub hosted runners (from 2026-01-01) and adds a new $0.002/minute cost for self-hosted runners (from 2026-03-01).


GitHub[recently wrote](https://github.blog/news-insights/product-news/lets-talk-about-github-actions/) that developers used 11.5 billion GitHub actions minutes in 2025. One can safely assume that a majority of this comes from enterprises, who in turn use self-hosted runners. In the earlier model with free self-hosted runner usage, GitHub had no way to monetize most of the actions usage.


This new GitHub actions self-hosted runner tax is a simple way for GitHub to monetize their actions platform and push users to use their own runners.


Note:[BitBucket recently announced](https://www.atlassian.com/blog/bitbucket/announcing-v5-self-hosted-runners) that they will be charging for self-hosted runners as well.


This is a significant change and we break down what it means.


## GitHub's new tax on self-hosted runners


GitHub adds a new $0.002/minute cost for self-hosted runners. This is a new cost charged by GitHub for all users except for GitHub Enterprise Server (GHES) customers.


## Reduced pricing for GitHub hosted runners


GitHub has reduced pricing for GitHub hosted runners. The computation is as follows:


Smaller runners see a lesser reduction in price, whereas the larger runners see a greater reduction. It's fantastic to see that GitHub is reducing pricing for GitHub hosted runners. However, the magnitude of the reduction is not as significant as one might expect.


## Pricing table


OS vCPUs Old Price New Price WarpBuild Price delta


ubuntu 2 $0.008 $0.006 $0.004 $0.002


ubuntu 4 $0.016 $0.012 $0.008 $0.004


ubuntu 8 $0.032 $0.022 $0.016 $0.006


ubuntu 16 $0.064 $0.042 $0.032 $0.010


ubuntu 32 $0.128 $0.082 $0.064 $0.018


windows 2 $0.016 $0.010 $0.008 $0.002


windows 4 $0.032 $0.022 $0.016 $0.006


windows 8 $0.064 $0.042 $0.032 $0.010


windows 16 $0.128 $0.082 $0.064 $0.018


windows 32 $0.256 $0.162 $0.128 $0.034


macos 6 $0.160 $0.102 $0.080 $0.022


Observations


The` delta` column shows the difference between the reduced GitHub hosted runner price and the WarpBuild price. Two important observations:


1. WarpBuild runners are cheaper, even after including the $0.002/minute self-hosted runner tax imposed by GitHub.
2. WarpBuild runners are ~twice as fast, so the observed costs are still significantly lower than the GitHub hosted runners.


## Optimizing for cost


Here are the practical implications and considerations to optimize for cost, given the new pricing. These are generic and ensure you think through your workflows and runners before making any changes.


### 1. Self-hosting runners or using WarpBuild runners is still cheaper


Despite the $0.002/minute self-hosted runner tax, self-hosting runners on your cloud (aws/gcp/azure/...) or using WarpBuild runners remains the cheaper option.


### 2. Prefer larger runners


If your workflow scales with the number of vCPUs, prefer larger runners. That ensures you spend fewer minutes on the runner, which reduces the GitHub self-hosted runner tax.


For example, using` actions-runner-controller` with heavy jobs running on 1 vcpu runners is not a good idea. Instead, prefer a 2vcpu runner (say) if it runs the job ~2x faster.


### 3. Prefer faster runners


All else being equal, prefer faster runners. That ensures you spend fewer minutes on the runner, which reduces the GitHub self-hosted runner tax.


For example, if you're self-hosting on aws and using a` t3g.medium` runner, it's better to use a` t4g.medium` runner since the newer generation is faster, but not much more expensive.


WarpBuild runners have higher single core performance than aws/gcp/azure hosted runners. This is coupled with directly attached NVMe disks for fast disk IO.


### 4. Prefer fewer shards


If you have a lot of shards for your jobs (example: tests on ~50 shards), consider reducing the number of shards and parallelizing the tests on fewer but larger runners.


### 5. Improve job performance


This is not new advice, but it's now more important than ever because of the additional GitHub self-hosted runner tax.


### 6. Use GitHub hosted runners for very short jobs


For linters and other very short jobs, it's better to use GitHub hosted runners.


## What's not changing?


1. Public repos generally stand to gain from this change.


- Standard runner size (` ubuntu-latest` ) is still free.
- No $0.002/minute tax for self-hosted runners.


2. GitHub Enterprise Server (GHES) users do not have the $0.002/minute tax.


## 🚀 Try WarpBuild


Using WarpBuild runners is cheaper than using GitHub hosted runners, even after including the $0.002/minute self-hosted runner tax.


WarpBuild Cloud Runners are baremetal servers with high single core performance and directly attached NVMe disks for fast disk IO. This paired with caching and snapshotting capabilities, WarpBuild runners are a great way to optimize your workflows and save money.
