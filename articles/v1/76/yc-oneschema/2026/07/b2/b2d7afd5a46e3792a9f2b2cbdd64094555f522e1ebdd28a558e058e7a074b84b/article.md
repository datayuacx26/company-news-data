---
schema_version: "1.0.0"
document_id: "b2d7afd5a46e3792a9f2b2cbdd64094555f522e1ebdd28a558e058e7a074b84b"
company_key: "yc-oneschema"
company: "OneSchema"
source_id: "yc-oneschema-news-import-43da02420b1d"
canonical_url: "https://www.oneschema.co/blog/copy-ai-oneschema"
published_at: null
first_seen_at: "2026-07-24T07:26:45.232367+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:c67a8d71372b67394744dcb4c9bf66e6f42eda0c0a79c7175599a14dee4df2de"
---

# Elevating AI Productivity: How Copy.ai Simplifies Data Uploads with OneSchema

## ‍ **Background & Use Case**


Lauded as one of the leading AI platforms on the internet,[Copy.ai](http://copy.ai/) is one of generative AI’s hottest startups. Copy.ai serves a broad range of copywriting use cases from helping companies generate 10x more content to helping sales teams instantly research thousands of accounts.


One of Copy.ai’s most recent products is Workflows. Workflows is built for repeatable, multi-step AI workflows and allows customers to upload data in bulk to run a series of AI prompts. An example would be scraping a LinkedIn profile and company homepage to generate a personalized email.


‍


## **Problem**


As Copy.ai was planning the launch of their Workflows feature, they built an early version of CSV import. Copy.ai software engineer Eric Karwatowski recalled, “we quickly discovered that managing all of the CSV import edge cases and handling the long-tail of formatting quirks is an intricate technical feat. The Jira tickets kept coming in, and we realized we were burning a lot of engineering hours on something we could outsource.”


Copy.ai Product Lead Patrick Hunton was also unhappy with the product experience. “Our original importer was extremely restrictive and minimal. We were concerned that its clunkiness would prevent users from successfully getting started with Workflows,” shared Hunton.


Copy.ai has an extremely flexible data import use case: customers can set up workflows with arbitrary inputs, and the CSV importer needs to adapt to the customer’s unique workflow. For example, say a sales team wanted to build a Workflow to generate personalized email copy. They might provide a CSV file with links to the prospect's LinkedIn profile, the company’s homepage, the company support docs, and the prospect’s university. Copy.ai would need to spin up an importer that accepts the fields linkedin_profile_url, company_url, support_docs_url, and university_name.


The team first investigated open source solutions to see what might be a fit. However, the open source solutions didn’t meet the team’s product requirements. “While the open-source options had basic functionality, the user interfaces weren’t intuitive and they couldn’t meet our data model requirements,” said Karwatowski.


‍


## **Solution & Results**


> ‍ *“The initial setup took about a day, and we spent the rest of the week configuring the dynamic templates feature. From there to production was the flip of a switch”*


After struggling with the open source importers, Hunton was excited to hear about OneSchema on a podcast for product managers. They spun up the importer in 1 day.


“I was stunned with how fast we got it live,” said Hunton. “The initial setup took about a day, and we spent the rest of the week configuring the dynamic templates feature. From there to production was the flip of a switch.”


Copy.ai was able to launch a production-ready importer during their 2 week trial period. From their first call with OneSchema to launching a live, production importer took 11 business days. The first week was spent on implementation and the second week on testing. “Moving from proof-of-concept to production was as simple as the flip of a switch,” said Hunton.


“We’ve only heard really good things about the OneSchema CSV import flow,” said Karwatowski.


Copy.ai uses OneSchema’s React SDK. To receive cleaned data from OneSchema, Copy.ai uses our frontend passthrough feature and reformats the output data before passing it along to their API endpoint.


‍


{{blog-content-cta}}


‍


## **Expansion**


As Copy.ai continues to expand their platform, CSV import will be an integral data ingestion method for their customers. While their more technical users can send data via API, a strong contingent of their customer base is non-technical and primarily uses CSVs.


“We’ve only scratched the surface of the power of OneSchema,” said Karwatowski. “We’re excited to continue to improve the importer with features like custom branding and more advanced validations.”


Karwatowski has also been impressed by OneSchema’s product velocity. Karwatowski noted, “The one issue we ran into during implementation was confusion around using a test instead of a production token. I was excited to see the launch of the team’s environments feature to resolve this issue.”


‍


**The CSV Importer That Went Viral**


Copy.ai’s Workflows product is making waves, assisted by a seamless data upload experience to make activating workflows fast and easy.


In a[viral Instagram reel](https://www.instagram.com/reel/Cxp69RwrB02/?igshid=MzRlODBiNWFlZA%3D%3D) / Tik Tok, AI productivity influencer[Aashi Gupta](https://www.instagram.com/marketrypro/?g=5) (marketrypro) shared her excitement around Copy.ai’s new Workflows product. She used Workflows to automatically write sales emails and cruises through uploading prospect metadata, powered by OneSchema.
