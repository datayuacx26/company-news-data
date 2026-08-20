---
schema_version: "1.0.0"
document_id: "523f7f7f48f7fa418abe69477172f4665ec80798a26448503be13325b272aede"
company_key: "yc-recall-ai"
company: "Recall.ai"
source_id: "yc-recall-ai-news-import-5dc65e725852"
canonical_url: "https://www.recall.ai/blog/recall-ai-and-zoom-announce-real-time-data-partnership"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-22T11:03:36.206412+00:00"
fetched_at: "2026-07-28T21:46:31.447870+00:00"
content_hash: "sha256:fe226efd11db9b96a6993a2dced7ef5823eed8eb22b660fd2ba9aac42b36fed0"
---

# Recall.ai and Zoom partner to help companies build with meeting data

## What is Recall.ai?


Recall.ai securely captures meeting data from Zoom and other video conferencing platforms with one unified API. Recall.ai allows developers to get audio, video, transcripts, and metadata from conversations – whether they happen in a virtual meeting, phone call, or in-person chat. Over 3,000 companies use Recall.ai to power meeting assistants, AI sales coaches, hiring intelligence platforms, notetakers for clinicians, and more.


[Recall.ai Build with meeting data video](https://youtu.be/AH7w6hKlXsk?si=EMNqq4Qgo3U_j-vh)


## Why is Recall.ai a Zoom partner?


Recall.ai holds Zoom Preferred Partner status – the highest tier in Zoom's partnership program and a mark of verified trust. Recall.ai’s[Meeting Direct Connect](https://docs.recall.ai/docs/meeting-direct-connect-overview) integration connects directly with[Zoom's Realtime Media Streams (RTMS) API,](https://developers.zoom.us/docs/rtms/) and Recall.ai works closely with Zoom to ensure that users have a smooth, reliable experience. Because Recall.ai is[HIPAA, SOC 2, ISO 27001, GDPR and CCPA compliant](https://security.recall.ai/) , it’s a secure solution for companies in all industries.


## How can companies use Zoom Realtime Media Streams (RTMS)?


RTMS allows companies to get live meeting data from Zoom without bots. Connecting directly to Zoom’s media servers over websockets, RTMS provides isolated audio streams, video streams, diarized transcripts, and metadata like participant events and speaker-change notifications. Delivering meeting data in real time and with low latency, RTMS enables a Zoom-native way to power automated note-taking, instant action items, AI assistants, and more.


RTMS helps companies meet regulations on media capture and supports enterprise compliance needs. When using RTMS, users are informed which apps are accessing meeting data, and meeting hosts have full, granular control over access. By streaming data directly to an application from Zoom’s servers, RTMS mitigates third-party data retention and security concerns.


## Why use RTMS through Recall.ai?


RTMS works by sending users a continuous, live feed of meeting data via WebSocket, with each meeting having its own open WebSocket channel. Companies need a reliable way to manage all of these WebSocket connections at the same time – opening them when a meeting starts, keeping them alive, and closing them when the meeting ends. As demand for meetings highly varies throughout the day, having scalable, cost-effective infrastructure to support this can be difficult.


By taking on the infrastructure load, Recall.ai’s integration makes RTMS faster to set up and easier to manage. For users who don’t need live data, Recall.ai handles concurrency and stream management considerations, and simply delivers the Zoom meeting data via an API. That way, companies don't have to create the infrastructure to maintain many WebSocket connections and process parallel streams reliably. Recall.ai’s API also allows companies to ensure meeting data is reliably captured across different scenarios, including in external meetings in which hosts may have outdated Zoom clients or during breakout rooms.


If you’re interested in RTMS, you can use it through Recall.ai to easily build products with Zoom meeting data. RTMS works out of the box with Recall.ai, as Recall.ai is an official reseller of Zoom RTMS. This means you can start using RTMS on a[pay-as-you-go plan](https://www.recall.ai/pricing) with Recall.ai, with volume discounts at scale.


Companies already use[RTMS through Recall.ai](https://www.recall.ai/blog/what-is-zoom-rtms) to simply and reliably get data from meetings. To get started, create a Zoom app on the App Marketplace and[add RTMS scopes](https://developers.zoom.us/docs/rtms/meetings/add-features/) . You can sign up for a[free Recall.ai account](https://www.recall.ai/) and follow[this guide](https://developers.zoom.us/blog/rtms-recall-ai-guide) to set up RTMS using Recall.ai’s Meeting Direct Connect.
