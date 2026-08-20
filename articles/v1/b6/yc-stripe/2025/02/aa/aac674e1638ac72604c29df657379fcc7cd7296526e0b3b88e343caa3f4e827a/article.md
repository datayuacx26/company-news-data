---
schema_version: "1.0.0"
document_id: "aac674e1638ac72604c29df657379fcc7cd7296526e0b3b88e343caa3f4e827a"
company_key: "yc-stripe"
company: "Stripe"
source_id: "yc-stripe-dev-blog-rss"
canonical_url: "https://stripe.dev/blog/building-serverless-usage-notification-with-aws"
published_at: "2025-02-27T00:00:00+00:00"
first_seen_at: "2026-07-19T22:29:11.601208+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:6c5394246666cae503982400afe8d489b726021859e560bb1ff8f91e4b0c8f3f"
---

# Building serverless usage notification with AWS

# Building serverless usage notification with AWS


/


Metadata


Date:


2025.2.27


Author:


[Hidetaka Okamoto](https://stripe.dev/authors/hidetaka-okamoto)


Reading time:


6 min read


Categories:


AWS


Billing


Event Destinations


Share:


[Twitter/X](https://twitter.com/intent/tweet?url=https://stripe.dev/blog/building-serverless-usage-notification-with-aws&text=Building%20serverless%20usage%20notification%20with%20AWS)[LinkedIn](http://www.linkedin.com/shareArticle?mini=true&url=https://stripe.dev/blog/building-serverless-usage-notification-with-aws&title=Building%20serverless%20usage%20notification%20with%20AWS&summary=In%20this%20article%2C%20we%20will%20guide%20you%20on%20how%20to%20set%20up%20usage%20threshold%20alerts%20based%20on%20consumption%20for%20customers%20with%20pay-as-you-go%20plans.%20By%20integrating%20AWS%27s%20serverless%20tools%20with%20Stripe%2C%20you%20can%20create%20a%20streamlined%20and%20easy-to-manage%20notification%20system%20with%20minimal%20coding.)


/


Article


## About the author


/


About the author


### Hidetaka Okamoto


Hide (ひで pronounced “Hee-Day”) is a former Developer Advocate at Stripe and maintainer of the Stripe Testing Tools MCP Server.


- [Importing sales data from Stripe into AWS](https://stripe.dev/blog/importing-sales-data-from-stripe-into-aws)
- [Japan community highlights: Effective testing and security](https://stripe.dev/blog/japan-community-highlights-2024-09)
- [Developing and investigating subscription data flow](https://stripe.dev/blog/developing-and-investigating-subscription-data-flow)
- [Managing SaaS access control with Stripe’s Entitlements API](https://stripe.dev/blog/managing-saas-access-control-with-stripe-entitlements-api)
- [Optimize payment flow while reducing code complexity with Stripe's A/B Testing](https://stripe.dev/blog/optimize-payment-flow-reduce-complexity-stripe-ab-testing)
- [Easily debug your 3DS authentication with Stripe Workbench](https://stripe.dev/blog/easily-debug-your-3ds-authentication-with-stripe-workbench)


/


Additional resources


- [Subscribe to Stripe Developers on YouTube.](https://www.youtube.com/stripedevelopers)
- [Check out the docs for the in-depth developer guidance.](https://docs.stripe.com/)
- [Join the Stripe Discord server to chat live with other developers.](https://discord.com/invite/RuJnSBXrQn)
- [Join a local Stripe Developer Meetup to learn about the latest features and network with your community.](https://www.meetup.com/pro/stripe/)


/


Related Articles


\[ Fig. 1 \]


10x


[Building resilient webhook handlers in AWS: Implementing DLQs for Stripe events Discover how to build reliable webhook handlers for Stripe events using AWS in this comprehensive guide. Learn about the challenges of processing... AWS Event Destinations Best Practices](https://stripe.dev/blog/building-resilient-webhook-handlers-aws-dlqs-stripe-events)


\[ Fig. 2 \]


10x


[Building production-ready Stripe subscriptions using Kiro powers Learn how to build production-ready Stripe subscriptions faster using Kiro Powers. Implement secure payment flows, webhooks, and scalable... AWS Partners Billing](https://stripe.dev/blog/building-production-ready-stripe-subscriptions-kiro-powers)


/


Docs


Explore our guides and examples to integrate Stripe.


[Learn more](https://docs.stripe.com/)


/


Social


[Youtube](https://www.youtube.com/stripedevelopers)[Twitter/X](https://x.com/stripedev)[Discord](https://discord.com/channels/841573134531821608/841573134531821612)


/


Resources


[Docs](https://docs.stripe.com/)[Developer Meetups](https://www.meetup.com/pro/stripe/)


© 2026 Stripe, Inc.


[Privacy](https://stripe.com/privacy)[Legal](https://stripe.com/legal)[Stripe.com](https://stripe.com/)
