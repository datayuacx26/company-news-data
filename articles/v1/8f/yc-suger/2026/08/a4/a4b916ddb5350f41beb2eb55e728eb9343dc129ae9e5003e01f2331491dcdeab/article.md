---
schema_version: "1.0.0"
document_id: "a4b916ddb5350f41beb2eb55e728eb9343dc129ae9e5003e01f2331491dcdeab"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/what-to-alert-on-in-marketplace-operations/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-20T00:55:49.760493+00:00"
fetched_at: "2026-08-20T00:55:51.412607+00:00"
content_hash: "sha256:544c7df5d821275d11b3a08c69d5e16eb976bc63367fec1b6a0cf6558f87dfd5"
---

# What to Alert On in Marketplace Operations

*Marketplace operations alerts are the subset of marketplace events worth interrupting a person for — the ones where a delay costs revenue, breaks a customer’s access, or lets a number drift out of reconciliation. The skill is not turning on every notification; it is deciding which events page someone now, which land in a channel for the morning, and which are just log lines.*


---


An alerting policy fails in one of two directions, and both end the same way. Too quiet, and a failed usage record sits unbilled for a month before anyone notices the gap. Too loud, and the fifth “offer created” ping of the afternoon trains the whole team to swipe the notification away — including, eventually, the one that mattered. A channel everybody mutes is worse than no channel, because it looks like coverage.


So the question is not “can Suger notify me about this?” — the answer to that is almost always yes. The question is which marketplace events earn an interruption, how urgent each one is, and where it should land so the right person sees it without the wrong people drowning. This post is that triage, event by event, with a table you can lift into your own runbook. It is the alerting companion to[the cloud marketplace operations runbook](https://www.suger.io/resources/blog/the-cloud-marketplace-operations-runbook/) , which lays out the daily, weekly, and monthly cadence these alerts interrupt.


---


## What makes a marketplace event worth alerting on?


**A marketplace event is worth an alert when it is time-sensitive and human-actionable — a delay makes it worse, and a person can do something about it.** Events that fail both tests are noise: they are either informational (nothing to do) or self-healing (nothing to do *yet* ). The two-question filter cuts a long event list down to a short alert list fast.


Apply it to the usual candidates. A failed metering record is time-sensitive (there is a submission window) and human-actionable (someone must fix the data), so it alerts. A brand-new offer being created is neither urgent nor a problem — it is the system working — so it belongs in a log or a low-priority channel, not a page. The mistake teams make is wiring alerts to *volume* (“tell me about every entitlement”) instead of to *consequence* (“tell me when an entitlement fails to provision”). Volume is what you review on a cadence; consequence is what you get paged for.


One more test sits underneath both: **would a delay of one hour change the outcome?** If yes, it is a page. If the honest answer is “a delay of a day is fine,” it is a digest. Sorting every event against that single question is most of the work of a sane alerting policy.


---


## Which marketplace operations events deserve an alert?


Five categories of event clear the bar, and they are not equally urgent. Below is the annotated triage table — the event, why it matters, how fast it needs a human, and where it should route. Treat severity as the decision about *who is interrupted and how* , not as a label.


Event Why it matters Severity Where it routes


**Failed metering / usage record** Rejected usage is revenue you earned and will not bill, and the submission window closes — a silence, not an error, on your invoice **Page now** A real-time channel (Slack/Teams) that an on-call engineer watches, plus email to billing ops


**Provisioning / entitlement failure** A buyer paid and cannot access the product; every minute is a support ticket forming and a refund risk **Page now** Same real-time engineering channel, with the account owner cc’d so CS is not blindsided


**Offer expiring / pending acceptance** A private offer nearing its end date unaccepted is a deal about to evaporate on a technicality **Same-day** The deal desk or AE channel, and email to the offer owner — soon enough to nudge the buyer, not a 2am page


**Disbursement variance** The money the marketplace paid you does not match what you expected — the earliest sign of a reconciliation break **Same-day / next close** Finance, in a channel and a digest; investigated at close, not overnight


**Agreement change** A renewal, co-term, cancellation, or amendment moves a number your forecast and provisioning both depend on **Awareness** The account team and RevOps, as a notification — logged and visible, rarely paged


Read top to bottom, the severity falls from “wake someone” to “make sure someone knows.” That ordering is the actual product of this exercise. Two of these — failed usage records and provisioning failures — cost money or access *the instant they happen* , so they page. The other three cost money *if ignored over days* , so they inform. Wire all five as pages and you have rebuilt the too-loud policy; wire only the first two and you miss the quiet leaks. The point is to route each to its own consequence.


---


## Why failed usage records are the alert you cannot skip


**A failed metering record is the highest-value alert in marketplace operations because it is invisible by default: a record the marketplace never accepted leaves no trace to be missing from.** A late invoice screams. Unbilled usage says nothing at all — it simply is not there, and no dashboard shows an absence you never recorded.


That silence is what makes it a page rather than a digest. Metering APIs enforce a submission window, and some failures are permanent while others are transient service errors that resolve on retry — so the alert has to fire fast enough to fix the fixable ones before the window closes. An alert on “records failed” that arrives in a weekly summary arrives after the deadline. This is the one event where the difference between a real-time channel and a daily digest is literally the difference between billed and unbilled revenue. For the mechanics of which failures to retry versus fix, see[when usage records fail](https://www.suger.io/resources/blog/when-usage-records-fail/) — the alert is only useful if the person who receives it knows which kind they are looking at.


---


## Where each alert should route


**An alert routes correctly when it reaches the one person who can act on it, in a place they actually watch, at a volume that keeps the channel trusted.** Routing is not a delivery detail bolted on after severity — it *is* the severity, made concrete. “Page now” means a real-time channel plus a named owner; “awareness” means a notification that logs without interrupting.


Suger delivers marketplace notifications over three channels, and the choice among them is about the response the event needs, not the technology:


- **Email** — the durable, addressable record. Recipients are configured as static TO, CC, and BCC lists per notification scope, which makes it the right destination for anything that needs an accountable owner and a paper trail: disbursement records, commission events, an offer owner who must chase an acceptance. Email is where “who is responsible for this” is written down.
- **Slack** — the real-time team channel. Suger maps notification scopes to specific channels, so` CREATE` actions can go to a sales-wins channel while` CANCEL` and` SUSPEND` go to customer success. This is where the “page now” events belong: a failed record or a provisioning failure in a channel an on-call engineer is already watching.
- **Microsoft Teams** — the same real-time model for Teams-first organizations, delivering the same offer and entitlement events to a configured channel, with both organization-level and per-user configuration.


The scope-to-channel mapping is the mechanism that keeps the too-loud policy at bay: because Suger lets you send different event scopes to different channels, you can put the two page-now events where they get an immediate response and route the routine confirmations somewhere quieter, rather than firehosing one channel with everything. Routing by scope is how a busy channel stays a trusted one.


A note on ownership, because it is a tempting shortcut: sending an alert to “whoever owns the account in the CRM” is not something to assume — Suger’s documented routing is to configured email recipients and named Slack or Teams channels per scope, and you should design your policy around that static mapping rather than a dynamic role lookup. In practice this is not a real constraint: name the channels after the function that responds (billing-ops, provisioning, deal-desk, finance) and the right people are already in them, which is more durable than routing that depends on a CRM field being current on the day the alert fires.


---


## How to set severity without crying wolf


**Set severity by consequence and reversibility: how much a delay costs, and how hard the situation is to undo once it slips.** A cheap, easily reversed event is low severity even if it happens often; an expensive, hard-to-reverse one is high severity even if it is rare. Frequency is a distraction — the loudest events are usually the least important, precisely because routine things happen the most.


Give every alert three properties before you turn it on: a **severity** (page, same-day, or awareness), an **owner** (the function that responds, not a person who might change teams), and a **channel** that matches the severity. An alert missing any of the three is the seed of alert fatigue — an unowned page gets diffused across everyone and actioned by no one, and a page-severity event in an awareness channel gets scrolled past. Then prune on a cadence: any alert that has fired fifty times and been acted on zero times is miscalibrated, and the fix is to demote it, not to mute the channel. The goal is a small set of alerts the team still trusts, because the day they stop trusting the channel is the day a failed usage record slips through it unread.


---


## Frequently asked questions


**What marketplace events should trigger an alert?** The time-sensitive, human-actionable ones: failed metering or usage records, provisioning or entitlement failures, offers expiring unaccepted, disbursement variance, and agreement changes. Routine confirmations like a new offer being created are informational — log them or send a low-priority notification, don’t page on them.


**Which marketplace alert is the most important?** Failed usage records. Rejected metering is revenue you earned but will not bill, and it is invisible by default because a record the marketplace never accepted leaves no gap to notice. It is also time-boxed by a submission window, so it needs a real-time alert, not a weekly summary.


**Where should marketplace alerts be delivered?** Match the channel to the urgency. Page-now events (failed records, provisioning failures) belong in a real-time Slack or Teams channel an on-call person watches. Ownership-and-record events (disbursement, offer acceptance, commissions) belong in email, where recipients are a configured, accountable list.


**Can Suger send marketplace notifications to Slack and Microsoft Teams?** Yes. Suger supports email notifications plus Slack and Microsoft Teams integrations. Slack and Teams both deliver offer and entitlement events (create, cancel, suspend, expire, and more) to channels you configure, and you can map different event scopes to different channels.


**How do I route an alert to the right team in Suger?** Configure notification scopes to specific destinations: static TO/CC/BCC lists for email, and named channels for Slack and Teams — for example, routing new-offer events to a sales channel and cancellations to customer success. Name channels after the function that responds so the right people are already present.


**How do I avoid alert fatigue in marketplace operations?** Alert on consequence, not volume, and give every alert a severity, an owner, and a matching channel. Send page-now events to a watched real-time channel and routine confirmations somewhere quieter using per-scope routing. Prune any alert that fires often and is acted on never — demote it rather than muting the channel.


---


## Takeaways


- Alert on consequence, not volume. An event earns a page only if it is time-sensitive *and* human-actionable — a one-hour delay would change the outcome. Everything else is a digest or a log line.
- Five events clear the bar: failed usage records, provisioning failures, expiring offers, disbursement variance, and agreement changes — and they are not equally urgent. The first two page now; the other three inform.
- Failed usage records are the alert you cannot skip, because unbilled usage is invisible by default and time-boxed by a submission window. It has to be real-time, never a weekly summary.
- Route by matching the channel to the response: real-time Slack or Teams for page-now events, email with a configured recipient list for anything needing an accountable owner and a record.
- Give every alert a severity, an owner, and a channel — and prune the ones that fire constantly and are acted on never, so the channel stays trusted.


---


Good alerting starts with clean events to alert on — validated usage records, tracked entitlements, and reconciled disbursements in one place, so a failure surfaces as a notification instead of a silence on next month’s invoice. See how[billing and metering automation in Suger](https://www.suger.io/platform/billing-metering/) turns marketplace operations events into alerts your team can actually route, across[the full Suger platform](https://www.suger.io/platform/) .
