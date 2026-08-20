---
schema_version: "1.0.0"
document_id: "2cf70cf9dca9bbd906fad78872a76240777f6c9b2861207ca9303acdf02026a4"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/windows-github-actions-runners"
published_at: "2025-06-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:7aa19de847dffbf20c36533cc5339778a498a631c82f08505a08996791d2d47a"
---

# Now available: Windows GitHub Actions runners

We're excited to launch Windows runners for Depot GitHub Actions across all plans! Cut your Windows CI costs by 75% while speeding up your builds by running Windows workloads on Depot with the speed and reliability you expect, like single-tenant runners, 10x faster caching, unlimited concurrency, per-second billing, and real-time analytics.


## Why Windows GitHub Actions runners have been holding teams back


Let's be honest about GitHub-hosted Windows runners. They're slow, expensive, and, frankly, kind of a mess. Windows runners are notoriously slow to start up. We're talking **2 to 5 minutes** just to initialize, depending on the runner size. That's a far cry from the quick startup times you get with Linux runners.


The cost impact is even more brutal. GitHub's Windows runners start at **$0.016 per minute** , and with GitHub's rounding up to the nearest minute, even a 31-second build counts as a full minute of usage. For teams running frequent Windows builds, those costs add up fast.


Beyond the speed and cost issues, teams consistently report flaky network connections, underpowered resources, and inconsistent performance for identical workflows. These reliability problems make Windows CI/CD pipelines frustrating and unpredictable.


That's exactly why we built Depot's Windows runners to solve these widespread pain points that have been slowing down folks using Windows runners.


## Introducing Windows GitHub Actions runners on Depot


We're bringing the performance and reliability you know from Depot to your Windows builds. Our initial optimizations mean your jobs should typically be running within **30 to 60 seconds** instead of waiting around for minutes, and we're working to drive that down even further.


Our Windows runners use Intel chips running **Windows Server 2022** , and our smallest runner costs **75% less** than GitHub's default Windows runner.


You can find the full list of runner types in our[runner types documentation](https://depot.dev/docs/github-actions/runner-types) , but here are the Windows runner specs:


Label CPUs Memory Disk size Per-minute price


` depot-windows-2022` 2 8 GB 100 GB $0.008


` depot-windows-2022-4` 4 16 GB 130 GB $0.016


` depot-windows-2022-8` 8 32 GB 150 GB $0.032


` depot-windows-2022-16` 16 64 GB 180 GB $0.064


` depot-windows-2022-32` 32 128 GB 200 GB $0.128


` depot-windows-2022-64` 64 256 GB 250 GB $0.256


### Limitations of Windows runners on Depot


While we are excited to bring Windows runners to Depot, there are a few limitations to be aware of:


- **No disk accelerator** : Unlike our Linux runners, our Windows runners do not currently have a disk accelerator (i.e.,[Ultra Runners](https://depot.dev/blog/introducing-github-actions-ultra-runners) ). They have faster CPUs, networking, caching, and all of the other performance enhancements of Depot. We will be looking at adding Ultra Runners to Windows in the near future.
- **No Hyper-V support** : Windows runners on Depot do not currently support Hyper-V due to a limitation with EC2. This means that you cannot run Windows containers or use Hyper-V features in your workflows. We are looking at adding this support in the future.


## What's next?


Windows runners are now officially generally available on any Depot pricing plan! We're excited to bring Depot performance to GitHub Actions runners for Windows. Over the coming weeks, we will be working on additional performance enhancements to drive boot times down even further and incorporating disk acceleration into Windows runners as well.


If you're interested in trying out Depot,[start a free trial](https://depot.dev/sign-up) and see how much faster & cheaper we can make your builds! If you have questions or want to share your feedback, jump into our[Community Discord](https://discord.gg/MMPqYSgDCg) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
