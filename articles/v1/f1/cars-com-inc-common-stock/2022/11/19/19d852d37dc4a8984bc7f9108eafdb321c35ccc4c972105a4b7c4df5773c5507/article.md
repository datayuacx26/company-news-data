---
schema_version: "1.0.0"
document_id: "19d852d37dc4a8984bc7f9108eafdb321c35ccc4c972105a4b7c4df5773c5507"
company_key: "cars-com-inc-common-stock"
company: "Cars.com Inc."
source_id: "cars-com-inc-common-stock-rss-49a4db916ec1"
canonical_url: "https://tech.cars.com/mysterious-metastable-crashing-3def82e767c6"
published_at: "2022-11-23T19:08:47+00:00"
first_seen_at: "2026-07-20T23:17:02.597310+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:26279302ca3d8c03f9b8f678186f073717cc672622a3ccad27d83facf2c6cae6"
---

# Mysterious Metastable Crashing

# Mysterious Metastable Crashing


[Christian Koch](https://medium.com/@ckochx?source=post_page---byline--3def82e767c6---------------------------------------)


10 min read


·


Jul 21, 2022


--


Press enter or click to view image in full size


Mysterious Metastable Crashes by Christian Koch


[Cars.com](https://www.cars.com/) is an automotive marketplace that connects car buyers and sellers. With nearly 25 years under our belt, Cars.com has become synonymous with car shopping and regularly ranks as №1 in our competitive set when it comes to brand awareness. Cars.com uses Elixir, Phoenix, and LiveView in our web stack.


## Background


For a period of 2 deploys, all of our web-serving application pods would enter a state where they were crashing constantly and could not recover. This occurs during our normal production deploy process. It occurs once during a deploy and once during a rollback.


Subsequently, approximately two days, one month, and three months later we observe anomalous crashes in both CarsWeb and other deployed applications. Initially we have no suspicion, nor reason to suspect all these crashes were related, spoiler alert, they were!


## Tangential Background


Is it pronounced meta-stable or metasta-ble?


## Day0 Jan 18 1:00 (around deploy) Incident0


During a production deploy, our customer facing website goes down. This triggers several paging alerts and the incident response plan is activated. Once the instability is noticed, the deploy is halted and reversed. During the period of the outage we notice that the crashing pods would start and then crash almost immediately, then another node in the cluster would come up and crash. This suggests that somehow we might be flooding the alive nodes with the majority of the website traffic. Because the nodes are getting overwhelmed and crashing so quickly, it is difficult to get any helpful logs. There are no obvious culprits nor anything in the deploy diff that suggests we had introduced a regression.


After much head scratching, the decision is made to block all inbound traffic to the website. Unfortunately, this is not something we practice, so the easiest viable option was to drop all the traffic at the edge router layer. This is also a slow layer to change as the rules need to propagate throughout the edge network and this takes some minutes to complete. Once a significant portion of the traffic is getting blackholed the cluster quickly begins to recover.


With the edge router unblocked, and traffic returned to the website the cluster appears to be stable running the previously released code. While the fix was a relief, the root cause is unclear. There are some commits in the release that are suspected culprits, but no data to definitively correlate the crashing with a source. This lack of data is exacerbated by our kubernetes-driven deployments, which make the pods ephemeral and make it difficult to impossible to capture crash dumps when the pods to crash. Without more definitive clues, we assume a regression in the code is a contributing factor.


## Remediation measures


- Halt deployment
- Increase pod count


## Day2 Jan 20 (around deploy) Incident0 continued


The day two outage was largely a repeat of the day zero outage, during a deploy, the cluster becomes unstable and suffers from near continuous restarts. An extra Socket connection (non LiveView) is added to the code either this day or D0. This change inadvertently adds memory pressure and makes the cluster more susceptible to metastable failure. In our logging and monitoring, we notice that the memory footprint of the web serving pods spiked during the deploy window and were subject to out of memory (OOM) kills from the cluster hypervisor.


A traffic blocking fix is applied and we were able to recover site stability. (This traffic block is applied a bit “closer” to the apps instead of at the edge router layer. )


## Remediation measures


- Block traffic
- Increase memory limit
- Retain increased pod count


## Investigation Findings (Day0 and Day2 outages)


> ***We were able to determine the specific chain of events that lead to the overall failure:***
>
>
> *Rapid memory overuse on the website pods lead to cluster instability. The primary culprit is a form of non-malicious traffic flood that rapidly degrades the ability of the website cluster to serve requests. When one pod goes down (for planned or unplanned reasons) the connections to the pod are redistributed to other pods. The increased connection memory pressure, when there is not enough slack in the system, causes one or more other pods to fail and redistribute their connections in a manner similar to the first. These connections redistributing cause a rapid cascade of failures and quickly put the cluster into a state where there are too few running pods to manage the total traffic volume.*
>
>
> *The memory overuse also makes diagnosis more difficult as memory limit violations are enforced by the hypervisor and not an internal mechanism. The effect is that when one pod violates its memory limit, it is killed and reaped, rendering further investigation impossible. Given the saturating nature of these crashes, it’s also not clear if the app is ever able to emit metrics or logs to our data aggregator service.*


## Day3 Jan 21 1:00 Incident0 continued


*AWS restart of Kafka nodes in MSK (Amazon Managed Streaming for Apache Kafka \[how is this MSK!?\])*


The day three outage is largely a repeat of the day two outage, except that the cluster is not deployed. Increased latency occasionally causes web timeouts and in this situation, causes enough timeouts for some nodes in the cluster to be considered unhealthy. Once enough nodes are unhealthy, there are too few viable nodes to serve the website traffic.


A traffic blocking fix is applied and we were able to recover site stability.


## Day 33 Feb 21 Incident1


*AWS restart of Kafka nodes in MSK*


## Get Christian Koch’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Another App crash (with minor outage) is noted. This has a similarly random seeming genesis without any obvious cause. It is determined that a rolling restart of pods may help and it does. ( *Have you tried turning it off an on again?* )


## Day 111 May 10 Incident2 (bonus Incident3!)


*Data Team manual restart of Kafka nodes in MSK*


## Metastability rears its head again.


In debugging the day 111 issue another issue is evident, that some of our tracking metrics have uniformly declined. Initially this is attributed to the crash and outage, but once the crashing has been resolved by a rolling restart of all the affected pods ( *Have you tried turning it off an on again?* ) the metrics differential remains.


While debugging the metrics delta we are able to determine with high confidence that it is unrelated to the outage. It is related to a regular expression (regex) that is not parsed correctly in the Safari browser only and reminds this author of the[Cloudflare outage from July 2019](https://blog.cloudflare.com/details-of-the-cloudflare-outage-on-july-2-2019/) , though with a much smaller blast radius, not an outage that blacked-out half the internet worldwide.


Also while debugging the outage and metrics delta, a wild clue appears. The data team mentions that our Kafka instance was restarted around the time of the outage.


> *“Pshaw” we confidently say with obvious foreshadowing, “our elixir app can handle a service getting restarted.”*


As we dig into the errors and log messages that have been emitted during this day, doubts begin to creep in about how well our elixir app can in fact handle the restart of a dependent service, spoiler alert, not well at all!


We are also able to correlate the Day3 and Day33 outages with this outage a pinpoint a Kafka restart as the genesis of the three non-deploy related outages. This is surprising turn of events and a relief, but also another mystery.


Kafka is a dependency of an app that is a transient dependency of all the user facing _web apps. This is pretty far down the supervision tree, so why is it able to cause a crash to bubble all the way up to the web-serving apps?


The answer, it turns out is both obvious and not at all obvious. The obvious part of the answer is that the way we setup our supervisor configuration uses default max_restarts and max_rerties values. This causes a subset of failures to restart fast enough to trigger an app crash which bubbles up into the _web app and cause those apps to fail.


The more nuanced answer is that there are some processes within our Kafka app that run into a race condition whereby the inbound request attempts to send kafka messages through a process which is no longer alive (probably crashed). Using a reference to a dead process, without a better means of restarting the process or dynamically spawning the process on demand, is not the intended design, but is a side-effect of the non-designed process architecture of the kafka app.


## Managing metastability


Until the May 10 (day 111) incident our system remained metastable and we could never definitively attribute a cause of the crashes. This metastable state is a not uncommon state for complex systems.


> *See*[this whitepaper](https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s11-bronson.pdf) *for a great discussion of the metastable failure mode. See also*[this blog](https://danluu.com/cache-incidents/) *for a discussion of some specific (cache-related) metastable failures.*


One of the signatures of metastable failures is a reduced ability to do work and limited means for a system to recover normally from a metastable failure state. The most reliable way for a complex system to recover from a metastable failure is to load shed. (Nearly) any other strategy will just move the metastability around into another, as yet undetected layer.


Complex systems need some ability to protectively drop work. In some cases this is a simple as restarting the system ( *Have you tried turning it off an on again?* ) to flush out the existing queued work and reset any potentially invalid states. But occasionally this is not sufficient on it own and an engineer needs to intervene at a higher level to load shed in order to protect the system so it is able to first recover into a stable state and then begin to process work again.


## Diagnosing metastability


Diagnosing metastability is a hard problem. It often involves understanding disparate parts of complex systems. In a functioning state this can be a difficult thing to grok. When failing, the urgency of the incident compounds the difficulty of sifting through the various failures to find the relevant errors.


The diagnosis of this problem was complicated by the volume of error logs that our system generates. Being diligent about about addressing[transient errors](https://app.datadoghq.com/apm/error-tracking) , instead of being complacent about their presence, is part of the engineering discipline that requires attention and ongoing maintenance.


One thing that eventually became apparent was that the failure states for our site were cause by two separate issues even though the outward failure modes were very similar. Reviewing the timeline it’s clear just how long it took us to understand the causes of the failure behavior we see in the system.


## What did we learn here?


Metastability (appears to be) an emergent property of complex systems, as systems grow in scope and complexity it’s harder to understand all the different pieces. When things are working as designed often the different layers are nearly transparent and have no impact on the load capacity of the system. The implications of degraded performance in one layer can have compounding effects in layers at some remove from the misbehaving layer and sometimes it’s not even clear that there is a connection between these two layers.


**Load shedding.** Metastability has the potential to reduce the load capacity of the system, often dramatically. When metastable failures occur, the system often ends up in a state of (sometimes greatly) reduced load capacity. In this failure mode, the only reliable way to recover stability is some form of load shedding. You have to have a means to protect your system from load so that the system as a whole can recover to a stable state.


**Chaos monkeys.** Metastable failures can very very difficult to replicate without load. They often need more than one trigger to induce the unstable state. In our kafka failures, we could not replicate the failure state. In addition to the restart of MSK, it also requires enough load that requests which trigger processing in the kafka layer fail fast enough to trip the supervisor failures in the supervising application(s). Chaos engineering, chaos monkeys, or chaos wombats are occasionally a popular topic, but implementing this adversarial strategy is an expensive challenge and depending on the design of your system, may not be feasible without significant engineering effort.


**Write more property tests.** This is not solution, on its own, to meta-instability, but if you are confident the code your write can handle unexpected or partial input, then it helps to add some confidence that your code may react better when a dependency misbehaves.


**Think adversarially.** As engineers, we have the tendency to write code for the happy path, and maybe one or two predictable failure cases. This is insufficient. You have to assume that any layer of your application could fail at any time. This includes code, services, interfaces, and entire branches of the supervisory tree. This makes it harder to add services and/or features to the system, but helps to design a more reliable and resilient system. To be sure, these are harder test cases to account for, but even figuring out how to kill a branch of the supervisory tree and what happens to your application afterwards, can be illuminating.


**Defaults matter.** Supervised applications are often just configured so that they work. Sometimes this is sufficient. ***Thanks BEAM!*** But being explicit is also valuable on its own. Even if you want to use the defaults, consider setting them explicitly. It’s a signal to the future engineer who has to investigate this supervised application that you considered some of the crash scenarios. (Based on my experience, that future engineer is just as often to be you. So be considerate of future you.)[Little’s Law](https://en.wikipedia.org/wiki/Little%27s_law) can help to estimate system capacity and this should be able to give you and estimate of the volume of requests likely to flow through your branch of the supervisory tree. Capture the data used in your capacity estimation exercises in the form of in-situ documentation. This documentation may help future engineers to understand what capacity was intended during system design.


All of these suggestions are similar. They share an assumption that you don’t trust the code you write or the system you design. If your designed system has to rely on any third party services your distrust level should be much higher. Plan and test for as many failures as you can. When you run out of imagination, ask your colleagues how your code (and the systems with which it interacts) can fail. You may be surprised by the answers.
