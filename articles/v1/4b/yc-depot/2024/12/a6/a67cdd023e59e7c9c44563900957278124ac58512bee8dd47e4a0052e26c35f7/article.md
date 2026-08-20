---
schema_version: "1.0.0"
document_id: "a67cdd023e59e7c9c44563900957278124ac58512bee8dd47e4a0052e26c35f7"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/container-builds-incident"
published_at: "2024-12-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:65ebb8c94803ab9ae16632566f7a12e2a345cb7816401914bc44b5ba3bac3c96"
---

# Knowledge share: Insights from our container builds outages at the start of December

Since we started building Depot, we've been relentlessly focused on build performance and stability. Our goal is to make your builds stupid fast and have you never worry about them. We think we've been doing pretty great at that, but we're not perfect. We had a couple of outages last week that took our container builds offline twice. We've learned a lot from these incidents and wanted to share some insights with you.


## What happened


Two incidents last week disrupted our build service for container images. The first incident started in the early morning Pacific Time on December 2nd, the first Monday back from the US holiday. The second incident began in the late afternoon Pacific Time on December 5th. Both incidents were isolated and had different root causes, as we will see below.


### Incident 1: December 2nd


[Incident from status.depot.dev](https://status.depot.dev/cm47exrcr0002ymvcl6rx25fz)


We received alerts in the early morning hours about a spike in build requests failing to acquire builder machines. Our internal systems alert us any time we can't assign a builder within 5 minutes. We immediately responded to the alert and started digging into a potential problem in our provisioning system.


At the moment, our provisioning system for container builds is separate from our provisioning system for GitHub Actions runners. This is primarily a legacy reason, as our container build orchestration predates our GitHub Actions runners. That said, the two systems live in the same AWS account and within the same VPC. We've leaned in this direction to optimize the network links between your Depot GitHub Actions runners and our container image builders. The closer they live to each other, the better the network performance and the faster your builds.


What was interesting about the increase in failed container build requests was that the load was not out of the ordinary. If anything, the container build system was underutilized at the time. Nevertheless, the provisioning system was unable to launch new builders. After some digging, we discovered it was hitting an AWS API rate limit for launching new EC2 instances, specifically the` StartInstances` API call.


The` StartInstances` API call is used to launch new EC2 instances in AWS. We use this API call to launch both new builders for container image builds and runners for GitHub Actions. The rate limit for this API call is based on the region and account. We were hitting the rate limit for the` StartInstances` API call in the us-east-1 region.


Once we discovered the rate limit was being hit, we opened a support case with AWS to increase the rate limit for the` StartInstances` API call. But, in the meantime, we needed to mitigate the issue. We started looking at where we could reduce our rate of calls to` StartInstances` . This led us to discover a bug in the new provisioning system that backs our GitHub Actions runners.


That system relies on the concept of *stacks* , where a stack is the underlying infrastructure for a new GitHub Actions runner. A stack moves through a state machine, first entering our standby pools where the stack is warmed to load the entire GitHub Actions runner AMI onto disk for fast boot times. The stack is then moved into a stopped state. When a request for a job comes in, we grab a warmed stack from standby, start the stack, and assign the job to it.


The bug we discovered was in the *start the stack* phase. Because we were being rate limited, stacks would fail to start because of the rate limit on` StartInstances` . This effectively marks the stack as errored and we would retry the launch in a loop, consuming any additional API limit that was remaining. So we changed the behavior to treat the errored stack as unrecoverable and tear down the stack because our logic would automatically find a new stack to start. This slowed down the rate at which we were calling` StartInstances` for failed stacks, freeing up additional capacity for the container build system.


This partially restored the container build system, but we weren't out of the woods just yet.


At this time, we alerted folks to switch their builds over to our EU Central region, where we still had headroom in our AWS rate limits. This allowed us to provision builders in the EU Central region and get builds running again, albeit with a fresh eu-central layer cache. This further reduced the load on that API and restored another chunk of builds. With the symptoms addressed, the system had now stabilized and we could work on addressing the root cause.


The root cause of the outage was a combination of factors:


1. The rate limit for the` StartInstances` API call in the us-east-1 region was hit. This was due to a spike in demand for GitHub Actions runners coming back from the holidays.
2. A bug in our provisioning system for GitHub Actions runners was causing failed stacks to consume additional rate limit capacity unintentionally.


These two factors combined consumed any remaining rate limit capacity for the container build system, causing the system to be unable to launch new builders.


#### So how did we fix it?


We deployed two mitigations in addition to working with AWS to get better rate limits for our use case.


1. We decreased the number of API requests to the` StartInstances` API call for GitHub Actions runners for failed stacks. This allowed us to partially restore the container build system by freeing up some rate limit capacity.
2. We fixed an issue in our legacy provisioning system that was not batching launch requests. This was causing the legacy provisioning system to consume more of the rate limit than necessary. By batching the launch requests, we were able to reduce the number of API requests being made to the` StartInstances` API call for container builds.


These two mitigations allowed us to fully restore container image builds in the US East region. Once those mitigations were deployed, we worked with AWS to adjust our rate limits to allow us to launch more instances per second.


#### Future looking


This incident highlighted that while having the two products live next to each other in the same AWS account and VPC has network latency benefits, it has an increased blast radius for AWS account level limits. We're looking at ways we can prevent that in the future, such as moving the container build system to a separate AWS account. But ultimately, we want to unify the two provisioning systems to make that transition smoother.


### Incident 2: December 5th


[Incident from status.depot.dev](https://status.depot.dev/cm4buf28m0005arzia7lj5h2s)


The second incident started in the late afternoon Pacific Time on December 5th. We were alerted to another spike in failed-to-acquire builder machine errors. This time, the root cause was not API rate limits but rather an issue with a new beta feature we've rolled out to Business plan customers called[Build autoscaling](https://depot.dev/docs/container-builds/overview#build-autoscaling) .


By default, all container builds for a Depot project get routed to a single builder + its cache for the requested architecture. This has been the way since 2022, when we first launched Depot. But we've been working on a new feature that allows you to scale your builds across multiple builders. This feature is called Build autoscaling.


Build autoscaling allows you to set the maximum number of concurrent builds a builder should handle. Once you reach that maximum, Depot will automatically provide a new builder to handle the next build. The next builder gets a recent copy of the cache, and away you go. We've had this feature in beta for a while and have begun rolling it out to several Business plan customers.


On December 5th, an issue with the Build autoscaling feature at high volumes caused our internal cache cluster to become overwhelmed with requests for cloning the cache (our way of ensuring you get a recent copy of your cache on a new builder). This caused the cache cluster to become unresponsive and unable to serve cache requests. Ultimately, this results in builds being unable to start because they couldn't attach their previous cache.


#### So how did we fix it?


To bring builds back online, we disabled the build autoscaling across Depot to restore builds. This worked almost immediately but impacted the build performance of folks leveraging this new beta feature. We then worked to scale up our cache cluster to handle the increased load from build autoscaling and re-enabled the feature. Full build performance was then restored.


#### Future looking


While this feature has been in beta for a while, this was the first time we had seen hundreds of concurrent builders for a single project based on the horizontal scale-out configuration for some folks. This had an unexpected performance impact on our cache cluster.


Looking forward, we're working on adding protections to our provisioning systems that will act as safeguards to maintain availability for all builds in unexpected usage scenarios like we saw. We're also working on several Docker layer cache optimizations that'll make builds faster and remove the capacity limitations of our cache cluster.


## Conclusion


This is the first time we've dived deep into some of Depot's operations. We value transparency and wanted to share some of the insights we've learned from these incidents. We're sorry for the inconvenience these incidents caused, but please know that we're constantly working to make Depot better and more reliable.


A special thank you to our growing team for their hard work in resolving these incidents. Thank you for your rapid response Jacob, Goller, Billy, and Luke. Outages are never fun but they're a great opportunity to learn and grow.


If you're not already, we recommend subscribing to our[status page](https://status.depot.dev/) to get real-time updates on incidents and maintenance.


If you have any questions about the events or how we are working to prevent them in the future, please reach out to us in our[Discord Community](https://discord.gg/MMPqYSgDCg) . We are always happy to chat.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
