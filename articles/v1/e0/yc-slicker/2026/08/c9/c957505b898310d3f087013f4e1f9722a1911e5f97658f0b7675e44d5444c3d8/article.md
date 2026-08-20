---
schema_version: "1.0.0"
document_id: "c957505b898310d3f087013f4e1f9722a1911e5f97658f0b7675e44d5444c3d8"
company_key: "yc-slicker"
company: "Slicker"
source_id: "yc-slicker-news-import-4c2e7cff2c72"
canonical_url: "https://www.slickerhq.com/resources/blog/code-05-decline-recovery"
published_at: "2026-08-14T09:22:52.653+00:00"
first_seen_at: "2026-08-14T22:24:30.888626+00:00"
fetched_at: "2026-08-14T22:24:32.355563+00:00"
content_hash: "sha256:daa3d9aedc41ce0b633d11bc827052784e4656cc19d430477e00d27c4ced8fd9"
---

# Code 05 'Do Not Honor': 4 Causes Decoded (August 2026)

A card decline with no explanation is annoying. A decline code that bundles four completely different failure types into one vague response is a real recovery problem. Code 05 does exactly that, and how you respond to it, whether to retry, when, or whether to reach out to the subscriber instead, depends entirely on the cause hiding underneath it.


**TLDR:**


- Code 05 ("Do Not Honor") hides 4 distinct causes: low funds, fraud flags, account holds, and data mismatches.
- Your recovery path differs for each cause; retrying a fraud-flagged card too fast makes the block worse.
- MAC codes on Mastercard transactions tell you exactly when to retry or stop; ignoring MAC 03 costs $0.10 per attempt.
- Timing retries to payday cycles (1st and 15th for US debit; month-end for UK/Europe) materially affects recovery rates.
- Slicker classifies each code 05 across 40+ variables before deciding whether to retry. Customers have seen roughly 20% relative improvement over baseline, each validated via AABB on their own transaction data.


## What a Generic Decline Actually Is


When a card payment fails, the issuing bank sends back a two-digit response code. Code 05 falls under the ISO 8583 messaging standard and carries the label "Do Not Honor." That label tells you nothing about why the bank refused.


The authorization path runs from the merchant's processor to the acquiring bank, through the card network, and finally to the issuing bank. The issuer's risk engine scores the transaction and selects a response. It can return something specific, like code 51 for insufficient funds, or it can return[code 05 "Do Not Honor"](https://www.slickerhq.com/solutions/failure-reason/do-not-honor) when it refuses to categorize the failure at all. Issuing banks[deliberately withhold the exact decline reason](https://www.adyen.com/knowledge-hub/do-not-honor) to avoid exposing their fraud detection logic to bad actors.


The same code 05 can arrive for four different underlying reasons, with no signal to distinguish them.


## The 4 Reasons Hiding Behind Code 05


Four distinct failures share one response code. That's the core problem with code 05, and[Stripe's documentation on do-not-honor refusals](https://stripe.com/resources/more/do-not-honor-card-refusals) confirms the ambiguity is by design. Here's what can actually be happening:


- Insufficient funds or credit limit reached. The card is valid, the account exists, but there isn't enough balance to cover the charge. This is the most recoverable scenario, a timing problem and not a structural one.
- A fraud or velocity flag. The bank's risk engine flagged the transaction as suspicious, often due to unusual spend patterns, a foreign merchant, or too many charges in a short window. Retrying too quickly makes this worse.
- A temporary account restriction or administrative hold. Banks place holds for reasons ranging from disputed transactions to internal compliance reviews. The card isn't dead; it's temporarily unavailable.
- A data mismatch. Some issuers collapse AVS (Account Verification Service) or CVV failures into a generic code 05 instead of returning a more specific response. The card and funds are fine; the transaction data simply doesn't match what the bank has on file.


The recovery path differs for each. Insufficient funds warrants a well-timed retry. A fraud flag requires a cooling-off period. An account hold often resolves within 24 to 48 hours. A data mismatch may require the subscriber to update their billing details before any retry will succeed.


## Generic Decline vs. Specific Decline Codes


Two factors explain why issuers return code 05 instead of something more specific.


First, specificity is a liability. If an issuer returns code 51 (insufficient funds) every time a fraudulent card is used on a low balance, fraudsters learn to fund cards just enough to probe authorization limits. Returning a vague code 05[keeps the fraud logic opaque](https://knowledge.antom.com/complete-guide-to-generic-authorization-declines-and-decline-codes) , making it harder to reverse-engineer detection thresholds.


Second, legacy infrastructure plays a real role. Many issuing banks run authorization systems built decades ago. When a transaction falls outside a recognized pattern, the system defaults to 05 as a catch-all since it cannot classify what it cannot recognize.


The result: code 05 functions as two different signals. For sophisticated issuers, it is a strategic choice to withhold information. For older systems, it is a technical default when classification fails. The merchant receives the same two-digit code either way.


## Is a Generic Decline a Soft or Hard Decline?


Code 05 sits in a gray zone that most decline classification guides skip over. Whether it behaves like a soft decline or a hard decline depends entirely on the underlying cause, which the code itself never reveals.


When the root cause is temporary (a low balance, a velocity flag that clears after a cooling period, or a transient risk score), code 05 is functionally a soft decline. The card is valid, the account is live, and a well-timed retry has a real chance of succeeding. When the root cause is a permanent restriction or an active fraud block, retrying achieves nothing and can accelerate the problem by signaling suspicious behavior to the issuer.


The code alone cannot tell you which scenario you're in. That requires reading additional signals: whether a[Merchant Advice Code](https://www.slickerhq.com/resources/blog/merchant-advice-codes-payment-recovery) (MAC) accompanied the decline, how the error code changes across subsequent attempts, and what the card type and issuer history suggest. A code 05 returning MAC 03 (Do Not Try Again) is a hard stop. The same code 05 returning MAC 26 (retry after 2 days) is an invitation to come back later.


> Treating every code 05 as retryable burns attempts on unrecoverable cards. Treating every code 05 as a hard decline abandons revenue that a timed retry would have captured.


The classification question is not answerable at the code level. It requires the full signal set around the transaction.


## How Code 05 Appears Across Payment Processors


The network code is universal. How your processor surfaces it is not.


Stripe translates code 05 into` generic_decline` , a human-readable label that strips out the underlying network code entirely. Checkout.com returns a gateway-level code like` 20005` , which maps to network code 05 but preserves the full signal set, including any accompanying Merchant Advice Code (MAC). The same card refusal looks completely different depending on which processor sits in the middle.


That gap matters for subscription businesses running multi-gateway infrastructure. A` generic_decline` from Stripe and a` 20005` with MAC 26 from Checkout.com can represent the same issuer decision, but only one tells you to retry in two days. The other leaves you guessing.


Processor


Gateway Code


Raw Network Code Exposed


MAC Exposed


Actionable Signal


Stripe


` generic_decline`


Buried in` outcome.network_decline_code` (nullable, added Dec 2024)


Buried in` outcome.network_advice_code`


Partial; only if you parse the charge outcome sub-fields


Checkout.com


` 20005` (Do not honour)


Yes, maps directly to 05


Sometimes, when the issuer returns one


Yes, if MAC present


Adyen


` Refused` (refusal reason code 2)


Raw acquirer text in` refusalReasonRaw`


Yes, via` additionalData.merchantAdviceCode`


Yes, if you read the raw fields; the top-level code is a catch-all bucket


Braintree


` 2000` (Do Not Honor)


Yes, via` network_response_code` /` network_response_text`


Yes, via` merchantAdviceCode` (GraphQL)


Yes, if you query beyond the processor code


Cybersource


` 203` (general decline)


Processor code in` processorInformation.responseCode`


Yes, via` processorInformation.merchantAdvice.code` /` codeRaw`


Yes, MAC exposure is explicit


Without a layer that reads both the gateway code and the network code together, identical failures get classified differently across processors, and identical declines end up with opposite recovery decisions depending on which gateway processed the transaction.


## Merchant Advice Codes When Code 05 Fires


When code 05 fires on a Mastercard transaction, any accompanying Merchant Advice Code (MAC) changes the required response entirely. Visa has no equivalent system, so the guidance below is Mastercard-specific.


MAC


Instruction


Retry?


02


Try Again Later


Yes, with caution


03


Do Not Try Again


No. Retrying costs $0.10 per attempt


21


Stop Recurring Payment


No. Cancel the recurring series


24


Retry after 1 hour


Yes


25


Retry after 24 hours


Yes


26


Retry after 2 days


Yes


27


Retry after 4 days


Yes


28


Retry after 6 days


Yes


29


Retry after 8 days


Yes


30


Retry after 10 days


Yes


MAC 21 deserves special attention: it signals a cardholder-initiated cancellation of recurring billing, not a temporary block. Retrying after MAC 21 means ignoring an explicit instruction from the subscriber's bank, which carries real compliance and fee consequences.


## Retry Strategy for Generic Declines in Subscription Billing


Three inputs drive every code 05 retry decision: the accompanying Merchant Advice Code (MAC), the card type, and the geographic payday pattern of the issuing bank.


On timing, consumer debit cards in the US follow biweekly payroll cycles, making[intelligent payday retries](https://www.slickerhq.com/resources/blog/intelligent-payday-retries-scheduling-failed-subscription-payments-slash-passive-churn) on the 1st and 15th the highest-probability recovery windows, with 12:01am optimal when payroll deposits clear. Western Europe and UK cardholders are typically paid monthly, so the best retry window falls within 48 hours of the last working day of the month.


[Visa and Mastercard payment retry rules](https://www.slickerhq.com/resources/blog/visa-mastercard-payment-retry-rules) create a hard ceiling. Visa caps attempts at 15 within 30 days per card; Mastercard allows[10 within 24 hours on soft declines](https://www.slickerhq.com/resources/blog/visa-mastercard-payment-retry-rules) . Burning those attempts on poorly timed retries shrinks your total recovery surface.


A[soft decline retry playbook](https://www.slickerhq.com/resources/blog/soft-decline-retry-playbook) reads code-level signals, issuer history, and payday patterns before scheduling each attempt. A fixed calendar schedule ignores all of it, firing regardless of card type or where you are in the billing cycle. That gap produces material differences in recovered MRR (monthly recurring revenue).


## When to Stop Retrying and Reach Out to the Subscriber


Retrying indefinitely on a code 05 is not a recovery strategy. At some point, the retry window closes and the only remaining path is the subscriber.


The trigger for escalating to outreach is signal quality, not time elapsed. Knowing[when to stop retries earlier](https://www.slickerhq.com/resources/blog/stop-retries-earlier-not-later) matters: if the error code moves across attempts toward something more specific, like a fraud block or stolen card indicator, the issuer is telling you the problem will not resolve on its own. If retries keep returning the same code 05 with no Merchant Advice Code guidance and no error progression, that pattern is your signal to stop.


When outreach is warranted, avoid speculating on the cause in the message. Focus on what the subscriber stands to lose, not the payment mechanics. That framing converts better and protects the brand relationship regardless of which root cause triggered the decline. All outreach should come from your domain, not a third-party sender, since an unfamiliar sender domain drives ignores and spam flags instead of action.


## Reducing Generic Declines Before They Happen


Not every code 05 is inevitable. A meaningful share originates from gaps in the payment request itself, problems you can fix before the transaction ever reaches the issuer's risk engine.


Three areas are worth auditing:


- Payment request completeness. Missing or malformed billing detail and CVV fields increase the probability that an issuer collapses an AVS or CVV mismatch into a generic code 05 instead of returning a more specific response.
- 3D Secure 2 (3DS2) implementation. In markets where Strong Customer Authentication (SCA) is mandatory, a properly configured 3DS2 flow moves liability away from the merchant and reduces risk-flag declines. Misconfigured 3DS data submissions are a documented source of preventable authorization failures.
- Payload review against issuer expectations. Some do-not-honor declines trace back to fields sent incorrectly or omitted entirely, not to account-level problems. Reviewing payment request payloads against issuer expectations can surface data hygiene issues that quietly suppress authorization rates, which also informs how to[build a retry allowlist and blocklist](https://www.slickerhq.com/resources/blog/retry-allowlist-blocklist-decline-codes) from recurring decline patterns.


Fewer code 05s at the authorization stage means fewer ambiguous failures to classify and recover on the back end, which compounds over time as a recovery advantage.


## How Slicker Recovers Generic Declines


Generic declines don't get a single recovery strategy at Slicker. They get classified first.


When code 05 arrives, Slicker's AI reads the full signal set: the gateway code, the network code, any accompanying Merchant Advice Code (MAC), card type, issuing bank behavior, and subscriber payment history. That classification determines whether to retry at all, and if so, when. A code 05 with MAC 03 stops immediately. A code 05 on a US consumer debit card with no MAC fires at 12:01am on the next payday window. The same surface-level failure gets two completely different responses because the underlying signals differ.


That depth requires more than a few inputs. Slicker's AI models weigh over 40 variables per transaction, including time-of-day precision over day-level scheduling. For debit cards,[smart retries vs fixed retry schedules](https://www.slickerhq.com/resources/blog/smart-retries-vs-fixed-retry-schedules-subscription-billing) is a material distinction: hour-level timing affects whether a retry authorizes, and a fixed calendar schedule cannot make that call.


On MACs, Slicker applies them with precision, not rigidly. When MAC 02 fires repeatedly, a naive system loops indefinitely. Slicker tracks the MAC response pattern, breaks the cycle, and escalates to[failure reason dunning cadence](https://www.slickerhq.com/resources/blog/failure-reason-retry-dunning-sequence) outreach instead, preventing fee accumulation and preserving the recovery window before the subscription lapses.


The performance case is verifiable. Slicker customers have seen roughly 20% relative improvement over baseline retry systems, measured via AABB testing on their own transaction data; results vary by billing mix. For a subscription business processing thousands of generic declines each billing cycle, the gap between a flat retry schedule and code-aware, timing-optimized logic is recovered MRR that would otherwise disappear quietly at month end.


## Final Thoughts on Do Not Honor Declines and What to Do With Them


Code 05 is ambiguous by design, and that ambiguity is not going away. What you can control is how much signal you read before deciding what to do with it. Merchant Advice Codes, card type, issuer history, and payday timing all narrow the uncertainty in ways the raw decline code never will. That is the difference between a retry strategy and a guess. If you want to dig into how this applies to your specific billing setup,[the Slicker team is worth a conversation](https://www.slickerhq.com/contact) .


## FAQ


### How should a subscription business handle retrying a generic decline (code 05)?


Start by reading the full signal set around the decline before scheduling any retry: the accompanying Merchant Advice Code (if present), the card type, and the issuer's geographic payday pattern. A code 05 with MAC 03 is a hard stop; the same code on a US consumer debit card with no MAC warrants a retry at 12:01am on the next payday window. Treating every code 05 as retryable burns attempts on unrecoverable cards, while treating every code 05 as permanent abandons revenue a well-timed retry would have captured.


### Is code 05 a soft decline or a hard decline?


Code 05 behaves as either, depending on the root cause the code never reveals. When the failure is a low balance or a velocity flag that clears after a cooling period, it functions as a soft decline and a timed retry has a real chance of succeeding. When it reflects an active fraud block or permanent restriction, retrying achieves nothing. The classification requires reading the accompanying MAC, how the error changes across subsequent attempts, and what the card type and issuer history suggest, not the two-digit code alone.


### How does code 05 appear differently across Stripe, Checkout.com, and other processors?


Stripe translates code 05 into` generic_decline` , stripping out the underlying network code entirely. Checkout.com returns a gateway-level code like` 20005` , which preserves the full signal set including any accompanying MAC. The same issuer refusal looks completely different depending on which processor sits in the middle: a` generic_decline` from Stripe leaves you guessing, while a` 20005` with MAC 26 from Checkout.com tells you to retry in two days. Without a layer that reads both the gateway code and the network code together, identical failures generate opposite recovery decisions across gateways.


### How do you standardize and interpret payment decline codes across multiple processors to avoid double-counting failures?


The key is reading the network code alongside the gateway code, not treating each processor's label as a standalone signal. Checkout.com's` 20005` and Stripe's` generic_decline` can represent the same underlying code 05 from the issuer, but only one exposes the MAC that makes the failure actionable. A recovery layer that normalizes codes across processors into a unified classification, and tracks outcomes at the invoice level instead of the retry-attempt level, prevents the same failure from being counted as multiple distinct declines across systems.


### What Merchant Advice Codes accompany code 05 on Mastercard transactions, and how do they change the retry decision?


MACs shift the required response entirely. MAC 03 means stop immediately; retrying costs $0.10 per attempt under Mastercard's Excessive Attempts program (per Mastercard's published fee schedule). MAC 21 signals a cardholder-initiated cancellation of recurring billing and requires ending the recurring series outright, not merely pausing it. MACs 24 through 30 prescribe specific retry windows ranging from 1 hour to 10 days. Visa has no equivalent system, so on Visa transactions, code 05 must be classified using card type, issuer history, and payday patterns in the absence of explicit network guidance.
