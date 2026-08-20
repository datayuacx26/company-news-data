---
schema_version: "1.0.0"
document_id: "bbb9846f3a2d6b8e2d295f779dba72469d7377ee8604a6a65319c946d77a5fc7"
company_key: "yc-courier"
company: "Courier"
source_id: "yc-courier-news-import-df9818472bef"
canonical_url: "https://www.courier.com/blog/notification-cost-at-scale"
published_at: "2026-07-20T17:00:00+00:00"
first_seen_at: "2026-07-25T00:50:30.955928+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:71b1159ccd73a33e1899f9027ee9755eab2cebe5a194fad6fac93aa9bbf1b815"
---

# The real cost of notifications at scale

A single message costs a fraction of a cent to send. Multiply that by a few channels, a growing user base, and every product event worth telling someone about, and "a fraction of a cent" turns into a line item your finance team asks about. The number that looked like a rounding error at 10,000 users behaves very differently at 10 million.


Cost is the question that decides most notification build-versus-buy debates, and it's the one people model worst. This covers what notifications actually cost at scale, whether it's transactional sends or full lifecycle journeys across email, SMS, push, and in-app: the three layers that make up the bill, why it grows faster than you expect, the levers that bring it back down, and where the right platform changes the math.


Up front, since we're Courier, you can probably guess where we land: for most teams, a platform like ours beats building and running your own. You shouldn't take that on faith, though, so the rest of this walks the cost logic layer by layer, for you to check against your own numbers.


## Key takeaways


- **Cost lives in three layers:** provider delivery fees, platform fees, and the cost of building and running the system yourself. Most estimates only count the first.
- **It scales on two axes at once.** Volume (messages sent) and audience (users reached) both push the number up, and platform pricing usually tracks one or the other.
- **The biggest hidden cost is owning it.** Building your own looks cheap because the real cost isn't writing the code, it's maintaining and running it forever, and that never shows up on a spreadsheet.
- **The strongest levers cut volume, not unit price.** Batching, digests, preferences, and picking the right channel save more than shaving a cent off a per-message rate.


## The three layers of notification cost


"Expensive" usually means someone is staring at one part of the cost and missing the other two. Notification cost comes in three layers, and you pay all of them.


**Layer 1: provider delivery cost.** What a carrier or email service charges to actually deliver a message. It depends entirely on the channel: SMS costs real money, email a fraction of a cent, and push, in-app, and chat effectively nothing. Where it applies, it scales with volume, so your channel mix drives this layer far more than your raw send count. You pay it no matter how you build.


**Layer 2: platform cost.** What the platform you send through charges on top of delivery, usually metered one of two ways: by messages sent, or by users reached (often monthly active users). Which one matters more than the headline rate. The same quoted price bills very differently depending on how many messages each user gets, so a per-user plan and a per-message plan that look identical on paper can diverge fast at scale.


**Layer 3: build and run cost.** The layer that never makes the comparison spreadsheet: what it takes to run notifications yourself. Building the integrations is the easy part now. The lasting cost is keeping them alive: retries and failover, stale push tokens, the 2am provider outage, the pager. That recurs every quarter whether or not you send a single extra message.


## What each channel costs at scale


No channel is truly free: your notification platform bills each one as a send. What changes from channel to channel is the delivery cost stacked on top of that send, and it ranges from nothing to the biggest line on your bill:


Channel Delivery cost on top of the send fee Why


Push (iOS, Android, web) None APNs and Firebase don't charge per message


In-app inbox None No third party runs it, so the platform (or you) does


Chat (Slack, Microsoft Teams) None Posting to Slack or Teams APIs is free


Email Low An ESP like SendGrid or SES charges a fraction of a cent


SMS High Carriers charge cents per segment, varying by country


Two things fall out of this table.


**SMS is the only channel that adds a real delivery cost on top.** Everything else is cheap or free to deliver, so your channel mix shapes the bill more than your volume does. Two products sending the same number of messages can have very different curves if one leans on SMS. Save it for messages that need it, and the bill scales gently instead of painfully.


**No delivery cost still isn't free.** Push, in-app, and chat add nothing at the delivery layer, but you still pay your platform to send them, and someone still runs the infrastructure behind them. An in-app inbox has to store messages, sync them across devices, and keep them ready when someone opens the app. You either run that yourself (Layer 3) or fold it into your platform fee (Layer 2). Watch how a platform meters it: some charge per monthly active user, so the bill tracks every user you have. Courier charges a standard send, no per-seat or add-on fee, so you pay to send, not to have users.


## Why "at scale" is where it bites


Notification cost surprises people because it grows on two axes at the same time.


The first axis is **volume per user** . As your product matures, you find more reasons to message someone: onboarding, receipts, activity, alerts, re-engagement. Messages per user per month drifts up quietly.


The second axis is **users** . Growth is the goal, but every new user multiplies whatever your per-user volume happens to be.


Multiply the two and cost compounds. The rough model:


**Monthly cost ≈ (messages per user per month) × (monthly active users) × (blended cost per message) + platform fees + build-and-run cost.**


Put real numbers on it. Take 100,000 monthly active users, 8 messages each per month, so 800,000 sends, on a mix that leans where it should: mostly in-app and push, some email, a little SMS.


- **Delivery (Layer 1):** in-app and push are free and email is a rounding error, so almost the whole delivery bill is the small slice going over SMS, a few hundred dollars a month.
- **Platform (Layer 2):** the 800,000 sends are the line you actually see, low thousands a month at a per-send rate, less with volume discounts.
- **Build and run (Layer 3):** zero if you buy. If you build, even a fraction of one engineer's time, fully loaded, rivals the platform fee every month, and it recurs forever while buying you nothing a vendor wasn't already offering.


The figures aren't the point, the shape is. SMS dominates delivery even as a sliver of your volume, the platform fee is the number you notice, and the engineering you'd spend rebuilding a solved problem is the cost nobody puts on the invoice.


The trap is estimating each factor at today's numbers. Model it at the scale you're planning for, and pull the per-message figure from your real channel mix, since that's what really drives it.


## The levers that actually control cost


Once you can see the three layers, the ways to cut the bill get obvious, and the best ones have nothing to do with negotiating a lower per-message rate.


**Send fewer, better messages.** The cheapest message is the one you didn't need to send. Two related levers cut volume without dropping anything people want: batching and digesting.[Batching](https://www.courier.com/docs/platform/automations/batching) groups related events so a burst of activity becomes one send instead of ten.[Digesting](https://www.courier.com/docs/platform/journeys/nodes/digest) rolls those events into a scheduled summary, a daily or weekly roundup instead of a live drip of pings. Both cut provider cost directly, and both keep people from muting you.


**Honor preferences.** Every message to someone who didn't want it is pure waste: you pay to deliver it, and you pay again in opt-outs and unsubscribes. Real[preference management](https://www.courier.com/docs/platform/preferences) trims volume and protects the channel at the same time.


**Route to the right channel.** Keep SMS for messages that truly need it, and let push, in-app, and chat carry the routine traffic. Matching each message to the cheapest channel that does the job is often the single biggest line-item mover.


**Route across providers.** At scale, one provider per channel leaves money on the table. Sending through several and routing by cost and deliverability is a real lever: a cheaper vendor for bulk email, a least-cost route per country for SMS. Automatic failover on top keeps a provider outage from turning into a pile of retries. The catch is that integrating and maintaining all those providers is its own Layer 3 cost, unless a routing layer handles it for you.


## What building it yourself costs now


AI changed one side of this and left the other untouched. A coding agent can scaffold multi-channel sending, a retry queue, and a preferences table in an afternoon, so the old "months of engineering" price on rolling your own has genuinely dropped. If the only cost were the initial build, building would look better than ever.


But the build was never the expensive part. Owning notification infrastructure costs you everything after the code ships: patching it, keeping providers from failing silently, and carrying it in your team's head forever. AI writes the code, it doesn't run it.[Code is a liability, not an asset](https://www.courier.com/blog/dont-build-your-own-infrastructure) : the capability is what you wanted, the code is the standing cost, and you pay it in attention, the one budget that never grows.


So the real question isn't "can AI build this?" It's "who runs it once it exists?" The move that wins is to point AI at a platform built for it to build on. An agent wires up Courier through its MCP server and SDKs, you get multi-channel sending, preferences, retries, and observability that someone else keeps running, and your team's attention stays on your product. You keep the speed of building with AI without inheriting the infrastructure.


Building your own still makes sense when notifications are part of what you sell, or when volume is huge and predictable enough to amortize a dedicated team. For most teams it isn't, and the point of AI is to ship your product faster, not to leave you maintaining infrastructure you'll still own in two years.


## How to compare platforms without getting surprised


When you evaluate a platform, the sticker price is the least useful number. Ask instead:


- **Which axis does it bill on,** messages or users, and which one is *your* growth axis?
- **What's included** versus metered separately (preferences, digests, in-app inbox, provider connections)?
- **How does the curve look at 10x** your current scale, not today's?
- **What does it remove from Layer 3,** the building and running you no longer own?


## Where Courier fits


Run the three layers back through a platform built for this, and the expensive one drops to near zero:


- **Layer 1, delivery.** Courier sits over 50+ providers, so you route across them, fail over automatically when one degrades, and swap providers on price without touching your code. You get multi-provider economics without maintaining multi-provider integrations.
- **Layer 2, platform.** One flat rate per send across every channel, no per-seat fee, no per-MAU inbox surcharge, and volume discounts as you grow. Your bill tracks what you send, not how many users you have.
- **Layer 3, build and run.** Close to nothing. An agent wires Courier in through its MCP server and SDKs, and the retries, failover, preferences, and observability are Courier's to keep alive, not yours.


That's the answer to "expensive at scale": the layer that usually balloons is the one you stop owning, while the other two stay predictable. For the exact numbers on your volume and plan, check the[pricing page](https://www.courier.com/pricing) , which stays current in a way a blog post can't.


## The bottom line


Notification cost at scale isn't one number, it's three layers stacked on two axes. Estimate all three, model them at the scale you're headed for, and the levers that matter turn out to be the ones that cut volume and eliminate maintenance, not the ones that shave a cent off delivery. Do that, and the "expensive at scale" surprise stops being a surprise.
