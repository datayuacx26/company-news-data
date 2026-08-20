---
schema_version: "1.0.0"
document_id: "456fb1251c5ce8983aaa509230bba2b6ce25a4ba725ca68dd6155a66e21d331b"
company_key: "yc-combinehealth"
company: "CombineHealth"
source_id: "yc-combinehealth-news-import-bb91426d417b"
canonical_url: "https://www.combinehealth.ai/blog/claim-submission-process"
published_at: "2026-07-30T18:30:00+00:00"
first_seen_at: "2026-08-01T00:10:51.285179+00:00"
fetched_at: "2026-08-01T00:10:52.546812+00:00"
content_hash: "sha256:3555477c101d0f3a4df6150a0dc50dc31145c8a4ad5b6c4df37659ba71a5b09a"
---

# The Claim Submission Process in Medical Billing And Where AI Helps

> **Key Takeaways**
>
>
> • Claim submission in healthcare involves sending a formatted claim to a payer, but acceptance of the claim doesn’t mean you get the same claim amount reimbursed; payment depends on the payer's adjudication process.
>
>
> • Most healthcare claim rejections and denials stem from preventable issues such as registration errors, outdated eligibility checks, unsupported documentation, missing authorizations, or filing deadlines.
>
>
> • Understanding the difference between claim rejections and denials is critical, as rejections require correction and resubmission, while denials may require a corrected claim or a formal appeal.
>
>
> • CombineHealth’s AI-powered medical coding and billing platforms help reduce claim submission errors by automating eligibility verification, validating documentation and codes, applying payer-specific rules, and identifying issues before claims reach the payer.


Submitting a single medical claim manually costs a provider $6.33 in staff time,[according to the CAQH Index](https://www.caqh.org/hubfs/Index/2024%20Index%20Report/CAQH_IndexReport_2024_FINAL.pdf) . This cost still excludes the work of gathering information beforehand, the follow-up afterward, and the cost of the software involved.


Electronic claim submission handles the transmission step well, which is why nearly every medical claim now travels that way.


What still takes time and money is the judgment around the claim: verifying insurance coverage, choosing medical codes the clinical documentation will support, and applying payer rules that constantly change.


That judgment work is what AI is now being used to automate: reading the chart, checking the codes against payer rules, and catching the problems that would otherwise come back as a denial or a rejection.


This guide walks through the claim submission process in medical billing step by step, and shows where AI can help.


> **Submit Cleaner Claims at High Volumes With AI**
>
>
> CombineHealth's AI-powered medical coding and billing platforms verify coverage, assign fully documented codes, and validate every claim against payer rules before submission, helping billing teams clear more claims on the first pass with less manual review.[Book a demo](https://www.combinehealth.ai/demo)


## On this page


- What Is Claim Submission in Medical Billing?
- The Claim Submission Process: Step by Step


- Patient Registration and Data Capture
- Insurance Eligibility and Benefits Verification
- Clinical Documentation and Medical Coding
- Charge Entry
- Claim Scrubbing and Pre-Submission Edits
- Submission to the Clearinghouse


- What Happens After You Submit a Healthcare Claim


- 1. Claim Rejection
- 2. Claim Denial


- Common Healthcare Claim Submission Mistakes That Lead to Denials or Rejections


- Registration and Demographic Errors
- Eligibility Verified Too Early
- Coding Not Supported by Documentation
- Missing Prior Authorization
- Formatting and Payer ID Errors
- Missing the Filing Deadline


- How AI Helps at Each Stage of the Healthcare Claim Submission


- Registration and Eligibility
- Documentation and Coding
- Charge Entry and Scrubbing
- After Submission


- Get Reimbursed Faster at Higher Volume With AI
- FAQs


## What Is Claim Submission in Medical Billing?


Claim submission in medical billing is the process of submitting a completed claim in a standardized format to an insurance payer, either directly or through a clearinghouse, so the provider can be reimbursed for services already delivered.


The claim itself is a structured record of the encounter. It carries:


- Patient and subscriber identifiers
- Rendering and billing provider details
- Dates of service
- Diagnosis codes
- Procedure codes
- Modifiers
- Charges


Basically everything the insurance company needs to decide whether they owe money to the hospital, and how much.


The claim format used for submitting medical claims depends on the type of provider and the services being billed.


**Paper form**


**Electronic transaction**


**Filed by**


**Example**


CMS-1500


837P


Physicians, non-institutional providers, and suppliers


Office visit, anesthesia service, independent lab


UB-04 (CMS-1450)


837I


Hospitals and other institutional facilities


Inpatient stay, emergency department facility charge


Today, almost all commercial and government claims are submitted electronically. Paper claims are still accepted in limited situations, but for **most healthcare organizations, the 837 transaction is the standard method** of claim submission.


> **Note:** Dental claims use a different electronic transaction (837D) and the ADA Dental Claim Form.
>
>
> ‍ **Recommended reading:**[What is Medical Billing](https://www.combinehealth.ai/blog/what-is-medical-billing)


## The Claim Submission Process: Step by Step


The claim submission process begins when a patient registers and ends when a claim is successfully submitted to the payer. Errors at any stage can lead to claim rejections, denials, payment delays, or lost revenue.


### Patient Registration and Data Capture


Patient registration **involves gathering patient details, such as** their legal name, date of birth, address, subscriber ID, group number, and the relationship between the patient and the subscriber.


**Most billing systems carry this information through every later step** without revalidating it. Even a small mistake, such as an incorrect member ID or outdated name, can cause the payer to reject the claim.


### Insurance Eligibility and Benefits Verification


Eligibility verification **confirms that the patient's coverage is active on the date of service and that the planned service is a covered** benefit under that plan.


The process uses standardized electronic transactions. A **270** inquiry is sent to the payer, and a **271** response returns coverage status, plan details, and the patient's financial responsibility.


However, electronic verification alone does not guarantee accurate claims. An incomplete or poorly reviewed 271 response can still result in claim denials.


### Clinical Documentation and Medical Coding


Medical coding converts the clinical documentation into **ICD-10, CPT, HCPCS,** and modifier codes used by payers to process claims.


The **selected codes must accurately reflect the services provided** and be fully supported by the clinical documentation. Even when the correct codes are assigned, insufficient documentation can lead to claim denials.


This stage also applies coding rules such as **NCCI edits, mutually exclusive procedure rules,** and **modifier requirements** to help ensure the claim meets payer guidelines.


### Charge Entry


Charge entry **attaches the fee for each coded service** and assembles the complete claim record in the billing system.


Key details such as **service units, place of service, rendering provider NPI, and referring provider information** are added during this step. Errors, such as incorrect units or assigning a service to the wrong provider, can lead to underpayments, overpayments, or claim denials.


Organizations using paper or scanned encounter forms may also introduce transcription errors during charge entry.


> **Recommended reading:**[Charge Capture in Healthcare](https://www.combinehealth.ai/blog/charge-capture-in-healthcare-rcm)


### Claim Scrubbing and Pre-Submission Edits


Claim scrubbing **checks the completed claim for errors** before it is submitted to the payer.


A claim scrubber validates required fields, diagnosis and procedure codes, **NCCI edits, modifier logic,** and payer-specific formatting requirements. Correcting issues before submission helps reduce claim rejections, denials, and payment delays.


However, claim scrubbers only validate claim data against predefined rules. They **cannot determine** whether the clinical documentation supports the billed services.


### Submission to the Clearinghouse


The clearinghouse receives the **837** claim file, validates it, converts it into the format required by each payer, and forwards it for processing.


Two acknowledgments are typically returned:


- **999 acknowledgment:** Confirms the electronic file was correctly formatted and accepted for processing.
- **277CA acknowledgment:** Reports the status of individual claims, identifying which claims were accepted for adjudication and which were rejected before processing.


Monitoring only the 999 acknowledgment can cause rejected claims to be overlooked. Each submitted claim should also be reconciled against the 277CA response.


> **Note:** Claims submitted directly through a payer portal bypass the clearinghouse—so they do not generate 999 or 277CA acknowledgments. These claims should be tracked separately.


## What Happens After You Submit a Healthcare Claim


When you submit the claim, the payer[adjudicates the claim](https://www.combinehealth.ai/blog/claims-adjudication-process-in-healthcare) , applies benefits and contract rates, and returns a remittance advice with payment.


There are two outcomes that come after claim adjudication, and they both need different responses.


### 1. Claim Rejection


A rejected claim does not enter the payer's adjudication system. Instead, the clearinghouse or the payer's front-end validation checks identify a technical or formatting error before processing begins.


Because the claim was never adjudicated, there are no appeal rights. The issue must be corrected, and the claim resubmitted.


### 2. Claim Denial


A denied claim has been adjudicated but is not approved for payment, either fully or partially. The payer returns a[Claim Adjustment Reason Code (CARC)](https://www.combinehealth.ai/blog/carc-in-medical-billing) explaining the reason for the decision.


If the error is yours, resubmit a corrected claim. If you disagree, appeal within the payer's deadline.


> **CombineHealth Case Study:** A community health center processing 5,000+ healthcare claims monthly **reduced denials by 20% with CombineHealth** by addressing upstream causes instead of reworking claims individually.[Read the case study](https://www.combinehealth.ai/resources/customer-success-stories/case-study/community-health-center)
>
>
> **Recommended reading:**[How to Appeal an Insurance Claim Denial](https://www.combinehealth.ai/blog/how-to-appeal-an-insurance-claim-denial)


## Common Healthcare Claim Submission Mistakes That Lead to Denials or Rejections


### Registration and Demographic Errors


Incorrect patient names, outdated addresses, or invalid subscriber IDs can **cause claims to fail validation before they reach the payer** . Verify patient information against the insurance card at every visit—not just during the initial registration.


> **Recommended reading:**[Common Reasons for Claim Denials](https://www.combinehealth.ai/blog/how-to-appeal-an-insurance-claim-denial)


### Eligibility Verified Too Early


A patient's insurance **can change between the scheduling date and the date of service** .


If eligibility was verified at booking and never rechecked, the claim goes out under a plan that has ended, and the payer denies it. Re-verifying shortly before the visit prevents it.


### Coding Not Supported by Documentation


Medical codes **should be fully supported by the clinical documentation** . Even when the selected codes are accurate, incomplete or insufficient documentation can result in claim denials during clinical review.


Identifying documentation gaps before claim submission is much more efficient than appealing denied claims later.


### Missing Prior Authorization


**Many payers require prior authorization** for specific services.


If authorization is not obtained before the service is provided, the **claim may be denied with limited or no opportunity for reimbursement** . Incorporating authorization checks into the scheduling process helps prevent these denials.


### Formatting and Payer ID Errors


Formatting errors, such as an **incorrect payer ID, invalid taxonomy code, or missing NPI** , can cause claims to be rejected before adjudication.


Most of these errors can be prevented through claim scrubbing and by reconciling submitted claims against the **277CA** acknowledgment to identify rejected claims promptly.


### Missing the Filing Deadline


Every payer sets a deadline for receiving claims.


Missing the timely filing limit can result in a **permanent denial, even if the claim is otherwise valid** . Track claim aging against each payer's filing requirements to ensure claims and resubmissions are submitted before the deadline.


> **Recommended reading:**[Clinical Documentation Improvement Software](https://www.combinehealth.ai/blog/clinical-documentation-improvement-software)


## How AI Helps at Each Stage of the Healthcare Claim Submission


AI reduces healthcare claim submission errors by validating data, codes, and payer rules before a claim is transmitted, rather than detecting problems after a payer has already refused to pay.


AI adoption is becoming increasingly common across healthcare administration.


According to the[2025 CAQH Index](https://www.globenewswire.com/news-release/2026/02/19/3241072/0/en/2025-caqh-index-shows-u-s-healthcare-avoided-258-billion-and-accelerated-automation-interoperability-and-ai-adoption.html) , more than half of health plans and one-quarter of provider organizations use AI in administrative workflows, with an estimated **$21 billion** in additional savings still available through greater automation.


Here's how AI supports each stage of the claim submission process.


### Registration and Eligibility


AI reads and reconciles patient and coverage data across sources, then runs eligibility verification without staff logging into individual payer portals. Coverage status, plan details, and patient financial responsibility come back in one place.


It reads the full 271 response rather than the top-line active or inactive flag, surfacing plan termination dates, benefit limits, and coordination-of-benefits indicators.


Verification can be re-run closer to the date of service, so any change since scheduling is caught before the encounter.


> **CombineHealth Case Study:** Eligibility checks at one anesthesia group were done manually for every case. **With CombineHealth, verification ran 80% faster and eligibility-related denials fell 8%.**[Read the case study](https://www.combinehealth.ai/resources/customer-success-stories/case-study/anesthesia-group-eligibility)


### Documentation and Coding


AI **reads the full chart and assigns ICD-10, CPT, HCPCS, and modifier codes** , attaching the passage of documentation that supports each one. It checks those selections against medical necessity criteria, NCCI edits, and payer-specific rules before the claim moves.


Flagging documentation that will not survive payer review gives coders a chance to query the provider while the encounter is recent, rather than defending a thin note months later in an appeal.


> **CombineHealth Case Study:** CombineHealth's medical coding AI platform was run against expert coders across thousands of emergency department charts. It matched them at **97% accuracy, returned results 50% faster** , and **surfaced 5x more documentation gaps.**[Read the case study](https://www.combinehealth.ai/resources/customer-success-stories/case-study/ai-matched-expert-coders)


### Charge Entry and Scrubbing


AI **applies payer-specific rules** that no team can hold in memory across dozens of contracts, checking units, place of service, modifier combinations, and rendering provider details on every line. Errors surface while they are still cheap to fix.


The same rules run on every claim regardless of volume. Anything that fails is separated out for review, so only claims that pass the checks get transmitted.


> **CombineHealth Case Study:** One anesthesia group bills for 100+ providers against more than 50 payer rule sets. With CombineHealth, claim generation reached 150 claims an hour.[Read the full case study](https://www.combinehealth.ai/resources/customer-success-stories/case-study/anesthesia-group-claims)


### After Submission


AI **monitors claim status across payers** and surfaces problems without staff working through portals one by one.


It **prioritizes follow-up** by recoverable value rather than by age, so the largest recoverable balances get attention first.


Claims still come back for reasons outside the submission itself. AI sorts rejections from denials, sends rejections for correction, and gathers what an appeal requires for denial cases.


> **Recommended reading:**[AI in RCM](https://www.combinehealth.ai/blog/artificial-intelligence-in-revenue-cycle-management)


## Get Reimbursed Faster at Higher Volume With AI


Claim volume rises faster than billing headcount, and payer rules change faster than any team can track by hand. Adding staff scales the manual work without reducing the error rate that causes denials in the first place.


Automating the judgment inside claim submission changes that ratio. CombineHealth runs that work through two AI agents that hand off to each other, so rising claim volume stops requiring a proportionally larger team.


**Amy** , the[AI medical coding solution](https://www.combinehealth.ai/ai-medical-coding-software) , reads the full chart and assigns ICD-10, CPT, HCPCS, E/M, and modifier codes in one pass. Each code carries its rationale, the guideline behind it, and the passage of documentation supporting it.


Amy checks every code against medical necessity, NCCI edits, and payer-specific rules before the claim moves, flags the documentation gaps that would otherwise return as denials, and routes judgment calls to your coders.


**Mark** , the[AI medical billing solution](https://www.combinehealth.ai/mark-ai-medical-biller) , handles eligibility checks, claim generation, and validation, then flags errors before submission and tracks claim status and payment afterward.


Across deployments, CombineHealth has reported:


- Up to 85% of manual coding effort removed,
- Coding accuracy above 98%, and
- Up to 75% fewer coding-related denials.


[Book a demo](https://www.combinehealth.ai/demo) to see how both agents work against your payer mix and specialty!


## FAQs


**What are the steps in the claim submission process?**


The claim submission process includes patient registration and data capture, insurance eligibility verification, clinical documentation and medical coding, charge entry, claim scrubbing, and transmission to the payer or clearinghouse.


**What is the difference between a claim rejection and a claim denial?**


A rejection is returned before the payer adjudicates it, usually for a data or formatting error, and is fixed by correcting and resubmitting. A denial is a decision made after adjudication and requires either a corrected claim or a formal appeal, depending on the reason code.


**What does a clearinghouse do in claims processing?**


A clearinghouse validates claim files, translates them into each payer's required format, and routes them onward. It returns a 999 confirming the file was accepted and a 277CA reporting which individual claims the payer accepted into adjudication.
