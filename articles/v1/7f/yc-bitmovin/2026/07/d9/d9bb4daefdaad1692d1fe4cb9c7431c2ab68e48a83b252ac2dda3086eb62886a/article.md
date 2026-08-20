---
schema_version: "1.0.0"
document_id: "d9bb4daefdaad1692d1fe4cb9c7431c2ab68e48a83b252ac2dda3086eb62886a"
company_key: "yc-bitmovin"
company: "Bitmovin"
source_id: "yc-bitmovin-news-import-1c765b973c81"
canonical_url: "https://bitmovin.com/blog/bitmovin-ad-observability-mcp-server-claude-chatgpt/"
published_at: "2026-07-31T14:24:00+00:00"
first_seen_at: "2026-08-18T14:35:46.745246+00:00"
fetched_at: "2026-08-18T14:35:27.794932+00:00"
content_hash: "sha256:d46ab4d9f18f502ca58a0a2dccdb26a710efa4bfb98a5baea4230dc0a0a38f38"
---

# How to Diagnose an Ad Revenue Drop from Claude and ChatGPT

# TL;DR


- Four new ad delivery metrics live today in the Observability solution’s dashboard
- Two isolate supply problems, two isolate completion problems
- Ask a question in plain language and the Bitmovin MCP server’s new ad tools return the number directly
- Bitmovin is live in the ChatGPT Plugin Directory and connectable in Claude as a custom connector


Ad revenue can drop for two very different reasons, and one blended revenue number cannot tell them apart. Inventory can go unsold, a supply problem, more of it filled with slate than with a paying ad. Or ads that did sell can fail to finish playing, a completion problem, whether from a technical failure or a viewer leaving mid break. Telling those two apart has meant filtering the dashboard by CDN, device, and ad break to isolate which one actually happened.


Four new metrics in the Observability solution now separate supply from completion directly, two metrics for each. The same data is also queryable through the Bitmovin MCP server in plain language, live in the[ChatGPT Plugin Directory](https://chatgpt.com/apps) and connectable in[Claude](https://github.com/bitmovin/skills) .


## What it is


These four metrics sit in the dashboard under Dashboard, Observability, Server side advertising, Ad Delivery. Fill Rate and Slate Duration cover the supply side, Incomplete Ads and Ad Breaks Abandoned cover completion.


- **Fill Rate.** Paid ads divided by total scheduled ads. A 60% fill rate means four in ten slots ran filler instead of a paying advertiser, a direct read on how much of the available inventory actually had a paying advertiser behind it.
- **Slate Duration.** The total time spent playing slate, the filler or house content that plays when there is no paid ad to fill a slot. It is the supply problem measured in time instead of a percentage.
- **Incomplete Ads.** An ad scheduled to play in a break that never finished, either because it never started at all or because the viewer left before it ended. It folds both causes, a technical failure and a viewer walking away, into one completion number.
- **Ad Breaks Abandoned.** Breaks that started but lost the viewer before every paid ad finished, with slate excluded from the completion count on purpose. Rising abandonment usually points to pod length, and it devalues whatever ad is running last.


Those same four numbers, plus engagement, quality, and watch time detail, are also exposed as 14 new tools in the Bitmovin MCP server, so they can be asked for directly instead of pulled from a dashboard filter or looked up by exact API metric name.


## Why it’s useful


The split matters because the fix, and the owner of the fix, is different on each side. A supply problem means selling more inventory or cutting how much falls to slate, which is a sales or yield question, not an engineering one. A completion problem means investigating technical failures in ad delivery or rethinking ad break length and placement, which sits with ad tech or product. Without separating the two, a completion problem often gets treated as a sales shortfall, or a supply problem gets escalated to engineering as a bug, and the team chasing it burns time before finding out it was never theirs to fix.


Fill Rate and Slate Duration answer the supply question on their own, without needing to check whether ads that did serve actually finished. Incomplete Ads and Ad Breaks Abandoned answer the completion question on their own, without needing to know whether inventory sold in the first place. Each pair stands alone, so whoever is looking at the number does not need the other pair’s context just to know which team should be looking at it.


## Step 1. Connect your tool


Getting to these numbers does not require dashboard access, but it does require ad delivery data already flowing into the Observability solution. Connecting does not backfill history. It only exposes data that is already being collected, so this is built for existing Bitmovin customers with ad tracking already integrated, not a way to analyze a stream that has never run through the platform. Both paths below take under a minute and neither requires developer setup.


- **ChatGPT.** Go to chatgpt.com/plugins, search Bitmovin, click Install Plugin, and sign in with your Bitmovin account. No server URL to paste and no developer mode to enable.
- **Claude.** Open Settings, then Connectors, in Claude.ai. Click Add, then Add custom connector. Enter Bitmovin as the name and https://mcp.bitmovin.com as the server URL. Click Add, click into the connector, hit Connect, then sign in on the Bitmovin login page and approve access. The directory listing is still in progress, so this is the current path.


## Step 2. Ask your first question


These are prompts that have actually been tested, not hypothetical ones.


- *“How many ad impressions were recorded this week and how many actually started playing?”* A delivery check in one line.
- *“Show me the full ad completion funnel for last week.”* Shows where viewers drop off, 25% to 50% to 75% to complete.
- *“What was the click through rate and skip rate for ads last week?”* The two numbers advertisers usually ask for.
- *“What are the most frequent ad error codes this week?”* Returns a ranked list of error codes by occurrence count.


## Step 3. One MCP server, every product


The same connection covers every Bitmovin product, not just ad analytics. One connector reaches Player, the Observability solution, VOD Encoder, and Live Encoder, so the same login that answers an ad question can also pull playback QoE data, encoding job status, or Stream Lab test results, all in the same chat window.


## Why this matters for advertising teams


Supply and completion problems carry different costs when they go unnoticed, and both compound the longer they run. A supply problem underdelivers against what was sold, and if it runs through a full billing cycle it can turn into a make good conversation with the advertiser, extra inventory given away for free to cover the shortfall. A completion problem burns ad spend on impressions that never finished, and if it shows up consistently across campaigns it starts to erode trust in the reporting itself, since the advertiser is paying for a number the platform cannot fully account for.


Catching which one is happening does not require dashboard access or a specialist’s help. The question can be typed directly into ChatGPT or Claude, and the answer comes back immediately, in time to fix a supply gap before the billing cycle closes or flag a completion issue before it shows up in the next campaign’s report. All four metrics are live today in the dashboard, and the MCP tools sit alongside them, live in the ChatGPT Plugin Directory and available in Claude as a custom connector while its directory listing is in progress.


Additional Links


Main documentation:[https://developer.bitmovin.com/playback/docs/bitmovin-observability-mcp-server](https://developer.bitmovin.com/playback/docs/bitmovin-observability-mcp-server)


Observability Assistant, the in-dashboard companion:[https://developer.bitmovin.com/playback/docs/the-bitmovin-observability-assistant](https://developer.bitmovin.com/playback/docs/the-bitmovin-observability-assistant)


Skills repo:[https://github.com/bitmovin/skills](https://github.com/bitmovin/skills)


Understanding MCP for Agentic AI Workflows, Bitmovin’s explainer on what MCP is and when to use it:[https://bitmovin.com/blog/understanding-mcp-agentic-ai-data-access/](https://bitmovin.com/blog/understanding-mcp-agentic-ai-data-access/)


How to Build a Streaming Platform in a Day, a hands-on walkthrough of an AI coding agent paired with Bitmovin’s MCP and CLI:[https://bitmovin.com/blog/how-to-build-video-streaming-platform-ai-bitmovin/](https://bitmovin.com/blog/how-to-build-video-streaming-platform-ai-bitmovin/)
