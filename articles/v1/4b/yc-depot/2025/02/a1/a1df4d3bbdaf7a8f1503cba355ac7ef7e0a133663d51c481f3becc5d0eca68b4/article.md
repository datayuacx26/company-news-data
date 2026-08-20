---
schema_version: "1.0.0"
document_id: "a1df4d3bbdaf7a8f1503cba355ac7ef7e0a133663d51c481f3becc5d0eca68b4"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/github-actions-breaking-five-second-barrier"
published_at: "2025-02-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:b08e39839c6dadbb2fb40cff5a503d93c6e4ef2be9cf14f5d46b8124e993e546"
---

# How we launch GitHub Actions runners in 5 seconds

At Depot, we like to *get shit done* , and to that end, we’re constantly looking for ways to *make shit faster* to better enable you to deliver software. Over time, we’ve identified opportunities to reduce the time it takes for us to start running one of your GitHub Actions jobs.


In this post, we’ll walk you through what we’ve done to minimize the time it takes for our infrastructure to launch a job's host EC2 instance.


[Use Depot GitHub Actions runners to speed up your builds. Fast machines hosted in AWS, with up to 10x faster caching, and half the cost of GitHub-hosted runners. performance. Try free for 7 days. To get started, create a Depot account and then visit our docs.](https://depot.dev/start)


## What we need from a runner system


Building a system that starts jobs as quickly as possible isn't the only goal. It also has to:


- **Scale quickly** to handle bursts of demand.
- **Offer flexibility** , supporting different runner sizes and base images (e.g., Ubuntu 22.04 vs. Ubuntu 24.04).
- **Provide high performance** , meaning high IOPS, fast CPUs, and strong network and disk throughput.


As with any software design, balancing all of these requirements was a challenge. Make improvements to scalability, for example, and you may have to sacrifice some flexibility and performance. Ultimately, though, we’ve developed a solution that maximizes scalability, flexibility, and performance. Let’s dive into how we approached it.


## How GitHub Actions Runners work


The way that we interface with Github is pretty straightforward. At a high level, GitHub notifies us about job events through a webhook called` workflow_job` . This webhook tells us whether we need to:


- Start a new job.
- Mark a job as completed.
- Provide status updates on an in-progress job.


Our system takes this information and assigns the job to an` actions-runner` EC2 instance, which kicks off the work for a specific job id.


One interesting side note about this setup is that GitHub webhooks are “best effort”, meaning that they can be delayed by several seconds, or even fail to send entirely 😱


We don’t control that part of the system, though, so we focus on what we can control through our system: optimizing what happens *after* we receive the webhook.


## Sorting through potential solutions


Most technical problems can be solved in different ways, and so of course, we explored multiple options for launching these GitHub Actions jobs as quickly as possible. We’ll talk about how we developed the solution that we ended up on, as well as potential alternative solutions that haven’t panned out (yet!).


### A simple solution: boot up a new EC2 instance for every job


Let’s think about a solution that could meet a few of these requirements, while optimizing heavily for performance. One solution is to spin up a new EC2 instance each time a job arrives, assign the incoming job to that instance, and then terminate the instance when we receive the \`finished\` webhook from Github. This approach has a few advantages:


- It’s technically simple to implement.
- It’s relatively flexible, since each instance can be sized to handle different runner sizes and scaled to meet customers’ usage.
- It’s cost-efficient, as we only pay for compute resources while they’re in use.


There is a downside to this approach, though. Booting a new EC2 instance can be *slow* — it can take **up to 55 seconds** , in fact. For us, that makes this approach a non-starter, because again: Our primary objective is to *make shit faster* .


### A slightly better solution: maintain a single pool of EC2 instances


We want to improve the speed of the system, while keeping the current amount of flexibility we have. Starting an EC2 instance a second time is[typically much faster](https://depot.dev/blog/faster-ec2-boot-time#start-the-instance-once) , and so a better approach is to maintain a standby pool of pre-provisioned EC2 instances and assign jobs to these instances instead of booting new ones as jobs come in. This eliminates the long EC2 startup delay and improves response times by an order of magnitude, launching in just **5 seconds** .


This solution comes with its own major drawback, though: the base image of an EC2 instance is immutable, meaning that we aren’t able to change the image that an instance is using. For example, if we pre-provision EC2 instances with an AMI for Ubuntu 22.04, we can’t switch them to Ubuntu 24.04 on demand. This would force us to lock users into a single image type, which for us, isn’t acceptable.


### The Depot solution: multiple pools with pre-provisioned runners


We need to improve the flexibility of the current solution, while retaining the boot speed benefits that a standby pool provides. So, we thought: Instead of just keeping one standby pool, why not have one **for each AMI** ? That way, when a user requests a specific base image (e.g.,` depot-ubuntu-22.04` vs.` depot-ubuntu-24.04` ), we can start an instance in the appropriate pool and assign their job to it.


We take this even further and scale the size of the standby pool based on historical job patterns. For us, as an example, we notice fewer jobs during the evening PST hours, so we scale our standby pool down accordingly.


This hybrid approach provides a good balance between flexibility, cost-efficiency, and performance.


#### Why stop there?


When we think about optimizing startup times, we know we have to deal with the realities, constraints, and – yeah, we’ll say it – downright shenanigans around GitHub webhook behavior. We purposefully don’t strictly couple job requests to specific hardware to ensure resilience against delayed or missing webhooks. Instead, our system assigns jobs dynamically to available runner instances that best match the requested hardware.


### Dreaming about the future


As we mentioned earlier, we’re always looking for ways to make things faster. And so despite the improvements we’ve described here, we’re still brainstorming what else we can do to improve the start time of action runners.


One idea is to keep a small number of runners in each pool constantly up and waiting for tasks, and then route requests over to these “warm” action runners first. If none are available, we could subsequently try to route them to runners that are on standby. Technically-speaking, we could do this at Depot today (and we actually have a[similar system](https://depot.dev/blog/infrastructure-provisioner-v3) for container builders). The reason we don’t do this is that you’d need warm runners for each AMI pool, plus each size of instance, which can quickly increase the overhead costs.


You may be wondering why we didn’t use[Firecracker](https://firecracker-microvm.github.io/) VMs on a large metal host. While this would significantly improve boot speeds, CI jobs typically require significantly higher IOPS and throughput than a shared Firecracker instance can provide. However, there *are* ways that we could make it work.


The main issue when it came to using firecracker VMs on a large metal host was that IO was really slow under higher workloads, making it less than economical compared to using EC2. However, we can avoid that problem altogether by using ramdisks when writing to the root volume. That way, we can keep the noisy neighbor problem to a minimum, while still getting the fast boot times of Firecracker VMs.


Ramdisks are the technology behind our Ultra Runners that[we are running today](https://depot.dev/blog/introducing-github-actions-ultra-runners) , which we weren’t doing when we first experimented with Firecracker, so we’d like to revisit using Firecracker VMs at some point.


## More ways we speed up GitHub Actions


When you run a GitHub Actions (GHA) job, speed matters. The faster your CI/CD pipeline runs, the quicker you get feedback on your changes, and the faster you’re able to deliver software to the folks who use it. In addition to optimizing our runner system, we employ several other techniques to accelerate CI/CD pipelines. You can check out resources around those other techniques here:


- **GitHub Actions Cache Optimization** –[Read more](https://depot.dev/blog/github-actions-cache)
- **Queue Time Reduction with Cached Schemas** –[Read more](https://depot.dev/blog/reducing-queue-time-with-cached-schemas)
- **RAMDisk-Backed Ultra Runners** –[Read more](https://depot.dev/blog/introducing-github-actions-ultra-runners)


Billy Batista


Software Engineer at Depot
