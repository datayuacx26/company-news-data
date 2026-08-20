---
schema_version: "1.0.0"
document_id: "517958254749893b5ee8e2f7c96c648e19ac948ebe97cc0fca11bb2fcefd721d"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/marketplace-events-and-webhooks/"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-15T09:12:25.375763+00:00"
fetched_at: "2026-08-15T09:12:26.807099+00:00"
content_hash: "sha256:fcb849f45f8d4149588dc1c12169535af05a7792b16091387a63a38791db2956"
---

# Marketplace Events and Webhooks: A Practical Guide

*A marketplace event is a message the platform sends when a customer’s subscription or entitlement changes. Most of them are informational. A few carry a deadline, and missing those costs you either revenue or a customer’s access.*


---


Marketplace integrations fail in a characteristic way. Everything works in testing, the first real customers land fine, and then months later somebody notices that a handful of accounts were never provisioned, or that a churned customer’s final month was never billed.


Almost always the cause is the same: a handler was written for the event everyone thinks about — the new subscription — and the other nine were left to a` default:` branch that logs and returns.


Here is the full set, what each one obliges you to do, and which two have a clock running.


---


## **What events does AWS Marketplace actually send?**


AWS Marketplace publishes subscription and entitlement changes to[your default EventBridge event bus](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-eventbridge-integration.html) . Every event uses the source` aws.agreement-marketplace` , and you filter on` detail-type` .


Your role determines which events you receive, and there are two:


- **Manufacturer** — you are the ISV who listed the product.
- **Proposer** — you extended the offer to the buyer.


On a direct public or private offer you are both. When a channel partner resells your product, the partner is the proposer and you are only the manufacturer — which means some events go to them and not to you. That asymmetry is the source of a lot of confusion in reseller motions, and it is worth reading alongside[CPPO, MPO and reseller plans](https://www.suger.io/resources/blog/cppo-vs-mpo-multiparty-private-offers/) .


Event What it means What you must do


` Purchase Agreement Created` New, replaced or renewed agreement Record it; run post-sale actions. Call` DescribeAgreement` to find out whether it is a free trial


` Purchase Agreement Amended` An existing agreement changed Amend your record


` Purchase Agreement Ended` Expired, cancelled or terminated Record closure; begin revoking entitlements


` Purchase Agreement Advisory Issued` AWS flagged a problem — account closure, compromise, abuse or fraud Contact the buyer; decide whether to restrict access


` Purchase Agreement Advisory Resolved` The flagged problem is resolved Restore access at your discretion


` License Updated` The buyer’s entitlement changed Call` GetEntitlements` and provision accordingly


` License Deprovisioned` The buyer’s entitlement ended ⏱ **One hour** to report final usage


` Spend Threshold Reached` Pay-as-you-go buyer hit a spend threshold; AWS is verifying their card Nothing — wait for the outcome


` Spend Threshold Vet Succeeded` Verification passed Nothing


` Spend Threshold Vet Failed` Verification failed; AWS will retry Nothing


The three spend-threshold events apply only to SaaS products with usage-based pricing. AWS Marketplace does not send them for contract-only or subscription products.


---


## **The two events with a clock**


Almost everything above can be handled late without consequence. Two cannot.


**` License Deprovisioned` starts a one-hour window.** For products with a usage-based component, AWS Marketplace gives sellers[one hour to submit final usage records](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-eventbridge-integration.html) using` BatchMeterUsage` . After that hour, entitlements are fully revoked and the API rejects reported usage — you cannot bill the customer for it.


This is the single most expensive detail in marketplace integration, because it is invisible until you audit. A nightly metering job is, by construction, up to 24 hours late. Every churned usage-based customer loses their final partial period, quietly, forever.


**The older SNS path says the same thing differently.** On SNS,` unsubscribe-pending` arrives first and gives the seller[about one hour](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-notification.html) to send final metering records before` unsubscribe-success` closes the door. If you are still on SNS, note that AWS states SNS notifications are being replaced by EventBridge — existing integrations continue to work, and new listings will eventually transition.


If you take one thing from this post: **final metering must be event-driven, not scheduled.**


---


## **The events people forget to handle**


Three are routinely missing from integrations, and each one has a characteristic symptom.


**` subscribe-fail` (SNS) / a failed agreement.** On the SNS path, a buyer can reach your landing page before payment succeeds. AWS is explicit: wait for` subscribe-success` before allowing consumption. Teams that provision on arrival at the landing page give away product to buyers whose payment never cleared.


**` Purchase Agreement Advisory Issued` .** Sent when AWS has identified account closure, compromise, abuse or fraud affecting the agreement. Nobody writes a handler for this, because nobody expects it. It is the one event where the correct action involves a human.


**Re-subscribe races.** AWS documents a specific case: if a buyer unsubscribes and immediately re-subscribes before the final` unsubscribe-success` is sent, that message is never sent and a` subscribe-success` arrives instead. A state machine that requires an unsubscribe before it will accept a new subscription will deadlock on that customer. Handlers must be idempotent and must not assume events arrive in the order you would have chosen.


The general class of failure — a customer who has paid but cannot get in — is covered in[Paid, and Still Locked Out](https://www.suger.io/resources/blog/marketplace-provisioning-failures/) .


---


## **Designing the handler**


Four properties separate integrations that hold up from ones that need a quarterly cleanup script.


**Acknowledge fast, process separately.** Whatever receives the event should do almost nothing: persist the raw message, return, and let a worker act on it. If provisioning is inline with receipt, a slow downstream call becomes a lost event. AWS’s own recommendation on the SNS path is to subscribe an SQS queue and poll it.


**Treat the event as a trigger, not as data.** Several events deliberately carry almost nothing. The SNS entitlement message is identical for a new contract, an upgrade, a renewal and an expiry — AWS notes that a subsequent` GetEntitlement` call is required to discover what actually changed. Reading state from the event body rather than from the API is a bug that will present as customers on the wrong plan.


**Be idempotent.** You will receive duplicates. Key on the agreement or customer identifier plus the change, not on the fact that a message arrived.


**Log the ones you ignore.** A` default:` branch that silently drops unknown` detail-type` values is how a new event type gets missed for a year. Log it loudly enough that somebody notices.


---


## **Frequently asked questions**


**What event source does AWS Marketplace use in EventBridge?** Events are published to your default event bus with the source` aws.agreement-marketplace` . You filter on` detail-type` , for example` Purchase Agreement Ended - Manufacturer` .


**How long do I have to report final usage when a customer leaves?** One hour. AWS Marketplace sends` License Deprovisioned` to start the window, and after it closes` BatchMeterUsage` rejects the usage and the customer cannot be billed for it.


**Is AWS Marketplace replacing SNS notifications with EventBridge?** Yes. AWS states SNS notifications for SaaS products are being replaced by EventBridge. Existing SNS integrations continue to function, and new listings will eventually transition.


**Why do I get different events than my channel partner?** Your role decides. The ISV who listed the product is the manufacturer; whoever extended the offer is the proposer. On a channel partner private offer the partner is the proposer, so some events go to them and not to you.


**Can I read the change directly from the event payload?** Not reliably. The SNS entitlement message is identical for a new contract, upgrade, renewal or expiry, and AWS requires a follow-up` GetEntitlement` call to discover what changed. Treat events as triggers.


**What happens if a buyer unsubscribes and immediately re-subscribes?** AWS documents that the final` unsubscribe-success` message is not sent, and a` subscribe-success` arrives instead. Handlers that require an unsubscribe before accepting a new subscription will stall on that customer.


---


## **Takeaways**


- Filter on` detail-type` from source` aws.agreement-marketplace` . There are ten events, not one.
- ` License Deprovisioned` starts a one-hour final metering window. Scheduled metering jobs miss it by construction.
- Wait for a successful subscription before provisioning. A buyer can reach your landing page before their payment clears.
- Events are triggers, not data. Call the entitlement API to find out what changed.
- Handlers must be idempotent and must tolerate out-of-order delivery, including the unsubscribe-then-resubscribe race AWS documents.


Suger consumes these events for every marketplace it supports and turns them into one agreement and entitlement record your product, CRM and finance systems can all read — including the final metering call inside the window.[See how Suger handles billing and metering](https://www.suger.io/platform/billing-metering/) , or[talk to our team](https://www.suger.io/contact-us/) about your current integration.
