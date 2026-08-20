---
schema_version: "1.0.0"
document_id: "8ee5a2d8426a32ce7d7a745393f5ff7930002ea9ebfa3b05d6bc3c876729a398"
company_key: "expensify-inc-class-a-common-stock"
company: "Expensify Inc."
source_id: "expensify-inc-class-a-common-stock-rss-4fd0a014ac5a"
canonical_url: "https://use.expensify.com/blog/business-travel-expense-reconciliation"
published_at: "2026-07-20T08:07:16+00:00"
first_seen_at: "2026-07-20T23:19:52.158779+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:09026fe9b45ede7a182aa1f58caf42d766ba2f409c6cf9fb9ca45a7e02c3afc6"
---

# Why business travel expense reconciliation breaks, and how to fix it at the source

## Corporate cards and reimbursement cycles: Why both models fail at reconciliation


Most companies manage business travel spend through one of two models: a central billing card shared across the organization, or a reimbursement cycle where employees book on personal cards and submit expenses after the fact. Both are widely used, but both create the same fundamental problem when it comes to travel expense reimbursement.


The payment happens before the record is complete. Money moves when the booking is made. The context (who traveled, what trip this was, whether it was in policy, who approved it) arrives separately, later, and often incomplete. That gap is where every[month-end reconciliation](https://use.expensify.com/corporate-card-reconciliation) problem begins.


***"The standard credit card statement doesn't give you the whole picture. A card is a payment instrument, not a system of record."***


***—***[Ryan Schaffer](https://www.linkedin.com/in/ryansschaffer/) ***, CFO at Expensify***


### The central billing card problem


Central billing cards solve one real problem: employees don't need individual corporate cards to book travel. But they introduce a structural flaw that becomes obvious fast, especially when travel is last-minute, unscheduled, or international.


One Expensify customer in the crisis-response space learned this the hard way. Their employees travel constantly, often at short notice, to handle emergencies at client sites. Two problems surfaced repeatedly.


**Problem one:** Hotels ask to see the card the reservation was booked with at check-in, a standard[fraud prevention](https://use.expensify.com/blog/preventing-expense-fraud) measure. Employees didn't have the card. It was a central billing card held by the company, not issued to individual travelers.


So, employees fell back on personal cards. Some trips ran long, and some employees ended up carrying $7,000 on their own cards while waiting for reimbursement.


**Problem two:** Booking dozens of last-minute trips across multiple countries on a single card triggered repeated fraud alerts. The card company called the executive whose name was on the account, at all hours, to authorize charges.


When he didn't answer, transactions were declined and employees were left without accommodation in unfamiliar cities.


These aren't edge cases but predictable consequences of a payment model that wasn't built for how corporate travel actually works.


### The reimbursement cycle problem


Reimbursement cycles push a different kind of cost onto finance teams. Instead of one card with fragmented transaction data, there are dozens of individual[travel expense reports](https://use.expensify.com/blog/travel-expense-report) , each one requiring manual matching after the fact.


***"Today they're matching a card line item to a traveler, to a trip, to a receipt, to a GL code, to a policy, after the fact, often with half the evidence missing."***


***— Ryan Schaffer, CFO, Expensify***


The GBTA Foundation puts a dollar figure on that friction: processing a single expense report costs an average of $58 and takes 20 minutes.[One in five reports (19%) contains errors or missing information](https://gbta.org/how-much-do-expense-reports-really-cost-a-company/) , adding another $52 and 18 minutes per correction. For a company processing hundreds of reports each month, that's a lot of money and time spent fixing a process that shouldn't need fixing.


Both models leave finance teams doing archaeology at[month-end close](https://use.expensify.com/blog/month-end-close-how-to-keep-employees-and-admins-happy) , reconstructing what happened from fragments of evidence. The real issue isn't the volume of work but that the work shouldn't exist in the first place.


## What the data problem actually is


The reconciliation problem looks like a process problem,but it isn't. It's a data problem, and it's structural.


A corporate card authorizes a payment. That's all it does. It has no idea whether that charge was for a flight to Chicago or a hardware purchase at an office supply store. It doesn't know whether the trip was approved, whether it fell within the company's[travel expense policy](https://use.expensify.com/resource-center/guides/travel-expense-policy-employees-will-actually-read-plus-free-template) , or who traveled.


That information lives somewhere else, like in a booking confirmation, an approval email, or a receipt. And it arrives, if it arrives at all, through a separate workflow.


### The gap the card can never close


The gap between the payment and the record is exactly where[expense management](https://use.expensify.com/expense-management) overhead lives. Every hour a finance team spends matching transactions to trips, every spreadsheet built to connect card lines to travelers, every month-end surprise: all of it traces back to the same root cause. The card sees the transaction, not the trip.


Adding more “process” on top of a broken model doesn't actually fix the model.


## A third model: Consolidated travel billing


Consolidated travel billing isn't a feature layered onto the existing models. It's a different structural approach to how travel spend moves through a company, and it's now available to US businesses with a bank account on file through[Expensify Travel](https://use.expensify.com/travel) .


### How centralized travel billing works


-


Employees book travel in Expensify Travel as they normally would


-


Trips and their associated expenses are created automatically at the time of booking


-


No personal cards, no shared central card


-


At the end of the month, all bookings settle via a single bill paid from the company's business bank account


-


Every line on the invoice ties back to the underlying booking (flight, hotel, or rail itinerary)


Finance teams can drill from a total down to who traveled, where, which trip, and the receipt behind it, without opening a second system.


***"One statement instead of fifty. It's organized by trip and by traveler, not just by transaction date. Every line ties back to the underlying booking, so you can drill from a total down to who, where, which trip, and the receipt or itinerary behind it, without opening a second system. It's a document for you to review, not a reconciliation project you have to tackle."***


***— Ryan Schaffer, CFO, Expensify***


### What changes for finance teams


What finance teams stop doing:


-


Reimbursing personal-card travel spend


-


Manually mapping card transactions to trips


-


FX reconciliation on mystery merchant lines


What's left is reviewing one statement on a set cadence and catching exceptions that were flagged before the trip happened, not hunting for them during the close. Now, there’s one predictable monthly bill instead of scattered charges across multiple tools.


### A note on hotel virtual card acceptance


Do know that when it comes to[virtual cards](https://use.expensify.com/unlimited-virtual-cards) , acceptance varies by property and region. The US is the most consistent; Europe is less uniform; Asia, South America, and Southeast Asia are more limited.


Expensify works with hotels proactively before check-in, completing authorization forms and providing backup documentation where needed. If a property can't accept a virtual card, travelers are notified in advance so they can rebook or plan accordingly.


## Who this is (and isn't) for


Consolidated travel billing isn't the right fit for every company, and it's worth being direct about that. Here’s who benefits the most and least:


-


**Best fit:** Companies currently using a central billing card, or considering one. It solves the same problems a central billing card was designed to solve (consolidated visibility, no need to issue individual cards to every traveler) while eliminating the drawbacks: the hotel check-in issue, the fraud alert calls, the card-sharing workarounds.


-


**Also a strong fit:** Companies whose[travel reimbursement process](https://use.expensify.com/travel-expense-reimbursement) has become its own administrative burden: employees paying out of pocket, submitting reports, waiting to get paid back. Consolidated travel billing eliminates that cycle, removing out-of-pocket spend for employees and month-end reconciliation overhead for finance in one move.


-


**Less marginal benefit:** Companies that already issue a corporate card to every employee. Consolidated travel billing still improves corporate travel[spend management](https://use.expensify.com/spend-management) and reconciliation, but the case is more about preference than fixing a broken model. Central billing cards exist for a reason: finance and travel managers genuinely like the consolidated view. This is that, without the structural problems that come with it.


Consolidated travel billing is currently available to US businesses with a bank account on file. A short eligibility review gets companies set up within a few business days.


## The reconciliation problem is a booking problem in disguise


Business travel expense reconciliation is painful because the payment model was never designed to produce a clean record.


Corporate cards and reimbursement cycles generate the same outcome. Money moves first, context arrives later, and someone in finance spends their month-end close manually trying to connect the two.


That's not a workflow problem. No amount of better spreadsheets or tighter submission deadlines changes the root cause. The only fix is connecting the booking record to the billing record at the moment of booking, so reconciliation stops being a month-end event and becomes automatic.


That's the structural shift Expensify Travel's Consolidated Travel Billing is built around. When the booking and the invoice live in the same platform, the chain (traveler, trip, receipt, GL code, policy) exists before anyone has to go looking for it.[Realtime travel expense management](https://use.expensify.com/blog/realtime-travel-expense-management) stops being a goal and becomes the default.


Ready to say goodbye to business travel expense reconciliation headaches? Click on the button below to get started.
