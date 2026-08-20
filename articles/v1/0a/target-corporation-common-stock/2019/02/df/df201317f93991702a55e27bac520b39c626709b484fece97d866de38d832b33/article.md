---
schema_version: "1.0.0"
document_id: "df201317f93991702a55e27bac520b39c626709b484fece97d866de38d832b33"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/chaos-engineering-at-target-part-1"
published_at: "2019-02-05T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:f376675767b9005970d9dc26eb5c538b5cd7af8689559ad533bca02276031a19"
---

# Ɔhaos Ǝnginǝǝring @ Target - Part 1

Chaos engineering is the discipline of experimenting on a distributed system in order to build confidence in the system’s capability to withstand turbulent conditions in production.


[principlesofchaos.org](https://principlesofchaos.org/)


Chaos engineering is to disaster recovery and continuity planning what DevOps was to development and operations not long ago. Across industries and organizations, teams are recognizing the need to do more to improve their systems, so they are experimenting through introducing “chaos.”


How Do You Introduce Chaos?


Chaos engineering requires us to change the way we think about systems and how we can make them more resilient. Instead of depending on disaster recovery plans or major incidents to inform us, we intentionally inject faults into our systems early and often to see how they respond. Through testing in a more controlled and scientific way, we can minimize outages and then apply what we’ve learned to all systems. When we do this, it provides insights into how the systems will behave when there is an actual problem.


Why Chaos?


Building chaos engineering into our best practices enables us to deploy highly robust and resilient systems that are measurable with analytics and financials. You can finally prove a system is as resilient as we know it to be. Think of this as the next evolution of disaster recovery, removing the need for recovery since the system never went down. Practicing chaos engineering will not replace disaster recovery plans; it will, however, enhance them. As a team’s chaos practice matures, its members begin to develop systems with disruptions and recovery in mind. This also leads to an increase in system knowledge and behavior. A positive byproduct of this knowledge is improved documentation, disruption responsiveness (a sense of urgency) and semi-automated recovery practices - which in turn demonstrate the fault tolerance of the system and ultimately result in greater trust in a given system.


Where Target Started Chaos-ing


Back in 2016, the team at Target responsible for re-platforming all of the infrastructure behind Target.com was preparing for the first peak season on brand new infrastructure. In order to get ready for what promised to be a busy peak season, the team decided to embark on a scientific experiment to break the infrastructure it had just built and see what happened, but it didn’t know how.


To begin, team members first identified fault patterns for their platform and then built a telemetry framework to inform them of the most critical failure types. They also created playbooks and escalation polices to help any engineer on the team resolve issues. Over the course of the months leading up to the busiest five days of the year for Target, the team broke every possible thing it could plan for, in lower environments using fire drill/gameday-style chaos testing limited just to the team. The prescribed “chaos-ing” exercised every part of the platform, from alerting and notifications to response protocols and playbooks, as well as engineering knowledge and escalation policies.


What the Team Discovered


Those first chaos tests helped the team understand with greater clarity and appreciation the ways in which applications and their infrastructure could fail in a complex system. In the following years, team members honed their prescriptive disruption with built in-expectations, alert validation, dashboarding and playbook validation, and post-mortem documentation to inform and improve the next round of chaos testing. They started including a broader set of teams in their testing, inviting application teams to observe and participate, report impacts and ultimately help break things in the name of making them better. The team eventually matured their chaos practice enough to break things in production. But still more needed to be done.


What Target is Doing Today


Today, teams across Target are practicing chaos testing, but they might not be calling it "chaos." It starts by simply wanting to verify that a recovery playbook is correct, like the team did above. So teams set aside some time and manually go break a part of their system. The goal is to verify that alerting is in place and recovery can happen quickly. This is a


fantastic


start! It brings together all of the resiliency principles to the front of their thinking and is fundamentally shifting the way they design and develop systems.


It can be difficult to codify what all of the teams at Target are learning and how they are evolving their processes, just like in any large organization. So today, teams self-organize and share their tribal knowledge every chance they get. Communicating about results and learnings is critical given the nature of system interdependency and complexity, especially at Target. Chaos engineering needs to break down the communication silos to enable the resiliency of systems.


The Future of Chaos Engineering at Target


For large enterprises, the challenge lies in developing/adopting a vetted set of tools and services that can run at scale and allow teams to run more tests more often. When we have the right tools and fault patterns in place, every team can run their experiments quickly and with less risk. By building these tools into the platforms where applications run, we remove the barriers preventing the best resiliency practices from emerging. Currently, Target’s Chaos & Resiliency Engineering Enablement team is evaluating several tools and techniques for conducting chaos experimentation. These are the first and fundamental steps in building a “Culture of Chaos” at Target.


Stay tuned for more stories about Target’s journey with chaos engineering.


## RELATED POSTS


### Ɔhaos Ǝnginǝǝring @ Target - Part 2


By Brian Lee and Jason Doffing, May 9, 2019


What is a Game Day?
