---
schema_version: "1.0.0"
document_id: "e12274ee717c5a129428fc7188c9a511117254484c01a8b3a57017d5d5c3d002"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/improved-quality-control-over-your-llm-s-responses-with-annotations-and-human-feedback"
published_at: "2024-05-31T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:eb4b245d01b2710afdcd94539818f9d4a042e8e97266473da05a549b5394cd39"
---

# Improved quality control over your LLM’s responses with annotations and human feedback

May 31, 2024


[Improved quality control over your LLM’s responses with annotations and human feedback](https://www.openlayer.com/changelog/improved-quality-control-over-your-llm-s-responses-with-annotations-and-human-feedback)


Setting up alerts is an essential first step to monitoring your LLMs, but in order to understand why issues arise in production, it’s helpful to have human eyes to review requests.


This process is now easier than ever in Openlayer - you can add annotations to any requests, with custom values. If you’ve set up tracing, you can annotate each individual step of the trace for more granularity.


Every request can also be rated as a thumbs up or thumbs down, making it easy to scan through good and bad responses and figure out where your model is going wrong.


We’ve released some other huge features and improvements this month, so make sure to read the full changelog below!


## Features


- •


UI/UX


Ability to export data from the UI (Now you can download requests data right from the workspace. This is especially helpful if you’ve applied filters and want to download the filtered cohort of data)
- •


UI/UX


Updated navigation (Our navigation has a new layout featuring breadcrumbs at the top, making it much easier to navigate between projects and understand the hierarchy)
- •


UI/UX


Annotation and human feedback (You can now annotate any request with custom values. You can also give every request a thumbs up or thumbs down to make identifying error patterns even easier)


## Improvements


- •


Templates


More project templates
- •


SDKs


Improved OpenAI SDK
- •


UI/UX


Improvements to billing page in settings
- •


Integrations


Project-level Git repository settings now available
- •


Integrations


Ability to now edit branch and root directory in project-level git settings
- •


UI/UX


With new navigation, ability to copy project name and inference pipeline ID
- •


UI/UX


Ability to add ground truths to requests and edit existing ground truths
- •


UI/UX


Data in individual test modals is now filtered by selected evaluation window


## Fixes


- •


Performance


Some tests were improperly skipped
- •


UI/UX


Openlayer Assistant was broken
- •


Security


Spaces in No PII test caused errors
- •


Performance


Issue with metric tests when input variable names were null
- •


UI/UX


Hovering over graph with no results shows broken tooltip
