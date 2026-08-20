---
schema_version: "1.0.0"
document_id: "7c3042f99a963e42b0a6a5878bd93313092b44b1e1dcb52519189069cf668cf0"
company_key: "yc-dialect"
company: "Dialect"
source_id: "yc-dialect-news-import-4cb56bc42b2e"
canonical_url: "https://www.dialect.to/blog/presenting-the-universal-inbox"
published_at: "2025-07-07T14:06:00+00:00"
first_seen_at: "2026-07-21T16:13:21.108809+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:25be8030a3c72d8fbd860389b36a694702171d4d4fda6a12e77e6d0a8e7e2703"
---

# Presenting the Universal Inbox

We’re pleased today to introduce the Universal Inbox: a single API for integrating alerts & comms from across Solana, powered by the Alerts Stack.


As part of the launch, we’ve teamed up with Jupiter to host the very first Universal Inbox, live today in their mobile app as Jupiter Radar.


### Trusted by leading teams from Day 1


We’re also proud to share that eight leading Solana teams have teamed up with us on Day 1, joining Jupiter to deliver notifications via the Universal Inbox and Jupiter Radar:


- Raydium,
- DRiP,
- Kamino,
- Meteora,
- MarginFi,
- Lulo,
- Marinade,
- and Sanctum.


The Inbox for Web3 has arrived.


These projects are using the Inbox to send token alerts, DeFi updates, community announcements, and DAO governance messages directly to their users.


### Why this matters


Jupiter users now receive alerts for all their token and trading activity. The Universal Inbox is live today in Jupiter Mobile as Radar, providing:


- Transfer notifications
- Filled Trigger order alerts
- Recurring purchase confirmations
- Perps liquidation events
- Jupiter DAO voting updates


This level of integration is now available to any dApp that opts in, including yours.


Rolling out today with product, company & community announcements from 8 incredible teams, and a small set of personalized, programmatic alerts that we will grow carefully along with more teams in the coming days.


If you're managing your activity across multiple Solana apps, you've probably found yourself juggling a dozen tabs just to stay on top of updates. Most apps operate in silos, with no central way to receive notifications.


That changes with the Universal Inbox.


### What is the Universal Inbox?


The Universal Inbox is a cross-app, wallet-native notification API that delivers alerts from across the Solana ecosystem to a single interface. Wherever it’s integrated, on web or mobile, you can receive updates from any supported app.


It’s powered by Dialect’s Alerts Stack, which handles real-time delivery via push, email, and in-app feeds.


Imagine one feed for:


- Trending tokens & price changes,
- Portfolio updates, including earnings on your loans & staked SOL,
- New markets & APYs across DeFi,


and more. All of the above not only delivers to the Inbox, but also triggers Jupiter Mobile’s push notifications. Our Mobile Alerts Stack bundles Firebase Push, & lets you send mobile push notifications right to your users' wallets in any inbox app.


### How it works


User attention is precious, & so are notifications.


Our Alerts Stack comes with full user configuration, so users can opt in & configure exactly which teams, & topics, they want to hear about, wherever they are hearing about it.


#### 1. App Discovery & Subscription


Users can opt in or out of messages from supported apps, at any time.


- GET /v2/apps to browse
- POST /v2/subscribe or /unsubscribe to manage preferences


#### 2. Unified Message Feed


All messages are delivered to one place, with easy access and sorting.


- GET /v2/history and /summary
- Mark as read with POST /v2/history/read


#### 3. Cross-Channel Delivery


Choose your delivery method:


- Push (via Firebase)
- Email
- In-app feed embedded in the dApp


The Universal Inbox is fully interoperable with our Alerts Stack. Integrate alerts into your site, & anything your users receive & do there will translate to Jupiter Radar, & vice versa


It's your users' wallets. Their notifications should come with them.


It also serves as a new distribution channel for apps. Our Dashboard lets you get up & running in minutes with no-code broadcasts for company & product announcements.


Get listed in Jupiter Radar, grow your user base, & start communicating with them — all with zero code.


### For Developers


The Universal Inbox is a shared communication layer for all Solana apps.


- No-code broadcast dashboard to send alerts in minutes
- Easy integration via existing APIs
- Discoverability via Dialect’s ecosystem
- Wallet-based delivery that works across all surfaces


Get started today with:


- [App Registration Guide](https://docs.dialect.to/alerts/integrate-inbox/universal-inbox)
- [Authentication Docs](https://docs.dialect.to/authentication)
- [No-code Alerts Dashboard](https://docs.dialect.to/alerts)


### A better way to connect with users


The Universal Inbox is designed to serve both users and developers.


#### For users:


- One inbox for all notifications
- Consistent UX across dApps
- Full control over sources and delivery


#### For developers:


- Shared infra, no need to build from scratch
- Improved retention and re-engagement
- New distribution via partner apps like Jupiter


The inbox for web3 is here.
