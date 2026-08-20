---
schema_version: "1.0.0"
document_id: "8522e6ec2d6dcfec31bb44af741bb211762dd26aca217b30fffa2ed53a02dbcc"
company_key: "yc-starsling"
company: "StarSling"
source_id: "yc-starsling-atom-ebb22850b8ad"
canonical_url: "https://starsling.dev/blog/beyond-cold-starts-benchmarking-sandboxes-for-real-workloads"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-23T20:09:12.458496+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:4364782544f3cf6e288020e0b280049c4725afb464f0f886713a3958a3a88add"
---

# Beyond Cold-Starts: Benchmarking Sandboxes for Real Workloads

**TL;DR:** After running 500K+ sandbox jobs in prod, we've[open-sourced benchmarks](https://github.com/starslingdev/hpc-sandbox-benchmarks) for cloud sandbox providers using real-world CI workloads. For our workloads, sandboxes from Blaxel and Daytona's microVMs have excellent performance.


Total completion time for Better-Auth's own CI task matrix, run cold on twelve fresh sandboxes per provider.


Over the past month, we've migrated 100% of StarSling compute load from AWS onto three microVM sandbox providers.


When we[launched StarSling Runners](https://starsling.dev/blog/starsling-runners-are-now-generally-available) , our architecture used high-performance EC2 VMs to run CI jobs and enable agents to continuously ship optimization PRs for our customers. This approach is tried and true, but maintaining a pool of fast-booting VMs from EBS required infrastructure upkeep that took our focus away from features.


We decided to migrate to managed microVM providers, to offload the generic compute infrastructure. Today, we're sharing[hpc-sandbox-benchmarks](https://github.com/starslingdev/hpc-sandbox-benchmarks) , our benchmarks for identifying the highest performance sandboxes for real development work.


## Why another benchmark?


There are lots of publicly available benchmarks for sandbox creation latency or concurrency. We are fans of the continuous startup benchmarking done by ComputeSDK. However, we couldn't find a reliable benchmark that actually measured sandbox performance for **real workloads** .


The problem is that a sandbox that starts in 300 ms but takes an extra minute to install dependencies is not faster for our CI/CD workflows. We wait for dependencies to install, artifacts to build and tests to complete.


We wanted to answer a more useful question:


**Which sandbox actually finishes real tasks fastest?**


This is the work that our coding agents actually do: clone, install, lint, build, test, and move data.


## What we found


### Real workloads disagreed with synthetic benchmarks


Real development tasks are systems workloads. A dependency install or build combines CPU, storage, memory, filesystem metadata, networking, and virtualization overhead. Raw performance on one synthetic dimension can hide subtle bottlenecks somewhere else.


The sandbox with the fastest raw CPU, disk, or memory won't necessarily finish every CI task first.


The provider with the fastest CPU (Blaxel) will not consistently finish all CI first. Initially, we assumed this was randomness due to noisy neighbors but the results have been consistent.


### Disk Metadata I/O mattered more than we expected


Most CI jobs start with checking out a codebase and installing the dependencies for a repo. The later step creates, reads, hardlinks, and inspects huge numbers of small files. That makes random I/O and filesystem metadata performance much more important than headline sequential throughput.


Filesystem metadata throughput (hardlinks, small-file operations) tracks cold-install time better than headline sequential throughput.


The raw disk throughput can look excellent in` fio` and still perform poorly during` pnpm install` , TypeScript compilation, or package graph construction that do more than just random writes.


### Prefer microVM over gVisor-based sandboxes


Modal's default gVisor-based sandboxes had the worst performance for all configurations currently tested by hpc-sandbox-benchmarks. Also, the gVisor-based Modal sandboxes showed much higher variance.


Across identical Modal configurations, the new microVM beta outperforms gVisor on real-world and synthetic benchmarks.


After sharing initial results with the Modal team, they opened a PR to enable benchmarking Modal microVM sandboxes (currently in beta). We recommend focusing on sandbox providers with microVMs, unless your workload is purely compute and infrequently makes Linux syscalls.


## How to choose a sandbox provider


For engineers or CTOs considering sandbox providers, we recommend that you:


1. Run your actual workflows.
2. Measure end-to-end completion time.
3. Compare variance and failure rates.
4. Use synthetic benchmarks to explain the result.


There is no universal fastest provider. The right choice depends on the workload.


For CI and coding agents, prioritize:


- real repository execution time
- cold dependency-install performance
- filesystem responsiveness
- runtime consistency
- workload completion rate


For highly interactive workloads, startup latency deserves more weight. But even there, it should be measured as one part of the full user experience rather than treated as the product.


## Reproducibility and limitations


The[hpc-sandbox-benchmarks](https://github.com/starslingdev/hpc-sandbox-benchmarks) repo reflects the provider infrastructure, hardware, regions, and pricing available when the openbenchmarking.org compatible tests were run.


It does not yet measure:


- large-scale provisioning latency
- scaling behavior
- regional capacity
- GPU workloads


We plan to publish the raw results, workload definitions, benchmark harness, aggregation logic, and visualization code so providers and developers can reproduce the tests and challenge the methodology.


## The takeaway


Sandboxes should be benchmarked like developer infrastructure: measure the actual work being done.


If you're still paying for GitHub Actions, we'll speed up your runs, optimize your workflows, and save you money. Get self-driving CI at[starsling.dev](https://starsling.dev/) .
