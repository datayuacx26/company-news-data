---
schema_version: "1.0.0"
document_id: "e99f7771003d2284fc0d00a4e10773903e61b8c7606deb41ea314b59fa8bee69"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/2023-03-08-deep-dive-into-incident-response/"
published_at: "2023-06-01T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:0ea8ad12fc55d50c696f3f9305462854bee0d91190a1047987a6c09ce045ab70"
---

# 2023-03-08 incident: A deep dive into our incident response

Laura de Vesine


In March, Datadog experienced a[global outage](https://www.datadoghq.com/blog/2023-03-08-multiregion-infrastructure-connectivity-issue/) . It was the first of its kind and called for a massive response that involved several hundred engineers working in shifts over the course of the outage, in addition to many concurrent video calls, chats, workstreams, and customer interactions. Within days, we had compiled hundreds of pages of internal deep dives and after-action reports.


Like many of you, as custodians of large-scale, complex systems, we have always lived with the realistic expectation that a massive outage was bound to happen one day—not *if* but *when* . We had been doing our best to be prepared, and gradually improving our incident response in order to meet disaster head-on. On March 8, 2023, that day came, and we were put to the test.


This post describes what our incident response looks like in general, where it succeeded and stumbled on March 8, and what we learned in the process.


## Datadog’s incident response process


### How we monitor


Datadog manages operations and incidents in a “you build it, you own it” model, and we follow industry-standard practices that large-scale platforms require. All of our systems—even those that might not directly affect the customer experience—are instrumented to provide many kinds of telemetry data to our teams. Teams configure monitors to track data 24/7 from the services they build and operate, so they will be alerted if there is a problem. Most engineering teams are expected to be able to respond to these alerts within a few minutes.


In addition to our Datadog-based monitoring, we also have basic, out-of-band monitoring that runs completely outside of our own infrastructure. That monitoring does not make any assumptions about how our platform is built and consumes our APIs exactly like users do. This is how we monitor the monitors—and ensure that we are alerted even in the rare event that our platform becomes largely unavailable (as it was on March 8).


We use Slack to support situational awareness of ongoing incidents across engineering. The Datadog incident app automatically creates a Slack channel to coordinate each incident. All incidents are easily visible at any given time, and engineers who were not directly paged are encouraged to join if they can help.


### The case of high-severity incidents


In addition to our many team-specific on-call rotations, we have a rotation of senior, experienced engineers who are on call for high-severity incidents (i.e., when customer impact is substantial or many teams need to be involved with remediation). After a member of this rotation is paged, they join the incident and take on the incident commander role.


The incident commander may personally manage communications and status updates among internal responders or delegate that role to another responder, who is often the secondary on-call for our high-severity incident rotation ( *communications lead* in the diagram). For the highest-severity incidents, we also page an engineering executive and a manager from our customer support team ( *customer liaison* in the diagram) to compose and coordinate customer-facing communications, and to provide any necessary business context. The incident commander remains responsible for the overall response to the incident.


### Practice makes perfect


Because we (like our customers) have high expectations for availability, we have a relatively low threshold for declaring incidents. A secondary effect of this is that we regularly undergo our incident management process, which helps keep our engineers up to date on tooling and incident response. We also require all engineers to complete comprehensive training before going on call and a refresher training session every six months. This training covers the expectations of going on call, the structure of our incident response process and team, and the expectations for blameless and thorough incident investigation and remediation.


We also follow best practices around preventing repeat outages by hardening and improving our systems. This means that each outage (especially the largest ones) is unique and unprecedented, so flexibility and creativity are key components in a successful response. For every high-severity incident, our engineers write a detailed postmortem exploring how we can prevent it (and similar incidents) in the future. Automation reminds engineers involved in incidents to draft a postmortem while the incident is still fresh in their minds, and we encourage folks to reach out if they need any support.


### Autonomy without blame


Like most large-scale systems, ours is constantly changing as we add features and scale, making it essentially impossible to keep pre-baked recovery procedures up to date. Rather than implementing detailed (and inherently rigid) procedures, engineers are empowered to make judgments about the best path forward for the services they know well. In agile parlance, this is placing “people over process,” and it helps ensure that we live up to our overarching philosophy of ownership.


We value our blameless incident culture, which empowers engineers to find creative solutions when responding to incidents. When complex systems fail, we know it is not because an individual made a mistake, but because the system itself did not prevent the failure. We demonstrate this across all levels, from individual engineers who are not blamed for incidents to executives who offer support and encouragement when responding to serious outages. Blamelessness is especially important in stressful situations like high-severity incidents.


## Trial by fire: Our initial response to the March 8 incident


### Timeline


The early minutes and hours of an incident are crucial. These precious moments condition a lot of the subsequent response: what we think happened, where to look for the trigger, what our initial message to the outside world should be, and even the mood of the group as more and more people join to troubleshoot. So we pay attention to the onset of our incidents and try to remove as much friction as possible. Here is what our timeline for March 8 looks like:


-


06:00 UTC:[Systemd upgrade](https://www.datadoghq.com/blog/2023-03-08-multiregion-infrastructure-connectivity-issue/#root-cause) starts, triggering the outage


-


06:03 UTC: Monitoring detects a problem with Datadog


-


06:08 UTC: Two engineering teams are paged


-


06:18 UTC: High-severity incident is opened


-


06:23 UTC: Incident commander joins response


-


06:27 UTC: Executive on call joins response


-


06:31 UTC: First status page update is posted


-


06:32 UTC: Global outage is officially diagnosed


-


06:40 UTC: Additional responders join for triage


-


07:20 UTC: Kubernetes failure identified as cause of global outage; intake identified as unhealthy


-


08:00 UTC: We validate that the Kubernetes failure is no longer happening on more nodes or new nodes


-


08:30 UTC: A working mitigation is identified for EU1


-


11:00 UTC: Most compute capacity in US1 automatically recovered; we begin handoffs for “the long haul” recovery


-


11:36 UTC: Unattended upgrades identified as incident trigger


-


12:05 UTC: Compute capacity (the first step to recovery) recovered in EU1


-


15:15 UTC: Compute capacity recovered in US1


-


15:54 UTC: We prepare and roll out mitigations to prevent a repeat failure


-


18:00 UTC: EU1 infrastructure fully restored


-


19:00 UTC: US1 infrastructure fully restored


### Declaring the incident


At about 06:08 UTC on March 8, 2023 (about 1:08 a.m. local time for the first responding teams), two engineering teams were paged for this incident:


-


A team working on our APM product, whose automation detected that their Kubernetes pods were not restarting properly


-


The team that received pages from our out-of-band monitors for issues with our own alerting systems


The first team assumed that the issue was local to their service, since there was not (at that moment) a large incident declared for issues with our compute infrastructure.


Meanwhile, the second team quickly determined that we had substantial issues across our products and called a high-severity incident, although they were hindered by intermittent incident-related failures of our incident management tool. As a result, it took us about 10 minutes (which was longer than usual) to open a high-severity incident once it was diagnosed.1


Our on-call high-severity incident responder got online and quickly diagnosed that this was an extremely severe incident. They assumed the role of incident commander and immediately:


-


Triggered escalation to the executive and customer support on-call rotation


-


Paged in the high-severity incident response secondary to provide additional support


-


Attempted a fast triage of what was actually happening—what was affected, and how badly?


### The communication clock is inexorably ticking


At this point, our incident commander had two priorities: communicating our status to customers, and driving diagnosis and mitigation of the system failure.


Failures in complex systems do not make it easy to decide if you should communicate first, or diagnose and mitigate first. For one thing, what to communicate beyond “the system is down” depends on what the initial diagnosis is and what the path to resolution looks like early on. Conversely, a lack of timely, meaningful updates is frustrating, as it prevents customers from planning their own actions and response. Over the years, we have found that a 30-minute update cadence is a reasonable balance and that it gives the response team time to focus on diagnosis, mitigation, and remediation.


### Hurdles out of the gate


Early on, it was clear to the incident commander that we needed to get a status page up to inform customers as soon as possible. However, posting accurate status page notices quickly presented some challenges because different regions were impacted differently. Some of our regions were experiencing issues loading the webpage and others were not, so we chose the “worst behavior” message (which indicated those loading issues as the most prominent symptom) for the sake of communicating with customers as quickly as possible.


Diagnosis and mitigation were unusually challenging as well. Datadog’s regions are fully isolated software stacks on multiple cloud providers. In these first few minutes, separating out and accurately identifying the[differing behaviors on different cloud providers](https://www.datadoghq.com/blog/engineering/2023-03-08-deep-dive-into-platform-level-impact/#cloud-provider-impacts) —combined with the fact that this outage affected our own monitoring—made it difficult to get a clear picture of exactly what was impacted and how. This remained a challenge throughout the early hours of the incident, both for accurately understanding the scope and depth of customer impact (which varied by region) and for beginning troubleshooting and mitigation. Because of our gradual, staged rollouts to fully isolated stacks, we had no expectation of and little experience with multi-region outages.


### Scaling our response with workstreams


Continuing to focus on the dual priorities of communication and ending the impact to customers, our incident commander kicked off two parallel workstreams that focused on:


-


Developing a better understanding of the impact to customers and posting more accurate communications, especially around whether we were experiencing issues with accepting customer data at intake


-


Diagnosing the cause of our global failure and mitigating its impact


For the first workstream, we paged responders from many engineering teams to assess how and whether their products were functioning for customers. This effort was hindered by the multimodal nature of the outage and the loss of most of our own monitoring, so it took tens of minutes from this point to determine the health of our intake systems. Doing so depended in part on realizing that the[causal failure was based in our compute and Kubernetes infrastructure](https://www.datadoghq.com/blog/engineering/2023-03-08-deep-dive-into-platform-level-impact/) .


### Debugging is telling the difference between “ought” and “is”


Diagnosing the failure required imagination and suspension of disbelief. At that time, responders in the room were not aware that we had any channels that could make updates to all of Datadog at the same time, and we knew that our different regions did not share any infrastructure. We had to force ourselves to identify the facts on the ground instead of “what ought to be,” and overrule our instincts to look for data in the places we normally looked (since our own monitoring was impacted).


To find the cause of the incident, we pulled in three teams a few minutes after we declared a major incident:


-


The networking infrastructure team that manages connectivity between services


-


The team in charge of managing our web UI, because they had made the last change to a shared configuration utility that we thought might have somehow behaved globally


-


Our compute infrastructure team, because some pods were either failing to start or stuck in restart loops


Once these teams joined the response at around 06:40 UTC, it took us about 30-40 minutes to identify that the underlying issue was failure across our Kubernetes nodes, and another 30-40 minutes to verify that the issue was no longer happening on any more nodes or new nodes.


### Incident responders run toward the problem


During this time, a large number of on-call engineers in both the US and EU noticed the activity in the shared Slack channel we use for announcing high-severity incidents. In spite of the fact that it was still outside their business hours, they began voluntarily joining the response, often without being directly paged: a clear demonstration of our proactive incident response culture in action. These engineers used their incident training to (mostly) self-organize into product-based response workstreams by using a new feature of our[Incident Management](https://www.datadoghq.com/product/incident-management/) product that we were dogfooding. These managed workstreams helped substantially for keeping track of the response and ensuring that all responders were on the same page about priorities.


Within the first hour, we had more than 50 engineers involved in the response. We would eventually open almost 100 workstreams, which involved between 500 and 750 engineers working in shifts to recover all our products and services.


### Scaling the response again


Our compute infrastructure team was able to identify a working mitigation for our EU1 region at about 08:30 UTC. At this point, the incident commander laid out an overall plan for recovery and pulled in engineers from relevant teams. Throughout the incident, responders were coordinated (generally working to a strategy that prioritized getting our customers access to usable live data) without feeling overwhelmed or distracted by the mitigation efforts and needs of other teams. This remained true even as our priorities pivoted several times (for example, as we realized that our AWS region was actually substantially more impacted than other regions, which had initially been obscured by the fact that our stateless services on AWS appeared to self-recover).


As the response got larger, we identified the need to designate workstream leads to act as incident sub-commanders. This generally worked well and helped reduce overhead for our main incident commander, but not every sub-commander understood their responsibilities completely (as this is not a process we actively train on). However, engineers’ regular experience with smaller team-scoped incidents and with observing our response to larger incidents generally helped them define the role.


Our ability to repair our infrastructure was impacted by our lack of global control surfaces. Our separated stacks meant that rather than recovering everything with a single set of actions, we needed to fix each region individually.[The damage in our EU1 region was most obvious](https://www.datadoghq.com/blog/engineering/2023-03-08-deep-dive-into-platform-level-impact/#cloud-provider-impacts) , and business hours had begun in the EU, so we initially prioritized mitigation there and then moved on to include US1 when we had engineering capacity.


### A sustainable long-haul response


By 11:00 UTC (5 a.m. local time for US-based responders), we knew this would be a[lengthy recovery](https://www.datadoghq.com/blog/2023-03-08-multiregion-infrastructure-connectivity-issue/#recovery) . Having teams on call for their own services and a well-staffed incident rotation gave us ample personnel, so we were confident that responders could hand off without significantly impacting the length of recovery. Our main incident commander initiated handoffs, which were maintained throughout the incident.


Although we were continually responding to this incident at a company-wide scale for nearly 48 hours, our incident commanders made sure that no responder needed to be active for longer than eight hours at a time. This, along with our blameless incident culture and the support from our executive team, allowed us to continue solving novel problems as they arose throughout our repair operations.


## Communications challenges


Throughout this incident, we faced a number of challenges with communications. We intended to share what we knew as soon as we had enough confidence to share it, but struggled at times to clearly communicate enough detail with the right set of people.


### Status pages are blunt instruments


Like all SaaS providers, our primary mass communication method for customers is our set of[public status pages](https://status.datadoghq.com/) . Unfortunately, a status page is poorly suited for conveying information when an unknown amount of work will be needed to restore functionality for customers. It is also a very blunt tool to describe the impact on a multi-tenant platform: Is it better to describe the worst case, the average case, or the best case that any customer may encounter? At what level do we decide to switch from an individual, in-product informational banner to a status page update? These questions need to be answered for each incident and the answers are not always obvious.


Over the years, we have developed heuristics and a set of initial status page updates for each product. But this time was different: On one hand, there was no mistake about how widespread the issue was. On the other hand, we did not have any language prepared, and though we were making progress towards resolution, our updates failed to convey the nuances of our recovery.


Finally, communicating per-product status page updates remained challenging throughout the incident. We needed to share updates for almost two dozen products across multiple regions. Getting these updates required reaching out to the teams responding for each product, and the impact was not always easy to define (or possible to monitor while our own systems were still affected). This meant that we were not always able to provide timely updates to customers on precisely how they were impacted throughout the outage.


### Keeping internal teams informed is imperative too


Many customers naturally turned to our support team for answers. When customers take the time to reach out to us, we consider it a matter of elementary courtesy to take the time to respond, and we try our best to make sure that each ticket receives a specific and relevant answer.


This exceptional incident stressed our response in ways we had not initially imagined. We received about 25 times more tickets than usual over the first 12 hours of this incident. The massive scale of this response meant that a larger-than-usual number of support engineers were involved and needed to speak with the same voice and get on the same page without having to ask the rest of the response team the same questions multiple times.


We were not always successful in giving all of our support engineers the information they needed to help customers understand how they were impacted and how far along we were in the recovery process—at least, not initially. Here again, the latitude we gave people involved in the response quickly led to spreadsheets and documents built on the fly to disseminate the state of the various recovery efforts in an intelligible way to our internal teams, who would then relay the information to our customers.


We received about 25 times as many tickets as usual during the first 12 hours of the March 8 incident. The number of tickets received during a typical hour is shown as 1x. We received about 25 times as many tickets as usual during the first 12 hours of the March 8 incident. The number of tickets received during a typical hour is shown as 1x.


### Cadence matters


We train our incident commanders to reassess impact and update our customers at least once an hour. This is particularly important at the onset of the incident, when responding is more important than waiting for the whole picture to emerge. Still, in the case of this global and severe outage, it meant a long series of mostly identical and short updates (“We are on it.”) In hindsight, we wish we had a more informative message to share at the time.


As the incident progressed and we began to restore services, we developed a number of regular check-ins to improve our overall customer communication. Every hour, our communications lead would check in with our engineering workstream leads to ensure that we could update our status page communications. Around every 40 minutes, our on-call executive gave updates on what we knew and what had been repaired on the engineering side so that our support engineers and CSMs could communicate that information to customers. These executives were also responsible for ensuring and supporting these communication cadences throughout the incident.


## What we have learned


Overall, we tried our best to resolve the issue as fast as we could. Considering the magnitude of the incident, the fact that it was our first global outage, and the number of people we needed to communicate with, we were positively surprised to see that our response scaled to a level commensurate with the magnitude of the incident.


Looking back, we benefited from a few factors:


-


The willingness of the responders to join the fray and help


-


The sense of ownership they get every day from building products, on the good days and the bad days


-


The autonomy they are given as a necessity of the response


-


The absence of blame during and after each incident


-


The regular cadence of smaller-scale, incident dress rehearsals that come with operating large platforms


-


The formal training we have given everyone who “carries the pager”


That said, not everything worked out as well as we would have hoped. We see a number of areas where we want to improve.


We will improve our internal response by:


-


Making it easier and faster to identify which parts of Datadog are most important to address first in an incident.


-


Improving our training and implementing practice drills to equip engineers to handle the special circumstances of the rare all-hands-on-deck events.


-


Refining per-product, out-of-band monitoring, which will help us even if our internal monitoring is down.


We also know that our customer communications did not meet the standard our customers expect from us, especially in terms of detail and specificity. We’re making a number of significant improvements to address this:


-


An overhaul of our status pages that better aligns its components with our products as customers see them. This includes clear definitions for “degraded behavior” and which critical user journeys are affected for each product on the status page.


-


Tested and documented ways to communicate any workarounds or mitigations available to customers during an outage.


-


More structured and rehearsed handoffs with the teams at Datadog who speak directly with customers; this includes customer support, customer success, technical account managers and account executives. The smoother the flow from the responders, the faster and more reliable customer updates will be.


## In closing


March 8, 2023 has been a humbling experience for all of us; it has also been an opportunity to learn and rise to the occasion in many different ways. Each lesson here has given us a refined understanding of what it means to run and repair software at scale, in an organization that is continually evolving. We are committed to taking those learnings and putting them into practice.


1.


We do have fallback procedures in case an incident affects our incident management tooling. In this case, we believe that utilizing this rare option would not have resulted in an overall faster response.↩


-
-
-
