---
schema_version: "1.0.0"
document_id: "af890e778dedd1d41170bdc9f3be7d64fa1a923f2dea6b64942e396ff3f4abc2"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-gamelift-streams/"
published_at: "2026-07-27T18:58:00+00:00"
first_seen_at: "2026-07-27T21:07:24.123621+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:fe426839bf1da0eb8bc5ab62b848299850251af61adce42f48cc482f57b86c3f"
---

# Amazon GameLift Streams now supports Custom Aspect Ratio and Dynamic Resolution

Amazon GameLift Streams now supports Custom Aspect Ratio and Dynamic Resolution, giving you greater control over the streaming experience across diverse player devices and network conditions.


With Custom Aspect Ratio, you can configure a specific resolution per stream session to match your player's device — including portrait, landscape, ultra-wide, and square aspect ratios. This eliminates letterboxing or pillarboxing, delivering native full-screen experiences on mobile phones, tablets, and non-standard displays. Specify any resolution from 320 to 4096 pixels per dimension (up to 1080p total pixel budget) using the DisplayConfiguration parameter in the StartStreamSession API. You can also try custom resolution from the AWS Console.


Dynamic Resolution automatically adapts stream quality when a player's network bandwidth fluctuates. When bandwidth drops, the stream gracefully reduces resolution to maintain smooth playback without frame drops or disconnections — and automatically recovers to full quality when conditions improve. Dynamic Resolution is enabled by default for all new stream groups with no configuration required. Customers will need to download the new Web SDK.


Both features are available in all AWS Regions where Amazon GameLift Streams is offered. To learn more, see Custom stream resolution in the Amazon GameLift Streams Developer Guide.


[https://docs.aws.amazon.com/gameliftstreams/latest/developerguide/custom-stream-resolution.html](https://docs.aws.amazon.com/gameliftstreams/latest/developerguide/custom-stream-resolution.html)
