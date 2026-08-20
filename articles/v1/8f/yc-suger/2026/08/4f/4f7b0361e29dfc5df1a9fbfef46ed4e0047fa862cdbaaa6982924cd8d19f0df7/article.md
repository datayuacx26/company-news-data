---
schema_version: "1.0.0"
document_id: "4f7b0361e29dfc5df1a9fbfef46ed4e0047fa862cdbaaa6982924cd8d19f0df7"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/creating-private-offers-without-the-console/"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T12:03:27.187135+00:00"
fetched_at: "2026-08-16T12:03:28.828961+00:00"
content_hash: "sha256:8efd4dd9af618ed93c0f7dba875fd1e6ab49462e5c72b283560c2c6ac4ae3ee3"
---

# Creating Private Offers Without the Console

*Creating a private offer through an API means writing a request and then handling everything that happens after it returns. The call itself is synchronous and boring; the offer’s existence is not. Every marketplace validates asynchronously, which makes the interesting engineering work happen after your 200.*


---


The reason teams move offer creation out of the console is rarely the typing. It is that the data being typed already exists somewhere else — an opportunity in the CRM, an approved discount in the deal desk, a term sheet in the quote tool — and a human retyping it is a human introducing a discrepancy between the quote and the offer.


So the goal is not “automate the console.” It is: given an approved deal record, produce an offer whose terms match it exactly, and know reliably whether that worked.


That second half is where the design decisions are.


---


## **What an offer is, before you automate it**


**An offer is a set of terms; an agreement is an offer a buyer accepted.** AWS states it directly: “An *offer* is a set of terms for the use of a product… An **agreement** is an offer that a buyer accepted.”


The distinction matters for an integration because it tells you what is mutable. Offers are drafts you may still change. Agreements are contracts, and changing one is a separate, more constrained operation. Code that treats “the offer” as a single long-lived object will eventually try to edit a signed contract.


If that vocabulary is new,[offer vs entitlement](https://www.suger.io/resources/blog/offer-vs-entitlement/) separates the three record types before any of this becomes an API problem.


---


## **Authenticate once, correctly**


The Suger API uses OAuth 2.0 client credentials. You create an OAuth app to get a client ID and secret, exchange them at the token endpoint with` grant_type=client_credentials` , and send the resulting bearer token on every request:


```text
POST https://apiv2.suger.cloud/oauth2/token
grant_type=client_credentials


Authorization: Bearer <token>
```


The token is short-lived — one hour. That single fact dictates the shape of your client. A worker that fetches a token at startup and runs for a day will begin failing silently at hour two, and the failure will look like an authorization problem rather than an expiry problem.


Cache the token with its expiry, refresh ahead of the boundary, and never refresh per request.[Authentication for marketplace APIs](https://www.suger.io/resources/blog/authentication-for-marketplace-apis/) covers the equivalent decisions on each cloud’s own API.


Requests are organisation-scoped, with most paths shaped` /org/{orgId}/…` . Treat` orgId` as configuration, not as something derived at call time.


---


## **The asynchronous part nobody plans for**


Here is the shape that surprises people. On AWS, catalog changes are submitted as a **change set** and processed later:


> “The change request is added to a queue and processed. This includes validating information to ensure that it meets the AWS Marketplace guidelines. The validation process can take a few minutes.”


Your` StartChangeSet` call returns a` ChangeSetId` . It does not tell you the offer is valid. To learn that, you call` DescribeChangeSet` and read the result — and the errors that come back there are a different class from the ones the initial call can return.


This has three consequences worth building for from the start:


**A 200 is not a success.** It is a receipt. Persist the change set identifier against your deal record immediately, before doing anything else, or you will have submitted work you cannot look up.


**Poll with backoff, and give up loudly.** “A few minutes” is not a guarantee. A job that polls forever hides a stuck offer; a job that polls three times and stops silently hides it better.


**Asynchronous errors are business errors.** They read like` INCONSISTENT_OFFER_CURRENCY_CODE` or` EXPIRED_OFFERS` — statements about the deal, not about your HTTP client. Surface them to whoever built the deal, in the language of the deal. An alliances manager cannot act on a stack trace.


Offer visibility lags too. AWS notes that an amended offer “will appear on the **Private Offer** page within approximately 45 minutes.” If your workflow notifies the seller the instant the API returns, you have promised something the console will not show for the better part of an hour.


---


## **Idempotency, and why it is not optional**


Any process that creates money-bearing records from an upstream event will, eventually, run twice. A retried webhook, a re-queued job, a human clicking twice.


The Suger API de-duplicates offer creation:` POST /org/{orgId}/offer` is de-duplicated for 60 seconds, so an identical repeated request returns the original response rather than creating a second offer.


Sixty seconds covers the immediate double-submit. It does not cover the job that retries after five minutes because a downstream system timed out. So the durable pattern is the one you own:


- Derive a deterministic key from the deal (opportunity identifier plus a revision counter), and store it with the created offer identifier.
- Check for that key before creating. If it exists, return the existing offer.
- Write the key **before** the API call, not after. A crash between the call and the write is exactly the case the key exists to survive.


Two offers for one deal is not a cosmetic bug. Both are acceptable to the buyer, and whichever they accept first determines what you have actually sold.


---


## **What to validate before you call**


Marketplace validation happens minutes later and in someone else’s vocabulary. Anything you can check locally, check locally.


- **Dates.** An offer whose availability window has already passed will be accepted by your code and rejected by the marketplace.
- **Currency consistency.** Where multiple offers travel together, marketplaces require matching currency — AWS rejects mismatched offer sets with` INCONSISTENT_OFFER_CURRENCY_CODE` .
- **Buyer identifiers.** A malformed account identifier is the single most common cause of an offer nobody can find, because it succeeds and targets no one.
- **Pricing arithmetic.** Confirm the schedule sums to the negotiated total before submission. The marketplace will accept a schedule that is internally consistent and commercially wrong.
- **Terms text.** If the deal uses custom terms rather than standard ones, resolve which document applies before building the offer, not after.[EULAs and custom terms](https://www.suger.io/resources/blog/marketplace-eulas-and-custom-terms/) covers what that choice commits you to.


---


## **Reading and listing**


List endpoints take` limit` and` offset` . Authenticated endpoints are not rate limited per request — usage is bounded by your organisation’s service quotas instead — which removes one worry and adds another: nothing will slow you down before you hit a quota, so a runaway loop is a quota incident rather than a 429.


Cap concurrency in your own client. It is the cheapest guardrail in the system.


---


## **Frequently asked questions**


**Can private offers be created entirely through an API?** Yes. The offer terms, buyer targeting, pricing schedule and expiry are all API-settable, and Suger’s offer endpoints are organisation-scoped under` /org/{orgId}/` . The console remains useful for inspection and exceptions.


**Does a successful API response mean the offer is valid?** No. AWS queues catalog changes and validates them asynchronously, stating the validation process can take a few minutes. Poll the change set for the real result.


**How do I stop duplicate offers?** Suger de-duplicates identical offer creations for 60 seconds. For anything longer-running, store a deterministic key derived from the deal before calling, and check it first.


**How long does a Suger API token last?** One hour. Cache it with its expiry and refresh ahead of the boundary rather than on every request.


**Why can a seller not see an offer that was just created?** Console visibility lags the API. AWS documents that an amended offer appears on the Private Offer page in roughly 45 minutes.


**Are the API rate limits per request?** No. Authenticated Suger endpoints are bounded by organisation service quotas rather than per-request throttling, so client-side concurrency limits are worth setting yourself.


---


## **Takeaways**


- An offer is mutable terms; an agreement is a signed contract. Model them separately or your code will try to edit the wrong one.
- Tokens are short-lived. Cache with expiry, refresh ahead of the boundary, never per request.
- A 200 is a receipt, not a validation. Persist the change set identifier and poll for the real outcome.
- Asynchronous errors describe the deal, not the request. Route them to the person who built the deal.
- Build your own idempotency key from the deal and write it before the call. Sixty seconds of de-duplication covers double-clicks, not retries.
- Validate dates, currency, buyer identifiers and pricing arithmetic locally, because the marketplace will tell you minutes later and in its own vocabulary.


---


Offer generation is one piece of a larger move off the console. See how Suger’s[private offer automation](https://www.suger.io/platform/private-offers/) builds offers from approved deal records across every marketplace, and[running marketplace operations API-first](https://www.suger.io/resources/blog/running-marketplace-operations-api-first/) for the operating model around it.
