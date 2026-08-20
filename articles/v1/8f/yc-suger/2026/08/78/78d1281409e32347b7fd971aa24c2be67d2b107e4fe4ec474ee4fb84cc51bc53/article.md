---
schema_version: "1.0.0"
document_id: "78d1281409e32347b7fd971aa24c2be67d2b107e4fe4ec474ee4fb84cc51bc53"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/marketplace-entitlement-management/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:43:09.652657+00:00"
fetched_at: "2026-08-13T02:43:11.361615+00:00"
content_hash: "sha256:f5117e0085245604e846496ade3cd20bab10ef3af4d23ba8afbaaa7a4f7540bb"
---

# Entitlement Management on Cloud Marketplaces

*Entitlement management is the work of turning a marketplace purchase into product access that is correct, current, and revocable. It is three hops — event, entitlement, access — and each cloud implements them differently. Getting it right is unglamorous. Getting it wrong produces customers who paid and can’t log in, and customers who cancelled and still can.*


---


Every marketplace integration starts the same way: a webhook arrives, the engineer wires it to a “create user” function, and it works. It keeps working for months, because most purchases are simple and most customers do not immediately change anything.


Then a customer upgrades mid-term, a payment fails after the landing page redirect, an agreement is replaced rather than renewed, and the model that treated events as instructions produces access that nobody can explain.


Here is the shape that holds up, and what each cloud actually gives you to build it from.


---


## **What is entitlement management?**


**Entitlement management is maintaining, for every buyer, an accurate answer to “what are they entitled to right now,” and making your product enforce it.** It is distinct from the two things it sits between: the commercial record of what was sold, and the user accounts in your product.


The distinction that matters most operationally is between an **event** and **state** :


- An **event** is a notification that something changed. It arrives once, may arrive twice, may arrive out of order, and describes a moment.
- **State** is what the buyer is entitled to now. It is authoritative, it lives with the marketplace, and you read it on demand.


**Build on state; use events only as a prompt to re-read it.** Every durable marketplace integration converges on this, and every fragile one is an event handler that mutates local records directly.


If the offer/entitlement vocabulary is new,[offer vs entitlement](https://www.suger.io/resources/blog/offer-vs-entitlement/) covers the two record types before this post’s mechanics.


---


## **The three hops, and where they break**


Hop What happens Failure mode


Purchase → event The marketplace notifies you Missed, duplicated, or out of order


Event → entitlement You read the authoritative state Trusting the event payload instead


Entitlement → access Your product grants or revokes Access granted before payment confirms


The middle hop is the one teams skip, and AWS makes the reason to keep it explicit. Its entitlement topic sends the same action value for every kind of change: “regardless of the action (new, upgrade, renewal, or expired), the message is the same,” and “a subsequent call to` GetEntitlement` is required to discover the content of the update.”


That is a design instruction disguised as a note. You are not being told what changed — you are being told to go and look.


---


## **How each cloud implements it**


### AWS


Two SNS topics carry the signals for SaaS products.


**` aws-mp-subscription-notification`** covers the subscription lifecycle with four action values:


- ` subscribe-success` — the point at which “the seller can begin sending metering records.” If an agreement-based offer is later accepted, the message is sent again with the new offer identifier.
- ` subscribe-fail` — payment may have failed even though the buyer already reached your landing page. AWS is direct about the implication: “The seller should wait for the` subscribe-success` message before allowing consumption of the product.”
- ` unsubscribe-pending` — the buyer has cancelled, and you have “a limited time (about one hour) to get final metering records sent before the buyer is cancelled completely.”
- ` unsubscribe-success` — cancellation is complete, and no further metering records will be accepted.


**` aws-mp-entitlement-notification`** covers contract products and, as above, always sends` entitlement-updated` .


Two subtleties worth designing for. If a buyer unsubscribes and immediately re-subscribes before the final message is sent, “the final` unsubscribe-success` message will not be sent and a` subscribe-success` message will be sent instead” — so a state machine that requires the cancellation to complete before a new subscription can start will deadlock. And for future dated agreements, the notifications fire on the **agreement start date, not the sign date** .


AWS recommends subscribing an SQS queue to the topics and polling it, which also gives you the durability and retry behaviour an HTTP endpoint does not. Note the account constraint: you can only subscribe to the topics from the AWS account used to sell the products, though messages can be forwarded elsewhere.


One forward-looking item: AWS states that “SNS notifications for AWS Marketplace SaaS products are being replaced with Amazon EventBridge notifications,” with existing SNS integrations continuing to function. A new build should target EventBridge.


### Microsoft


Microsoft’s flow is a redirect rather than a pure event stream. The buyer lands on your **landing page** carrying a marketplace purchase identification token, which you exchange for subscription details using the **resolve** API, then complete onboarding and call **activate** to start the subscription period.


Asynchronous changes arrive at your **connection webhook** — Microsoft describes it as “the only way you get notified about updates to your customers’ SaaS subscriptions.” Both the landing page and the webhook “should be running 24/7.”


One behaviour that changes the shape of your code: if auto-activation is enabled on a plan, “the subscription activates and billing starts immediately at purchase. You don’t need to call the Resolve or Activate APIs for auto-activated plans. Instead, you receive a` Subscribe` webhook notification with the subscription details.” A single implementation therefore has to handle both an interactive path and a silent one.


### Google Cloud


Google Cloud Marketplace splits the job across three service accounts: the **Partner Procurement API** for purchase information and entitlement management, **Pub/Sub** for notifications, and the **Service Control API** for reporting usage on consumption-priced products. Google’s own framing is that the integration lets you “manage users’ accounts and entitlements, which indicate that users have bought your product from Cloud Marketplace.”


---


## **Design rules that survive contact with production**


**Make handlers idempotent.** The same message will arrive twice. Processing it twice must produce the same result as processing it once. This is cheap to build in and expensive to retrofit.


**Reconcile on a schedule, not only on events.** A nightly pass that reads every entitlement and compares it to the access you have granted catches the missed message, the handler that threw, and the manual change somebody made in a console. Teams that add this find drift in the first run — that is the point.


**Never provision on the offer.** Wait for the confirmation event. On AWS that is explicitly` subscribe-success` . Granting access at the landing page means supporting customers whose payment later failed.[When a buyer pays but provisioning never happens](https://www.suger.io/resources/blog/marketplace-provisioning-failures/) covers the whole failure surface.


**Revoke as carefully as you grant.** Most products have a well-tested grant path and a revoke path nobody has run. Expiry, cancellation and non-renewal all end an entitlement, and if nothing reacts you are giving software away in a way no report will show.


**Keep an audit trail keyed to the marketplace identifier.** When finance asks why a customer had access in March, the answer needs to be a record, not a reconstruction.


**Design for the one-hour window.** On AWS,` unsubscribe-pending` starts a clock for final metering. If your metering pipeline batches hourly, you can miss the last records of every cancellation — which is revenue you earned and did not bill.


---


## **Frequently asked questions**


**What is entitlement management on a cloud marketplace?** Maintaining an accurate, current answer to what each buyer is entitled to, and enforcing it in your product. It sits between the commercial record of the sale and the user accounts in your system.


**Should I trust the event payload or call the API?** Call the API. AWS’s entitlement topic sends the same action for new contracts, upgrades, renewals and expiry, and states that a` GetEntitlement` call is required to discover what changed.


**When can I start metering an AWS Marketplace customer?** On` subscribe-success` . AWS states that this message signals when the seller can begin sending metering records, and that you should wait for it before allowing consumption.


**What happens when an AWS buyer cancels?** You receive` unsubscribe-pending` first, giving about one hour to send final metering records, then` unsubscribe-success` , after which no further metering records are accepted.


**Do I need the resolve and activate APIs on Microsoft?** Not for auto-activated plans. Microsoft states that with auto activation the subscription activates and billing starts at purchase, and you receive a Subscribe webhook notification instead.


**Why reconcile entitlements if I handle every event?** Because messages get missed, handlers fail, and consoles get edited by people. A scheduled reconciliation against authoritative state is the only thing that catches drift you did not cause.


---


## **Takeaways**


- Events are prompts; entitlements are state. Read the authoritative record rather than acting on the payload.
- AWS sends one action value for every entitlement change and requires a` GetEntitlement` call to learn what it was.
- Wait for` subscribe-success` before granting access or metering. A landing page redirect is not a confirmed payment.
- ` unsubscribe-pending` opens a window of about one hour for final metering records. Batch pipelines miss it.
- Microsoft’s auto-activated plans skip resolve and activate entirely and arrive as a Subscribe webhook, so one implementation must handle two paths.
- Reconcile entitlements on a schedule. The first run always finds drift, which is the argument for having it.


---


Three clouds, three event models, one question your product has to answer correctly every time. See how Suger’s[agreement and entitlement management](https://www.suger.io/platform/agreements/) keeps entitlement state consistent across every marketplace, so your product reads one source instead of three.
