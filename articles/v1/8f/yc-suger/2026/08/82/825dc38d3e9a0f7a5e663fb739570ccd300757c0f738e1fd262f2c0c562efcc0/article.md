---
schema_version: "1.0.0"
document_id: "825dc38d3e9a0f7a5e663fb739570ccd300757c0f738e1fd262f2c0c562efcc0"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/designing-metering-dimensions/"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-15T09:12:25.375763+00:00"
fetched_at: "2026-08-15T09:12:26.807099+00:00"
content_hash: "sha256:2fe699a9e93d73fc94cf70475accecb33aa91ecca0068f008c2ee77954342f7a"
---

# Designing Metering Dimensions You Won't Regret

*A metering dimension is the unit a marketplace bills your customer in — a seat, a node, a gigabyte, a run. It is the most consequential decision in a listing and the one most often made in an afternoon.*


---


Pricing is easy to change. You can run a promotion, issue a private offer at a different rate, or reprice next quarter.


Dimensions are not pricing. Dimensions are the vocabulary your price is expressed in, and they reach into your product, your billing system, your contracts and your customers’ budgets. Changing one after launch means every existing agreement is denominated in a unit you no longer use.


Most teams discover this about nine months in, when the first customer asks for a unit the listing cannot express.


---


## **What a dimension actually commits you to**


A dimension is a promise that you can count something, accurately, forever, in a way a customer will accept on an invoice.


That is three separate commitments and teams usually only check the first:


- **You can count it.** Your product emits the number.
- **You can count it *the same way twice* .** Two runs over the same period produce the same figure. If your count depends on when you ask, you have a support ticket, not a dimension.
- **A customer will accept it.** The unit maps onto something they believe they bought. “API calls” is defensible; “internal job executions” invites an argument every month.


The third is the one that generates disputes, because it is the only one that involves somebody else’s opinion.


---


## **What you cannot change later**


Two constraints from AWS Marketplace’s own documentation set the stakes.


**The pricing model is fixed at publication.** AWS states that[once you create your listing and publish it to limited, you can’t change the pricing model](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-pricing-models.html) . The choice between SaaS subscriptions, SaaS contracts, SaaS contracts with pay-as-you-go, and SaaS free is made once. Moving between them later means a new listing, and a migration for every existing customer.


**Trials inherit your dimensions.** For SaaS subscription free trials, the[product dimensions cannot be changed](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-free-trials.html) — the trial uses what the public offer uses. For contract free trials you can set quantity limits per dimension and add or remove dimensions. If your trial strategy depends on metering something differently, that constrains which pricing model you can pick.


The practical reading: **the reversible decisions are prices, and the irreversible ones are shapes.** Spend your design time on the shapes.


---


## **Five questions to answer before you publish**


# Question Why it bites later


1 What is the smallest unit a customer would recognise on an invoice? Dimensions coarser than the customer’s mental model feel arbitrary; finer ones feel like nickel-and-diming


2 Can you produce that number from production data, without a human? A dimension that needs a monthly spreadsheet is a dimension you will eventually stop reporting accurately


3 Does the number only ever go up within a period? Dimensions that can decrease mid-period force you to decide between peak, average and end-of-period — and to defend the choice


4 What happens when the customer’s usage is zero? Some dimensions should still bill at zero usage; if the contract expects a floor, that is a contract dimension, not a usage one


5 Would you be comfortable showing the customer the raw records behind the figure? If not, the dimension is not auditable, and the first dispute will be expensive


Question 3 is the one most often skipped. “Active users” sounds obvious until a customer deactivates 200 seats on the 20th and asks why they were billed for them.


---


## **Contract, usage, or both**


The pricing model decision is really a decision about who carries the risk of variance.


- **Contract** — the customer commits up front. Revenue is predictable for you, and the customer bears the risk of under-use. Procurement generally prefers this because it can be budgeted.
- **Usage (pay-as-you-go)** — the customer pays for what they use. They bear no commitment risk; you bear all the forecasting risk.
- **Contract with pay-as-you-go** — a committed floor plus metered overage. This is where most infrastructure-shaped products land, because it gives procurement a number and still captures growth.


The mechanics of the first two, and how entitlements differ between them, are covered in[Contract Pricing vs Usage Pricing](https://www.suger.io/resources/blog/aws-marketplace-contract-vs-usage-pricing/) .


One asymmetry worth naming: a contract dimension is checked at entitlement time, and a usage dimension is reported continuously. That means usage dimensions carry an operational burden every day of the agreement, and contract dimensions carry it once. Teams routinely underestimate the difference.


---


## **The operational tail nobody prices in**


Choosing a usage dimension signs you up for a reporting obligation with a deadline attached.


When a customer’s entitlement ends, AWS Marketplace gives sellers[one hour to submit final usage records](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-eventbridge-integration.html) using` BatchMeterUsage` . After that window, entitlements are revoked and the API rejects the usage — the last partial period is unbillable.


That single constraint rules out a whole class of architecture. If your usage numbers are produced by a nightly warehouse job, they are by construction too late for the final period of every churning customer. Metering has to be able to run on demand, triggered by an event, on data that is current.


The failure is silent and cumulative: no error, no alert, just a small, permanent shortfall against every cancellation. What that looks like when finance eventually reconciles it is covered in[Why Marketplace Numbers Never Match](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) .


---


## **A test for a dimension you will not regret**


Before publishing, write the sentence a customer’s finance lead would use to describe what they are buying. If the sentence needs a footnote, the dimension is wrong.


- “We pay per active seat per month.” — fine.
- “We pay per gigabyte scanned.” — fine.
- “We pay per workflow execution, where an execution is counted at the parent level unless the child runs in a separate namespace.” — that is not a dimension, that is a negotiation you will have every quarter.


Complexity in a dimension does not stay in the listing. It becomes support load, dispute load, and eventually a discount.


---


## **Frequently asked questions**


**Can I change my pricing model after listing on AWS Marketplace?** No. AWS states that once you create your listing and publish it to limited, you cannot change the pricing model. Moving between subscription, contract and contract-with-overage means a new listing.


**What SaaS pricing models does AWS Marketplace support?** Four: SaaS subscriptions billed on hourly usage, SaaS contracts billed in advance or on a payment schedule, SaaS contracts with pay-as-you-go overage, and SaaS free where all dimensions are set to zero.


**Can free trials use different dimensions from the paid offer?** Not for SaaS subscription free trials, where product dimensions cannot be changed. For SaaS contract free trials you can set quantity limits per dimension and add or remove dimensions.


**What makes a metering dimension hard to dispute?** It is reproducible and auditable: two runs over the same period give the same number, and you would be willing to show the customer the underlying records that produced it.


**Why can’t metering run as a nightly batch job?** Because the final reporting window after a customer’s entitlement ends is one hour. A nightly job is always too late for the last partial period of every customer who leaves.


---


## **Takeaways**


- Prices are reversible; dimensions and pricing models are not. Spend the design time on the shapes.
- A dimension is three commitments: you can count it, you can count it identically twice, and a customer will accept it.
- Dimensions that can decrease mid-period force a peak-versus-average decision you will have to defend.
- Usage dimensions carry a daily operational burden; contract dimensions carry it once.
- The one-hour final reporting window rules out nightly metering for anything usage-based.


Suger meters usage across every marketplace it supports from one set of records, reports it inside each platform’s window, and keeps the resulting invoices reconcilable against your own ledger. If you are designing dimensions for a first listing,[see how Suger handles billing and metering](https://www.suger.io/platform/billing-metering/) or[talk to our team](https://www.suger.io/contact-us/) .
