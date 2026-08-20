---
schema_version: "1.0.0"
document_id: "013bec1e58c9d9c206dd3f5db3a1eef7a1192cbcf158bdc41bf4bcc5c343e852"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/october-20-us-east-1-outage"
published_at: "2025-10-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:55:32.133421+00:00"
content_hash: "sha256:f9a5ee55b0125f7608a2d8da30e7dac1186e98b9dd12b0f4bbfc1d7344bf8375"
---

# Knowledge share: Insights from the us-east-1 outage on October 20th

On October 20th, 2025, AWS experienced a major outage in their us-east-1 region that affected several core services, including DynamoDB, EC2, IAM, Console access and many others. This outage had a significant impact on Depot, as we run our primary infrastructure in` us-east-1` and utilize several key services, including EC2, DynamoDB, SQS, and Lambda.


Before we begin, we want to acknowledge the impact this had on folks who rely on Depot for their builds and GitHub Actions runners. We know many of you were impacted by this outage in ways that were frustrating and disruptive. We sincerely apologize for the disruption this caused.


## A few elements to understand about Depot's architecture


Before diving into the details of the outage and how we responded, it's important to understand a few key elements about Depot's architecture that influenced how we were affected by the AWS outage.


Our provisioning system for GitHub Actions runners relies heavily on Dynamo, SQS, and Lambda to manage the lifecycle of EC2 instances that are spun up dynamically to service every job request. We run a set of shadow runners per GitHub org connection and store their state in Dynamo. When a job is requested, we place a message on an SQS queue, which is then processed by a set of Lambda functions that read from the queue, check DynamoDB for the runner's state, and make EC2 API calls to launch new hosts as needed. Once the job is complete, we delete the EC2 instances and update the runner state via our control plane. That instance is then never used again.


Container build provisioning works in a similar way but without the need for or dependencies on Dynamo directly, as builds are launched directly via our control plane via the EC2 API. But the process still relies on being able to get a fresh EC2 instance launched to service the build request and then we tear it down when complete. Again, never to be used again.


Depot Registry today relies on ECR as the backing store for container image manifests and Tigris behind our global CDN to serve the actual layer blobs. Effectively, today, a registry is tied directly to a container build project. Meaning, that if your project is in` us-east-1` , half of your registry API lives in` us-east-1` and the other half lives globally behind our CDN backed by Tigris. So we have to be able to call ECR to get manifests for images and thus determine which layer blobs to load from Tigris.


## How the outages unfolded


We have several monitors and alerts set up to notify us when container builds or GitHub Actions jobs fail to launch across our platform. In addition, we have alerts for high queue times, errors in our provisioning system, and other key metrics that help us understand the health of our systems.


You can find the status page timeline of the incident[here](https://status.depot.dev/incidents/01K807WKQ57RH1VS529GWCSNR1) . The timeline below summarizes the key events during the incident.


**At 12:00 AM PT** , we began receiving alerts that container builds and GitHub Actions jobs were failing to acquire hosts from our provisioning system for a large percentage of customers. We began investigating the issue. By 12:05 AM PT, we had determined that the bulk of the errors were coming from our` us-east-1` region, with secondary errors also from` eu-central-1` for a chunk of IAM-specific errors.


**At 12:11 AM PT** , AWS opened its first incident regarding error rates and latencies in` us-east-1` . With that incident, we were able to correlate the issues we were seeing with the larger AWS outage.


**By 12:45 AM PT** , we took stock of our systems and determined that we were unable to connect to any DynamoDB tables in` us-east-1` , a core component of our provisioning system. Additionally, our provisioning system was unable to process messages from SQS queues because the event triggers between Lambda and SQS were not firing. Any container build or GitHub Actions job requests that were in flight were failing to acquire hosts because EC2 API calls were returning errors or capacity issues.


**At 1:00 AM PT** , we determined that any GitHub Actions job, container build, Depot Cache request, Depot Registry request for projects in` us-east-1` were all failing to process. We effectively had no network connectivity to any core AWS services in` us-east-1` , including DynamoDB, SQS, EC2, ECR, and IAM.


**By 2:20 AM PT** , we began to see recovery in` us-east-1` as AWS restored connectivity to DynamoDB. We were able to resume launching instances via EC2 to serve some container image builds, but capacity remained limited. GitHub Actions jobs were still failing to launch because SQS to Lambda event triggers were still not functioning, preventing our provisioning system from processing job requests. ECR requests began succeeding with some rate limiting, allowing some Depot Registry requests to succeed again.


Without event triggers between SQS and Lambda functioning, our provisioning system could not process any GitHub Actions jobs. We also couldn't fail any of those queued requests to another region, as that is work that GitHub Actions is waiting for a runner to complete. We had to wait for AWS to restore this functionality.


**At 3:50 AM PT** , we started to see some recovery in event triggers between SQS and Lambda, but only a few messages were being released. Our provisioning system began processing the backlog of GitHub Actions jobs that had accumulated during the outage. However, the backlog was significant, and the sheer volume of jobs attempting to launch simultaneously was triggering GitHub rate limits for some customers.


**By 4:30 AM PT** , the backlog of GitHub Actions jobs had started to process, but we were still seeing rate limits from GitHub for some customers due to the high volume of jobs trying to launch simultaneously. We were also continuing to see capacity errors in EC2 as we tried to launch a large number of instances simultaneously to process the backlog and service container image builds.


**At 4:54 AM PT** , we observed full recovery of our API and Dashboard services, with container builds fully recovered for x86 and ARM in the region. GitHub Actions jobs were still being processed at a limited rate due to the backlog, event-trigger throttling between SQS and Lambda, and GitHub rate limits.


It was at **6:16 AM PT** that we were still observing significant delays in SQS to Lambda event triggers, causing a slow processing of the GitHub Actions job backlog. We reached out to AWS support to understand when we could expect full recovery of SQS event triggers to Lambda.


The **second outage** in` us-east-1` became visible to us around **7:00 AM PT** , when we started seeing errors again in our provisioning system for both container builds and GitHub Actions jobs. We immediately began investigating the issue and found that, once again, we had lost connectivity to the EC2 API and were receiving capacity errors on every request.


During the second outage, we observed connectivity issues with DynamoDB, SQS, and EC2.


We began evaluating how we could restore at least some build service while the network was unavailable. We confirmed that we could support launching container image builds in our` eu-central-1` region, as that region was still fully functional.


**At 8:23 AM PT** , we advised container build customers who could have their container image builds run in the EU to update their project to the EU Central region to restore container build functionality. At the same time, we worked to restore` us-east-1` .


We then began evaluating if our backup regions for GitHub Actions in` us-west-2` and` us-east-2` could be used to restore GitHub Actions job functionality. However, we determined that the backup regions lacked the capacity or quotas to support the volume of` us-east-1` workloads. That said, we determined that some queuing was better than no runner at all with infinite queuing.


**By 9:00 AM PT** , we had deployed the necessary changes to our provisioning system to divert GitHub Actions jobs to our backup region in` us-east-2` . This allowed us to resume servicing some GitHub Actions jobs, albeit with significant delays due to limited capacity in the backup region.


With` eu-central-1` available for service container builds and` us-east-2` available for some GitHub Actions jobs, we were able to restore some functionality for our customers while we worked to restore` us-east-1` . We were still seeing significant delays and networking issues across` us-east-1` that were impacting our main API, but work was still able to process.


**At 11:00 AM PT** , we began to see EC2 launches succeed again in` us-east-1` , but capacity was still minimal. Some container image builds were able to start launching again, but GitHub Actions jobs stuck in that region were still unable to launch, as SQS to Lambda event triggers were down again.


**By 11:22 AM PT** , we saw additional recovery in the region, with more EC2 launches succeeding, but capacity remained very limited. We also started seeing our provisioning system's SQS queues come back online, and our Lambda events began processing. We started seeing some GitHub Actions jobs stuck in the queue start processing again, albeit with heavy delays and not at our normal rate.


**At 12:05 PM PT** , we saw more messages being processed from our SQS queues as AWS increased the rate of event-trigger processing again. But we were still seeing API errors in the control plane, and our ability to launch EC2 instances remained very limited due to capacity constraints in the region. We were mainly down to AWS reducing EC2 throttling and increasing SQS to Lambda event trigger rates for us to have our full capacity restored in the region.


**By 1:45 PM PT** , we started to see SQS to Lambda event triggers returning to normal rates, but had to wait for the full backlog of events to be replayed. EC2 capacity was also starting to recover as AWS continued to reduce the throttling on our account.


At this time, any new GitHub Actions jobs were automatically being diverted to` us-east-2` to avoid further delays, while container builds were being serviced in` eu-central-1` if customers changed their project region. Capacity remained limited in` us-east-2` , and AWS was unable to approve our quota increase while the outage in` us-east-1` was ongoing.


**By 3:00 PM PT** , we observed full recovery of queue processing for the backlog of GitHub Actions jobs and container builds being launched in` us-east-1` , and we were finally processing at full EC2 capacity. We decided to route GitHub Actions jobs back to` us-east-1` as the backlog was clearing and capacity was restored.


**At 3:30 PM PT** , we were seeing signs of complete recovery with both container builds and GitHub Actions jobs processing at normal rates again. We began the process of closing the incident.


We left our incident open for several more hours throughout the rest of the day to monitor for any additional issues, but none were observed. We worked with high-throughput customers to ensure they were fully caught up on any backlogs of GitHub Actions jobs that had built up during the outage period. This required us to carefully throttle job launches to avoid GitHub rate limits while we worked through the backlog.


## What we learned and what we're doing about it


Incidents like this are always a learning opportunity. Throughout the incident and in our post-incident review, we identified several areas where we can improve our systems and processes to make us more resilient to similar outages in the future.


1. **Maintain a warm backup region for GitHub Actions:** We realized that our backup regions for GitHub Actions did not have sufficient capacity or quotas to handle the volume of workloads from` us-east-1` . This made it incredibly hard for us to fail over to it to service demand anywhere near that of our primary region. We are working on turning` us-east-2` into a warm backup region with sufficient capacity and quotas to handle failover in the event of a significant outage. **We have completed that work as of today October 28th, 2025.**
2. **Better container build region failover:** We could have restored container build functionality faster if we had rerouted container builds to our` eu-central-1` region automatically when we detected the outage in` us-east-1` . We have explicitly avoided doing this automatically as several customers have compliance requirements that do not allow their data to be processed in the EU. However, we are exploring options to make this failover process smoother and faster for customers who can have their container builds run in other regions.
3. **Removing dependencies on ECR:** Depot Registry was impacted during the outage because of its dependency on ECR in` us-east-1` . We are exploring options to remove this dependency to make Depot Registry more resilient to regional outages.


## Conclusion


This incident was a big reminder of how vital core AWS services are to Depot. At the rate we launch EC2 instances per second, even a minor service hiccup can have an outsized impact on our ability to serve our customers. This outage shined a light on where we can improve core parts of our system to be more resilient to region outages and also where we can have tighter processes to fail over to backup regions when needed.


We are constantly working to improve Depot and make it more reliable. We are committed to being transparent with our customers about the issues we face and how we are working to fix them. We believe in learning from our mistakes to make improvements so that we don't make the same mistake again.


If you're not already, we recommend subscribing to our[status page](https://status.depot.dev/) to get real-time updates on incidents and maintenance.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
