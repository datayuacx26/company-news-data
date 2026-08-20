---
schema_version: "1.0.0"
document_id: "3e80fe6fb93a71e748fbf58a456f88f088d75fc380c952d3f138b40ec5247f63"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/infrastructure-provisioner-v3"
published_at: "2023-10-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:74775712e46d60262359ed5b92847483f5b79707e3c8e0870bed906330332b6d"
---

# Reducing time-to-start

One of Depot's core health metrics is **time-to-start** , defined as the time from when a user decides they would like to build their application with Depot to when the build has started on a machine. Reducing this time is critical for us to deliver an extraordinary experience; if this time is too long, users will leave.


Note that this metric is separate from making the build itself faster. Depot is heavily optimized around delivering fast compute, filesystem, and cache performance. But this performance is only useful if we can deliver it when requested.


This week is our second drop week, and alongside the new features we're announcing, we also wanted to share an update on how we're thinking about time-to-start and what we've been working on to improve it.


[It's the fourth day of Drop Week #02 — check back tomorrow for more](https://depot.dev/drop-week)


## Steps to a running build


We can divide the steps involved in starting a build into the following:


1. The user runs` depot build` on their machine or in CI
2. The Depot CLI sends a request to the Depot API to start a build
3. Our infrastructure provisioning system assigns an EC2 instance to the build
4. Once the machine is ready, the API communicates connection details for the assigned instance to the CLI
5. The CLI connects to BuildKit running on the instance and begins the build


Each of these steps is a potential source of delay, with the most significant being step 3: the time it takes to acquire an EC2 instance actively running BuildKit.


We've recently deployed our third generation of infrastructure provisioning, which has significantly reduced overall time-to-start. 🚀


## Depot infrastructure


With Depot, we run project builds in dedicated EC2 instances — these EC2 instances are never shared between projects and are destroyed when unused. This single-tenant model is crucial for security, as due to the nature of container builds, it's very difficult to provide strong isolation between builds running on the same machine.


Running each project in a dedicated EC2 instance is also important for high-performance: there are no extra layers of virtualization or containerization between the build and the raw performance of the host machine, and we're able to deliver significant performance gains by using the latest compute-optimized Intel and Arm instance types from AWS.


However, the most significant drawback to this model is actually time-to-start, since booting new EC2 instances can be rather slow.


### v0 with Fly Machines


The original version of Depot was built on top of[Fly.io Machines](https://fly.io/docs/machines/) — Fly provides an API on top of Firecracker-based VMs, running on bare metal servers, which we used to run BuildKit on-demand. When a build request arrived, we would send a request to the Fly API to boot a new machine, wait for that machine to be ready, and then inform the CLI that the builder was ready to start. Fly's boot performance was excellent, usually starting the machines in just a few seconds.


### v1 with AWS


However, very early on we realized we needed to support multi-platform container builds, specifically for Arm architectures (at the time, I was using an M1 MacBook and couldn't use Depot-built containers on my machine). So, while we loved the boot performance Fly provided, we decided to move to AWS, to support Arm builds with Graviton instances.


Like the Fly.io architecture, the very first version of the AWS architecture also created a new instance for each new build request. This was functional, but *very* slow, as it can take a while for a new EC2 machine to boot.


There are two factors to boot time in EC2:


1. The time from calling` RunInstances` to the time when the kernel begins to boot
2. The time from when the kernel begins to boot until all services are initialized and the instance is ready to accept connections


From our observation, it takes around 5 seconds from calling` RunInstances` until the kernel begins to start — sometimes faster, but generally around 5 seconds for the types of instances we launch (` c6i.4xlarge` and` c7g.4xlarge` in` us-east-1` and` eu-central-1` ).


The second factor of the kernel and service initialization heavily depends on what operating system you're running, what system services are enabled, and the application services themselves.


There's some[benchmarking from 2021](https://www.daemonology.net/blog/2021-08-12-EC2-boot-time-benchmarking.html) ([HN discussion](https://news.ycombinator.com/item?id=28199994) ), evaluating how different operating systems affected boot time, that concluded that Clear Linux was the fastest, potentially booting in as little as 1.23 seconds. Debian 10 and Amazon Linux 2 booted in around 10-11 seconds.


Our testing also confirmed the Clear Linux result. However, we were unable to use Clear Linux as our base image since we need to support both Intel and Arm architectures, and Clear Linux (made by Intel) only supports Intel.


Ultimately, we chose Amazon Linux 2, with around 35 seconds from the time we issued the` RunInstances` call until BuildKit was ready to accept connections.


### v2 with AWS


35 seconds is too slow to wait for a build to start — ideally, builds start in under 5 seconds.


To make 5-second starts possible, we introduced a "warm pool" into our infrastructure provisioner architecture. This a group of EC2 instances that are always running, waiting to be assigned to a build. If a warm instance is available, then the provisioner can assign new builds to that instance immediately, eliminating the need to wait for a full cold boot.


The warm pool successfully reduced time-to-start for most builds to under 5 seconds, and this architecture has powered Depot over the large part of the last year.


However, there are two main issues with the warm pool:


1. If there's no warm instance available, then the build has to wait the full 35+ seconds for a cold instance to boot fully
2. Running a warm machine is much more expensive than the eventual time it will spend running builds


This means there's a trade-off between cost and performance: we could either run more unnecessary instances to cover higher bursts of build requests, or run fewer but have a higher chance that builds would fall back to cold instances during peak traffic times.


### v3 with AWS


To improve this situation, we added a new "standby pool" to our provisioner architecture. This is a group of EC2 instances that boot once, perform the initial kernel, system service, and application service initialization, then shut down and remain in a stopped state.


Then, when build requests arrive, if we don't have an existing warm machine that we can assign to the build, we pull a standby machine instead.


The standby machine still needs to be started, but critically, it does not need to perform initial kernel and service initialization, so the overall is much faster, potentially **less than 5 seconds** from when the kernel begins to boot.


This means that builds that are unable to be assigned to a warm machine don't need to wait for a cold boot, reducing the potential variance in time-to-start. But also, we can feed the warm pool with machines from the standby pool, which reduces the window of time between when a warm instance is claimed and another is started in its place.


Deploying the standby pool has resulted in more consistent performance; this graph shows the time to acquire a running machine for Intel and Arm builds, for both warm and cold machines (note that builds do not fall back to cold machines anymore):


## Application UX: another time-to-start


Time-to-start applies to more than just infrastructure. We've also been working on the user experience of even beginning to use Depot at all.


From the time someone signs up for a Depot account, there's a series of steps they must perform before they can run their first build:


1. Create an organization
2. Create a project
3. Install the Depot CLI
4. Authenticate
5. Initialize a project
6. (finally) Run a build with` depot build`


We've been working to reduce the number of steps required to actually run` depot build` and have made a few key changes:


- Account setup is simpler, with simplified UI for creating organizations and projects that eliminates several multi-step flows
- We added an "empty state" for projects to show how to install the Depot CLI and run the first build
- The` depot build` command now automatically prompts for authentication or project details if needed


With the CLI changes, our goal is that you should be able to run` depot build` and actually get a build, no matter what state you are in.


## What's next


We still have more work to do! Especially around app onboarding and the CLI experience, we'd like to surface better contextual help and make the docs more interactive and organized.


And for build infrastructure, we're also exploring some changes to BuildKit to effectively disable the parts of it that Depot does not use to improve startup time.


If you're interested in trying Depot yourself, feel free to[create an account](https://depot.dev/start) . (and let us know how you find the new onboarding flow!)


Jacob Gillespie


CTO & Co-founder of Depot
