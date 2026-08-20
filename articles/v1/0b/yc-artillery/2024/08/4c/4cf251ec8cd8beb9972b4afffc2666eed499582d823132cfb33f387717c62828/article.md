---
schema_version: "1.0.0"
document_id: "4cf251ec8cd8beb9972b4afffc2666eed499582d823132cfb33f387717c62828"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/load-testing-on-azure"
published_at: "2024-08-07T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:5bd7ca5a9333d9e60fa7f03287f84d506a6d9c7e9d3cc146801792755eddd7ea"
---

# Load testing on Azure with Artillery

August 7th, 2024[Announcement](https://www.artillery.io/blog/tag/announcement)


# Load testing on Azure with Artillery


Hassy Veldstra


Today, we’re announcing Azure support in Artillery. It’s been one of the most requested features from prospective users, and we’re excited to finally make it available.


Artillery can now run distributed load tests on Azure, using your organization’s Azure subscription. Azure Container Instances (ACI) are used to execute the load test, and Artillery will create all resources it needs on-the-fly and remove them at the end of the test. There is no long-lived infrastructure that needs to be created or managed in order to run load tests.


## Features


- Serverless with no infrastructure to create or manage
- Run tests from 47 different Azure regions (full list:[https://docs.art/azure#supported-regions](https://docs.art/azure#supported-regions) )
- Use with Artillery Cloud ([https://app.artillery.io](https://app.artillery.io/) ) for real-time reporting, custom charts, Playwright traces, and more
- Use Artillery’s features like CSV payloads, custom JS/TypeScript code, and plugins (including custom plugins) as normal


## Which services does Artillery use on Azure?


Artillery uses the following services to run load tests on Azure:


- Azure Container Instances
- Azure Blob Storage
- Azure Queue Storage


All resources are created dynamically on-the-fly and removed at the end of the test. There is no long-lived infrastructure to be created or managed by the user.


## How much does this cost?


This feature is available to all users at no charge for non-production and/or evaluation use. Running Artillery tests on Azure for non-evaluation, continuous and/or production purposes requires a commercial license.


More information on licensing:[https://docs.art/azure#licensing](https://docs.art/azure#licensing) .


## How do I get started?


Please see the guide on[https://docs.art/azure](https://docs.art/azure) .
