---
schema_version: "1.0.0"
document_id: "11365d9a44e093693371fa3eb87fa4254cab4682dfdada29dd76e7fbef623bb6"
company_key: "yc-segmed"
company: "Segmed"
source_id: "yc-segmed-news-import-4f813eeed56d"
canonical_url: "https://www.segmed.ai/resources/blog/fda-real-world-evidence-guidance-what-imaging-data-sponsors-need"
published_at: null
first_seen_at: "2026-08-18T21:29:54.153526+00:00"
fetched_at: "2026-08-18T21:29:56.196502+00:00"
content_hash: "sha256:7bd6b4554b8f85347e6f87a1b284d7689e275745ac0aa22977a646756c7cff59"
---

# FDA Real-World Evidence Guidance: What Imaging Data Sponsors Need

**TL;DR**


- FDA's final guidance on real-world evidence for medical devices took effect February 17, 2026.


- It replaces the 2017 guidance and applies to devices only, not drugs or biologics.


- Two questions decide whether your data can be used for FDA purposes: are they relevant, and are they reliable?


- Imaging databases are on FDA's list of acceptable real-world data sources.


- Training and monitoring AI-enabled devices is a recognized use of real-world data.


- Most of the work happens before results exist. You document where the data came from and how they were handled.


- eSTAR now has a dedicated real-world data section you must complete.


‍


---


‍


## Introduction


If you are building an AI-enabled imaging device, you have probably already asked whether real-world imaging data can support your submission. FDA's answer is yes. The harder question is which imaging datasets survive regulatory scrutiny, and what you have to prove about them.


FDA published the final version of *Use of Real-World Evidence to Support Regulatory Decision-Making for Medical Devices* on December 18, 2025, replacing the 2017 guidance of the same name. This article walks through what changed, what it means in practice, and what to ask a data partner before you commit.


‍


## The update was mandated, and it is already in force


Congress required this revision. Section 3629 of the Food and Drug Omnibus Reform Act of 2022 directs FDA to update its guidance on real-world data and evidence, and FDA committed to the same update under the MDUFA V performance goals. So the document is less a change of heart than a scheduled reckoning with eight years of accumulated experience.


Publication came with a 60-day transition period. At its February 18, 2026 town hall, FDA confirmed the recommendations took effect on February 17, 2026, and expects submissions after that date to include the newly recommended information. Anything submitted earlier will still be reviewed.


None of it is binding, technically. FDA guidance describes the Agency's current thinking, and "should" means recommended rather than required. You may take another approach if it satisfies the applicable statutes and regulations. That distinction matters less than it sounds, though, because this document tells you exactly what reviewers will be looking for.


‍


## The scope is devices, and it is wide


Drugs and biologics are excluded, and the guidance points those sponsors to the separate CBER and CDER program. Within devices, almost nothing is left out. IDEs, 510(k)s, PMAs, HDEs, De Novo requests, post-approval studies, postmarket surveillance responses, CLIA Waiver applications and Duals all fall inside.


What the guidance sets aside is narrower: non-clinical data, adverse event reports, systematic literature reviews, and the reuse of data from traditional clinical studies. If your evidence comes from any of those, look elsewhere.


### ‍


## Everything reduces to relevance and reliability


The MDUFA V commitment letter asked FDA to explain what "fit-for-purpose" real-world data means. FDA answered by splitting the phrase into two words, and that split organizes the entire document. You write a relevance and reliability assessment, FDA reviews it, and every other requirement sits underneath one of those two headings.


‍


#### **Evidence snapshot**


**‍** Under 21 CFR 860.7(c)(1), FDA relies only on valid scientific evidence when judging safety and effectiveness. Real-world evidence built from relevant and reliable data can meet that standard. Whether it does depend on your study question, design, data sources, and the regulatory decision at stake.


Relevance asks whether the data can answer your question at all. Four things settle it. Availability comes first: the data need to capture device exposure, outcomes, covariates and enough time per patient, and FDA asks specifically whether your source captures the device identifier portion of the unique device identification system, since that reduces the risk of misclassifying exposure.


Where one source falls short, linkage to another may fill the gap. Timeliness then asks whether the data still describe how medicine is practiced today, because staging criteria and diagnostic definitions shift, and data collected before a major change may no longer reflect the current clinical environment. Generalizability closes the set by asking whether your sample resembles the US population the device is intended for.


Reliability is the harder half, and where most datasets come undone. You have to explain how the data were captured, what happened to them afterward, and what protected them from error along the way. In practice that means documenting the point of capture, every transformation including anything done for privacy, the data dictionary, version control, and the timing of transfers between systems. Missingness runs through all of it. Assess it for each data element, decide in advance how much is too much, and where a variable is badly incomplete, quantify the resulting bias and carry that into your interpretation.


‍


## Imaging sits squarely inside the guidance


Four points land directly on imaging teams, and together they are more encouraging than most sponsors expect.


Imaging databases are explicitly recognized. When FDA lists what can serve as a real-world data source, medical device data repositories appear, with imaging databases given as the example. AI development is recognized too: training an AI-enabled device and monitoring it across its life cycle are both named as legitimate purposes, as is developing the algorithms inside a device.


The trade-off arrives with machine learning. Use it to define study criteria and you owe FDA a detailed account of the model, covering training, tuning and testing, along with the demographic composition of the data behind it and evidence that the approach was verified and validated.


The last point is a trap worth knowing about. Patients visit more than one hospital, so pooling de-identified records across many sites can make a single person appear twice and inflate your counts. FDA flags this directly, and expects your linkage method to be defined in advance, scientifically valid and privacy-preserving, naming privacy preserving record linkage as one example.


‍


### Where your data trail actually starts


One concept is easy to skim past and expensive to get wrong. Your first instance is the data as they first reached you. Receive raw EHR extracts and that is the point of capture; receive a curated file from a vendor and the curated file is your first instance, even though real data existed upstream. From that point forward, documenting quality is your responsibility, and where the trail begins before you, you are expected to obtain as much audit history from the data holder as you can.


FDA is realistic about the limits. Some sources will never hand over everything, and the Agency does not discourage using them. It does warn that uncertainty grows when it cannot see the underlying processes, and it asks you to make the case for reliability regardless. Participant-level data follow the same logic: try to obtain it, and if you cannot, explain what the gap does and does not change about your evidence.


Assessment area What you document


Data availability Device exposure, outcomes, covariates, longitudinally, continuity of care


Linkages Predefined method, line-level accuracy, privacy protection, duplicate correction


Timeliness Time from collection to research release; update schedule


Generalizability How the sample maps to the US intended use population


Data accrual Point of capture, transformations, data dictionary, version control, transfer timing


Quality and integrity Missingness thresholds, cross-site consistency, audit trail, sample size justification


## ‍


## The guidance tells you where each piece belongs


This is the practical payoff, and it removes a lot of guesswork. Flag real-world data in the cover letter, naming the source, provider, version number, and extraction date and range, which lets reviewers triage the submission and pull in the right specialists early.


The protocol carries your conceptual and operational definitions, the causal diagram, data management and quality control plans, the deidentification plan and human subject protections, and it should be finalized before you look at outcome data. The report then carries the relevance and reliability assessment itself, along with any protocol deviations and why they did not compromise the result.


eSTAR changed to match. Marketing submissions now include a real-world data section, and answering yes to the first question makes the rest mandatory, asking where in your submission each element sits. The pre-STAR for IDEs has a similar section, and Q-submissions gained a real-world data category. The documents themselves still attach under existing questions, such as the clinical section or cover letter.


Appendix A of the guidance is the closest thing to a checklist available, mapping each element to where it belongs. FDA is careful to call it neither mandatory nor exclusive, and the tables are not submitted.


‍


### How Segmed fits


Segmed provides real-world, de-identified clinical imaging data for AI and medical device development. The requirements above are what separate a dataset you can develop on from one you can submit on.


Before committing to an imaging data source, ask about provenance documentation, deidentification methodology, linkage approach, consistency across contributing sites, and how the cohort's demographics compare to your intended use population. If you are scoping imaging data for a device submission, talk to our team about what your relevance and reliability documentation would need to cover.


‍


## F.A.Q


### **Does this apply to drug and biologic sponsors?**


No. The guidance covers medical devices only. Drug and biologic sponsors should refer to the separate CBER and CDER real-world evidence program.


### **When did it take effect?**


February 17, 2026, after a 60-day transition period following publication on December 18, 2025.


### **Can real-world evidence be the primary clinical evidence?**


Yes, in the right circumstances. The guidance describes a real example, generalized, where registry data from outside the United States served as the primary clinical evidence supporting an original PMA.


### **Do I need an IDE to use real-world data?**


It depends on the facts. If the device is used in the normal course of medical practice, and collecting data does not influence treatment decisions, an IDE is likely not required. The guidance works through three examples.


### **Do imaging databases qualify as a real-world data source?**


Yes. Medical device data repositories are on FDA's list, with imaging databases named as an example.


### **What if machine learning defines my study criteria?**


You describe the model specifications covering training, tuning, and testing, the demographic composition of the underlying data, and how the approach was verified and validated.


### **Is the guidance legally binding?**


No. FDA guidance reflects the Agency's current thinking and does not create enforceable obligations. An alternative approach is acceptable if it satisfies the applicable statutes and regulations.


### References


U.S. Food and Drug Administration. Use of Real-World Evidence to Support Regulatory Decision-Making for Medical Devices: Guidance for Industry and Food and Drug Administration Staff. Silver Spring, MD: FDA; December 18, 2025. Docket No. FDA-2023-D-4395.[https://www.fda.gov/regulatory-information/search-fda-guidance-documents/use-real-world-evidence-support-regulatory-decision-making-medical-devices](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/use-real-world-evidence-support-regulatory-decision-making-medical-devices)


U.S. Food and Drug Administration. CDRH Town Hall: Use of Real-World Evidence to Support Regulatory Decision-Making for Medical Devices, Final Guidance.
[CDRH Town Hall Use of Real-World Evidence to Support Regulatory Decision-Making for Medical Devices - YouTube](https://www.youtube.com/watch?v=-WJv6Z2mn4A)


‍


### Related Resources


[Access Segmed high-quality, regulatory-grade de-identified imaging data at scale for Foundation Models in days](https://segmed.ai/solutions/foundation-models)


[Unlock the power of real-world imaging data through our proprietary, self-service platform, Openda](https://segmed.ai/solutions/openda)


[Get to know pre-built cohorts with PRISM, the only real-world data platform rooted in imaging, featuring pre-built, multimodal, full-patient cohorts](https://segmed.ai/solutions/rwid-multimodal)


‍


‍
