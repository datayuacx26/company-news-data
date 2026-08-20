---
schema_version: "1.0.0"
document_id: "baf7f16a753ff03a93b28c580f9ea2199036ed03b5b09a25cc01fd127ec41e3f"
company_key: "yc-phases"
company: "Phases"
source_id: "yc-phases-news-import-61e19d289ed9"
canonical_url: "https://phases.ai/blog/ai-drug-discovery-clinical-trials"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-24T08:18:43.014149+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:5f23d14c4a80593160ca4e796c018f0345d43e8665c19ec50ce8bc3b7a919ccd"
---

# The thing no one wants to admit about AI drug discovery

Monitoring is also document-heavy and difficult to review consistently at scale. Most sponsors do not have the capacity to review every monitoring visit report. As more monitoring activities are performed remotely, the volume of documentation continues to grow, making consistent review even harder. Reports are often only checked selectively despite containing important signals such as unresolved issues, protocol deviations, inconsistent site performance and retraining needs.12


AI can take a[first pass over every monitoring visit report](https://phases.ai/blog/you-cant-trust-chatgpt-or-claude-to-run-your-clinical-trials) and compare it against the Clinical/Study Monitoring Plan, protocol, and other study documents. It can check whether required sections are present, whether there are any protocol deviations, whether any findings contradict each other, whether follow-ups were properly documented, and whether the visit itself is reflective of the monitoring plan.


The bigger value is that AI can connect signals that are usually fragmented across disparate sources such as visits, sites, CRAs, action items, and study documents. That gives sponsors a more complete view of site and CRA performance, and any risks that are building before they become problematic.


The main idea here is not to replace the CRA or the clinical operations team but rather to give sponsors broader and more consistent oversight coverage across every site.


### Data management


Data management is the process of collecting, cleaning, reconciling, and preparing clinical trial data for analysis and submission. It is the part of the process where raw trial data becomes evidence that sponsors and regulators can rely on.14


The challenge is that data cleaning is slow, with manual review and query generation accounting for up to 30% of data-management effort, and the data cleaning and analysis period between last patient last visit and database lock remaining one of the slowest parts of getting a trial ready for filing.15


Traditional edit checks are good at simple problems such as missing fields or out-of-range values. But many clinical data issues require cross-domain reasoning and context. For instance, a lab value may be consistent with a known drug toxicity, but there may be no matching adverse event or a change in dose may not line up with the recorded reason.14


AI can help surface these issues across various sources such listings and labs. It can also assist with reconciliation across EDC, lab, imaging, safety, and other vendor datasets, helping teams identify discrepancies earlier and build a more complete picture of patient safety and efficacy. The findings can then be passed on to a human reviewer or be pushed into the EDC as a query.16


Much like in monitoring, the goal is to augment rather than replace data managers by giving them better coverage and reducing the manual work needed to find the problems that matter.


## Why hasn't this already happened?


Part of the answer is that the current wave of AI is still new. ChatGPT was released only three and a half years ago,17 and the models only really became good enough for complex, agentic workflow in late 2025.


But the bigger reason is that clinical trials are more complex than traditional software environments as they are regulated, audited, fragmented, and full of handoffs. Sponsors, CROs, sites, labs, vendors, ethics committees, and regulators are all involved in different parts of the process and data sits across disparate sources that were never designed to work cleanly together.18


There is also understandable regulatory caution. Sponsors and CROs do not want black boxes in processes that affect patient safety, data integrity, or inspection readiness. Any AI system needs to be created in such a way to ensure that teams know where an output came from, what evidence supports it, who reviewed it, and what changed because of it. That means adoption will ultimately be slower than in other less regulated industries.19


## The bottom line


The Cambridge vaccine is a real milestone and AI will keep improving the discovery process. But despite all of that, every one of those candidates still has to go through clinical trials, where many of the biggest avoidable losses are operational.1


That's exactly why we are building[Phases](https://phases.ai/) .


Clinical trials already generate the information teams need to identify many problems earlier. The challenge is that there is simply too much of it for any individual or team to review consistently and effectively.


The next decade of progress in drug development will not come only from discovering better candidates. It will come from running better trials.


---


## References


1.


University of Cambridge / ScienceDaily. "AI-designed universal coronavirus vaccine passes first human trial." June 2026.[https://www.sciencedaily.com/releases/2026/06/260605023357.htm](https://www.sciencedaily.com/releases/2026/06/260605023357.htm)↩↩2↩3


2.


Isomorphic Labs. "Isomorphic Labs announces Series B investment round." May 2026.[https://www.isomorphiclabs.com/articles/isomorphic-labs-announces-series-b-investment-round](https://www.isomorphiclabs.com/articles/isomorphic-labs-announces-series-b-investment-round)↩


3.


Fierce Biotech. "Anthropic acquires stealth startup Coefficient Bio in $400M deal." April 2026.[https://www.fiercebiotech.com/biotech/anthropic-acquires-stealth-ai-startup-coefficient-bio-400m-deal](https://www.fiercebiotech.com/biotech/anthropic-acquires-stealth-ai-startup-coefficient-bio-400m-deal)↩


4.


NVIDIA Newsroom. "NVIDIA and Lilly Announce Co-Innovation AI Lab to Reinvent Drug Discovery in the Age of AI." January 2026.[https://investor.nvidia.com/news/press-release-details/2026/NVIDIA-and-Lilly-Announce-Co-Innovation-AI-Lab-to-Reinvent-Drug-Discovery-in-the-Age-of-AI/default.aspx](https://investor.nvidia.com/news/press-release-details/2026/NVIDIA-and-Lilly-Announce-Co-Innovation-AI-Lab-to-Reinvent-Drug-Discovery-in-the-Age-of-AI/default.aspx)↩


5.


PhRMA. "Biopharmaceutical Research & Development: The Process Behind New Medicines."[https://www.readkong.com/page/biopharmaceutical-research-development-the-process-1139510](https://www.readkong.com/page/biopharmaceutical-research-development-the-process-1139510)↩↩2


6.


Applied Clinical Trials. "Tufts CSDD: Cost to Develop New Drug is $2.6B." November 2014.[https://www.appliedclinicaltrialsonline.com/view/tufts-csdd-cost-develop-new-drug-26b](https://www.appliedclinicaltrialsonline.com/view/tufts-csdd-cost-develop-new-drug-26b)↩


7.


Wouters OJ et al. "Use of Clinical Trial Characteristics to Estimate Costs of New Drug Development." JAMA Network Open. 2025.[https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2828689](https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2828689)↩


8.


BIO, Informa Pharma Intelligence, QLS Advisors. "Clinical Development Success Rates and Contributing Factors 2011–2020."[https://go.bio.org/rs/490-EHZ-999/images/ClinicalDevelopmentSuccessRates2011_2020.pdf](https://go.bio.org/rs/490-EHZ-999/images/ClinicalDevelopmentSuccessRates2011_2020.pdf)↩↩2


9.


Norstella. "Why are clinical development success rates falling?" May 2024.[https://www.norstella.com/insight/why-are-clinical-development-success-rates-falling/](https://www.norstella.com/insight/why-are-clinical-development-success-rates-falling/)↩


10.


ICH GCP. "Essential Documents for the Conduct of a Clinical Trial."[https://ichgcp.net/8-essential-documents-for-the-conduct-of-a-clinical-trial/](https://ichgcp.net/8-essential-documents-for-the-conduct-of-a-clinical-trial/)↩


11.


UK MHRA. "Clinical trials for medicines: Good clinical practice inspections."[https://www.gov.uk/guidance/clinical-trials-for-medicines-good-clinical-practice-inspections](https://www.gov.uk/guidance/clinical-trials-for-medicines-good-clinical-practice-inspections)↩


12.


ICH GCP. "Monitoring."[https://ichgcp.net/monitoring](https://ichgcp.net/monitoring)↩↩2


13.


Andersen JR et al. "Impact of monitoring approaches on data quality in clinical trials." British Journal of Clinical Pharmacology. 2023.[https://bpspubs.onlinelibrary.wiley.com/doi/10.1111/bcp.15615](https://bpspubs.onlinelibrary.wiley.com/doi/10.1111/bcp.15615) ; Hines S. "Targeting Source Document Verification." Applied Clinical Trials. 2011.[https://www.appliedclinicaltrialsonline.com/view/targeting-source-document-verification](https://www.appliedclinicaltrialsonline.com/view/targeting-source-document-verification) ; TransCelerate BioPharma. "Position Paper: Risk-Based Monitoring Methodology." 2013.[https://www.transceleratebiopharmainc.com/wp-content/uploads/2016/01/TransCelerate-RBM-Position-Paper-FINAL-30MAY2013.pdf.pdf](https://www.transceleratebiopharmainc.com/wp-content/uploads/2016/01/TransCelerate-RBM-Position-Paper-FINAL-30MAY2013.pdf.pdf)↩


14.


Medidata. "Clinical Data Management: Everything You Need to Know."[https://www.medidata.com/en/life-science-resources/medidata-blog/clinical-data-management/](https://www.medidata.com/en/life-science-resources/medidata-blog/clinical-data-management/)↩↩2


15.


CurexBio. "The Future of Clinical Data Management: Automation, AI, and Real-Time Data."[https://curexbio.com/the-future-of-clinical-data-management-automation-ai-and-real-time-data/](https://curexbio.com/the-future-of-clinical-data-management-automation-ai-and-real-time-data/) ; Society for Clinical Data Management / Journal of the Society for Clinical Data Management. "Data cleaning and query management in clinical trials."[https://www.jscdm.org/article/id/20/](https://www.jscdm.org/article/id/20/)↩


16.


CD Connect. "Data Reconciliation in Clinical Data Management: An Overview."[https://cdconnect.net/data-reconciliation-in-clinical-data-management/](https://cdconnect.net/data-reconciliation-in-clinical-data-management/)↩


17.


OpenAI. "Introducing ChatGPT." November 2022.[https://openai.com/index/chatgpt/](https://openai.com/index/chatgpt/)↩


18.


ICON. "Understanding Roles in a Clinical Trial." 2026.[https://careers.iconplc.com/blogs/2026-2/understanding-roles-in-a-clinical-trial](https://careers.iconplc.com/blogs/2026-2/understanding-roles-in-a-clinical-trial)↩


19.


U.S. Food and Drug Administration. "Considerations for the Use of Artificial Intelligence to Support Regulatory Decision-Making for Drug and Biological Products."[https://www.fda.gov/regulatory-information/search-fda-guidance-documents/considerations-use-artificial-intelligence-support-regulatory-decision-making-drug-and-biological](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/considerations-use-artificial-intelligence-support-regulatory-decision-making-drug-and-biological)↩
