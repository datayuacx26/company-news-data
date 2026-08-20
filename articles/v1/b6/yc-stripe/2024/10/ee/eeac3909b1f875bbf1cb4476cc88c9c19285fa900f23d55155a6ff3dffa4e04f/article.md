---
schema_version: "1.0.0"
document_id: "eeac3909b1f875bbf1cb4476cc88c9c19285fa900f23d55155a6ff3dffa4e04f"
company_key: "yc-stripe"
company: "Stripe"
source_id: "yc-stripe-dev-blog-rss"
canonical_url: "https://stripe.dev/blog/easily-debug-your-3ds-authentication-with-stripe-workbench"
published_at: "2024-10-22T00:00:00+00:00"
first_seen_at: "2026-07-19T22:29:11.601208+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:e958504988f470834c78af1266850e6bbad81bf47504869cf7f3161edbdd3338"
---

# Easily debug your 3DS authentication with Stripe Workbench

# Easily debug your 3DS authentication with Stripe Workbench


/


Metadata


Date:


2024.10.22


Author:


[Hidetaka Okamoto](https://stripe.dev/authors/hidetaka-okamoto)


Reading time:


5 min read


Categories:


Workbench


Payments


Share:


[Twitter/X](https://twitter.com/intent/tweet?url=https://stripe.dev/blog/easily-debug-your-3ds-authentication-with-stripe-workbench&text=Easily%20debug%20your%203DS%20authentication%20with%20Stripe%20Workbench)[LinkedIn](http://www.linkedin.com/shareArticle?mini=true&url=https://stripe.dev/blog/easily-debug-your-3ds-authentication-with-stripe-workbench&title=Easily%20debug%20your%203DS%20authentication%20with%20Stripe%20Workbench&summary=In%20this%20article%2C%20you%27ll%20learn%20how%20to%20investigate%20the%20payment%20process%20with%20just%20a%20few%20clicks%20on%20the%20Stripe%20dashboard.%20You%27ll%20also%20see%20how%20to%20obtain%20event%20data%20for%20testing%20code%20related%20to%20the%203DS%20authentication%20flow.)


/


Article


## About the author


/


About the author


### Hidetaka Okamoto


Hide (ひで pronounced “Hee-Day”) is a former Developer Advocate at Stripe and maintainer of the Stripe Testing Tools MCP Server.


- [Importing sales data from Stripe into AWS](https://stripe.dev/blog/importing-sales-data-from-stripe-into-aws)
- [Building serverless usage notification with AWS](https://stripe.dev/blog/building-serverless-usage-notification-with-aws)
- [Japan community highlights: Effective testing and security](https://stripe.dev/blog/japan-community-highlights-2024-09)
- [Developing and investigating subscription data flow](https://stripe.dev/blog/developing-and-investigating-subscription-data-flow)
- [Managing SaaS access control with Stripe’s Entitlements API](https://stripe.dev/blog/managing-saas-access-control-with-stripe-entitlements-api)
- [Optimize payment flow while reducing code complexity with Stripe's A/B Testing](https://stripe.dev/blog/optimize-payment-flow-reduce-complexity-stripe-ab-testing)


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


[Testing subscriptions with Stripe Test Clocks and Workbench Validating payment logic that gets triggered over a period of time is challenging and sometimes results in inelegant solutions.The combination... Workbench Payments Testing Billing](https://stripe.dev/blog/testing-subscriptions-with-stripe-test-clocks-and-workbench)


\[ Fig. 2 \]


10x


[Bringing your Stripe objects to life with Workbench This post shows how to use the Stripe Workbench Inspector to examine the lifecycle of a PaymentIntent object.... Workbench Payments](https://stripe.dev/blog/bringing-your-stripe-objects-to-life-with-workbench)


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
