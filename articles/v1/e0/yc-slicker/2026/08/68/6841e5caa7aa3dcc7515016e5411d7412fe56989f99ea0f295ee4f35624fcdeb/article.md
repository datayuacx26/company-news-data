---
schema_version: "1.0.0"
document_id: "6841e5caa7aa3dcc7515016e5411d7412fe56989f99ea0f295ee4f35624fcdeb"
company_key: "yc-slicker"
company: "Slicker"
source_id: "yc-slicker-news-import-4c2e7cff2c72"
canonical_url: "https://www.slickerhq.com/resources/blog/payment-failure-email-retry-copy"
published_at: "2026-08-05T09:53:13.549+00:00"
first_seen_at: "2026-08-05T16:20:17.848681+00:00"
fetched_at: "2026-08-05T16:20:19.225413+00:00"
content_hash: "sha256:2a5cbedf679d9b4b1df0c1a21c0f36fd7b3a49f9ffabb1567597dd98621bad5d"
---

# What to Send in Payment Failure Emails at Each Attempt (July 2026)

A recurring payment failure email on attempt 1 should feel like a friendly heads-up. By attempt 5, that same friendly tone reads as if you're not paying attention. Getting the escalation arc right across your full sequence is where most of the real recovery gains are sitting.


**TLDR:**


- Industry data shows roughly 15% of recurring payments fail on the first attempt; most are soft declines that silent retries resolve without customer contact.
- Dunning emails are the fallback, not the default. Send them only when the failure code requires the subscriber to act.
- Each attempt needs a different tone: informational at attempt 1, consequential by attempt 4, and plainly final at attempt 5.
- Match copy to the failure reason. A stolen card requires different instructions than an expired one or an insufficient funds decline.
- Slicker routes failed payments to silent smart retries first, then sequences dunning messages by failure reason, attempt number, and subscriber history.


## Why Recurring Payments Fail


Industry data shows roughly 15% of recurring payments fail on the first attempt. Most of those failures are soft declines: temporary holds, insufficient funds, or issuer-side friction that has nothing to do with a customer wanting to cancel.


That distinction matters because it shapes your entire recovery approach. Soft declines often resolve on their own or with a well-timed retry. Hard declines (stolen cards, closed accounts) require the customer to act, and both contribute to[involuntary churn](https://www.slickerhq.com/resources/blog/what-is-involuntary-churn-and-why-it-matters) if left unaddressed.


Your dunning sequence should reflect that split. Sending a "please update your payment method" email on a soft decline wastes a touch and erodes trust.


## When Not to Send a Dunning Email


Silent recovery is the right first move. When a payment fails due to a soft decline, retrying at the optimal time recovers most subscribers without them ever knowing there was a problem. Dunning emails are the fallback, not the default.


Skip the email when:


- The decline code signals a temporary issue the issuer is likely to approve on retry, such as insufficient funds or a processing error, where customer action would not change the outcome.
- It is the first retry attempt and your retry logic has not yet been exhausted, since contacting the subscriber prematurely creates unnecessary friction with customers who would have recovered silently.
- The subscriber has a strong payment history, where a single failed payment is almost certainly a transient issue and not a signal of account risk.


Sending an email too early in the recovery sequence costs you goodwill with subscribers who never needed to hear from you in the first place. Reserve dunning for situations where the payment error genuinely requires the customer to act, and let[silent payment recovery](https://www.slickerhq.com/resources/blog/how-to-recover-failed-subscription-payments-without-email-dunning) do the work first.


## How the Escalation Arc Works Across a Full Sequence


Each retry attempt should carry a different emotional weight. Attempt 1 is a gentle nudge; by attempt 5, you are communicating genuine urgency about service loss. The arc matters because subscribers read tone as much as words.


A well-structured[dunning management recovery process](https://www.slickerhq.com/resources/blog/dunning-management-recovery-process) typically spans four to five touchpoints, with each step shifting from helpful to urgent:


- Attempt 1 keeps the tone informational, assuming a simple bank error and giving the subscriber no reason to feel alarmed.
- Attempts 2 and 3 introduce mild urgency, acknowledging that the issue persists and prompting the subscriber to verify their payment details.
- Attempt 4 raises the stakes by naming what the subscriber stands to lose, grounding the message in service value over billing mechanics.
- Attempt 5 is the final notice, written with clear finality and a single recovery action the subscriber can take before access ends.


Skipping this progression and sending the same copy at every step is a recoverable mistake in early sequences but becomes a material revenue problem at scale.


## What to Send on Attempt 1


Attempt 1 is the easiest email you'll write in your dunning sequence. The subscriber almost certainly doesn't know their payment failed, so the tone should match that reality: calm, factual, and focused on a clear action.


Keep the message short. Tell them what happened, what they need to do, and what they stand to lose if they don't act. Frame it around the service they value, not around the inconvenience of a declined card.


The best-performing first attempt emails share a few traits:


- The subject line is clear, not clever. "Action needed on your \[Service\] account" outperforms vague urgency bait every time.
- The body leads with the value at stake, not the payment failure itself. "Your access to \[X\] is at risk" lands harder than "Your card was declined."
- The call to action is a single, specific link to update payment details. One action, no distractions.
- The tone stays neutral. No guilt, no pressure. The customer made a mistake; help them fix it.


Timing matters here too. Sending attempt 1 within 24 hours of the failed charge captures subscribers while the transaction is still fresh in memory and before they've had time to disengage.


## What to Send on Attempts 2 Through 4


Each retry attempt after the first carries a different signal, and your messaging should reflect that.


By attempt 2, the initial grace period has passed. The tone stays calm, but the framing moves from "heads up" to "action needed." Remind the subscriber what they stand to lose access to, beyond the fact that a charge failed.


Attempt 3 is where urgency becomes appropriate. Be direct about the timeline: how many days remain before access changes. Customers who haven't acted by now need a concrete deadline, not softer language.


By attempt 4, you are close to the final retry. Knowing when to act requires a solid[soft decline retry playbook](https://www.slickerhq.com/resources/blog/soft-decline-retry-playbook) . If your dunning sequence ends at attempt 5, this email should function as a near-final warning. Keep it short, lead with the consequence, and give one clear call to action.


### What changes across these attempts


- Subject lines should grow more specific as attempts increase, moving from "Payment issue on your account" toward "Your access ends in 48 hours"
- Body copy gets shorter and more direct with each attempt, stripping out context the subscriber has already seen
- The call to action remains singular throughout: one button, one link, one job for the reader to do
- Tone moves from informational to consequential without crossing into punitive language, which tends to increase cancellations and hurt recoveries


## What to Send on Attempt 5


By attempt 5, the tone has to shift. Your subscriber has now ignored multiple emails, and continued gentle nudging reads as indifference on your part. This is the moment to acknowledge the situation plainly and give the customer a clear path forward.


A fifth-attempt email should do three things:


- State the consequence directly: their subscription is at risk of cancellation, and they will lose access to whatever they value most about it.
- Give them one obvious action to take, with a link that goes straight to a payment update page, not a login screen.
- Offer a human out, such as a support contact or a brief pause option, so the email feels like a genuine conversation and not an automated wall.


What to avoid here is just as telling. Skip the apology language ("we're sorry to bother you") because by this point it undermines urgency. Skip multi-step instructions. And never use generic copy like "your payment didn't go through" without context; if you know the failure reason, say it. A card flagged as stolen requires a different action than a card that's simply expired, and your copy should reflect that. Understanding the tradeoffs between[dunning emails vs. automatic AI retries](https://www.slickerhq.com/resources/blog/dunning-emails-vs-ai-retries-payment-recovery-2025) helps clarify when each approach applies.


The goal at attempt 5 is to recover the subscriber, and the relationship behind the payment. Frame the email around what they lose, not what you're owed.


## Writing Copy for Specific Failure Reasons


Each failure reason carries a different recovery path, and your email copy should reflect that. A subscriber whose card was reported stolen cannot fix the problem by updating their expiration date. A subscriber who hit a temporary insufficient funds decline may need nothing more than a gentle heads-up that you will retry soon.


Here are the four most common failure reasons and how to frame each one:


- Insufficient funds: Keep the tone low-pressure and reassuring. Let the subscriber know you will attempt the charge again and give them the option to update their payment method if they prefer not to wait.
- Expired card: Make the action step obvious. Link directly to the billing update page and remind them what they stand to lose if access lapses.[ACI Worldwide data](https://www.aciworldwide.com/blog/recurring-success-exploring-subscription-payment-trends) shows 20% of consumers have missed a subscription payment due to an expired card, so a direct, frictionless update path is critical.
- Stolen or lost card: Treat this as a high-urgency, empathetic message. The subscriber likely already knows something is wrong. Your job is to make the update path frictionless, not to alarm them further.
- Soft decline with no specific reason: Default to a value-retention frame. Remind them what their subscription includes and give them a clear path to resolve the issue without explaining the technical decline code.


Matching copy to failure reason also affects your retry logic, which means following[automatic payment retry best practices](https://www.slickerhq.com/resources/blog/automatic-payment-retry-best-practices-subscription-businesses) is critical. There is no point sending a "we will retry automatically" message for a hard decline where retry attempts will fail regardless. Your dunning sequence and your retry schedule should share the same underlying failure data.


## Timing and Cadence Across the Full Sequence


Most dunning sequences run three to five emails over seven to fourteen days. That window exists because industry data shows the majority of recoverable soft declines resolve within a week, and[smart retries vs. fixed retry schedules](https://www.slickerhq.com/resources/blog/smart-retries-vs-fixed-retry-schedules-subscription-billing) for subscription billing explains why timing within that window matters, making emails beyond day fourteen subject to sharply diminishing returns.


A well-paced sequence might look like this:


Attempt


Timing


Primary goal


1


Same day as failure


Inform, low friction


2


Day 3


Gentle follow-up


3


Day 7


Escalate urgency


4


Day 10


Surface consequences


5


Day 14


Final notice


Space early emails further apart than later ones. A subscriber who missed your first two messages needs time to act, not a flood of reminders within 48 hours.[Baremetrics' dunning research](https://baremetrics.com/blog/dunning-emails) also flags the day-13 email (just before most businesses lock out accounts at day 15) as one of the most reliable steps sequences skip.


## Metrics That Tell You Whether the Sequence Is Working


Recovery rate per attempt is your north star. Track what percentage of failed payments resolve at each step, because the drop-off between attempt 2 and attempt 3 often reveals whether your retry timing or your email copy is the bottleneck.


Three other numbers belong on your dashboard:


- Reactivation rate by email: which specific message in your dunning sequence converts the most subscribers back to good standing, so you can weight your sequence toward what works. Choosing the right[dunning email tools for subscription businesses](https://www.slickerhq.com/resources/blog/best-dunning-email-tools-subscription-businesses) directly affects this metric.
- Days to resolution: how long the average failed payment stays open before recovering or churning, since longer windows signal timing gaps in your retry logic.
- Involuntary churn rate: the share of cancellations caused by payment failure, as distinct from deliberate cancellation. This is the number your CFO cares about most, and it ties every dunning decision directly to MRR (monthly recurring revenue) impact.


If your recovery rate per attempt is declining steeply after step 2, the sequence has a copy or timing problem worth fixing before you add more steps. Closing that gap directly reduces involuntary churn and protects the MRR you have already earned.


## How Slicker Approaches Dunning Across the Full Recovery Sequence


Slicker treats dunning as a sequenced fallback, not a first response. When a payment fails, automated smart retries handle recovery silently whenever the decline type allows it. Customer-facing emails only enter the sequence when the failure code signals that action is required on the subscriber's end.


From there, each message is scoped to where the subscriber is in the sequence. Early attempts carry a lighter tone and a single clear action. Later attempts reflect urgency and lost access without tipping into pressure tactics that damage retention.


[Smart dunning](https://www.slickerhq.com/resources/blog/what-is-smart-dunning-ai-vs-rules-based-recovery-explained) reads the failure reason, attempt number, and subscriber history to adjust copy, timing, and send channel at each step, so your dunning sequence says the right thing at the right moment without manual intervention.


## Final Thoughts on Getting Your Payment Reminder Timing and Copy Right


Dunning works best when it follows a clear arc: silent retries first, then a sequence that grows more direct with each attempt without ever tipping into pressure tactics. Your copy, your timing, and your retry logic should all read from the same failure data so every touchpoint says something your subscriber can actually act on. Build it that way and recovery becomes a repeatable process, not a guessing game.[Connect with the Slicker team](https://www.slickerhq.com/contact) to dig into how that looks in practice for your subscriber base.


## FAQs


### What should your recurring payment failure email say on attempt 1 versus attempt 5?


Attempt 1 should be calm and factual, assuming a simple bank error, with a single link to update payment details and a subject line focused on service access, not the declined card. By attempt 5, the tone moves to plain finality: state directly what the subscriber loses, give one clear action, and offer a human contact option so the message reads as a genuine last step, not another automated reminder.


### How do you write dunning step copy for a stolen card versus an insufficient funds decline?


These two failure reasons require completely different messages. A stolen card means the subscriber needs to contact their bank and add a new payment method, so your copy should be empathetic and frictionless, not a generic retry notice. An insufficient funds decline is often a temporary issue, so a low-pressure message letting the subscriber know you will retry automatically, with an option to update their details if they prefer, typically performs better than an urgent call to action.


### What is the right payment reminder timing across a five-step dunning sequence?


Send attempt 1 within 24 hours of the failed charge, attempt 2 around day 3, attempt 3 on day 7, attempt 4 on day 10, and attempt 5 on day 14. Industry data shows the majority of recoverable soft declines resolve within a week, so spacing the sequence across 14 days captures most of the recovery opportunity without flooding subscribers in the first 48 hours.


### Should you send a dunning email on every failed payment attempt?


No. When a soft decline signals a temporary issuer-side issue, silent retries recover most subscribers without any customer-facing outreach. Sending a dunning email before your retry logic is exhausted wastes a touch and erodes trust with subscribers who would have recovered silently. Reserve customer-facing emails for failures where the subscriber genuinely needs to act, such as an expired or stolen card.


### How do you measure whether your dunning sequence is recovering revenue or just generating opens?


Track recovery rate per attempt, not open rate: the percentage of failed payments that resolve at each step tells you whether your copy or retry timing is the bottleneck. Pair that with involuntary churn rate, the share of cancellations caused by payment failure, and days to resolution. A steep drop-off after step 2 almost always points to a timing gap in the retry schedule or copy that does not match the failure reason.
