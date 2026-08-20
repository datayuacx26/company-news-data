---
schema_version: "1.0.0"
document_id: "5cca12aebc71d459998fc9051fdb2a5b66cbcf79a7c6bc0215bb962fca08031b"
company_key: "yc-getcho"
company: "Getcho"
source_id: "yc-getcho-rss-e8ccd9728c81"
canonical_url: "https://getcho.app/blog/the-90-minute-window-perishables-and-gifts/"
published_at: "2026-05-07T15:30:00+00:00"
first_seen_at: "2026-07-20T23:20:29.886918+00:00"
fetched_at: "2026-07-28T21:14:13.187831+00:00"
content_hash: "sha256:04c9057ef7820ec7a2c86f63218a0668c5e538932cd791adc2c5f1e15d1ad0b7"
---

# The 90-Minute Window: Delivery as a Time Product

# The 90-Minute Window: When Delivery Is Actually a Time Product


A bouquet was ordered for a wife’s office lunch on a Tuesday. The husband ordered it the night before. The flowers arrived on time, in good condition, signed for. The husband called the florist, livid.


The flowers arrived at 4pm. The lunch was at noon.


By every system the florist used to measure success, that delivery worked. The driver picked up. The driver dropped off. The package was photographed at the door. Tracking pinged “delivered.” A standard delivery dashboard logs that one as a green tick.


The husband cancelled his account.


We see this pattern constantly, and it’s why a meaningful chunk of our work is not about getting things from A to B — it’s about getting them there at the *moment* they’re supposed to arrive.


## Time-anchored deliveries are a different product


Most delivery is binary. The package showed up or it didn’t. Within a generous “we said today, it’s today” window, it counts.


A subset of delivery is not binary. The package has a moment when it has to arrive, and arriving early is a different kind of failure from arriving late, but they’re both failures.


- **Flowers for an office lunch** — arrives early, wife sees them an hour before the surprise. Late, husband doesn’t get the moment he paid for.
- **A birthday cake delivered to a school party** — arrives early, the school office signs and the cake sits in a hallway for two hours. Late, the party is over.
- **Sushi for a corporate lunch meeting** — arrives early, sits at room temperature for an hour. Late, the meeting eats whatever’s left.
- **An anniversary gift to a hotel room** — arrives early, the husband isn’t checked in yet. Late, the dinner reservation is gone.
- **Wine for a 7pm dinner party** — arrives at 8 instead of 6:30. The host ate the cheese course alone.


What these have in common: **the customer paid for a moment, not a parcel.**


The fastest carrier in the world doesn’t necessarily win these orders. The carrier that respects the *window* does. ASAP delivery is sometimes worse than scheduled delivery — because ASAP often means “as fast as possible after pickup,” and the recipient isn’t ready that fast.


## What goes wrong when delivery is treated as binary


The default behavior of every gig fleet is “pick up immediately, drive to drop, deliver as soon as the driver arrives.” That works for a pizza. It doesn’t work for a wedding cake.


A few specific failure modes we see when retailers run time-anchored orders through default ASAP dispatching:


- **Early arrivals at offices.** Delivery shows up before the recipient is at their desk. The doorman or front desk signs. The recipient finds the package at 3pm when they meant to receive it at noon. Surprise destroyed.
- **Early arrivals at homes.** Delivery shows up while the customer is still at the office. Package sits on the porch in the sun. Cake melts. Sushi warms. Flowers wilt.
- **Hotel deliveries before check-in.** Hotel front desk holds the package, often in a back room, often forgets to release it on check-in.
- **Restaurant catering deliveries that beat the staff.** Catering arrives at the restaurant at 11am for a 12:30 event. Restaurant staff has to find space for it.
- **Late arrivals because the platform “tried to be fast.”** ASAP dispatch sometimes routes around the requested window because the model thinks earlier is better. It isn’t.


The pattern: **velocity becomes a liability** when the customer asked for a window.


## Hold-for-time vs. ASAP — and why almost no carrier does this well


A delivery platform that respects time-anchored orders has to support what’s usually called **hold-for-time** dispatch. The driver picks up at the right window relative to drop, not whenever convenient. The platform tracks the drop window and works backward.


What good hold-for-time looks like:


1. The customer picks a delivery window at checkout. “Tuesday, 11:45am-12:15pm at this office.” The order knows.
2. The platform calculates the pickup time backwards: drive distance, prep buffer, traffic.
3. The driver isn’t dispatched to pick up until the right moment. ASAP becomes wrong; the *right time* becomes the goal.
4. The driver delivers within the window. Not before, not after.
5. If something delays the driver, the platform proactively notifies the customer with a revised ETA — and ideally asks if a new window works.


We use hold-for-time constantly for time-anchored orders — flowers being the largest single use case. The order goes into a “scheduled” state, and dispatch fires at the calculated pickup time, not at order placement.


What most ASAP-only carriers do instead is dispatch at order placement, hope the driver doesn’t arrive too early, and treat any in-window delivery as a success. That works at scale for binary delivery. It doesn’t work for time products.


## Categories where hold-for-time is the right default


Verticals where retailers should not be running ASAP by default:


- **Florists.** Almost every floral delivery is time-anchored. Office lunches, hospital visits, recovery rooms, condolence drops, wedding ceremonies, anniversaries. ASAP is wrong for >80% of orders.
- **Bakeries.** Cakes for parties. Cookies for gifts. Most bakery customers ordering same-day are buying for an event, and the event has a time.
- **Sushi and time-sensitive food gifts.** Corporate lunches, gift baskets to offices.
- **Wine for events.** Dinner parties, anniversaries, bachelor parties. The host knows when the cheese course starts.
- **Gift retailers.** Anniversary gifts, birthday gifts, holiday gifts, corporate gifts. Almost all anchored to a delivery moment.
- **Hospital deliveries.** Visiting hours, post-surgery moments.
- **Catering for events.** Events have start times.


If your retail vertical is on this list and your same-day program runs ASAP-only, you are losing customers like the one whose wife got 4pm flowers for a noon lunch — and you may not even know, because your dashboard says everything was delivered on time.


## How we handle it


We dispatch a non-trivial volume of time-anchored deliveries every day. The platform supports both ASAP and hold-for-time, and the choice is set per order based on the retailer’s configuration and the customer’s selected window. Florists default to hold-for-time. Bakeries default to hold-for-time. Restaurants on lunch rush default to ASAP. Wine retailers split: ad-hoc orders to ASAP, gift orders to hold-for-time.


When a window is set, the platform tracks the drop window, calculates the pickup time, fires dispatch at the right moment, and surfaces a clear ETA to the customer. If something delays the driver, the customer gets the revised ETA and can confirm whether the new window works — without anyone on the retailer’s side having to send a manual update.


That last part is what makes hold-for-time actually feel like a real product instead of a half-finished feature. The carrier delivering on a windowed promise needs to communicate when the window is at risk, before it’s blown.


## Time-anchored deliveries deserve hold-for-time dispatch


Florists, bakeries, gift retailers, wine merchants — we run hold-for-time as a default, with proactive customer comms when windows shift. Book a demo and we'll show you the dispatch flow.


[Book Your Free Demo →](https://calendly.com/d/cnfr-7br-mkh/getcho-demo)
