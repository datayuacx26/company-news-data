---
schema_version: "1.0.0"
document_id: "f172b0784a479c31d6527ad90b99202ce05f8572d57e6e449f631bf22122d8eb"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2022-07-18-sre-on-call-procedure-at-trivago/"
published_at: "2022-07-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:431f4f868495432a3ab03b9d7af5899243e0da7f92ed6685519a96daab3fa39b"
---

# SRE: On-Call Procedure at trivago

SRE: On-Call Procedure at trivago


## Introduction


One of the many responsibilities of a Site Reliability Engineer (SRE), is to ensure uptime, availability and in some cases, consistency of the product. In this context, the product refers to the website, APIs, microservices, and servers. This responsibility of keeping the product up and running becomes particularly interesting if the product is used around the world 24 hours every day like trivago. And just like in the medical profession, someone has to be on call to react on failures and outages outside of the office hours.


## About trivago Product and Infrastructure


trivago is a metasearch that offers hotel and accommodation price comparisons and deals. At the core of this product are several microservices that run independently but harmoniously to ensure we provide our users with great experiences.


trivago’s infrastructure relies heavily on Kubernetes, with many clusters spread across multiple regions. It is important to set this premise here to help you understand our approach to on-call.


## On-call onboarding


Every engineer at trivago has to go through the on-call onboarding process before being involved in on-call rotations. The main goal here is to get the engineer acquainted with being on-call at trivago, how to do it, and how to excel at it. The onboarding process includes the signing of some legal paperwork, attending seminars organised by one or two more senior engineers who have had experience being on-call at trivago, elevated access rights, and shadowing other engineers on-call. The on-call shadowing is considered a necessary stage in the onboarding process. On-call shadowing means that the new engineer is actually on-call but is only expected to learn rather than mitigate issues as they occur. So, they will receive alerts, join the ‘mitigation’ zoom call sessions, and be actively involved in writing post-mortems. The key objective here is for the engineer to learn how to troubleshoot issues, communicate effectively, and escalate and document incidents as they occur.


## On-call Scheduling


At trivago, there are always two engineers on-call at the same time, 24 hours a day. The scheduling and duration are managed in Opsgenie.


## Rules of engagement while on-call


The rules of engagement are the procedures that the on-call engineer follows to detect, communicate and resolve an incident as they occur. We adopted the on-call principles of the emergency medics; check for the following in this order: critical bleeding, airway, breathing, circulation, disorientation, etc.


What is common between the emergency medics and SRE at trivago is that they are both the first responders to incidents.


An on-call engineer at trivago follows these procedures:


-


**Acknowledge the alert** . There are 3 channels to receive alerts: Email notifications, Slack messages, and phone (calls or alarms). The very first thing we do when we receive an alert is acknowledge the alert. This lets our incident manager know that someone is available to attend to this alert. An unacknowledged alert will be escalated to other engineers or even the managers after multiple unresponded notifications to the on-call engineers. This is a rare case but it is a mechanism put in place to ensure that an incident is not left unattended.


-


**Understand the alert** . Every alert contains a detailed error message, a Grafana dashboard link, and a link to the runbook for the failing service. The first step is to read and understand the error message. This step is important to us because it leads the engineer to the area of “critical bleeding”. An example of a typical alert is pod “crashloopbackoff” in a kubernetes cluster.


An alert is considered an incident if, after a couple of minutes, the alert cannot be resolved and it has an impact on the entire system and the users.


Sometimes, an incident is a result of an upstream or downstream service issue. This is why we also take a look at those. This is synonymous with checking the “Airways” and “Breathing” mentioned above. The goal is to ascertain if the incident is an isolated one.


-


**Verify incident impact** . From the understanding of the incident, we look at some critical dashboards to ascertain the impact of the alert. Different alerts have different priority levels and depending on the level, the urgency of the fix and its impact on the business need to be communicated.


-


**Communicate** . Effective communication is required while on call, not just when there is an incident. While resolving an incident, one of the on-call engineers (incident commander) is expected to leave a short and descriptive message on the outcome of the entire process. A failing Kubernetes pod may just be a result of an even bigger issue.


In some cases, the engineer is expected to apply a quick fix and then write a detailed suggestion for a stable fix to the team.


-


**Apply a fix** . “Stop the bleeding…” After discovering the issue, the next goal will be how to fix it. The fix could be temporary, and if that is the case, the on-call engineer will have to pass the information with proper detail of the incident to the team in charge of the microservice.


## Monitoring


The backbone of our on-call duty is our monitoring setup. Without proper and accurate monitoring, it will be like driving at night without headlights. We have a robust and resilient monitoring stack built with Prometheus, Thanos, and Grafana.


One thing is clear from the above architectural diagram; we care about our monitoring setup. What we also care about while on-call is getting to the right Grafana dashboards in a split of seconds after receiving an alert. For that reason, every alert message contains the following:


- The error message
- The impacted service
- The Grafana dashboard to look at
- The runbook to help debug and fix the error


## Escalations


While the on-call engineers are the first responders to any incident, alerts can be escalated if any of the following happens:


1. The alert was not acknowledged by either of the engineers on-call after repeated alerting by Opsgenie (our on-call tool)
2. The severity of the alert is at a much larger scale and will require a ‘war room’ kind of resolution. This can be done by manually adding the responders to an alert or incident. War room in this context refers to engineers coming together to mitigate an incident. This could be done over a Zoom call or with the use of the Opsgenie’s Incident Command Center.


## Postmortems


Postmortem are written with these questions in mind:


- How many of our users were affected?
- How many of our services were degraded?
- What were the learnings from the incident?
- What is the financial impact of the incident?


At least, one of the above questions has to be answered before we can write a postmortem. We write a detailed postmortem that highlights the incident, its impact on the business (if any), the steps that were taken to resolve them, and actionable steps to forestall future occurrences.


## Alignment meetings


Finally, we have weekly alignment meetings where we discuss everything that happened in the past week. This discussion centers mostly on the on-call engineers giving an account of an incident that was encountered during the week. It is the practice that new on-call engineers become part of these meetings and ask clarifying questions as these discussions are going on.
