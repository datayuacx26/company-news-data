---
schema_version: "1.0.0"
document_id: "9c4d40eda4a3fe52dd407690c8c514b3700a331c1f66582a824da08d76bc3b00"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/six-new-airbyte-connectors"
published_at: "2026-08-14T07:36:00+00:00"
first_seen_at: "2026-08-14T20:06:31.097915+00:00"
fetched_at: "2026-08-14T20:06:31.917012+00:00"
content_hash: "sha256:eb5ab5d16e24eb8fc3e866d3be9c8a49b96db3b8ebe31b81e23e39504a675b7c"
---

# Six New Community Connectors for Airbyte

Six new connectors just landed in Airbyte thanks to our community contributors. The following connectors are all live now:


- **Ramp** : Connects corporate cards and spend management to your warehouse. It syncs cards, transactions, and reimbursements so finance teams can reconcile spending automatically without exporting CSVs.
- **Claude** : Brings org-level analytics for teams on the Claude Platform. It syncs daily usage, cost, and seat engagement to understand model spend goes and who is using it.
- **Dust** : Covers the enterprise no-code AI platform teams use to build custom, context-aware assistants. The connector syncs usage, agent, and tool-adoption metrics to show which internal assistants are getting traction and which tools they lean on.
- **Reddit Ads** : Pulls from Reddit's advertising platform, syncing campaigns, ads, and performance metrics. Marketing teams running spend on Reddit can now land that data next to every other channel.
- **Mntn** : Integrates the connected TV ad platform that facilitates commercials across streaming shows, movies, and live sports. It syncs advertisers, campaigns, and creative performance, bringing CTV metrics into the same reporting dashboard as all your other sources.
- **Commission Junction** : Syncs from this affiliate network so that advertiser commissions and affiliate revenue show up beside paid and organic revenue.


A big thank you to the contributors who took the time to create and submit these connectors. Your continued support makes Airbyte the leading tool for open source data movement.


## What you can expect moving forward


Airbyte's connector catalog relies on community contributions to extend our functionality to enterprise and startup connectors alike. However, as we shifted engineering resources to develop Airbyte’s new agentic infrastructure features, community pull requests piled up faster than we could review them. Contributors who took the time to build and test a connector waited far longer than they should have to. With the release of these six connectors, we're revamping the way we review and approve community connectors to cut down on wait times and offer definitive answers faster.


We're putting sustained attention back on community contributions. That means faster first responses on new connector PRs and a clearer timeline between open to closed PR. The six connectors in this post are the first of many.


As we review PRs, we're prioritizing the connectors that match our AI focus. Four of the six connectors move ad, spend, and performance data, and two move AI-usage data. These are the tools that build a clearer picture of what companies spend across channels and what your AI tools cost.


If you have a connector sitting in the queue, you can always[connect with us on Slack](https://airbytehq.slack.com/join/shared_invite/zt-3brj7lzsx-nVFc49_BIBTwR8Qo2isX7Q#/shared-invite/email) for updates. We're working through the backlog, and we want to know if you're still waiting on a review.


## Airbyte's connector review priorities


Airbyte's data replication platform hosts more than 600 connectors moving production data for thousands of companies. This platform has allowed us to understand how to build better context for Airbyte Agents. An agent is only as good as the data it can access, and reaching data at scale means handling auth, schema changes, and hundreds of small API quirks that our connectors allow you to abstract away from. Every source the community adds is one more system an agent can use without the guesswork.


Community contributors extend the reach of the platform. Airbyte reviews and ships work quickly without sacrificing quality. Every team building on Airbyte, whether they're loading a warehouse or feeding an agent, gets more of the data they need in the right form. The more the community broadens the catalog, the more connected your data gets, and the more information agents derive from it.


## Build with us


If there's a source you wish connected to Airbyte, the fastest path is often to build it. The Connector Builder gets you from an API to a working connector through guided YAML generation. These YAML files are easiest to review and approve; the Connector CDKs allow you to build more complex source and destination connections. You can find more information in[the Connector Development docs](https://docs.airbyte.com/platform/connector-development) .


Thanks to everyone who contributed these, and to everyone whose pull request is next. We're paying attention and ready for your next connector. Find out more and[contribute to Airbyte](https://docs.airbyte.com/community/contributing-to-airbyte) .
