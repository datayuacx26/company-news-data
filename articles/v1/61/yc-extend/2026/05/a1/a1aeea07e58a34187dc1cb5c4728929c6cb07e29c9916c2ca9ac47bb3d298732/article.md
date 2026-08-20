---
schema_version: "1.0.0"
document_id: "a1aeea07e58a34187dc1cb5c4728929c6cb07e29c9916c2ca9ac47bb3d298732"
company_key: "yc-extend"
company: "Extend"
source_id: "yc-extend-news-import-054f4f06cd55"
canonical_url: "https://www.extend.ai/resources/flatiron-case-study"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-24T07:25:34.796069+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:3f3303ca445b9b7e0a7492a0ffcf38a9d2e9a092f3a01325dd255e118a379d2e"
---

# How Flatiron Health scaled document extraction to 100M+ pages with Extend

> We were able to replicate 6 months of work in 2 weeks (!) with Extend. We're now scaling this up across all 5 million people with cancer in our network, truly transforming our work against this disease.
>
>
> **George Ho, Machine Learning Technical Lead**


[Flatiron Health](https://flatiron.com/) has spent more than a decade building one of the most important data platforms in oncology. Their mission: to improve and extend lives by learning from the experience of every person with cancer. Their network covers over 5 million patients, and the document volume runs into the billions of pages.


Flatiron turns messy cancer charts into research-grade data that can be used to understand how treatments work. Close to 50 people across the company work on the unstructured data stack, including 20 to 30 machine learning engineers. Because of their in-house expertise, when a new extraction problem shows up, the default instinct is to build.


## **The hardest document in the chart**


For years, one document type eluded the Flatiron team: next-generation sequencing (NGS) reports. NGS reports show which genetic alterations a tumor carries, making them some of the most valuable documents in the chart for oncology research.


They're also dense and highly variable. A single report can include complex tables, cells split across sections, formats that change by lab vendor, and layout patterns that carry meaning beyond the text. Flatiron's team couldn't get all the biomarker data out of every NGS report at scale. Each mutation in those reports points to a different cancer therapy, and a meaningful share of that data sat locked away.


> Every year I'd think, something is wrong with us that we don't have NGS extraction deployed in production.
>
>
> **Guy Amster, Machine Learning Senior Principal**


## **Build vs Buy**


Flatiron spent 2018 to 2023 trying to build in-house, using a combination of NLP and OCR. Each approach had its own ceiling.


Plain OCR lost too much structure. Traditional NLP missed visual cues. Hard-coded rules were brittle and fell apart when the next report format appeared. AWS Textract, which is Flatiron's main OCR engine, could read some tables, but when it missed one cell, the meaning of the entire table could shift, rendering the output unreliable for clinical decision making.


By 2024, the math had stopped favoring the build. Flatiron made the call to stop building NGS extraction in-house and bring in a vendor.


> Which is more valuable to us, our time or our money? We probably could have built our own solution, but it would have taken us longer, and it probably would have been worse. Building LLM-powered document-processing platforms is just not something where we think Flatiron has a right to win.
>
>
> **George Ho, Machine Learning Technical Lead**


## **Proving Extend's Reliability and Accuracy**


Flatiron replicated 6 months of in-house work from 2023 in just 2 weeks with Extend, with high accuracy and reliability. Three things made this work for Flatiron's NGS pipeline.


1. **Accurate splitting.** NGS reports can run to fifty pages, but only a handful contain the fields Flatiron needs. Extend's` /split` API lets them apply different logic per section: cheap parsing to find the relevant pages, premium parsing to extract every detail.
2. **Multimodal extraction.** NGS reports include tables, diagrams, visual markers, and layout patterns that all carry clinical meaning. Extend's` /extract` with multimodal capabilities handled the visual elements that had broken Flatiron's earlier attempts.
3. **The platform.** Flatiron needed a way for the team to inspect documents, validate outputs, and iterate. Extend Studio gave them that out-of-box.


> The UX is not something to be taken for granted. It was really easy for one person to just go on extend.ai and start working. I'm a technical person, but a non-technical person could have done that as well.
>
>
> **George Ho, Machine Learning Technical Lead**


Extend now runs Flatiron's NGS extraction pipeline, pulling biomarker results across hundreds of genes across Flatiron's network of 5 million patients. The pipeline uses Extend's` /split` API and` /extract` API.


For a team that puts oncologists on manual error analysis for its most sensitive deliveries, "really well" is a high bar.


## **What comes next**


Flatiron is now able to expand access to structured biomarker data across hundreds of genes, giving researchers more visibility into genetic testing patterns and how cancer treatments work in the real world.


By partnering with Extend, Flatiron can spend less time on document extraction and more time doing work that Flatiron is known for: turning oncology records into research-grade evidence and improving personalized medicine.


The Flatiron team also wrote about this.[Read it here!](https://resources.flatiron.com/real-world-evidence/a-geyser-of-biomarkers-extracting-data-from-ngs-reports)
