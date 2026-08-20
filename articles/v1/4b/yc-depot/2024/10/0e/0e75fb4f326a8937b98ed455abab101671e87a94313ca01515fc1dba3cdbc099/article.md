---
schema_version: "1.0.0"
document_id: "0e75fb4f326a8937b98ed455abab101671e87a94313ca01515fc1dba3cdbc099"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/mac-github-actions-runners"
published_at: "2024-10-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:26378b62aa60efc04e283cb36977e24ee9afb996cc5dce18b2c8afde07cd1136"
---

# Now available: macOS GitHub Actions runners

Since launching[Depot GitHub Actions runners](https://depot.dev/blog/depot-github-actions-runners) in March, the most requested feature has been macOS runners.


GitHub Actions performance and cost are a massive pain point across the developer ecosystem. We opted back in March to start bringing the performance of Depot to all of GitHub Actions with our runners, and we're excited to extend that offering with our own macOS runners now available on Depot!


## Introducing macOS GitHub Actions runners on Depot


With macOS runners on Depot, you can now run your macOS builds on Depot's infrastructure for faster builds, with 10x faster caching, and at half the cost of GitHub-hosted runners. We also don't round your build minutes up, so a 31-second build isn't counted as a minute until you do another 29 seconds of work.


### Better macOS runners at half the price


We are launching macOS runners with M2 chips and macOS 14. A macOS runner on Depot:


- Comes with 8 CPUs, 24 GB of memory, and 150 GB of disk space
- Comes with a disk provisioned with 12000 IOPS and 1000 MB/s throughput
- Costs $0.08 per minute, half the price of GitHub-hosted macOS runners


These runners are currently only available on our[Startup plan](https://depot.dev/pricing) and above. We are hoping to bring them to all plans in the future once we have additional capacity.


You can launch macOS runners on Depot with the[following labels](https://depot.dev/docs/github-actions/runner-types) :


Label CPUs Memory Disk size Per-minute price


` depot-macos-latest` 8 24 GB 150GB $0.08


` depot-macos-14` 8 24 GB 150GB $0.08


## What's next?


We're still considering the first version of macOS runners on Depot as **beta** for the moment while we monitor queue times and bring additional capacity online. So you should expect queue times to improve over time as we balance capacity with demand.


We're also working on general performance improvements for **all runners** that we will launch in the coming weeks. We're excited to share more details about that soon!


If you're interested in trying out Depot,[start a free trial](https://depot.dev/sign-up) and see how much faster & cheaper we can make your builds! If you have questions or just want to share your feedback, jump into our[Community Discord](https://discord.gg/MMPqYSgDCg) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
