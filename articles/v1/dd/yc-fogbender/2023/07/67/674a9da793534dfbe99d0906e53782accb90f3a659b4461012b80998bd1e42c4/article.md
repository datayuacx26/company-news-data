---
schema_version: "1.0.0"
document_id: "674a9da793534dfbe99d0906e53782accb90f3a659b4461012b80998bd1e42c4"
company_key: "yc-fogbender"
company: "Fogbender"
source_id: "yc-fogbender-news-import-46cf0bf99c36"
canonical_url: "https://fogbender.com/blog/fogbender-slack-customer-integration"
published_at: "2023-07-04T00:00:00+00:00"
first_seen_at: "2026-07-21T20:41:04.212025+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:04612880ca7319837e74d0fb3bfe867a507289087258959d658cf2cf018d0ad8"
---

# Customer Communication in Slack without Slack Connect or Shared Channels

In this article, we show how to use Fogbender to enable a customer team to receive support in Slack without using a Shared Channel or Slack Connect.


For a similar integration on the vendor side, check out[Using a single Slack channel to safely monitor B2B support traffic](https://fogbender.com/blog/using-a-single-slack-channel-to-safely-monitor-b2b-support-traffic) .


Scenarios where a customer needs to receive support in Slack without Slack Connect may initially come off as a clot of frivolous whims, but under certain circumstances, such a capability may be advantageous, or even necessary. Let’s ground this with a few concrete examples.


### # Scenario 1: Customer can’t pay for Slack


Your customer may be a large community whose primary real-time communication medium is a Slack workspace. The community has several thousand members and paying for the Slack workspace is out of the question, which precludes the possibility of using Slack Connect. Your customer invites you to this free workspace and creates a channel for your product or service, with the expectation that you’ll be there to respond.


### # Scenario 2: Customer stops paying for Slack


Your customer is an established company that recently hired a Chief Revenue Officer, who is scrutinizing recurring expenses in light of an ongoing industry-wide belt-tightening operation. Upon discovering that the company pays a nontrivial amount for both Microsoft Teams *and* Slack, and failing to see any discernible difference between the two, drops the axe on the Slack subscription, thereby severing your Shared Channel.


### # Scenario 3: Your company (vendor) stops paying for Slack


*Your* company’s Chief Revenue Officer axes *your* Slack subscription upon discovering that your company is paying for both Microsoft Teams and Slack, forcing you to disconnect your Shared Channels with customers.


### # Scenario 4: Your company (vendor) sets a floor on access to Shared Channels


Because maintaining Shared Channels for all customers turned out to be difficult and expensive to scale, your new company policy is to only offer Shared Channels to customers who are paying at least $1,000,000 per year. “The customer must pay us enough to hire a dedicated account manager to monitor their channel, otherwise they use email, like everyone else,” said your Chief Revenue Officer at an emotional all-hands, following a round of layoffs.


### # Scenario 5: Customer discovers crucial out-of-band communication via Slack Connect DMs


After a long silence, your customer champion brings up an issue in a Shared Channel and one of your customer-facing engineers responds with a mention of a customer-side Slack user, explaining that they were in DM communication over the past few months, and that the customer-side Slack user in question should be able to help. Furious, the customer champion says that the user in question is no longer with the company, and that the only reason they spun up the Shared Channel in the first place was to keep everyone - on both sides of the relationship - abreast of all the issues, and that this is entirely unacceptable.


### # Scenario 6: You (vendor) discover crucial out-of-band communication via Slack Connect DMs


A star engineer on your team goes to a tech meetup, and - long story short - accepts a job at an AI upstart making 3x - base - what you were paying. As soon as the engineer leaves, your customer Shared Channels light up with questions regarding the whereabouts of the star engineer, complaining that all the DMs with the engineer have disappeared, along with a trove of valuable information. Furious, your VP of Customer Support wants to know how this could have happened and weren’t Shared Channels there to prevent this sort of thing in the first place.


### # Fogbender Slack (Customer) integration to the rescue


The Fogbender Slack (Customer) integration provides a mechanism for your customer to designate a single Slack channel to talk to you - the vendor - without using Slack Connect.


Even if you don’t use Fogbender as your main customer support platform, you can take advantage the Fogbender Slack (Customer) integration to connect a customer Slack channel with your Slack workspace by enabling the Fogbender Slack (Agent) integration.


Note that you can try the Fogbender Customer (Slack) integration with your own Slack workspace by using a test customer from the[🕵️ Try a live demo!](https://fogbender.com/admin/-/-/settings/embed) feature.


### # Setting it up:


1.


Enable the Slack (Customer) integration for your workspace:[https://fogbender.com/admin/-/-/settings/integrations#comms-integrations](https://fogbender.com/admin/-/-/settings/integrations#comms-integrations)


2.


Open the[Triage room](https://fogbender.com/blog/what-are-customer-triage-rooms) of the customer who wants to chat in Slack


3.


Issue` @Slack (Customer) init` command in Triage


1. Have someone on the customer end click the **Connect to Slack** URL and follow instructions - they’ll eventually be redirected to a screen where they can name their channel and establish its visibility:


1. Once the customer successfully completes the connection process, you’ll see a message from the **Slack (Customer)** bot in the Triage room:


1. Test the connection:


Note that if you also[embed the Fogbender team messaging widget](https://fogbender.com/admin/-/-/settings/embed) on your customer-facing web dashboard, all messages, images, and files sent in Slack will appear on your web dashboard as well.
