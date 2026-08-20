---
schema_version: "1.0.0"
document_id: "7368dcf46a9b4388589065a7e1af5bc818fb83ffef7f2e58565d7bcc0b5b0c0a"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/marketplace-provisioning-failures/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T08:10:57.347630+00:00"
fetched_at: "2026-08-11T08:11:00.150542+00:00"
content_hash: "sha256:a92b13939778e69cb90056cd59bef0c086a5430c4d2f0689d5a9550195ab14cc"
---

# When a Buyer Pays but Provisioning Never Happens

*A marketplace provisioning failure is when a buyer completes a purchase but never gains access to the product. It happens because the purchase and the provisioning are two separate events on two separate systems, joined by a handshake that has no retry built into it and fails quietly when it breaks.*


---


Of all the tickets a marketplace program produces, this is the worst one: a customer who has been charged, has a contract, and cannot get in. It arrives as a support escalation, it is usually diagnosed as a bug in your product, and it is usually not.


The cause is structural. On a cloud marketplace, buying and provisioning are separate transactions on separate systems. The marketplace records a purchase; your product grants access; a handshake connects them. When the handshake fails, the marketplace still thinks everything worked — because from its side, it did.


Here is the handshake, the events around it, and the six places it breaks. AWS is used throughout because AWS documents its mechanics most fully; the shape is the same on the other marketplaces.


---


## **How is a marketplace purchase supposed to reach your product?**


**Through a token exchange at the moment the buyer arrives, plus an asynchronous confirmation that arrives separately.** Two channels, and both have to work.


The synchronous path:


1. The buyer subscribes in AWS Marketplace.
2. AWS redirects them to your registration landing page, carrying a registration token.
3. Your page calls **` ResolveCustomer`** , exchanging that token for a customer identifier and product code.
4. You create or link an account in your system against that customer identifier.
5. You call **` GetEntitlements`** to see what they are actually entitled to.


The asynchronous path runs in parallel: AWS publishes messages about the subscription and the entitlement, which your service consumes and acts on. Those messages — not the redirect — are what tell you the purchase is real.


---


## **The messages, and what each one means**


AWS provides two topics for SaaS products. Both are being migrated from Amazon SNS to Amazon EventBridge, and AWS states that existing SNS integrations “will continue to function” while “new listings will eventually transition.”


**` aws-mp-subscription-notification`** — subscription state changes, for all pricing models:


Action Meaning


` subscribe-success` ”Signals when the seller can begin sending metering records”


` subscribe-fail` Payment may have failed, even though the buyer already reached your landing page


` unsubscribe-pending` The buyer is cancelling. “About one hour” to send final metering records


` unsubscribe-success` Cancellation complete. “No further metering records will be accepted”


**` aws-mp-entitlement-notification`** — contract changes, for products with a contract model. The action is always` entitlement-updated` , and AWS is explicit about the consequence: “regardless of the action (new, upgrade, renewal, or expired), the message is the same. A subsequent call to` GetEntitlement` is required to discover the content of the update.”


That last sentence is a design constraint disguised as a footnote. The message tells you *something* changed and nothing else. A handler that only reacts to messages it recognises will silently ignore upgrades, renewals, and expiries.


---


## **The six failure points**


### 1. The buyer never lands on your page


They subscribe, then close the tab, or get pulled into a meeting, or hand the purchase to procurement who has no reason to continue into a product signup.


Now AWS has an agreement and you have nothing — no customer identifier, no account, no way to contact them. The` subscribe-success` message *does* arrive, so you know a customer exists, and you cannot map them to a person.


**The fix is a reconciliation job, not a better landing page.** List agreements, compare against provisioned accounts, and treat any agreement with no matching account as an alert. This is the single highest-value control on this list, because it catches every other failure on it too.


### 2. Provisioning on arrival instead of on confirmation


The buyer lands, your page resolves them, and you grant access immediately. Then` subscribe-fail` arrives.


AWS’s guidance is unambiguous: “If the` subscribe-fail` message is generated, payment might have failed even though the buyer has already transitioned from the AWS Marketplace to the seller’s SaaS landing page. The seller should wait for the` subscribe-success` message before allowing consumption of the product.”


The correct pattern is to create the account on arrival and *activate* it on` subscribe-success` . Skip the split and you have a customer using your product with no contract behind them — a variant of the offer-versus-entitlement confusion covered in[offer vs entitlement](https://www.suger.io/resources/blog/offer-vs-entitlement/) .


### 3. Nobody is consuming the queue


The messages are delivered; nothing is listening.


AWS recommends subscribing an Amazon SQS queue to the topics and notes you “must define a service that continually polls the queue.” There is also an account constraint that trips teams with a multi-account setup: “You can only subscribe to AWS Marketplace SNS topics from the AWS account used to sell the products.”


Failure modes here are all quiet: a consumer that crashed and was never restarted, a dead-letter queue nobody alerts on, an IAM change that silently revoked access. Alert on the *absence* of expected messages, not only on processing errors — an empty queue looks identical to a quiet week.


### 4.` entitlement-updated` is treated as a no-op


Because the action string never varies, a handler written against` subscribe-success` alone appears to work perfectly. Contracts are created, customers get access, everyone is satisfied.


Then a customer upgrades and does not get their new limits. Or an entitlement expires and access continues. Every` entitlement-updated` must trigger a` GetEntitlement` call and a diff against what you currently believe. AWS also notes this topic fires on the agreement *start* date for future-dated agreements, not the signature date — so a deal signed in one quarter provisions in another.


### 5. The unsubscribe window is missed


` unsubscribe-pending` gives roughly an hour to send final metering records before` unsubscribe-success` closes the door permanently. Usage in that window that you never transmit is revenue you cannot bill.


There is also a re-subscribe edge case that breaks naive state machines: “If a buyer unsubscribes and then immediately successfully re-subscribes before the final` unsubscribe-success` message is sent, the final` unsubscribe-success` message will not be sent and a` subscribe-success` message will be sent instead.” A handler expecting a matched pair will deprovision a customer who is now paying.


### 6. The free trial flag is parsed as a boolean


` isFreeTrialTermPresent` looks like a boolean and is not. AWS: “The JSON value of this property is not a *boolean* datatype. Instead, the value is converted to a *string* datatype.”


In most languages a non-empty string is truthy, so` "false"` evaluates to true and every subscription is provisioned as a trial. It is a one-line bug that is invisible until the first paying customer is capped at trial limits.[Free trials on cloud marketplaces](https://www.suger.io/resources/blog/free-trials-on-cloud-marketplaces/) covers the surrounding mechanics.


---


## **What to build**


Five controls, in the order they earn their cost.


**1. The reconciliation job.** Agreements without accounts, accounts without agreements, both directions, daily. Nothing else on this list catches as much.


**2. A two-phase account.** Created on` ResolveCustomer` , activated on` subscribe-success` . Never one phase.


**3. An entitlement refresh on every notification.** Never trust the message body to describe the change; fetch and diff.


**4. Absence alerting.** Alert when expected messages stop arriving, not only when processing fails.


**5. A named owner for the queue.** This is production infrastructure carrying revenue events. It needs an on-call owner like any other.


If you are still on SNS, the move to EventBridge is worth planning rather than deferring —[five reasons to transition to AWS EventBridge from SNS](https://www.suger.io/resources/blog/5-reasons-to-transition-to-aws-eventbridge-from-sns/) covers the case, and[simplifying the shift to EventBridge](https://www.suger.io/resources/blog/simplify-the-shift-to-eventbridge-with-suger/) covers doing it without rewriting your handlers.


---


## **Frequently asked questions**


**Why does an AWS Marketplace buyer have no access after subscribing?** Usually the handshake broke. Either they never reached your landing page so no customer identifier was resolved, or the confirmation message was never processed. AWS considers the purchase complete either way.


**What is ResolveCustomer used for?** It exchanges the registration token AWS sends to your landing page for a customer identifier and product code, which is how you link a marketplace buyer to an account in your own system.


**Should I grant access when the buyer reaches my landing page?** No. Create the account then, but wait for` subscribe-success` before allowing consumption — AWS states payment may have failed even after the buyer reached your page.


**What does entitlement-updated mean?** That something changed — new contract, upgrade, renewal, or expiry. The message is identical in every case, so you must call` GetEntitlement` to find out what actually changed.


**How long do I have to send final metering records when a buyer cancels?** About an hour, between` unsubscribe-pending` and` unsubscribe-success` . After` unsubscribe-success` , no further metering records are accepted.


**How do I find buyers who paid but were never provisioned?** Run a daily reconciliation between marketplace agreements and provisioned accounts in both directions, and alert on anything unmatched. It is the only control that catches every failure mode at once.


---


## **Takeaways**


- Purchase and provisioning are separate events joined by a handshake with no built-in retry. When it breaks, the marketplace still reports success.
- Create the account when the buyer arrives; activate it only on` subscribe-success` . AWS says payment can fail after they reach your page.
- ` entitlement-updated` never says what changed. Every one must trigger a fetch and a diff, or upgrades and expiries pass silently.
- ` unsubscribe-pending` gives about an hour to send final metering records, and an immediate re-subscribe suppresses` unsubscribe-success` entirely.
- ` isFreeTrialTermPresent` is a string, not a boolean. In most languages` "false"` is truthy.
- The daily agreement-to-account reconciliation is the control that catches everything else on this list.


---


Provisioning failures are silent because nothing is watching both sides at once. See how[billing and metering in Suger](https://www.suger.io/platform/billing-metering/) holds marketplace subscription and entitlement events alongside your own account state — so an agreement with nobody behind it becomes an alert instead of a support ticket.
