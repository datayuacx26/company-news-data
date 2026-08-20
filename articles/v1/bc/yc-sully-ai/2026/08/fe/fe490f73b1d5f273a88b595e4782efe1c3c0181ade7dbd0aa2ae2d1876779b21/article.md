---
schema_version: "1.0.0"
document_id: "fe490f73b1d5f273a88b595e4782efe1c3c0181ade7dbd0aa2ae2d1876779b21"
company_key: "yc-sully-ai"
company: "Sully.ai"
source_id: "yc-sully-ai-news-import-101ec319ffc2"
canonical_url: "https://www.sully.ai/blog/ai-scribe-for-pediatrics-and-the-half-of-the-note-nobody-says-out-loud"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T04:59:15.933945+00:00"
fetched_at: "2026-08-11T04:59:18.055748+00:00"
content_hash: "sha256:dd564c3f52f05bf4a2f50d6164f31d32c09519b99fd1cc5b33f589cfbb9c7aa1"
---

# Best AI Scribe for Pediatrics: Billing & Notes Guide 2026

A parent fills in a developmental screening form in the waiting room. A nurse draws up three vaccines. Someone plots a weight on a growth curve.


None of that happens in the conversation, and all of it is what makes the visit billable.


That is the part most[AI scribe](https://www.sully.ai/medical-scribe) pitches for pediatrics skip. They talk about squirming toddlers and talkative parents, as though the hard thing about a pediatric visit is hearing it clearly. The hard thing is that half the note was never spoken aloud.


> **Key Takeaway:** An AI scribe for pediatrics has two problems no other specialty has. First, most of what makes a pediatric visit billable is never said out loud: the growth percentile plotted on a chart, the M-CHAT score from a form filled in the waiting room, the vaccine components a nurse administered, the name of the screening tool and its result. A scribe that transcribes the conversation perfectly still misses all of it. Second, the adolescent note has the opposite requirement, because parts of it have to stay out of the parent's view, and one safety-net study found 60 percent of adolescent portal accounts were potentially being accessed by guardians \[1\]. Documentation is also not where pediatricians lose their evenings: access-log data from 56 primary care pediatricians shows only 8 percent of after-hours EHR time goes to documentation and orders, against 78 percent spent reviewing data and reports \[2\]. Sully.ai's AI Scribe writes into the EHR across Office Practicum, Epic, Cerner, Meditech, and Athenahealth, and the AI Coder carries the note through to the coded claim.


## Pediatric Documentation Is Mostly Not the Conversation


A[pediatric visit](https://www.sully.ai/specialty/pediatrics) generates a conversation and a pile of structured data, and the two barely overlap.


The conversation holds the parent's concerns, the symptom history, and the counseling. The structured data holds almost everything a payer will look for.


### Where a Pediatrician's EHR Hours Actually Go


The best evidence on this is not a survey. Researchers pulled EHR access logs for 56 primary care pediatricians at Nationwide Children's Hospital, covering more than 1.5 million access-log data points across 1,069 physician workdays \[2\].


Those pediatricians averaged[4.4 hours per workday inside the EHR](https://www.sully.ai/help-center/articles/how-much-time-does-an-ai-scribe-save) during scheduled clinic hours, plus another 0.8 hours outside them. Roughly 10 minutes of after-hours EHR time for every hour spent in clinic \[2\].


Now the part nobody quotes. Of that after-hours time, **78 percent went to reviewing data and reports. Only 8 percent went to documentation and orders** \[2\].


A scribe addresses the 8 percent. That is worth having, and it is not the same thing as giving a pediatrician their evening back. Any vendor who tells you otherwise is selling past the evidence.


Look at what the other 92 percent is, though. Reviewing results, working an inbox, chasing coordination. That is not writing work, so a better transcriber cannot touch it. It is work you reassign.


Which is the real argument against buying a single-purpose scribe and calling the problem solved, and a good reason to judge a pediatric tool on something other than minutes.


### One Visit, Several Documents


A well-child visit does not produce a note. It produces a note, a growth chart entry, a set of screening instrument results, an immunization record that has to reach a state registry, and a record of anticipatory guidance.


A tool that produces a polished narrative has handled one of the five.


## Why an Ambient Scribe Misses What Makes a Pediatric Visit Billable


Here is the argument, stated plainly.


**Pediatrics is the specialty where the note has to record what was never said.** The reimbursable substance of a well-child visit is mostly structured data that never enters the dialogue. A percentile on a growth curve. A score from a form a parent completed before you walked in. A count of vaccine components administered by a nurse who was not part of the conversation.


Even the vendors closest to this concede it. Commure's own pediatric scribe review states that the tools it evaluated cannot pull vaccine identifiers or registry submission data from audio, and that those functions stay with the EHR \[3\].


That is a competitor telling you the mechanism. Three specific failure modes follow, and all three are testable in a demo.


### The Percentile Is the Screen, Not the Number


A weight of 14 kilograms is data. The percentile is the finding.


Head circumference through 24 months, weight-for-length under 2 years, BMI from 2 years, and blood pressure percentile are all plotted rather than spoken. Nobody says "fifteenth percentile" out loud in a way a transcript can reliably capture, and a measurement without its percentile is not a screen.


### The Score Comes From a Form, Not the Room


Developmental and autism screening runs on a validated instrument, usually completed by a parent before the clinician arrives.


CPT requires that the instrument be named, scored, and documented. A non-standardized screen is not separately reportable at all unless a specific payer allows it \[4\].


Nothing in the conversation supplies the tool name or the score. If your scribe cannot pull them, the screening either goes unbilled or goes on the claim unsupported.


### The Vaccine Count Comes From the Nurse


Immunization administration is coded per component, not per injection. The components were drawn up by someone who was not part of the dialogue at all.


This is where the admission above bites hardest. Vaccine identifiers, lot numbers, and registry data are not in the audio, and they never will be.


## The Coding Rules That Decide a Well-Child Claim


These are the rules that separate a paid well-child visit from a denied one. Neither of the two leading pediatric scribe guides contains a single one of them.


### The Age on the Date of Service Picks the Code


Preventive medicine codes are age-banded. New patient: 99381 infant under 1 year, 99382 early childhood 1 to 4, 99383 late childhood 5 to 11, 99384 adolescent 12 to 17. Established patient: 99391 through 99394 across the same bands \[4\].


The age **on the date of service** governs, not the age at scheduling. A birthday between booking and the visit changes the code.


Diagnosis matters too. Z00.129 is a routine child health exam without abnormal findings. Z00.121 is the same exam with abnormal findings \[4\].


### Developmental Screening and Immunizations Are Reported Separately


AAP is explicit about this, and practices still get it wrong. Immunization administration and any screening test that has its own CPT code are reported **separately** from the preventive medicine service, never bundled into it \[4\].


The preventive code already includes the age-appropriate history, the exam, the counseling, the anticipatory guidance, and the ordering of labs. It does not include the screens.


96110 covers developmental and autism screening, per instrument, with scoring and documentation. 96127 covers brief emotional and behavioral assessment, also per instrument \[4\]. Per instrument means a parent form and a teacher form are two units, not one.


### Vaccine Administration Is Counted Per Component


This is the most commonly mis-coded thing in pediatrics, and the rule is precise.


90460 covers the first or only component of each vaccine, and only when a physician or qualified health professional performs face-to-face counseling. Then +90461 covers each additional component within that vaccine \[4\].


A component is the set of antigens that prevent disease caused by one organism. So MMR is three components. Conjugates and adjuvants do not count as components \[4\].


Without counseling, or for a patient 19 or older, you use 90471 and +90472 instead, and those are counted per injection rather than per component \[4\].


### A Trivial Problem at a Well Visit Is Not Separately Billable


AAP's wording is worth knowing exactly: an insignificant or trivial illness, abnormality, or problem encountered in the process of performing the preventive medicine service should not be separately reported \[4\].


To bill a problem E/M on the same day as a well visit, that problem needs its own chief complaint, its own history, an exam relevant to it, and medical decision making, with modifier 25 on the E/M.


## Free Pediatric Note Templates


Three templates covering the visits where documentation decides the payment. Each carries the payer rules and the recommended screening ages printed on the page, which is the part a blank note in the EHR will never give you.


-


[Pediatric well-child visit note template](https://www.sully.ai/templates/pediatric-well-child-visit-note) for the periodic visit, two pages following the Bright Futures and AAP schedule, with a screening table that asks for the instrument and the score rather than a checkbox


-


[Pediatric sick visit note template](https://www.sully.ai/templates/pediatric-sick-visit-note) for the acute visit, with a weight-based dosing record and the modifier 25 boundary printed on the page


-


[Pediatric ADHD evaluation template](https://www.sully.ai/templates/pediatric-adhd-evaluation) built to the AAP clinical practice guideline, with the two-setting and two-informant requirements as fields rather than assumptions \[6\]


## The Adolescent Note Is the Opposite Problem


Everything above is about capturing more. Adolescence flips the requirement.


From roughly age 12 the note starts carrying content that has to stay out of a parent's view, and the tool's default behavior stops being neutral.


### From About Age 12 the Note Carries Confidential Content


The Bright Futures and AAP periodicity schedule adds depression and suicide risk screening from 12 through 21 years, and it asks clinicians to make every effort to preserve the confidentiality of the adolescent while doing it \[5\].


That is not a soft suggestion in a footnote. It is a documentation requirement that conflicts directly with how a chart is normally shared. One estimate cited in the safety-net literature puts the share of adolescent patient notes containing confidential information at 25 percent \[1\].


### Proxy Portal Access Breaks Adolescent Confidentiality in Practice


A 2023 quality-improvement project at Boston Medical Center reviewed 4,455 adolescent portal accounts. It found **60 percent were potentially being accessed by guardians** rather than by the adolescent \[1\].


Of the 2,255 accounts flagged and emailed, only 425 were updated by the adolescent. The remaining 1,830, or 81.2 percent, were deactivated \[1\].


The wider literature it cites is worse: erroneous guardian use of adolescent portal accounts runs at 64 to 80 percent \[1\].


So the working assumption should be that an adolescent's note is visible to a parent unless someone has actively arranged otherwise.


### The Cures Act Removed the Easy Answer


NASPAG and SAHM issued a joint statement on the ONC Final Rule implementing the 21st Century Cures Act, and their recommendations are concrete: separate and differential portal access for the adolescent and the proxy, confidential note types in the EHR, and staff workflows built around confidentiality \[7\].


Worth being honest about the constraint. The preventing-harm exception is not a general licence to withhold a parent's access simply to protect a minor's confidentiality. This is a workflow and configuration problem, not something a policy exception solves for you.


### What This Means for an Ambient Tool


A scribe whose default is to capture everything said and write it into the chart is the wrong default for an adolescent visit.


Ask any vendor two questions. Can the output be routed to a confidential note type? And can sensitive content be kept in its own section rather than merged into one narrative a proxy can read?


If the answer to both is no, the tool will make your confidentiality problem worse at exactly the ages where it matters most.


## How to Evaluate an AI Scribe for a Pediatric Practice


Four questions, in the order that matters.


### Does It Capture What Was Never Said


Bring a real well-child visit. Look at the output and check whether the percentile, the screening instrument name and score, and the vaccine components appear at all, or whether you got a nicely written version of the conversation.


This is the test that separates the tools, and most demos are not designed to show it.


### Does It Support the Confidential Note


Ask directly. Can output go to a confidential note type, and how is sensitive adolescent content kept out of a proxy's view?


### Does It Handle Age-Banded Coding


Ask whether it knows the age on the date of service drives the preventive code band, and whether screenings and immunization administration surface as separately reportable items rather than getting folded into the visit.


### Does It Reach the Registry and the Claim


Immunization data has to reach a state registry. The coded claim has to leave the building.


A note that stops at the chart has solved the smaller half of the problem, and in pediatrics the smaller half is 8 percent of the evening \[2\].


## Where Sully.ai Fits in a Pediatric Practice


Sully is a set of AI roles rather than a single tool, which matters for the 92 percent of after-hours work a scribe cannot reach.


Sully's[AI Scribe](https://www.sully.ai/agents/scribe) captures documentation during and after the visit and writes it into the EHR, on a single integration across[Office Practicum, Epic, Cerner, Meditech, Athenahealth, and ambulatory systems including Elation, Practice Fusion, NextGen, and AdvancedMD](https://www.sully.ai/integrations) . Office Practicum matters here because it is the EHR a lot of independent pediatric practices actually run on, and it is on the same single integration as the enterprise systems. Clinicians can save how they want a note structured, so a screening block or a confidential adolescent section stays a distinct field rather than dissolving into a paragraph.


The[AI Coder](https://www.sully.ai/agents/medical-coder) then extracts the ICD-10 and CPT codes and submits the claim. That matters more in[pediatrics](https://www.sully.ai/specialties/pediatrics-scribe) than almost anywhere else, because one well-child visit can carry a preventive code, per-component vaccine administration, and multiple per-instrument screenings. Three coding systems on a single claim is where a note-only tool stops being enough.


The[AI Triage Nurse](https://www.sully.ai/agents/triage-nurse) handles pre-visit intake, which is where screening instruments actually get completed. That is a direct answer to the problem this article opened with: the data an ambient tool cannot hear is often collected before the visit starts.


And the[AI Receptionist](https://www.sully.ai/agents/receptionist) handles recall and outreach, which is what keeps a periodicity schedule actually periodic.


Sully operates across[5,000+ providers](https://www.sully.ai/customer-stories/:WxcUfLIZH) , has delivered 50M+ hours of AI work, and prices each AI role[80 to 90 percent below](https://www.sully.ai/help-center/articles/what-is-the-roi-of-an-ai-medical-scribe) the human equivalent \[8\].


[Book a demo](https://www.sully.ai/contact) and bring a well-child visit with three vaccines and two screenings. That one encounter will tell you more than any feature list.


## FAQ


**Q: What should an AI scribe for pediatrics do that a general scribe does not?** It has to capture what was never said out loud. Most of a well-child visit's billable substance is structured data rather than dialogue: the growth percentile, the screening instrument name and its score, and the count of vaccine components a nurse administered. It also has to handle the reverse problem for adolescents, where parts of the note need to stay out of a parent's view. A tool that only transcribes the conversation misses both. Sully.ai's AI Scribe writes into the EHR and hands the note to the AI Coder, which submits the coded claim.


**Q: How much time do pediatricians spend in the EHR?** Access-log data from 56 primary care pediatricians found an average of 4.4 hours per workday inside scheduled clinic hours plus 0.8 hours outside them, roughly 10 minutes of after-hours EHR time for every hour in clinic \[2\]. Worth knowing before you buy: only 8 percent of that after-hours time went to documentation and orders, while 78 percent went to reviewing data and reports \[2\]. A scribe addresses the 8 percent.


**Q: How is vaccine administration coded for children?** Per component, not per injection. 90460 covers the first or only component of each vaccine when a physician or qualified health professional counsels the family face to face, and +90461 covers each additional component within that vaccine. A component is the set of antigens preventing disease caused by one organism, so MMR counts as three. Conjugates and adjuvants do not count. Without counseling, or for patients 19 and over, 90471 and +90472 apply instead and are counted per injection \[4\].


**Q: Which preventive code applies to a well-child visit?** It depends on the patient's age on the date of service and whether they are new or established. New patient: 99381 for an infant under 1 year, 99382 early childhood 1 to 4, 99383 late childhood 5 to 11, 99384 adolescent 12 to 17. Established patient: 99391 through 99394 across the same bands. Diagnosis is Z00.129 for a routine child health exam without abnormal findings, or Z00.121 with \[4\]. Screening tests and immunization administration are reported separately, not bundled into the preventive code \[4\].


**Q: Do you have free pediatric note templates?** Yes. Sully publishes three: a two-page well-child visit note built to the Bright Futures and AAP periodicity schedule, a sick visit note with a weight-based dosing record, and an ADHD evaluation built to the AAP clinical practice guideline. Each carries the payer rules and the recommended screening ages printed on the page.


## Sources


\[1\] **Applied Clinical Informatics** —[Electronic Health Record Adolescent Confidentiality in a Safety Net Setting](https://pmc.ncbi.nlm.nih.gov/articles/PMC10620039/) \[2\] **JMIR Medical Informatics** —[Characterization of Electronic Health Record Use Outside Scheduled Clinic Hours Among Primary Care Pediatricians](https://pmc.ncbi.nlm.nih.gov/articles/PMC9136654/) \[3\] **Commure** —[AI Scribe for Pediatrics](https://www.commure.com/blog-scribe/ai-scribe-for-pediatrics) \[4\] **American Academy of Pediatrics** —[Coding for Pediatric Preventive Care](https://downloads.aap.org/AAP/PDF/Coding%20Preventive%20Care.pdf) \[5\] **Bright Futures and American Academy of Pediatrics** —[Recommendations for Preventive Pediatric Health Care](https://downloads.aap.org/AAP/PDF/periodicity_schedule.pdf) \[6\] **American Academy of Pediatrics** —[Clinical Practice Guideline for the Diagnosis, Evaluation, and Treatment of Attention-Deficit/Hyperactivity Disorder in Children and Adolescents](https://publications.aap.org/pediatrics/article/144/4/e20192528/81590/Clinical-Practice-Guideline-for-the-Diagnosis) \[7\] **NASPAG and SAHM** —[The 21st Century Cures Act and Adolescent Confidentiality](https://adolescenthealth.org/advocacy/press-releases/the-21st-century-cures-act-adolescent-confidentiality/) \[8\] **Sully.ai** —[The AI Workforce for Healthcare](https://www.sully.ai/)
