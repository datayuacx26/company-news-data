---
schema_version: "1.0.0"
document_id: "e6463d312b2ce0e556796f00237bbfa76cddb1b1745d6dd00b4bad8b7284697a"
company_key: "yc-howdy-com"
company: "Howdy.com"
source_id: "yc-howdy-com-news-import-7de6c48bfb23"
canonical_url: "https://www.howdylatam.com/blog/idempotency-retry-vs-double-charge"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-12T22:01:56.869680+00:00"
fetched_at: "2026-08-12T22:01:58.313817+00:00"
content_hash: "sha256:f61940519d6fd429a36826754b09359b341ecd5cda397a232ed19d93dbfbe6a9"
---

# Idempotency Sounds Like a Checkbox. It's the Difference Between a Retry and a Double Charge.

"Just make it idempotent." You've said it in a design review. You've heard it in a system design interview, usually as the answer that's supposed to close out the question about network failures and retries. It gets said so often, so quickly, that it starts to sound like a setting you flip on, like enabling gzip compression. It isn't. Idempotency is a design decision that touches your data model, your API contract, and how much your team is willing to pay upfront to avoid a category of bug that's brutal to clean up after the fact.


The gap between knowing that idempotency matters and actually implementing it correctly is where a lot of senior engineers get separated from everyone else who read the same system design blog post.


## **What idempotency actually promises, and what it doesn't**


An idempotent operation produces the same result no matter how many times you run it with the same input. Call it once, call it five times because the network hiccupped, the end state is identical. That's the whole definition, and it's short enough that people assume the implementation is short too.


It isn't, because idempotency isn't a property of a single function, it's a property of the entire path a request takes. Your endpoint can be written to be idempotent and your system can still double-charge a customer if the database write and the "mark this as done" step aren't part of the same atomic operation. A retry that lands between those two steps sees a world where the charge hasn't been marked complete yet, tries again, and now you've charged someone twice despite having "idempotent" code on both attempts. The promise only holds if every layer underneath it holds too.


## **Idempotency keys are the mechanism, not the whole solution**


The standard pattern is a client-generated idempotency key sent with the request. The server checks whether it's seen that key before. If it has, it returns the stored result instead of doing the work again. This is the part every tutorial covers, and it's the easy twenty percent of the problem.


The hard eighty percent is everything the tutorial skips. What happens if two requests with the same key arrive at the same time, before either has finished processing? Without a lock or a unique constraint at the database level, you'll process both, defeating the entire point of the key. What happens if the first request is still in progress when the second one checks and finds no stored result yet? You need an explicit "in progress" state, not just "done" and "not seen," or you'll race yourself. What happens six months later when someone reuses an idempotency key by accident, or a client library retries with a stale key from a completed transaction? You need a policy for how long keys live and what happens when they expire.


None of this is exotic. It's just work that doesn't show up in the one-paragraph explanation, and it's exactly the work that determines whether your idempotency implementation holds up under real concurrent traffic or only under the sequential requests you tested with locally.


## **The database transaction boundary is where idempotency actually lives or dies**


An idempotency check that isn't inside the same transaction as the operation it's guarding is decorative, and that's the part most teams get wrong. If you check for an existing key, find none, then do the actual work, then record the key, you've built a window where a concurrent duplicate request can slip through between the check and the record. Under low traffic you'll never see it. Under real load, with retries arriving close together because that's precisely when retries tend to cluster, you will.


The fix is a unique constraint at the database level on the idempotency key, combined with a transaction that does the work and records the key atomically, so the database itself rejects the second attempt instead of your application code trying to catch the race after the fact. This is less elegant to write and it's the difference between a guarantee and a best effort. Senior engineers reach for the database constraint precisely because application-level checks can't close a race condition that the database can close trivially.


## **Idempotency and webhooks are the same problem wearing different clothes**


If you've ever built a webhook handler, you've already run into this. Providers deliver events at least once, which means your handler needs to be idempotent against duplicate deliveries of the same event ID. The pattern is identical to payment idempotency keys: store the event ID, check before processing, use a database constraint to close the race, decide on a retention window. The domains look different but the underlying problem, and the underlying discipline required to solve it, are the same.


This is worth internalizing because it means idempotency isn't a payments-specific concern you can compartmentalize. Anywhere you're on the receiving end of a retry, whether that's a webhook, a message queue with at-least-once delivery, or a client that resends a request after a timeout, you're facing the exact same design problem with a different label on it.


## **Why "we'll add it later" almost never works**


Idempotency is one of those things that's cheap to build in from the start and expensive to retrofit. Adding an idempotency key to an API that's already live means every existing client needs to start sending it, which usually means supporting both the old and new behavior for a transition period, which means the code gets messier before it gets safer. Worse, by the time a team decides idempotency is worth the investment, it's often because a duplicate-processing incident already happened, and now there's a cleanup job to reconcile the bad data on top of the actual feature work.


The teams that treat this correctly build the idempotency key into the API contract on day one, even before there's a documented incident forcing the conversation. It costs a little more time during initial design. It costs dramatically less time than the alternative.


## **Testing idempotency means firing the same request twice on purpose**


Most test suites verify that an endpoint works. Almost none verify that it works correctly when called twice with the same key, at the same time, before the first call has finished. That second scenario is the one that actually happens in production, and it's the one that a single sequential test will never catch, because a sequential test never creates the race condition in the first place.


Testing this properly means writing a test that fires two identical requests concurrently and asserts that exactly one operation happened, not that both requests returned a 200. It means testing what happens when a key is reused after the original transaction failed partway through, since "failed" and "never happened" need to be distinguishable states, not the same bucket. It means testing the expiration boundary: what happens to a request that arrives with a key one second after your retention window closes. None of these are edge cases in the pejorative sense. They're the actual cases idempotency exists to handle, and skipping them in testing means you find out whether your implementation works from a production incident instead of from a test run.


## **The monitoring signal most teams don't have**


Even a correct idempotency implementation is only doing its job quietly if nobody's watching. The number of duplicate requests your system successfully deduplicated is a metric worth tracking on its own, not just as a debugging tool after something goes wrong. A sudden spike in deduplicated requests usually means an upstream system started retrying more aggressively, which is worth knowing about regardless of whether your idempotency layer caught it cleanly.


The absence of this metric is a common gap. Teams build the idempotency check, confirm it works in a manual test, and move on without instrumenting it, which means the first time anyone actually looks at how often it's firing is during an incident review, when the question "was this happening before today" doesn't have an answer. A counter that increments every time a duplicate key gets caught costs almost nothing to add and turns a silent safety net into something you can actually reason about over time.


## **This is a design conversation, not an implementation detail**


The reason idempotency separates senior engineers from everyone who can define it in an interview is that doing it right requires a decision most people don't want to make explicitly: how much complexity are you willing to add to prevent a failure mode that might happen rarely, but costs real money or real trust when it does. That's a tradeoff conversation, not a code review comment. It involves the database schema, the API design, the retry policy of every upstream system you don't control, and an honest estimate of how bad a duplicate actually is in your specific domain.


A duplicated "like" on a social post is an annoyance. A duplicated payment is a refund, a support ticket, and a customer who now double-checks every future charge from your company. Idempotency isn't about treating every operation with the same paranoia. It's about knowing which operations in your system genuinely can't afford to run twice, and being deliberate enough to guarantee, at the database level, that they never will.
