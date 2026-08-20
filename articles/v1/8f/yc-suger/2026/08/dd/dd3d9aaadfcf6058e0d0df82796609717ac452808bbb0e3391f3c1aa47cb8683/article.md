---
schema_version: "1.0.0"
document_id: "dd3d9aaadfcf6058e0d0df82796609717ac452808bbb0e3391f3c1aa47cb8683"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/aws-marketplace-refunds-and-cancellations/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T06:20:29.574702+00:00"
fetched_at: "2026-08-07T06:20:30.243690+00:00"
content_hash: "sha256:d30af89eb710c274876e0c92fbcc2257033b53a4cb72f922f7f8ef2ec3433cdf"
---

# Refunds and Cancellations on AWS Marketplace

*An AWS Marketplace refund returns money to a buyer for a transaction that already billed. A cancellation ends an active agreement before its term is up. Since March 2026, sellers can request both themselves — from the seller portal or the Agreement APIs — instead of filing a support case and waiting.*


---


Nobody plans the refund path. It gets built the first time a customer asks for money back, usually by whoever picks up the ticket, and it usually starts with someone in finance asking a question nobody can answer: *how do we even do this?*


For years the honest answer on AWS Marketplace was “open a case and wait.” Refunds and cancellations ran through AWS Marketplace Seller Operations, on their clock, with your customer’s renewal conversation hanging on the outcome.


That changed on 31 March 2026, when AWS launched self-service refunds and agreement cancellations for sellers. Here’s what the flow looks like now, what the buyer sees, what it does to your disbursement — and the one step that cannot be reversed.


---


## **How do refunds work on AWS Marketplace?**


A refund on AWS Marketplace returns funds to the buyer for an invoice that has already been issued, and adjusts what AWS disburses to you accordingly. As of March 2026, the seller requests it directly — from the Agreements page in the seller portal, or programmatically through the **AWS Marketplace Agreement APIs** — rather than through a support ticket.


Two things follow from that design. First, the money moves through the same rails as the original transaction: the adjustment lands in your disbursement rather than being settled between you and the customer directly. Second, refunds do not wait for the buyer to agree. The billing adjustment processes automatically, because the party giving money back is you.


Refunds are also, in AWS’s own framing, irreversible once processed. There is no undo. That single property should shape every internal approval you put in front of the request.


---


## **How cancellations differ from refunds**


A cancellation ends an agreement early; a refund returns money on one that billed. They are separate actions with different consent models, and conflating them is the most common source of a bad customer conversation.


The difference that matters operationally is the buyer’s role. A refund is applied. A cancellation is *proposed* : the buyer receives a notification and has **seven days to respond** before the cancellation proceeds automatically. That window is the whole reason to send a cancellation request early rather than on the last day of a quarter.


Cancellations also reach into the channel. Where a Channel Partner Private Offer (CPPO) is involved, the channel partner initiates the request — which means your reseller relationship, not just your customer relationship, is part of the process.


---


## **The full path: request to disbursement adjustment**


Here is the sequence end to end, with the decision each stage actually requires.


Stage What happens What it needs from you


1. Trigger A buyer asks for money back, or a deal needs to be unwound — wrong quantity, duplicate purchase, a term that changed, a cancelled project Decide *refund* or *cancellation* first. They are not interchangeable.


2. Internal approval Someone with authority signs off on the amount and the reason The gate that matters, because the next step can’t be reversed


3. Request Seller submits from the seller portal Agreements page, or via the Agreement APIs The agreement, the invoice, and whether you are refunding a paid invoice or reducing an unpaid balance


4. Buyer window (cancellations only) Buyer is notified and has seven days to respond before the cancellation proceeds automatically Tell the customer it’s coming. A surprise notification from AWS is a bad way to learn about it.


5. Processing AWS applies the billing adjustment. KYC verification applies only where an invoice requires compliance validation Nothing — but track the status


6. Notification All parties receive email updates, and status changes are published to EventBridge Wire the EventBridge events into your own systems so finance isn’t reading email


7. Disbursement adjustment The refunded amount is netted against what AWS disburses to you Reconcile the adjustment against the original transaction in your books


The capability is available in all commercial AWS Regions where AWS Marketplace operates.


Stage 7 is where most of the pain actually lands. A refund does not arrive as a separate, clearly labelled event in your accounting system — it shows up as a smaller disbursement, and if nobody links it back to the original agreement, marketplace revenue stops reconciling. The[marketplace billing and revenue guide](https://www.suger.io/resources/guides/marketplace-billing/) covers how to structure that reconciliation before you need it.


---


## **What to decide before you click**


Three questions decide whether a refund goes cleanly or becomes a dispute.


- **Refund the paid invoice, or reduce an unpaid balance?** If the invoice has not been paid yet, reducing the outstanding balance is cleaner than a full refund cycle. If it has been paid, a refund is the only route.
- **Who is authorised?** Because processed refunds are irreversible, the approval should sit with someone accountable for the revenue, not with whoever is closest to the ticket.
- **Does this end the relationship or reset it?** A refund on a mistaken quantity is an administrative fix. A cancellation is a churn event, and it should trigger the same review any other churn does.


If the self-service flow does not fit the case — an unusual dispute, or an agreement it cannot reach — AWS still accepts requests through its refund and cancellation form, which asks for the buyer account ID, seller account ID, product ID, and billing date. Collect those four fields at the start of any refund conversation regardless of the route, because you will need them either way.


---


## **Where refunds get expensive**


Refunds themselves are rarely the problem. The cost sits in three places around them.


**Reconciliation.** A marketplace disbursement is already a netted number — listing fees deducted, multiple agreements combined. Adding refund adjustments to that without an agreement-level record turns your marketplace revenue into a figure finance cannot tie out.


**Commission clawback.** If the refunded deal paid a partner commission or a rep’s quota credit, both need reversing. That is a policy question with a clawback window attached, and it should have been settled in the[partner commission plan](https://www.suger.io/prm/commissions/) long before the refund.


**Silence.** The buyer gets an email from AWS. If that email is the first they hear of a cancellation you initiated, the refund stops being an operational event and becomes a relationship one.


Suger sellers request refunds and cancellations from the entitlement itself, so the request, the resulting billing adjustment, and the original agreement stay attached to one record. The[refunds and cancellations documentation](https://doc.suger.io/aws-marketplace/refunds-cancellations) walks the exact steps.


---


## **Frequently asked questions**


**Can sellers issue AWS Marketplace refunds themselves?** Yes. Since March 2026, sellers can request refunds and agreement cancellations from the Agreements page in the seller portal or through the AWS Marketplace Agreement APIs, rather than filing a support case.


**Can an AWS Marketplace refund be reversed?** No. Once AWS processes a refund it is irreversible. Put the approval gate before the request, not after it, and confirm the amount and invoice first.


**Does the buyer have to approve a refund?** No. Refund billing adjustments process automatically. Cancellations are different: the buyer is notified and has seven days to respond before the cancellation proceeds.


**How does a refund affect my disbursement?** The refunded amount is netted against what AWS disburses to you. It arrives as a smaller disbursement rather than a separate line, so link it back to the original agreement in your reconciliation.


**What happens to refunds on a CPPO deal?** Requests on Channel Partner Private Offer agreements are initiated by the channel partner. Plan for the reseller to be part of the process, and settle commission clawback terms in advance.


**What if the self-service flow doesn’t cover my case?** AWS still accepts refund and cancellation requests through its form, which requires the buyer account ID, seller account ID, product ID, and billing date. Collect those fields at the start of any refund conversation.


---


## **Takeaways**


- Decide refund or cancellation before anything else — refunds apply automatically, cancellations give the buyer a seven-day window to respond.
- Put the approval gate before the request. AWS processes refunds irreversibly.
- Subscribe to the EventBridge status events rather than relying on email, so finance sees adjustments as they happen.
- Reconcile every refund back to its agreement. A netted disbursement with no agreement-level record is where marketplace revenue stops tying out.
- Settle partner commission clawback rules before the first refund, not during it.


---


Refunds are a billing event before they are a support event. See how[marketplace billing and metering in Suger](https://www.suger.io/platform/billing-metering/) keeps agreements, invoices, and disbursement adjustments on one record.
