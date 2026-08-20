---
schema_version: "1.0.0"
document_id: "ffd0ddcc1ddc22f75bb6c0a5043d30eeca0583a142e35d3e45775fb9247e9d43"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/faster-github-actions"
published_at: "2024-07-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:cd779eaf949798ad7c4904556c595b746633c5a2d03c5217234cdc0cd64fe9b5"
---

# Faster GitHub Actions with Depot

We've spent a large chunk of our careers building, testing, and deploying software using various tools and services. Whether it was working as application engineers owning the entire lifecycle of deployment. Or working as platform engineers tasked with building all the glue tools and providing fast, stable build infrastructure for organizations.


We've become intimately familiar with a range of CI providers, from Jenkins to GitHub Actions and everything in between. But along the way, we've seen most organizations settle into using GitHub Actions.


We've spent many hours optimizing, reoptimizing, and tinkering with various YAML configs in GitHub Actions to get builds to run faster. Yet, no matter what we tried, it always seemed like the tool getting in the way was Github Actions. The service was riddled with inefficiencies like per-minute billing, slow networks, slow CPU performance, strange cache limits, and outrageous pricing for larger runners.


## Why are GitHub Actions slow?


GitHub-hosted runners are notoriously slow and expensive. They use older CPUs, have slow network speeds, and are billed per minute. We believe that GitHub Actions can be exponentially faster, more reliable, and more affordable.


GitHub gives you runners with CPUs that have slow single-core and multi-core performance. The networks are slow as well, impacting cache performance, which is at ~145 MiB/s, and interacting with the internet at large.


You do have the option to self-host your own GitHub Actions runners. But this requires overhead to deploy, maintain, and scale them. It also means you have to keep them online and be on call for any GitHub outages as well as your own. It's an infrastructure and support burden that not all teams want to deal with.


To make matters worse, GitHub Actions are billed per minute, and build times are rounded up to the nearest minute. So, if your build takes 35 seconds, you pay for a full minute.


## Introducing Depot GitHub Actions runners


Having used GitHub Actions for several years, we've been just as frustrated as everyone else with all these pain points. So earlier this year, we built the solution we'd always wanted to use ourselves.


We built Depot GitHub Actions Runners to replace your existing GitHub-hosted runners. To do so, simply change the` runs-on` key in your workflow file.


Our runners consist of a few key elements that, when combined, make your GitHub Actions jobs faster, more reliable, and more affordable:


- Our runners run on top of faster 4th Gen AMD EPYC Genoa CPUs for Intel and AWS Graviton2 CPUs for ARM workloads, giving a 30% performance boost for both single-core and multi-core workloads
- We launch runners with access to 12.5 Gbps networks, giving you up to 1000 MiB/s of cache upload and download speeds
- We've optimized` actions/cache` and any other actions using GitHub Actions cache to work with our distributed cache architecture, giving you 10x faster cache speeds
- We've removed the need to self-host runners, giving you a fully managed service that's always online and ready to run your jobs, but if you want those runners in your cloud for data security, we can do that too
- No cache limits and no concurrency limits on the number of jobs you can run
- Our runners are half the cost of GitHub-hosted runners, and we bill by the second, not the minute, and don't enforce a one-minute minimum


Depot GitHub Actions runners are launched as full AWS EC2 instances with up to 64 CPUs and 258 GB of memory for both Intel & ARM. Our runners are fully compatible with your entire ecosystem of GitHub Actions tooling, including` actions/cache` ,` actions/setup-node` , and` actions/setup-python` .


Our runners come with faster CPUs, networking, and caching, all at half the cost of GitHub-hosted runners. To try them out for yourself, change the` runs-on` key in your GitHub Actions workflow file to a Depot label after you've connected Depot to your GitHub organization:


```text
name: Example workflow
on: push
jobs:
build:
-   runs-on: ubuntu-22.04
+   runs-on: depot-ubuntu-22.04
```


## One more thing...


We've already started iterating on our faster GitHub Actions runners to make builds even faster inside your existing CI jobs. We've launched our ARM runners, which you can use for any job that requires a native arm64 architecture for your build.


But we haven't stopped there. We've also launched I/O-optimized runners with local NVMe disks for jobs that need fast disk access. These runners are perfect for jobs that need to read and write a lot of data to disk. For *even faster* disks, we have ultra runners in beta that leverage a ramdisk underneath the hood, pushing your disk throughput up to nearly 20 GB/s.


## Join our community


Our goal with Depot is to become the fastest place to build software. We started by making Docker image builds up to 40x faster by creating a platform that leverages native CPUs, fast caching, and performance-tuned and secure builders.


We're now using that same platform and architecture to build faster GitHub Actions runners. These runners will have fast hardware, fast networking, and fast caching, all at half the cost of GitHub-hosted runners.


If you have ideas or things you'd love for us to build next, you can jump into our[Discord Community](https://discord.gg/MMPqYSgDCg) and chat with us directly. We're always looking for feedback and ideas to make Depot even better for you and your team.


If you want to try out faster GitHub Actions runners, you can[sign up for a free trial](https://depot.dev/start) and start running your GitHub Actions on Depot in less than 5 minutes.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
