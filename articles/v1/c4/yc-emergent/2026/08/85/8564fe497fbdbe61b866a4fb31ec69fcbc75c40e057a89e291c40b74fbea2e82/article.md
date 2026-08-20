---
schema_version: "1.0.0"
document_id: "8564fe497fbdbe61b866a4fb31ec69fcbc75c40e057a89e291c40b74fbea2e82"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-fc8f7e880d65"
canonical_url: "https://emergent.sh/blog/how-our-pricing-engine-evolved-from-an-if-ladder"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-14T20:06:54.154269+00:00"
fetched_at: "2026-08-14T20:06:55.252693+00:00"
content_hash: "sha256:c94aa00466c3a943e2757d6673b44aaff190ee152f7f216192c41367ea960d85"
---

# How Our Pricing Engine Evolved from an If-Ladder

Last month Flux: our` Engine` , answered 169 million versions of one question: does this person, buying this service, right now, get a discount on it?


Each answer is computed from current inputs. Flux evaluates stack of facts available when the page loads: the request country, the accountʼs plan, warehouse-derived lifetime spend, and a propensity score for the likelihood of a top-up. It repeats the evaluation for every item on the page. One itemʼs result cannot determine anotherʼs.


A campaign request can fit in one sentence: 20% off the 500-credit tile for repeat top-up users in the US, paying through one gateway, during the nine days around Black Friday. In code, those clauses cross user, billing, data, and time boundaries. Put them in checkout, and checkout still knows about Black Friday in March.


We wrote that code multiple times before we admitted it was a product.


## THE SHAPE OF THE PROBLEM


**Every tile on the screen is a separate question.**


Every pricing surface has this shape. A catalog of things you can sell, a person looking at it, and a handful of campaigns that apply to some pairings of the two. Ours happens to be credit bundles and subscription plans. Yours might be seats, add-ons, shipping tiers, or storage. The count does not matter. What matters is that the page cannot show a price until something has decided, one item at a time, whether this particular person gets a discount on that particular thing.


The obvious implementation lives in the payments service. Load the active campaigns, loop over the tiles, and for each pair check the conditions in an if-ladder: right country, big enough amount, campaign still running, user has topped up before.


That version works. We shipped it, and it was the correct call at the time.


Here is what it grows into. The following is a compressed facsimile of our own checkout path, renamed but structurally honest, after four campaigns had landed in it:


None of that is a payments concern. Every branch in it is a marketing decision that happens to be written in Python and shipped by the team that owns money movement. The comment declaring the priority order is load bearing, because the order is not expressed anywhere a person could read it except that comment.


The cost arrives later, and not where you expect. Every new campaign becomes a code change riding the billing deploy train. Then the growth team needs the same answer on a different surface, writes the ladder a second time, and gets it subtly wrong, because nothing forces the second copy to agree with the first. Now two services disagree about who qualifies, and the bug arrives as a sentence no engineer wants to read: the app showed me 20% off and checkout charged me full price.


## THE MODEL


**Separate the item, the audience, and the exposure.**


Flux evaluates eligibility through three independently authored predicates.


The rule lives on The question it answers Changes when


The offer Does this *item* qualify? Is it a big enough purchase, in the right window? a campaign is designed


The segment Is this *person* the kind of customer we mean? High spenders in the US, accounts on a trial, anyone who churned last month. growth defines an audience


The rollout May this audience see it yet? as launch exposure expands


The last two look similar on a whiteboard, but the schema gives them different vocabularies. A segment may use any of the 33 attributes. A rollout is limited to only a few exposure attributes like: internal user, tier, user id, timestamp etc.


That is what makes a staged launch possible. A discount goes live to internal users on the real production path with real money, sits there while someone watches whether anyone takes it, then opens to a tier, then to everyone the segment already described. The audience never changes. Only the door moves.


Two of the three are append-only. Segments have no update endpoint at all, and an offer’s benefit cannot be edited after it is created; you supersede either one by writing a new row and archiving the old. Dates, priority and status still move, because those are operational. The discount itself never does. A redemption from March still points at the exact benefit that was granted, and an audience defined in May still means in November what it meant then, which is how we avoid answering “what did this customer actually get” by reading a changelog.


An offer is visible when its own rule passes and at least one rollout passes. A rollout passes only when both its segment and its exposure condition pass: offer rule AND ((segment AND exposure) OR ...) . Another audience can therefore be added without changing the first.


That formula also sets the execution order. Because the whole predicate is an AND, the cheap half can go first. Flux loads the active offers once and filters them in memory by date window and offer rule, with no network calls. If nothing survives, the resolver returns without touching the database, which on most requests is what happens. Only then does the second pass load rollouts and segments.


More important than the order is the resolver's default state: an empty list. It appends an offer only after an affirmative match, and never starts from everything and filters down.


That sounds like a stylistic preference. It is the difference between a bug that shows an offer to the wrong people and a bug that shows an offer to nobody. An offer with no rollout is invisible. An archived segment is a tombstone that fails every rollout attached to it. A rule that throws while evaluating gets logged and treated as false, so a malformed rule hides an offer rather than returning a 500 to the checkout page. Every failure mode we could think of collapses in the same direction, and it is the boring direction.


A segment is a JSON document:


The evaluator and its fifteen built-in operators are about 800 lines of hand-written Go, with no rule library underneath.


The same segment can then gate a plan bundle, a collection, or a discount without being reimplemented. When growth wants the same audience for a plan bundle instead of a discount, nothing is rewritten.


## VOCABULARY IS CONFIG, NOT CODE


Here is the part we would tell another team to steal.


The set of things a rule may talk about is a growing list of 33 attributes in a YAML file, compiled into the binary at build time. Country, continent, subscription status, payment gateway, lifetime value, hours since first subscription, propensity score. Adding one is a diff to that file and nothing else.


Vocabulary size does not affect evaluation cost. Attribute lookup is a map read, and work scales with the nodes in the rule tree rather than the number of attributes declared in YAML. 10x more attributes would not add work to a resolve. The harder limit is semantic: defining each attribute precisely and keeping its meaning stable.


The version of this you probably have is worse, and we had it too: a migration, a struct field, a parser change, a deploy, and a backfill, per attribute. At that cost, teams stop extending the vocabulary, and the targeting language freezes around whatever mattered in month one.


The practical gain is cheap reversibility. On one day in July, we added two attributes, narrowed the options on one, renamed the other, and deleted the first. Four changes of mind took about eight hours. Each was a one-file diff, with no schema migration or coordinated rollout. If each change had required a migration, we would have delayed the decision and likely kept the wrong abstraction longer.


This is the part of shipping fast that does not come from anyone working faster. It comes from the common change being small enough that nobody has to schedule it: no migration, no coordination between two teams, no deploy train, no design review for a dropdown value.


### WHAT IT COSTS TO RUN


Flux is ten pods holding 121 MB between them. At the 30-day peak the whole deployment used 0.77 CPU cores. That is the entire operating cost of 169 million resolve requests over those 30 days, 99.7% of which finished in under 20 milliseconds. In the same timeframe we observed zero 5xx errors or downtimes.


Flux's only explicit concurrency component is a bounded worker pool: twenty lines, no dependencies. It earned its place when we added a batch endpoint so a client rendering a topup ladder could make one request instead of six. The first version gave each item its own goroutine, making concurrency proportional to batch size, and we replaced it with a semaphore-bounded pool in the same pull request, before it reached production.


Goroutine-per-request came free with the standard library, so there was never a concurrency model to choose. That is an operations argument rather than a speed one, and so is the other Go-specific decision here.


Compiling the attribute vocabulary into the binary with` go:embed` costs a deploy every time it changes, and buys the guarantee that the attribute list cannot disagree with the code evaluating it. If your vocabulary changes hourly, make the other choice.


### WHAT TRANSFERS


The thing we would take to the next system is the split between mechanism and vocabulary.


Flux is genuinely generic in its mechanism, and you can see it in the schema. The rollouts table has a foreign key to the segment because a segment is a row Flux owns. The gated entity is stored as a bare type string and ID, with no enum or foreign key. Database does not validate what the entity is. Adding collections therefore required no rollout-table migration and no second applicability engine; the existing path began receiving a new entity type,` COLLECTION` .


There is also no cache. Every resolve goes to DB, and the per-segment lookups inside a request are not even batched into one query. That is the most obvious optimization in the codebase and it has sat there for months, because at 0.77 cores and sub-10-millisecond responses nothing has ever asked us to do it. We would rather ship the batching when a number demands it than carry a cache invalidation bug we did not need.


The vocabulary is unapologetically ours: the attributes are Emergent’s business, the benefit types are commerce, the entity IDs are our price ladder.


We initially tried to generalize both layers. The mechanism benefits from indirection because each new entity can reuse it. The vocabulary is business-specific and already cheap to edit.


Keep the evaluator generic. Put business meaning in a vocabulary.


Build the engine to not care. Let the config care.
