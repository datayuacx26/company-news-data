---
schema_version: "1.0.0"
document_id: "a7f3415dbdeebc72ae5bd5684470b7332bbaf06be53c806fb9066926d913f154"
company_key: "yc-aragorn-ai"
company: "Aragorn AI"
source_id: "yc-aragorn-ai-news-import-274f7d45efe4"
canonical_url: "https://www.aragorn.ai/articles/plaid-workday-integrations-configuration-not-projects"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-26T22:42:00.205385+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:9059620e368d011738443cc1d851ee14dc5441a56b3bccbcbee7cf078a198622"
---

# Plaid Turned Workday Integrations Into Configuration | Aragorn Case Study

## Plaid turned Workday integrations into configuration, not projects


**Company Size:** 1500+ **Industry:** Financial technology


How Plaid's People Systems team launched 17 Workday integrations on a single reusable pattern, and compressed nine-month roadmaps into a single quarter.


17


Workday integrations live


8x


Reduction in monthly integration workload


< 1 quarter


To deliver work that previously took 6 to 9 months


$10K


Consultant cost avoided per integration


### Key Results


- 17 Workday integrations live on a single reusable pattern
- 5 to 6 integrations delivered in under one quarter, work that previously required 6 to 9 months
- 40 to 90 hours of net-new build effort avoided per integration after the first deployment
- Stakeholder count per integration reduced from 4 to 6 cross-functional teams to a core People Systems workflow
- Integration-related workload at the function level moved from approximately 320 hours per month to approximately 40 hours per month
- Up to roughly $10,000 in consultant cost avoided per integration


Plaid is a financial infrastructure company powering digital banking and fintech experiences for thousands of businesses and millions of consumers. With more than 1,500 employees, Plaid operates across a growing ecosystem of HR, recruiting, benefits, and finance systems that must stay continuously aligned.


Before Aragorn, integrations consumed the majority of the People Systems team's time. The team was operating at maximum capacity. A one-way Workday feed took roughly 40 hours of build effort. A bi-directional feed ran 80 to 90 hours. Each integration touched four to six stakeholders across People Tech, vendors, FP&A or Benefits, IT, and often outside consultants. Calendar time, measured end-to-end, sat in months. Per-integration consultant cost ran up to roughly $10,000 when external capacity was required.


Plaid was not short of expertise. Plaid had Workday. Ownership of integrations sat in the right place. What was missing was a layer that turned a Workday event into a downstream action without requiring a new project every time.


### Why the conventional alternatives don't solve this


The team evaluated three options. Workday Studio produces integrations as code, which the People Systems team has to maintain. Generic iPaaS connects systems but requires modeling HR concepts inside a vertical-agnostic platform. Consultant-built feeds deliver quickly but keep operational expertise outside the team that owns the workflow. None of these change the operating model. Faster individual builds with no shared pattern still produce a function in which every integration is a project.


### What Plaid built instead


Plaid stood up the Workday-to-Aragorn pattern with People Tech and Benefits only. No external consultants on the initial integrations. The architecture sat on three elements: Workday RaaS reports as the source, an Aragorn project as the orchestration layer, and an integration back to Workday or out to a vendor as the destination.


Once that pattern was in production, each new integration stopped being a build. It became a configuration exercise. Align Aragorn custom fields with the relevant Workday RaaS report fields. Set up the vendor mapping. Run.


The first production wave was benefit carriers. Five to six went live in under a quarter. Seventeen integrations are now live, all on the same Workday-and-Aragorn pattern.


Aragorn is built around People data models, with native use of Workday RaaS reports and People events as triggers. Field alignment between Aragorn and Workday reports happens directly inside People Tech, without filing a ticket. Most changes, including adding fields, adjusting mappings, and re-running failed syncs, sit inside the People Systems team. Consultants are rare. IT involvement is bounded to one-time SSO and networking setup.


### The operating model changed, not just the speed


Delivering five to six integrations in under a quarter was the visible outcome. The more important change happened inside the People Systems function itself.


Before Aragorn, integration work consumed approximately 320 hours per month across build work, debugging, reconciliation, and stakeholder coordination. After establishing reusable Workday-to-Aragorn patterns, that workload dropped to approximately 40 hours per month.


The difference was not simply faster delivery.


Plaid stopped treating each integration like a standalone engineering project.


Once the core orchestration pattern was established, most new integrations became configuration and workflow management instead of custom build work.


The People Systems team now operates more like a product organization for HR workflows, owning automation, data quality, and operational reliability end-to-end.


> Before Aragorn, we were always at max capacity and integrations took up the majority of our time. Now we have Workday-native workflows and Aragorn orchestrating the data flows end-to-end, with clear ownership, logs, and dashboards. The biggest difference is that People and Finance can move at the speed of the business without sacrificing data quality or compliance.
>
>
> - Mai Herr (Head of People Technology)


### Takeaway


HR integration problems are rarely integration problems. They are operating model problems.


Plaid's seventeen integrations are not the proof point. The cost structure of the function is. The first integration is still a build. The next one is not. That difference compounds. When the orchestration layer exists, integrations become configuration, workflows become system-driven, and the People Systems team stops being a project queue.


Not connection. Control.


### See what HR looks like when it controls its operating layer


Aragorn is the People Operations Operating System. Workday-native orchestration, reusable integration patterns, and end-to-end visibility for the team that actually owns HR.


[Book a working session →](https://www.aragorn.ai/get-started)
