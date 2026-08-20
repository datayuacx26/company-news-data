---
schema_version: "1.0.0"
document_id: "0777b3bf54e0409ae8623b57ad70fd4769d6d98856652f15d45ec8183626f4f3"
company_key: "yc-aragorn-ai"
company: "Aragorn AI"
source_id: "yc-aragorn-ai-news-import-274f7d45efe4"
canonical_url: "https://www.aragorn.ai/articles/plaid-backfill-workflow-workday-triggered-system"
published_at: "2026-05-22T00:00:00+00:00"
first_seen_at: "2026-07-26T22:42:00.205385+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:be4bcb439203af485e3c488643c677709f7917011a69f092f2151f7688d022b2"
---

# How Plaid Rebuilt Backfill as a Workday-Triggered System | Aragorn Case Study

## How Plaid replaced email-driven backfills with a Workday-triggered system


*Termination in Workday triggers the questionnaire, the approval, the requisition, and the position close. Hourly. Auditable. Owned end-to-end by People Systems.*


**Company:** Plaid **Industry:** Financial technology **Workflow:** Headcount backfill and position close **System of record:** Workday **Orchestration layer:** Aragorn


200–500


Manual hours saved annually on backfill


Hourly


Workday-triggered requisition and close runs


1–3 hrs


Per week recovered for FP&A


1–2 hrs


Per week recovered for managers


### Key Results


- 200 to 500 manual hours saved annually on the backfill workflow
- 1 to 3 hours per week recovered for FP&A
- 1 to 2 hours per week recovered for managers
- Workday RaaS pulled four times daily, Aragorn integrations run hourly
- Segregation-of-duties exposure eliminated by structurally separating initiation from approval


Backfill at Plaid was slow because every step lived in email or in a spreadsheet, not because any single component was technically broken. Managers emailed headcount aliases or specific FP&A contacts when a role opened up. Templates varied by department. Approval paths varied by department. Approvals lived in email threads.


FP&A often initiated and approved the same request, which produced a segregation-of-duties exposure that no attestation policy could fully clean up. People Ops and recruiters were pulled into the thread every time it stalled. Backfill status lived in spreadsheets that drifted out of sync with Workday.


The People Systems team had Workday and the right data already. What they needed was an orchestration layer that could treat Workday events as triggers and run the resulting workflow end-to-end.


### What Plaid built


The team built two Workday RaaS reports: Create Backfill Job Req and Close Position. Aragorn projects pull from those reports four times daily. Integrations run hourly to create requisitions or close positions in Workday as appropriate.


When a termination posts in Workday, the manager receives a backfill questionnaire task with four options: backfill, downgrade, delay, or close. Approvals route automatically to the Function Lead and FP&A. If approved as a backfill, an Aragorn integration creates the requisition on the next hourly run. If closed, the position closes on the next hourly run.


The path is identical regardless of department. The audit trail lives in Workday and Aragorn logs. No email. No spreadsheet.


### The workflow became a system


Backfill is no longer a coordination workflow. It is a system workflow.


Ownership is now unambiguous. Managers own the decision. FP&A approves rather than initiates and approves. The system carries the rest. The segregation-of-duties concern that the prior email-driven process produced was eliminated structurally, not policed by attestation after the fact.


### Quantified Results


Each savings figure ties to a defined population doing defined work the system now handles. None are bundled together.


**200 to 500 manual hours saved annually** across managers, People Ops, recruiters, and FP&A. The range reflects variation in backfill activity across quarters.


**1 to 3 hours per week recovered for FP&A.** Time previously spent receiving, routing, and approving backfill emails, plus reconciling spreadsheet headcount trackers against Workday. Now spent on financial planning rather than approval logistics.


**1 to 2 hours per week recovered for managers.** Managers no longer draft templated emails to request a backfill, follow up on silent threads, or respond to clarifying questions. The Workday questionnaire replaces the cycle.


> Now we have Workday-native workflows and Aragorn orchestrating the data flows end-to-end, with clear ownership, logs, and dashboards.
> - People Systems Team


### Takeaway


At Plaid's scale, this replaced informal coordination with a system. The hours saved are the second-order effect.


The structural change is that approval, initiation, reconciliation, and audit no longer live in email and spreadsheets. They live in Workday and Aragorn logs. The same operating logic now applies across departments, which is what consistency in an HR workflow actually requires.


This is what HR workflow automation looks like when the orchestration layer is owned by the People Systems team. Not faster email. Not better templates. A different operating model.


### See what HR looks like when it controls its operating layer


Aragorn is the People Operations Operating System. Workday-native orchestration for the workflows HR has always been on the hook for and never been able to fully own.


[Book a working session →](https://www.aragorn.ai/get-started)
