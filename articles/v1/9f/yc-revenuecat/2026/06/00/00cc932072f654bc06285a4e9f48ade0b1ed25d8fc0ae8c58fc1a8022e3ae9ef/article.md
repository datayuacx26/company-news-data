---
schema_version: "1.0.0"
document_id: "00cc932072f654bc06285a4e9f48ade0b1ed25d8fc0ae8c58fc1a8022e3ae9ef"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/company/why-we-removed-always-decline-refund-preference"
published_at: "2026-06-17T13:06:54+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:23fca4848fc4ceebc4d6c578eb92800bd501cd7c207b8b690e130e079f79d42b"
---

# Why we removed “always decline” as a refund preference

If you’ve recently tried to set your refund preference to “Always decline” in the RevenueCat dashboard, you may have noticed the option is gone. We removed it because blanket declines lose their effectiveness over time and can push users into chargebacks – the worse outcome for everyone. If “Always decline” was your existing setting, it’s still active. Below: the full reasoning, and how it affects you.


## What changed


The RevenueCat dashboard no longer allows users to select “Always decline” as a global refund preference.


‘Always prefer declining refunds’ is no longer an option in the Handling of refund requests section.


The available options now are:


- **Let Apple decide:** Neutral, no preference expressed
- **Grant in full:** You prefer the refund is approved
- **Grant prorated:** You prefer a partial refund based on consumption


We didn’t unilaterally change settings for developers who already had “Always decline” enabled — it’s still active if that was your setting. If you do switch it off, you’ll see a confirmation prompt letting you know you won’t be able to re-enable it. That’s your call to make, but the case for keeping it on is getting weaker.


## Why “always decline” was removed


This comes down to how Apple’s refund system actually works:


**Refund preferences are signals, not instructions.** When you set a refund preference and send it to Apple via the[App Store Server API](https://developer.apple.com/documentation/appstoreserverapi) , you’re not making a binding decision.[Apple uses your input as one of many factors when evaluating a refund request](https://developer.apple.com/documentation/appstoreserverapi/consumptionrequest) . The` refundPreference` field in a` ConsumptionRequest` is advisory by design.


**Blanket declines erode their own effectiveness.** When a preference is always set to “decline” regardless of context — whether the user barely touched the app or encountered a genuine bug that made it unusable — Apple’s systems start to discount it. A preference that clearly doesn’t account for the specifics of a transaction carries less weight over time. The signal becomes noise.


**Rejecting valid refund requests can backfire.** When a legitimate refund request is declined, some users go straight to their bank and issue a chargeback instead. Chargebacks are worse for developers, as a high chargeback rate puts your developer account at risk of being suspended by Apple. A blanket “always decline” policy can inadvertently push users toward the worse outcome for everyone.


## What to do right now


If you want to decline a specific refund, you can still do that today using the` $appleRefundHandlingPreference` customer attribute. Set it to` DECLINE_REFUND` for any customer before they submit their refund request, and RevenueCat will send that preference to Apple instead of your global setting.


You can set this via the SDK, the REST API, or manually from the customer page in the dashboard.[See the full docs →](https://www.revenuecat.com/docs/platform-resources/apple-platform-resources/handling-refund-requests#overriding-refund-preference)


## The bottom line


“Always decline” was a blunt instrument, and it was becoming less effective over time. We’ve seen enough cases of it backfiring that we felt it was important to be straight with you about this. We know it’s not what everyone wanted to hear. At the end of the day, a setting that doesn’t do what you think it does isn’t worth the risk.


**See also:**


- [How to handle refunds in RevenueCat](https://www.revenuecat.com/docs/refunds)
