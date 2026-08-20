---
schema_version: "1.0.0"
document_id: "39711eace370b9a6f1553f629b576a6e037790a33af53b939f56dcc556339112"
company_key: "yc-kaizen"
company: "Kaizen"
source_id: "yc-kaizen-news-import-1021911e948a"
canonical_url: "https://www.kaizenautomation.com/blog/prior-authorization-workflow"
published_at: null
first_seen_at: "2026-07-22T01:12:15.212824+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:c5166e1e33bace9b73eb255d863f51ef98affd6fe6e23d690ab1dbdc949b4534"
---

# How Prior Authorization Workflow Works and Where It Breaks

## What is a prior authorization workflow?


A prior authorization workflow is the process payers use to **require advance approval before a service qualifies for coverage** , from the initial order capture through submission, status tracking, and claim attachment.


[An AMA's 2024 survey](https://www.ama-assn.org/practice-management/prior-authorization/fixing-prior-auth-nearly-40-prior-authorizations-week-way) found that practices complete **39 prior authorizations per physician weekly** and spend **13 hours per week managing them** , with 89% reporting that it contributes to burnout.


## The prior authorization workflow, step by step


The workflow touches scheduling, clinical staff, billing, RCM, and sometimes the patient. A clean process protects three things: **patient access, revenue, and staff capacity.**


### Step 1: Capture the order


Scheduling, intake, or referral staff **capture the order within 24 hours** of the provider placing it. The intake record needs patient demographics, insurance plan, ordering and rendering providers, CPT/HCPCS/ICD-10 codes, requested date of service, and clinical reason.


**A missing diagnosis code or wrong member ID** can turn a 5-minute submission into three phone calls and a portal hunt.


### Step 2: Verify eligibility and benefits


The team **confirms active coverage** through real-time eligibility tools (270/271 EDI transactions), clearinghouse lookups, or payer portal queries. Eligibility says **the patient is covered** . Benefits say **what the plan pays for** . Prior authorization asks **whether the payer must approve** the service before it happens.


### Step 3: Check whether authorization is required


Payer rules vary by plan, line of business, state, site of care, and date range. **Staff checks requirements** through[Availity's authorization lookup](https://www.availity.com/authorizations/) , individual payer policy portals, or third-party rule engines.


Payer requirements change throughout the year. **Medicare publishes official quarterly updates** and ad hoc revisions, and **commercial payers** can change requirements **monthly or mid-year with limited notice** .


### Step 4: Gather clinical documentation


The authorization specialist **pulls evidence from the EHR** , document management systems, or shared drives.


**Common documents include:**


- Office notes for the visit that prompted the order
- Lab results and imaging reports
- Medication history for step therapy requirements
- Therapy evaluations for PT/OT/speech
- Medical necessity letters for high-scrutiny cases


**Clinicians supply the clinical argument** ; administrative staff handles the rest. When that line blurs, physicians end up doing data entry.


### Step 5: Submit the request


**Submission lead time depends** on the payer, plan, and service. Each commercial payer (Aetna, United, Cigna, BCBS) publishes its **own decision timelines** , and **complex cases often require additional documentation** that extends the cycle.


For Medicare Advantage, Medicaid, and CHIP managed care plans, the[CMS Interoperability and Prior Authorization Final Rule](https://www.cms.gov/newsroom/fact-sheets/cms-interoperability-prior-authorization-final-rule-cms-0057-f) now requires payers to **issue decisions within 7 calendar days** for standard requests and **72 hours for expedited requests** . Build the team's internal submission cutoff against the slowest payer in your active mix.


### Step 6: Track the status


Every request needs **an owner and a tracker entry** showing submission date, payer, auth ID, requested service, status, expected decision date, and next action. Status checks run **daily on aging requests** and **weekly on routine submissions.**


### Step 7: Work exceptions


Exception queues separate routine requests from cases that need judgment. Each exception type needs its own internal SLA.


**Common targets teams use:**


- **Missing clinical notes:** 14-day response window
- **Peer-to-peer requests:** physician availability within 24 to 72 hours
- **Additional information requests:** typically 14 days
- **Mismatched codes:** clinical review before resubmission
- **Portal errors and locked accounts:** same-day escalation
- **Denials:** appeal or alternate treatment plan


**Peer-to-peer windows** vary by payer. **UnitedHealthcare** , for example, requires **inpatient P2P requests within 3 business days** and **outpatient within 21 calendar days** . Build your internal targets around the strictest payer in your mix.


### Step 8: Attach the authorization to the claim


**The auth ID lands on the claim** through EHR integration, billing field updates, or manual RCM entry. For practices using separate auth platforms and billing systems, the handoff happens via API, CSV, or shared tracker, and **this is where many auths get lost** .


**RCM needs** the auth ID, approved units, date range, service location, approved code, and payer notes. Without those details, the team can still face a **denial after doing the work upfront** .


### Step 9: Handle denials, appeals, and renewals


Standard appeal windows vary widely by payer and plan type. For **Medicare Advantage** , Level 1 appeals (reconsiderations) must be filed **within 65 calendar days** from the date of the organization determination notice.


For **commercial plans** , the windows are **set by contract** : UnitedHealthcare gives providers 65 days, while Aetna, most BCBS plans, and Cigna allow 180 days.


For therapies, specialty drugs, ABA, home health, imaging, and surgery, **renewal tracking is its own ongoing workflow** . Missing a renewal stops billing the same way a denied initial request does.


## Where prior authorization workflows break


Prior authorization breaks when ownership, rules, or documentation are unclear.


**The most common breakdowns:**


- No single queue. Requests live across inboxes, spreadsheets, EHR worklists, and payer portals.
- Unclear payer rules. Staff check the wrong policy or use outdated requirements.
- Missing clinical evidence. The payer asks for notes that should have been in the first submission.
- Weak status tracking. Nobody knows which requests are aging.
- Manual portal checks. Staff logs into payer sites repeatedly just to find "pending."
- Claim disconnect. The auth gets approved, but never lands on the claim.


[The AHA reports](https://www.aha.org/news/headline/2024-06-20-ama-survey-shows-physicians-patients-heavily-burdened-prior-authorization) that 94% of physicians say prior authorization delays access to care, 93% say it negatively affects patient outcomes, and 24% report a prior authorization-related adverse event.


## How automation improves the workflow


Automation handles the **repetitive parts of prior auth** : eligibility checks, payer rule lookups, portal submissions, document uploads, status tracking, and tracker updates. **The clinical decision stays with humans.**


**Provider-side automation** runs eligibility checks, reviews payer rules against the requested service, fills portal fields, uploads documents, captures tracking numbers, refreshes statuses on a schedule, and writes updates back to the team's tracker.


CMS finalized the[2024 Interoperability and Prior Authorization Rule](https://www.cms.gov/priorities/burden-reduction/overview/interoperability/policies-regulations/cms-interoperability-prior-authorization-final-rule-cms-0057-f) to push payers toward **better data exchange** , with API requirements largely due by 2027. Even after that, **payer portals, PDFs, faxes, and manual follow-ups** will remain part of the workflow for years. That gap is where **browser automation closes the loop** .


## What automating prior auth actually changes


The clearest way to see what prior auth automation does is to compare a typical specialist's week before and after.


**Before automation:** Consider a prior auth specialist handling **100 requests a week** . A common pattern looks like this: **25 to 30 hours spent on portal logins** , status refreshes, and tracker updates **across 5 payer portals** . Most of the day is reactive: log in, check, refresh, escalate, repeat.


**After automation:** This is where a browser automation platform built for healthcare ops takes over the repetitive portal work.[Kaizen](https://www.kaizenautomation.com/) is designed specifically for this kind of payer portal workflow. It **runs scheduled jobs** against Availity, United, Aetna, BCBS, and other payer portals, submitting new requests, checking statuses, uploading documents, and **writing results back to the tracker** .


The specialist's morning starts with a **list of exceptions** : peer-to-peer requests, denials, and missing documentation.


## What metrics should prior authorization teams track?


**Useful metrics include:**


- Average days from order to submission
- Average days from submission to decision
- First-pass clean submission rate
- Requests aging by payer, with follow-up thresholds
- Denial rate by payer and code
- Peer-to-peer volume and clinician hours consumed
- Appeals filed and won
- Authorizations expiring within 14 days
- Claims denied for missing or invalid authorization
- Staff hours spent on portal checks


The more useful benchmark is the **team's own before-and-after data** on a single payer or service line.


## When automation pays off


Prior auth is the use case Kaizen handles well. The work is **browser-heavy, deadline-sensitive, payer-specific** , and a constant source of downstream denials. Teams running prior auth across **more than three payer portals with high weekly volume** see the math break open fastest.


For practices with low prior auth volume or work concentrated in a single payer with a strong API, the case is weaker.


**Still chasing status checks manually?**[Book a call](https://calendly.com/kaizenautomation/sales-call) to see how Kaizen handles prior auth workflows for your team.


## Frequently asked questions


### Who handles prior authorization in a practice?


Prior authorization is usually handled by **scheduling staff, authorization specialists, medical assistants, clinicians** , and **RCM teams** . Scheduling flags the need, clinicians support medical necessity, and RCM protects the claim.


### What is the hardest part of prior authorization?


The hardest part of prior authorization is **managing payer-specific rules and follow-ups** . Requirements change by payer, plan, code, service location, and clinical scenario, which makes **the same workflow look different** across each payer.


### Can prior authorization be automated?


Yes, **parts of prior authorization** can be automated. Eligibility checks, payer rule lookups, portal submissions, document uploads, status checks, and tracker updates are **strong automation candidates** . Clinical decisions, peer-to-peer conversations, and appeals strategy should **stay with staff** .


### What should not be automated in prior authorization?


**Clinical judgment** , medical necessity decisions, peer-to-peer conversations, and **appeals strategy** should not be fully automated. **Automation should prepare the file** and route the case to the right person for **human review** .
