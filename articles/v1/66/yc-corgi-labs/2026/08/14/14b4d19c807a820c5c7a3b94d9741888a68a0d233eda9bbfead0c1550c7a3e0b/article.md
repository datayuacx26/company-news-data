---
schema_version: "1.0.0"
document_id: "14b4d19c807a820c5c7a3b94d9741888a68a0d233eda9bbfead0c1550c7a3e0b"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/last-minute-travel-bookings-false-declines"
published_at: "2026-08-07T15:42:48.633+00:00"
first_seen_at: "2026-08-07T17:41:27.322640+00:00"
fetched_at: "2026-08-07T17:41:28.532137+00:00"
content_hash: "sha256:bfcc734a152ba23be9faddc90c01d61b01beb66888f19ea43b1cfd0ab819ce1a"
---

# When Real Travelers Look Like Fraud (and What to Do About It)

## The $1,400 flight that sets off every alarm


Ten hours before takeoff, a longtime customer books a $1,400 international flight for a family member. The name on the ticket doesn't match the card, and the card, the airport Wi-Fi, and the destination all point to different countries. To a fraud filter, this looks terrible.


It's also a completely ordinary travel purchase. The example is invented, but the pattern is one every travel business recognizes.


When a rule blocks a real customer like this, the industry calls it a **false decline** : a legitimate payment stopped because it looked risky. Before you can fix false declines, though, you have to answer a more basic question. Who actually said no?


## Booking late is normal now


Waiting until the last minute isn't fringe behavior. Expedia Group reported that in the second quarter of 2025, the share of searches for trips zero to 13 days out grew 20% quarter over quarter, with international searches in that window up 30%.


Those numbers describe searches, not completed bookings or fraud, and they cover one season. But they make a simple point: booking close to departure is something real customers do all the time.


Travel businesses aren't wrong to be careful, either. Back in 2019, IATA estimated that card-not-present fraud (fraud on purchases where no physical card is present) cost airlines close to $1 billion a year. That figure is dated and says nothing about how many good customers get blocked, but the risk behind it is real.


So the mistake is treating urgency as a verdict in either direction. A last-minute booking isn't proof of fraud, and it isn't proof of innocence. It's a segment worth measuring on its own.


## Why good travelers trip the wire


Travel purchases naturally stack up signals that look suspicious one at a time:


-


A bigger charge than your typical order


-


A card, billing address, and destination in three different countries


-


A new device, or airport Wi-Fi


-


Someone paying for someone else's trip


-


Several travelers or flight segments on one order


-


A departure only hours away


Real fraud can look like this too. That overlap is exactly why a single yes-or-no threshold throws away information.


The card networks know it. EMVCo, the standards body behind card security, worked with IATA and Amadeus on a way for travel businesses to pass ticket, itinerary, and traveler details into the bank's verification process. It doesn't guarantee approvals. Its existence simply confirms that travel purchases need travel context to be judged fairly.


A payer who isn't the traveler is a good example. On its own, that mismatch adds risk. Next to an established account, past successful trips, a familiar device, and a completed verification step, it reads very differently.


## Find out who said no


When a booking dies at payment, one of several things happened, and Stripe's documentation separates them for good reason. Your own fraud rules blocked it, the customer's bank declined it, a technical call failed, or the customer gave up. Each has a different fix, so one blended decline rate points your team at the wrong problem.


Walk the funnel in order:


1.


The customer reaches the payment page.


2.


They attempt to pay.


3.


Your rules, risk model, or review team weigh in.


4.


Any extra verification step completes, fails, or gets abandoned.


5.


The bank approves or declines.


6.


The booking confirms, or a technical error eats it.


7.


Weeks later, the real outcome arrives: a completed trip, a refund, a cancellation, or a dispute.


Keep the raw record of who decided what at each step. A rule change can't fix a bank decline, and checkout polish can't fix an overzealous rule.


## Compare like with like


Once you can tell the failure types apart, group similar bookings and compare them. Start with how far ahead people book. Then add whatever your business lawfully tracks: order size, domestic or international, new or returning customer, device history, verification results, and who's paying for whom.


Traveler and location data is sensitive, so keep collection minimal, access controlled, and retention short.


Here's an invented example of what the comparison looks like. Say bookings made within 24 hours of departure get blocked by your rules 12% of the time, while bookings made a month ahead get blocked 3% of the time. That 9-point gap is a lead, not a conviction.


The follow-up questions do the real work. Were the blocked customers new or returning? Did they finish verification? Did they come back and pay another way? And months later, how many of the approved last-minute bookings actually turned into disputes?


## Give the answers time to arrive


An approved booking isn't a proven-good booking until disputes, refunds, and cancellations have had time to show up. How long that takes depends on your product, your routes, and your customers. There's no universal waiting period.


Blocked payments are harder still. You can't watch what would have happened to a booking that never existed. Later successful attempts, support recoveries, manual reviews, and carefully governed holdout tests give you evidence, never certainty.


That's why no single number settles the question. A Merchant Risk Council session for its 2026 conference lists approval rate, false declines, fraud rate, precision, and review workload as measures that only work together, and it's cross-industry guidance rather than a travel benchmark. Watch recovered revenue and fraud at the same time, always.


## Change one control at a time


When a group of bookings looks over-blocked, make one narrow change and hold everything else still. Stripe's guidance points the same way: review the history a rule would have matched before creating it, and don't force extra verification on everyone, because blanket verification costs you sales.


A working sequence:


1.


Preview what the change would have caught historically.


2.


Define the one rule, model, or verification change you're testing.


3.


Ship it, and touch nothing else.


4.


Track approvals, completed bookings, verification completion, and, once they've had time to arrive, disputes and refunds.


5.


Expand only if both revenue and fraud stay inside your tolerance.


Skip universal thresholds. A returning customer on a known device isn't the same risk as a brand-new account, even when both book the same flight at the same hour.


## When your own data becomes the answer


Generic rules judge your travelers by everyone else's fraud. Once you have enough transaction history and enough settled outcomes, your own patterns can do better, because they show what your legitimate last-minute customers actually look like.


Corgi Labs builds that kind of merchant-specific analysis and decisioning inside your existing Stripe setup. The goal isn't to declare last-minute travel safe. It's to show you where your own controls block real buyers, with the fraud numbers kept in plain view.


Want to see what your last-minute segment is really doing?[Book a Demo](https://www.corgilabs.ai/#book-demo) .


## Sources


-


Expedia Group. **Travel trends Q3 2025: global search and booking insights for travellers.** August 13, 2025.[https://partner.expediagroup.com/en-gb/resources/blog/q3-2025-travel-trends-insights](https://partner.expediagroup.com/en-gb/resources/blog/q3-2025-travel-trends-insights)


-


International Air Transport Association. **Fraud prevention: Strengthening the defences.** March 6, 2019.[https://airlines.iata.org/2019/03/06/fraud-prevention-strengthening-defences](https://airlines.iata.org/2019/03/06/fraud-prevention-strengthening-defences)


-


EMVCo. **Supporting the Travel Sector's Fight Against Transaction Fraud.**[https://www.emvco.com/knowledge-hub/supporting-the-travel-sectors-fight-against-transaction-fraud/](https://www.emvco.com/knowledge-hub/supporting-the-travel-sectors-fight-against-transaction-fraud/)


-


EMVCo. **Travel Industry Message Extension.**[https://www.emvco.com/dynamic/emv-3-d-secure-whitepaper-v2/3ds-message-extensions/travel-industry-message-extension/](https://www.emvco.com/dynamic/emv-3-d-secure-whitepaper-v2/3ds-message-extensions/travel-industry-message-extension/)


-


Stripe. **Fraud prevention rules.**[https://docs.stripe.com/radar/rules](https://docs.stripe.com/radar/rules)


-


Stripe. **Radar for Platforms.**[https://docs.stripe.com/radar/radar-for-platforms](https://docs.stripe.com/radar/radar-for-platforms)


-


Merchant Risk Council. **Don't Say Maybe: Refining Your Decision Accuracy for a New Era of Fraud.** March 18, 2026.[https://merchantriskcouncil.org/learning/resource-center/events/vegas/26/dont-say-maybe-refining-your-decision-accuracy-for-a-new-era-of-fraud](https://merchantriskcouncil.org/learning/resource-center/events/vegas/26/dont-say-maybe-refining-your-decision-accuracy-for-a-new-era-of-fraud)
