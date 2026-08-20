---
schema_version: "1.0.0"
document_id: "b53d5f70e9f6ec3da20d0be4cfbad7165e9928f61fb2d3c10e97bcfc4e29b761"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/aws-marketplace-contract-vs-usage-pricing/"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-10T02:28:43.687936+00:00"
fetched_at: "2026-08-10T02:28:45.704186+00:00"
content_hash: "sha256:e0b67572b175517e3eea2381c21094048ea3bb3529082ba32fec08526058699f"
---

# AWS Marketplace Contract Pricing vs Usage Pricing

*AWS Marketplace contract pricing bills a buyer upfront (or on a payment schedule) for a fixed quantity of entitlement, which your software verifies but never meters. Usage pricing — what AWS calls SaaS subscriptions — bills the buyer from metering records your software sends every hour. The two models put the billing burden in completely different places.*


---


Most teams pick a marketplace pricing model the way they pick a database: quickly, on the basis of what the sales team asked for, and permanently.


The permanence is the part that catches people. In AWS’s own words: “Once you create your listing and publish it to limited, you can’t change the pricing model.” Not “it’s difficult.” You create a new listing, and every buyer who found the old one is on the old one.


So it is worth understanding what the choice actually commits your engineering team to — because the commercial difference between the two models is small, and the architectural difference is enormous.


---


## **What pricing models does AWS Marketplace support for SaaS?**


AWS Marketplace supports four SaaS pricing models, and the names matter because they map to different integrations:


- **SaaS subscriptions** — “A pay-as-you-go model where we bill buyers for their hourly usage of your SaaS product.”
- **SaaS contracts** — “Buyers are either billed in advance for the use of your software, or you can offer them a flexible payment schedule. Customers can also pay for additional usage above their contract.”
- **SaaS contracts with pay-as-you-go** — the same upfront billing, plus “an additional metered rate for usage on top of the contract price.”
- **SaaS free** — every pricing dimension set to $0.00.


In everyday conversation people say “contract” and “usage.” AWS says contracts and subscriptions. The distinction that matters is not how the buyer pays. It is which API your software has to call.


---


## **The architectural difference: entitlements vs metering**


A contract product reads entitlements. A subscription product writes metering records. That single sentence is the whole design decision.


**With SaaS contracts** , AWS is unusually blunt: “When using the SaaS Contract pricing model, your application never sends metering records. Instead, it verifies entitlement by calling the AWS Marketplace Entitlement Service.” The buyer has purchased a defined quantity — 50 users, 10 TB, three tiers of feature access — and your job is to ask AWS what they bought and enforce it.


**With SaaS subscriptions** , the burden inverts. “AWS Marketplace bills your customers based on the metering records that you send to us. All charges must be measured and reported every hour from the software deployed in the customer’s account.” Then the sentence that should be printed and stuck to a wall:


> “Our ability to bill customers for usage of your product is dependent upon receiving metering records from you. You are responsible for ensuring that your product’s metering records are successfully transmitted and received.”


There is no reconciliation process that recovers unmetered usage after the fact. An hour your software failed to report is an hour nobody bills. On a subscription product, your metering pipeline *is* your revenue pipeline, and it needs the monitoring you would give a payment processor.


**Contracts with pay-as-you-go** is the hybrid, and it is the one people misread. Your software calls the Entitlement Service for the committed quantity *and* the Metering Service for anything above it. Overage is metered; the entitlement is not. Meter the whole usage figure instead of the delta and you will bill the customer twice for what they already paid upfront.


---


## **How the two models behave in the deal**


SaaS contracts SaaS subscriptions


Buyer pays Upfront, or on a defined payment schedule Monthly, in arrears, calculated from metering


Your software calls Entitlement Service Metering Service, every hour


Durations offered Monthly, 1 year, 2 years, 3 years — and up to 144 months on a private offer Ongoing until the buyer unsubscribes


Overage Optional pay-as-you-go per dimension, above the entitlement Not applicable — everything is usage


Mid-term change Upgrade to higher value with prorated credit; **cannot decrease mid-term** Usage simply changes


Renewal Buyer can agree to automatic renewal, and can change quantity or duration at renewal No renewal event; the subscription continues


Revenue predictability Known at signature Known after the month closes


Two rows deserve a second look.


**“Cannot decrease mid-term.”** AWS: “Customers can upgrade a contract to one of a higher value… Customers can’t decrease the size of their existing contract. They can only decrease the size at renewal, or cancel their renewal.” That is a commercial feature, not a limitation — a contract product protects the floor of the deal in a way a usage product structurally cannot.


**Contract durations up to 144 months on a private offer.** Public listings are capped at monthly, 1, 2, or 3 years. A private offer can run a custom duration in months up to twelve years, which is how multi-year enterprise commitments get structured. Details on how those offers are built sit in the[AWS Marketplace seller’s guide](https://www.suger.io/resources/blog/aws-marketplace-for-sellers-complete-guide/) .


---


## **What happens at the end**


Both models end abruptly, and both give you exactly one hour.


When a **contract** expires, “Your SaaS product receives an` entitlement-updated` notification indicating the buyer’s entitlement has changed. The AWS Marketplace Entitlement Service returns an empty response.” Note what that means: the API does not error, it returns nothing. Software that treats an empty entitlement response as a transient failure will happily keep serving a customer who no longer has a contract. You then have one hour to meter any remaining overage.


When a **subscription** ends, your EventBridge bus receives a purchase agreement ended event, you have one hour to meter remaining usage, and then a` license deprovisioned - manufacturer` event arrives and metering for that customer is closed permanently.


One hour is not much of a window for a batch job that runs nightly. If your metering is batched, the end-of-agreement event has to be able to force an immediate flush.


---


## **Which one should you choose?**


Choose by the shape of your product and your buyer, not by the shape of your revenue target.


**Choose SaaS contracts when** the buyer thinks in seats, nodes, or capacity tiers; when procurement wants a fixed number on a purchase order; when the deal needs to draw down committed cloud spend in one predictable transaction; or when you have no reliable per-hour usage signal. Contracts are also far less engineering to ship — read an entitlement, cache it, enforce it.


**Choose SaaS subscriptions when** consumption genuinely varies and the customer would reject a commitment; when the product’s value scales with a metric the customer already watches; and when you can operate an hourly metering pipeline with alerting on failure.


**Choose contracts with pay-as-you-go when** you want a committed floor with elastic upside — the most common enterprise shape, and the one that needs the most careful metering logic because two systems are billing the same customer for the same product.


A practical filter: if your team cannot confidently answer “what happens to billing if our metering job fails for six hours on a Saturday,” you are not ready to operate a subscription product. Contracts fail safe. Subscriptions fail silent.


---


## **Frequently asked questions**


**What is the difference between AWS Marketplace contracts and subscriptions?** Contracts bill the buyer upfront for a fixed entitlement your software verifies through the Entitlement Service. Subscriptions bill from hourly metering records your software sends to the Metering Service. Contracts read; subscriptions write.


**Do I need to send metering records for a SaaS contract?** No — not for the contract itself. AWS states that under the SaaS Contract model your application never sends metering records. You only meter usage above the entitlement, and only if you offer pay-as-you-go overage.


**Can I change my AWS Marketplace pricing model after publishing?** No. AWS states that once you create your listing and publish it to limited, the pricing model can’t be changed. Switching means creating a new listing and migrating buyers.


**What contract durations does AWS Marketplace support?** Monthly, 1 year, 2 years, and 3 years on a public listing. On a private offer you can set a custom duration in months, up to 144 months.


**Can a customer reduce a contract mid-term?** No. Customers can upgrade to a higher-value contract and receive a prorated credit, but they can only decrease the size at renewal or cancel the renewal.


**What happens to metering when an agreement ends?** You get one hour. After a contract expires or a subscription ends, you have one hour to send any remaining metering records, after which metering for that customer is closed.


---


## **Takeaways**


- The pricing model is permanent. AWS does not let you change it after the listing is published to limited.
- Contracts read entitlements and never meter. Subscriptions meter hourly, and unreported usage is unbilled revenue you cannot recover.
- Contracts with pay-as-you-go meter only the overage above the entitlement. Metering total usage double-bills the customer.
- Contracts protect the floor: buyers can upgrade mid-term but can only shrink at renewal.
- Both models close metering one hour after the agreement ends. A nightly batch job will miss it.
- If you cannot answer what happens when metering fails for six hours, choose contracts.


---


The model you pick decides how much billing logic you own. See how[billing and metering in Suger](https://www.suger.io/platform/billing-metering/) handles entitlement checks, hourly metering, and overage across every marketplace from one integration — and how[cloud marketplace pricing models](https://www.suger.io/resources/blog/cloud-marketplace-pricing-models/) compare once Microsoft and Google Cloud are in scope.
