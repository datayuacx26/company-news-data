---
schema_version: "1.0.0"
document_id: "95bc161eaf8268f5acfe46333c2e2b92698263901ad94185d06527f4e14180cb"
company_key: "yc-courier"
company: "Courier"
source_id: "yc-courier-news-import-df9818472bef"
canonical_url: "https://www.courier.com/blog/dont-text-a-truck-driver-whos-driving"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-28T05:46:40.105211+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:ace449b65e5f07e6c9d261dc43d949fb8dc83b0ce971df1b5d4df236e131d6e7"
---

# The federal texting rule your transportation and logistics software is breaking

A single text message to a truck driver whose wheels are turning can trigger a federal fine: up to $2,750 for the driver and up to $11,000 for the motor carrier that allowed it. The FMCSA writes the rule, DOT enforcement backs it, and repeat violations can pull a driver's CDL for months.


Here's the uncomfortable part for anyone building transportation and logistics software. Your platform is often the thing sending that text.


Picture the notification your dispatch platform just fired: "New load available on your lane, tap to accept." It went out the instant the load posted, to every matching driver, one of whom is doing 65 on I-80 with your app open on the dash mount. That's not just a rough user experience. Under federal law it can be a violation, and the carrier running your software is the one on the hook for it.


This bug lives in a surprising amount of the stack: TMS platforms, fleet and telematics apps, freight marketplaces, driver apps, last-mile tools. They treat a commercial motor vehicle (CMV) driver the way a consumer app treats a user, as an address you can push to whenever there's something to say. A CMV driver is not that. Building as if they are creates a legal exposure for your customers, a safety problem on the road, and a product that quietly trains drivers to do something dangerous. This is how to design driver-facing notifications for what a driver actually is: a person operating a heavy vehicle, on a federal clock, moving in and out of coverage, who legally cannot touch a phone while the truck is moving.


## The FMCSA rule most transportation software ignores


In the United States, the FMCSA's federal rule[49 CFR 392.80](https://www.law.cornell.edu/cfr/text/49/392.80) prohibits texting while driving a commercial motor vehicle. It also prohibits the motor carrier from allowing or requiring it.


The detail that catches software teams off guard is the definition of "driving." It means operating the vehicle with the motor running, "including while temporarily stationary because of traffic, a traffic control device, or other momentary delays." A driver stopped at a red light or crawling in a jam is still driving under the rule. It only stops applying once they've moved the vehicle off the road and parked somewhere safe.


The penalties are real. A driver can be fined up to $2,750, and a carrier that allowed or required the texting up to $11,000, both as adjusted for inflation. Repeat convictions disqualify a driver from operating a commercial vehicle: 60 days for a second offense within three years, 120 for a third. There's a narrow exception for contacting law enforcement or emergency services, and that's the whole list.


On top of the federal rule sits a patchwork of state handheld-device laws, most of which also treat a stopped-in-traffic driver as driving. So "wait until they're at a light" is not a loophole anywhere.


You're not the one holding the phone, so why should your software care? Two reasons. If your platform pushes an interactive notification to a driver you can tell is driving, you've built a product that nudges a safety-critical worker toward breaking that rule. And if you're the software a carrier runs on, you're squarely in the "allow or require" half of it: a carrier that mandates drivers use your app to accept loads or confirm stops, with no regard for driving state, has a compliance exposure your product created. "We just sent the notification" is not where you want to be standing.


## A truck driver is not an app user


The assumption to drop is that a recipient is always reachable. For most product notifications that's harmless: someone gets a push or a Slack ping and deals with it when they can. A driver operating a truck can't deal with it, shouldn't look at it, and often can't legally touch the phone to clear it.


What actually makes drivers different, and what your notification logic has to account for:


- They operate the vehicle for up to 11 hours in a shift, and can't legally interact with a screen while it's moving.
- They pass through long stretches with no signal, so a fire-and-forget push can vanish and never be seen.
- Their availability is governed by a federal duty clock, not by whether your app is in the foreground.
- Many aren't reading in English as a first language, so a missed or misread instruction has real operational cost.


Most logistics platforms flatten all of that into "user has a device token, send push." The fix isn't more rules bolted onto each message. It's feeding one signal you almost certainly already have into your notification layer: duty status.


## The signal you already have: ELD duty status


If you're building for trucking, you're already touching electronic logging device (ELD) data, directly or through a telematics integration. Every driver's ELD reports a duty status, and it's the exact signal your notifications need. The four statuses:


- **Off duty**
- **Sleeper berth**
- **On duty, not driving** (fueling, at a dock, doing paperwork, inspecting)
- **Driving**


That last one is your hard gate. When a driver is in the driving state, they are, by the federal definition, someone you must not send an interactive notification to. The other three are windows when a driver can actually read and act.


Layered on top is the hours-of-service (HOS) clock, which tells you not just whether a driver is reachable but how much road time and shift they have left. Federal HOS gives a property-carrying driver an 11-hour driving limit inside a 14-hour on-duty window, after 10 consecutive hours off duty, with a required 30-minute break and a weekly cap of 60 hours in 7 days or 70 in 8. The natural, safe delivery windows for non-urgent driver notifications are the 30-minute break and the off-duty and on-duty-not-driving periods, all of which the ELD already marks.


So the raw material is there. The gap is that most notification systems never read it. They know the driver's phone number and device token; they don't know the driver is currently doing 65.


## Designing duty-status-aware driver notifications


The core move is to treat driving state the way a consumer app treats quiet hours, except the quiet period is defined by the vehicle, not by a fixed clock. When the ELD says driving, non-urgent notifications hold. When the driver goes on-duty-not-driving, off duty, or into the sleeper berth, they release.


Concretely, four rules:


1. **Gate non-urgent driver notifications on duty status.** If the driver is driving, hold the message.
2. **Release at the next safe window,** when duty status flips out of driving.
3. **Batch what accumulated into one digest,** not a burst of ten pings the moment they stop. A pile of notifications hitting at once is its own kind of distraction and its own kind of ignored.
4. **Route the genuinely urgent to a hands-free channel** designed for it: in-cab voice, or an audio prompt through the ELD or a hands-free integration, not a text that begs a glance.


The hard part in practice is classifying your notifications, because not everything can wait for a stop and not everything is urgent. Here's how the common logistics notifications sort out:


Notification Urgency Delivery


Load offer / tender Can wait Hold to next stop, then inbox + push


Dispatch assignment (next load) Can wait Hold to next stop


Dock door / check-in instructions Timed to arrival Release on arrival (already stopped)


Detention clock started Informational Inbox, surfaced at the stop


Document capture nudge (BOL, POD, lumper) Can wait Hold to next stop


Route change, current load Time-sensitive Hands-free voice if en route; else hold


HOS / break reminder Time-sensitive, safety Hands-free or dashboard, timed to the clock


Reroute for a hazard or closure ahead Urgent, safety Hands-free voice


Breakdown / roadside dispatch back to driver Urgent Hands-free voice or call


Notice that most of what a logistics platform sends a driver, load offers, assignments, document nudges, settlement updates, can wait for the next stop with zero downside. The genuinely can't-wait cases are few, and they belong on a channel built for a moving vehicle. Once you sort your notifications this way, the compliance problem mostly dissolves, because you're no longer sending taps-required messages to a driver in motion.


## The channel stack that fits a driver


Push alone is the wrong primary channel for a driver. It's interruptive by design and it dies in dead zones. The stack that actually fits driver-facing logistics notifications:


- **In-app inbox as the backbone.** It's durable. The load offer, the dock assignment, the document nudge all wait in the inbox until the driver is stopped and back in coverage, in order, without duplicates. A driver who spends two hours in a canyon with no signal comes out to find everything waiting, not lost.
- **Push as the doorbell.** It surfaces what's in the inbox, released only when duty status says it's safe to surface.
- **SMS as the backup.** For reach when the app isn't installed or a push doesn't land.


There's a business reason to lean on in-app and push for routine driver traffic, beyond safety: it keeps you out of per-message SMS costs and the A2P 10DLC consent overhead that comes with texting, with SMS held in reserve for the cases where you actually need the reach. And it keeps the durable record (the inbox) as the source of truth, which is what makes the dead-zone case work at all.


## Make your messaging ELD-aware


All of this hinges on one connection: your notification system has to know the driver's duty status. That's a wiring problem between two systems you already run, your ELD or telematics integration and your messaging layer. The data flow is straightforward:


1. Your ELD or telematics provider (Samsara, Motive, or whichever you integrate) exposes duty status and HOS through an API and webhooks.
2. Your backend keeps each driver's current duty status and remaining hours up to date as those webhooks arrive.
3. Your messaging layer reads that state at send time and decides: send now, hold for the next stop, or route to a hands-free channel.


There are two places to put that decision, and the second one scales better:


- **Gate in your own code.** Check duty status before every call to your notification API, and hold or send accordingly. It works, but the rule ends up copy-pasted across every place you send, and it drifts.
- **Push the state into your messaging platform and let it decide.** Attach the driver's duty status, remaining hours, locale, and channel preferences to their profile, define "driving" as a quiet period once, and let the platform apply it to every send automatically.


The second is where a flexible notification layer pays off, and it's the case for putting Courier between your ELD data and your drivers. Courier is built to take arbitrary signal like this: you attach duty status and HOS to the driver's profile or pass it on the send, express the hold as quiet hours and preferences, and every driver notification inherits the gate without you re-checking in code. It's API-first and provider-agnostic, so it doesn't care whether your duty-status feed comes from Samsara, Motive, a raw ELD stream, or your own telematics stack.


Every design decision in this post maps to something you configure in Courier rather than build and maintain yourself:


- **Hold non-urgent messages while driving** becomes preferences and quiet hours, with the driving state as the quiet window.
- **Release one clean summary at the next stop** becomes a digest node that batches what piled up.
- **Survive dead zones** becomes the in-app Inbox, a durable feed that syncs when the device reconnects, so nothing is lost.
- **Push now, SMS as backup** becomes multi-channel routing with automatic fallback, from a single send.
- **Route on behalf of many carriers or fleets** becomes tenants, each with its own branding and rules.


You bring the ELD signal; Courier applies the rule. Because the routing, preferences, channels, and tenants are all configuration rather than hard-coded logic, you wire the ELD-aware behavior once instead of rebuilding it on every message type and every customer.


## Turn the clock into a feature, not just a constraint


Here's the part that flips this from a compliance headache into a product advantage. The same duty-status and HOS data that tells you when *not* to send also powers some of the most valuable notifications you can send a driver.


- **"You're running low on hours to make this appointment."** If the ELD shows a driver won't have the drive time to reach a delivery window, that's a proactive alert that saves a missed appointment and a detention argument.
- **"You're approaching your 14-hour window."** A heads-up before a violation, timed to the clock.
- **Don't offer a load a driver can't legally finish.** If you have the duty data, use it to filter load offers, not just to time them. Sending a driver an offer they'd have to break HOS to complete is worse than sending nothing.


A platform that treats the ELD feed as only a compliance record is leaving these on the table. A platform that treats it as a live signal of driver state can both stay quiet at the right moments and speak up at exactly the right ones.


## Timing is a safety lever, not just a courtesy


There's a reason to get this right beyond avoiding fines. The American Transportation Research Institute found that trucks detained at a stop drove, on average, 14.6% faster afterward than trucks that weren't. Drivers who lose time waiting make it up on the road. Distraction, separately, ranks among the trucking industry's top-10 concerns in ATRI's annual survey.


Notification timing sits right on top of both. A badly timed alert, an interactive ping to a driver in motion, adds distraction to a job that already has too much. A well-timed one, a dwell alert that lets a dispatcher intervene before detention runs long, or an early heads-up that lets a driver plan a stop, takes pressure off. When you design driver notifications around the clock and the duty status, you're not only staying on the right side of a rule. You're building software that makes the road marginally safer, which is not a small thing when your users weigh 80,000 pounds.


## Build the gate once


The reason most systems get this wrong is that duty-state logic ends up scattered, a check bolted onto one message type and forgotten on the next. Modeling it once, at the messaging layer, is what keeps it from drifting: duty status read on every driver send, a "driving" state that behaves like a quiet period, a digest that releases the held pile at the next stop, and an in-app inbox as the durable backbone.


A truck driver is not an app user. Build for that, wire in the ELD signal you already have, and the compliance, the safety, and the driver experience all improve together.


## Frequently asked questions


### Can you legally text a truck driver while they're driving?


No. Federal rule 49 CFR 392.80 prohibits texting while driving a commercial motor vehicle, and motor carriers can't allow or require it. Design driver notifications to hold non-urgent messages until the driver is safely stopped.


### Does the rule still apply when the truck is stopped in traffic?


Yes. The rule defines driving as operating the vehicle with the motor running, including while temporarily stationary because of traffic or a signal. It only stops applying once the driver has pulled off the road and parked somewhere safe.


### How should a logistics app notify a driver who's on the road?


Gate non-urgent notifications on ELD duty status and hold them while the driver is in the driving state, then release them at the next stop, batched into one digest. Use an in-app inbox as the durable backbone, push as the doorbell, and SMS as backup, and reserve hands-free voice for the genuinely urgent.


### What is ELD duty status and why does it matter for notifications?


ELD duty status is the driver's current state (off duty, sleeper berth, on duty not driving, or driving) reported by the electronic logging device you're likely already integrated with. Feeding it into your notification layer lets you send only when a driver can safely read and act, and hold everything else.


---


*This is a companion to our[complete guide to logistics notifications](https://www.courier.com/guides/the-complete-guide-to-logistics-notifications) , which covers the full picture: how one freight event fans out to drivers, dispatchers, shippers, warehouses, and customers, and how to build and operate the whole system.[See chapter 3](https://www.courier.com/guides/the-complete-guide-to-logistics-notifications/trucking-notification-rules-and-constraints) for the other constraints that reshape driver-facing notifications, including consent, alert fatigue, driver language, and dead zones.*


> **Building driver notifications into your logistics platform?**[Talk to a solutions expert](https://www.courier.com/request-demo) or[get started for free](https://app.courier.com/signup) .
