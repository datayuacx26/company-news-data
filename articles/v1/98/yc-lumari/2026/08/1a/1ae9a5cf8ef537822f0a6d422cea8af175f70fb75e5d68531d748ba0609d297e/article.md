---
schema_version: "1.0.0"
document_id: "1ae9a5cf8ef537822f0a6d422cea8af175f70fb75e5d68531d748ba0609d297e"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/purchase-order-acknowledgment"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-17T09:30:40.286528+00:00"
fetched_at: "2026-08-17T09:30:42.493630+00:00"
content_hash: "sha256:824db6cb99c6461589881966768c89de17de2088b00717e139fe5043c86c2b63"
---

# Purchase Order Acknowledgment: The Step Most Suppliers Skip

> **TL;DR:** A purchase order acknowledgment is the supplier's formal reply confirming they received your PO and accept the price, quantity, and delivery date, or proposing changes. EDI 855 automated this for high-volume retail and distribution. In direct materials, most of your supply base will never send one, which means the confirmed dates in your ERP are frequently your own assumptions reflected back at you.


The date in your ERP says June 12. Your planner built the build schedule around June 12. The buyer who placed the order in March is three fire drills downstream and hasn't looked at it since.


Nobody at the supplier has ever typed June 12. Or seen it. Or agreed to it. June 12 is a planned delivery date your system calculated from a lead time someone keyed in during a data cleanup two years ago, and nobody has revisited that lead time since.


That gap, between a date your system generated and a date your supplier committed to, is what a purchase order acknowledgment is supposed to close. Most teams file the acknowledgment when it arrives and don't think about the ones that didn't.


## What Is a Purchase Order Acknowledgment?


A purchase order acknowledgment (POA) is a supplier's formal response to a PO confirming they received it and accept the terms, or flagging what they can't meet. It typically confirms the PO number, line items, unit price, quantity, and a committed ship or delivery date. An acknowledgment turns your offer into an accepted order.


## Why EDI 855 Didn't Fix This


[EDI 855](https://lumari.ai/glossary#edi-855) is the X12 transaction set for purchase order acknowledgment, and it works. A supplier's system receives your 850, evaluates it, and fires back an 855 that says accepted, accepted with changes, or rejected, line by line, in minutes, with no human involved.


EDI 855 also solved acknowledgment for the segment of your supply base that was never really the problem.


Think about who's on the other end of an 855: high-volume distributors, contract packagers, anyone with an EDI team, a VAN contract, and enough annual transaction volume to justify both. If you're buying fasteners by the pallet from a national distributor, you probably get clean acknowledgments already.


Now think about who holds your risk. The 14-person machine shop cutting your long-lead housings. The regional heat treater. The single-source castings foundry with a six-month queue and a fax machine that still works. These suppliers are where your schedule breaks, and none of them are sending you an 855. Some of them are running the whole business out of one shared Outlook inbox.


So the industry's answer covers your highest-volume, lowest-risk suppliers beautifully and your highest-risk suppliers not at all. The constraint sits on the other side of the relationship, which means no amount of EDI maturity on your side closes it. Those suppliers would have to invest, and at the volume they do with you, the math doesn't work for them.


Portals were the second attempt, and they went worse. The pitch is reasonable enough: skip EDI, give the supplier a web form, let them click Acknowledge. In practice you're asking a shop foreman to remember a password for a system he touches four times a year, on behalf of a customer who represents 3% of his revenue. He won't. He'll email you back instead, if he replies at all, and now the acknowledgment is sitting in a buyer's inbox where no system can see it. We've written before about why[supplier onboarding through portals keeps failing](https://lumari.ai/blog/supplier-onboarding) , and acknowledgment runs into the same wall.


## The Three Things That All Get Called "Confirmation"


Part of why this stays muddy is that three different events share a vocabulary, and teams conflate them constantly.


There's the technical receipt. In EDI terms that's the[997 functional acknowledgment](https://www.edi2xml.com/blog/understanding-edi-997-functional-acknowledgment-in-electronic-data-interchange-edi/) , which confirms your transmission arrived and parsed correctly. It says nothing about whether anyone read it or agreed to anything. A 997 is a delivery receipt for an envelope.


Business acceptance is the next layer up: the 855, or its email equivalent, where the supplier says yes to price, quantity, and date. It's the only one with commercial weight. The 855 is sent in addition to the 997, not instead of it.


The third layer is the schedule commitment, and it's where direct materials buyers live. Accepting the PO is not the same as committing to a date, and a supplier who acknowledges on day two can still go silent about a slip four months later. An acknowledgment is only good as of the day it was sent.


Most teams get burned at the seam between the second and third. You have an acknowledgment on file, the record looks clean, and the date gets treated as locked for the next four months without anyone going back to re-check it.


## What Should a Purchase Order Acknowledgment Actually Contain?


A useful POA gives you enough structured detail to update your system without a phone call. At minimum:


- Your PO number and revision
- Line-level status: accepted, changed, or rejected, per line rather than for the whole order
- Confirmed unit price against each line, which is where quiet repricing surfaces
- Confirmed quantity
- A committed ship date, and a separate delivery date if the supplier controls freight
- The reason for any exception, in words a buyer can act on


That last one gets dropped constantly, and it saves the most time. "Line 3 delayed" is not actionable. "Line 3 delayed four weeks, raw bar stock allocated to another order, can partial-ship 40 units on the original date" tells a buyer whether to expedite, resource, or tell the planner to move the build.


Note what's not on the list: a signature, a stamp, a formal letter. Plenty of suppliers send back a nicely formatted PDF restating your own PO, which their sales admin considers a professional reply and which gives you nothing to work with. Those six fields in plain text are worth more than a signed document.


## The Cost of an Unacknowledged PO


An unacknowledged PO is an unpriced option you've written to your supplier. Until they accept, they hold all the flexibility. They can come back in six weeks with a different price, or a different date, or nothing at all if the order got buried. Meanwhile you've committed capacity, cash, and a build slot against a number your own system generated.


Your MRP doesn't model that. A planned date and a supplier-confirmed date are the same field to it, carrying no confidence weighting. Material availability, build sequencing, and the promise dates you give your own customers all inherit the assumption without anyone flagging it.


This is why acknowledgment rate belongs in your supplier scorecard alongside on-time delivery and quality, and almost never appears in one. On-time delivery tells you what already happened. Acknowledgment latency tells you what's about to. A supplier whose acknowledgment time has drifted from two days to eleven is telling you something about their order book before a single shipment goes late. If you're building out[supplier performance scorecards](https://lumari.ai/blog/supplier-performance-scorecards) , it's cheap to add and it moves earlier than anything else on the sheet.


Whether it's worth tracking per-supplier or just in aggregate depends on how many suppliers you have and how much of your spend is concentrated. We've seen it argued both ways by people who know what they're doing.


## What Good Looks Like Without EDI


Assume most of your suppliers will never adopt a portal or an EDI connection. Once you accept that, the two expensive answers are off the table, and what's left is email plus process discipline.


Start by deciding which POs need acknowledgment. Not all of them do. Long-lead items, single-source parts, anything feeding a customer commitment, anything above a spend threshold, and any first order with a new supplier. A standing order for shop consumables from a distributor you've used for nine years can go unacknowledged and nothing bad happens. Teams that apply the rule to every line usually end up applying it to none.


Then set a response window and hold it. Put the number on the PO itself. Most suppliers won't read it, but it gives your team something to point at when they follow up.


Ask for the reply in a format you can use: PO number, line status, confirmed price, and committed ship date, in the body of the email. Suppliers comply with this more often than people expect. Typing four lines into a reply is easier than opening a portal.


Then make the follow-up someone's actual job, or stop pretending it happens. It collapses here at almost every company we see. Chasing acknowledgments is repetitive, low-judgment, and endless, and a buyer will always deprioritize it in favor of anything with a person waiting on the other end. It loses the calendar fight to a quality escape every time, and it should. The buyer is making the right call.


## How Long Should You Wait Before Escalating?


Two business days for a standard part, five for anything needing engineering or a quote refresh, then escalate. If a supplier hasn't acknowledged a long-lead or single-source PO within five business days, treat it as a schedule risk rather than an admin backlog item, and get a human on the phone.


The instinct is to give it another week because chasing feels like nagging. Weigh that against the alternative. A phone call in week one costs fifteen minutes. Discovering in week nine that the order was never entered into the supplier's system costs you a line-down event, and every recovery option at that point is expensive. Silence in the first week is the cheapest signal you will ever get about a PO, and it expires quietly. We see it go unused in most[open PO reviews](https://lumari.ai/blog/open-purchase-orders) we sit in on.


## The Short Version


Coverage is the whole problem, and the document is just where it shows up. The suppliers most likely to hurt your schedule are the least likely to have the systems everyone assumes they have, so meeting them on email is the only option that scales down to a 14-person shop.


Get acknowledgment on the POs that matter, in a format you can parse, within a window you enforce, and put the chasing somewhere it will actually get done.


Lumari handles the acknowledgment loop the way most teams wish they had time to: it sends the request, follows up when the window closes, reads whatever format the supplier replies in, pulls out the confirmed price and ship date, and writes them back to your ERP. No portal for your machine shop to log into, no EDI project. If you want to see what your real acknowledgment rate looks like,[we can show you on your own open POs](https://lumari.ai/) .
