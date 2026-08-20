---
schema_version: "1.0.0"
document_id: "6498f48c4897eed2eee4feb53bcf8ad03b89e991bac49d4d8842770f8c0acab5"
company_key: "yc-sully-ai"
company: "Sully.ai"
source_id: "yc-sully-ai-news-import-101ec319ffc2"
canonical_url: "https://www.sully.ai/blog/ai-scribe-for-gastroenterology-and-the-note-written-while-the-patient-is-asleep"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T12:52:08.665123+00:00"
fetched_at: "2026-08-13T12:52:10.473578+00:00"
content_hash: "sha256:af004383a632ed84e1c1458c5ba24bd8565b9844c59da39f63a57c7b63d01bb0"
---

# Best AI Scribe for Gastroenterology: Notes & Billing 2026

[Ambient AI](https://www.sully.ai/medical-scribe) has one premise. A clinician and a patient talk, and the tool turns the talking into a note.


Now think about a colonoscopy. The patient is sedated. There is no conversation to capture, and the note still has to get written.


That is not a small edge case in gastroenterology. It is the procedure that defines the specialty.


> **Key Takeaway:** An AI scribe for gastroenterology runs into a problem no other specialty has: for the procedure that defines the specialty, there is no conversation to capture. The patient is sedated. A colonoscopy note is built from what the endoscopist observed, the images, the clock times, and the specimens, none of which is dialogue. That note then has three readers with different requirements. The payer needs to know whether the exam began as a screening, because when a screening becomes therapeutic the claim needs modifier PT for Medicare and modifier 33 for commercial and Medicaid plans, and getting that wrong is what produces surprise patient bills \[1\]. The next endoscopist needs the surveillance interval. The quality registry computes adenoma detection rate, cecal intubation rate, withdrawal time and resection documentation straight from the note, against 2024 ASGE and ACG targets of 35 percent, 95 percent, 8 minutes and 98 percent \[2\]. Sully.ai's AI Scribe writes into the EHR and the AI Coder carries the note through to the coded claim.


## In Gastroenterology the Note Has No Conversation in It


Every other specialty in this series has a dialogue problem of some kind.[Psychiatry](https://www.sully.ai/blog/ai-scribe-for-psychiatry-and-what-prescribers-should-look-for) needs the mental status exam preserved.[Pediatrics](https://www.sully.ai/blog/ai-scribe-for-pediatrics-and-the-half-of-the-note-nobody-says-out-loud) needs data that was never spoken. Obstetrics needs a record that stays open for months.


[Gastroenterology](https://www.sully.ai/specialties/gastroenterology-scribe) has something more basic. For the central procedure, there is nothing to listen to.


### The Patient Is Asleep for the Part That Matters


A colonoscopy or an upper endoscopy happens under sedation. Whatever conversation exists happens before, during consent, and after, during instructions.


The procedure record itself is not speech. It is what the endoscopist saw, the photographs taken, the times on the clock, and the labels on the specimen jars.


### What the Note Is Actually Made Of


Indication. Bowel preparation quality. Extent of examination and the landmarks that prove it. Insertion and withdrawal times. Lesion-level findings with sizes in millimetres. Specimens. Complications. A surveillance interval.


Almost none of that is something anybody said out loud.


## Why Ambient AI Has the Least to Work With in Gastroenterology


Three consequences follow, and all three are worth testing before you buy anything.


### There Is No Dialogue to Summarize


A tool can transcribe the consent conversation flawlessly and still have produced nothing that belongs in the procedure note.


That is not a criticism of the technology. It is a statement about where the information lives. Vendors selling into GI tend to talk about handling complex terminology, which is real, and which does not address this at all.


### The Numbers Are Times and Measurements, Not Speech


Withdrawal time. Cecal intubation confirmed by landmark and photograph. A polyp measured at 6 millimetres. A Boston Bowel Prep Score by segment.


These come from the scope, the clock, and the endoscopist's eye. A microphone is not the input.


### One Note, Three Readers, Three Different Requirements


Here is the part that actually decides whether a GI note is any good.


**The payer** needs the indication and, if you removed something, the right conversion modifier. **The next endoscopist** needs enough lesion detail to plan the follow-up, plus the interval you recommended. **The quality registry** computes your numbers from fields most notes treat as optional.


A note optimised to read well serves none of the three completely. That is the thing to judge a tool on.


## The Coding Rule That Creates Surprise Patient Bills


If you take one operational thing from this article, take this one.


### Screening, Surveillance and Diagnostic Are Three Different Claims


For Medicare, **G0121** is average-risk screening at a 10-year interval and **G0105** is high-risk screening at 24 months \[1\].


High risk has a specific definition: a sibling, parent or child with colorectal cancer or an adenomatous polyp, familial adenomatous polyposis, Lynch syndrome, a personal history of adenomatous polyps or colorectal cancer, or inflammatory bowel disease \[1\].


If the indication is not documented plainly, the coder has to infer it, and the patient discovers the inference on a bill.


### When a Screening Becomes Therapeutic the Modifier Depends on the Payer


A screening colonoscopy where you remove something is no longer only a screening. The claim needs the therapeutic code: 45380 for biopsy, 45384 for hot biopsy forceps, 45385 for snare, 45388 for ablation \[1\].


It also needs a modifier that preserves the screening origin, so the patient keeps the screening benefit. **That modifier is payer-dependent. Modifier PT for Medicare. Modifier 33 for commercial and Medicaid plans** \[1\].


Same clinical event, different modifier depending on who is paying. Use the wrong one and the screening benefit is lost, which is the mechanism behind a large share of angry post-colonoscopy phone calls.


### What the Patient Actually Owes


Under the Consolidated Appropriations Act, when a polyp is removed during a Medicare screening colonoscopy the beneficiary owes **15 percent from 2023 to 2026** , then 10 percent from 2027 to 2029, and nothing from 2030 onward \[1\].


That reduction is not automatic. It depends on the claim carrying the screening origin correctly.


## Free Gastroenterology Note Templates


Three templates for the notes where documentation decides the payment and the quality number. Each carries the rules printed on the page.


-


[Colonoscopy procedure note template](https://www.sully.ai/templates/colonoscopy-procedure-note) , two pages, with the indication as the first field and a findings table whose columns are exactly what the quality indicator asks for


-


[Upper endoscopy procedure note template](https://www.sully.ai/templates/upper-endoscopy-note) , for EGD, with extent, landmarks and the biopsy protocol as explicit fields rather than assumptions


-


[GI consultation note template](https://www.sully.ai/templates/gi-consultation-note) , for the new referral, with alarm features recorded as present or absent and an E/M path the note can support


## Your Quality Numbers Are Documentation Numbers


This is the second thing worth knowing, and it changed recently enough that a lot of habits are out of date.


### The 2024 Update Moved Withdrawal Time From Six Minutes to Eight


The ASGE and ACG quality task force updated the colonoscopy quality indicators in 2024, the first revision since 2015 \[2\].


The minimum average withdrawal time in normal colonoscopies for patients 45 and older is now **8 minutes** , up from six. Forty-five is the age screening now starts \[4\]. Evidence suggests around nine minutes optimises detection of both adenomas and serrated lesions \[2\]\[3\].


If your unit is still auditing against six, it is auditing against the previous standard.


### The Priority Indicators and Their Targets


Adenoma detection rate **≥35 percent** for screening, surveillance and diagnostic exams combined in patients 45 and older, and **≥50 percent** after a positive fecal test. Sessile serrated lesion detection **≥6 percent** . Cecal intubation **≥95 percent** . Adequate bowel preparation **≥90 percent** in outpatient settings \[2\].


### Resection Documentation Is Itself an Indicator


This is the one that makes the argument. Among the new indicators: lesion **size, shape, location and resection method documented for ≥98 percent of resected lesions** , and cold snare used for ≥90 percent of 4 to 9 millimetre lesions \[2\].


Read that first one again. It is a documentation target, not a technique target. A technically perfect polypectomy described as "polyp removed from the sigmoid" fails it.


### Why This Makes the Note the Measurement


A 1 percent increase in adenoma detection rate is associated with roughly a 3 percent lower colorectal cancer incidence and a 5 percent lower mortality risk \[3\].


That is a real clinical effect. And it can only be computed from what the note records, which means in practice your quality numbers measure your documentation at least as much as your endoscopy.


## How to Evaluate an AI Scribe for a GI Practice


Four questions, in the order that matters.


### Does It Do Anything for the Procedure Note


Ask where the procedure note is going to live. In most endoscopy units it lives in a dedicated endoscopy reporting system rather than the EHR, and a documentation platform that writes into the EHR is not going to change that. The fair question is not whether a vendor solves the procedure note, it is whether they are honest about which part of your documentation they touch.


### Can It Capture Times and Measurements, Not Just Speech


Withdrawal time, prep score, lesion size, cecal photodocumentation. Ask where each one comes from in their workflow.


### Does It Carry the Indication Through to the Claim


The indication is set at the start and has to survive to the modifier on the claim line. Ask what happens to it in between.


### Does It Reach the Claim and the Registry


A note that stops at the chart has not helped the claim and has not helped the quality report either.


## Where Sully.ai Fits in a GI Practice


Worth being straight about the limit first, because a GI practice will spot it immediately. No ambient tool listens its way to a colonoscopy findings table, and Sully's does not either. In most units that findings table lives in a dedicated endoscopy reporting system, and it will keep living there.


So here is the honest scope. The clinic side of gastroenterology, the consults, the follow-ups, the IBD and reflux and liver visits, is ordinary ambient territory and a large share of a GI physician's week. The chart and the claim are where a connected platform earns its keep.


Sully's[AI Scribe](https://www.sully.ai/agents/scribe) captures documentation during and after the encounter and writes it into the EHR, on a single integration across[Epic, Cerner, Meditech, Athenahealth and ambulatory and specialty systems](https://www.sully.ai/integrations) . Clinicians can save how they want a note structured, so a findings table stays a table instead of dissolving into a paragraph.


The[AI Coder](https://www.sully.ai/agents/medical-coder) then extracts the ICD-10 and CPT codes and submits the claim. That is the handoff that matters here, because the indication set at the start of the encounter has to survive all the way to the modifier on the claim line, and a note-only tool stops well short of it.


The[AI Receptionist](https://www.sully.ai/agents/receptionist) handles recall and outreach. In GI that is not just a care-gap tool: adherence to recommended surveillance intervals is itself one of the 2024 quality indicators \[2\].


Sully operates across[5,000+ providers](https://www.sully.ai/customer-stories/:WxcUfLIZH) , has delivered 50M+ hours of AI work, and prices each AI role[80 to 90 percent below](https://www.sully.ai/help-center/articles/what-is-the-roi-of-an-ai-medical-scribe) the human equivalent \[5\].


[Book a demo](https://www.sully.ai/contact) and bring a screening colonoscopy where you removed a 6 millimetre polyp. That single case exercises the indication, the modifier, the resection documentation and the surveillance interval all at once.


## FAQ


**Q: What should an AI scribe for gastroenterology do that a general scribe does not?** It has to be useful for a note with no conversation in it. During a colonoscopy or EGD the patient is sedated, so the procedure record comes from observation, images, clock times and specimens rather than dialogue. It also has to carry the indication through to the claim, because whether the exam was screening, surveillance or diagnostic decides how it bills, and it has to capture the times and measurements the quality indicators are computed from. Sully.ai's AI Scribe writes the note into the EHR and hands it to the AI Coder, which submits the coded claim.


**Q: What modifier do you use when a screening colonoscopy becomes therapeutic?** It depends on the payer. Report the therapeutic CPT code, 45380 for biopsy, 45384 for hot biopsy forceps, 45385 for snare, or 45388 for ablation, and append modifier PT for Medicare claims or modifier 33 for commercial and Medicaid claims. The modifier preserves the screening origin so the patient keeps the screening benefit. Using the wrong one for the payer is a common cause of surprise patient bills \[1\].


**Q: What does a Medicare patient owe if a polyp is removed during a screening colonoscopy?** Under the Consolidated Appropriations Act the beneficiary is responsible for 15 percent of the cost from 2023 to 2026, 10 percent from 2027 to 2029, and nothing from 2030 onward \[1\].


**Q: What are the current colonoscopy quality indicator targets?** The 2024 ASGE and ACG update, the first since 2015, sets adenoma detection rate at ≥35 percent for screening, surveillance and diagnostic exams combined in patients 45 and older and ≥50 percent after a positive fecal test, sessile serrated lesion detection at ≥6 percent, cecal intubation at ≥95 percent, withdrawal time at ≥8 minutes, adequate bowel preparation at ≥90 percent in outpatient settings, and documentation of lesion size, shape, location and resection method for ≥98 percent of resected lesions \[2\].


**Q: Do you have free gastroenterology note templates?** Yes. Sully publishes three: a two-page colonoscopy procedure note, an upper endoscopy note, and a GI consultation note. The colonoscopy findings table uses the four fields the quality indicator asks for, and the coding block carries the modifier PT versus modifier 33 rule.


## Sources


\[1\] **American Gastroenterological Association** —[Coding FAQ: Screening Colonoscopy](https://gastro.org/practice-resources/reimbursement/coding/coding-faq-screening-colonoscopy/) \[2\] **ASGE and ACG Quality Task Force** —[Quality Indicators for Colonoscopy, 2024 Update](https://info.asge.org/083024-colon-asge/acg-quality-task-force-updates-quality-indicators-for-colonoscopy) \[3\] **Gastroenterology Report** —[Key Quality Indicators in Colonoscopy](https://pmc.ncbi.nlm.nih.gov/articles/PMC10005623/) \[4\] **US Preventive Services Task Force** —[Colorectal Cancer: Screening](https://www.uspreventiveservicestaskforce.org/uspstf/recommendation/colorectal-cancer-screening) \[5\] **Sully.ai** —[The AI Workforce for Healthcare](https://www.sully.ai/)
