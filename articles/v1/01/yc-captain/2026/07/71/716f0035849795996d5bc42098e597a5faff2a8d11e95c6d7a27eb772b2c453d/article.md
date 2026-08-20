---
schema_version: "1.0.0"
document_id: "716f0035849795996d5bc42098e597a5faff2a8d11e95c6d7a27eb772b2c453d"
company_key: "yc-captain"
company: "Captain"
source_id: "yc-captain-rss-30d4a88671b2"
canonical_url: "https://www.runcaptain.com/blog/ragie-to-captain-migration"
published_at: "2026-07-14T12:00:00+00:00"
first_seen_at: "2026-08-18T01:28:25.127468+00:00"
fetched_at: "2026-08-18T01:28:26.919384+00:00"
content_hash: "sha256:e0fb6111c1d559d510fc2548c146b565d30a62d266273d0f1f0ab262be47f047"
---

# Migrating from Ragie to Captain

# Migrating from Ragie to Captain


Ragie’s hosted service will end on Sunday, July 19, 2026. To help customers keep their applications running, many Ragie customers have been migrating to Captain.


Captain provides managed multimodal indexing and retrieval for AI applications. Most customers with access to their original source data can begin setting up their migration in under 15 minutes.


**Need help migrating? Emailsupport@runcaptain.com with your company name, approximate data volume, and current Ragie integrations.**


## Step 1: Locate your source data


First, determine where your original documents and data are stored.


If your data remains available in a source such as local storage, Amazon S3, Google Drive, Google Cloud Storage, Dropbox, or another supported storage system, you can connect that source directly to Captain. You do not need to move the original data through Ragie first.


If the only copy of your data is stored in Ragie, emailsupport@ragie.ai as soon as possible with:


- The subject line “Data export request”
- Your company name
- The email address associated with your Ragie account
- Any relevant partition or dataset information


Ragie will provide current information about export availability, processing time, pricing, and the data included in the export. Because exports require manual processing, customers should submit requests as early as possible.


## Step 2: Create your Captain collection


Captain organizes indexed data into collections. A Captain collection generally serves the same role as a Ragie partition: it provides a logical boundary for a particular application, customer, workspace, or dataset.


Captain can index text, documents, images, audio, video, and other file types. It includes document parsing, hybrid search, reranking, relationships between related chunks, multimodal understanding, and built-in retrieval evaluations.


To receive migration assistance, emailsupport@runcaptain.com with:


- Your company name
- Your approximate number of documents or total file volume
- Your current Ragie integrations
- Your target migration date (if you have one)
- Any features or workflows that are critical to your application


Captain is providing hands-on migration support and discounted pricing for Ragie customers.


**Using an AI coding assistant?** Provide[docs.runcaptain.com/llms.txt](http://docs.runcaptain.com/llms.txt) as context for current Captain API documentation and migration guidance.


## Step 3: Connect or upload your data


Captain currently supports sources including:


- Local files
- Web pages and URLs
- Amazon S3
- Google Cloud Storage
- Azure Blob Storage
- Cloudflare R2
- Dropbox
- Supabase Storage
- Backblaze B2
- Google Drive
- Microsoft SharePoint
- Microsoft OneDrive
- Notion
- Snowflake


See[Captain’s documentation](http://docs.runcaptain.com/) for specific guides and endpoints.


Some integrations are available through Captain Studio but do not yet support programmatic indexing through the Captain API. Captain’s team can confirm the best migration method for your particular sources.


When migrating exported Ragie data, Captain will help determine whether the export should be uploaded directly, transformed, or reindexed from the underlying source documents. In most cases, Captain will generate a new index rather than relying on previously generated embeddings.


## Step 4: Update your application


After your indexing jobs complete successfully, update your application to query the corresponding Captain collection through[Captain’s v3 Query API](https://docs.runcaptain.com/reference/v3/query) .


Before directing production traffic to the new collection, test a representative set of existing searches and application workflows. Compare the returned results, citations, metadata, filters, latency, and access-control behavior with your existing Ragie implementation.


Once you have validated retrieval quality and application behavior, update your production credentials and endpoint configuration to direct traffic to Captain.


Captain’s migration team is prioritizing Ragie customers during the transition. Contactsupport@runcaptain.com for assistance mapping your existing implementation to Captain.


## Frequently asked questions


### Can my application remain online during the migration?


In most cases, yes. You can build and test your Captain collection while your current Ragie integration remains active, then switch your application after validating the new setup.


Because Ragie’s hosted service is scheduled to end on July 19, customers should begin the process immediately and allow enough time for indexing and testing.


### How long does indexing take?


Indexing time depends on the number, size, and type of files being processed. Large initial migrations may require a temporary indexing-capacity increase. Contact Captain in advance if your initial migration is large so the team can help plan appropriate indexing capacity.


### How does pricing work?


Captain is offering discounted pricing for transitioning Ragie customers. Contactsupport@runcaptain.com with your company name and expected data volume to receive migration pricing.


### What if Captain does not support one of my Ragie integrations?


Tell Captain which integration or workflow is blocking your migration. The team will assess the requirement and determine whether it can provide a workaround, migration service, or prioritized integration support.


During the Ragie migration effort, Captain has already expanded support for capabilities including Dropbox API indexing, source change detection, Supabase Storage indexing, and improved transcription.


### Who should I contact?


For questions about Ragie data exports or your existing Ragie account, contactsupport@ragie.ai .


For Captain onboarding, indexing, API migration, or pricing assistance, contactsupport@runcaptain.com .
