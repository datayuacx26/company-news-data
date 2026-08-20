---
schema_version: "1.0.0"
document_id: "39058aaff2147b23789e631e6fe41e4dc3bb8b9ed319b41b286750960ec819f6"
company_key: "yc-stripe"
company: "Stripe"
source_id: "yc-stripe-dev-blog-rss"
canonical_url: "https://stripe.dev/blog/how-stripe-uses-graph-search-and-state-machines-to-auto-remediate-a-global-database-fleet"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-19T22:29:11.601208+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:218544730d1ce8d8c28b7af9cc54fc0d5a6978f8d00f280bc2fa1b2e97aeb7f6"
---

# How Stripe uses graph search and state machines to auto-remediate a global database fleet

# How Stripe uses graph search and state machines to auto-remediate a global database fleet


/


Metadata


Date:


2026.7.16


Authors:


[Pragya Mehta](https://stripe.dev/authors/pragya-mehta)[Sai Samant](https://stripe.dev/authors/sai-samant)


Reading time:


8 min read


Categories:


Engineering


Share:


[Twitter/X](https://twitter.com/intent/tweet?url=https://stripe.dev/blog/how-stripe-uses-graph-search-and-state-machines-to-auto-remediate-a-global-database-fleet&text=How%20Stripe%20uses%20graph%20search%20and%20state%20machines%20to%20auto-remediate%20a%20global%20database%20fleet)[LinkedIn](http://www.linkedin.com/shareArticle?mini=true&url=https://stripe.dev/blog/how-stripe-uses-graph-search-and-state-machines-to-auto-remediate-a-global-database-fleet&title=How%20Stripe%20uses%20graph%20search%20and%20state%20machines%20to%20auto-remediate%20a%20global%20database%20fleet&summary=Discover%20how%20we%20modeled%20our%20MongoDB%20infrastructure%20as%20a%20traversable%20graph%20and%20then%20use%20pathfinding%20algorithms%20to%20dynamically%20compute%20and%20execute%20recovery%20plans.%20This%20automated%20approach%20reduced%20pager%20volume%20by%2030%25%20(~200%20pages%2Fyear)%2C%20eliminated%2012%20days%20of%20unhealthy%20shard%20states%20annually%2C%20and%20supports%20new%20shard%20layouts%20with%20zero%20manual%20effort.)


/


Article


## About the authors


/


About the authors


### Pragya Mehta


Pragya Mehta is an engineer on the Document Database Control Plane Platform team at Stripe


### Sai Samant


Sai Samant is a technical writer at Stripe working across the engineering organization.


- [Extending Stripe’s network with stablecoins](https://stripe.dev/blog/extending-stripes-network-with-stablecoins)
- [How we built Stripe Credits: A programmable, auditable way to pay your Stripe fees](https://stripe.dev/blog/how-we-built-stripe-credits-a-programmable-auditable-way-to-pay-your-stripe-fees)


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


[Extending Stripe’s network with stablecoins Most of the world's money still moves in batches. A wire submitted at 2pm might settle by end of day, if you made the cutoff. ACH takes one to three business... Engineering Crypto](https://stripe.dev/blog/extending-stripes-network-with-stablecoins)


\[ Fig. 2 \]


10x


[How API changes flow into Stripe's developer products When a Stripe engineer makes changes to an API, merging the PR is just the beginning. Stripe maintains a vast developer product suite that needs... Engineering](https://stripe.dev/blog/how-api-changes-flow-into-stripes-developer-products)


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
