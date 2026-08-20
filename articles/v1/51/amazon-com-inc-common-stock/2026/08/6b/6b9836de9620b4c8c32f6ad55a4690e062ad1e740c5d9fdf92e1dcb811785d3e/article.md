---
schema_version: "1.0.0"
document_id: "6b9836de9620b4c8c32f6ad55a4690e062ad1e740c5d9fdf92e1dcb811785d3e"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-gamelift-streams/"
published_at: "2026-08-03T20:24:00+00:00"
first_seen_at: "2026-08-03T23:16:15.731839+00:00"
fetched_at: "2026-08-04T00:20:40.433623+00:00"
content_hash: "sha256:264bda31157a35f184c88d20f1d9f30d4ee817975f909f36b5d40e5d816a699f"
---

# Amazon GameLift Streams now supports sharing streams with stream URLs

Amazon GameLift Streams now offers stream URLs, which give end users temporary, unauthenticated access to a playable stream session in a supported web browser. Recipients need no AWS account, no credentials, and no software install.


To share a playable stream, create a stream URL for a stream group and one of its applications, set how long the stream URL stays valid and how many sessions it can start, and send the link. Each person who opens the link starts an independent stream session, and Amazon GameLift Streams routes them to a nearby streaming location from the locations you selected. No client integration or backend service is required. You can create, monitor, and revoke stream URLs in the Amazon GameLift Streams console or with the new CreateStreamUrl, GetStreamUrl, ListStreamUrls, and RevokeStreamUrl APIs.


There is no additional charge for stream URLs. You are charged for the stream capacity that sessions started from a stream URL consume, as described on the[Amazon GameLift Streams pricing page](https://aws.amazon.com/gamelift/streams/pricing/) . For a full list of supported Regions, see the[AWS Region table](https://docs.aws.amazon.com/gameliftstreams/latest/developerguide/regions-quotas-rande.html) .


To get started, see[Share stream sessions with stream URLs](https://docs.aws.amazon.com/gameliftstreams/latest/developerguide/stream-urls.html) in the Amazon GameLift Streams Developer Guide and the[CreateStreamUrl API Reference](https://docs.aws.amazon.com/gameliftstreams/latest/apireference/API_CreateStreamUrl.html) . To learn more about the service, see the[Amazon GameLift Streams product page](https://aws.amazon.com/gamelift/streams/) .
