---
schema_version: "1.0.0"
document_id: "543b82ce1cc00708cba897dc03442814d1f72bff3e99be94db2c43aa2fa4ac51"
company_key: "yc-slicker"
company: "Slicker"
source_id: "yc-slicker-news-import-4c2e7cff2c72"
canonical_url: "https://www.slickerhq.com/resources/blog/payment-decline-code-mapping-adyen-stripe-checkout"
published_at: "2026-08-17T09:14:04.215+00:00"
first_seen_at: "2026-08-17T19:13:39.534867+00:00"
fetched_at: "2026-08-17T19:13:41.624605+00:00"
content_hash: "sha256:a40e38ff6b43cb4615c7a3b453883de8c7fad4d408645f34545602cb82800af0"
---

# Adyen vs Stripe vs Checkout.com: Decline Codes (Aug 2026)

Your payment stack might be multi-gateway, but your decline data probably isn't consistent across all of it. A stolen card comes back as` stolen_card` in Stripe,` Stolen Card (24)` in Adyen, and a` 30xxx` hard decline in Checkout.com. Counting failures, building retry rules, and flagging subscribers for dunning all get messier when the same event wears a different label depending on which processor handled the charge. Here's how those labels map to each other, and to the ISO codes underneath them.


**TLDR:**


- Adyen, Stripe, and Checkout.com all receive the same ISO network codes but translate them into different labels, so` do_not_honor` ,` Refused (2)` , and` 20005` are the same underlying` 05` response.
- Surface-level decline codes alone cannot drive retry logic; the MAC and raw network code determine whether to retry, wait, or stop permanently.
- Ignoring MAC 03 on Mastercard declines costs $0.10 per excessive retry attempt; Visa penalties for exceeding 15 attempts in 30 days range from $1 to $25.
- Normalize across gateways using the ISO network code as your anchor, count at the invoice level, and route Stripe Radar blocks separately since no network code exists for them.
- Slicker ingests all three response layers simultaneously and collapses gateway-specific labels into a single classification for invoice-level recovery reporting.


## Why Each Gateway Speaks a Different Decline Language


Every card network runs on ISO 8583, a shared messaging standard that assigns numeric codes to issuer responses. Adyen, Stripe, and Checkout.com all receive those same underlying codes, then translate them into their own proprietary taxonomies before surfacing anything to you.


The same issuer response that Stripe labels` do_not_honor` appears as` 20005` in Checkout.com and` REFUSED` in Adyen. For a subscription business running multiple processors, the average failure rate hits[7.9%](https://www.kaplancollectionagency.com/news/subscription-facts-55-saas-and-b2b-payment-statistics-for-2025/) across industries, climbing to 14.7% in certain sectors. A meaningful share of those failures will surface under three different labels depending on which gateway processed the charge. Attribution gets murky, retry logic misfires, and revenue leaks.


That translation gap is what this article maps.


## How Adyen Structures Decline Information


Adyen's decline response is built in layers, each serving a different purpose. At the top sits the` resultCode` , a high-level status like` Refused` ,` Cancelled` , or` Error` . Below that, the` refusalReason` field provides a human-readable string such as "Not enough balance" or "Refused." The` refusalReasonCode` is a numeric identifier that maps to those reasons and is useful for triggering test scenarios in Adyen's sandbox environment.


The fourth layer is where things get interesting. For Visa and Mastercard transactions processed on[Adyen's own acquiring infrastructure](https://docs.adyen.com/development-resources/raw-acquirer-responses) , Adyen exposes` refusalReasonRaw` and` refusalCodeRaw` : the underlying ISO network code the issuer actually returned before Adyen translated it. That's where you'll find the raw` 51`[code 51 insufficient funds](https://www.slickerhq.com/resources/blog/code-51-soft-decline-retry-timing-mac-codes) or` 05` (do not honor) that the card network sent back.


Adyen's own[refusal reason documentation](https://docs.adyen.com/development-resources/refusal-reasons) explicitly recommends against surfacing raw refusal details to shoppers. The normalization layer exists for a reason: raw issuer codes are ambiguous, often alarming, and not designed for end-user consumption.


## Hard Declines vs Soft Declines in Adyen's Refusal Reason System


Adyen's refusal reasons sort into three practical buckets, and getting the classification wrong costs money in opposite directions: retrying a hard decline risks network penalties; failing to retry a[soft decline](https://www.slickerhq.com/resources/blog/smart-retries-vs-fixed-retry-schedules-subscription-billing) loses recoverable revenue.


Here is how those buckets break down in practice.


### Soft Declines (retry warranted)


- ` Not enough balance` /` refusalReasonCode 2` : insufficient funds; the card is valid, retry after the next payday cycle
- ` Transaction Not Permitted` in certain contexts: can be soft depending on the accompanying network code
- ` Referral` : issuer wants verbal authorization; often retryable in recurring billing contexts


### Hard Declines (stop immediately)


- ` Expired Card` : requires cardholder to update payment details before any charge will succeed
- ` Stolen Card` /` Lost Card` : hard stop; retrying risks fraud escalation and MID (Merchant ID) reputation damage
- ` Blocked Card` /` Restricted Card` : issuer has placed a block; no retry path without cardholder action


### Ambiguous Codes (context required)


` Do Not Honor` and bare` Refused` are where retry logic most commonly misfires. Both can surface from an issuer temporarily declining for risk reasons (soft) or permanently blocking a card (hard). Adyen surfaces both under identical` refusalReason` strings. The actual network code inside` refusalCodeRaw` separates a retryable` 05` from a network-level` 100` that will not resolve. Without reading the raw layer, two identical Adyen responses point toward opposite decisions.


## How Stripe Structures Decline Codes


Stripe's primary decline identifier is a human-readable snake_case string:` insufficient_funds` ,` card_declined` ,` stolen_card` ,` generic_decline` . Where Adyen leads with a numeric` refusalReasonCode` , Stripe leads with a label designed to be immediately interpretable without a lookup table.


Two fields bring Stripe closer to Adyen's raw acquirer layer:` network_decline_code` exposes the underlying ISO code the issuer returned, and` network_advice_code` carries the network's retry guidance. Both fields matter for building accurate retry logic, since` card_declined` alone tells you nothing about whether to retry.


Stripe Radar introduces a third decline category that sits entirely outside the issuer response flow. When Radar blocks a payment before it reaches the network, the charge returns` network_status: not_sent_to_network` with no issuer decline code attached. There is no issuer to negotiate with and no network code to parse. The resolution path is Radar rule configuration, not retry timing, which makes mixing these declines into a standard retry queue a costly mistake.


## How Checkout.com Structures Response Codes


Checkout.com organizes decline responses around a five-digit numeric taxonomy where the prefix carries the functional meaning. Codes in the` 20xxx` range are soft declines where the issuer may authorize on a subsequent attempt. The` 30xxx` range signals hard declines requiring cardholder or issuer action. The` 40xxx` range is risk-triggered, where the outcome depends on the merchant's own fraud rules instead of the issuer's response.


### The Underlying Network Layer


Two additional fields expose the raw network data. The` partner_response_code` field surfaces the scheme's raw authorization response, equivalent to Adyen's` refusalCodeRaw` . Checkout.com also publishes Visa and Mastercard recommendation codes separately, giving explicit retry guidance alongside the decline code itself.


The numeric prefix system makes cross-payment-method comparison cleaner within a single Checkout.com integration, but a` 20005` (do not honor) and a` 20051` (insufficient funds) carry very different retry implications despite sharing the same prefix. That consistency helps internally, but the moment you compare a Checkout.com` 20005` against its Adyen or Stripe equivalent, you're back to manual mapping.


## The Cross-Gateway Mapping: Adyen, Stripe, and Checkout.com Side by Side


The table below maps eight common issuer failure scenarios to their exact field identifiers across all three gateways. The ISO network code column shows the underlying value each gateway receives before translation.


Failure Scenario


ISO Network Code


Adyen` refusalReason` (` refusalReasonCode` )


Stripe` decline_code`


Checkout.com` response_code`


Insufficient funds


51


Not enough balance (2)


` insufficient_funds`


20051


Do not honor


05


Refused (2)


` do_not_honor`


20005


Expired card


54


Expired Card (6)


` expired_card`


30xxx (hard decline)


Stolen card


62


Stolen Card (24)


` stolen_card`


30xxx (hard decline)


Lost card


41


Lost Card (23)


` lost_card`


30xxx (hard decline)


Generic / issuer decline


varies


Refused (2)


` generic_decline`


20005


Velocity / fraud flag


varies


Transaction Not Permitted (20)


` card_velocity_exceeded`


40xxx (risk-triggered)


Radar / gateway block


not sent


Fraudulent (25)


` generic_decline`


40xxx (risk-triggered)


Two rows warrant extra attention. Both "Do not honor" and "Generic decline" can surface as` 20005` in Checkout.com and` Refused (2)` in Adyen, yet they require opposite retry decisions depending on the accompanying network code and advice code. Stripe's` generic_decline` presents the same ambiguity: the label alone cannot distinguish a temporary issuer block from a Radar-triggered hard stop, which is why the` network_decline_code` field is required before any retry logic fires.


## The "Do Not Honor" Problem Across All Three Gateways


The decline code that looks like a single signal behaves like three different ones depending on what accompanies it.


All three gateways receive ISO code` 05` from the issuer. The surface label changes by gateway:` do_not_honor` in Stripe,` Refused (2)` in Adyen,` 20005` in Checkout.com. None of those labels tell you what to do next. The[Merchant Advice Code](https://www.slickerhq.com/resources/blog/merchant-advice-codes-payment-recovery) (MAC) sitting alongside the` 20005` is what actually determines the correct action:


- ` 20005` + MAC 26: soft stop, retry after two days
- ` 20005` + MAC 03: hard stop, do not retry under any circumstances
- ` 20005` + raw network code` 100` : permanent denial, no retry path


Merchants who treat every` 20005` or` do_not_honor` as a uniform category face a costly choice. Retry too aggressively and Mastercard charges $0.10 per attempt on MAC 03 violations. Stop too early and you abandon payments that would have authorized on day two. Reading the MAC and network code before any retry decision fires is the only reliable path forward.


## Mastercard Merchant Advice Codes and How Each Gateway Surfaces Them


Mastercard issues MACs alongside certain decline responses to give merchants explicit guidance on next steps. The full set matters for retry logic, and each code carries a distinct instruction.


- MAC 01: new account information available; trigger a card updater flow before retrying
- MAC 02: try again later (no specific window; defer to a more specific code if present)
- MAC 03: do not try again; hard stop with a $0.10 per-attempt Mastercard penalty if ignored
- MAC 21: stop recurring payment; this is a cardholder-initiated cancellation, not a generic refusal, and requires cessation of all further retry attempts
- MAC 24: retry after 1 hour
- MAC 25: retry after 24 hours
- MAC 26: retry after 2 days
- MAC 27: retry after 4 days
- MAC 28: retry after 6 days
- MAC 29: retry after 8 days
- MAC 30: retry after 10 days
- MAC 40: consumer non-reloadable prepaid card
- MAC 41: consumer single-use virtual card number


Two practical constraints shape how useful these codes are. Not all Mastercard issuers return a MAC, so your retry engine cannot depend on one being present. Visa also publishes no comparable set, meaning MAC-based logic applies only to Mastercard transactions.


Each gateway surfaces the MAC differently. Adyen delivers it automatically in` additionalData.merchantAdviceCode` on the authorization response. Stripe exposes it through` network_advice_code` . Checkout.com surfaces it through its recommendation codes endpoint (not inline on the decline response itself), which can require a separate API call depending on your integration pattern.


## Visa Retry Rules and How They Appear Across Gateways


Visa structures its retry guidance around four categories: permanent declines (no retry under any circumstance), temporary declines (retry permitted), declines requiring updated card information before any attempt, and a catch-all where retry is permitted. Unlike Mastercard's Merchant Advice Code (MAC) system, Visa publishes no time-specific retry windows. The guidance is directional, not prescriptive.


Network penalties apply when merchants retry outside these boundaries.[Visa and Mastercard payment retry rules](https://www.slickerhq.com/resources/blog/visa-mastercard-payment-retry-rules) cap attempts at 15 within 30 days per card and transaction amount for Visa; Mastercard allows 10 retries within 24 hours on soft declines. Exceeding either threshold triggers penalties ranging from $1 to $25 per excessive attempt.


Each gateway surfaces Visa's categorization differently:


- Checkout.com publishes explicit Visa recommendation codes through a dedicated endpoint, separate from the inline decline response.
- Adyen surfaces raw Visa response codes in` refusalReasonRaw` on its own acquiring infrastructure, giving you the scheme code directly.
- Stripe's` network_advice_code` field delivers equivalent Visa guidance alongside Mastercard MACs through the same field, which simplifies integration but requires you to identify which network issued the advice before acting on it.


## Normalizing Decline Codes Across Multiple Gateways


Running Adyen, Stripe, and Checkout.com simultaneously creates a reporting problem that compounds the retry logic problem. Roughly[15% of credit card payments](https://wallid.co/blog/tpost/7j3z2hljp1-payment-decline-rates-by-industry) tied to recurring billing are declined. At scale, a single subscriber failure can generate separate records across three gateways, each using different label strings, making unique failed invoice counts unreliable without a normalization layer above them.


Three steps make that layer workable in practice:


- Build a canonical failure taxonomy that maps each gateway's surface codes to a shared internal classification:[soft retryable, soft non-retryable](https://www.slickerhq.com/resources/blog/soft-decline-retry-playbook) , hard, and ambiguous requiring Merchant Advice Code (MAC) context before a retry decision fires.
- Count at the invoice level, not the transaction level. A single failed invoice retried four times across two processors creates multiple transaction records; collapsing to invoice-level identifiers measures actual subscriber impact, not raw attempt volume.
- Use the ISO network code as the closest universal anchor. Gateway labels diverge, but the underlying` 05` or` 51` the issuer returned does not. Normalizing on that raw code before applying any[retry allowlist and blocklist](https://www.slickerhq.com/resources/blog/retry-allowlist-blocklist-decline-codes) removes the translation noise each gateway introduces.


This anchor breaks down in one scenario: Stripe Radar blocks, where no network code exists. Those require a separate classification path based on` network_status: not_sent_to_network` , since no issuer code is present to anchor on.


## How Slicker Uses Decline Codes Across Adyen, Stripe, and Checkout.com


Slicker's retry engine ingests all three response layers simultaneously: the gateway code, the raw network code, and the Merchant Advice Code (MAC). That combination drives the per-transaction decision in ways no single field can.


The same Checkout.com` 20005` response produces opposite outcomes depending on what accompanies it. MAC 26 schedules a retry in two days. MAC 03 stops all attempts and routes to[failure reason dunning outreach](https://www.slickerhq.com/resources/blog/failure-reason-retry-dunning-sequence) requesting updated payment details.


Hard declines follow a different path. A` stolen_card` from Stripe or` Lost Card (23)` from Adyen bypasses the retry queue entirely. Where cardholder action is genuinely required, dunning is triggered with messaging specific to the failure reason, not a generic payment-update prompt.


For finance and retention teams reviewing their[subscription payment retry strategy](https://www.slickerhq.com/resources/blog/complete-payment-retry-strategy-subscription) , Slicker's dashboard surfaces normalized, human-readable failure reasons across all connected gateways. A` 20005` , a` Refused (2)` , and a` do_not_honor` that all originated from the same ISO` 05` response collapse into a single classification, so invoice-level recovery counts reflect actual subscriber impact, not retry attempt volume scattered across three different label systems.


## Final Thoughts on Cross-Gateway Decline Code Mapping for Subscription Businesses


Three gateways, three label systems, one underlying ISO code from the issuer. The translation noise each gateway adds is manageable once you know where to look: the raw network code and the Merchant Advice Code carry the actual retry signal, and the surface label is mostly there for human readability. Your retry engine and your reporting both get more accurate the moment you normalize on that raw layer instead of the gateway string. If you want to see how Slicker handles this across Adyen, Stripe, and Checkout.com simultaneously,[contact Slicker](https://www.slickerhq.com/contact) .


## FAQ


### How do you standardize payment decline codes across Adyen, Stripe, and Checkout.com without double-counting failures across retry attempts?


Build a canonical failure taxonomy that maps each gateway's surface codes to a shared internal classification using the ISO network code as the anchor. A` Refused (2)` from Adyen, a` do_not_honor` from Stripe, and a` 20005` from Checkout.com can all originate from the same underlying ISO` 05` response. Count at the invoice level, not the transaction level; a single failed invoice retried across two processors creates multiple transaction records, and collapsing to invoice-level identifiers measures actual subscriber impact, not raw retry attempt volume.


### How does Slicker use decline code data from Adyen, Stripe, and Checkout.com to decide whether to retry or stop?


Slicker ingests all three response layers simultaneously: the gateway code, the raw network code, and the Merchant Advice Code (MAC). The same Checkout.com` 20005` response produces opposite outcomes depending on what accompanies it; MAC 26 schedules a retry in two days, while MAC 03 stops all attempts and routes to failure-specific dunning outreach instead. Hard declines such as` stolen_card` from Stripe or` Lost Card (23)` from Adyen bypass the retry queue entirely, preventing the network penalties that fire when merchants retry unrecoverable codes.


### What configuration settings need to be aligned when running Slicker retries alongside Stripe Billing?


You must extend Stripe's default subscription cancellation grace period to match Slicker's multi-week retry windows; if Stripe closes a failed subscription before Slicker completes its retry schedule, those recovery opportunities are permanently lost. For full orchestration deployments where Slicker manages grace periods and edge-case handling, Stripe's native Smart Retries must be disabled entirely (not run in parallel), since concurrent execution conflicts with Slicker's subscription-level logic. India card payments require separate attention: Stripe does not perform automated retries on Indian cards, making Slicker's retry logic the only recovery path in that market, not an incremental improvement over a baseline.


### Does a smart retry platform surface normalized, human-readable decline codes across Adyen, Stripe, and Checkout.com, or does it expose raw gateway-specific codes?


A well-built retry platform does both and translates between them. Slicker harmonizes gateway decline codes across connected processors and surfaces human-readable failure reasons in dashboards, so your team never has to interpret a` 20005` , a` Refused (2)` , and a` do_not_honor` as three separate signals. The raw network code is preserved internally as the anchor for retry decisions; what finance and retention teams see is a single normalized classification, so invoice-level recovery counts reflect actual subscriber impact, not retry attempts scattered across three different label systems.


### How does Slicker handle account updater data from Adyen when processing hard decline codes like lost or stolen card?


When Adyen returns a hard decline such as` Lost Card (23)` or` Stolen Card (24)` , Slicker routes the invoice out of the automated retry queue immediately, since the card instrument itself is the problem and not a temporary authorization issue. Where account updater data signals that new card details are available, MAC 01 triggers a card updater flow before any retry attempt fires. When no updated instrument is available and the failure requires cardholder action, Slicker triggers dunning outreach with messaging specific to that failure reason, not a generic payment-update prompt that gives the subscriber no actionable guidance.
