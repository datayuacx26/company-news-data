---
schema_version: "1.0.0"
document_id: "961cd7b41fba186b024606761922905f9a2c75a858e587f5d9b26c0745a7fa6c"
company_key: "yc-encord"
company: "Encord"
source_id: "yc-encord-news-import-59af355da1b0"
canonical_url: "https://encord.com/blog/fda-pccp-rule/"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-21T18:06:14.355617+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:a67bd72a5ff23dcdf2c24403eecc351f67e85185f1830096d5e18b1dc3d4e25f"
---

# FDA's New Predetermined Change Control Plan Rule for Radiological AI Software, Explained

# FDA's New Predetermined Change Control Plan Rule for Radiological AI Software, Explained


[Tom Inglis](https://encord.com/author/tom-inglis/)


MLOps Regulated Industries at Encord


July 17, 2026


|


5 min read


Summarize with AI


-
-
-


On 17 June 2026 the FDA codified a brand new device category and wrote it into federal regulation as[21 CFR 892.2055](https://www.law.cornell.edu/cfr/text/21/892.2055) . It is called *radiological machine learning-based quantitative imaging software with a[predetermined change control plan (PCCP)](https://www.gov.uk/government/publications/predetermined-change-control-plans-for-machine-learning-enabled-medical-devices-guiding-principles/predetermined-change-control-plans-for-machine-learning-enabled-medical-devices-guiding-principles)* , and it sits in Class II with special controls.


## What Did the FDA's New PCCP Classification Actually Do?


- **It created a named category.** Previously there was no codified device type for this kind of software. Now there is, along with a defined set of requirements to meet.
- **It set the risk level at Class II.** Reserved for devices where general controls are not enough on their own but, in the FDA's words, "there is sufficient information to establish special controls that, in combination with the general controls, provide reasonable assurance of the safety and effectiveness of the device."
- **It kept the device under review.** This is not an exemption. The device "is therefore subject to premarket notification requirements under section[510(k) of the FD&C Act](https://www.fda.gov/medical-devices/premarket-submissions-selecting-and-preparing-correct-submission/premarket-notification-510k) ."


## From Class III to Class II: What Changed for AI Imaging Software


The change is a trade: an easier route to market, in exchange for keeping your documentation current over time.


**The old way.** Anything new counted as high risk by default. That meant Class III and premarket approval, the longest, most expensive route onto the market, no matter how modest the real risk.


**The new way.** This rule puts the device type into Class II instead. Lighter path, and it becomes a predicate, so the next company can follow through the quicker 510(k) route rather than starting over.


null Before After


Default class Class III Class II


Route to market Premarket approval 510(k)


Usable by others as a predicate No Yes


## What Is a Predetermined Change Control Plan (PCCP)?


- Before, you paid to change your model at the door: every update meant going back to the FDA with fresh evidence.
- With a PCCP, you agree on the likely changes and how you will validate them up front, then ship updates within that plan without filing again.
- The trade-off: documentation becomes a running job. Annotation methods, ontologies, cohorts and performance results have to stay current and on the record, version after version.


For teams building these tools, that is the moment data governance stops being background admin and becomes part of staying cleared.


## Which products and devices does this cover?


A device fits the new category if it is:


- [Software only](https://www.fda.gov/medical-devices/device-software-functions-including-mobile-medical-applications/examples-device-software-functions-fda-regulates) , no hardware.
- Working on **radiological images** .
- Producing **quantitative outputs** , actual measurements, not just a yes/no flag.
- Built around functions like **view selection, segmentation and landmarking** .
- Designed to **evolve under a PCCP** .


The example that triggered the rule is a cardiac tool that automates an ejection fraction measurement, but the wording is broad enough to cover measurement and segmentation software across radiology more widely.


**What it is not:** detection-only tools that just flag a finding, or static models with no plan to change.


## FDA Special Controls Are Really Data Governance Requirements


Strip away the regulatory language and the[Class II special controls](https://www.fda.gov/medical-devices/guidance-documents-medical-devices-and-radiation-emitting-products/class-ii-special-controls-documents) read like a spec for a data pipeline. What the rule asks for:


- **Training data, documented.** A description of the data, the annotation methods, and the cohorts within it (demographics, confounders, image acquisition).
- **A clean test set.** Independent from training data, with enough cases per cohort to prove performance by subgroup, not just on average.
- **Hard metrics.** Named yardsticks like Dice score, Hausdorff distance, sensitivity and specificity, measured against ground truth.
- **Labelling that shows the limits.** Performance broken down by subgroup, with confidence intervals, plus where the device is likely to fail.


Special control What it requires from your data


Describe training data, annotation methods, cohorts Documented labelling process and defined subgroups


Independent test set with enough cases per cohort Traceable provenance and controlled data splits


Objective performance measures Ground-truth labels and measurable quality metrics


Labelling with sub-analyses and failure modes Cohort-level evidence and edge-case coverage


Validated planned modifications (PCCP) A repeatable data-and-validation loop per update


**The takeaway:** modelling alone will not get you there. Every label, cohort, dataset split and metric needs a traceable history behind it, showing what changed, when, and why, because that record is exactly what you have to stand behind.


## How Encord Supports FDA PCCP Compliance for Imaging AI


The evidence the FDA needs lives in your data, and it has to be produced, organised, and kept traceable across every model version.


Mapped to the special controls, an **FDA compliance data pipeline** built on Encord covers:


- **Native[DICOM and NIfTI](https://encord.com/dicom/) support** for the imaging modalities the category covers, so training and test data can be ingested, viewed, and versioned in the formats radiologists and reviewers already work in.
- **Annotation and ontology tooling built for annotation methods for FDA submission.** Encord's ontology structure documents label definitions, class hierarchies, and annotator instructions as a standing record.
- **DICOM[curation and indexing](https://encord.com/curation/)** to define, slice, and audit the cohorts and subsets performance must be reported against so subgroup performance claims in your 510(k) are backed by a queryable dataset.
- **Review workflows and annotator-agreement metrics** that produce documented evidence of label quality, giving you inter-annotator agreement, review history, and consensus data as **training data documentation for medical AI** models, the evidence special control (1) and (4) ask for.
- **Support for the recurring[labelling and validation loop](https://encord.com/post-training-alignment/) a PCCP creates** , so each planned modification comes with its own validation package, generated from the same pipeline rather than rebuilt from scratch every time the model changes.


What it does is make the special controls part of your existing process. Documented data, cohorts and performance measures stop being a submission-time burden and become a byproduct of how the team already works. For a fuller walk-through of the approval process itself, see[Encord's guide to getting AI models through the FDA.](https://encord.com/blog/ai-algorithm-fda-approval/)


## Key takeaways


- The FDA has created a new Class II device type for radiological ML quantitative imaging software with a PCCP, codified at 21 CFR 892.2055.
- Devices like this previously defaulted to Class III and premarket approval. This rule opens the lighter 510(k) route.
- The classification acts as a predicate, so other manufacturers can build on it rather than starting from scratch.
- The predetermined change control plan is now part of the device's codified identity, and planned updates carry their own risk and validation burden.
- The special controls are, in substance, data requirements: documented annotation, defined cohorts, an independent test set, and measurable performance.


[< Previous Complete Guide to LiDAR and Point Cloud Annotation for Autonomous Systems](https://encord.com/blog/complete-guide-to-lidar-and-point-cloud-annotation-for-autonomous-systems/)[Next > The Eval Stack the Top AI Teams Are Building Right Now \[Webinar Recap\]](https://encord.com/blog/eval-stack-webinar-recap/)


## Frequently asked questions


-


No. The rule states the device "is therefore subject to premarket notification requirements under section 510(k) of the FD&C Act". It is a lighter pathway, not an exemption.


-


No.The category is defined broadly as software that "employs machine learning algorithms on radiological images to provide quantitative imaging outputs".


-


It is a pre-agreed plan describing the modifications a device will undergo and the methodology used to validate them, so the device can be updated without a new submission each time.


-


yes in practice, since staying within an authorised PCCP means keeping annotation methods, ontologies, cohorts and performance evidence current and traceable across every model version.


-


This order is effective June 17, 2026.


## Get the data right.


300+ of the best AI teams in the world use Encord.


[Take a tour](https://encord.com/explore-product/)[Book a demo](https://encord.com/book-demo/)
