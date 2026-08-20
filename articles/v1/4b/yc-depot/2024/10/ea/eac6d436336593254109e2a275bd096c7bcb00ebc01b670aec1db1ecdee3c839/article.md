---
schema_version: "1.0.0"
document_id: "eac6d436336593254109e2a275bd096c7bcb00ebc01b670aec1db1ecdee3c839"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/introducing-github-actions-ultra-runners"
published_at: "2024-10-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:c7cf95e955f933f76261d22b76b3a884acd7f09fb8ee8d75c9f95f22f38dc5cc"
---

# Introducing Ultra Runners — Up to 3x faster GitHub Actions jobs

Slow builds are annoying and take you out of the flow, and we're on a mission to make builds exponentially faster.


With Depot's remote container build service, you can build Docker images up to 40x faster, with fast, native CPUs, automatic caching, and easier debuggability.


And earlier this year, we launched our own[Depot-managed GitHub Actions runner offering](https://depot.dev/products/github-actions) , with faster compute, 10x faster caching, with a wide variety of runner types including macOS runners, ARM runners, Intel runners, and even the ability to[use GPUs in your GitHub Actions jobs](https://depot.dev/docs/managed/using-gpus) . All at half the cost of GitHub's own runners, billed by the second.


Today, we're excited to announce the next evolution of Depot-managed GitHub Actions runners, **Ultra Runners** , that make your builds **up to 3x faster** .


[Build faster. Waste less time. Depot accelerates Docker image builds and GitHub Actions workflows. Easily integrate with your existing CI provider and dev workflows to save hours of build time. Get Started →](https://depot.dev/sign-up)


## Speeding up GitHub Actions runners


One of the hidden bits of complexity when running Depot GitHub Actions runners is the lack of control over the actual runner code. For example, everyone can create an image that runs a GitHub Actions runner. In fact, the image definitions are even[open source](https://github.com/actions/runner-images) .


But these are merely all the tools and dependencies that must be installed into the runner's VM. It's not the actual runner code; that is a black box that GitHub controls. We receive a webhook,[start a runner as fast as possible](https://depot.dev/blog/depot-github-actions-runners#how-it-works) , and then the runner starts & waits for the mothership of GitHub to pass it a job.


We optimize a lot of the hops in that chain, but it's really annoying to not control the code itself. We can't make the runner faster from within.


But our goal is to make builds exponentially faster, so how do we do that? We can't change the runner code, but we can change the runner's environment. So, we did just that.


## Introducing Ultra Runners


Ultra runners are our next iteration on this problem of slow and expensive GitHub Actions jobs. The same runner code from before now runs in an even faster environment.


We've benchmarked our default ultra runner` depot-ubuntu-latest` against the default GitHub runner` ubuntu-latest` using[upstream BuildKit](https://github.com/moby/buildkit) as our repository.


Comparing a default Depot Ultra Runner to a GitHub default runner


The results show that our default runner with our new Ultra Runner environment is **up to 3x faster** than the default GitHub runner.


So how did we do it? Memory. More specifically, Ultra Runners are a new type of runner that comes pre-configured with a RAM disk.


Generally speaking, RAM disks are orders of magnitude faster than anything else, such as SSDs, EBS volumes, etc. Because of how we provision VMs for GitHub Actions jobs, we can pre-configure the VM with all of the bits it needs to create a RAM disk and mount it to the runner when a job starts.


Reserving up to 25% of memory for a RAM disk allows any GitHub Actions job that touches disk to be several orders of magnitude faster. The use cases for an Ultra Runner are vast, from heavy LLM/AI workloads, basic` npm install` operations, or even just running a` git clone` .


## How do I use Ultra Runners?


If you use any Depot GitHub Actions runner labels, you will take advantage of our new Ultra Runners. For example, if you use` depot-ubuntu-latest` , you're already using Ultra Runners.


### Do Ultra Runners cost more?


Nope. Our pricing for GitHub Actions runners applies to Ultra Runners. We're excited to bring this new performance improvement to everyone at no additional cost. Check out our[pricing page](https://depot.dev/pricing) for more details on per-minute prices.


## What's next?


This is a significant jump forward in making all of GitHub Actions faster. But our larger goal remains unchanged, make all builds exponentially faster. We have some upcoming announcements that are outside of our current build inputs that we can't wait to show you.


If you'd like to try out our faster GitHub Actions runners, you can[sign up for a free trial](https://depot.dev/sign-up) and get started in a few minutes. Hop into our[Community Discord](https://discord.gg/MMPqYSgDCg) and let us know what you think!


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
