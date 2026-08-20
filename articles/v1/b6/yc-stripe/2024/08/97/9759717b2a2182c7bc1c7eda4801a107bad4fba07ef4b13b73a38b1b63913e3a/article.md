---
schema_version: "1.0.0"
document_id: "9759717b2a2182c7bc1c7eda4801a107bad4fba07ef4b13b73a38b1b63913e3a"
company_key: "yc-stripe"
company: "Stripe"
source_id: "yc-stripe-dev-blog-rss"
canonical_url: "https://stripe.dev/blog/peeking-under-the-hood-of-stripe-invoicing"
published_at: "2024-08-26T00:00:00+00:00"
first_seen_at: "2026-07-19T22:29:11.601208+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:a6e6fcf5c7c5d02a69ddb9bd56cd47d59d6d77701fa80fcfd91df2bb022106df"
---

# Peeking under the hood of Stripe Invoicing

# Peeking under the hood of Stripe Invoicing


/


Metadata


Date:


2024.8.26


Author:


[David Edoh-Bedi](https://stripe.dev/authors/david-edoh-bedi)


Reading time:


6 min read


Categories:


Workbench


Billing


Share:


[Twitter/X](https://twitter.com/intent/tweet?url=https://stripe.dev/blog/peeking-under-the-hood-of-stripe-invoicing&text=Peeking%20under%20the%20hood%20of%20Stripe%20Invoicing)[LinkedIn](http://www.linkedin.com/shareArticle?mini=true&url=https://stripe.dev/blog/peeking-under-the-hood-of-stripe-invoicing&title=Peeking%20under%20the%20hood%20of%20Stripe%20Invoicing&summary=Stripe%20Invoicing%20offers%20a%20no-code%20solution%20for%20sending%20invoices%20to%20customers.%20Because%20this%20option%20handles%20the%20complexity%20of%20all%20underlying%20API%20calls%2C%20developers%20sometimes%20struggle%20to%20understand%20the%20different%20phases%20a%20Stripe%20invoice%20goes%20through%2C%20which%20is%20problematic%20when%20attempting%20to%20debug%20payment%20failures.%20)


/


Article


## About the author


/


About the author


### David Edoh-Bedi


After starting his Stripe journey as an integration engineer helping large users build and scale their payment solutions, David pivoted to his current developer advocate role. He began his professional career as an engineer at Microsoft, working on various aspects of the Windows operating system. He’s passionate about all things data and connecting with developers from around the world. Outside of tech, he’s a huge soccer fan, avid reader, travel addict, and amateur triathlete.


- [Testing Connect onboarding with Sandboxes](https://stripe.dev/blog/testing-connect-onboarding-with-sandboxes)
- [Crush errors with Sandbox testing](https://stripe.dev/blog/crush-errors-with-sandbox-testing)
- [Debugging your Stripe Invoicing integration with Workbench](https://stripe.dev/blog/debugging-your-stripe-invoicing-integration-with-workbench)


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


[Debugging your Stripe Invoicing integration with Workbench With Stripe Invoicing, you can create and manage invoices for one-time and recurring payments. Whether caused by infrastructure issues or coding... Workbench Billing](https://stripe.dev/blog/debugging-your-stripe-invoicing-integration-with-workbench)


\[ Fig. 2 \]


10x


[Developing and investigating subscription data flow This article shows you how to use Stripe's sandbox to simplify your development process. You'll learn to create isolated test environments, simulate... Workbench Billing Sandboxes](https://stripe.dev/blog/developing-and-investigating-subscription-data-flow)


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
