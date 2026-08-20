---
schema_version: "1.0.0"
document_id: "ffdf1dacf063ccf76992dd142d4801151753bc6c2e43d420a065b43ddb76de50"
company_key: "yc-stripe"
company: "Stripe"
source_id: "yc-stripe-dev-blog-rss"
canonical_url: "https://stripe.dev/blog/fund-segregation-stripe-connect"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-19T22:29:11.601208+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:d1b77ab6a37ede72ef6cefacd7b2a48c73c0775f6c676ba6c4ec66da08d500ea"
---

# Keeping funds where they belong: Implementing fund segregation in Stripe Connect

# Keeping funds where they belong: Implementing fund segregation in Stripe Connect


/


Metadata


Date:


2026.7.02


Author:


[Andrew Robinson](https://stripe.dev/authors/andrew-robinson)


Reading time:


6 min read


Categories:


Connect


Share:


[Twitter/X](https://twitter.com/intent/tweet?url=https://stripe.dev/blog/fund-segregation-stripe-connect&text=Keeping%20funds%20where%20they%20belong:%20Implementing%20fund%20segregation%20in%20Stripe%20Connect)[LinkedIn](http://www.linkedin.com/shareArticle?mini=true&url=https://stripe.dev/blog/fund-segregation-stripe-connect&title=Keeping%20funds%20where%20they%20belong%3A%20Implementing%20fund%20segregation%20in%20Stripe%20Connect&summary=Learn%20how%20to%20implement%20fund%20segregation%20in%20Stripe%20Connect%20using%20allocated%20balances%20to%20ring-fence%20seller%20funds%20from%20your%20platform%27s%20operational%20balance%20-%20covering%20payment%20creation%2C%20transfers%2C%20refunds%2C%20disputes%2C%20and%20preparing%20for%20upcoming%20PSD3%20regulatory%20requirements.)


/


Article


## About the author


/


About the author


### Andrew Robinson


Andrew Robinson is a Solutions Architect at Stripe.


- [Stop juggling multiple POS devices with Stripe Terminal](https://stripe.dev/blog/stripe-terminal-on-screen-input-collection)
- [Tracking customer spend in an omnichannel or multiprocessor environment](https://stripe.dev/blog/tracking-customer-spend-omnichannel-multiprocessor-environment)
- [Enhance your monitoring by integrating Stripe events with AWS CloudWatch Log Groups](https://stripe.dev/blog/enhance-your-monitoring-by-integrating-stripe-events-with-aws-cloudwatch-log-groups)


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


[Using Connect embedded components to streamline your Connect onboarding flow Replace hosted onboarding with embedded Connect components. Keep users on your platform while collecting requirements and enabling payments..... Connect](https://stripe.dev/blog/connect-embedded-components-streamline-onboarding)


\[ Fig. 2 \]


10x


[Testing Connect onboarding with Sandboxes As a platform, you can accelerate your Connect development with Sandboxes, by replicating your live settings to test out the merchant onboarding... Connect Sandboxes Testing](https://stripe.dev/blog/testing-connect-onboarding-with-sandboxes)


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
