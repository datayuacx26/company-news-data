---
schema_version: "1.0.0"
document_id: "e0dae03a73db05adb1d94acc44cb45ba2f640ba291e787f8124f4db146fd426a"
company_key: "yc-bitmovin"
company: "Bitmovin"
source_id: "yc-bitmovin-news-import-1c765b973c81"
canonical_url: "https://bitmovin.com/blog/icymi-q1-2026-streaming-product-updates/"
published_at: "2026-04-09T11:48:53+00:00"
first_seen_at: "2026-08-18T14:35:46.745246+00:00"
fetched_at: "2026-08-18T14:35:27.794932+00:00"
content_hash: "sha256:f6824d888561b36b077f0da4f4ae5b30902a4829c6d369c7cc69b8628feeb193"
---

# ICYMI: Q1 2026 Product Updates Across Playback, Encoding, and AI Scene Analysis

## TL;DR


- **Observability** extends to Dolby workflows: OptiView Collectors bring full QoE and ad analytics to Dolby-powered pipelines across iOS, Android, Web, and Roku.
- **Playback Score** is now fully operational: dedicated UI, hourly granularity, and API access make it a practical health monitor, not just a metric.
- **VOD Encoder** gets more reliable at scale: improvements across Dolby Vision, subtitles, trimming, and Azure Blob reduce stalled jobs and friction.
- **Live Encoder** simplifies recurring event workflows: persistent RTMP endpoints, multi-audio CMAF, extended language tags, and better SCTE-35 signaling ship together.
- **AI Scene Analysis** automates ad placement: Ad Opportunity Score and Scene Boundaries for Ad Breaks remove manual decision-making from ad scheduling.


---


## Table of Contents


Q1 2026 continued the momentum from our[Q4 2025 updates](https://bitmovin.com/blog/icymi-q4-2025-streaming-product-updates/) , with improvements across our full product portfolio focused on helping teams deliver better viewer experiences, sharpen monetization, and unlock greater operational flexibility across live and on-demand workflows.


Below is a recap of what shipped in Q1, along with a look at where product focus is headed into Q2.


## **Playback (Player + Observability)**


Q1 extended Bitmovin’s Observability solution into Dolby-powered workflows while making playback health data more accessible and actionable across engineering and operations teams.


### **Q1 delivered**


- **OptiView Collectors for Dolby Workflows.** Collectors across iOS, Android, Web, and Roku extend Bitmovin’s Observability performance monitoring and root cause analysis to Dolby-powered workflows, with full CSAI and SSAI advertising analytics support.
- **Playback Score with Dashboard UI, Hourly Granularity, and API Access.** Combines startup, smoothness, and success into a single health indicator, now with a dedicated dashboard UI, hourly granularity, and API access for near real-time quality monitoring.
- **Improved Live Channel Switching on Android.** Near-instantaneous transitions on Android, Android TV, and Android STB reduce viewer disruption during channel changes.


### **Focus in Q2**


- **Advertising QoE Analytics and Alerts.** Brings playback performance monitoring, abandonment and fill rate tracking, and real-time alerting across CSAI, SSAI, and SGAI, helping teams identify and reduce revenue lost to poor ad playback.
- **Expanded MCP Server and AI Assistant.** Adds broader app support and intent-based querying, enabling deeper AI-driven workflows directly within the Bitmovin Dashboard.
- **Advertising Support for Player Web X.** Unlocks ad revenue across all formats in a single, lightweight player.


## **VOD Encoder**


Q1 improvements focused on expanding subtitle customization options, giving teams more storage control, and improving reliability at scale across complex encoding workflows.


### **Q1 delivered**


- **ASS/SSA Burn-in Subtitle Fonts and Styling.** Font selection and styling for burned-in subtitles are now configurable, enabling more flexible branding and localization across playback environments.
- **Configurable S3 Output Storage Classes.** Direct control over S3 output storage classes lets teams optimize cost and retention strategies for encoded assets.
- **Improved Encoding Reliability at Scale.** Stability improvements across Dolby Vision, subtitle rendering, trimming, and Azure Blob workflows reduce stalled jobs and operational friction.


### **Focus in Q2**


- **HEVC and AV1 Improvements.** Higher visual quality at lower bitrates for more efficient content delivery.
- **HDR10+.** Dynamic metadata support to enhance HDR playback across compatible devices.
- **Extended Encoding Templates.** More flexibility and control when defining and reusing encoding configurations.


## **Live Encoder**


Q1 Live Encoder updates focused on improving contribution workflow flexibility, multi-audio support, and ad signaling accuracy for teams managing recurring events and complex multi-language streams.


### **Q1 delivered**


- **Persistent RTMP Contribution Endpoints.** RTMP ingest endpoints can now be reused across recurring live events, simplifying operational management for scheduled programming.
- **Multi-Audio CMAF Output for DASH-IF Live Media Ingest.** Multiple audio output tracks are now supported when publishing CMAF over the[DASH-IF Live Media Ingest protocol](https://dashif.org/Ingest/#interface-1) , enabling flexible audio language and commentary workflows.
- **Extended Language Tags.** The CMAF muxer now supports full[IETF BCP 47 language tags](https://datatracker.ietf.org/doc/html/rfc5646) (e.g. en-US, pt-BR, sr-Latn-RS) instead of short ISO 639 codes, improving compliance for multi-language and regional workflows.
- **Improved SCTE-35 Ad Signaling.** Splice timestamp accuracy, explicit last-segment signaling, and filtering of internal/private descriptors improve reliability of downstream ad insertion.


### **Focus in Q2**


- **Dynamic Live Standby Pools.** Auto-scaling to reduce operational costs while keeping the ability for instant stream start.


## **AI Scene Analysis**


AI Scene Analysis expanded its ad automation capabilities in Q1, with new tooling to evaluate ad break effectiveness and align placement boundaries to the right moment in content.


### **Q1 delivered**


- **Ad Opportunity Score.** Analyzes scene type, narrative, tension, and pacing to score how effective a boundary is as an ad break, helping automate placement decisions and reduce viewer disruption.
- **Scene Boundaries for Ad Breaks.** Aligns boundaries to segment boundaries, keyframes, and optional cue points, enabling ad scheduling via a custom VMAP.


### **Focus in Q2**


- **Live Scene Boundary Detection.** Enables intelligent SCTE marker insertion on live streams, improving ad quality for FAST and catch-up TV channels.
- **Intelligent Vertical Cropping.** Tracks contextually relevant objects and speakers during clip generation, automating engaging vertical video creation for social platforms.


## **Looking Ahead**


Q2 focus is on expanding ad quality intelligence, from Observability alerting across CSAI, SSAI, and SGAI to live scene detection for smarter SCTE marker insertion. Across the encoding stack, HEVC and AV1 improvements, HDR10+ support, and Dynamic Live Standby Pools continue to push on quality, cost efficiency, and operational agility.


As always, if you have questions about any of these updates or want to dig deeper into a specific product area, reach out to your Bitmovin account team or explore the developer documentation.
