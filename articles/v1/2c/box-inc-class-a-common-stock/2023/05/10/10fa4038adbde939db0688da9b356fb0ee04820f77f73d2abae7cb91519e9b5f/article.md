---
schema_version: "1.0.0"
document_id: "10fa4038adbde939db0688da9b356fb0ee04820f77f73d2abae7cb91519e9b5f"
company_key: "box-inc-class-a-common-stock"
company: "Box Inc."
source_id: "box-inc-class-a-common-stock-rss-6b6ba587c738"
canonical_url: "https://medium.com/box-tech-blog/software-rot-why-exercise-is-important-for-your-software-dab504058288"
published_at: "2023-05-26T21:34:07+00:00"
first_seen_at: "2026-07-20T04:36:04.074136+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:8bb2d196821ba5f1beadc0223e21c016eba53efe768206f942940ca03760bc47"
---

# Software Rot: Why Exercise is Important for Your Software

# Software Rot: Why Exercise is Important for Your Software


[Priyanka Reddy](https://medium.com/@priredd?source=post_page---byline--dab504058288---------------------------------------)


5 min read


·


May 26, 2023


--


Press enter or click to view image in full size


Illustrated by Navied Mahdavian / Art directed by Sarah Kislak


Once upon a time (well really in 2016), we built a life-changing service named Spatula that would automatically detect and flip failing databases out of the customer path. We deployed it and waited. It did nothing most of the time, except when a database failed. And that’s when it shined, deftly flipping traffic away from the ailing database to one that was fit as a fiddle. All within a matter of minutes, without human involvement. It was a sight to behold and we were all so proud of it and ourselves.


But then arrives the fateful night when we came face-to-face with our own mortality. It starts out just like many nights in the past. A database host fails. Of course it holds the data that powers the most critical parts of the site. Of course it’s Saturday. Of course it’s 3 am (seriously, why do all database failures happen between the hours of 3 and 4 am?). But no matter. Spatula springs into action and detects the failure as it should. But then when it came time to executing the flip, Spatula fails!


Luckily, Spatula pages the oncall DBA that the auto-remediation didn’t succeed. The oncall DBA proceeds to drag herself out of bed and because it’s been so long since she’s had to manually remediate a failed database (yay, Spatula!), she has to relearn the process of doing so while being half asleep. A short while later, we’re back up.


In this situation, not only has Spatula’s existence not made things better, it’s actually made things much worse (boo Spatula). By automating this critical process, we’ve erased it from the collective minds of the DBAs.


To make matters worse, Spatula is still broken. It’s still 3 am on a Saturday. And, we’re at risk of reoccurrence until Spatula is functional again. After some debugging, it’s discovered that Spatula has been dysfunctional since the previous Monday at 2 pm when a maintenance unrelated to Spatula took place. Not only has a key piece of our database availability infrastructure been silently broken for over 4 days waiting to bite us at the worst possible moment, but now we have to fix it in the middle of the night.


What’s brought us to this less-than-ideal situation in the first place? The most direct cause is the Monday maintenance that changed something in the environment, rendering Spatula broken. It’s easy to get overly focused on the fact that an unrelated maintenance broke Spatula. But that’s a fact of life when it comes to software, especially if critical paths in the software sit idle for long stretches of time. It’s a phenomenon known as “dormant software rot”. Dormant software rot is the idea that “software that is not currently being used gradually becomes unusable as the remainder of the application changes”. (wikipedia). Software does not work in a vacuum. It interacts with and relies on upstream and downstream systems owned by different teams. It also lives in an ever-evolving shared environment. Any of those could change and put the software into an unusable state. And the longer the software goes without being executed, the more likely that dormant software rot sets in. The only way to combat it, then, is to ensure that the software is exercised regularly.


How do we apply this to our scenario? Spatula reacts to failed databases, something that thankfully doesn’t happen too often. It’s not something we want to force to happen more often than necessary as each database flip can cause a brief service degradation. Instead, we decided to introduce a synthetic check on a test database.


The diagram below shows the full architecture of the Spatula pipeline and the modifications that we made to ensure a seamless health-checking pipeline. It wasn’t trivial but it wasn’t rocket science either. And, the return far outweighed the investment.


Press enter or click to view image in full size


Architecture of the Spatula pipeline and modifications


Although we considered testing only portions of the pipeline for the sake of simplicity, we ultimately decided that in order to fully detect dormant software rot, the entire pipeline had to be tested. Today, we test the pipeline 3 times during the day (8 am, 3 pm and 6 pm), with the end of the workday being most important to ensure that no changes during the day negatively impacted the pipeline.


Once the synthetic check was deployed, here’s how the events unfolded during the next problematic maintenance.


Thursday


- 6:00 am: A maintenance is performed on the configuration pipeline that Spatula relies on. The maintenance breaks the configuration pipeline, putting Spatula in an unusable state.
- 8:00 am: Spatula’s synthetic check runs, detects that Spatula is broken and alerts the team. The team sees that the previous night’s 6 pm synthetic check succeeded so something must have changed since then to break Spatula. They comb through all production changes that were made between 6 pm and 8 am, identifying the 6 am maintenance as a possible culprit. Once the team verifies that the maintenance is indeed the cause of the breakage, they make the owning team aware of the issue so they can start investigating.
- 2:00 pm: After several hours of investigation and remediation, the configuration pipeline is functional once again and so is Spatula.


One doesn’t have to look hard to see how this incident was a vast improvement from the earlier one.


1. The team was alerted to a dysfunctional Spatula by a proactive alert rather than a customer-impacting database failure.
2. All of the investigation and remediation was done during the workday rather than at 3 am.
3. Spatula was dysfunctional for far fewer hours (6 hours vs. 100+ hours)


Although we’ve had several repeats of issues that look much like the Thursday morning incident, the Spatula synthetic check has saved us from repeating that fateful night.


This idea of synthetically executing a rarely exercised code path is not unique to the Spatula pipeline. The same principles can be applied to any system that is not constantly in use and therefore subject to experiencing dormant software rot. A couple examples include service discovery changes, active-passive systems, configuration delivery pipelines, etc.


There’s a pretty good chance that at least one of your own systems has this plague hiding within, waiting to bite you at the least opportune time. Don’t wait until you have your own fateful night. Go forth and seek out your software rot!


*If you’re interested in joining us, check out our*[open opportunities](https://box.eightfold.ai/careers?location=United+States&domain=box.com) *.*
