---
schema_version: "1.0.0"
document_id: "861ade8dc8505ec12e4d3999ce9ae0d92215e9b4e0b51cf81eda2c5b09341eff"
company_key: "yc-polytomic"
company: "Polytomic"
source_id: "yc-polytomic-news-import-c5d7ea331448"
canonical_url: "https://www.polytomic.com/blog-posts/announcing-polytomic-enrich"
published_at: null
first_seen_at: "2026-07-25T19:31:15.738590+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:de6ced9bf12cf919fe8060eaac0da836104623e9a1b3d512aec798b7b28a438e"
---

# Announcing Polytomic Enrich

Today we are exceedingly-excited to launch Polytomic Enrich, a universal interface for enriching your data.


The number of data enrichment providers has exploded, whether it's those that enrich people and company data based on partial profiles, or IP addresses and email addresses with fraud scores, or any other myriad enrichments. Everyone prefers a full dataset rather than a partial one.


But each provider has its own APIs and interfaces. One needs to learn the intricacies of each one in isolation before any use.


Polytomic Enrich is a single interface for executing data enrichment, no matter the provider and no matter the source of your partial data. You can enrich data in your data warehouse, databases, CRMs and cloud applications, spreadsheets, and even arbitrary APIs.


## How it works


Starting today, every Polytomic data model, no matter what system it's pulled from, comes with a Data Enrichment section. This lets you select an enrichment provider:


After which you'll be asked to provide any fields from your data model as input to your provider. Doing so will automatically provide you with a list of enriched fields from your provider which you can add to your Polytomic model:


Selected fields become part of your Polytomic model and can be treated like any other fields: they can be synced anywhere, even back to the model's source system.


Today we are starting with support for these enrichment providers:


-[ZoomInfo](https://www.zoominfo.com/)
-[Apollo.io](https://www.apollo.io/)
-[Harmonic](https://www.harmonic.ai/)
-[PredictLeads](https://www.predictleads.com/)
-[MailerCheck](https://www.mailercheck.com/)
-[Scamalytics](https://www.scamalytics.com/)


In a few clicks you are now able to enrich any data, in any system, through a single interface. We hope you enjoy this new power.


Documentation on Polytomic Enrich is located[here](https://docs.polytomic.com/docs/enrich-overview) . As always, please email us at support@polytomic.com with any questions.


‍


[Back to blog](https://www.polytomic.com/blog)
