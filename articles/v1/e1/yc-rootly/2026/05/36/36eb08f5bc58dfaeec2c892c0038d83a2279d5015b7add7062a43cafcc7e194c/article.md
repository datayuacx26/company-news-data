---
schema_version: "1.0.0"
document_id: "36eb08f5bc58dfaeec2c892c0038d83a2279d5015b7add7062a43cafcc7e194c"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/how-to-leave-pagerduty-without-missing-a-page"
published_at: "2026-05-14T13:00:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:76681b15f7ff4bf8511f3a61a3d047fb2be75c23cbed94e4265cb72ba7736cf3"
---

# How to leave PagerDuty without missing a page.

A page that reaches the right engineer is not one event. It is a chain of six handoffs, and every one of them has to hold: a monitoring tool emits an alert, a routing rule catches it, an escalation policy picks a target, a schedule resolves who that target is right now, a notification rule decides how to reach them, and a device makes a sound that wakes them up.


When teams tell us a PagerDuty migration feels risky, this chain is what they mean, whether they say it this way or not. The fear is never "the import will fail." Imports fail loudly and get fixed. The fear is that one link in the chain will differ silently between the old system and the new one, and nobody will find out until a real alert fires at 3 AM and goes nowhere.


That fear deserves a better answer than "trust us." So here is how we de-risk each link, followed by the risks that live outside the system entirely. Those are the ones that actually stall migrations.


## Verify the chain, link by link.


The way you make a migration safe is not by being careful in general. It is by verifying each handoff specifically, with the old system still running, before anything depends on the new one.


**Alert sources.** Your monitoring tools keep pointing at PagerDuty while they also point at Rootly. Both systems receive the same events for the full transition window. This is not a courtesy. It is the mechanism that makes every other verification possible, because you can compare what each system did with identical input.


**Routing.** Years of routing logic accumulate quirks. Rules written by engineers who left. Conditions that reference services that were renamed twice. Before migration, Rootly audits your PagerDuty configuration read-only and hands you a findings report: every routing rule, every event orchestration, and specifically the ones that will not translate one-to-one. You see the exceptions list before you commit to anything, not during cutover week.


**Escalation policies.** These import directly, including delays, repeats, and round-robin behavior. But importing a policy and trusting a policy are different things, so the validation step fires test pages through real escalation paths and confirms the right people get reached in the right order. A policy existing in the system is not evidence. A page arriving on a phone is evidence.


**Schedules.** Rotations, overrides, and future shift generation transfer completely. Built-in gap detection then flags any hour where a schedule resolves to nobody, which catches both import artifacts and coverage holes that existed in PagerDuty all along. More than one team has discovered during migration that their "fully covered" schedule had a recurring Sunday gap nobody had hit yet.


**Notification rules.** This is the link most migrations skip, and it is where pages die quietly. Each engineer's contact methods and notification timing carry over from PagerDuty, and Rootly seeds default rules for anyone whose configuration came over incomplete, so no responder ends up reachable in theory and unreachable in practice.


**The device, and the human holding it.** The last link is the one no importer can fix. A perfect configuration still fails if the on-call engineer never installed the app, never verified their phone number, or has the new tool sitting muted in a notification tray. Rootly treats responder readiness as its own workstream: communication waves before cutover, in-product nudges, a 30-minute training, and an admin dashboard that shows install and verification status per person. You do not switch until you can see that every responder in the first wave is actually reachable. Named individuals, not a percentage.


Run the cutover team by team, starting with a willing team of moderate complexity rather than your most critical service. By the time your payment infrastructure moves, the chain has been verified end to end a dozen times on real traffic.


## The risks that live outside the system.


Talk to enough teams that evaluated a switch and never moved, and a pattern emerges. The technical chain was never the blocker. These were.


**Nobody owns it.** A migration without an owner does not fail. It just never starts. Budget exists, interest exists, the evaluation went well, and six months later nothing has happened because driving it was nobody's job. The fix is to make the project small enough that one person can own it. Rootly assigns a dedicated implementation engineer and a CSM who carry the plan, the training delivery, and the per-team coordination, so your owner's actual job is making decisions, not running spreadsheets. One champion with authority and a forcing function beats a committee every time.


**We just renewed. The contract math feels upside down.** Paying two vendors during a transition is a real cost, not a negotiating posture, and a recent renewal signed under deadline pressure is the most common reason a clear decision gets parked for a year. So we removed it: Rootly buys out up to 12 months of your remaining PagerDuty contract. If the business case is clear, the renewal date you signed under pressure does not have to be the date you get to act on it.


Even with the overlap covered, run the full comparison, not just the line items: the add-ons, the per-feature paywalls, and the engineering hours you currently spend maintaining workarounds, because the invoice was never the whole number. And start the audit now regardless of where you are in your contract. It costs you nothing, and arriving at any negotiation with a findings report and a phased plan beats arriving with a hunch.


**Security review takes longer than anyone budgeted.** In any regulated environment, a new vendor with access to operational data goes through review, and those reviews stretch when they start late. Bring security in during evaluation, not after the decision. Rootly is SOC 2 Type II certified with documentation ready for enterprise review, but the bigger lever is sequencing. A review that runs in parallel with your trial costs you nothing. A review that starts after the business decision can stall a signed project for a quarter.


**PagerDuty works, so why move?** It does work, for paging. The honest version of this question is whether paging is still the job. Incidents do not end when the page is acknowledged. Someone coordinates the response, updates stakeholders, writes the retrospective, and tracks the action items, and on a paging-only tool all of that happens somewhere else, stitched together by hand. The configuration debt also compounds in only one direction. Every year on the old platform adds workarounds that make the eventual move heavier. Teams that switched after a punitive renewal or a bad incident all say a version of the same thing: the migration was easier than they feared, and they wish they had not waited for the trigger.


## What "done" means.


A migration is not done when the import finishes. The import is an afternoon. It is done when an engineer gets paged at 3 AM, acknowledges from the Rootly app, asks the @Rootly agent in Slack what changed in the last hour, and never thinks about the old tool once. When your teams stop reflexively checking PagerDuty, turn it off. Not before, and no deadline should force it earlier.


If you are evaluating a move, start with the part that costs nothing: a read-only audit of your PagerDuty environment. You get the findings report and the phased plan whether or not you go any further, and you will know exactly what is in your tenant either way. And if a contract is the only thing in the way, it is not. We cover up to 12 months of the overlap, so the decision can be made on the merits.


[Book a migration assessment](https://rootly.com/demo)
