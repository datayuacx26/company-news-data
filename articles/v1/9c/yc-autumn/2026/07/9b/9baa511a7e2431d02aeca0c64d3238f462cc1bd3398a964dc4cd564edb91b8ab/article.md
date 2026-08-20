---
schema_version: "1.0.0"
document_id: "9baa511a7e2431d02aeca0c64d3238f462cc1bd3398a964dc4cd564edb91b8ab"
company_key: "yc-autumn"
company: "Autumn"
source_id: "yc-autumn-news-import-b2979559dee3"
canonical_url: "https://useautumn.com/blog/how-mintlify-is-scaling-sales-led-gtm"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T18:43:44.752687+00:00"
fetched_at: "2026-07-29T18:43:46.659006+00:00"
content_hash: "sha256:6fdf9503bfc472f5fd3cfe8b4d68d5b4c0fa8ec0306739bd7a2a2df8597bca8e"
---

# How Mintlify is scaling sales-led GTM

Mintlify made developer documentation fun and beautiful to write, and doing so earned them intense, product-led product-market fit. As interest continues to arrive from larger and larger enterprises, they have rapidly built out a sales team, which today makes up close to half the company.


When a business-model shift like this happens in a company's lifecycle, a lot underneath it has to change: the operational processes that turn a signed deal into a provisioned customer, and the pricing model sitting behind them. This is how Mintlify handled the transition.


### What was broken


The problem was that there were two separate systems being held together by people:


1. Billing: a signed contract flowed from Salesforce to Stripe, from where invoices would get sent to customers
2. Enablement: provisioning the plan, custom entitlements and credit quotas.


These systems fell out of sync with each other frequently because these deals were often complex. The system they had built for self-service signups also had to handle custom plans, multiple deployments, enterprise pilot periods and ramp schedules. A sales or ops person needed to understand how everything worked together.


The process for sales was very, very disjointed; they basically had to have the billing completely separate from actually enabling a customer.


So there were times where enablement was missed, where a customer paid and they were like, 'hey, it's been a week, why don't I have my features?' And very frequently people would not have overage prices assigned in Stripe, so they just wouldn't get charged for them.


Kyle Finken


Engineering at Mintlify


As Mintlify moved upmarket, pricing changes came with it: new credit models and expansion options. But with each customer's state living partly in Stripe and partly spread across internal systems, making updates was slow and painful. They had a team of 2-4 engineers working on billing almost full-time.


### One source of truth


The solution was to combine billing and enablement into one model. A customer's plan became a single record that defines what they are charged, what they get, and when each of those applies. To apply a plan to a customer, all must be defined together.


before


stripe


salesforce


internal db


→


plan record


acme docs · enterprise


Charged


$2,500 / mo


$0.02 / credit over


no seat price


Get


100k ai credits


3 deployments


∞ editor seats


When


30d pilot


m3 ramp


12mo term


The temporal aspect is important and typically overlooked. It means that pilot periods can expire, enterprise plan trials can revert back to previous states after completion, and ramp schedules can automatically adjust quotas.


It also narrows what sales can set up to configurations that are actually billable.


> We effectively lock people down to a specific set of things that we have approved for actually setting up for billing... if this doesn't fit within the constraints we've set, you can't charge for it.
>
>
> I would prefer to go through the pain of finding a workaround for a one-off customer than loosen everything for everyone and then run into a ton of data issues down the road.


### A hierarchical account model


A single customer can have many deployments, and each one can sit on a different plan, whether hobby, pro, or enterprise, bought either through self-serve or negotiated as part of a contract.


Previously, that complexity carried into the billing: deployments, overage prices, and add-ons could each live in their own subscription, so one account could be split across several subscriptions and invoices.


The account model now follows how customers are actually organized. Deployments still carry their own plans, but they roll up to one customer, billed as a single subscription with one monthly invoice.


Entitlements can be segregated by deployment, but credit quotas are pooled at the customer level, so usage draws from a shared balance even when the deployments underneath are on different plans.


subscription


monthly


invoice


docs · enterprise


$2,500


api-ref · pro


$250


credit overage


$240


total


$2,990


→


customer · acme


100k pooled credits


docs


enterprise


custom domain


sso


60k credits


api-ref


pro


analytics


—


30k credits


internal


hobby


—


—


10k credits


The change was moving from a single-deployment assumption to a multi-deployment hierarchy, which meant giving the data model more thought up front: what belongs to a deployment, what belongs to the customer above it, and how billing and usage aggregate across the two.


### Automating provisioning


Today, when a Mintlify deal closes in Salesforce, it posts into a Slack channel as a closed-won message with the account, the org, and the contract attached. An AI agent picks that up, reads the contract, and provisions the plan.


In one API call, it determines the shape of the plan, adds custom credit amounts and entitlements, backdates the subscription when needed, and sends an invoice. The rules that used to live in a salesperson's head over multiple steps are now written down as instructions the Slack agent follows. A simplified version looks like this:


```text
🎉 <Customer> closed-won! Provision this in Autumn from the attached contract.
- Parse the contract for any custom credit amounts or entitlements
- Backdate the subscription to the contract signing date if it differs from today.
- Attach the plan to a specific deployment, not to the customer.
- For Enterprise, enable the plan immediately rather than after payment.
- If the contract lists multiple deployments, confirm the same number exists in Autumn. If they do not match, flag a human.
- (+ a few more rules)


```


When anything is ambiguous, like a missing signature or a mismatch in the deployment count, it updates billing and flags a person rather than provisioning on a guess.


People do still step in to correct off states. Plans occasionally get cancelled by accident, attached to the wrong deployment, or left in a legacy state from before the migration. However, the level of automation is much higher as the agent can intelligently parse the account data model, pick out any custom terms, and apply them in a single API call.


### Repricing for expansion


As Mintlify moved up-market, they reworked how the product is priced with two aims: make the model simpler to run, and tie expansion to how customers actually get value.


They started by reworking their usage model. Pricing started as a flat $0.25 per assistant message, which only covered that one feature. As Mintlify added more AI surfaces, they moved to a single AI credit that customers can spend across the docs agent, workflows, and the slackbot, so one balance covers usage wherever it happens rather than a separate price per feature.


They also completely dropped seat-based pricing. Charging per seat is a common way to grow a contract, but Mintlify's value sits in usage rather than headcount, so they made editor seats unlimited and let accounts expand through credits and usage instead.


With the tiers cleaned up and consistent, sales had a defined and explainable ladder to move customers up.


### How Autumn helped


The result is that billing stopped being a constraint on growth. Sales can structure a custom deal and have it provisioned the same day. Pricing and packaging can change as the market teaches them what to charge, without rebuilding billing each time. Customers are billed and provisioned more predictably, with cleaner underlying data.


Instead of building this themselves, Mintlify worked with Autumn to put billing, entitlements and usage into our platform. That meant they got:


- A single source of truth for customer state across PLG and enterprise, queryable in real-time from their app.
- Trials, pilots, schedules and other complex deals handled out the box.
- A parent-child account model to manage deployment-level entitlements under a single customer subscription.
- Autumn team support to handle credit and seat pricing changes and other experiments.


Our experience has been great. I would literally recommend it to every person who asked.


Since the migration, I have a lot more confidence that customers are actually getting what they're paying for.


I have incredibly high trust in you guys right now. We put feature requests into a lot of vendors that take a long time, and it never does with you.


Kyle Finken


Engineering at Mintlify


We're proud to support the Mintlify team as they continue to scale beyond Series B and all the way past IPO!
