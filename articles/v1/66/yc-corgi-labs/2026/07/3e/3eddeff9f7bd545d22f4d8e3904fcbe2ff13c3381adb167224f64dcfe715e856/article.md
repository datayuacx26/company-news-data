---
schema_version: "1.0.0"
document_id: "3eddeff9f7bd545d22f4d8e3904fcbe2ff13c3381adb167224f64dcfe715e856"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/payment-metrics-founders-should-know"
published_at: "2026-07-24T18:09:00+00:00"
first_seen_at: "2026-07-24T19:00:40.962467+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:6de9e8de5d4c07279ebf8490ed44ce6ce519d358bf60eb8448d0caf9db3f0aac"
---

# Want Zero Payment Fraud? Accept Zero Payments

## The fastest way to zero fraud


Ask how to get payment fraud to zero and there's one guaranteed answer: accept zero payments. Block everything, and nothing bad ever happens. I opened a recent talk to a room of founders in San Francisco with that line, and it landed because half the room was quietly running a version of that strategy without knowing it.


Fraud tools love one half of the story. "We blocked 1,000 fraudulent orders" makes a great slide. "We blocked 100,000 good orders along the way" does not, so nobody puts it on a slide.


That second number is where your hidden revenue lives. Across the merchants we analyze, roughly an extra 3% of revenue is sitting in payments that were blocked or failed for bad reasons, waiting for someone to look.


## Three rates decide what you keep


Every payment that comes in runs a gauntlet with two gates, and each gate has a rate you should know:


1.


**Acceptance rate** : of all attempted payments, how many your own fraud model and rules let through.


2.


**Authorization rate** : of the ones you let through, how many the banks and card networks approve.


3.


**Payment success rate** : how many survive end to end, including payments people abandoned at checkout.


These compound. Say 90% of payments get past your rules, and the banks trim another 10%: you keep about 80% of attempted revenue, which is actually a decent funnel.


At the talk I asked who knew their payment success rate. The best answer in the room was "we know how to look it up." That's the norm, and it means most founders are running growth experiments on top of a leak they've never measured.


## Blocks you control, declines you mostly don't


The terminology varies by processor, but the useful split is this: a **block** is you saying no, through your own rules or fraud model. A **decline** is the bank or card network saying no after you already said yes.


Blocks are fully in your control, which is exactly why they're where the self-inflicted damage hides. One merchant we spoke with had an old rule that said, in effect, "if Brazil, then block." Nobody remembered writing it. Their revenue in that market was half what it should have been, and everyone assumed the market was just soft.


Declines are trickier because banks don't explain themselves. But there's a pattern we see repeatedly: when your fraud rate improves, your authorization rate tends to improve with it. Issuers score your traffic too. Send them cleaner payments and they say yes more often.


## False declines are the expensive kind of safety


A false decline is a real customer you turned away. It feels free because nothing bad shows up in your fraud reports, but it's the most expensive outcome in payments.


Research reported by Digital Commerce 360 found that 33% of US consumers won't shop again with a merchant that falsely declined them. You paid to acquire that customer, and one bad checkout ended the relationship for good.


The math gets worse over time. For every $100 in false payment declines, our analysis puts the lifetime value lost at over $750, because you lose every future order along with the first one.


Verification friction does the same damage in slow motion. One of our customers was expanding into the US with 3D Secure (the "enter the code we texted you" step, common in Europe) switched on for everything. Adyen's analysis found that applying 3DS to all US transactions cuts conversion by an average of 43%, and sure enough, this merchant was losing roughly half their US revenue to a single setting. Their US expansion looked like a product failure. It was a checkout configuration.


## Not all fraud looks like fraud


Professional fraud is loud and mechanical. A ring buys a batch of stolen cards, tests them against a low-friction checkout (cloud providers are a favorite target), and the cards that survive go buy five handbags each. Machines are good at catching machines: that pattern shows up clearly in your data.


The harder problem is friendly fraud, and it doesn't look like fraud at all. Picture a customer who signs up for your gym, uses it for 11 months, then disputes the whole year with "I never used this service and couldn't cancel."


Javelin Strategy & Research estimates friendly fraud now accounts for 60% to 80% of all chargebacks. Most of your dispute problem isn't shadowy card rings. It's real customers with buyer's remorse and a bank's phone number.


## A dispute costs you even when you did nothing wrong


Quick definitions, because these get conflated: a **dispute** is a customer telling their bank a charge was wrong. A **chargeback** is you losing that fight: the money goes back, the product is already gone, and a fee lands on top. Stripe, for example, passes through a $15 fee per dispute.


For a $20 per month subscription, one dispute erases most of that month's revenue even when the customer is wrong. And merchants lose most of these fights: industry data compiled by Chargeback.io puts overall win rates at 20% to 30%. Prevention beats representment, every time.


The card networks raised the stakes in April 2025. Visa consolidated its fraud and dispute programs into VAMP, the Visa Acquirer Monitoring Program, with enforcement starting that October. The excessive-dispute threshold started at 2.2% of transactions and tightened to 1.5% in April 2026. Cross it and you're in the naughty corner: monitoring, fines, and an acquirer suddenly very interested in your business.


Inside the program, the fines are per item: roughly $10 for every disputed transaction, stacked on the processor fee you were already paying. The dispute doesn't have to be your fault, and you pay whether or not you'd have won it.


One counterintuitive note: not every dispute is an enemy. "Product not as described" disputes are free product feedback. If orders aren't arriving, that's your logistics partner failing, not your fraud model. Read your disputes before you fight them.


## Start with one number


You don't need a payments team to act on this. You need an afternoon:


1.


Find your payment success rate, and the acceptance and authorization rates behind it.


2.


Map the funnel: where exactly do payments die? Blocks, bank declines, 3DS friction, or abandonment.


3.


Audit any fraud rule older than a year, and ask what good revenue it's blocking, not just what bad revenue it stops.


4.


Track your dispute ratio monthly against Visa's 1.5% VAMP line, and treat 1% as your early warning.


If you'd rather have this dug up for you, that's what we built Corgi Intelligence to do: it connects to your Stripe account, shows you these exact metrics and the funnel behind them, and flags the insights worth acting on. Corgi Model then works the other side, blocking fraud instead of buyers, and merchants typically recover about 3% of revenue.


Your payments data already knows where the leak is. Go look.


[Book a Demo →](https://www.corgilabs.ai/#book-demo)


## Sources


-


Digital Commerce 360, "33% of US consumers drop retailers after a false decline. Here's how to prevent those losses."[https://www.digitalcommerce360.com/2020/07/16/33-of-us-consumers-drop-retailers-after-a-false-decline-heres-how-to-prevent-those-losses/](https://www.digitalcommerce360.com/2020/07/16/33-of-us-consumers-drop-retailers-after-a-false-decline-heres-how-to-prevent-those-losses/)


-


Adyen, "Adyen analysis reveals worldwide impact of 3D Secure on transaction conversion rates"[https://www.adyen.com/press-and-media/adyen-analysis-reveals-worldwide-impact-of-3d-secure-on-transaction-conversion-rates](https://www.adyen.com/press-and-media/adyen-analysis-reveals-worldwide-impact-of-3d-secure-on-transaction-conversion-rates)


-


Mastercard Insights, "Friendly Fraud Explained: Risks and Prevention" (Javelin Strategy & Research estimate)[https://www.mastercard.com/us/en/news-and-trends/Insights/2024/what-is-friendly-fraud.html](https://www.mastercard.com/us/en/news-and-trends/Insights/2024/what-is-friendly-fraud.html)


-


Visa, "Visa Acquirer Monitoring Program fact sheet (2025)"[https://corporate.visa.com/content/dam/VCOM/corporate/visa-perspectives/security-and-trust/documents/visa-acquirer-monitoring-program-fact-sheet-2025.pdf](https://corporate.visa.com/content/dam/VCOM/corporate/visa-perspectives/security-and-trust/documents/visa-acquirer-monitoring-program-fact-sheet-2025.pdf)


-


Chargeflow, "Visa VAMP Rules 2025: New Dispute Thresholds Explained"[https://www.chargeflow.io/blog/what-are-visas-new-vamp-rules-a-2025-guide-for-merchants](https://www.chargeflow.io/blog/what-are-visas-new-vamp-rules-a-2025-guide-for-merchants)


-


Chargeback Gurus, "Visa Acquirer Monitoring Program (VAMP)" (per-item enforcement fees)[https://www.chargebackgurus.com/visa-acquirer-monitoring-program-vamp](https://www.chargebackgurus.com/visa-acquirer-monitoring-program-vamp)


-


Chargeflow, "Chargeback Fees by Processor (Stripe, PayPal, Square, More)"[https://www.chargeflow.io/blog/chargeback-fees](https://www.chargeflow.io/blog/chargeback-fees)


-


Chargeback.io, "Chargeback Statistics 2026: 25+ Facts on Fraud, Costs & Trends"[https://www.chargeback.io/blog/chargeback-statistics](https://www.chargeback.io/blog/chargeback-statistics)


-


Saif's talk at the Founder Meetup, San Francisco, May 2026


-->
