---
schema_version: "1.0.0"
document_id: "c06fdf8810b30ea859c4a385e09e3424df1a3f29beaa32e2a669ce434298769a"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/superwall-and-adjust-integration-for-full-funnel-subscription-attribution"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:f33734d27df46473f59cfec37b9f6409ff0a72a228c6ebaf60e92bb7d7b3953e"
---

# Superwall & Adjust integration for full-funnel subscription attribution

## How it works


Mobile user acquisition teams have historically faced a significant measurement challenge: while app installations are tracked, the paywall experience and subsequent revenue events remain disconnected from the campaigns that drove them. This integration resolves that issue by enabling subscription events to flow directly from Superwall to Adjust as device-level app events.


The integration leverages Adjust's server-to-server event API, eliminating the need for additional SDK implementation. Superwall transmits subscription events — mapped to your Adjust event tokens — directly from the server. The integration supports fourteen event types spanning the complete subscription lifecycle, including trial initiation, conversion to paid plans, renewals, and cancellations. Revenue events include transaction amounts reported as either gross or net figures after store fees.


[Full setup details are available in the Adjust integration documentation](https://superwall.com/docs/integrations/adjust) .


## In practice


Consider a fitness application running a paid campaign on Meta. A user engages with the ad, installs the app, encounters the paywall, and initiates a trial. After seven days, they convert to a paid subscription plan.


**Without integration:** Adjust captures the install event. Trial initiation and conversion events exist within Superwall but remain disconnected from the mobile measurement platform and unattributed to the campaign source.


**With integration running:** The install, trial start, and conversion all become attributed to the same campaign and creative. The growth team gains visibility into return on ad spend based on subscription revenue rather than installation metrics, enabling more informed budget allocation.


## Getting started


Configure event tokens for each subscription event type within Adjust's AppView, then establish mappings in Superwall's Integrations dashboard. Setting the Adjust device ID in your application via` setIntegrationAttributes` is required — attribution requires at least one device identifier.


[Complete walkthrough: Adjust integration documentation](https://superwall.com/docs/integrations/adjust) .


Available now on iOS and Android.


For existing Superwall users: open Integrations in the dashboard to connect Adjust in minutes. New users can[create a free account](https://superwall.com/sign-up) to begin.
