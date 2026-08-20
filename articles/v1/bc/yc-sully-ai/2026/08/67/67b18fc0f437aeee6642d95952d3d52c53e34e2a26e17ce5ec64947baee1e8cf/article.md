---
schema_version: "1.0.0"
document_id: "67b18fc0f437aeee6642d95952d3d52c53e34e2a26e17ce5ec64947baee1e8cf"
company_key: "yc-sully-ai"
company: "Sully.ai"
source_id: "yc-sully-ai-news-import-101ec319ffc2"
canonical_url: "https://www.sully.ai/blog/ai-scribe-for-primary-care-and-the-notes-that-decide-what-gets-paid"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-11T04:59:15.933945+00:00"
fetched_at: "2026-08-11T04:59:18.055748+00:00"
content_hash: "sha256:ed1540921a28c4dfc973906851a8ec25d0c1748f3e82e4189dd679198aa7fc33"
---

# Best AI Scribe for Primary Care: Billing & Notes Guide 2026

A physical that turns into a conversation about blood pressure is two services. Most primary care notes record it as one.


That gap is where a lot of primary care revenue quietly goes. The work happened, the physician did it, and the note described it so smoothly that nobody could tell where the wellness visit ended and the problem visit began.


Which is a strange thing to ask an AI scribe to fix, because a smooth, readable note is exactly what an[ambient scribe](https://www.sully.ai/medical-scribe) is built to produce.


> **Key Takeaway:** An AI scribe for primary care has to do more than shorten the note. It has to keep the note billable. Between 40 and 55 percent of preventive visits surface an additional problem, and when the preventive service and the problem-oriented E/M get blended into one narrative, wellness visits with same-day problem evaluations see denial rates of 22 to 35 percent \[1\]. Modifier 25 only pays when the note shows a chief complaint, an HPI, an exam, and medical decision making for the problem that stand apart from the physical. The evidence on time savings is also more modest than the marketing suggests: a randomized trial of 238 physicians found a 9.5 percent reduction in time-in-note for one tool and no significant change for another \[2\]. Sully.ai's AI Scribe writes into the EHR across Epic, Cerner, Meditech, and Athenahealth, plus primary care systems including Elation, Practice Fusion, NextGen, and AdvancedMD, and the AI Coder carries the note through to the coded claim.


## Primary Care Documentation Decides What the Visit Is Worth


In most specialties the note is a clinical record that also happens to support a bill. In primary care it is closer to the reverse. The note is the evidence for a set of coding decisions that get made on every single encounter.


That is not true of a colonoscopy or a joint replacement, where the procedure code is obvious. It is very true of a 20 minute visit covering three chronic problems, a medication change, and a flu shot.


### Family Medicine Spends Two Hours in the EHR for Every Hour of Care


The scale of the documentation problem is well measured. Researchers at the University of Wisconsin and the AMA tracked 142[family medicine physicians](https://www.sully.ai/specialties/family-medicine-scribe) using EHR event logs plus direct time-motion observation, and found they spent close to two hours on EHR work for every hour of direct patient care \[3\].


In a typical 11.4 hour workday, that came to[roughly six hours inside the record](https://www.sully.ai/help-center/articles/how-much-time-does-an-ai-scribe-save) .


Documentation and order entry made up the largest slice. Which is why it is the obvious thing to automate, and why every scribe vendor points at this number.


### The Note Is the Claim


Here is the part the time-savings pitch skips.[Primary care](https://www.sully.ai/specialty/primary-care) carries more coding decisions per visit than almost any other outpatient specialty.


Every encounter needs an E/M level. Preventive codes are banded by age and by whether the patient is new or established. Medicare wellness visits use their own G-codes with their own rules.[Modifiers decide whether two services on one day both get paid](https://www.sully.ai/blog/improving-cpt-modifier-accuracy-with-ai-assisted-medical-coding) .


Family medicine encounters average around three diagnoses each. Every one of those decisions is defended, or not, by what the note says.


## Why One Smooth Note Is the Wrong Output for Primary Care


Nearly every AI scribe guide written for primary care argues about minutes saved per note. That is the wrong first question, and the strongest evidence says the minutes are smaller than advertised anyway.


**The question that matters is whether the note keeps the work billable.** Primary care is the specialty where a single visit is most often two services. A physical that turns into a blood pressure decision. A wellness visit where the patient mentions their knee. Those are separately payable, but only when the documentation shows two services that stand on their own.


An ambient scribe is built to produce one clean, flowing narrative. That instinct is precisely what payers deny. A tool that writes a beautiful blended note has not saved you anything. It has moved the cost from your evening to your accounts receivable.


### A Wellness Visit Is Often Two Services


Somewhere between 40 and 55 percent of patients who come in for a wellness exam raise an additional problem while they are there \[1\].


This is not an edge case. It is closer to half of the preventive schedule.


That problem-oriented work is separately payable alongside the preventive service. Most practices absorb it instead, because sorting out the documentation feels like more trouble than the extra reimbursement is worth.


### Blended Documentation Is What Payers Deny


For the practices that do bill it, the second failure point is documentation quality. Wellness visits billed with a same-day problem evaluation see denial rates of 22 to 35 percent when the modifier use or the documentation is wrong \[1\].


Modifier 25 is among the most audited modifiers in Medicare, and MAC scrutiny of annual wellness visit claims increased through 2025 and 2026 \[1\].


The failure mode is specific and worth stating plainly. The history, assessment, and plan for the problem have to stand alone. Blended documentation results in the problem-oriented E/M being denied \[1\].


### Ambient Tools Optimize for Readability, Not Separability


An ambient scribe hears one conversation and writes one narrative, because that is what a well-written note looks like to a human reader.


Separability is a billing property, not a prose property. Nothing in a general-purpose summarizer knows to preserve it.


Some vendors have gotten close to this by accident. A few now structure notes by problem, giving each its own history and plan, and they market it as clinical clarity. It happens to be the right shape for a modifier 25 claim too, but that is not why they built it, and none of them tell you to use it that way.


## The Three Documentation Traps in a Primary Care Day


These are the three places where a note either supports the claim or quietly fails it.


### Selecting an E/M Level on Time or Medical Decision Making


Since the 2021 revision, office visit E/M is selected on either total time on the date of the encounter or medical decision making. Never both. It is 99202 to 99205 for a new patient, 99212 to 99215 for an established one.


Pick the time path and the note has to record the total time. Pick the MDM path and the note has to show the problems addressed, the data reviewed, and the risk.


A note that lists diagnoses without any reasoning supports neither path. That is the most common shape an AI-generated assessment takes.


### Modifier 25 and the Four Elements It Requires


Append modifier 25 to the problem E/M when the problem work is significant and separately identifiable from the preventive service.


Four things have to be visible in the note for that problem specifically:


1.


A chief complaint distinct from the preventive visit


2.


An HPI for that problem


3.


An exam relevant to it


4.


Medical decision making with a treatment plan


Then link the diagnoses correctly. The problem diagnosis goes on the E/M line, the preventive diagnosis on the preventive line \[1\].


### The Medicare Annual Wellness Visit Element List


The AWV is a fixed checklist, and a visit that misses an item does not get paid.


G0438 is the initial visit, once per lifetime, and it has to come before G0439. Neither is payable within 365 days of G0402, the initial preventive physical exam. A second G0438 auto-denies \[4\].


Two elements are mandatory and get missed constantly: the health risk assessment, and the written personalized prevention plan actually furnished to the patient \[4\]\[5\]. Screenings for depression, cognition, and functional ability need the tool named and the result recorded. "Screened, negative" is what gets rejected.


One more thing that surprises people. An AWV is not a head-to-toe physical, and Medicare does not cover a routine annual physical.


## Free Primary Care Note Templates


These three cover the visits where the documentation actually decides the payment. Each one has the payer rules printed on the page, which is the part a blank SOAP box in the EHR will never give you.


-


[Primary care office visit note template](https://www.sully.ai/templates/primary-care-office-visit-note) for the everyday problem-oriented visit, structured so the note supports the level of service you bill on either time or medical decision making


-


[Medicare Annual Wellness Visit template](https://www.sully.ai/templates/medicare-annual-wellness-visit) covering every required element of G0438 and G0439 across two pages, including the health risk assessment and the written prevention plan


-


[Preventive visit plus problem visit template](https://www.sully.ai/templates/preventive-plus-problem-visit) for modifier 25, splitting the visit into a preventive section and a problem section with the four required elements as numbered fields


## What the Evidence Actually Says About AI Scribes in Primary Care


The published research on AI scribes is better than most vendor pages suggest and worse than most vendor claims. Both things are true at once, and the spread is wide enough to matter when you are choosing.


### Where the Time Savings Are Real


The Permanente Medical Group ran the largest evaluation to date. Across 7,260 physicians and more than 2.5 million encounters over 63 weeks, AI scribe use added up to the equivalent of 1,794 working days saved in a year \[6\].


Physicians reported a positive effect on patient interactions at 84 percent, and improved work satisfaction at 82 percent \[6\]. Primary care was among the highest-utilization specialties.


Patients noticed too. Forty-seven percent said their doctor spent less time looking at a computer \[6\].


### Where the Numbers Disagree


Then there is the randomized evidence, which is more sober.


A pragmatic randomized trial put 238 outpatient physicians across 14 specialties into three arms: two different ambient scribes and a usual-care control. One tool produced a 9.5 percent decrease in time-in-note against control, about 41 seconds per note. The other showed no significant change at all \[2\].


A primary care simulation study reported a 69.1 percent reduction in documentation time, but that was nine physicians working with standardized patients rather than a real panel \[7\].


The honest read: the ceiling is real, the average is modest, and most of the variance comes down to the tool and how well it fits the workflow. Which is a good argument for judging a scribe on something other than its time claim.


### What AI Scribes Still Get Wrong


Family medicine faculty writing in CMAJ flagged the failure modes directly. AI scribes can hallucinate content that did not happen, omit key information, and struggle with languages other than English \[8\].


They also specifically struggle with documenting physical examinations \[8\].


That last one lands harder in primary care than anywhere else, because the exam is the substance of a preventive visit. If the exam documentation is thin, the preventive code is the thing at risk.


The same authors note that AI scribes remain unregulated, and that the clinician is responsible for the final note no matter who drafted it \[8\].


None of that is an argument for waiting. It is an argument for choosing on the right criteria, because every one of those failure modes is something you can test for before you sign anything.


## How to Evaluate an AI Scribe for a Primary Care Practice


Four questions, in the order that matters.


### Does It Separate Problems or Blend Them


Give it a recorded visit that starts as a physical and turns into a hypertension decision. Look at the output and ask whether it contains two sections that could each stand alone, or one story.


This is the test most demos will fail, and it is the one that costs you money.


### Does It Support the Level of Service You Bill


Ask whether it captures total time on the date of the encounter. Then ask whether the assessment carries reasoning or just a diagnosis list.


Those are the two paths to an E/M level. A note has to serve one of them.


### Does It Carry the Preventive Element List


Run an annual wellness visit through it. Check whether the health risk assessment, the screening tools and their scores, and the written prevention plan come out as discrete elements or get summarized into prose.


### Does It Write Into the EHR and Reach the Claim


A note you copy and paste is a note you handled twice. At 22 patients a day that adds up fast.


And a note that stops at the chart, never informing the code, has solved half the problem.


## Where Sully.ai Fits in a Primary Care Practice


Separability is really a note-structure problem, and note structure is something you set once instead of policing every visit. Clinicians using Sully can save how they want a note built, so a preventive section and a problem section stay distinct by default rather than depending on discipline on a busy Thursday.


Sully's[AI Scribe](https://www.sully.ai/agents/scribe) captures documentation during and after the visit and writes it into the EHR on a single integration, across[Epic, Cerner, Meditech, Athenahealth, and primary care systems including Elation, Practice Fusion, NextGen, and AdvancedMD](https://www.sully.ai/integrations) .


The[AI Coder](https://www.sully.ai/agents/medical-coder) then extracts the ICD-10 and CPT codes and submits the claim. That handoff is the point. The note and the claim stop being two separate jobs done by two separate people weeks apart.


The[AI Receptionist](https://www.sully.ai/agents/receptionist) handles the recall and outreach that fill the wellness visit schedule in the first place, which is the other half of preventive revenue.


Sully operates across[5,000+ providers](https://www.sully.ai/customer-stories/:WxcUfLIZH) , has delivered 50M+ hours of AI work, and prices each AI role[80 to 90 percent below](https://www.sully.ai/help-center/articles/what-is-the-roi-of-an-ai-medical-scribe) the human equivalent \[9\].


[Book a demo](https://www.sully.ai/contact) and bring a real visit. Ideally one that started as a physical and turned into something else.


## FAQ


**Q: What should an AI scribe for primary care do that a general scribe does not?** It has to keep the note billable, not just short. Primary care visits frequently contain two services, a preventive one and a problem-oriented one, and those are separately payable only when the documentation for each stands on its own. A general ambient scribe produces one flowing narrative, which is exactly the blended documentation payers deny. It also has to support E/M level selection on either total time or medical decision making, and carry the required element list for a Medicare Annual Wellness Visit.


**Q: How much time do AI scribes actually save primary care physicians?** Less than the marketing suggests, and it depends heavily on the tool. A randomized trial of 238 outpatient physicians found one scribe cut time-in-note by 9.5 percent, about 41 seconds per note, while another showed no significant change against control \[2\]. Larger operational evaluations report bigger effects: The Permanente Medical Group measured the equivalent of 1,794 working days saved across 7,260 physicians in a year \[6\]. Primary care physicians start from roughly two hours of EHR work per hour of direct patient care \[3\].


**Q: When can you bill a problem visit and a preventive visit on the same day?** When the problem work is significant and separately identifiable from the preventive service. Append modifier 25 to the problem E/M code, 99202 to 99205 for a new patient or 99212 to 99215 for an established one, and document four things for the problem specifically: a chief complaint distinct from the preventive visit, an HPI, a relevant exam, and medical decision making with a treatment plan. Link the problem diagnosis to the E/M line and the preventive diagnosis to the preventive line \[1\].


**Q: Why do Medicare Annual Wellness Visits get denied?** Three reasons. Sequence, since G0438 is once per lifetime and must come before G0439, so a second G0438 auto-denies. Timing, since neither is payable within 365 days of the G0402 initial preventive physical exam. And missing elements, since the health risk assessment and the written personalized prevention plan furnished to the patient are both required, and screenings need the tool and the result recorded rather than a bare conclusion \[4\]\[5\].


**Q: Do you have free primary care note templates?** Yes. Sully publishes three: an office visit note built around E/M level selection, a two-page Medicare Annual Wellness Visit template covering every required element, and a preventive-plus-problem template that lays out the four elements modifier 25 requires. Each has the payer rules printed on the page.


## Sources


\[1\] **Medical Billers and Coders** —[How Does Incorrect Modifier Usage Impact Preventive Care Billing?](https://www.medicalbillersandcoders.com/blog/how-does-incorrect-modifier-usage-impact-preventive-care-billing/) \[2\] **NEJM AI** —[Ambient AI Scribes in Clinical Practice: A Randomized Trial](https://pubmed.ncbi.nlm.nih.gov/41497288/) \[3\] **Annals of Family Medicine** —[Tethered to the EHR: Primary Care Physician Workload Assessment Using EHR Event Log Data and Time-Motion Observations](https://www.annfammed.org/content/15/5/419.short) \[4\] **American Academy of Family Physicians** —[How To Implement And Code Medicare's Annual Wellness Visit](https://www.aafp.org/family-physician/practice-and-career/getting-paid/coding/annual-wellness-visits.html) \[5\] **Centers for Medicare & Medicaid Services** —[Annual Wellness Visit: Social Determinants of Health Risk Assessment](https://www.cms.gov/files/document/mm13486-annual-wellness-visit-social-determinants-health-risk-assessment.pdf) \[6\] **The Permanente Medical Group** —[Analysis: AI Scribes Save Physicians Time, Improve Patient Interactions and Work Satisfaction](https://permanente.org/analysis-ai-scribes-save-physicians-time-improve-patient-interactions-and-work-satisfaction/) \[7\] **JAMIA Open** —[Evaluating the Impact of Artificial Intelligence Scribes on Clinical Documentation in Primary Care: A Simulation Study](https://academic.oup.com/jamiaopen/article/9/4/ooag101/8726639) \[8\] **CMAJ** —[Artificial Intelligence Scribes in Primary Care](https://pmc.ncbi.nlm.nih.gov/articles/PMC11412733/) \[9\] **Sully.ai** —[The AI Workforce for Healthcare](https://www.sully.ai/)
