---
schema_version: "1.0.0"
document_id: "0e4bcc95e629c4782f2b07b78296e3cbf192707db0cb87fc3256d989fcb6b0ce"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/automated-visual-inspection-validation/"
published_at: "2026-08-14T11:40:00+00:00"
first_seen_at: "2026-08-18T17:48:33.547197+00:00"
fetched_at: "2026-08-18T17:48:34.683897+00:00"
content_hash: "sha256:c889e023cafb161fa63baa4c529267b6413e89fa4e1e8a31e30d2631ca1233c2"
---

# Moving from Manual Inspection to Machine Vision: A Regulatory Field Guide for Medical Device Quality Teams

[Grant Nelson](https://blog.roboflow.com/author/grant/)


Published Aug 14, 2026 • 8 min read


> **Not legal advice.** This is general educational information, not regulatory or legal advice.


On February 2, 2026, the FDA's Quality System Regulation (QSR) was replaced by the Quality Management System Regulation (QMSR), which incorporates ISO 13485:2016 by reference (89 FR 7496, published February 2, 2024).


This change brings two core updates to computer vision and quality teams at[medical device manufacturers](https://roboflow.com/ai/medical-device-manufacturing?ref=blog.roboflow.com) :


1. **The inspection method changed.** The Quality System Inspection Technique (QSIT) has been retired in favor of a QMSR-aligned approach.
2. **Internal inspection records are more visible to the FDA.** This is the major landmine for teams - a class of internal documents are now subject to FDA review, causing teams to re-evaluate whether their inspection systems are in need of an upgrade. The legacy 820.180(c) exemption that kept management-review minutes, internal audit reports, and supplier audit reports out of routine FDA review has been removed. Under QMSR, FDA can now inspect those records (via 820.35). Many teams have underestimated this: records once written for internal eyes only are now fair game at inspection.


In light of this, and advances in the vision AI industry, many teams are thinking about replacing a manual visual inspection step with an automated, camera-based[machine vision](https://roboflow.com/industry/machine-vision?ref=blog.roboflow.com) one.


## Adopting Automated Computer Vision Inspection Doesn't Change Documentation Requirements Much


A Class II device[manufacturer](https://roboflow.com/industries/manufacturing?ref=blog.roboflow.com) (think fusion pumps, motorized wheelchairs,[contact lenses](https://blog.roboflow.com/contact-lens-defect-inspection/) ) swapping manual inspection for[automated vision inspection](https://blog.roboflow.com/vision-inspection-systems/) will usually handle the change as a retained, inspection-ready "Letter to File", not a new 510(k) submission, as long as the change touches only the manufacturing/[QA process](https://roboflow.com/ai/quality-control?ref=blog.roboflow.com) and not the device's design, materials, intended use, or indications for use.


That distinction comes from 21 CFR 807.81(a)(3) and the FDA's October 2017 guidance on "Deciding When to Submit a 510(k) for a Change to an Existing Device." If the finished device is unchanged and only *how you inspect it* is different, the burden shifts from *submitting to the agency* to *documenting for the record* .


In other words: **nothing is filed with FDA in advance.** You make the change, you document it thoroughly, and you retain that documentation. FDA may review it *reactively* , during a routine establishment inspection.


That's the good news. The policy goal and the requirements are aligned to encourage companies to improve their inspection coverage and quality by migrating from slow, error-prone manual inspection to highly intelligent automated scaled inspections.


## 510(k) Letter to File Requirements for Inspection Method Changes


The 2017 guidance supersedes the 1997 K97-1 guidance. It walks companies through six decision flowcharts and more than 32 examples. The main question is whether you made the change to significantly improve **safety** or **effectiveness** . If not, the flowcharts ask whether residual risks or new failure modes force a submission to the FDA.


For an inspection-method change alone, the usual conclusion is that no new 510(k) submission is required. You must demonstrate three things:


1. Nothing changes in the device design, materials, chemical composition, energy source, indications, or intended use. Upgrading to use[computer vision](https://roboflow.com/?ref=blog.roboflow.com) for inspection does not trigger a flag here.
2. The new method[detects defects](https://roboflow.com/solutions/defect-detection?ref=blog.roboflow.com) at least as well as the method it replaces, supported by method-comparison data.
3. No new failure modes are introduced that can let defective product reach patients. Unless the inspection coverage somehow decreases by adding computer vision, this is usually easily satisfied.


Your Letter to File records the change description, the rationale, the risk assessment, the safety-and-effectiveness conclusion, and the supporting validation evidence. Explicitly work through the flowcharts published by the FDA.


Two scope notes to keep in mind. The FDA's companion guidance, "Deciding When to Submit a 510(k) for a Software Change to an Existing Device," governs *the medical device* software, not production software. A vision-inspection retrofit generally does not trigger it.


Class III devices (for example[pacemakers](https://roboflow.com/ai/cardiology-device-inspection?ref=blog.roboflow.com) ) live under a much stricter rule: 21 CFR 814.39 requires a PMA supplement for changes affecting safety or effectiveness, and 814.39(d)(2)(iv) names new manufacturing controls and test methods specifically.


## Establish A Baseline


Your Letter to File will claim the new method performs at least as well as the old one. You cannot prove that claim without a documented picture of the old one. Capture this early:


- Current SOPs, work instructions, inspection aids, and written acceptance criteria.
- Historical inspection data: defect rates by category, first-pass yield, sampling records, trend charts.
- Process capability studies for the manual method.
- Any gauge R&R and operator qualification studies for manual gauges.
- Training records for the current inspectors (820.25).
- Historical CAPAs (Corrective and Preventive Action), deviations, and complaints tied to inspection escapes.


Your company should have this data already. If not, implementing a process, perhaps using computer vision immediately to evaluate the baseline is imperative.


## Required Documentation for Method Changes


Think of the change as producing one evidence package. It has a handful of pillars.


### 1. Validate the process (the IQ/OQ/PQ backbone)


Process validation is triggered whenever "the results of a process cannot be fully verified by subsequent inspection and test" (21 CFR 820.75). A vision inspection
that outputs a pass/fail or a measurement you can't practically re-check unit-by-unit with a second independent method is the textbook example. The FDA's own process-validation training has long used automated camera inspection as *the* example.


The accepted framework is IQ/OQ/PQ, drawn from GHTF/SG3/N99-10:2004 (Edition 2):


- **Installation Qualification (IQ):** objective evidence the system is installed to spec:[cameras](https://ai1.roboflow.com/?ref=blog.roboflow.com) , lenses, lighting, sensors, controllers, firmware, and network, with utilities verified, calibrations current, and configuration documented.
- **Operational Qualification (OQ):** characterizing the operating envelope: lighting and contrast thresholds, defect sensitivity and specificity across *worst-case* conditions, repeatability, and a confusion matrix against known-defect samples.
- **Performance Qualification (PQ):** proving consistent performance on *real production units* , across operators, shifts, and lots, against pre-defined acceptance criteria.


In practice, proving that the computer vision system works on the line in real production shifts satisfies the full ladder. Proving that the system works by running it and comparing its assessment against the human operators (or the previous system) is the point.


### 2. Validate the implementation


A vision inspection system is production/quality-system software, not software as a medical device, so it's governed by 21 CFR 820.70(i). The "Computer Software Assurance (CSA) for Production and Quality System Software" guidance is the key authority.


The CSA approach is deliberately risk-based and least-burdensome: you classify each software feature as "high process risk" (a failure could foreseeably compromise safety) or not, and scale your assurance effort accordingly.


CSA explicitly endorses unscripted and ad hoc testing for lower-risk features,
and lets you *leverage vendor-supplied evidence and ISO 13485 supplier certifications* rather than re-deriving everything yourself.


For a vision system, the final accept/reject decision itself is almost certainly "high process risk" so plan on scripted testing, boundary testing against a curated defect library, and rigorous records of intended use, risk classification, issues and resolutions, tester identity, and dates.


Your computer vision system should be able to automatically log samples of defect/no defect for easy review and model retraining. Roboflow customers use our[Vision Events product](https://docs.roboflow.com/deployment/monitoring-and-analytics/vision-events?ref=blog.roboflow.com) , which logs relevant images from cameras and inspection streams to store and review these examples.


### 3. Document the statistical rationale


This one is frequently under-documented and frequently cited. 21 CFR 820.250 is short but firm: *sampling plans, when used, "shall be written and based on a valid statistical rationale,"* and procedures must be reviewed *when changes occur.* An inspection-method change is exactly such a trigger.


Two paths:


- If vision enables 100% inspection, document the migration *away* from sample-based acceptance.
- If you keep sampling, document the new plan with attributes and variables with a written rationale.


Either way, the validation evidence should include the right statistics for the output type:


- Cpk/Ppk for variable (measurement) outputs.
- Gauge R&R for measurement systems.
- Attribute agreement analysis or sensitivity/specificity/ROC
analysis for pass/fail classification outputs.


The keystone is an equivalence (method-comparison) study on paired samples, ideally including *seeded, known defects,* showing the new method is at least as capable as the old one. That study is what ultimately backs the Letter-to-File conclusion that safety and effectiveness are not adversely affected.


For companies migrating to an automated inspection system, the main driver is often both increased output and safety. So they choose to increase their coverage to 100% of samples, which has the benefit of eliminating statistical sample size as a landmine to trip on.


### 4. Log the 820.80 Required Records


Every acceptance activity must produce a record, and that record goes in the device history record. **** 21 CFR 820.80(e) lists the required content:


Element Citation


The acceptance activities performed 820.80(e)(1)


Dates of those activities 820.80(e)(2)


Results 820.80(e)(3)


Signature of the individual conducting the activity 820.80(e)(4)


Equipment used, where appropriate 820.80(e)(5)


Decide early how to map the inspection system's outputs and image logs to these elements: how acceptances are logged, stored, and retrieved. An automated inspection system should make it easy to generate and access and store the inspection records such as detections, images, and conclusions.


### 5. Update the risk file and handle rejects


ISO 14971:2019 requires you to update the device Risk Management File for the new production control. Record the control itself, its residual risk contribution, and the overall residual risk of the device. The 2019 edition treats production information as an active input to risk review.


A vision system will likely be more accurate, resulting in more true positives (rejects) while also reducing false positives (increasing throughput). 21 CFR 820.90 requires procedures for identifying, documenting, evaluating, segregating, and dispositioning nonconforming product. This shouldn't be a new process.


### 6. Run it through change control


21 CFR 820.70(b) requires procedures for changes to a specification, method, process, or procedure, and mandates that such changes be verified or validated before implementation (per 820.75), with documented activities and 820.40 approval.


21 CFR 820.40 (document controls) requires review, approval, controlled distribution, prompt removal of obsolete documents, and change records capturing the description, affected documents, approver signature, approval date, and effective date.


### 7. Don't forget people and suppliers


Training records (21 CFR 820.25) must cover operators, maintenance technicians, engineers, and quality staff with effectiveness verified. Supplier qualification (21 CFR 820.50) must be current for the vision-system vendor.


## Where Teams Actually Get Cited


The FDA Form 483 observations and warning letters in this exact area are remarkably consistent, and all of them are preventable with the package above:


- **Inadequate process validation:** equipment run for years unvalidated, or validation that only checks whether it powers on rather than whether it reliably rejects out-of-spec product.
- **Inadequate software validation:** the memorable failure mode being a vision system that accepts out-of-spec product while the investigator watches.
- **Change-control failures:** missing verification/validation before implementation, missing approvals, uncontrolled distribution of superseded SOPs.
- **Absent statistical rationale:** no documented justification for sample sizes or sampling plans.


**Further reading on how to make CFR Part 11 compliant solutions with Roboflow:**


- [Medical Device Assembly Verification with Vision AI](https://blog.roboflow.com/medical-device-assembly-verification/)
- [Visual Quality Management System: Automate Defect Detection](https://blog.roboflow.com/visual-quality-management-system/)
- [Automated Visual Inspection for Pharmaceuticals](https://blog.roboflow.com/visual-inspection-in-pharmaceuticals/)


### References and Primary Sources


**Regulations**


- [21 CFR Part 820 (QMSR)](https://www.ecfr.gov/current/title-21/chapter-I/subchapter-H/part-820?ref=blog.roboflow.com)
- [21 CFR 807.81 (510(k) change trigger)](https://www.ecfr.gov/current/title-21/chapter-I/subchapter-H/part-807/subpart-E/section-807.81?ref=blog.roboflow.com)
- [21 CFR 820.75 (process validation)](https://www.ecfr.gov/current/title-21/chapter-I/subchapter-H/part-820/subpart-G/section-820.75?ref=blog.roboflow.com)
- [21 CFR 820.80 (acceptance activities)](https://www.ecfr.gov/current/title-21/chapter-I/subchapter-H/part-820/subpart-H/section-820.80?ref=blog.roboflow.com)
- [21 CFR 820.250 (statistical techniques)](https://www.ecfr.gov/current/title-21/chapter-I/subchapter-H/part-820/subpart-O/section-820.250?ref=blog.roboflow.com)


**Rulemaking**


- [QMSR Final Rule, 89 FR 7496 (February 2, 2024)](https://www.federalregister.gov/documents/2024/02/02/2024-01709/medical-devices-quality-system-regulation-amendments?ref=blog.roboflow.com)
- [QMSR Technical Amendments (December 4, 2025)](https://www.federalregister.gov/documents/2025/12/04/2025-21955/medical-devices-quality-management-system-regulation-technical-amendments?ref=blog.roboflow.com)


**FDA guidance**


- ["Deciding When to Submit a 510(k) for a Change to an Existing Device" (October 25, 2017)](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/deciding-when-submit-510k-change-existing-device?ref=blog.roboflow.com)
- ["Computer Software Assurance for Production and Quality System Software" (final September 24, 2025)](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/computer-software-assurance-production-and-quality-system-software?ref=blog.roboflow.com)
- ["General Principles of Software Validation" (January 11, 2002)"](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/general-principles-software-validation?ref=blog.roboflow.com)
- [QMSR FAQ](https://www.fda.gov/medical-devices/quality-management-system-regulation-qmsr/quality-management-system-regulation-frequently-asked-questions?ref=blog.roboflow.com)
- [FDA Inspection Observations data](https://www.fda.gov/inspections-compliance-enforcement-and-criminal-investigations/inspection-references/inspection-observations?ref=blog.roboflow.com)


**Standards**


- [ISO 13485:2016](https://www.iso.org/standard/59752.html?ref=blog.roboflow.com)
- [ISO 14971:2019](https://www.iso.org/standard/72704.html?ref=blog.roboflow.com)
- [GHTF/SG3/N99-10:2004 Ed. 2 (process validation, the IQ/OQ/PQ source)](https://www.imdrf.org/sites/default/files/docs/ghtf/final/sg3/technical-docs/ghtf-sg3-n99-10-2004-qms-process-guidance-04010.pdf?ref=blog.roboflow.com)
- [AAMI TIR102:2019 (21 CFR to ISO 13485 mapping, useful as a gap-analysis tool)](https://store.aami.org/s/store?ref=blog.roboflow.com#/store/browse/detail/a152E000006j5zrQAA)


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Grant Nelson](https://blog.roboflow.com/author/grant/) . (Aug 14, 2026). Moving from Manual Inspection to Machine Vision: A Regulatory Field Guide for Medical Device Quality Teams. Roboflow Blog: https://blog.roboflow.com/automated-visual-inspection-validation/*


### Written by


Grant Nelson


[View more posts](https://blog.roboflow.com/author/grant/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Manufacturing](https://blog.roboflow.com/tag/manufacturing/)
- [Machine Vision](https://blog.roboflow.com/tag/machine-vision/)
