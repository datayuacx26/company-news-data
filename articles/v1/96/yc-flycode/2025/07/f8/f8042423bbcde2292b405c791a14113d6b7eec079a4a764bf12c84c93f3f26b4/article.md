---
schema_version: "1.0.0"
document_id: "f8042423bbcde2292b405c791a14113d6b7eec079a4a764bf12c84c93f3f26b4"
company_key: "yc-flycode"
company: "FlyCode"
source_id: "yc-flycode-news-import-523b281c6a73"
canonical_url: "https://www.flycode.com/blog/stripe-generic-decline-code-what-it-means-why-it-happens-and-how-flycode-recovers-the-revenue"
published_at: "2025-07-27T00:00:00+00:00"
first_seen_at: "2026-07-23T09:50:45.349061+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:58cfcf00a5cd1a4cab2a55c3b746bdadc1a63c5a28624bf579092c104b917b11"
---

# Stripe Generic Decline Code: What it means, why it happens, and how FlyCode recovers the revenue

**Generic Decline (**` **generic_decline**` **) is a “soft” decline** —the issuer didn’t approve the charge but you’re allowed to retry.The root cause is **unclear** by definition, so merchants often waste retries or simply give up.


## What Does Generic Decline Code Mean?


When a payment processor (e.g., Stripe) returns` generic_decline` , the issuing bank refused the transaction but offered **no specific reason** . The card was declined for an unknown reason or Stripe Radar blocked the payment. It’s effectively a “please try again later” response. Because it’s classed as *soft* , the network allows subsequent attempts.


Top reasons it shows up


Likely trigger


Why it matters to SaaS merchants


**Insufficient funds**


Temporary cash‑flow issue on the customer side.


**Wrong card details**


Fat‑fingered digits or an outdated CVV.


**Issuer risk rules**


Bank flags unusual activity and blocks the charge.


**Expired / inactive card**


Card was never activated or has lapsed.


**Network glitches**


Brief outages between gateway and issuer.


*(All are reversible, which is why smart retries work.)*


## Soft vs. Hard: know the difference


When a transaction fails, the response code that comes back from the issuer tells you whether the blockage is **temporary (soft)** or **permanent (hard)** . Treat them the same and you’ll either lose recoverable revenue or annoy customers with pointless retries.


### What exactly is a *soft* decline?


Signal


Meaning


Temporary problem


The card itself is still good; something transient stopped the authorization.


Typical issuer codes


` insufficient_funds` ,` authentication_required` ,` card_velocity_exceeded` ,` issuer_not_available` ,` do_not_honor` (soft flavour).


FlyCode's playbook


Retry intelligently, send outreach when needed or route to an alternate acquirer.


Soft declines cover ~80‑90 % of all failed authorizations in subscription businesses — so getting the retry logic right is where most incremental revenue lives (According to FlyCode research with 500+ SaaS businesses)


### What makes a *hard* decline different?


Signal


Meaning


Permanent problem


The card (or underlying account) can’t ever approve this charge.


Typical issuer codes


` lost_card` ,` stolen_card` ,` expired_card` ,` invalid_account` ,` pickup_card` ,` fraudulent` .


FlyCode's playbook


**Do not retry** ; prompt the customer for a new payment method


### **Why knowing the difference between hard decline and soft declines matter?**


-


**Revenue recovery:** Because most declines start soft, smart retries, account‑updater services, and AI‑driven products like FlyCode can recover a large slice of “lost” payments before they become churn.


-


**Customer experience:** An immediate hard‑decline prompt (“Your card expired — please update”) feels helpful; hammering a stolen card with 10 retries feels spammy and risks brand trust.


-


**Cost control:** Every unnecessary retry burns processor fees and increases chargeback exposure.


## What are Hard decline codes in failed payments?


If the card issuer returns the payment with a hard decline code, then FlyCode can’t automatically retry the payment.
These codes are one of the following:
❌ incorrect_number
❌ lost_card
❌ pickup_card
❌ stolen_card
❌ revocation_of_authorization
❌ revocation_of_all_authorizations
❌ authentication_required
❌ highest_risk_level


In hard declines, we need that the customer will change their card on file. For hard declines, FlyCode adjust the messaging cadence to be faster, use dynamic templates with specific actions (like “update your card”), and sequence outreach based on past engagement.


## How would I know if I can retry a soft decline or a hard decline?


Type


Retry allowed?


Examples


**Soft**


✅ Yes


` generic_decline` ,` insufficient_funds`


**Hard**


❌ No


` stolen_card` ,` lost_card`


## Summary of Soft decline and hard declines playbook


Soft declines


Hard declines


• **Smart retries with FlyCode**


• **Immediate outreach:** Clear, friendly ask for a fresh card or alternative payment method.


• Outreach: FlyCode has robust models powering communications. It means we will coordinate emails with retries - we don’t send emails immediately on soft declines to reduce voluntary churn, we set different email schedules for hard declines (Stripe sends only 1-2)


• **Auto‑switch rails with Stripe orchestration and FlyCode**


• **Cascade acquirers:** Route a retry to a secondary processor with higher local approval rates.


• **Pre‑emptive expiry emails**


• **ML scoring using FlyCode:** Train models to predict which soft declines are likely to clear and skip hopeless ones.


• **Tokenization & network updater:** Auto‑refresh stored credentials to avoid future hard declines on replaced cards.


## FlyCode’s 5‑step recovery playbook


1.


Precision Retry Windows


Our model scores every decline and schedules the **next attempt at the issuer’s highest‑approval hour** , avoiding wasted retries.


2.


Alternate‑Card Fallback


If a second card exists on the customer’s account, FlyCode **auto‑routes the charge** —often salvaging the sale without an email.


3.


One‑Click Card‑Update Link


No log‑in wall. A secure, pre‑filled link lets the payer add a new card from any device in under 20 seconds.


4.


Smart outreach


coordinate emails with retries


5.


Real‑time Metrics


Every recovery attempt is logged. FlyCode surfaces uplift, cost, and ROI so teams can have full visibility


6.


## Cast studies and real examples


**Framer**


[Framer, the web builder for stunning sites, enhancing failed payment recovery by 18%+ with 6% lift in ARR](https://www.flycode.com/case-studies/framer-the-web-builder-for-stunning-sites-enhancing-failed-payment-recovery-by-10)


“Flycode has been a great help in improving our recovered revenue. They do the hard work with all the optimizations, while we can focus on our core product.” Andreas van der Griendt, Product Lead at Framer


**Workiz**


Workiz is the #1 field service management software. The leading platform for field service teams, trusted by over 120,000 pros.. Schedule jobs, dispatch, invoice, track performance, and get paid — all in one place.


[Workiz boost failed payments recovery by 15%.](https://www.flycode.com/case-studies/how-flycode-helped-workiz-to-boost-failed-payments-recovery-by-15)


“We flipped the FlyCode toggle in January and almost immediately saw dunning emails that actually sounded like us—not generic SaaS spam. By the end of Q2, the platform was recovering ~10-15 % more failed payments than Stripe’s default retries and we’d added a shade over alot in net-new MRR we’d have written off. Their outcome-based pricing means my finance team only pays for real wins, and their Slack support channel feels like having a payments analyst on speed dial. If churn from declines is on your dashboard, this is low-effort, high-yield.” Shai Forster, CFO at Workiz


## FAQs


**Is a generic decline permanent?**
No. It’s almost always temporary; intelligent retries or a new card resolve it in the majority of cases.


**How many times should I retry?**
Use FlyCode's product to stay compliant and recover more payments


**Can I just block access until they pay?**
You can, but use it as a last resort. A graceful fallback (partial feature gating, payment plans) preserves the relationship and lifetime value.


**What are decline codes in the context of failed payments?**


Decline codes are short strings returned by the card-issuing bank (and surfaced by Stripe and other payment processors) that explain why a transaction was refused—for example insufficient_funds or do_not_honor. They guide merchants on whether to retry, request a new card, or tell the customer to contact their bank.


**Why do decline codes matter for my subscription business?**


Knowing whether a decline is soft or hard lets you automate the right next step—smart retries for soft declines or prompting for new details for hard declines—so you save revenue and reduce involuntary churn.


**Where can I see decline codes in Stripe and FlyCode?**


Every failed PaymentIntent and Invoice shows a decline_code inside the last payment error. The same code comes through webhooks, so tools like FlyCode can act on it in real time.


**Are decline codes the same across all payment processors?**


No. Some processors pass through the same issuer responses, but each may add gateway-specific aliases. Stripe maps any aliases back to a canonical list, which FlyCode also uses.


**What does the insufficient_funds decline code mean?**


The cardholder didn’t have enough balance at the moment of the charge; Stripe classes it as a soft decline.


**How should I handle an insufficient_funds decline?**


Retry when the customer is likely to have funds. FlyCode’s AI schedules the optimal moment instead of using a fixed retries.


**Why is insufficient_funds the most common decline code?**


FlyCode’s study of 500+ merchants shows it accounts for 40 %–65 % of all declines, largely because customer cash-flow gaps recur every billing cycle.


**How much better does FlyCode recover insufficient_funds declines?**


Across USD 1 billion in volume (March 2024 – June 2025), FlyCode lifted recovery on these declines by 17.6 %–26.2 % over Stripe and Profitwell baselines.The figures come from a June 2025 banking and issuers data cohort study covering 14 months of live data from more than 500 SaaS and eCommerce merchants.


**What does the transaction_not_allowed code indicate?**


The issuer blocked the charge for policy reasons—often a restricted merchant category or cross-border rule. Ask the customer to request approval from their bank, then retry.


**Why would Stripe return invalid_account?**


The card or linked bank account has been closed or entered incorrectly. Stop automatic retries and collect a new payment method or use FlyCode recovery.


**What does do_not_honor mean and should I retry?**


It’s a blanket refusal, sometimes fraud-related. Use AI recovery to find the best time to retry; if it fails again, request a different card.


**Where can I find the full, up-to-date list of Stripe decline codes?**


FlyCode hosts an annotated reference—complete with soft/hard labels and suggested actions—at[flycode.com/stripe/decline-codes](http://flycode.com/stripe/decline-codes) ; each Stripe error object also links to Stripe’s official docs.
