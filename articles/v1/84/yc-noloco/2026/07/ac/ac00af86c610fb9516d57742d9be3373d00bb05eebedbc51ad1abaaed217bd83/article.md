---
schema_version: "1.0.0"
document_id: "ac00af86c610fb9516d57742d9be3373d00bb05eebedbc51ad1abaaed217bd83"
company_key: "yc-noloco"
company: "Noloco"
source_id: "yc-noloco-news-import-a32087056898"
canonical_url: "https://noloco.io/blog/airtable-automation-limit"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-25T17:05:03.549572+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:8a6c0eaf731571cbd381100824267ec4985de1ff19983cd4ee89c3f64bd4de76"
---

# The Airtable automation limit nobody warns you about

Your invoice reminder automation stopped sending three weeks ago. Nobody noticed until a client called asking why they never got a late notice. You open Airtable and everything looks fine. The automation is still switched on. There's no obvious error. It just isn't running.


For many growing teams, this is how they first discover Airtable's monthly automation run limit.


The limit isn't hidden; it's documented. But it's easy to miss until your business grows and your automations start running more often than you expected.


Every Airtable workspace has a monthly automation run limit. Once you reach it, automations pause until the next billing cycle (or until you upgrade your plan). Since warning emails are only sent to workspace owners, the rest of the team may not realize anything has changed until a workflow is affected.


## TL;DR


- Airtable automations run against a hard monthly cap: 100 runs on Free, 25,000 on Team, 100,000 on Business, and 500,000 on Enterprise Scale ([Airtable pricing, 2026](https://airtable.com/pricing) ).
- A "run" is counted every time an automation's trigger fires, whether or not any action actually happens, and whether the automation succeeds or fails ([Airtable Support, Managing Airtable automations](https://support.airtable.com/docs/managing-airtable-automations) ).
- Airtable emails the workspace owner at 80%, 90%, and 100% of the monthly limit. If nobody happens to read that email, the automations simply stop running until the 1st of the next month.
- Firms running client onboarding, invoicing, and reminder automations on the Team plan often hit the 25,000-run ceiling well before they get anywhere near the 50,000-record ceiling on the same plan.
- Upgrading plans isn't always the right fix. Hitting Airtable's automation limit is often the first visible sign that your operational system has outgrown what spreadsheet-based workflows were designed to handle.


## What is Airtable's automation run limit, exactly?


Every Airtable plan comes with a monthly cap on automation runs. Free workspaces get 100 runs a month. Team plans get 25,000. Business plans get 100,000. Enterprise Scale gets 500,000 ([Airtable, 2026](https://airtable.com/pricing) ). These numbers sit quietly on the pricing page, next to record limits and attachment storage, easy to skim past when you're comparing plans on price alone.


The table below lays out where each plan actually sits.


If you're trying to... Airtable automations[Noloco workflows](https://noloco.io/product/workflows)


Automate reminders ✓ ✓


Run client onboarding across multiple teams Gets difficult as workflows grow Built for multi-step workflows


Let clients approve work Limited Built in


See why something didn't happen Basic logs Workflow history tied to each record


Grow without worrying about automation caps Monthly limits apply No workflow run limits


Read[our full breakdown of Airtable's 2026 pricing](https://noloco.io/blog/airtable-pricing) for how these limits sit alongside record caps, the Portals add-on, and AI credits.


## How does an automation actually get counted as a "run"?


This is why the run limit often catches growing teams by surprise. You don't need dozens of automations—you just need a handful watching busy tables. A run isn't counted per automation, or per action, or per successful outcome. It's counted every time the trigger condition fires, regardless of what happens next ([Airtable Support](https://support.airtable.com/docs/managing-airtable-automations) ).


So if you have one automation that checks a "when record matches conditions" trigger, and 500 records happen to match that condition three times a day as statuses change, that's 1,500 runs a month from a single automation. If the automation fails halfway through, or has nothing to actually do, it still counts. Loops inside an automation don't multiply the count, but the trigger firing does, every time.


This is why the run limit tends to surprise firms that feel like they're "not doing that much automation." You don't need dozens of automations to hit 25,000 runs. You need a handful of automations watching busy tables.


## What happens when you hit the cap?


Airtable sends the workspace owner an email when usage crosses 80%, 90%, and 100% of the monthly limit. Once you're at 100%, every automation on every base in that workspace stops running until the usage resets on the 1st of the following month, or until you upgrade the plan.


There's no in-app warning banner sitting next to the automation you're building. If the person who set up the client onboarding automation isn't the person who reads the billing emails, it can take a while before anyone realizes workflows have stopped running. The onboarding emails, task creation or internal notifications you expected to happen simply don't.


## Why do professional services firms hit this limit first?


Client-facing operations are automation-heavy by nature. A typical setup for a consultancy, agency, or accounting firm running client work through Airtable often includes an onboarding sequence, a status change notification, an invoice reminder, and a project handoff automation, each one watching a table that changes constantly as client work moves through stages.


As your client base grows, the number of automation triggers grows with it. This is a different ceiling than the 50,000-record cap on the Team plan. A firm with a lean, well-organized 20,000-record base can still burn through 25,000 automation runs in a busy month, because the record count and the automation-trigger count are two completely separate meters.


According to[Asana's Anatomy of Work Index (2026)](https://asana.com/resources/work-isnt-working) , which surveyed more than 10,000 knowledge workers, roughly 60% of the average workday goes to "work about work": chasing status, switching between apps, and managing shifting priorities rather than doing the actual client work. Automations are there to reduce that operational overhead. When they stop running, manual follow-ups and status checks quickly find their way back into the team's day.


## How do you know if you're close to the ceiling?


A few signs worth checking for, especially if nobody on the team is actively watching the automation usage panel:


- You've added client onboarding, reminder, or notification automations in the last few months without removing any older ones.
- Your automations watch tables with frequent status changes (deal stages, project phases, invoice statuses) rather than one-time triggers.
- You're on the Team plan with more than a handful of active clients or projects running through Airtable at once.
- Nobody on the team can say, off the top of their head, roughly how many automation runs you used last month.


Airtable shows current usage under the workspace's billing settings. It's worth a five-minute check even if nothing feels broken yet.


## What should you do about it?


Upgrading your Airtable plan is often the simplest next step—and for many teams, it's the right one. But it's also worth asking *why* you're approaching the limit in the first place. First, whether you're running duplicate or overlapping automations doing effectively the same job across multiple bases, which is common when a team builds automations independently over time. Second, whether the automation logic you're relying on has outgrown what built-in spreadsheet automations were designed to handle, like multi-step approval chains or conditional logic across several tables.


Both of those point toward the same underlying issue Airtable's automation limit is actually surfacing: many growing businesses reach the point where they benefit from a dedicated[workflow layer.](https://noloco.io/product/workflows) If you're relying on[Zapier](https://noloco.io/blog/when-zapier-stops-being-enough-for-your-service-firm) or Make to stitch together growing operational workflows, you'll eventually hit their own scaling considerations for different reasons.


The table below lines up the two approaches directly.


Approach What changes per client What you maintain


**One duplicated interface per client** Everything, built separately each time Every copy, by hand, for every future change


**One interface, native record filter** Which records the client sees One interface, but field-level privacy still needs manual handling


**Noloco, one page, dynamic permissions** Records and fields, per role, automatically One page, with[granular permissions](https://noloco.io/product/permissions) set once


Keeping Airtable as the data layer and adding a dedicated workflow engine on top isn't a rip-and-replace decision. It's closer to what we cover in[the 8 agency tasks spreadsheets break as you scale](https://noloco.io/blog/8-agency-tasks-spreadsheets-break-as-you-scale) : the underlying data can stay put while the layer that runs it grows up.


## Final thoughts


If you're adding more automations, more workflows and more tools just to keep client work moving, it may be worth stepping back and asking a different question. Rather than *"How do we get more automation runs?" ask* *"How do we build a system that scales with the way we actually work?"*


For many growing businesses, that doesn't mean replacing Airtable. It means keeping Airtable as the data layer and adding an operating system on top that's built for workflows, approvals, permissions and client collaboration. See[how Noloco works with Airtable](https://noloco.io/data/airtable) to turn your existing base into a system your team and your clients can actually run on, without rebuilding everything from scratch.


## Frequently asked questions


### What counts as an automation run in Airtable?


**‍** Every time an automation's trigger condition is met, Airtable counts one automation run. It doesn't matter whether the automation completes successfully, fails halfway through, or exits without taking any action—the trigger firing is what counts.


### Why did my Airtable automation suddenly stop working?


One common reason is that your workspace has reached its monthly automation run limit. When that happens, every automation in the workspace stops running until the limit resets at the start of the next month or you upgrade your Airtable plan. It's also worth checking your automation history for errors, disabled automations, or changes to your trigger conditions.


### Does Airtable warn you before you hit the monthly limit?


**‍** Yes. Airtable emails the workspace owner when usage reaches 80%, 90% and 100% of the monthly automation limit. However, those warnings don't appear inside the automation builder itself. If the person receiving billing emails isn't the person managing operations, it's easy for the limit to be missed until workflows stop running.


### Can you buy extra automation runs on Airtable?


**‍** Not as a standalone add-on the way you can buy extra AI credits. The main options are upgrading to the next plan tier or contacting Airtable support, which has occasionally extended limits for specific cases.


### Do automation runs reset on a rolling basis or a fixed date?


**‍** They reset on a fixed date, the 1st of each calendar month, not on a rolling 30-day window.


### How can I reduce Airtable automation runs?


The fastest way is to review your automations and look for duplicate workflows, unnecessary triggers and automations watching high-activity tables. For example, multiple automations that react to the same status changes can often be consolidated into a single workflow. If you're regularly approaching the limit, it's often a sign your operational workflows have become more sophisticated (not necessarily that you're doing anything wrong). See[how Redrock Entertainment managed it.](https://noloco.io/customer-stories/redrock-entertainment)


### Is Zapier or Make a way around Airtable's automation limit?


**‍** Using an external automation tool moves the trigger logic outside Airtable's own automation panel, which can help, but it introduces its own separate usage limits and a second tool to maintain. It shifts the ceiling rather than removing it.


### What's the alternative to building more Airtable automations?


If you're only automating a handful of repetitive tasks, Airtable's built-in automations are often enough. But if you're coordinating client work across multiple teams, approvals, portals and external collaborators, it can make more sense to separate your **data layer** from your **operational layer** .


Many growing service businesses[use Airtable to store their data, while using Noloco to run workflows,](https://noloco.io/data/airtable) manage permissions and give both their team and clients a single place to work. That way, you can keep the flexibility of Airtable without relying on spreadsheet automations to run your entire business.


## Related resources


- [Airtable Pricing 2026: Plans, Hidden Costs & the Best Frontend for Portals](https://noloco.io/blog/airtable-pricing) : the full plan breakdown, including where automation and record limits sit alongside cost.
- [When Zapier stops being enough for your service firm](https://noloco.io/blog/when-zapier-stops-being-enough-for-your-service-firm) : what happens one layer up, once external automation tools hit their own ceiling.
- [How Redrock Entertainment built a custom Operating System and cut costs by 60% with Noloco](https://noloco.io/customer-stories/redrock-entertainment)
- [Noloco Workflows](https://noloco.io/product/workflows) : the workflow engine referenced in the comparison table above.
- ‍[Noloco for Airtable](https://noloco.io/data/airtable) : how the two-way sync with your existing Airtable base actually works.


**Not sure if the automation limit is the only ceiling you're bumping into?** Continue to[6 signs your firm has outgrown Airtable as its source of truth](https://noloco.io/blog/6-signs-your-firm-has-outgrown-airtable) to check the other five.
