---
schema_version: "1.0.0"
document_id: "827cf863394d26ea7772ac92fd13b58232f2a9da06ddc269e2c18a13d43a4afe"
company_key: "yc-serif-health"
company: "Serif Health"
source_id: "yc-serif-health-news-import-98e88b19e297"
canonical_url: "https://www.serifhealth.com/blog/payer-name-matching-price-transparency"
published_at: "2026-02-03T02:45:00+00:00"
first_seen_at: "2026-07-24T01:12:39.675945+00:00"
fetched_at: "2026-07-28T22:22:21.045319+00:00"
content_hash: "sha256:abcbc57ff1b3715ac539c6842811520a0b990e0e01028eab745d552fe0e95df6"
---

# Making Sense of Healthcare Data: How We Solved the Plan Name Matching Problem

Healthcare transparency laws have given us access to incredible amounts of pricing data. But having the data and making it useful are two different challenges entirely.


At Serif Health, we've been working on a problem that might seem simple at first glance: when a hospital reports in their transparency postings that they have a contract with "phcsnational/multiplan" for "primarypponetworks(phcs)", how do we map this to the transparency postings provided by MultiPlan to connect and cross-compare the data? Hospital files and payer files describe the same codes and payment arrangements, but they use completely different naming conventions for their plans.


## **The Real Challenge**


Hospital systems report payer and plan information in their own ways. Some use internal codes or IDs provided by their Revenue Cycle Management system, others use abbreviated names, and many use catch-all categories like "ALL OTHERS" or simply "COMMERCIAL." In our data alone, we've identified roughly 170,000 distinct payer and plan combinations. Meanwhile, our payer database contains 553 standardized plan names derived from 607 distinct plan_name strings found across all table of contents files, names like "Aetna Open Access Managed Choice" or "UnitedHealthcare Select Plus." These two string sets rarely align.


This mismatch creates problems for several critical use cases. If you want to create an agreement verification column that confirms when negotiated rates align across both datasets within a tolerance range like 5%, you need to ensure you are cross comparing rates for the correct plan in both assets. Cross-filling or extending data in one asset by calculating off the other requires similar precision.


## **Defining the Problem**


We considered different scenarios and identified a pattern that would suit most of them. Looking at a single value like "PPO" as the network name from the hospital side wouldn't give us much. But looking at the raw plan name, the raw payer name, and the location where the hospital file comes from, altogether, allows us to identify the bigger picture for each plan name based on the supporting records.


A few principles emerged:


**Context matters.** The same plan name can mean different things depending on the state, the payer, or the type of arrangement.


**Multi-value results are real.** Going back to the "COMMERCIAL" and "ALL OTHERS" examples from earlier, trying to map these catch-all terms to a single plan seems wrong. Sometimes the honest answer is "this could be several different networks." Rather than forcing a single answer, we designed our system to return multiple possibilities when the input is genuinely ambiguous, and return a single match when the mapping is clear.


**Patterns evolve.** Payers change their naming conventions, merge with other companies, and launch new products. Our system needs to adapt without requiring constant manual updates.


## **Our Solution: Three Approaches Working Together**


Rather than trying to solve this with a single method, we built a system that uses three different approaches.


### **Non-Commercial Filtering**


Since our target data (Transparency in Coverage postings) only contains commercial plans, we start by filtering out anything that is not commercial. Medicare, Medicaid, workers' compensation, and other non-commercial arrangements don't belong in our commercial network taxonomy.


### **Pattern Recognition First**


We identified the most common naming patterns by comparing results between two different algorithms. When they agree, we consider the match correct. Blue Cross Blue Shield appears in dozens of state-specific variations. UnitedHealthcare has consistent patterns for their major product lines. Kaiser uses different naming conventions for Northern versus Southern California.


Our pattern engine handles the straightforward cases, and there are many of them. This part of the model covers about 40% of the data, reducing costs while keeping the system efficient with an average processing time of 0.014 seconds per value.


### **Language Models for Complex Cases**


For the trickier cases (about 60% of the data), we use a large language model trained on healthcare payer data. This handles situations like ambiguous plan names that could map to multiple networks, regional distinctions that aren't obvious from the name alone, employer group arrangements that need special handling, and out-of-network indicators that affect network classification.


The language model processes batches of hospital records and provides both a network classification and a confidence score. If it doesn't find a strong classification, it returns a null value with 0% confidence, preferring accuracy over coverage.


### **Classification Pipeline Architecture**


## **The Results**


Our classification system achieves 95% accuracy and 78% coverage over a balanced test set of thousands of rows spanning all 553 standardized plan names, a sufficient sample size for the approximately 100K unique rows containing all the different variations of the plan names in the hospital files. By building this approach into our ingest automation pipelines, we can now process hospital transparency files automatically.


This automation delivers three key advantages. First, we process vastly more data than manual review would allow - delivering results in hours instead of weeks. Second, the standardized approach ensures consistent accuracy across all records, eliminating the variability and fatigue inherent in manual classification. Third, as hospitals release updated files monthly, our system continuously ingests and classifies new data without additional human effort, providing confidence scores and flagging cases that need review.


This standardized, high-quality output enables us to develop data verification features and crosswalk between hospital and payer price transparency assets - an ability demanded by our customers. More importantly, it gives us the foundation for building more sophisticated analytics as the healthcare transparency data landscape continues evolving.


## **Looking Forward**


Solving the payer name matching problem was just the first step. With reliable mappings between the two datasets, we can now tackle agreement verification at scale, automatically flagging when negotiated rates diverge between hospital and payer files. Over time, this crosswalk will enable us to cross-fill missing data by extending information from one transparency asset to the other. These capabilities move us closer to data transparency that delivers answers instead of generating more questions.


Price transparency requirements are driving the healthcare industry to generate more data than ever before. Our job is making sure that data actually helps people understand what healthcare costs and why.


Interested in learning more about our approach to healthcare data challenges? Check out our[technical documentation](https://www.serifhealth.com/blog) or reach out to our team!


[https://www.serifhealth.com/blog](https://www.serifhealth.com/blog)


[https://www.serifhealth.com/solutions/](https://www.serifhealth.com/solutions/)
