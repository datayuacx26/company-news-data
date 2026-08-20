---
schema_version: "1.0.0"
document_id: "114d5992b86607bced841ae9064cdd4060c72ea657e0a550d5be6f5ae76a64e0"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/may-8-outage"
published_at: "2025-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:30a4c22be06437ea6bbbb27133f140ac9aa16640ab643c29861825e09e01ae23"
---

# Knowledge share: Insights from our major outage on May 8th

After nearly three years of building Depot, we've grown exponentially in the past 12 months. While we've unlocked new performance that is making builds exponentially faster, we've largely run Depot on a lot of the existing systems and architectures up until the start of this year.


Our existing systems and architectures have served us well, but as we've grown, we've started to see some of their limitations. We've been working hard to build out new systems and architectures that will allow us to scale Depot even further. But, those changes take time and are not without their own challenges.


Those existing systems caught up with us on May 8th. A major outage took our core provisioning system for builds offline for several hours. This was a significant incident that disrupted thousands of builds across Depot.


First and foremost, we apologize for the disruption this caused everyone. Making builds exponentially faster is only as good as you actually being able to run builds, and we failed there. We are writing this to share the timeline of what happened, our root cause analysis, and what we are working on to avoid these types of major outages in the future.


## The outage timeline


We have logging and monitoring in place to help us understand what is going on in our systems, including AWS CloudWatch, Sentry, Grafana, Incident.io, and internal observability tools. We also have a status page that we use to communicate with our customers during incidents.


You can find the status page timeline of the incident[here](https://status.depot.dev/cmafni8la004z9pwuozks8vwx) . The timeline below is a summary of the key events that occurred during the incident.


- 10:30 AM PT: We started to receive alerts that both container builds and GitHub Actions jobs were failing to acquire hosts from our provisioning system for a large percentage of customers.
- 10:35 AM PT: The entire team jumped on a video call to start diagnosing where the root cause is.
- 10:38 AM PT: We opened an incident at` status.depot.dev` to communicate with our customers about the incident.
- We are aware that this incident was opened as a partial outage. This was a mistake on our part. We should have opened it as a major outage. We later changed it to reflect the correct severity.
- 10:45 AM PT: We diagnosed the issue as extremely heavy load on our primary database, with it remaining at 100% CPU utilization. Additionally, the DB transaction pool was exhausted due to high query volume. This was causing our provisioning system to be unable to acquire hosts for builds. Meanwhile, replicas in the database cluster were still able to serve reads without issue.
- 10:50 AM PT: We started turning off an initial set of non-critical services to reduce load on the primary database. We were also alerted by AWS that we had exhausted our account's vCPU quota due to the number of queued jobs attempting to start, and so we opened a critical support case to increase the quota.
- 11:00 AM PT: Our initial set of non-critical services were turned off, but the primary remained pegged. At this point, the backlog of GitHub Actions jobs was growing into the several thousands, further increasing the load on the database. We deployed an additional workaround to our provisioning system to try and slow down the thundering herd of GitHub Actions jobs so that we could get enough headroom to start the database recovery process.
- 11:30 AM PT: We began to scale up the database cluster to increase headroom on the primary.
- 11:45 am PT: While the database scale up was in progress, we started introducing more drastic exponential backoffs into our provisioning system to limit the number of jobs trying to hit the primary simultaneously.
- 12:10 PM PT: The database scale up completed and database CPU utilization began to drop, but this then allowed some of the job backlog to start processing, again causing the database to be overloaded.
- 12:30 PM PT: We began turning off additional services that were not the core provisioning system to create more room in the primary to process the backlog faster.
- 12:40 PM PT: We started to see some headroom on the primary database. However, the sheer backlog of Docker image builds and GitHub Actions jobs were causing additional failures as our provisioning system was trying to launch thousands of new hosts a minute, and our AWS vCPU quota increase request was still not approved.
- 12:53 PM PT: While waiting for more CPUs, we began scaling additional services in to reduce connections on the database to create more headroom. We kept the application and API online, but significantly reduced so that we could reserve as much of the database as possible for the provisioning system.
- 1:10 PM PT: the vCPU increase was approved by AWS.
- 1:15 PM PT: We moved additional frequent or expensive queries from the primary DB node to read replicas, such as authentication queries from build hosts. This wasn't ideal, as read replicas have some amount of lag as they receive updates from the primary, which can cause jobs to have authentication issues, but it did reduce load on the primary database node.
- 1:20 PM PT: As CPU pressure on the primary was reduced, the backlog of jobs began processing. We limited the concurrency of jobs that could be processed to ensure that the database would remain healthy during the process.
- 1:25 PM PT: We slowly increased the concurrency limit until it was restored to the regular value. This was a slow process as we wanted to verify that the primary was stable and that we wouldn't run into any additional issues.
- 1:36 PM PT: We observed the backlog of work was fully caught up for our primary regions (us-east-1 and eu-central-1). We began working to bring additional regions back online.
- 1:50 PM PT: We started seeing normal processing and complete backlog processing in all our regions.
- 2:00 PM PT: We discovered some additional issues with cache volumes for container builds.
- 2:10 PM PT: We discovered that cache volumes were not detaching from the builders because of the earlier outage. We began the process of cleaning these up automatically.
- 2:15 PM PT: We cleared out broken cache volumes for customers who saw issues with their container builds launching. This brought container builds back online for all customers.
- 2:26 PM PT: We closed the incident as container builds and GitHub Actions jobs were fully restored. We also updated any customers that had contacted us about the incident to let them know that their builds were back online.


Once the incident was closed, we discovered a new bug in our provisioning system, causing a small percentage of GitHub Actions jobs to fail. This was a separate issue from the incident, and instead, it was database replication lag causing some jobs to fail to authenticate with our control plane. We diverted some authentication queries from replicas to the primary database to resolve.


## Root cause analysis


Once the incident was closed, we began our root cause analysis. We wanted to understand what happened, why it happened, and how we can prevent it from happening in the future.


The root cause of the incident was a combination of factors:


First, the primary database had been running quite hot for a while, and we believed we had enough headroom to handle spikes in load. But we grossly underestimated the degree of load in the event of a major outage, caused by tens of thousands of GitHub Actions jobs all authenticating at the same time.


Because the database was running hot, any heavy enough query would cause the primary to hit 100% CPU utilization. In the case of this incident, a query from our cache explorer page tried to load over 140,000,000 cache entries and their metadata, causing the database to be unable to process other queries while the expensive calculation completed. This then caused downstream effects in the provisioning system.


When the database became unresponsive, the provisioning system could not respond to requests for new hosts, authenticate builds, and process jobs. This caused a cascading failure. GitHub Actions runner hosts could not authenticate with our provisioning system to retrieve work, so they continued to retry this failed request, causing additional load on the already overloaded database.


So, while the queries pushed the database to 100% CPU utilization, it was actually the provisioning system and the authentication calls for jobs and builds that kept it pegged at 100%. This is why moving the authentication of workloads to a replica was necessary to get the primary back online.


## How did we fix it?


We've already taken several actions to fix the issues that caused the incident. We are still working on some of the longer term fixes, but we wanted to share what we've done so far.


1. We scaled up our database cluster to create more headroom on the primary database.
2. We moved queries additional queries to replicas that can live with any replication lag.
3. We added additional monitoring to our provisioning system to allow us to monitor request rates at a finer grain so that we can detect bottlenecks in the system earlier.
4. We've increased our AWS CPU limits to allow for extra headroom in the event we need to quickly launch even more hosts at the same time than our traditional normal operating load.
5. We've implemented a tighter on-call process to quickly start shedding load on the primary database in the event of an outage that is caused by a spike in load.


In the long term, we are working on some additional changes to our architecture to help us scale better and gracefully degrade in the event of an outage. Some of these changes include:


- Moving our provisioning system to a separate database cluster that is optimized for the types of queries we make. This will allow us to scale the provisioning system independently of the rest of Depot.
- Adding circuit breakers to non-critical services to allow us to shed load in an outage without needing to deploy any new code.


## Conclusion


Outages suck. They are never fun, and they are always disruptive. This one is no exception. We are sorry for the disruption this caused everyone.


We are constantly working to improve Depot and make it more reliable. We are committed to being transparent with our customers about the issues we face and how we are working to fix them. We believe in learning from our mistakes to make improvements so that we don't make the same mistake again.


If you're not already, we recommend subscribing to our[status page](https://status.depot.dev/) to get real-time updates on incidents and maintenance.


If you have any questions about the events or how we are working to prevent them in the future, please reach out to us in our[Discord Community](https://discord.gg/MMPqYSgDCg) . We are always happy to chat.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
