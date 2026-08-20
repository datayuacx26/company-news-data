---
schema_version: "1.0.0"
document_id: "6ffd685661bd5dfe9f84bcf160220a87acd93648a4a2d840833bc22d76aadbea"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/ultra-runners-for-macos"
published_at: "2025-02-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:63bf2296c0bde9c24b6affc469cdb5239235b18bd373eb17205c04be28732435"
---

# macOS GitHub Actions Runners are now twice as fast

Last October we announced[Depot Ultra Runners](https://depot.dev/blog/introducing-github-actions-ultra-runners) , an upgrade to our GitHub Actions runners that makes builds up to 3x faster by accelerating disk I/O performance using a RAM disk.


As of today, disk I/O is now **two times faster than before** for Depot macOS Actions Runners! This performance upgrade is automatically applied to all jobs using the` depot-macos-14` label.


[Build faster. Waste less time. Depot accelerates Docker image builds and GitHub Actions workflows. Easily integrate with your existing CI provider and dev workflows to save hours of build time. Get Started →](https://depot.dev/sign-up)


## How do Ultra Runners work?


Ultra Runners accelerate disk I/O performance by reserving a portion of the runner's memory to act as a disk accelerator. This memory acts like a transparent RAM disk, accepting reads and writes for disk operations first before the underlying root NVMe disk.


This allows file operations to take advantage of the blazing fast throughput and IOPS that memory provides, achieving speeds much quicker than would be possible with any physical disk. This has a large impact on build times, especially in CI workflows that clone repos, install dependencies, compile code, etc.


## What changed for macOS Ultra Runners?


Depot macOS Actions jobs are processed by dedicated physical macOS hardware, hosted in Amazon Web Services. When a job is created, we assign the job to one of these physical hosts, which creates an ephemeral VM for that job's runner using Apple's` Virtualization.framework` . The job is processed in that VM, and once complete, the VM is destroyed.


Previously we relied on the host macOS operating system page cache to cache portions of the VM's disk image in memory. This allowed the VM to access disk I/O performance at speeds faster than the host's physical disk. However, this method was not as fast as we wanted it to be, and we saw an opportunity to greatly improve the efficiency of the disk cache.


We implemented our own **custom block device** to act as the VM's disk - this gave us direct control over which blocks were cached in memory and when they were evicted.


## Performance impact


By taking direct control of the disk cache at the block device layer, we were able to achieve more than a **2x performance improvement** for macOS Ultra Runners disk performance!


As an example, a test workflow that used` actions/cache@v4` to restore 1GB of` node_modules` dependencies, an IOPS-heavy workflow, previously took 17 seconds to restore, but now takes only 8 seconds. 🚀


## How do I use Ultra Runners?


The new block device has been deployed to all Depot macOS runners, so simply using the existing` depot-macos-14` or` depot-macos-15` label in your workflow will automatically take advantage of the new performance improvements:


```text
jobs  :
build  :
runs-on  :   depot-macos-15
steps  :   ...
```


If you'd like to try out our faster GitHub Actions runners, you can[sign up for a free trial](https://depot.dev/sign-up) and get started in a few minutes. Hop into our[Community Discord](https://discord.gg/MMPqYSgDCg) and let us know what you think!


Jacob Gillespie


CTO & Co-founder of Depot
