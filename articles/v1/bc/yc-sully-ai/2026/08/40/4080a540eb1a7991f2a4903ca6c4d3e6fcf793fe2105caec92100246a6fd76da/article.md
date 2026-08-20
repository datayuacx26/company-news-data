---
schema_version: "1.0.0"
document_id: "4080a540eb1a7991f2a4903ca6c4d3e6fcf793fe2105caec92100246a6fd76da"
company_key: "yc-sully-ai"
company: "Sully.ai"
source_id: "yc-sully-ai-news-import-101ec319ffc2"
canonical_url: "https://www.sully.ai/blog/ai-scribe-for-pain-management-and-the-second-rulebook-nobody-reads"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-15T05:04:27.970735+00:00"
fetched_at: "2026-08-15T05:04:29.269953+00:00"
content_hash: "sha256:3a256a6acfe94318449640b5fadcfee078bb7a064c9df9a39e5ea33b40866acc"
---

# Best AI Scribe for Pain Management: LCD & Notes Guide 2026

Ask a pain physician what governs their[documentation](https://www.sully.ai/medical-scribe) and you will hear about the CDC opioid guideline. Fair enough. It is the one everybody writes about.


There is a second rulebook, though, and it is the one that decides whether the injection you just performed gets paid.


It is your Medicare Administrative Contractor's Local Coverage Determination, it is far more prescriptive than most clinicians expect, and almost nobody covers it.


> **Key Takeaway:** Pain management runs on two separate rulebooks, and almost every documentation guide covers only one. The 2022 CDC guideline tells you how to prescribe opioids safely: establish goals for pain and function, review the PDMP, offer naloxone, reassess within one to four weeks of any dose change \[1\]. The Medicare Local Coverage Determination tells you what the record must contain for an interventional procedure to be paid, and it is far more specific. It requires that the scales used to measure pain and disability be documented, and it names the acceptable ones including the NRS, VAS, Oswestry Disability Index and PROMIS. For a therapeutic injection it requires documentation of why the patient is not a candidate for radio frequency ablation. Where multiple levels are treated it requires the medical necessity of each to be documented separately. And modifier KX attests that all of it was met \[2\]. Sully.ai's AI Scribe writes into the EHR and the AI Coder carries the note through to the coded claim.


## Pain Management Documentation Answers to Two Different Authorities


Most[specialties](https://www.sully.ai/specialties/orthopedics-scribe) have one set of documentation expectations. Pain management has two, written by different bodies, enforcing different things.


### The CDC Guideline Governs the Prescription


The 2022 CDC Clinical Practice Guideline covers 12 recommendations for outpatients with acute, subacute or chronic pain. Establish treatment goals for pain and function before starting ongoing therapy. Review the state PDMP on initiation and periodically. Offer naloxone as part of risk mitigation. Reassess benefits and risks within one to four weeks of starting or escalating. Consider toxicology testing. Use particular caution with benzodiazepines \[1\].


This is the safety rulebook, and getting it wrong is a clinical and regulatory problem.


### The Local Coverage Determination Governs the Procedure


The LCD is a different document entirely. It is issued by your Medicare Administrative Contractor, it concerns coverage rather than safety, and its failure mode is not a citation. It is a denial.


It is also much more specific than most people realise, and that is where the rest of this article goes.


## Pain Management Has Two Rulebooks


Here is the argument. **Every pain documentation guide worth reading covers the CDC guideline thoroughly. Almost none covers coverage policy at all.** The most thorough guide in this space runs to nearly sixty sections, handles PDMP checks and MME arithmetic and tapering scenarios properly, and contains no mention of a Local Coverage Determination, modifier KX, or the requirement to name the instrument you assessed function with.


That is the expensive half to be missing. Three examples of what is actually in there.


### It Is Not Enough to Assess Function, You Have to Name the Instrument


CMS puts it plainly: **the scales used to assess the measurement of pain and/or disability must be documented in the medical record** \[2\].


And it publishes the list of what counts. For pain intensity: verbal rating scales, the **NRS** and the **VAS** . For function and disability: the **PDAS** , the **Oswestry Disability Index** , the **Oswestry Low Back Pain Disability Questionnaire** , the **Quebec Back Pain Disability Score** , the **Roland Morris** scale, the **Back Pain Functional Scale** , and **PROMIS** profile domains \[2\].


"Severe pain, function significantly limited" is a clinically reasonable sentence and a documentation failure. A reviewer was looking for a named instrument and a score, and prose does not contain one.


### For a Therapeutic Injection You Have to Say Why Not RFA


This is the requirement that surprises people most, and it is a single line in the policy: **documentation of why the patient is not a candidate for RFA must be submitted for therapeutic injection procedures** \[2\].


Not why the injection is indicated. Why radio frequency ablation is not the better option for this specific patient. If that sentence is not in the note, the field a reviewer is looking for is empty.


### Multiple Levels Means Multiple Justifications


Where multiple injections are performed at the same or different spinal levels, the medical necessity of **each** injection has to be clearly documented. Frequent reporting of multiple blocks on the same day can trigger focused medical review \[3\].


One justification covering four levels is one justification, not four.


## The Rest of the LCD, in Plain Terms


None of the following is difficult. It is just specific, and it is invisible if you have never read the policy.


### Legibility, Identification and Signature


Every page of the record must be legible and carry appropriate patient identification including the dates of service. The documentation must include the legible signature of the clinician responsible for the care. And there has to be a signed and dated office visit record or operative report \[2\].


The record must also contain the performing provider's assessment as it relates to the patient's complaint for that visit, relevant history, and the results of pertinent tests \[2\].


### Sedation Has to Be Justified or Left Off


Moderate or deep sedation, general anesthesia and monitored anesthesia care are **not considered medically reasonable and necessary during facet joint injections** . For epidural steroid injections they are described as usually unnecessary or rarely indicated, and in exceptional cases the documentation must clearly establish the need in that specific patient \[3\]\[4\].


### Modifier KX Is an Attestation, Not a Formality


KX means "requirements specified in the medical policy have been met" \[2\].


Appending it is a statement about your record. It is only a true statement if the record actually meets the policy, which is the whole reason the fields above matter.


### One Caveat That Matters More Than the Rest


LCDs are issued by Medicare Administrative Contractors, and they vary. Several contractors publish their own facet joint policies with similar but not identical requirements, and commercial payers set their own criteria on top of that.


So do not treat any single policy, including the one quoted throughout this article, as a national rule. Read your own contractor's. The requirements above are representative and they are the right things to be capturing, but the specifics are local.


## Free Pain Management Note Templates


Three templates covering the visits where documentation decides both safety and payment. Each carries the rules printed on the page.


-


[Chronic pain initial evaluation template](https://www.sully.ai/templates/chronic-pain-initial-evaluation) , two pages, recording pain and function on named instruments, with treatment goals split between pain and function


-


[Opioid management follow-up template](https://www.sully.ai/templates/opioid-management-follow-up) , with PDMP review, toxicology, naloxone and concurrent CNS depressants as explicit fields rather than remembered steps


-


[Spinal injection procedure note template](https://www.sully.ai/templates/spinal-injection-procedure-note) , built to the LCD, including the RFA-candidacy field and the KX attestation


## What the CDC Guideline Actually Asks For


The safety rulebook is well covered elsewhere, so this is short. Two things are worth being precise about.


### Goals for Pain and Goals for Function Are Two Fields


Recommendation 2 asks clinicians to work with patients to establish treatment goals for pain **and** function before starting ongoing opioid therapy for subacute or chronic pain \[1\].


Two goals, two fields. A single line about improving quality of life is neither.


### The 2022 Version Removed the Dose Threshold on Purpose


This one still catches practices out. The 2022 guideline deliberately omits specific dosages from its recommendation statements, precisely to stop them being applied as inflexible rules. It advises avoiding increases above levels likely to yield diminishing returns in benefits relative to risks \[1\].


If your practice is still auditing against a fixed morphine milligram equivalent ceiling, it is auditing against the superseded 2016 framing. Record the total MME per day and the reasoning behind it, not a number treated as a limit.


### Reassess Within One to Four Weeks of Any Change


Recommendation 7: evaluate benefits and risks within one to four weeks of starting opioid therapy for subacute or chronic pain, or of any dosage escalation, and reevaluate regularly after that \[1\].


## How to Evaluate an AI Scribe for a Pain Practice


Four questions, in the order that matters.


### Does It Capture the Instrument Name, Not Just the Score


Ask whether the output records "ODI 42" or "significant functional limitation." Only one of those survives a review.


### Does It Produce a Procedure Note That Survives an LCD


Give it a four-level medial branch block. Check whether the output justifies each level, records the pre-procedure score on a named instrument, and contains a place for the RFA-candidacy statement.


### Does It Track the Risk-Mitigation Steps as Discrete Fields


PDMP, toxicology, naloxone, concurrent CNS depressants. Prose that mentions them is not the same as fields that can be audited or reported.


### Does It Reach the Claim


KX is an attestation and the per-level justification has to reach the claim line. A note that stops at the chart has not helped either.


## Where Sully.ai Fits in a Pain Practice


Unlike endoscopy, an interventional pain procedure is usually performed with the patient awake and the physician narrating, so ambient capture has more to work with here than it does in a sedated procedure suite.


Sully's[AI Scribe](https://www.sully.ai/agents/scribe) captures documentation during and after the visit and writes it into the EHR, on a single integration across[Epic, Cerner, Meditech, Athenahealth and ambulatory and specialty systems](https://www.sully.ai/integrations) . Clinicians can save how they want a note structured, so an instrument table stays a table with a name and a score in it rather than dissolving into a sentence.


The[AI Coder](https://www.sully.ai/agents/medical-coder) then extracts the ICD-10 and CPT codes and submits the claim. That matters here because KX is an assertion about the record, and the level-by-level justification has to survive from the note to the claim line.


The[AI Triage Nurse](https://www.sully.ai/agents/triage-nurse) handles pre-visit intake and interval follow-up tracking. Of every specialty in this series, opioid management is the one where that fits best, because most of the work is monitoring between visits rather than during them.


Sully operates across[5,000+ providers](https://www.sully.ai/customer-stories/:WxcUfLIZH) , has delivered 50M+ hours of AI work, and prices each AI role[80 to 90 percent below](https://www.sully.ai/help-center/articles/what-is-the-roi-of-an-ai-medical-scribe) the human equivalent \[5\].


[Book a demo](https://www.sully.ai/contact) and bring a four-level medial branch block on a patient already taking opioids. That one case exercises both rulebooks at once.


## FAQ


**Q: What should an AI scribe for pain management do that a general scribe does not?** It has to serve two rulebooks at once. The 2022 CDC guideline governs opioid prescribing and asks for goals for pain and function, PDMP review, naloxone offering, and reassessment within one to four weeks of a dose change \[1\]. The Medicare Local Coverage Determination governs whether an interventional procedure gets paid, and it is more prescriptive: it requires the pain and disability scales used to be named, requires documentation of why a patient is not an RFA candidate for therapeutic injections, and requires separate medical necessity for each level treated \[2\]\[3\]. Sully.ai's AI Scribe writes the note into the EHR and hands it to the AI Coder, which submits the coded claim.


**Q: What does Medicare require in a facet joint injection note?** Every page legible and identified with patient information and dates of service, the legible signature of the responsible clinician, and a signed and dated operative report. The record must contain the performing provider's assessment as it relates to the complaint for that visit, relevant history, and pertinent test results. The scales used to measure pain and disability must be documented, from named instruments including the NRS, VAS, PDAS, Oswestry Disability Index, Oswestry Low Back Pain Disability Questionnaire, Quebec Back Pain Disability Score, Roland Morris, Back Pain Functional Scale and PROMIS. For therapeutic injection procedures, documentation of why the patient is not a candidate for radiofrequency ablation must be submitted \[2\].


**Q: Is there still a maximum MME per day?** Not in the 2022 CDC guideline. It deliberately omits specific dosage numbers from its recommendation statements to prevent them being applied as inflexible rules, and instead advises avoiding increases above levels likely to yield diminishing returns relative to risks \[1\]. Record the total MME per day and your reasoning. A practice still auditing against a fixed threshold is working from the superseded 2016 framing.


**Q: Can you use sedation for a facet joint injection?** Medicare guidance states that moderate or deep sedation, general anesthesia and monitored anesthesia care are not considered medically reasonable and necessary during facet joint injections. For epidural steroid injections they are described as usually unnecessary or rarely indicated, and in exceptional cases the documentation must clearly establish the need in that specific patient \[3\]\[4\].


**Q: Do you have free pain management note templates?** Yes. Sully publishes three: a two-page chronic pain initial evaluation, an opioid management follow-up, and a spinal injection procedure note. The evaluation records pain and function on named instruments, the follow-up makes each risk-mitigation step a field, and the procedure note includes the RFA-candidacy requirement and the KX attestation.


## Sources


\[1\] **CDC and MMWR** —[CDC Clinical Practice Guideline for Prescribing Opioids for Pain, United States, 2022](https://pmc.ncbi.nlm.nih.gov/articles/PMC9639433/) \[2\] **Centers for Medicare & Medicaid Services** —[Billing and Coding: Facet Joint Interventions for Pain Management, Article A56670](https://www.cms.gov/medicare-coverage-database/view/article.aspx?articleid=56670) \[3\] **Centers for Medicare & Medicaid Services** —[Facet Joint Interventions for Pain Management, LCD L34892](https://www.cms.gov/medicare-coverage-database/view/lcd.aspx?lcdid=34892&ver=123) \[4\] **Centers for Medicare & Medicaid Services** —[Billing and Coding: Epidural Steroid Injections for Pain Management, Article A58995](https://www.cms.gov/medicare-coverage-database/view/article.aspx?articleId=58995&ver=14) \[5\] **Sully.ai** —[The AI Workforce for Healthcare](https://www.sully.ai/)
