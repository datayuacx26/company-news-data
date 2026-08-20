---
schema_version: "1.0.0"
document_id: "9bcd21a67dbbcf8e47b22256cc32bb2f6664b4443ca4e264ab2cfc063057fa27"
company_key: "yc-phases"
company: "Phases"
source_id: "yc-phases-news-import-61e19d289ed9"
canonical_url: "https://phases.ai/blog/jevons-paradox-doing-more-with-less-in-clinical-trials"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-23T20:18:02.061944+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:6d49b6b278d2eff0ed7cf8b86e5a66e7fce839d1a45993acb35b2dbe2b622c4f"
---

# Jevons paradox: Doing more with less in clinical trials

## Jevons paradox in monitoring


The Clinical Research Associate (CRA) has a central role in a trial. CRAs visit sites, check protocol compliance, and write monitoring visit reports. Limited capacity forces periodic monitoring, often once every one to two months, with report review sampled. In conversations we have had with 100+ CROs and sponsors, sponsors often reviewed only a small share of monitoring visit reports, around 10%, while CRO teams carried the operational burden. This creates an execution risk: hidden issues can stay buried until they are harder to fix.


A[monitoring AI agent](https://phases.ai/blog/copilot-ceiling-in-clinical-trials) supports CRAs and clinical operations teams by preparing visits, reviewing site documents, and comparing reports against the clinical monitoring plan and protocol. It surfaces findings the report missed. The point is a better execution loop: the agent notices when the same finding reappears visit after visit, when the same issue appears across sites, or when follow-up is missing. Then it recommends the next action while the context is still fresh.


None of this displaces the CRA. It makes the CRA's judgment and relationships more valuable. When routine preparation, document review, report checking, and follow-up tracking run continuously, CRAs can spend more of their time on the work that changes site performance: building trust with coordinators and investigators, understanding why issues keep recurring, and helping sites resolve them before they become trial-wide problems.


## Jevons paradox in data management


A data manager is responsible for turning raw trial data into something a regulator can rely on. They clean data, reconcile it across systems, and manage queries through repeated review-and-response loops with sites and clinical teams. That work is constrained by manual review cycles, query response loops, and database-lock timelines, which is why data cleaning and query resolution are treated as critical path activities before lock.3


A data management agent runs those checks as data lands, reconciling values across Electronic Data Capture (EDC), labs, imaging, and vendor datasets for 100% of the data. It raises queries when inconsistencies or anomalies are identified. Unlike static edit checks, the agent uses reasoning to catch cross-domain problems those checks miss, such as a lab value that sits in range but matches a known drug toxicity with no recorded adverse event, or a dose change with no documented reason.


These findings are pushed to a reviewer or into the EDC as a query while the entry is still fresh. This turns data management into a continuous quality loop. Data managers still own judgment calls, but the agent keeps every record under review, bringing exceptions forward while they are still easy to resolve.


Continuous cleaning across 100% of study data preserves the data manager's role in overseeing quality, deciding issue resolution, and coordinating with sites when judgment is required.


## Jevons paradox in the TMF


The TMF is the regulatory record of the study. ICH GCP treats these documents as a contemporaneous record, which only holds if the file stays current as the study runs.4 The EMA's TMF guideline expects a complete and available file on request throughout the trial,5 and the FDA's bioresearch monitoring program assumes a sponsor can produce records and[audit trails on demand](https://phases.ai/blog/part-11-never-written-for-ai-agents) .6


This lack of continuous completeness and inspection-readiness is exactly what a TMF agent changes. This is not just the repository workflow that Veeva and other eTMF systems already support. The agent acts on the live state of the file: it identifies missing artifacts against the expected document list, checks incoming documents against ALCOA principles, flags stale or misfiled records, detects duplicates, and pushes the next action to the person who can close the gap. Instead of needing to dedicate large resources to TMF quality or panic before inspections, study teams get continuous audit readiness and visibility at a fraction of today's operational cost.


For TMF managers, this is the Jevons paradox effect in practice: the job shifts from periodic rescue work before inspections to owning the control layer that runs continuously in the background. The team still owns judgment, but the baseline work of finding gaps, chasing documents, checking quality, and showing readiness runs across 100% of the TMF without requiring the team to work more hours.


## Thank you, Mr. Jevons


Much like more efficient steam engines increased the use of steam power, AI will push the marginal cost of doing important execution work in a clinical trial close to zero. Execution quality stops being something a team trades off against budget and timeline and becomes a standard part of how every study runs.


This is the future we are building toward at Phases. Trials already produce almost everything a team needs to catch problems early, but the binding constraint has always been the review overhead required to act on it. Jevons paradox removes that constraint.


The next decade of clinical trial improvement will come from doing more of the execution work that matters, continuously, at a cost that makes complete review and follow-through the default state.


---


## References


1.


William Stanley Jevons. "The Coal Question." 1865 (Econlib edition).[https://www.econlib.org/library/YPDBooks/Jevons/jvnCQ.html](https://www.econlib.org/library/YPDBooks/Jevons/jvnCQ.html) ; Northeastern Global News. "How a 160-Year-Old Economic Paradox Could Predict AI's Future." February 2025.[https://news.northeastern.edu/2025/02/07/jevons-paradox-ai-future/](https://news.northeastern.edu/2025/02/07/jevons-paradox-ai-future/)↩


2.


TransCelerate BioPharma. "Position Paper: Risk-Based Monitoring Methodology." 2013.[https://www.transceleratebiopharmainc.com/wp-content/uploads/2016/01/TransCelerate-RBM-Position-Paper-FINAL-30MAY2013.pdf.pdf](https://www.transceleratebiopharmainc.com/wp-content/uploads/2016/01/TransCelerate-RBM-Position-Paper-FINAL-30MAY2013.pdf.pdf)↩


3.


CurexBio. "The Future of Clinical Data Management: Automation, AI, and Real-Time Data."[https://curexbio.com/the-future-of-clinical-data-management-automation-ai-and-real-time-data/](https://curexbio.com/the-future-of-clinical-data-management-automation-ai-and-real-time-data/) ; Society for Clinical Data Management / Journal of the Society for Clinical Data Management. "Data cleaning and query management in clinical trials."[https://www.jscdm.org/article/id/20/](https://www.jscdm.org/article/id/20/)↩


4.


ICH GCP. "Essential Documents for the Conduct of a Clinical Trial."[https://ichgcp.net/8-essential-documents-for-the-conduct-of-a-clinical-trial/](https://ichgcp.net/8-essential-documents-for-the-conduct-of-a-clinical-trial/)↩


5.


European Medicines Agency. "Guideline on the content, management and archiving of the clinical trial master file (paper and/or electronic)." EMA/INS/GCP/856758/2018.[https://www.ema.europa.eu/en/documents/scientific-guideline/guideline-content-management-and-archiving-clinical-trial-master-file-paper-andor-electronic_en.pdf](https://www.ema.europa.eu/en/documents/scientific-guideline/guideline-content-management-and-archiving-clinical-trial-master-file-paper-andor-electronic_en.pdf)↩


6.


U.S. Food and Drug Administration. "Bioresearch Monitoring Program (BIMO) Information."[https://www.fda.gov/inspections-compliance-enforcement-and-criminal-investigations/fda-bioresearch-monitoring-information/bioresearch-monitoring-program-information](https://www.fda.gov/inspections-compliance-enforcement-and-criminal-investigations/fda-bioresearch-monitoring-information/bioresearch-monitoring-program-information)↩
