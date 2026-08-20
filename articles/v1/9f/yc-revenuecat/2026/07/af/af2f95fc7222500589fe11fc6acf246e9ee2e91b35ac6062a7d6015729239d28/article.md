---
schema_version: "1.0.0"
document_id: "af2f95fc7222500589fe11fc6acf246e9ee2e91b35ac6062a7d6015729239d28"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/stripe-projects-cli"
published_at: "2026-07-15T17:21:52+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:c8f3a236c87543fad801c7af787964a4ac189c6003b37455e6f0372438074db8"
---

# We partnered with Stripe so your agent can monetize your app

Your coding agent can now set up RevenueCat, provision a project, generate API keys, and get your app ready to accept payments. The whole thing takes just one CLI command.


stripe projects add revenuecat/app


We built this with Stripe as part of[Stripe Projects](https://projects.dev/) .


## **Before this, monetization setup was still manual**


Every other piece of your stack provisions in seconds. Database? One command. Auth? One command. Hosting? Done. But *until now* , monetization required you to stop and spend an afternoon creating accounts, reading StoreKit documentation, figuring out which shared secret goes where, generating API keys, configuring sandbox environments, and setting up server-to-server notifications.


It takes hours. Sometimes days. And you haven't written a single line of product code yet.


That gap between "I have a working app" and "I can charge for it" is absurdly wide for something that should be a solved problem.


So we fixed it.


## One command, working credentials


If you're starting a new project today, here's what this looks like in practice.


You (or your agent) run one command. A few seconds later, you have a RevenueCat account, a configured project, and working API keys written directly to your` .env` file. You stay in the terminal the entire time.


$ stripe projects add revenuecat/app


✓ Created RevenueCat account
✓ Created project "my-app"
✓ Generated API keys
✓ Wrote REVENUECAT_APP_UUID to .env
✓ Wrote REVENUECAT_DASHBOARD_URL to .env
✓ Wrote REVENUECAT_SECRET_API_KEY to .env


Ready. Run \`stripe projects open revenuecat\` to view your dashboard.


Products, entitlements, and offerings can be configured from the dashboard later, or your agent can handle them via our[MCP server](https://www.revenuecat.com/docs/tools/mcp) .


## Agents provision it automatically


If you build in Cursor, Claude Code, or Warp, the provisioning step is something your agent handles between writing your UI code and wiring up the SDK. You don't type the command. You just say "add monetization."


You: "I want to add a $9.99/month subscription to this app."


Agent: \[runs stripe projects add revenuecat/app\]
Agent: \[reads .env, configures Purchases SDK\]
Agent: "Done. I've added the Purchases SDK to your project and
configured it with your API key. Want me to create a
product and build a paywall next?"


The agent doesn't need special RevenueCat knowledge. It picks up the Stripe Projects CLI and the skill files that` stripe projects init` already created. Monetization becomes a routine provisioning step, same as adding a database or setting up auth.


## Account, project, keys, full platform


When you run` stripe projects add revenuecat/app` , the CLI provisions:


- A RevenueCat account (or links to your existing one)
- A new project with sandbox and production environments
- API keys synced to your local` .env`
- Access to the full platform: paywalls, entitlements, offerings, analytics, webhooks, and our AI Toolkit for ongoing management


You'll still need Apple and Google developer accounts to go live on those stores. But RevenueCat itself (the account, the project, the API keys, the entitlement configuration) provisions from the terminal. And with RevenueCat's test store, you can test monetization on a real device without those store accounts.


From there, you integrate the[Purchases SDK](https://www.revenuecat.com/docs/getting-started/installation) for iOS, Android, React Native, Flutter, KMP, or web. That's the path from "I have an idea" to "I'm making money."


## Try it


1. Install the Stripe CLI with the Projects plugin:


stripe plugin install projects


1. Initialize your project and add RevenueCat:


stripe projects init my-app
stripe projects add revenuecat/app


1. Your` REVENUECAT_SECRET_API_KEY` is in` .env` . Integrate the SDK and start making money.


For the full lifecycle, check our[documentation](https://www.revenuecat.com/docs/getting-started/stripe-projects-quickstart) or ask your coding agent to handle product creation, paywall configuration, and entitlement management using the[RevenueCat AI Toolkit](https://www.revenuecat.com/blog/company/ai-toolkit/) .


This is a developer preview.[Give it a try](https://projects.dev/) and tell us what you think, especially if you're building with agents.
