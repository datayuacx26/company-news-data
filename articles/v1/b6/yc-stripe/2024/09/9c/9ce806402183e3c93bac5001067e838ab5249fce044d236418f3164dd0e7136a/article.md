---
schema_version: "1.0.0"
document_id: "9ce806402183e3c93bac5001067e838ab5249fce044d236418f3164dd0e7136a"
company_key: "yc-stripe"
company: "Stripe"
source_id: "yc-stripe-dev-blog-rss"
canonical_url: "https://stripe.dev/blog/upgrading-your-stripe-plugin-security"
published_at: "2024-09-19T00:00:00+00:00"
first_seen_at: "2026-07-19T22:29:11.601208+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:44e3af45c96cdd4b493210f3f303f99ef13d2e753e8303d0241adf324c3d2c2b"
---

# Upgrading your Stripe plugin security

# Upgrading your Stripe plugin security


/


Metadata


Date:


2024.9.19


Author:


[Ben Smith](https://stripe.dev/authors/ben-smith)


Reading time:


4 min read


Categories:


Best Practices


Security


Share:


[Twitter/X](https://twitter.com/intent/tweet?url=https://stripe.dev/blog/upgrading-your-stripe-plugin-security&text=Upgrading%20your%20Stripe%20plugin%20security)[LinkedIn](http://www.linkedin.com/shareArticle?mini=true&url=https://stripe.dev/blog/upgrading-your-stripe-plugin-security&title=Upgrading%20your%20Stripe%20plugin%20security&summary=Secure%20your%20Stripe%20integrations%20by%20ditching%20unrestricted%20secret%20keys%E2%80%94learn%20how%20to%20protect%20merchant%20accounts%20with%20restricted%20access%20API%20keys%20and%20OAuth%202.0%2C%20and%20avoid%20compliance%20fees.)


/


Article


## About the author


/


About the author


### Ben Smith


Ben is a Staff Developer Advocate at Stripe, based in the UK. Previously, he was a Principal Developer Advocate at AWS, specializing in serverless architecture. With a background in web development, he is passionate about empowering developers through knowledge sharing and community engagement, making complex technologies accessible to all.


- [What it feels like building with Stripe Projects](https://stripe.dev/blog/what-it-feels-like-building-with-stripe-projects)
- [Introducing Stripe Workflows: Tailoring Payments to Your Business Needs](https://stripe.dev/blog/introducing-stripe-workflows)
- [Because nobody likes being charged twice](https://stripe.dev/blog/because-nobody-likes-being-charged-twice)
- [How do I store inventory data in my Stripe application](https://stripe.dev/blog/how-do-i-store-inventory-data-in-my-stripe-application)
- [Data access patterns for simple Stripe integrations](https://stripe.dev/blog/data-access-patterns-for-simple-stripe-Integrations)
- [Growing your Stripe integration With Event Destinations](https://stripe.dev/blog/growing-your-stripe-integration-with-event-destinations)
- [Choosing the right sandbox strategy for your organization](https://stripe.dev/blog/choosing-the-right-sandbox-strategy-for-your-organization)
- [Avoiding test mode tangles with Stripe Sandboxes](https://stripe.dev/blog/avoiding-test-mode-tangles-with-stripe-sandboxes)
- [Advanced error handling patterns for Stripe enterprise developers](https://stripe.dev/blog/advanced-error-handling-patterns-for-Stripe-enterprise-developers)
- [Simple error handling strategies with Stripe Workbench](https://stripe.dev/blog/simple-error-handling-strategies-with-stripe-workbench)
- [Bringing your Stripe objects to life with Workbench](https://stripe.dev/blog/bringing-your-stripe-objects-to-life-with-workbench)


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


[Scaling up your microservice testing with Apache Spark—Part 2 Some microservices are difficult to test because their behavior depends on a long tail of inputs that are hard to model by hand. This post focuses... Best Practices Testing](https://stripe.dev/blog/microservice-testing-with-apache-spark-part-2)


\[ Fig. 2 \]


10x


[Scaling up your microservice testing with Apache Spark Some microservices are difficult to test because their behavior depends on a long tail of inputs that are hard to model by hand. For that class of... Best Practices Testing](https://stripe.dev/blog/microservice-testing-with-apache-spark)


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
