---
schema_version: "1.0.0"
document_id: "6e3b599317d27ff1906896202fece99c0a162628675a853b29d037bd66ded04e"
company_key: "yc-mdhub"
company: "mdhub"
source_id: "yc-mdhub-news-import-bbf3c05d7e92"
canonical_url: "https://www.mdhub.ai/blog-posts/spravato-billing"
published_at: "2026-08-13T14:57:13.991+00:00"
first_seen_at: "2026-07-24T11:04:12.059669+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:e04fe0a4430771bcdb4905bb8af40fa8b929c1bfe7f27319f6a64cf8b8dec728"
---

# Spravato Billing Codes Won't Fix a Broken Clinic Workflow

Most clinics that struggle with Spravato billing already know the codes. They have looked up G2082 and G2083. Some are still filing S0013, a code CMS retired on January 1, 2026. The denials keep coming anyway.


The assumption behind every code-lookup guide is that the clinic's only problem is missing information. That assumption is wrong. The real problem is that knowing the right code and reliably applying it are two different things, separated by a workflow gap that no code list closes.


Spravato billing fails at the point where clinical documentation and billing preparation have to happen at the same time, during a two-hour observation window, with staff attention split between the patient and the paperwork. That is not a coding problem. That is a coordination problem.


The sections below cover which codes apply and when, where the handoff breaks during a session, what repeated denials actually cost the practice, and what a repeatable process looks like.


## The Codes Exist. The Denials Still Come.


G2082, G2083, and J0013 are publicly documented in[Janssen's Spravato billing and reimbursement guide](https://www.spravatohcp.com/payer-coverage-and-reimbursement/) . Clinics with those codes on file still face systematic denials, because the code that applies changes based on variables a static list cannot resolve.


Per Janssen's guide, **G2082 and G2083 are bundled Medicare codes: each covers the established-patient visit, the esketamine itself, and the required two-hour post-administration observation.** G2082 applies when the dose is 56 mg or less, G2083 when it exceeds 56 mg. J0013 is the drug-only code, reported in 1 mg units, that commercial payers and state Medicaid programs use when the drug is billed separately. J0013 replaced S0013 on January 1, 2026, and claims still filed under S0013 now deny on submission.


Code What it covers 2026 status


**J0013** Esketamine drug code, billed in 1 mg units, when the drug is billed separately (commercial and Medicaid) New January 1, 2026. Replaced S0013. Not payable by Medicare, which bundles the drug into the G-codes


**G2082** Bundled Medicare code: established-patient visit, the drug, and the 2-hour observation, dose of 56 mg or less Active. Never unbundle


**G2083** Same bundle for doses above 56 mg Active. Never unbundle


**S0013** Former esketamine drug code Retired December 31, 2025. Claims still filed under it now reject


Two variables determine which code fires: the drug supply source and the payer type. Neither variable lives on the code list. Both require someone to check before the claim is built. For broader context on how these codes fit into[psychiatric billing](https://www.mdhub.ai/blog-posts/the-complete-guide-to-psychiatric-billing-from-e-m-to-psychotherapy-with-ai-powered-support) , the relationships between drug codes, monitoring codes, and payer rules follow the same logic that governs most specialty mental health services.


### G2082 vs. J0013: Which Code Applies and When


**The supply source determines the drug code.** When the clinic purchases and administers Spravato directly under the buy-and-bill model, Medicare claims run through G2082 or G2083, which already include the drug. When the drug comes through a specialty pharmacy and is billed separately from the administration, J0013 carries the drug charge for commercial and Medicaid claims.


On the pharmacy-benefit path, the specialty pharmacy has already billed the drug. The clinic bills only the administration and the evaluation, and a second drug line is a duplicate denial that is hard to appeal. When a separate, identifiable assessment is performed on the same visit, add modifier 25 to the evaluation, or the payer folds it into the administration and pays nothing for the cognitive work.


The distinction matters because payers audit drug codes against supply chain records. Filing a bundled G-code on a pharmacy-supplied claim, or J0013 on a claim where the drug is already bundled, creates a mismatch that triggers a denial or a clawback. The 2026 transition adds one more trap: prior authorizations approved in late 2025 often list S0013, and when the claim goes out under J0013, the mismatch triggers an automatic denial. Re-verify every active authorization against the new code before the next session, not after the denial arrives.


### Medicare, Medicaid, and Commercial: How Payer Type Changes the Claim


**Each of the three payer types treats Spravato reimbursement differently.** Medicare covers Spravato under Part B with specific prior authorization and REMS enrollment requirements. Medicaid coverage and billing rules vary by state, and some state programs require separate prior authorization processes that differ from Medicare's. Commercial payers set their own coverage policies, and J0013 handling is not yet uniform: during the transition some plans still route the drug through J3490, the unclassified drug code.


These rules are current as of mid-2026, and payer policies rotate, so verify the plan's current medical policy before each submission. A single patient moving between payers, or a clinic treating patients on all three, cannot use a single claim template. The payer type must be confirmed before the code is selected, every session, without exception. The code is only the final step. The workflow that fires the right code, for the right payer, from the right supply source, is what most clinics have not built.


30% MORE BOOKINGS


mdhub, powering clinics with AI


Built for mental health


[BOOK A FREE DEMO →](https://calendly.com/d/cyph-mkm-tjg/demo-meet-the-mdhub-team-website)


## What Actually Breaks During a Spravato Session


The two-hour observation window is where clinical documentation and billing preparation must happen simultaneously. That overlap is where most coordination failures begin.


The clinician or monitoring nurse must document the session in real time during the observation window, not after. Waiting until the session ends means documentation is incomplete when billing staff need it, and billing cannot wait long before the claim falls behind.


**Billing staff must capture session type, drug supply source, and payer before building the claim.** That information lives in the clinical record, not the billing system. If billing staff cannot read the session notes directly, they have to ask the clinician, adding a step and a delay that invites error.


### The Two-Hour Window Where Documentation and Billing Collide


**The observation window compresses two separate workloads into the same block of time.** The clinician monitors the patient and documents the session. Billing staff need session data to prepare the claim. Both demands are urgent. Neither can wait for the other.


mdhub AI agents save clinicians 2+ hours every day, time that otherwise disappears into exactly this kind of double duty: documenting while monitoring, then re-answering billing questions after the session ends.


### Why EHRs Don't Solve the HCPCS Problem


**Most EHRs do not auto-populate HCPCS codes like G2082 or G2083.** Manual entry is the default. Manual entry introduces error, especially when the person entering the code is also managing the patient during the observation window.


If session notes are not reconciled with billing before claim submission, the wrong code gets filed. The reconciliation step requires both sets of information to be readable in the same workflow, and most EHR-to-billing handoffs are not built that way. The operational cost of that failure is not one denied claim. It is a pattern that compounds across every session the clinic runs.


## What a Miscoded Spravato Claim Costs the Practice


A miscoded Spravato claim carries two consequences: delayed cash flow on a high-cost, high-labor service and audit exposure if overbilling is detected. Both are serious. They are not the same problem.


Reworking a denied Spravato claim costs staff time across resubmission, documentation retrieval, and payer contact. Spravato sessions are long and expensive to run. A denial on each session does not just delay revenue, it consumes staff capacity that the clinic cannot recover. For a fuller picture of how billing errors affect the practice's financial health,[behavioral health revenue cycle management](https://www.mdhub.ai/blog-posts/behavioral-health-revenue-cycle-management) covers how denial patterns translate into A/R problems and owner decisions.


**Clinics running mdhub report up to 50% lower operational costs.** That headroom opens up when billing errors stop triggering rework cycles. It is the operational context for why a code list, on its own, is not a solution.


### Denials vs. Compliance Risk: Two Different Problems


**A denial delays revenue. An overbilling pattern attracts a payer audit.** Clinics often treat both as the same inconvenience. They are not. A denied claim can be corrected and resubmitted. An incorrect billing pattern on a REMS-restricted drug like Spravato draws payer scrutiny and exposes the practice to clawbacks.


Inconsistent code selection, sometimes correct, sometimes not, is not a neutral baseline. It is a documentation pattern that payers can identify. The compliance risk accumulates even when individual claims pass review.


### When Billing Complexity Quietly Kills a Service Line


**Clinics that cannot bill Spravato predictably stop offering it.** The decision is not always announced. It shows up in scheduling, fewer Spravato slots, longer waits, eventually no availability. The clinic loses the revenue. The patients who depend on the service lose access.


That outcome is the owner consequence of sustained billing complexity. The question is not whether the clinic knows the right code. It is whether the process that fires that code is repeatable enough to protect the service line.


30% MORE BOOKINGS


mdhub, powering clinics with AI


Built for mental health


## Building a Repeatable Spravato Billing Workflow


A repeatable Spravato billing workflow has three checkpoints: before the session, during the session, and after the session. Each checkpoint handles one decision so no single step carries the whole weight of accuracy.


**The goal is a process that does not depend on any one person catching the right variable at the right moment.** Staff change. Attention varies. A process built on individual vigilance will fail. A process built on checkpoints will not.


- **Pre-session:** Confirm the payer type and the drug supply source before the patient arrives. Map both to the correct HCPCS code and record the selection in a format billing staff can read without asking the clinician.
- **During session:** Clinical staff document the observation in real time using a format that translates directly into billing data, no interpretation layer, no follow-up questions.
- **Post-session:** Billing staff validate the selected code against session notes before submission. Catch errors before the claim goes out, not after a denial comes back.


Using[mental health billing software](https://www.mdhub.ai/use-cases/mental-health-billing-software) built for this workflow removes the manual reconciliation step and reduces the chance that a variable gets missed between checkpoints.


### The Three Checkpoints Every Spravato Session Needs


**Each checkpoint handles one class of error.** Pre-session confirmation stops supply-source and payer mismatches before they reach the claim. Real-time documentation during the session stops the reconciliation gap that delayed note completion creates. Post-session validation stops the wrong code from leaving the practice.


mdhub AI agents save clinicians 2+ hours daily. When documentation and billing prep no longer compete for the same staff attention during the observation window, that time becomes available for clinical work instead of administrative recovery.


### How Automated Claim Validation Reduces HCPCS Errors


[mdhub Billing Specialist](https://www.mdhub.ai/products/eric) **(Eric) automates claim creation and validation to reduce Spravato-specific coding errors.** Eric checks the claim against session data before submission, catching code mismatches, missing fields, and payer-rule conflicts without requiring a staff member to run a manual review on every claim.


A clinic that bills Spravato cleanly does not need more staff. It needs checkpoints that keep holding when payer policies rotate, staff turn over, and reauthorization windows, which run anywhere from one to twelve months depending on the plan, quietly expire.


## Streamline Your Practice


The gap this article covers is specific: knowing G2082, G2083, and S0013 is not the same as having a clinic process that applies the right code across payers, supply scenarios, and session types without adding steps to the clinician running the observation. If you have already trained staff and updated your billing spreadsheets and the denials have not stopped, the problem is the process, not the people. mdhub Billing Specialist (Eric) automates claim creation and validation so the right code fires every time, no manual reconciliation, no one-person catch point.[Book a demo with the mdhub team](https://calendly.com/d/cyph-mkm-tjg/demo-meet-the-mdhub-team-website) to see how it works in a clinic like yours.


If our clinic uses a specialty pharmacy to supply the drug, does that change which HCPCS code we bill, and does the answer differ between Medicare and a commercial payer?


Yes, the supply source changes the code, and the answer does differ by payer type. When a specialty pharmacy supplies the drug and bills it separately, J0013 carries the drug charge for commercial and Medicaid claims, reported in 1 mg units. J0013 replaced S0013 on January 1, 2026, so any authorization or claim template still referencing S0013 needs updating first. For Medicare, G2082 and G2083 apply when the clinic purchases and administers the drug directly, and the drug is already bundled into those codes, so confirm the specific requirements with your Medicare Administrative Contractor before submitting. The safest practice is to document the supply source at the pre-session checkpoint and map it to the correct code for each payer before the patient arrives.


Our billing staff submit Spravato claims correctly most of the time, is occasional inconsistency actually an audit risk, or only a denial risk?


Occasional inconsistency creates both risks, and they operate independently. A single miscoded claim is a denial risk, correctable through resubmission. A pattern of inconsistent coding on a REMS-restricted drug like Spravato is an audit risk, because payers can identify billing variation across a claims history even when individual claims pass review. Spravato's REMS status means it receives closer payer scrutiny than standard drug codes. An inconsistent billing pattern on a restricted drug signals to auditors that the practice lacks a controlled process, which increases the likelihood of a deeper review and potential clawback. Treating the two risks as separate problems, and addressing the pattern, not just individual denials, is the correct response.


Can a single billing workflow cover Medicare, Medicaid, and commercial payers for Spravato, or does each payer require a separate process?


A single workflow structure can cover all three payer types if it builds payer confirmation into the pre-session checkpoint. The workflow itself does not need to be different for each payer, but the code and prior authorization requirements it fires must reflect the payer in question. Medicare, Medicaid, and commercial plans each have distinct Spravato coverage rules, and those rules determine which code is correct and whether prior authorization is required before the session. A workflow that confirms payer type before each session and maps that confirmation to the correct code set handles all three without requiring separate processes. Automated claim validation tools like mdhub Billing Specialist apply those payer-specific rules at the claim level so the correct code fires without manual lookup.
