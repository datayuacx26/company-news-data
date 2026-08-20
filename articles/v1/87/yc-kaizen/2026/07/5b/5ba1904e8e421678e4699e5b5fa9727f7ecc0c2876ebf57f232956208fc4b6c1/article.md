---
schema_version: "1.0.0"
document_id: "5ba1904e8e421678e4699e5b5fa9727f7ecc0c2876ebf57f232956208fc4b6c1"
company_key: "yc-kaizen"
company: "Kaizen"
source_id: "yc-kaizen-news-import-1021911e948a"
canonical_url: "https://www.kaizenautomation.com/blog/medical-billing-process"
published_at: null
first_seen_at: "2026-07-22T01:12:15.212824+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:6ab4b7da8ae24528bc8c2ad413380dbcb91232caa4afe0ebeba85f6d6d2cb1a6"
---

# Medical Billing Process: 9 Steps + How to Automate

Billing teams spend hours each day logging into payer portals to check eligibility, submit prior auth, and track claim status, and most of that work happens before a claim is ever submitted. Let's break down the medical billing process to show where manual steps create delays, denials, and write-offs.


## Where the medical billing process actually breaks down


A claim doesn't fail because of one mistake. **An eligibility issue nobody caught on day 1** becomes a coding error on day 3, which then becomes a denial on day 30 that **nobody has the bandwidth to appeal** .


The weak points are almost always the same:


- Eligibility verification done too late or skipped entirely
- Prior authorization requested but never tracked to completion
- Coding errors that clear internal review but fail at the clearinghouse
- Claim scrubbing that catches formatting errors but misses payer-specific policy failures
- Denial management that logs rejections without acting on them


## The medical billing process: 9 steps


### Step 1: Patient registration and data capture


Collect the patient's full name, date of birth, address, insurance carrier, member ID, and group number before or at the time of their first visit. **Enter it accurately into your PMS.** A single digit wrong on a member ID will cause a **denial three weeks later.**


**Time estimate:** 5 to 10 minutes per new patient, less for returning patients with verified records.


**How to automate this:** Connect your intake forms directly to your PMS using a tool that **auto-populates demographic fields** on submission. Then set up a **demographic verification layer** that flags mismatches before the record is saved: **wrong member ID format, missing group number.**


**Pro tip:** Build a **mandatory field checklist** into your intake workflow. If the insurance card front and back aren't scanned and attached to the record before the visit starts, the file isn't complete.


### Step 2: Insurance eligibility verification


**Before every appointment,** confirm active coverage, check deductible and copay status, and identify any prior auth requirements for the planned service. Do this the day before, not at check-in.


**Time estimate:** 3 to 5 minutes per patient when done manually through a payer portal. Across a full day's schedule, that's an hour of work before anyone sees their first patient.


**How to automate this:** Use[browser automation](https://www.kaizenautomation.com/) to run **eligibility checks** across payer portals (United, Aetna, BCBS, and others) the evening before each day's schedule. **Results get pushed directly to your PMS:** active coverage status, deductible balances, copay amounts, and any prior auth flags.


Your front desk **starts the day with a complete picture** instead of spending the first hour logging into portals.


**Pro tip:** Set your eligibility automation to **flag coverage issues automatically** and route them to your front desk before confirmation goes out.


### Step 3: Prior authorization


For any service that requires **payer approval before treatment** , submit the prior auth request and get written confirmation before the appointment. **Each payer has different requirements.** What United needs isn't what Aetna needs, even for the same procedure.


**Time estimate:** 15 to 45 minutes per request for manual portal submissions, plus 1 to 5 business days waiting for approval, depending on the payer.


**How to automate this:** Automate the **portal submissions** themselves. A tool like Kaizen logs into **Availity, United, Aetna,** and other payer portals, fills and submits the prior auth request. It then **monitors approval status** and notifies your team when confirmation comes back, via Slack or your PMS.


**Pro tip:** Build a **payer-specific prior auth requirements list** and review it quarterly. Payers update their lists constantly, and what didn't need an auth six months ago might need one today.


### Step 4: Clinical documentation and charge capture


After the visit, the provider documents services in the EHR. The billing team then reviews that documentation to **capture every billable charge** , including procedures, supplies, and any additional services that weren't part of the original appointment.


**Time estimate:** 10 to 20 minutes per encounter for the provider to document, plus 5 to 10 minutes for the billing team to review and capture charges.


**How to automate this:** This step can't be fully automated since clinical review is required. However, you can use AI-assisted tools to **flag incomplete documentation and surface potential missed charges** before the claim reaches coding.


**Pro tip:** Set a **24-hour rule.** All charges must be captured within one business day of the visit. **Anything older than 48 hours is a revenue risk** , and anything older than a week puts timely filing at risk for some payers.


### Step 5: Medical coding


Convert documented services into **the right CPT codes for procedures and ICD-10 codes for diagnoses.** Attach any needed modifiers. The diagnosis code has to support the procedure code. If the clinical notes don't justify what's being billed, the claim will get denied.


Most billing mistakes trace back to coding issues, whether that's an incorrect code, a missing modifier, or a diagnosis that doesn't support the procedure. A single coding error can turn a **$2,000 paid claim into a $2,000 denial.**


**Time estimate:** 5 to 15 minutes per claim depending on complexity and specialty.


**How to automate this:** AI-assisted coding tools can **suggest CPT and ICD-10 codes based on clinical documentation** . For high-volume, low-complexity specialties, this can reduce review time, though a coder should still **spot-check outputs regularly** .


For behavioral health, surgery, and other complex specialties, **a trained coder still reviews AI suggestions** and approves before submission.


**Pro tip:** Run a **quarterly coding audit** on your top 20 most-billed CPT codes. If you're seeing a pattern of denials on specific codes, your team may need **retraining** .


### Step 6: Claim scrubbing


Before submitting, run every claim through a **scrubbing check to catch errors** . Verify that patient demographics match the payer's records, CPT and ICD-10 codes are compatible, all required fields are filled in, and payer-specific formatting rules are met.


**Time estimate:** 2 to 5 minutes per claim manually, seconds per claim with an automated scrubbing tool.


**How to automate this:** Set up a **rules-based scrubbing tool** that runs every claim through a pre-submission check automatically: demographics against payer records, CPT-ICD-10 compatibility, required fields, and payer-specific formatting. No manual review needed for clean claims; **human eyes only on the flagged ones** .


**Pro tip:** Don't rely on your clearinghouse alone for scrubbing. **Clearinghouses catch formatting errors.** Payer-specific policy errors need a separate layer of checking on top of that.


### Step 7: Claim submission


Submit the scrubbed claim to the payer, either electronically through a clearinghouse or directly through the payer's portal. **Confirm receipt and note the submission date** , which starts your timely filing clock.


Each payer has a filing window, usually **90 days to one year** from the date of service. Only a valid submission through the correct channel counts toward that deadline.


**Time estimate:** 1 to 3 minutes per claim for electronic submission. Manual portal submissions can take 10 to 20 minutes per claim.


**How to automate this:** Clearinghouse submission is already largely automated for most practices. **For payers that require portal submission** , run a workflow that logs in, submits the claim, and captures the confirmation automatically.


**Pro tip:** Save your **submission confirmation for every claim** . If a payer says they never received it, that timestamp is what you use to prove timely filing.


### Step 8: Payment posting and reconciliation


When the payer sends back an Explanation of Benefits (EOB) or Electronic Remittance Advice (ERA), **post the payment against the original claim** in your PMS. Check it against the contracted rate, and bill any remaining patient balance. **Underpayments usually slip through** during posting.


**Time estimate:** 2 to 5 minutes per ERA line item when posted manually. High-volume practices can spend hours per day on payment posting alone.


**How to automate this:** Turn on **ERA auto-posting in your PMS** to match payments to claims automatically. Set rules to flag any payment that doesn't match the contracted rate and route it for review. This **removes manual line-by-line checks** while still catching underpayments and secondary payer issues.


**Pro tip:** Build a **monthly check** that flags any payer where the **paid amount is consistently below the contracted rate** . Payers sometimes apply the wrong fee schedule and it goes unnoticed for months.


### Step 9: Denial management and AR follow-up


**Work every denied or unpaid claim within the appeal window.** Use the CARC code to identify the issue, correct soft denials, and appeal hard ones. Track each claim until it's paid or closed.[60% of medical group leaders reported an increase in denial rates](https://www.mgma.com/mgma-stat/strategic-improvements-in-your-rcm-to-reduce-your-practices-claim-denials) compared to the prior year, and most teams don't have the bandwidth to work them all before the window closes, which is why they turn into write-offs.


**Time estimate:** 20 to 90 minutes per appeal depending on denial type. Claim status checks across payer portals take 3 to 5 minutes per claim when done manually.


**How to automate this:** Automate **claim status checks across payer portals** like United, Aetna, and Availity. Pull denial codes, track claim status changes, and flag claims that need action. Your team works a **prioritized queue** while the system handles the portal logins, navigation, and data retrieval.


**Pro tip:** Prioritize **high-value claims** in your denial queue. Working a $5,000 denial first recovers more revenue per hour than clearing smaller balances.


## Where billing teams get the steps wrong


- Eligibility isn't rechecked for returning patients. Insurance changes. Plans lapse. A patient who was active six months ago might be on a new plan today. Run eligibility before every appointment, regardless of how recently the patient was last seen.
- Prior auth gets submitted but is not tracked. The request goes into a payer portal, but no one checks the status before the visit. The service gets delivered without approval, and the claim is denied for missing prior auth, which then moves to appeal or write-off.
- Claims pass internal review but fail payer edits. Coding and documentation clear internal checks, but payer-specific rules or missing modifiers cause the claim to fail at the clearinghouse or get rejected after submission. The error only shows up after the claim leaves the system.
- Claims get submitted in batches instead of daily. Claims sit unsubmitted for days, which compresses your timely filing window and delays error detection. When something is wrong, you find out later with less time to fix it.
- Portal access sits with one person. If the only person who knows the payer portal logins leaves, billing stops. Document every portal login, store credentials in a HIPAA-compliant password manager, and make sure at least two people can access each payer portal.


## Kaizen makes the medical billing process faster


Most of the time lost to medical billing happens within payer portals: logging in, navigating, pulling information, and re-entering it elsewhere.


Kaizen automates the browser-based steps that are still being done manually:


- **Eligibility verification can be run the night before each day's schedule automatically, with results pushed directly to your practice management system:** coverage status, deductible balances, copay amounts, and prior auth flags included.
- Prior auth submissions and status tracking across Availity, United, Aetna, and other payer portals. The request goes in, approval status gets monitored, and your team gets notified when confirmation comes back.
- Claim status retrieval is pulled automatically across payer portals without anyone logging in manually.
- Denial reason code flagging can be surfaced and routed to the right person as they come in, so your team works a prioritized queue instead of hunting through portals.


If you're running a multi-location practice or a digital health company managing dozens of providers, these time savings compound fast. Each eligibility check is only a few minutes of portal work, but stack a full day's schedule together and those minutes become someone's entire job.


Everything that requires judgment stays with your team. Kaizen handles the repetitive portal work so they can focus on the exceptions that actually need a human.


> **Ready to stop losing hours to payer portals?**[Book a call](https://www.kaizenautomation.com/) and we'll map out what your first automation can actually look like in practice.


## Frequently asked questions


### How long does the medical billing process take from visit to payment?


The medical billing process takes **14 to 30 days from date of service** to payment for clean claims with no issues. Claims that need prior authorization, have coding errors, or get denied and appealed can take **60 to 120 days or longer** . Automating eligibility verification, prior auth tracking, and claim scrubbing is the most reliable way to shorten that timeline.


### What's the hardest part of the medical billing process?


The hardest part of the medical billing process is **prior authorization and denial follow-up** because both require repeated portal checks and manual tracking. Each payer has different workflows, so **teams spend hours logging into portals** instead of working claims, which leads to delays and missed follow-ups.


### Do I need a dedicated billing team to manage this process?


Whether you need a dedicated billing team **depends on claim volume** . Practices billing fewer than 100 claims per month can often manage with a **part-time biller** . Above that, the portal work alone across eligibility, prior auth, and claim status starts to require dedicated staff. **Automating those portal-dependent steps** reduces the headcount needed to keep the process running cleanly.


### Can Kaizen help automate the medical billing process?


Yes, Kaizen automates the **browser-based steps** of the medical billing process that can't be handled through APIs alone: eligibility checks, prior auth submissions and tracking, claim status monitoring, and denial reason code retrieval across **United, Aetna, Availity, and any other web-based payer portal** .


### What happens if I miss a payer's timely filing window?


If you miss a payer's timely filing window, **the claim is denied permanently** . There's no appeal path once the window closes. Most payers allow **90 days to one year** from the date of service. The safest way to protect your filing window is to **submit daily rather than in batches** , and to save submission confirmations for every claim.


### What happens if a claim is denied after submission?


If a claim is denied after submission, **check the CARC code first** . If it's a soft denial, correct the issue and resubmit. If it's a hard denial, file a formal appeal within the payer's window, which is usually **30 to 180 days from the denial date** . **Document everything:** the original claim, the denial letter, your supporting clinical notes, and your submission confirmation.
