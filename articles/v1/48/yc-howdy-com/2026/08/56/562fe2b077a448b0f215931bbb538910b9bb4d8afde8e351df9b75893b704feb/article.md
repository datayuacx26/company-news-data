---
schema_version: "1.0.0"
document_id: "562fe2b077a448b0f215931bbb538910b9bb4d8afde8e351df9b75893b704feb"
company_key: "yc-howdy-com"
company: "Howdy.com"
source_id: "yc-howdy-com-news-import-7de6c48bfb23"
canonical_url: "https://www.howdylatam.com/blog/webhooks-simple-in-docs-not-in-production"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-18T00:05:15.826908+00:00"
fetched_at: "2026-08-18T00:05:16.728051+00:00"
content_hash: "sha256:ef06644da5148ad10469ddb40dd967fa25b4c2b7c2e65a5a5acf6b83335c176b"
---

# Webhooks Are Simple in the Docs. In Production, They're Not.

You wire up a webhook in twenty minutes. Register a URL, pick the events you care about, watch the first payload land in your logs. It feels like one of those rare pieces of infrastructure that just works. Then, three weeks later, a payment provider fires the same "charge succeeded" event four times in a row, your handler processes it four times, and a customer gets charged for one order twice.


That's the part the getting-started guide never covers. Webhooks are one of the simplest ideas in distributed systems: when something happens, call this URL. But the gap between "call this URL when something happens" and "build something that survives the real internet" is enormous, and almost nobody closes it until something breaks in production.


## **The delivery guarantee nobody promised you**


Most teams assume "at least once" delivery when they're actually building for "exactly once," and they don't notice the mismatch until it costs them money. Every serious webhook provider, Stripe, GitHub, Shopify, take your pick, documents that events can and will be delivered more than once. Network blips, timeouts on your end, provider-side retries after a 5xx, a load balancer that kills a connection mid-response. All of it looks the same from the sender's side: no confirmation received, so send it again.


If your handler treats every incoming webhook as a fresh, never-seen-before event, you don't have a webhook integration. You have a system that will eventually double-process something important, and it's usually money, inventory, or a notification that goes out twice and makes your product look broken.


The fix isn't complicated in theory. Every event has an ID. You store which IDs you've already processed. You check before you act. In practice this means an extra table, a unique constraint, and a decision about how long you keep that history around, because "forever" isn't free and "24 hours" might not be enough if a provider decides to retry a failed delivery a week later. Stripe, for context, will retry certain failed webhooks for up to three days. Design your dedup window for that reality, not for the happy path you tested against.


## **Ordering is a promise your provider almost never makes**


The second assumption that quietly breaks things is order. You built your handler assuming \`order.created\` arrives before \`order.shipped\`, because that's the order they happened in. Except HTTP requests don't queue themselves politely. Two events fired sixty milliseconds apart can arrive at your server in either order, especially once you're running more than one instance behind a load balancer, or once a retry gets queued behind a fresh event.


Most providers are explicit about this in their docs, and most integrations ignore the warning anyway, because building an ordering-agnostic handler is more work than building one that assumes a timeline. The honest options are: design your state machine so out-of-order events are safe (an "already shipped" order that gets a late \`order.created\` event just gets ignored), or re-fetch current state from the source API instead of trusting the payload to tell the full story. Both cost more up front than "just handle the event as it arrives." Both save you from the bug report that says the system shows an order created after it shipped, which is genuinely hard to explain to a non-technical stakeholder.


## **Signature verification is not optional, and skipping it is common**


Your webhook endpoint is a public URL that triggers real actions in your system when it receives a POST request. If you're not verifying that the request actually came from the provider, you've built an API that anyone on the internet can call to make your system believe a payment succeeded, an account was created, or a shipment went out.


Every provider that takes this seriously signs payloads with an HMAC signature in a header, and every provider's docs explain how to verify it. The reason this still shows up as a gap in production systems isn't ignorance. It's that verification feels like ceremony when you're testing locally with curl and a hardcoded payload, so it gets stubbed out "for now," and the ticket to add it back never gets prioritized once the feature ships and works.


The engineers who get this right treat signature verification as part of the endpoint, not an enhancement to it. There's no version of a webhook handler that's "done" without it, the same way there's no version of a login flow that's "done" without checking the password.


## **Retries will outlive your assumptions about timing**


Providers don't give up after one failed attempt. They retry, usually with exponential backoff, sometimes for days. That means your handler needs to survive being called for an event that happened last Tuesday, with state that has since changed underneath it. An order that got cancelled after the webhook fired but before the retry arrived. A user that got deleted between the first attempt and the fifth.


This is where idempotency and ordering stop being separate concerns and start compounding. A handler that's idempotent but assumes current state matches the payload's timestamp will still get it wrong. The events that actually cause incidents are rarely the first delivery. They're the third retry, four days later, hitting a system that has moved on without telling anyone.


## **Timeouts are a contract you didn't read**


Providers expect your endpoint to respond fast, usually inside a handful of seconds, and they count a timeout as a failure worth retrying. If your handler does real work synchronously, updating a database, calling a downstream API, sending a notification, before it returns a 200, you're gambling that all of that finishes inside a window you don't control.


The pattern that holds up: acknowledge receipt immediately, do the actual work asynchronously in a queue. This adds a moving part, which is exactly why teams skip it early on. It's also why the teams that skip it end up debugging mysterious duplicate processing later, because a slow synchronous handler that times out looks, from the provider's side, exactly like a handler that never got the message at all. So it retries. And now you're back to the first problem, except this time you caused it yourself.


## **Testing this properly means simulating failure, not success**


Most teams test a webhook integration by clicking "send test event" in the provider's dashboard once and calling it done. That test sends a clean, well-formed, on-time payload, exactly the one scenario that was never going to break your handler.


Real testing means simulating duplicate delivery, out-of-order arrival, malformed signatures, and slow responses, none of which a dashboard's test button reproduces well. Some teams build a small local harness that replays captured production payloads with intentional duplicates, delay, and reordering baked in. That's tedious to set up and it's the only way to find out how your handler behaves under the exact conditions that break it, before a customer finds out for you.


## **The cost of getting this wrong compounds silently**


The dangerous thing about a bad webhook handler is that it can run for months without anyone noticing. Duplicate processing doesn't crash your server. It quietly double-charges one customer out of ten thousand, or double-fires one notification out of a hundred thousand, and unless someone is specifically looking for duplicates, the first sign of trouble is a support ticket that looks like an isolated fluke instead of a pattern.


By the time someone connects the dots, there's usually a backlog of affected records with no clean way to tell which ones were actually duplicated versus which were legitimately processed twice for other reasons. Untangling that after the fact takes far longer than building the dedup check would have taken up front. This is the part that makes webhook reliability a genuinely senior concern rather than a checkbox: the failure mode doesn't announce itself, it accumulates.


## **None of this makes webhooks the wrong choice**


None of this is an argument against webhooks. Polling is worse: slower, more expensive at scale, and it doesn't eliminate any of these problems, it just hides them behind a different set of assumptions. Webhooks are still the right tool for event-driven integration between systems that don't share infrastructure, and they're not going anywhere.


The point is that "add a webhook" is not a small task, even when the code to receive one is ten lines. Receiving a payload is the small task. Building something that survives duplicate delivery, out-of-order arrival, forged requests, delayed retries, and its own response time, all at once, without anyone noticing when it works, is the real one. That's not a weekend feature. It's infrastructure, and it deserves to be budgeted, reviewed, and owned like infrastructure, long before the first "why did this customer get charged twice" ticket lands in your inbox.
