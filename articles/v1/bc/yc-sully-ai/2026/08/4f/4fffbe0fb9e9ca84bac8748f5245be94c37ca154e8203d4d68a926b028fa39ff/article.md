---
schema_version: "1.0.0"
document_id: "4fffbe0fb9e9ca84bac8748f5245be94c37ca154e8203d4d68a926b028fa39ff"
company_key: "yc-sully-ai"
company: "Sully.ai"
source_id: "yc-sully-ai-news-import-101ec319ffc2"
canonical_url: "https://www.sully.ai/blog/ai-scribe-for-ob-gyn-and-the-note-that-bills-eight-months-later"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-12T23:45:28.500711+00:00"
fetched_at: "2026-08-12T23:45:33.547478+00:00"
content_hash: "sha256:6cad0604a9036586ddaf56b5d3601e9bc1294cae2b86087b9f14dd42fd26a290"
---

# AI Scribe for OB-GYN and the Note That Bills Eight Months Later

Write a prenatal note today and no claim goes out. Not that day, not that month.


That note joins twelve others and a delivery, and the whole thing bills once, as a package, sometimes eight months after the visit you just documented.


Which makes obstetrics the one specialty where the note is not finished when the visit is. It stays provisional until the pregnancy closes.


> **Key Takeaway:** An AI scribe for OB-GYN has to understand that the unit of documentation is the pregnancy, not the visit. A single prenatal note does not produce a claim. It contributes to a global obstetric package billed once, after delivery. Routine antepartum care is usually about 13 visits, and if your group does not provide all of them, because the patient transferred, was referred, changed insurer, or miscarried, the pregnancy does not bill as a package at all: it becomes 59426 for seven or more antepartum visits, 59425 for four to six, or itemized E/M for one to three \[1\]. The number of visits is therefore a billing fact, and almost nobody is recording it. Ambient scribes are built around one conversation producing one note, which is the wrong shape for a record whose billing state stays open for most of a year. Sully.ai's AI Scribe writes into the EHR across Epic, Cerner, Meditech, Athenahealth and specialty platforms including ModMed, and the AI Coder carries the note through to the coded claim.


## In Obstetrics the Unit of Documentation Is the Pregnancy


Most specialties settle up at the end of the encounter. You document, you code, the claim goes out, the visit is closed.


[Obstetric care](https://www.sully.ai/specialties/ob-gyn-scribe) is billed as a package instead. One code covers antepartum care, the delivery, and postpartum care as a single event \[1\].


### One Prenatal Note Does Not Produce a Claim


The global codes are 59400 for vaginal delivery, 59510 for cesarean, 59610 for vaginal after previous cesarean, and 59618 for cesarean following an attempted VBAC \[1\].


All routine prenatal visits until delivery sit inside them, approximately 13 for an uncomplicated pregnancy \[1\].


So a prenatal visit is real clinical work that generates no claim of its own. It is a deposit against a payment that comes later.


### The Visit Count Is a Billing Fact


Here is the part that catches practices out. Whether this pregnancy bills as a package at all depends on how many antepartum visits your group actually provided.


Seven or more, without completing the package, is **59426** . Four to six is **59425** . One to three, and you itemize each E/M visit instead \[1\].


That is a number, not a narrative. And it is a number almost nobody is keeping, because no part of the documentation workflow asks for it until the moment it matters.


## Why an Encounter-Shaped Scribe Cannot Close an Obstetric Package


Every[AI scribe](https://www.sully.ai/medical-scribe) pitch for OB-GYN is organised by visit type. Prenatal, postpartum, annual gyn, contraception, infertility. That framing treats obstetrics as a set of encounters that happen to be about pregnancy.


**The problem is not the encounters. It is that they belong to one record whose billing state stays open for most of a year.** A tool built to produce one note per conversation has no concept of that.


Three failure modes follow, and all three are testable before you buy.


### It Has No Concept of an Open Package


An ambient scribe finishes when the conversation finishes. It has done its job.


In obstetrics nothing has been resolved yet. The note is one of thirteen contributions to a claim that does not exist yet.


### It Cannot Tell You Which Visits Were Inside the Package


A routine prenatal visit is bundled. A visit for a condition unrelated to the pregnancy, a chest infection, a urinary tract infection, is separately reportable \[1\].


On a recording those sound identical. Both are a clinician and a pregnant patient talking about a symptom. The distinction is a billing category, not an audible one, and it is the same[separability problem primary care runs into with modifier 25](https://www.sully.ai/blog/ai-scribe-for-primary-care-and-the-notes-that-decide-what-gets-paid) , and getting it wrong costs money in both directions: under-bill the unrelated problem, or double-bill something already inside the package and face recoupment.


### A Transfer of Care Rewrites What You Already Documented


Itemization applies when a patient transfers into or out of a practice, is referred, delivers with a physician outside the group, terminates or miscarries, or changes insurer mid-pregnancy \[1\].


At that moment every note you have already written changes meaning. Six routine prenatal visits stop being package components and become the basis for a 59425 claim.


Nothing in a per-conversation tool knows that the last six notes were just re-classified.


## What the Global Obstetric Package Includes and Excludes


This is the list that prevents both under-billing and recoupment, and not one of the vendor guides written for OB-GYN publishes it.


### The Confirmatory Visit Is the One You Can Still Bill


The initial E/M to diagnose pregnancy is excluded from the package and separately reportable, but **only if the antepartum record was not initiated at that visit** , supported by diagnosis code Z32.01 \[1\].


Start the record at the confirmatory visit and you have absorbed it. That is a decision made by a front-desk workflow, not a clinical one.


### What Is Bundled and Must Not Be Billed Twice


Inside the package: all routine prenatal visits, initial and subsequent history and physical exams, recording of weight, blood pressures and fetal heart tones, routine chemical urinalysis, hospital admission including history and physical, inpatient E/M within 24 hours of delivery, management of uncomplicated labor, the delivery and the placenta, administration of IV oxytocin, repair of first- and second-degree lacerations, simple removal of cerclage, uncomplicated inpatient visits after delivery, routine outpatient E/M within six weeks of delivery, and breastfeeding and newborn education \[1\].


### What Sits Outside the Package


Separately reportable: laboratory tests other than routine chemical urinalysis, maternal and fetal ultrasound, amniocentesis, amnioinfusion, chorionic villus sampling, fetal contraction stress tests, fetal non-stress tests, external cephalic version, insertion of a cervical dilator more than 24 hours before delivery, E/M for conditions unrelated to the pregnancy, and additional visits for complications or high-risk monitoring beyond the typical 13 \[1\].


One more that gets missed. Third- and fourth-degree laceration repair is reported by appending **modifier 22** to the global or delivery-only code, and the record has to contain documentation that justifies it \[1\].


### Antepartum-Only Codes and the Visit Thresholds


59426 for seven or more antepartum visits. 59425 for four to six. Itemized E/M for one to three \[1\].


Which means the difference between two codes, and a real difference in payment, can come down to whether a patient attended a sixth or a seventh visit before moving away. If nobody counted, nobody knows.


## Free OB-GYN Note Templates


Three templates for the documentation that decides the payment. Each carries the payer rules printed on the page.


-


[Prenatal record template](https://www.sully.ai/templates/prenatal-record) for the pregnancy as a whole, two pages covering the initial obstetric visit and a numbered 13-row flow sheet with a package tracker, because the count is what decides the code


-


[Postpartum visit note template](https://www.sully.ai/templates/postpartum-visit-note) for the fourth trimester, with mood screening by named instrument and the long-term risk handoff most postpartum notes leave out


-


[Well-woman annual exam template](https://www.sully.ai/templates/well-woman-annual-exam) for the preventive visit, with screening recorded as a test and a next-due date rather than an assumed annual cadence


## The Postpartum Window Your Patient Needs Is Not the One You Get Paid For


There is a gap here that nobody names out loud, and clinicians live inside it.


### ACOG Wants Contact at Three Weeks and a Comprehensive Visit by Twelve


ACOG recommends contact with a maternal care clinician within the first three weeks after birth, ongoing care as needed, and a comprehensive postpartum visit no later than 12 weeks, covering physical, social, and psychological well-being \[2\].


That reframed postpartum care as a process rather than a single appointment.


### Payers Still Define Postpartum as Six Weeks


Payer policy generally still treats the postpartum period as six weeks following delivery \[1\].


So the care your patient needs at ten weeks and the window your contract recognises are two different things. Worth knowing which one you are working in before you plan the visit.


### What Is Separately Reportable After Delivery


Uncomplicated outpatient visits related to the pregnancy are bundled. So is discussion of contraception \[1\].


Evaluation and management of a genuine problem or complication is separately reportable \[1\]. And 59430 is the postpartum-care-only code, for when your group did not provide the rest of the package.


## Cervical Cancer Screening Changed in 2026


This one matters because a lot of templates and a lot of habits are still running on the previous guidance.


### The Current Intervals


ACOG's current statement recommends cytology alone every three years for ages 21 to 29, and **clinician-collected primary hrHPV screening every five years for ages 30 to 65** \[3\].


That is the change. For the 30 to 65 band, primary hrHPV screening is now the recommended method rather than one of three equivalent options alongside cytology and co-testing. Routine screening stops after 65 for patients with adequate prior negative screening \[3\].


### Record the Test and the Next Due Date


Because intervals now differ by test, the annual visit has stopped being the unit of screening.


The practical fix is small: record which test was performed and when it is next due, rather than relying on a remembered interval. That also means your record stays correct the next time guidance moves, which it will.


### The Medicare Codes That Get Billed Wrong


G0101 is the screening pelvic and clinical breast examination. Q0091 is obtaining and conveying the screening Pap specimen \[4\].


Medicare covers them every 24 months at normal risk, and annually for patients at high risk and for patients of childbearing age who have had an abnormal Pap within the past three years \[4\]\[5\].


Do not bill Q0091 separately when the specimen collection is already part of a preventive visit you are billing.


## How to Evaluate an AI Scribe for an OB-GYN Practice


Four questions, in the order that matters.


### Does It Know a Pregnancy Is One Record


Ask how it handles a longitudinal record rather than an encounter. If the demo is a single prenatal visit, you have not seen it do the job.


### Does It Count Antepartum Visits


Ask directly whether anything in the workflow tells you how many antepartum visits this patient has had with your group. If the answer is that you would check the chart, that is the gap.


### Does It Separate Bundled From Billable


Give it a prenatal visit that turns into treatment for a urinary tract infection. Check whether the output makes the unrelated problem separable, or writes one narrative.


### Does It Reach the Claim


A note that stops at the chart has not helped assemble a claim built from eight months of visits.


## Where Sully.ai Fits in an OB-GYN Practice


Sully's[AI Scribe](https://www.sully.ai/agents/scribe) captures documentation during and after the visit and writes it into the EHR, on a single integration across[Epic, Cerner, Meditech, Athenahealth, and ambulatory and specialty systems including ModMed, Elation, Practice Fusion, NextGen and AdvancedMD](https://www.sully.ai/integrations) . Clinicians can save how they want a note structured, so a flow sheet stays a flow sheet rather than dissolving into prose.


The[AI Coder](https://www.sully.ai/agents/medical-coder) then extracts the ICD-10 and CPT codes and submits the claim. That handoff matters more in obstetrics than anywhere else, because the claim is assembled from a year of visits rather than written at the end of one.


The[AI Receptionist](https://www.sully.ai/agents/receptionist) handles recall and outreach. In most specialties that is a care-gap tool. In obstetrics it has a direct billing consequence, because a missed visit can move a pregnancy from a global code to an antepartum-only one.


Sully operates across[5,000+ providers](https://www.sully.ai/customer-stories/:WxcUfLIZH) , has delivered 50M+ hours of AI work, and prices each AI role[80 to 90 percent below](https://www.sully.ai/help-center/articles/what-is-the-roi-of-an-ai-medical-scribe) the human equivalent \[6\].


[Book a demo](https://www.sully.ai/contact) and bring a pregnancy that transferred care at 20 weeks. That is the case that shows you what a tool actually understands.


## FAQ


**Q: What should an AI scribe for OB-GYN do that a general scribe does not?** It has to treat the pregnancy as the record, not the visit. A prenatal note produces no claim on its own; it contributes to a global obstetric package billed once after delivery. So the tool has to help you track how many antepartum visits your group provided, keep bundled visits separate from separately reportable ones, and survive a transfer of care that changes how everything already documented gets billed \[1\]. Sully.ai's AI Scribe writes the note into the EHR and hands it to the AI Coder, which submits the coded claim.


**Q: What is included in the global obstetric package?** All routine prenatal visits until delivery, usually about 13 for an uncomplicated pregnancy, initial and subsequent history and physical exams, weight, blood pressure and fetal heart tones, routine chemical urinalysis, hospital admission, inpatient E/M within 24 hours of delivery, management of uncomplicated labor, the delivery and placenta, IV oxytocin, repair of first- and second-degree lacerations, uncomplicated inpatient visits after delivery, routine outpatient E/M within six weeks of delivery, and breastfeeding and newborn education \[1\].


**Q: When do you bill 59425 or 59426 instead of a global code?** When your group did not provide the whole package. 59426 covers seven or more antepartum visits, 59425 covers four to six, and you itemize each E/M if you provided only one to three. This comes up when a patient transfers into or out of the practice, is referred, delivers with a physician outside the group, terminates or miscarries, or changes insurer mid-pregnancy \[1\].


**Q: How often should cervical cancer screening be done?** ACOG's current statement recommends cytology alone every three years for ages 21 to 29, and clinician-collected primary hrHPV screening every five years for ages 30 to 65, which is a change from the previous three equivalent options for that age band. Routine screening stops after 65 with adequate prior negative screening \[3\]. Because intervals now differ by test, record the test performed and the next-due date rather than assuming an annual cadence.


**Q: Do you have free OB-GYN note templates?** Yes. Sully publishes three: a two-page prenatal record with a 13-visit flow sheet and package tracker, a fourth trimester postpartum visit note, and a well-woman annual exam updated for the current screening guidance. Each carries the payer rules printed on the page.


## Sources


\[1\] **UnitedHealthcare** —[Commercial and Individual Exchange Reimbursement Policy: Obstetrical Policy 2026R0064A](https://www.uhcprovider.com/content/dam/provider/docs/public/policies/comm-reimbursement/COMM-Obstetrical-Policy.pdf) \[2\] **American College of Obstetricians and Gynecologists** —[Optimizing Postpartum Care](https://www.acog.org/clinical/clinical-guidance/committee-opinion/articles/2018/05/optimizing-postpartum-care) \[3\] **American College of Obstetricians and Gynecologists** —[Screening for Cervical Cancer](https://www.acog.org/clinical/clinical-guidance/committee-statement/articles/2026/07/screening-for-cervical-cancer) \[4\] **Centers for Medicare & Medicaid Services** —[Screening Pap Tests and Pelvic Examinations, MLN909032](https://www.cms.gov/Outreach-and-Education/Medicare-Learning-Network-MLN/MLNProducts/Downloads/Screening-PapPelvic-Examinations.pdf) \[5\] **Centers for Medicare & Medicaid Services** —[National Coverage Determination 210.2: Screening Pap Smears and Pelvic Examinations](https://www.cms.gov/medicare-coverage-database/view/ncd.aspx?NCDId=185) \[6\] **Sully.ai** —[The AI Workforce for Healthcare](https://www.sully.ai/)
