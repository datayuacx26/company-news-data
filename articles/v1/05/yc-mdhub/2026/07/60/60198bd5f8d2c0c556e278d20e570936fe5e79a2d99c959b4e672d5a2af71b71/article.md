---
schema_version: "1.0.0"
document_id: "60198bd5f8d2c0c556e278d20e570936fe5e79a2d99c959b4e672d5a2af71b71"
company_key: "yc-mdhub"
company: "mdhub"
source_id: "yc-mdhub-news-import-bbf3c05d7e92"
canonical_url: "https://www.mdhub.ai/blog-posts/spravato-prior-authorization"
published_at: "2026-08-11T09:39:39.809+00:00"
first_seen_at: "2026-07-24T11:04:12.059669+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:98c50175c7a5495c31213a405c3ceabc6fc18cf68671e035e67463256ebb0433"
---

# Spravato Prior Authorization: A Payer-by-Payer Denial Guide

Prior authorization is the gate every Spravato session passes through before a claim has any chance of being paid. When the authorization fails, the denial lands on a clinic that has already purchased the drug, scheduled a two-hour visit, and held the room. That is what makes Spravato prior authorization denials more expensive than almost any other denial in behavioral health.


The frustrating part is how predictable they are. Most Spravato prior authorization denials come from a short list of causes, and every one of them is visible before the patient walks in.


This guide covers where those denials start, how UnitedHealthcare, Cigna, Aetna, and Medicare each define treatment-resistant depression as of mid-2026, the 2026 code change that quietly invalidated a wave of older authorizations, and the pre-visit verification that catches all of it in time. For the code sets themselves, see our guide to the[Spravato billing workflow](https://www.mdhub.ai/blog-posts/spravato-billing) .


## Where Spravato Prior Authorization Denials Start


Across payers, the same five failures account for most denied Spravato claims:


- **The authorization never made it to the visit.** The request was not submitted, was still pending on the day of service, or was approved for a different dose tier or provider NPI than the one that ends up on the claim.
- **The treatment history misses the payer's definition.** The documented antidepressant trials do not match the number, classes, doses, or durations the plan requires. This is the most common source of a medical necessity denial.
- **The authorization references a retired code.** Approvals issued in late 2025 often cite S0013, which no longer matches a 2026 claim billed under J0013.
- **The REMS record is incomplete.** The required observation documentation does not hold up when the payer reviews it, sometimes weeks after payment.
- **The reauthorization lapsed.** Induction approvals expire, maintenance windows run anywhere from one to twelve months depending on the plan, and a missed renewal opens a coverage gap that swallows every session inside it.


Notice that none of these are knowledge problems. Every clinic offering Spravato knows prior authorization exists. These are coordination problems, the same workflow gap we cover in the[Spravato billing guide](https://www.mdhub.ai/blog-posts/spravato-billing) : the right information exists somewhere in the clinic, and nobody is assigned to check it at the moment it matters.


30% MORE BOOKINGS


mdhub, powering clinics with AI


Built for mental health


[BOOK A FREE DEMO →](https://calendly.com/d/cyph-mkm-tjg/demo-meet-the-mdhub-team-website)


## How Each Major Payer Defines Treatment-Resistant Depression


The most expensive denials come back reading "medical necessity not established." In practice that almost always means the documented treatment history did not line up with the payer's specific definition of treatment-resistant depression. As of mid-2026, the major plans converge on at least two failed antidepressant trials at adequate dose and duration, but the details differ, and the details are what deny. Payer policies rotate, so verify the current policy document before every submission.


Payer What the policy asks for Worth checking


**UnitedHealthcare / Optum** At least 2 different antidepressants or treatment regimens, each trialed at least 8 weeks, with medication, dates, and duration documented Requires a documented baseline score on a recognized depression assessment before starting, and Spravato alongside a newly initiated or optimized oral antidepressant ([policy](https://www.uhcprovider.com/content/dam/provider/docs/public/prior-auth/drugs-pharmacy/commercial/r-z/PA-Med-Nec-Spravato.pdf) )


**Cigna / Evernorth** At least 2 antidepressants from different classes at adequate dose, at least 6 weeks each, within the current depressive episode Expects documented non-response (25% improvement or less), a concurrent oral antidepressant, prescribing by a psychiatrist, and a prescription monitoring program check on file ([policy](https://static.cigna.com/assets/chcp/pdf/coveragePolicies/cnf/cnf_606_coveragepositioncriteria_psychiatry_spravato_pa.pdf) )


**Aetna** At least 2 failed antidepressant trials with a confirmed major depressive disorder or TRD diagnosis in an adult, administered under medical supervision with an oral antidepressant Moderate or severe substance use disorder that is not in treatment can make a patient ineligible, and a separate criteria set covers depression with acute suicidal ideation, so cite the right one ([policy](https://www.aetna.com/cpb/medical/data/900_999/0950.html) )


**Medicare** TRD diagnosis with at least 2 adequate antidepressant trials and documented medical necessity Covered under Part B and billed through the bundled G2082/G2083 codes; the drug is never billed separately


Two clinics can submit the same patient to two different plans and get one approval and one denial, with identical clinical facts. The variable is not the patient. It is whether the documentation was assembled against that plan's checklist.


### Documenting Antidepressant Trials the Way Reviewers Read Them


**A reviewer counts discrete generic names, drug classes, doses, and durations.** Record every prior trial as medication, class, dose, start and stop dates, and outcome. Two medications from the same class may not count as two trials at plans that require different classes. Duration thresholds differ too, 6 weeks at Cigna and 8 at UnitedHealthcare, so document actual weeks on therapy rather than rounding.


One more pattern worth flagging at intake: a patient who discontinued their oral antidepressant mid-course can trip a renewal denial at plans that expect concurrent therapy. Address it in the documentation before the maintenance request goes out, not after the denial comes back.


## The 2026 Code Change That Quietly Invalidates Old Authorizations


On January 1, 2026, CMS retired S0013 and replaced it with J0013 as the esketamine drug code. The clinical criteria did not change. The paperwork did.


**An authorization approved in late 2025 that lists S0013 no longer matches a 2026 claim billed under J0013.** Automated adjudication systems compare the code on the claim against the code on the approval, and when they disagree, the claim denies as unauthorized even though the treatment was clinically approved. The fix is mechanical: audit every active Spravato authorization, and re-verify the code references before the patient's next session rather than after the denial arrives. The full 2026 code picture, including the bundled Medicare G-codes, is in our[Spravato billing workflow guide](https://www.mdhub.ai/blog-posts/spravato-billing) .


Check the benefit rail at the same time. A plan may cover Spravato under the medical benefit, where the clinic buys and bills the drug, or under the pharmacy benefit, where a specialty pharmacy dispenses it and the clinic bills services only. The authorization has to route down the same rail the claim will follow. An approval on one rail does not authorize the other.


30% MORE BOOKINGS


mdhub, powering clinics with AI


Built for mental health


## The Pre-Visit Verification That Prevents Almost All of This


Every failure in this guide is visible the day before the visit. That is what makes Spravato prior authorization a workflow problem rather than a luck problem. A pre-visit verification pass checks six things:


- Authorization status is approved, and the approval matches the dose tier and the rendering provider's NPI
- The trial documentation attached to the request matches this payer's definition, not a generic template
- Code references are current: J0013 or the bundled G-codes, with no S0013 left anywhere in the file
- REMS enrollment is active for both the site and the patient
- The reauthorization window has not expired, and the renewal date is on someone's calendar
- The benefit rail is confirmed, so the claim will follow the same path the authorization took


This is exactly the kind of repetitive, deadline-driven checking that software should own and staff should supervise.[mdhub Billing Specialist](https://www.mdhub.ai/products/eric) (Eric) validates claims against payer rules before submission and flags authorization mismatches while there is still time to fix them, and your team stays in control of every decision. The AI does the checking; the clinician and the billing lead make the calls. Clinics running[mental health billing software](https://www.mdhub.ai/use-cases/mental-health-billing-software) built this way report up to 50% lower operational costs, largely because denied-claim rework stops consuming the week.


## Streamline Your Practice


Spravato prior authorization denials cluster around five causes: authorizations that never landed, treatment histories that miss the payer's definition, retired code references, incomplete REMS records, and lapsed reauthorizations. Each one is preventable with a verification pass that runs before the patient arrives, built against the specific payer's checklist rather than a generic template. If your clinic is absorbing these denials today, the gap is not effort or knowledge. It is a workflow that no one has been given the time to run.[Book a demo with the mdhub team](https://calendly.com/d/cyph-mkm-tjg/demo-meet-the-mdhub-team-website) to see how Eric runs that verification automatically for clinics like yours.


How long does Spravato prior authorization take?


For non-urgent requests, major commercial payers commonly take one to three weeks, though timelines vary by plan and by how complete the submission is. Requests for depression with acute suicidal ideation can qualify for expedited review when the clinical urgency is explicitly documented. The operational rule: build the approval timeline into scheduling, and do not book the induction series until the authorization is confirmed, because a pending request on the day of service is one of the most common denial causes.


Does an approved authorization carry over if the patient changes insurance?


No. Prior authorization is specific to the patient and the plan. When a patient changes insurance mid-treatment, the new plan requires a fresh request evaluated against its own criteria, and the reauthorization clock restarts. January is the highest-risk month, because plan changes take effect while maintenance treatment is mid-course. Flag insurance changes at every check-in for patients in active Spravato treatment, and start the new request before the next scheduled session.


What should a clinic do after a medical necessity denial?


Rebuild the treatment history first. Most medical necessity denials reflect incomplete documentation rather than a true criteria miss, so assemble every prior trial as medication, class, dose, duration, and outcome, and compare it line by line against the payer's current policy. Then appeal with the complete record, a letter of medical necessity, and a request for a peer-to-peer review. If the patient genuinely does not meet the plan's criteria yet, document exactly what is missing so the care team can decide the clinical path with full information.
