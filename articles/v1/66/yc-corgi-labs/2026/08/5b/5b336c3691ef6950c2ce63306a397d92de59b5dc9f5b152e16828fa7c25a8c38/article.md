---
schema_version: "1.0.0"
document_id: "5b336c3691ef6950c2ce63306a397d92de59b5dc9f5b152e16828fa7c25a8c38"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/hotel-booking-platform-approval-rate"
published_at: "2026-08-10T16:00:00+00:00"
first_seen_at: "2026-08-10T16:05:54.052962+00:00"
fetched_at: "2026-08-10T16:05:55.428815+00:00"
content_hash: "sha256:32ace498d0b76dc0cbc8070485bf4fd6a1184a5695937cea25906bf1a659b6bc"
---

# Why One Approval Rate Can't Explain Your Hotel Bookings

## The card said yes. The hotel said no.


Picture a guest booking a $620 prepaid room through your platform. The card is approved, the money moves, and your payment dashboard logs a win. Then the supplier comes back: that rate sold out an hour ago.


Now the guest is calling support about a charge with no room attached, and your payment numbers insist nothing went wrong.


The reverse happens too. A guest reserves a pay-at-the-hotel rate, hands over a card only as a guarantee, and walks away with a perfectly valid confirmation. No charge appears at checkout, so a payment-only dashboard counts the booking as nothing at all.


**One approval rate can't describe both stories.** You need two views that talk to each other: what happened to the payment, and what happened to the reservation. Both examples here are illustrations, but the gap they describe is real on every hotel platform.


## One checkout, four ways the money can move


To the guest, your checkout looks like one storefront. Behind it, the money takes different paths depending on the property, the supplier, the rate, and the cancellation policy.


Four setups show up across the industry:


-


Your platform charges the guest directly and owns the transaction.


-


The hotel charges the guest at check-in.


-


Your platform charges the guest, then pays the supplier with a single-use virtual card.


-


Nobody charges anything at checkout. The card is stored to guarantee the booking.


The industry term for that first case is **merchant of record** : the company whose name sits on the charge and who answers for it. On a hotel platform, that role can change from one booking to the next.


This isn't theoretical. Expedia Group's developer documentation splits bookings into Expedia Collect and Property Collect. Under Property Collect, the hotel may preauthorize the card, run a small validation charge, take a deposit, or charge a cancellation penalty before the guest ever arrives.


Booking.com documents another version. A travel platform charges the traveler, generates a virtual card, and Booking.com charges that card on the reservation's schedule.


Different setups mean different companies make the payment decision, at different times, in different systems. When a booking fails, the explanation lives wherever the decision was made.


## Follow the booking from click to check-in


Before touching a single fraud rule, write down the full journey a booking takes:


1.


The guest picks a rate, which locks the price, currency, payment timing, and cancellation terms.


2.


Your platform determines who will charge the guest, and when.


3.


The guest pays, or hands over a card as a guarantee.


4.


Your fraud checks, any extra verification, and the guest's bank each say yes or no.


5.


Your platform sends the reservation to the supplier.


6.


The supplier accepts it, rejects it, or comes back with a new price.


7.


The confirmation reaches the guest and the property.


8.


The booking survives until check-in, or gets canceled along the way.


9.


Refunds, payouts, and any disputes settle over the following weeks.


At each step, record who acted, when, and why. Without that trail, every lost booking gets filed under "declined," even when no bank ever said no.


## "Declined" is doing too much work


A payment can fail because your own rules blocked it, because the guest's bank turned it down, because the guest gave up mid-verification, or because a request timed out. Those are payment problems, and each has a different fix.


A booking can fail for none of those reasons. The supplier rejects the rate, the reservation never arrives, the confirmation fails to send, or the property cancels after saying yes.


Expedia's documentation calls out that last case directly. Properties sometimes cancel a confirmed booking, often over an invalid card, and a broken notification feed can keep the guest from ever finding out.


An approved payment answers exactly one question: did the charge go through? It says nothing about whether a room is waiting.


## Compare bookings that are actually alike


Averages hide problems. Prepaid rooms and pay-at-the-hotel rooms will never show the same checkout numbers, because one charges money today and the other charges nothing for weeks. Mix them together and a healthy business can look broken.


So split your bookings into groups that behave the same way:


-


Who charges the guest, and when


-


Which supplier provides the room


-


How far ahead the booking was made


-


Whether the trip crosses borders


-


Which currencies are involved


-


Whether the guest is new or returning


-


Whether the guest completed extra verification


Stick to data you lawfully collect, and give traveler, device, and location details proper access controls and retention limits.


Then attach the reservation outcomes to each group. Did the supplier accept? Did the confirmation land? Was the room still there at check-in?


## Watch the gaps between your systems


The most revealing numbers sit where your payment records and your reservation records disagree. Two deserve their own names:


**Paid but unconfirmed.** The guest's money moved, but no usable reservation exists. This is the charge-with-no-room case, and it needs a clear owner and a recovery process.


**Confirmed but unresolved.** A reservation is live, but the payment behind it hasn't settled the way it should. Different risk, same need for an owner.


Around those, track how often payments get approved, how often suppliers confirm, how often confirmations reach guests, and how refunds and disputes settle out over time. No single number gets a veto. A better approval rate isn't a win if supplier confirmations drop at the same time.


## Change one thing, then check both sides


When one group of bookings looks unhealthy, resist the urge to loosen rules broadly. Same-day bookings can be great customers, but urgency alone doesn't tell you who's paying, whether the supplier has the room, or whether the guest is who they claim.


Pick one narrow change instead. For example, adjust one rule for returning guests on devices you've seen before. Then watch the whole chain: approvals, supplier confirmations, check-ins, refunds, and disputes once they've had time to arrive.


Expand only if both the payment side and the booking side stay healthy.


## See the half of the picture that lives in Stripe


If your platform processes guest payments on Stripe, that's the half Corgi Labs can help you read: which payments your rules blocked, which ones banks declined, and where patterns in your transaction data point to lost revenue. Corgi Intelligence surfaces those payment decisions. Your booking and supplier events complete the story.


That boundary is worth keeping honest. It stops a payment fix from taking credit for a room that never confirmed, and it stops a supplier failure from being blamed on a bank.


Curious where your booking payments actually stop?[Book a Demo](https://www.corgilabs.ai/#book-demo) and we'll dig into your payments data with you.


## Sources


-


Expedia Group Developer Hub. **Property Collect Payments.**[https://developers.expediagroup.com/rapid/lodging/booking/property-collect](https://developers.expediagroup.com/rapid/lodging/booking/property-collect)


-


Expedia Group Developer Hub. **Invoices and Booking Receipts.**[https://developers.expediagroup.com/rapid/lodging/manage-booking/booking-receipts](https://developers.expediagroup.com/rapid/lodging/manage-booking/booking-receipts)


-


Booking.com Developers. **Partners Collect the Payment (VCC Flow).**[https://developers.booking.com/demand/docs/payments/models/partner-collects](https://developers.booking.com/demand/docs/payments/models/partner-collects)


-


Booking.com Developers. **Understanding Payouts.**[https://developers.booking.com/connectivity/docs/payments-by-booking-onboarding-api/understanding-payouts](https://developers.booking.com/connectivity/docs/payments-by-booking-onboarding-api/understanding-payouts)
