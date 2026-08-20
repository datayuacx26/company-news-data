---
schema_version: "1.0.0"
document_id: "c93aebee58fdb73b54d7e1edea3e239e3f645784b3aaf78b9e6d997586eddc74"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/replay-dont-rebuild-adding-week-long-deferrals-to-a-pipeline-that-handles-millions-of-alerts"
published_at: "2026-04-30T15:05:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:ae3411f6d6445a9f1d2ec89a8898c9b793ee0c27562a86f8e999aac081626e65"
---

# Replay, don't rebuild: adding week-long deferrals to a pipeline that handles millions of alerts

When an alert fires at Rootly at 3am on a Saturday, our job is to decide who gets paged. Until recently, that decision happened immediately. The escalation policy evaluated, the page went out, the engineer woke up. Fine for real emergencies. Not so much for low-urgency alerts on a weekend that could perfectly wait until Monday.


"Wait until Monday morning" wasn't a thing our system knew how to do.


Deferred paging, which we recently launched, changes that. The idea is simple: hold the alert until the responder is in working hours, then page as if the alert had just arrived.


The engineering question is where it gets interesting: given that you already have an escalation pipeline handling millions of alerts, where does "wait until business hours" actually belong in the architecture?


## The design we cancelled


Our first plan was a to build a dedicated pipeline. A new SQS FIFO queue to hold deferred alerts, a new Lambda function to process them when the deferral window expired, monitoring and dead-letter handling for the new queue. A clean, isolated system.


We cancelled it. The blocker was time: deferrals can last seven or more days. A Friday evening alert deferred until Monday morning sits for over 60 hours. SQS isn't designed for long-term scheduled storage, it's a queue, optimized for "process this soon."


Every workaround we considered — cycling visibility timeouts, shuffling messages through dead-letter queues — was fighting the primitive. A queue says "process this when you can." A deferral says "process this next Tuesday at 9am in America/Los_Angeles." Those are fundamentally different operations.


## The bet: replay deferred pages through existing entry points


Instead of building a parallel pipeline, we chose to replay the deferred alert through the same entry point that new alerts use. When the deferral window expires, the alert re-enters the paging system as if it just arrived (same alert ID, same timeline, same database record) but carrying context that says "I've been here before."


This is elegant on a whiteboard. In practice, though, the team had to consider important constraints:


- **The paging pipeline was never designed for re-entry.** Every code path downstream assumes events arrive once. An alert that arrives twice could trigger duplicate pages, double-count in metrics, or loop infinitely if the deferral evaluation runs again on wakeup.
- **The serialization boundary between services would grow.** Our platform serializes escalation policy configuration for the notification service. Adding deferral fields means extending a cross-service contract that both sides must agree on and deploy in the right order.
- **State mutations become ordering-sensitive.** In a shared pipeline, clearing a` deferred` flag changes what every downstream check sees. So many edge cases can appear when specific mutations happen in specific sequences.
- **Days can pass between scheduling and execution.** Configuration can change, paths can be deleted, policies can be restructured. The wakeup payload carries references to objects that might not exist by the time it fires.


We chose replay because of what it gives you. The existing paging pipeline already handled path selection with custom rules, time restrictions, default fallbacks, alert grouping, and multi-policy fanout. Replay means a deferred alert gets all of that for free.


Every future improvement to the pipeline (ie. a new notification channel, a new routing rule type, a new escalation behavior) automatically applies to deferred wakeups without anyone writing deferral-specific code. The risks are real, but they're enumerable: you can list the re-entry hazards and build a guard for each one. The upside compounds over time.


The rest of this post is about how we paid down each of those potential risks. But first, here's what the replay actually looks like.


When an alert is deferred, the notification service pushes an entry to our internal scheduler — and the wakeup destination is the same queue that brand-new alerts arrive through:


```text
def     schedule_deferred_alert  (  until_time:  ,   team_id:  ,   payload:  )
raw_alerts_queue_url = ENV[  "queue_url_raw_alerts"  ]


scheduler.schedule_items(
id:     @alert_id  ,
queue_url:   raw_alerts_queue_url,
items:   [payload.merge(  schedule_item:   {   score:   until_time.to_i })],
type:     :deferred_alert
)
end
```


` ‍` The scheduler is a generic internal service: "deliver this JSON to this SQS queue at this epoch time." It doesn't know what an alert is. The` score` is the epoch timestamp when deferral ends. The` queue_url` tells the scheduler where to push the payload when it's due.


Under the hood, the scheduler stores entries in a Redis sorted set — the member is a JSON envelope, the score is the wakeup epoch. The envelope carries just enough metadata for delivery:


```text
{
payload: { ... },        # Alert data — the scheduler never parses this
queue_url:   "..."  ,        # Where to deliver on wakeup
message_group_id:   "..."  , # SQS FIFO ordering key
type: :deferred_alert    # For observability, not routing
}
```


The same scheduler already handled escalation level delays and shift reminders. Adding deferred paging didn't require changes to the scheduler itself, just a new caller that pushed entries with a different` type` and a longer` score` .


Because the scheduler had been built to be destination-agnostic (the caller encodes where the replay should land, and the scheduler just delivers) we got long-duration scheduling for free.


## What happens at paging time


When the scheduler fires, the alert lands back in the same queue as new alerts. The paging pipeline picks it up and does what it always does: evaluates the escalation policy to decide which path to follow. The alert's paging context carries a flag` deferred: true` and optionally a` forced_escalation_path_id` . Those two fields determine what happens next.


If there's no forced path the alert should follow, the policy is evaluated fresh. The pipeline runs through custom rules, time restrictions, and the default fallback as usual. The deferred alert doesn't get special treatment here. The only difference is upstream: the guard that decides whether to defer (which we'll cover in the next section) sees the` deferred` flag and skips deferral evaluation, so the alert doesn't get deferred a second time. Beyond that, it's a normal page.


If the deferral was configured with a specific escalation path to execute on resume, the paging context carries that path's ID. The path selection method checks for it before doing anything else:` ‍` ‍


```text
def     active_path  (  alert:  ,   paging_context:   {})
# ... existing setup ...


forced_path_id = paging_context[  "deferred_forced_escalation_path_id"  ]


if   deferral_enabled && forced_path_id.present?
forced_path = escalation_policy.find_escalation_path(  id:   forced_path_id)
return   forced_path   if   forced_path
end


# ... normal path selection continues ...
end
```


The` return forced_path if forced_path` line is doing two things at once. When the path exists, it short-circuits the entire evaluation. No rules checked, no time restrictions, straight to the configured path.


When the path doesn't exist (it could have been deleted in the days between deferral and resume), the` if forced_path` fails and execution falls through to normal path selection. The alert gets re-evaluated as if it just arrived, which is a reasonable outcome, better than failing a page because a configuration changed while the alert was waiting.


## When types cross service boundaries


Replay through an existing system means extending an existing serialization boundary. Our platform serializes escalation policy configuration (including the new deferral rules) and the notification service deserializes it. The interesting part is how they agree on what a rule means.


Each rule in the serialized escalation path carries a` kind` field, a Ruby class name as a plain string:


```text
# platform side
escalation_path_rules.map   do     |rule|
{
id:   rule.id,
kind:   rule.ruleable_type,    # => "EscalationPathDeferralWindowRule"
rule:   serialize(rule.ruleable)
}
end
```


On the notification service side, that string is turned back into a live object:` ‍`


```text
# Notification service side
def     resolved_rule
kind.safe_constantize.new(rule)
end
```


` safe_constantize` returns` nil` if the class doesn't exist. The caller uses` resolved_rule&.apply?` . If the platform ships a new rule type before the notification service has a matching class, the rule is silently skipped. No error, no log, no indication the deferral window was ignored.


The deferral feature added three new attributes to an existing serializer and introduced a new rule class that both sides must agree on by name. The contract isn't a REST endpoint or a protobuf schema, it's a shared type discriminator that both services implement independently.


This shaped our deployment process. The notification service had to have the new class before the platform started serializing rules that reference it. We deploy these services independently, so getting the order wrong would mean deferral windows silently stop matching, the kind of failure that doesn't show up in health checks.


It's a trade-off we accepted knowingly: the polymorphic dispatch pattern gives us flexibility to add new rule types without changing the serialization infrastructure, but it turns deployment ordering into a load-bearing concern.


## Five guards against infinite replay


An event re-enters a pipeline that wasn't designed for re-entry. If nothing stops it, it loops forever. We built layered guards with different owners.` ‍` **‍**


```text
def     should_attempt_deferral?  (  alert:  ,   paging_context:  )
return     false     unless   deferred_paging_enabled?                     # Ops kill switch
return     false     if   paging_context[  "manual_page"  ]                    # Human override
return     false     if   paging_context[  "skip_deferral_for_retrigger"  ]    # Retrigger bypass


alert.eligible_for_deferral?   ||   alert&.deferred?                 # Entity state
end
```


**The feature flag as kill switch.**` deferred_paging_enabled?` is a per-team flag that does double duty: flag off means all execute-path wakeups degrade to re-evaluate, because the forced-path block is gated on the same flag. We designed it this way deliberately, "off" means "the old behavior," not "broken." During rollout, this let us disable execute-path for specific teams without affecting their re-evaluate wakeups.


**Human override as a first-class signal.**` manual_page` is one boolean, and it's the first check in the guard. If a human explicitly triggered this page, deferral is bypassed entirely. No conditional nesting, no special code path. We put it first because it's the one case where the intent is unambiguous: a person decided this alert needs attention now.


**Entity state prevents re-entry.** When the scheduler wakes the alert, it already has` deferred? == true` in the database:


```text
def     eligible_for_deferral?
!in_deferral_lifecycle?
end


def     in_deferral_lifecycle?
deferred?   ||   deferred_until.present?   ||   forced_escalation_path_id.present?
end
```


` ‍` So` eligible_for_deferral?` returns false, and the alert passes through to paging without being deferred again. It's the same idea as max-redirect counts in HTTP clients — a marker on the entity that prevents re-processing.


**Per-owner guards, not per-entity.** A single alert can be paged by multiple escalation policies — one team in New York defers until 9am EST, another in London defers until 10am GMT. The` || alert&.deferred?` clause looks like it contradicts the entity-state guard — but it lets a currently-deferred alert attempt deferral on a *different* policy, while the matching logic scopes to the current policy's rules. We needed this because without it, the first policy's deferral would block the second policy from evaluating its own window.


**Configuration prevents impossible state.** The platform validates at save time that deferral time blocks can't cover the entire week. An interval-merging algorithm checks all seven days, handles overnight spans. If there's no gap in coverage, the save is rejected. This caught a class of problem we didn't want to debug at runtime — an alert deferred into a window with no exit.


Any single guard technically prevents infinite loops. Having five means the system degrades gracefully when one has an inconsistency. Each guard is owned by a different concern — operational control, caller intent, entity state, configuration validation — so an issue in one layer doesn't compromise the others.


## When QA pushed the edge cases


An alert is actively deferred, ie. sitting in the scheduler, waiting for business hours. A retrigger arrives from the monitoring integration: the same alert fires again. At wakeup time it's 9:05am: business hours have started, the deferral window no longer matches. The system should cancel the deferral and start paging immediately.


Our QA team found out that, under certain conditions, the deferral was cleared and paging proceeded, but the platform was never told the alert resumed. The timeline still showed "deferred" while engineers were being paged.


The culprit was mutation ordering. The retrigger handler clears the` deferred` flag *before* the downstream paging method checks it. By the time the triggered-event check runs,` deferred?` was already false, so the event didn’t fire. And because this particular alert had no routing rules, the code path that would normally notify the platform was skipped entirely.


Four independently-correct decisions created the gap:


1. retriggers reuse the existing alert record
2. unrouted alerts skip a round-trip to the platform as an optimization
3. deferred state is cleared before paging begins so downstream code sees clean state
4. the triggered-event check reads live state from the database.


Each decision made sense in its original context. The issue only became apparent at their intersection.


We fixed it with what we've started calling the snapshot pattern:` ‍` ‍


```text
# Capture BEFORE clearing deferred state
alert_was_actively_deferred =
integration_retrigger &&
alert.deferred? &&
alert.deferred_until.present? &&
alert.deferred_until > Time.current.to_i


# ... deferred state is cleared here ...


# Emit the event using the snapshot, not live state
if   alert_was_actively_deferred && no_routing_rules && paging_will_proceed
emit_triggered_event(  alert_id:   alert.id,   team_id:   team_id)
paging_context[  "triggered_event_already_emitted"  ] =   true
end
```


Capture the state you need before any mutation begins. Carry it forward as a flag. Use it to fill the gap that the mutation created.


This was our clearest lesson from the replay approach: in a shared pipeline, every flag you clear changes what downstream checks see. No unit test of any individual code path caught this, it only appeared when independently-correct decisions combined under specific runtime conditions.


We've since adopted the snapshot pattern as a default whenever a handler needs to both mutate state and emit events based on the pre-mutation state. It's not the most ‘elegant’, but it's explicit, and explicit has been more reliable for us than clever.


## The trade-off vs. an isolated system


Building a parallel pipeline isolates risk: new code breaks new code. But isolation has compounding costs: every future change to the alert lifecycle needs to be implemented in both pipelines. The path selection method already handles forced paths, custom rules, time restrictions, and default fallbacks. A parallel pipeline either reimplements all of that and keeps it in sync forever, or drifts.


The replay approach trades isolation for leverage. But that leverage comes with a specific cost: mutation ordering becomes load-bearing. The retrigger issue exists because state mutations in a shared pipeline are visible to every downstream consumer. In an isolated pipeline, each pipeline owns its own state transitions, and that particular class of problem doesn't exist.


For our system, replay was the right call. The existing pipeline's branching logic was the thing we'd most regret reimplementing. But we'd think twice in a system where the new feature's state transitions are harder to enumerate upfront. The choice depends on what kind of complexity you'd rather manage: duplicated logic that drifts, or shared state that requires ordering discipline.


## What we learned


Thirteen lines of wakeup code are the punchline of this project. The architecture is everything around them: a domain-agnostic scheduler, layered guards that prevent infinite replay, a cross-service type contract that turned deployment ordering into a real concern, and a snapshot pattern that prevented edge cases we couldn't have predicted from reading any single code path.


The feature is "defer this page until business hours." The engineering is "replay safely through a system that wasn't originally designed for replay." The gap between those two sentences is where the interesting work lives.


*In a future post, we'll go deeper on the scheduler itself — a Go service backed by Redis sorted sets that we built to replace AWS EventBridge, and what we've learned running it in production.*


‍
