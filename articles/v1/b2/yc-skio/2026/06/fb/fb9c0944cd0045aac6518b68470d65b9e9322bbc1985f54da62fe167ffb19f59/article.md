---
schema_version: "1.0.0"
document_id: "fb9c0944cd0045aac6518b68470d65b9e9322bbc1985f54da62fe167ffb19f59"
company_key: "yc-skio"
company: "Skio"
source_id: "yc-skio-news-import-cdeaee5c7d9f"
canonical_url: "https://skio.com/blog/advanced-subscription-journeys-targeting-by-criteria"
published_at: "2026-06-26T14:37:00.744+00:00"
first_seen_at: "2026-07-24T01:02:11.347387+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:29c7c4a98c02049e46a1f5bba8c08f602cabefb9858542a3e5dcce87fbef741c"
---

# Advanced Subscription Journeys: Targeting by Criteria | skio

Most search results for "conditional logic" want to teach you LSAT prep or how to hide a form field. Useless if what you actually need is to reward your loyal subscribers without handing the same gift to everyone, or swap a seasonal product for the right customers automatically. That's what conditional logic does inside Journeys, and here's how to use it.


Conditional logic is the "if this is true, do this, otherwise do that" engine inside a Journey. In plain terms, your Journey checks subscriber data and takes different actions depending on what it finds. Send a discount only to subscribers on their third order. Add a gift only to people past order six. Set it once and it runs forever, which is the whole point.


## The criteria you can actually target


This is the menu. These are the attributes a Journey can check before it does anything.


-


**Order count.** Target subscribers between orders 3 and 6, or anyone past 10.


-


**Product or variant.** Only subscribers with Product A.


-


**Subscription status.** Active, paused, or failed.


-


**Tags.** Customer tags or subscription tags.


-


**Subscription length.** Days active.


-


**Next order date proximity.** How close the next charge is.


-


**Prepaid versus recurring.** Treat the two billing models differently.


You target subscribers by order count, specific products, subscription status, tags, how long they've been subscribed, and whether they're prepaid or recurring. That covers the overwhelming majority of real targeting jobs.


## Real scenarios operators actually need


Abstract logic is boring. Here's what it looks like in the wild.


-


Send a free gift only to subscribers who've hit 6 orders, so loyalty rewards don't leak to everyone.


-


Auto-swap a seasonal product for subscribers on Product X when inventory runs out.


-


Apply a retention discount only to subscribers active 90 days or more.


-


Pause subscriptions tagged "vacation-hold" and resume them 30 days later.


-


Send a survey only to subscribers who cancelled in the last 7 days.


Each one is a trigger plus a condition plus an action, and each one runs without you touching it again.


## How to set up conditional targeting


The build itself is short.


1.


Choose your trigger. What starts the Journey: an order placed, a subscription created, a status change.


2.


Add a Condition step in the Journey builder.


3.


Define your criteria using the dropdown menus.


4.


Set the "if true" action and the "if false" action, or no action for the false path.


5.


Test with a real subscription before you turn it on.


The most common mistake is forgetting to define the "else" path, which leaves you unsure what happens to subscribers who don't match. The Journey setup guide walks through the builder screen by screen.


## Combining criteria with AND/OR


This is where targeting gets sharp. Use AND when subscribers must meet every condition, like order count over 5 AND product equals Coffee Blend A. Use OR when meeting any condition is enough, like tagged "VIP" OR order count over 10. You can stack both to build something genuinely precise, like subscribers who have Product X AND have been active 60-plus days AND are not tagged "churned." That's how you stop spraying offers at everyone and start hitting the exact segment you meant to.


## What you can't target, and the workarounds


Honesty section. A few things Journeys can't do directly, and how to get around them.


You can't target by lifetime value or total dollars spent. Use order count as a proxy, since someone with 10 orders has likely spent more than someone with 2. You can't target by Klaviyo segment or external data, so sync those segments to Shopify customer tags with Shopify Flow, then target the tags. You can't target by geographic location directly, so tag customers by region with Shopify Flow based on shipping address and target the tag. The limitations exist because Journeys run inside Skio's own data model, which is also why they're fast and reliable.


## Test before you go live


One bad condition can fire the wrong action at thousands of subscribers, so testing isn't optional. Create a test subscription that matches your criteria, trigger the Journey, and check the subscription audit log to confirm the right action fired. The classic testing mistake is using your own subscription, which often doesn't actually match the criteria you set, so you "test" something that was never going to trigger.


## Common mistakes with conditional logic


Watch for these:


-


**Criteria too broad.** "All active subscribers" when you meant "active subscribers with Product X."


-


**No else path defined.** Subscribers who don't match fall through with no action and you don't notice.


-


**Conflicting Journeys.** One applies a discount, another removes it, and the last one to fire wins.


-


**Forgetting to turn it off.** A promotional Journey that nobody switched off keeps running past the promo.


Start narrow, document what each Journey does, and audit monthly.


## Journeys versus Rules


Quick clarification that saves confusion. Rules are for one-time bulk actions on existing subscriptions, like "add a discount to everyone with Product X right now." Journeys are for ongoing automation triggered by events, like "every time a subscriber hits 5 orders, send a gift." Use Rules to fix a problem or run a limited promo. Use Journeys for evergreen, set-it-and-forget-it automation. You can use both together: a Rule to fix historical data, a Journey to handle everyone going forward. The Rules overview covers the difference in depth.


## Audit your Journeys monthly


A quick monthly pass keeps things clean. Review active Journeys for anything outdated. Check the analytics for how many subscribers entered and how many matched. Look for conflicts where multiple Journeys target the same people. Verify the criteria still make sense, since product names and tags change. And document what each Journey does, because future you will need it.
