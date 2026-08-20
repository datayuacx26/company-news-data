---
schema_version: "1.0.0"
document_id: "5f674d03897af69380aba78d115c34f7de81db65be42d49a5fd1702356c1ae22"
company_key: "yc-sieveai"
company: "sieve"
source_id: "yc-sieveai-news-import-c83e13839616"
canonical_url: "https://usesieve.com/blog/bitcoin-metric-tracking"
published_at: "2024-01-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:04:09.595708+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:5c54475fc8c82b90be6cefdf3b8690c1acc9426420661ebf56f031c86ce1a2ff"
---

# Automating Bitcoin Mining Metrics from Manual PDF Transcription

## Context


Reid is an equities investor trading bitcoin mining companies. His strategy involves keeping up to date with the latest metrics (e.g., hashrate) of miners across the industry.


## Issues


Reid's best source of this data comes from sell-side analyst reports, which are often sent in PDF format. Although he is technical in other areas, it seems that Reid's best option to use his analyst report data is to manually transcribe the data from PDF to Excel. This is cumbersome, error-prone, and blocks his goals of real-time tracking and alerting, since the process is bottlenecked on him finding time to transcribe the data.


## sieve solution


We built Reid an API endpoint to which he can supply links to publicly available documents, or supply his own documents, and receive consistently formatted, human-verified data extracted from the files. Behind the scenes, sieve finds the right document, uses AI to extract the requested data points, and forwards the data to a team of human reviewers. After careful human review to ensure accuracy, the data is returned to Reid via the API. This opens up a world of real-time monitoring and alerting possibilities that allow Reid to react more quickly in the wake of new information.


### Turn your PDF reports into real-time data?


Stop manually transcribing sell-side reports. Let sieve extract and deliver consistently formatted data so you can enable real-time tracking and alerting.


Contact us athello@usesieve.com to discuss your use case.
