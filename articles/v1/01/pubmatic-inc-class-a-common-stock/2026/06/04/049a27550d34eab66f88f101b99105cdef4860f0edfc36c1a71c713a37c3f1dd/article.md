---
schema_version: "1.0.0"
document_id: "049a27550d34eab66f88f101b99105cdef4860f0edfc36c1a71c713a37c3f1dd"
company_key: "pubmatic-inc-class-a-common-stock"
company: "PubMatic Inc."
source_id: "pubmatic-inc-class-a-common-stock-news-import-2f81e182a3a2"
canonical_url: "https://pubmatic.com/blog/decision-fabric-how-we-put-partner-intelligence-inside-the-auction/"
published_at: "2026-06-01T12:00:33+00:00"
first_seen_at: "2026-07-24T13:12:14.280427+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:9022c1efcf9f648ccc8669cd224b352bd43045473df5ebc5f47e0f8489da4033"
---

# Decision Fabric: How We Put Partner Intelligence Inside the Auction

Programmatic advertising was built on a structural assumption that has quietly cost the industry billions. Decisioning belongs on the buy side. The DSP holds the brain. The supply path delivers the pipes.


The problem is that the richest signals and broadest access to inventory in programmatic transactions live on the supply side: full auction context, publisher data, content signals, real-time inventory dynamics and access to inventory before traffic shaping filters what buyers can see. By the time a bid request reaches a DSP, those signals are already compressed, filtered, and delayed with buyers seeing only a subset of the available opportunity set. Audience qualification happens on an incomplete picture and against a constrained pool of inventory, reduced to binary in-or-out decisions that cannot capture the full complexity of what makes an impression valuable, resulting in wasted spend, missed moments and performance that has never lived up to its promise.


As AI raises the bar for what decisioning models can do, the gap becomes impossible to ignore. More sophisticated models running on compressed, stale, buy-side-only data do not close it. They make it more expensive. The vision the industry has been circling for years is one where buy-side intelligence and sell-side signals converge at the moment of the transaction i.e., where qualification happens in real time, on the richest data available, at the exact point where inventory meets demand. Not upstream but live at auction.


Decision Fabric brings that vision to life.


#### What Changed


Decision Fabric is the real-time intelligence layer we built inside PubMatic’s platform. It allows partners, i.e., DSPs, curators, and algorithm companies, to deploy their proprietary decisioning models natively inside the auction, where they run inference on live bidstream signals at the exact moment inventory is available.


Not connected from the outside. Not processing a compressed bid request after it has traveled to an external system. Inside the auction. On the full signal. At bid speed.


The architecture is straightforward. The partner brings the model. PubMatic provides the data, the infrastructure, and the inventory scale to make that model perform. What that unlocks for buyers is something the industry has been trying to solve for years: audience qualification that reflects what is true right now, not what was true last week.


#### How It Works


Decision Fabric follows a three-step process: Train, Deploy, Activate.


**Train.** Partners begin with data. PubMatic delivers a privacy-safe sample of exchange-wide bidstream data, drawn from winning impressions across the US market to a partner-specified location. The dataset includes more than 60 OpenRTB fields: content signals, device type, geo, audience identifiers, and contextual attributes across web, mobile, app, and CTV inventory. All personal data is hashed before delivery. No raw identifiers leave PubMatic’s environment.


What makes this data valuable is not just its volume. It is its scale and composition. Partners training models on PubMatic’s bidstream are training on a materially larger and more representative pool of inventory and users than they ever had access to, before traffic shaping filters out large portions of auction traffic downstream. Supply-side bidstream data captures signals that are compressed or absent by the time a bid request reaches a DSP, i.e., full content context, publisher signals, auction dynamics.


**Deploy.** Partners package their trained model as a Docker container image, the industry-standard containerization format built on Open Container Initiative specifications. PubMatic hosts that container within its controlled infrastructure. The partner retains full control of what is inside, i.e., their decisioning logic, their proprietary IP, and their model architecture. PubMatic provides the environment, the quality controls, and the infrastructure to run it at exchange scale.


Current scope is one container per partner; US ad requests only. Traffic filters give partners precise control over what their model sees: platform selection across web, mobile web, mobile app, and CTV; publisher allow lists; and per-publisher sampling rates. Partners are not processing every impression indiscriminately. They are processing the impressions that matter to them.


**Activate.** At each qualifying auction, PubMatic routes the bid request through the partner’s container. The container receives the request, applies the partner’s decisioning logic, and returns a list of segment IDs, i.e., the audiences that qualify for this impression, in this moment, against live signals. Those segment IDs flow back into the auction and qualify the impression for the appropriate curated deals. Total response time: under ten milliseconds.


Because decisioning happens before traffic shaping, partner models see a materially larger pool of users and inventory than any DSP ever processes. That is not a marginal advantage. It is a structural one built into where Decision Fabric sits in the supply path.


#### Why Here


The environment matters as much as the model.


PubMatic’s infrastructure is owned and operated globally, built specifically for the latency and throughput demands of real-time programmatic decisioning. There are no cloud egress costs, no third-party dependencies, no latency penalties from infrastructure that was adapted for this rather than built for it. Partners running models here are running on infrastructure designed from the ground up for auction-speed inference.


The data scale compounds this. PubMatic processes 2.7 trillion advertiser bid requests per day across more than 100,000 streaming channels, apps, and websites on the open internet, including 28 of the top 30 streamers. Models trained and activated on PubMatic’s bidstream are operating on the most representative, diverse signal available anywhere in programmatic. The longer a model runs here, the more it learns. The more it learns, the better it performs.


For buyers, this translates directly into outcomes. Real-time audience qualification means campaigns reach the right people with greater precision. Media budgets work harder. Every dollar goes further across formats and channels.


#### It Gets Better


Partner models do not enter an empty environment. They enter one where AgenticOS, PubMatic’s agentic platform is already operating at scale. More than 1,000 AI-powered deals are active, with agents running across inventory discovery, deal management, and campaign optimization. Partner decisioning models work in concert with that existing intelligence. The combination produces outcomes that no single-layer system can replicate.


This is the beginning of the roadmap, not the end of it. Brand safety scoring and yield optimization are all on the path. Each new capability makes the decisioning layer more sophisticated. Each expansion compounds the performance advantage for buyers who are already running here.


#### Work With Us


Decision Fabric is live today with inPowered, with Chalice in onboarding and many additional partners in the pipeline through the end of 2026.


To participate, you need a decisioning model, the ability to package it as a Docker container, and a focus on the US market for the initial integration. The onboarding process starts with a discovery conversation with our product and partnerships team to confirm fit and walk through the data access and deployment framework.


If you are thinking about where your models should run and what environment gives them the best chance to perform, we want to talk.
