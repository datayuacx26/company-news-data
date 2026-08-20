---
schema_version: "1.0.0"
document_id: "d395ad6759906147708516b52c45749aeb978a78facf92e60af10f7946b6aa8a"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/cancel-aws-marketplace-subscription/"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-10T02:28:43.687936+00:00"
fetched_at: "2026-08-10T02:28:45.704186+00:00"
content_hash: "sha256:906cb79e97b7e71d136e3f2c1922d3fba0f835f511417c0bb073f00005054354"
---

# How to Cancel an AWS Marketplace Subscription

*How a buyer cancels on AWS Marketplace depends on what they bought. A SaaS subscription is cancelled by the buyer from the “Your Marketplace Software” page on the AWS Marketplace website. A SaaS contract requires a request through AWS Support — and any refund must be requested within 48 hours.*


---


A customer emails your support alias: *we want to cancel.*


Whoever picks that up has about four minutes before they have to say something specific, and the specific thing depends on a purchase detail they probably do not have to hand. Answer wrong and you either promise a refund that does not exist, or you send an enterprise buyer into a support queue they did not need.


This is the decision framework for that conversation — organised by what the buyer actually bought, because that is the only thing that determines the path.


---


## **How does a buyer cancel an AWS Marketplace subscription?**


For a **SaaS subscription** — the pay-as-you-go model billed from hourly metering — the buyer cancels themselves. AWS: “A customer can cancel their subscription to your SaaS subscription product \[on\] the **Your Marketplace Software** page of the AWS Marketplace website.”


What happens next, on your side, is a fixed sequence:


1. Your default Amazon EventBridge bus receives a purchase agreement ended event for that customer.
2. You have **one hour** to meter any remaining usage.
3. After that hour you receive a` license deprovisioned - manufacturer` event, and metering for that customer is closed permanently.


There is no negotiation window and no seller approval step. The buyer clicks; you get an event; you have an hour.


For a **SaaS contract** — the upfront-committed model — the path is different and much less discoverable. AWS: “Customers can request a cancellation and refund for SaaS contract products through AWS Support.” Two constraints attach:


- **“Customers must request refunds within 48 hours through AWS Support.”**
- “The full or prorated refund is typically granted in 3–5 business days.”


Forty-eight hours from the transaction. That is the single most consequential fact in this article, and almost nobody selling a contract product tells their customers about it.


---


## **The decision framework**


What the buyer bought Who initiates Where Refund available?


SaaS subscription (pay-as-you-go) Buyer ”Your Marketplace Software” page on the AWS Marketplace website No refund concept — billing simply stops after the final metered hour


SaaS contract (upfront) Buyer AWS Support Full or prorated, **if requested within 48 hours**


SaaS contract, past 48 hours Seller Seller self-service refund or cancellation Seller-initiated; a cancellation gives the buyer a 7-day response window


Contract renewal the buyer no longer wants Buyer Renewal settings on the agreement Not a refund — they cancel the renewal, and the current term runs out


CPPO (channel partner) deal Channel partner The channel partner initiates the request Follows the seller-initiated path, through the partner


The fourth row resolves more cancellation conversations than any other. A large share of “we want to cancel” emails are really *we do not want to pay for this again* , which is a renewal setting, not a cancellation. The customer keeps what they paid for until the term ends, and nothing is refunded because nothing needs to be. Ask which one they mean before you do anything else.


---


## **What to actually say**


Four scripts, in the order the cases occur.


**“We don’t want to renew.”** Confirm they mean the next term, not this one. Point them at the renewal settings on the agreement, where they can cancel the renewal or change quantity and duration. Then treat it as a renewal at risk with real runway —[marketplace renewals](https://www.suger.io/resources/blog/aws-marketplace-renewals/) covers the timeline. This is the best possible version of this conversation and it is worth steering toward it.


**“We bought yesterday and it was a mistake.”** If it is a contract, they are inside the 48-hour window and should raise it with AWS Support now. Say the number out loud: forty-eight hours. Do not let a buyer discover the window after it closes because your team was being reassuring.


**“We bought three months ago and want out.”** The buyer’s self-service refund path has closed. This is now a seller-initiated decision — you request the refund or cancellation yourself. Be careful with the promise: a refund on AWS Marketplace is irreversible once processed, and a cancellation is *proposed* to the buyer with a seven-day response window before it proceeds. The mechanics of that path are covered in[AWS Marketplace refunds and cancellations](https://www.suger.io/resources/blog/aws-marketplace-refunds-and-cancellations/) .


**“We just want to use less.”** On a contract, they cannot. AWS: “Customers can’t decrease the size of their existing contract. They can only decrease the size at renewal, or cancel their renewal.” Say that clearly and early — it is far better received in month two than in month eleven, and it turns into a renewal conversation instead of a cancellation one.


---


## **The rule sellers most often break**


AWS states it directly: “If a customer indicates that they want to cancel through your product, direct the customer to AWS Marketplace. To guarantee that there will be no future charges, customers should confirm the cancellation with AWS Marketplace.”


Disabling a customer’s account in your own product does **not** cancel their marketplace agreement. Billing continues. On a contract, the entitlement remains. On a subscription, the meter keeps running unless you also stop metering — and if you stop metering without the agreement ending, you have stopped billing for an agreement that is still active, which is its own problem.


The corollary is a product requirement, not a support one: when a customer asks to cancel inside your application, the correct behaviour is to tell them where the cancellation actually happens, and to notify them that the cancellation is in progress once the marketplace event arrives.


---


## **How Microsoft differs**


Worth knowing, because the same customer often buys on both.


Microsoft’s general position is that “the customer can cancel your offer at any time” — but there is a significant carve-out: “Subscriptions with multiyear contract durations and pending payments aren’t eligible for cancelation by customers. This restriction applies when the billing frequency of the purchase is monthly, annual, or has a flexible schedule as part of a private offer. To request cancelation in these scenarios, contact Marketplace support.”


And on committed terms: “For contract durations with equal payments, payment collection is enforced for the entire term and the standard refund policy applies.”


In practice a multi-year Microsoft subscription paid in instalments is harder for a buyer to walk away from than an equivalent AWS contract. If a customer holds both, do not assume the answer transfers.


---


## **What to fix so this is easier next time**


**Publish the 48-hour window** in your onboarding email for contract purchases. A buyer who learns it on day one is a buyer who does not learn it on day three.


**Handle the end-of-agreement event properly.** One hour is the whole metering window. If your metering is a nightly batch, an end-of-agreement event has to force an immediate flush — the pricing-model consequences are covered in[contract pricing vs usage pricing](https://www.suger.io/resources/blog/aws-marketplace-contract-vs-usage-pricing/) .


**Put the agreement’s renewal setting and expiry date in front of the account team.** Most cancellation requests are mistimed renewal conversations. They only look like cancellations because nobody saw the date coming.


**Do not treat cancellation as a support ticket.** It is a revenue event with a fixed clock, a legal counterparty, and an irreversible step in the middle.


---


## **Frequently asked questions**


**How do I cancel an AWS Marketplace subscription?** For a SaaS subscription, the buyer cancels from the “Your Marketplace Software” page on the AWS Marketplace website. Billing stops after the seller’s final metered hour; there is no refund step.


**How do I cancel an AWS Marketplace contract?** Contracts are cancelled by request through AWS Support. Refunds must be requested within 48 hours, and a full or prorated refund is typically granted in 3–5 business days.


**Can a buyer get a refund on AWS Marketplace after 48 hours?** Not through the buyer’s own request path. After that window it becomes a seller-initiated refund or cancellation, which the seller requests directly.


**Does cancelling in the vendor’s product cancel the marketplace agreement?** No. AWS directs sellers to send customers to AWS Marketplace to cancel. Disabling an account in your own product does not stop marketplace billing.


**Can a customer reduce a contract instead of cancelling?** Not mid-term. Customers can only decrease the size of a contract at renewal, or cancel the renewal. Mid-term changes can go up, not down.


**What happens to metering when a customer cancels?** You receive an EventBridge event and have one hour to send any final metering records. After that, metering for that customer is closed permanently.


---


## **Takeaways**


- The path depends on the purchase type. Subscriptions cancel from a console page; contracts go through AWS Support.
- Buyer-requested refunds on contracts must be raised within 48 hours. Tell customers at onboarding, not at cancellation.
- Most “cancel” requests are really “don’t renew.” Ask which one before doing anything.
- Contracts cannot shrink mid-term. Say so early and turn it into a renewal conversation.
- Turning off the account in your product does not end the marketplace agreement or the billing.
- Every cancellation closes the metering window one hour after the event. Batch metering will miss it.


---


Cancellations are only manageable when the agreement, its pricing model, and its renewal setting are visible in one place. See how[agreement management in Suger](https://www.suger.io/platform/agreements/) tracks every marketplace agreement through its full lifecycle — purchase, entitlement, renewal, and end.
