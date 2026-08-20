---
schema_version: "1.0.0"
document_id: "b5e70ab3fe0de5e9c0c312fa8fb3ede1db3fa60caeb41a45a7a49e87d51fb1ed"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/incident-report-jun-22-2026"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-24T05:26:40.243460+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:f23e235a8a9b7a70d710c5c64bce690c243e9ff9fcab93b42c625a684a63fd6d"
---

# Incident report on June 22, 2026

Before we get into what happened I want to emphasize how important reliability is to us. This has fallen very short of providing you all with a great experience and a reliable service. We're really sorry for the problems this has caused for you all.


**Every paying customer will be credited back everything they spent over the three full days from 00:00 on 21 June to 23:59 on 23 June (UTC).** This comes off your June invoice automatically, so there's nothing you need to do. The minimum is $10 per account so smaller customers aren't left with a token amount.


If you paid for more concurrency during the incident, and no longer need it, we'll reset it and refund you. Let us know via our[contact page](https://trigger.dev/contact?type=concurrencyRefund) .


## TLDR impact and causes


First though let's cover what was impacted and top-level causes.


Between June 22nd ~19:00 UTC and Jun 23 ~13:13 UTC our us-east-1 region and then eu-central-1 regions had periods of slow dequeuing and full outages where no runs were executing. The recovery took the best part of a day.


This impacted tens of thousands of organizations using the cloud product.


It started in our us-east-1 region with a temporary AWS capacity shortage that led to a large batch of worker machines all appearing at once, right as a wave of scheduled runs fired. This flood overwhelmed the Kubernetes control plane that schedules cold starts for runs. Its coordination database (etcd) lost quorum and the region could no longer start new runs.


While we were trying to recover this region we recommended switching to eu-central-1.


This region operated normally until Jun 23, ~08:38 UTC. Unfortunately it had the same weaknesses, and was starting from a far smaller size, making the scaling velocity higher. The same chain of events took down eu-central-1 until Jun 23, ~13:13 UTC when both regions were operating stably again.


**No runs were lost.** Work that was already executing kept running, and runs that queued up during the outage stayed queued and ran once capacity returned.


**There were very long delays.** The regions were down or recovering for a long period, so in those regions runs couldn't start. Also, switching regions was painful because bulk replaying between regions was not supported until late on. We know these delays disrupted your business, and your customers in turn, and we're sorry for it.


---


## What we're changing so this can't happen again


This is the part that matters most, so we'll be specific. A multi-hour region recovery from a cluster failing is not acceptable, and almost everything below is aimed squarely at making sure it can't happen the same way again.


Some changes are done, some will land soon, and some will take a bit longer.


### 1. More than one isolated cluster per region, with automatic failover


This is the single biggest roadmap re-prioritization to come out of this incident. This was planned but not prioritized highly enough.


Each region will always run more than one Kubernetes cluster, so if a control plane fails or critical systems fail, workloads can move to a healthy cluster in the same region automatically.


This achieves several valuable things:


1. Fewer single points of failure.
2. Automatic failover to a healthy cluster in the same region.
3. We can horizontally scale regions.
4. Smaller clusters are less likely to suffer from pressure issues because the ratio between control plane and worker nodes is better.


Each cluster's control plane will be sized like today's, but each cluster will run well below its limit, with a capped node count, so a healthy cluster has the headroom to absorb a failed one. We scale a region by adding more small clusters, not by growing each one, so we never drift back to a single oversized cluster.


### 2. A us-west-2 region


Many of you would like a us-west-2 region because you run your database and compute (near) there already.


It will help alleviate pressure on us-east-1 and be a good alternative customers can choose when underlying server capacity is limited.


This is one of the easiest wins which we're working on now.


### 3. A self-serve way to bulk move runs between regions


We have a powerful[bulk actions](https://trigger.dev/launchweek/2/bulk-actions) feature but it didn't support replaying to a different region. This was implemented in the dashboard towards the end of the incident.


We will add API endpoints and SDK functions so Claude can perform these actions too.


### 4. Don't permanently lock concurrency to a region for not started runs


Our multi-tenant fair queue system has two phases:


1. Concurrency evaluation phase. Evaluate concurrency limits (defined on your environment and individual tasks/queues). If there's availability it gets moved to…
2. Ready to execute phase. A long list of runs that can be picked up by the specific region. The region pulls runs from the list as fast as it can to execute.


This allows fairness and high performance but it has a drawback right now: the second phase means runs are locked to a region and take up concurrency.


In this incident we recommended people move regions. After moving they had limited concurrency because it was consumed by runs in phase 2 in the original region. The mitigation for this was to use our bulk cancel feature on those runs in the original region.


Our fix will shift phase 2 items back and prevent unhealthy regions from consuming concurrency.


### 5. Backpressure


We recently built a mechanism to shed load when the cluster can't schedule new work.


It would have softened the capacity pressure that crushed the control plane, but it was running in observation mode and so applied no actual limiting. Terrible timing.


It's now switched on and we're actively configuring and improving it.


### 6. A hardened etcd database


etcd is moving onto its own fast, dedicated disk. It will have better alarms, be better sized, have improved automatic compaction, and better defragmentation so it can't quietly bloat into something fragile.


### 7. Moving to managed Kubernetes


Running our own Kubernetes control planes means we carry all of this operational complexity ourselves: etcd tuning and defragmentation, control plane sizing, and recovery when it goes wrong. We had to go with self-managed Kubernetes because we use[CRIU for checkpoint/restores](https://trigger.dev/blog/v3-announcement) . This incident showed how sharp that edge is.


We're moving our new Firecracker MicroVM clusters to managed Kubernetes (EKS), where AWS runs and scales the control plane and etcd for us. The hardening above protects what we run today; this takes the whole category of failure off our plate for good.


### 8. Faster cluster recovery


We'd never lost a whole region's control plane before, so we worked the recovery out live at the worst possible time. Bringing the regions back online took far too long.


Overwhelming the control plane was the common theme during the recovery.


We will be practicing region recovery. From that we can figure out the optimum path but it will most likely include:


- A fast break-glass path into a control plane that's unreachable because it's overloaded.
- A mechanism to quickly take the pressure off the control plane to avoid reboot loops.
- The ability to roll out reconnections of still-running essential systems
- Fast and safe cold boot of essential systems.


Cold run starts create new resources, require new servers, and pull uncached images. This means they have a far higher performance cost than warm starts which are just routing. This made rebooting our large regions challenging because a reboot is 100% cold. Again running multiple smaller clusters with lower max total pressure makes this recovery easier.


### 9. Earlier detection


We have monitoring and alerting, but it wasn't watching the signals that would have caught this early, so by the time we were paged the control plane was already going down. We've been analyzing the data to find the warning signs that were building before the spike: control plane CPU, etcd size and write latency, and queue depth.


Those signals can drive alerts, and they can also feed back into the system to ease off pressure automatically. So next time we can shed load before it cascades.


### 10. Queue metrics


We were already working on Queue metrics – graphs for your queues and concurrency that can be seen in the dashboard and accessed via our[query](https://trigger.dev/changelog/query-and-dashboards) feature.


This will give better visibility into queue health and performance. We will be able to build alerts for you on top of this.


### 11. New status page with more frequent pushed updates


Our[status page](https://status.trigger.dev/) was too coarse. Our new one will break out regions and services so it's easier to see what's happening. This will be live in the next 48 hours.


You can subscribe to updates from our status page (top-right) but it's opt-in. We are working on pushing updates automatically to our customer support channels, like Slack and the shared Discord.


---


## The full detail


### What was impacted


Our us-east-1 (Northern Virginia, USA) region was hit first and worst. For several hours it couldn't schedule new work. Runs that were already executing kept going, but new runs queued up instead of starting, and the backlog grew through the evening. At the peak, millions of runs were queued across the platform.


We pointed customers at eu-central-1 (Frankfurt, Germany) while us-east-1 was down. This was a major mistake. The sum of the pressure on us-east-1 and eu-central-1 was now combined, in a region that was far smaller than us-east-1.


The eu-central-1 recovery was faster (because we'd figured it out now) but still far too slow. This resulted in a window in the early afternoon of the 23rd where neither region was operating at full dequeuing speed.


The actual impact:


1. No runs were lost. Work that was already running kept running. Runs that were queued during the outage stayed queued and executed once capacity came back. Our default run retention (14 days) is far longer than this incident lasted.
2. There were very long delays. The regions were down or recovering for a long period, so in those regions runs couldn't start. Also, switching regions was painful because bulk replaying between regions was not supported until late on.


### Timeline of major events


Time (UTC) What happened


Jun 22, ~19:00 ~10,000 runs enqueued in us-east-1. AWS can't provision worker capacity across availability zones, so pending work piles up while we wait.


Jun 22, ~19:56–20:03 AWS capacity frees up and a large batch of worker nodes joins us-east-1 all at once, coinciding with the top-of-the-hour scheduled runs.


Jun 22, ~20:00 The control plane saturates at 100% CPU; etcd starts dropping internal messages.


Jun 22, ~20:09 us-east-1 loses etcd quorum. The control plane goes down and new runs stop starting.


Jun 22, ~20:20 onward Recovery begins. The control plane briefly comes back, then collapses again repeatedly under the storm of reconnecting nodes.


Jun 22, ~22:00 The coordination database's disk I/O limit is raised; etcd is brought back up in isolation and defragmented (~5 GB → ~600 MB).


Jun 23, ~01:10–01:45 With every component throttled hard, the control plane holds for the first time and a healthy three-node setup is restored.


Jun 23, ~02:00–02:30 We raise the control plane's request limits gradually, one node at a time, checking it stays healthy at each step.


Jun 23, ~03:30 The run queue's backing store becomes the next bottleneck under the backlog and pins its CPU. We scale it up.


Jun 23, ~04:20–04:44 One of our own internal tools runs an oversized query that briefly re-floods the recovering control plane; it recovers once we stop it.


Jun 23, ~05:18 etcd has quietly bloated again, to around 4 GB, slowing down under the ongoing churn.


Jun 23, ~05:28 A second defragmentation brings etcd back down to ~300 MB, one node at a time, without losing quorum.


Jun 23, ~06:08 We pause dequeuing entirely to take the last of the pressure off the run queue's backing store.


Jun 23, ~06:40 We begin resizing the control plane onto larger machines, replacing nodes one at a time and re-checking etcd health at each step.


Jun 23, ~06:53 The resize completes. us-east-1 is back to full dequeuing speed and the backlog is draining.


Jun 23, ~08:38 eu-central-1 loses its control plane the same way.


Jun 23, ~10:11 eu-central-1 etcd quorum restored.


Jun 23, ~12:18 eu-central-1 control plane resized onto larger machines.


Jun 23, ~13:13 A post-recovery cascade in core components is cleared. Both regions fully recovered, dequeuing back at full speed.


### What caused this


It started with a capacity shortage at AWS. We asked for worker nodes in us-east-1 and couldn't get them in any availability zone, so pending work piled up while we waited.


Then the capacity freed up all at once. A large batch of nodes joined the cluster at the same moment the top-of-the-hour scheduled runs fired. Every new node has to register with the Kubernetes control plane, and we believe that flood of registrations, on top of the scheduled spike, drove all three control-plane nodes to 100% CPU.


etcd, the coordination database behind the control plane, couldn't keep up. It started dropping internal messages, lost quorum, and the control plane went down at around 20:09 UTC. From there it kept failing to form a stable quorum: members would briefly come back and then collapse again, which is a big part of why this dragged on for so long. The machines running your work stayed healthy throughout, but we'd lost the ability to schedule anything new, or even to see clearly into the cluster.


A few things turned a capacity blip into a full outage:


- **etcd had grown large over time** , which made it slow and fragile under load instead of riding out the spike. Automatic compaction was on but not tuned for our churn, there was no scheduled defragmentation, and no alarm on its size.
- **It shared a disk with the operating system, and that disk had an I/O limit.** On restart it couldn't read its data back fast enough before the health check gave up and killed it. It would come up, fail, get killed, and try again, over and over.
- **Our own recovery automation made it worse.** Every time the control plane came back for a few seconds, the systems that scale and replace worker nodes immediately launched another wall of queries and nodes, each one hammering the control plane the instant it reappeared. The cluster is built to aggressively keep capacity topped up, and during a recovery that behavior was exactly wrong. It knocked the control plane back down faster than we could bring it up.


Underneath all of it is a single point of failure: one cluster per region. When it went, the whole region went with it. That is the thing we are most determined to fix, and it's why running more than one isolated cluster per region is the biggest change we're making.


### How we recovered


When the control plane is down, you lose most of the tools you'd use to fix it. And every time we brought it back, the same storm that took it down knocked it over again. Losing shell access made it worse. The pattern that finally worked was to bring back one piece at a time, in an order that kept the pressure off until etcd could take it.


**Bring back one node.** We force-rebooted a single control-plane node. One etcd member can't reach quorum (you need 2 of 3), so it came up doing nothing, and we disabled its apiserver, scheduler, and controller-manager so it stayed that way.


**Work through a private API.** We bound an apiserver to localhost, unreachable from the rest of the cluster. That gave us somewhere safe to operate while everything else stayed locked out.


**Take the pressure off.** We cut off the clients hammering the API, suspended node scaling and automated recovery, and deleted the tens of thousands of leftover runner pods that had bloated etcd and made every list expensive. Until that pile-up was gone, every API we exposed to the cluster died in under two minutes.


**Give etcd room to restart.** With the dead weight gone we defragged etcd, taking it from around 5 GB down to about 600 MB, and raised the IOPS limit on its disk so reloading the database couldn't peg the volume and trip the liveness probe mid-replay.


**Rejoin quietly, never by reboot.** We brought the second member back by disabling its manifests offline through a disk attached to a temporary instance, then booting it etcd-only. A plain reboot starts the apiserver, advertises it, and the fleet stampedes (we learned that the hard way). Quorum came back at 2 of 3, then 3 of 3 with the last node.


**Throttle, then ramp.** We brought every component back hard-throttled, then lifted the limits slowly, one node at a time, watching etcd the whole way.


The control plane reached a stable, healthy state in the early hours of the 23rd. We kept seeing wobbles for a while as we brought load back, so we spent the rest of the night draining the queue and restoring capacity gradually. Pushing too hard risked starting the whole thing over.


Quorum coming back was not the end of the incident, it was the start of a long ramp. It restored coordination, not service, and the system fought back at every step:


- **Every time the control-plane API became reachable, it fell over again within a couple of minutes.** The moment it came back, hundreds of internal components reconnected at once and asked it to list tens of thousands of stale run records, which knocked it straight back down. It only held once we'd deleted those records, and even then we had to bring it back one node at a time, behind limits we raised by hand over half an hour.
- **etcd bloated a second time.** The first defragmentation was part of getting quorum back. A few hours later etcd had ballooned again to around 4 GB, most of it fragmentation, and we had to defragment the whole cluster a second time. A manual defrag is the only thing that shrinks it, and it climbs straight back, so this was a holding action, not a fix. That's why a hardened etcd is on the list above.
- **Everything was slow and by hand.** Our automated deployment was paused so nothing would fight us, we had almost no way into the affected machines, and every change took minutes to take effect. The final resize onto larger machines was a hand-driven, node-by-node replacement, watched one health check at a time, any of which could have collapsed the cluster again if we'd rushed it.


### The knock-on problems


A few separate issues showed up during recovery and made things harder for customers who tried to keep working.


**The worst one: queued runs held onto concurrency.** When a run moves from the main queue into a per-worker queue but hasn't started yet, it still counts against your concurrency limit. us-east-1 was so far behind that a lot of runs were stuck in that state. For some of you, your concurrency looked completely full of stuck us-east-1 work, so even after switching to eu-central-1, your new runs there wouldn't start. It worked both ways: runs waiting in one region's worker queue could quietly hold concurrency that blocked the other region too. The failover region didn't work for the people who needed it most. Clearing the stuck concurrency by hand got individual customers moving again, but it wasn't something we could safely do across the board mid-incident.


**The queue's backing store got hammered.** The further behind the queue gets, the more expensive each attempt to pull work becomes, and its CPU sat at 100% for long stretches. We vertically scaled it to get headroom.


**Failover wasn't smooth.** eu-central-1 is smaller than us-east-1, and it had to scale up under the same AWS capacity pressure. Its queue had never handled this much work, so dequeuing was slower than usual. The images for shifted workloads weren't cached, so a lot of runs had slow cold starts. And our bulk replay tool pins runs to their original region, so customers couldn't easily move a backlog of us-east-1 runs to eu-central-1. We shipped the ability to bulk replay between regions but it didn't come until near the end of the incident.


We also hit a bug in how we balance scheduling across environments under an unusual backlog, which could leave some work waiting longer than it should. A community member flagged it, and we've tracked it down and have a fix ready to ship.


### Then it happened in eu-central-1


Around 08:38 UTC on the 23rd, eu-central-1 went down the same way, and we'd helped push it there. By pointing customers at eu-central-1 while us-east-1 was down, we stacked the combined load of both regions onto a smaller cluster with exactly the same weaknesses. It had less control-plane headroom to begin with, so it took the strain even less well, and the same chain played out: capacity pressure, a storm of work, etcd falling over. A separate failure in our container image registry at around the same time caused a wave of image-pull failures across the cluster, which we recovered from without data loss.


The recovery followed the same playbook, and was faster because we'd just been through it. But it wasn't without unique challenges.


This time we scaled the control plane up to larger machines first, then turned dequeuing back on, rather than the other way around. It still wasn't clean. Bringing the bigger control plane up left a stale node behind that kept some core components unhealthy and drove API latency up until we cleared it.


We made the same mistake twice: we forgot to enable backpressure during the eu-central-1 recovery. It's now enabled permanently for both regions and under active development and monitoring.


By around 13:13 UTC on June 23rd both regions were back, dequeuing was ramping to full speed, and the backlogs were draining. Working through that many queued runs takes time, so some runs started later than they normally would, but they ran.


## What's next


The protection we most want to give you is multiple clusters in the same region, with automatic failover between them. But it doesn't exist yet. We're building it, and it's the biggest of the changes listed above.


It's also the hardest, so while we build it we're shipping the rest in parallel, including the earlier detection, automatic backpressure, and more robust recovery that prevent a spike like this from cascading in the first place.


I'm sorry for the problems this caused for you all. We have scaled enormously because of you all in the past six months (and it hasn't slowed down). This was a brutal reminder to focus even more on reliability so we can deliver a consistently great experience.


–[Matt](https://x.com/mattaitken)


If you have any questions about this incident or anything in this post,[reach out to us](https://trigger.dev/contact) .
