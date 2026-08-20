---
schema_version: "1.0.0"
document_id: "2edaf720327452530d2a2fa7a4b75b43683af6a8cf9b43f4462d8edefd241017"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/scheduled-reports-in-bi-tools-how-automated-report-delivery-works/"
published_at: "2026-06-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:39b8f67b55c93e1eef94edaf1f5a2ed740237ff290dbe0b385666a0d16629a5c"
---

# Scheduled reports in BI tools: how automated report delivery works

A scheduled report is a saved report or dashboard that a BI tool runs on a recurring schedule and delivers to people automatically, usually by email or Slack, without anyone opening the tool. The point is reach: most people who need a number will never log into a dashboard, but they will read an email at 8am or glance at a Slack message. Scheduling turns a dashboard that a few analysts visit into a report that a whole team receives.


This guide is for analysts, operators, and founders who want recurring reports to land reliably in the right inboxes with the right data. It explains what a scheduled report is (and how it differs from a live dashboard and an alert), how automated delivery actually works step by step, the parts that quietly break, and how the major BI tools handle scheduling. It is a capability deep dive, not a tool ranking. If you want a ranked list, see our[comparison of automated reporting tools](https://www.basedash.com/blog/best-automated-reporting-tools-compared-2026) .


## TL;DR


- A scheduled report runs on a cron-style schedule, renders to a format (PDF, image, CSV, or link), and pushes to a channel (email, Slack, webhook, or file storage).
- Scheduled reports, alerts, and live dashboards solve different problems. Use a schedule for routine reach, an alert for exceptions, and a live dashboard for investigation.
- The most common failures are timing races (the report fires before the data refreshes), time zone mistakes, and silent empty reports after a query breaks.
- Report bursting sends one report definition to many recipients with each person seeing only their own slice. It depends on row-level filtering, not separate reports.
- Treat every scheduled report like a small data pipeline: it needs an owner, a freshness guarantee, a failure path, and an expiry date.


## What a scheduled report actually is, and what it is not


The three recurring-data patterns get blurred together, but they answer different questions.


Pattern Question it answers Trigger Best for


Scheduled report “What do the numbers look like right now, on a routine?” A clock (cron) Recurring reach: morning metrics, weekly summaries, monthly board packs


Alert “Did something cross a line I care about?” A data condition Exceptions: revenue drop, error spike, churn threshold


Live dashboard “Let me dig into this myself” A person opening it Investigation and ad hoc follow-up questions


A scheduled report is time-triggered: it fires whether or not anything changed. An alert is condition-triggered: it fires only when a metric crosses a threshold or an anomaly model flags it. A live dashboard is person-triggered: nothing happens until someone opens it.


The practical rule: schedule the things people should see on a cadence regardless of whether they are interesting, alert on the things people should never miss, and keep a live dashboard for the follow-up questions both of those will provoke. Sending a daily email for every metric is how you train people to ignore email. For the exception side of this, see our overview of[anomaly detection and smart alerting](https://www.basedash.com/blog/best-bi-tools-for-ai-anomaly-detection-and-smart-alerting-2026) .


## How automated report delivery works, step by step


Under the hood, every scheduled report runs the same four-stage pipeline. The names differ across tools, but the stages do not.


### 1. The schedule (trigger)


A scheduler decides when the report runs. Most tools expose this as a friendly picker (“every weekday at 8:00am”) backed by cron under the hood. Two details matter more than people expect:


- **Time zone.** The schedule runs in a specific zone. If the tool defaults to UTC and your team is in Pacific time, an “8am” report lands at midnight. Set the zone explicitly per report or per workspace.
- **Calendar awareness.** A clock does not know about weekends, holidays, or month boundaries. “Last day of month” and “first business day” usually need either a tool feature or a small workaround, so confirm the scheduler supports the cadence you actually want.


### 2. The data step (query or refresh)


When the schedule fires, the report needs current data. This is where scheduled reports collide with[refresh strategy](https://www.basedash.com/blog/dashboard-refresh-strategies-live-queries-scheduled-refreshes-and-cached-extracts) . There are two models:


- **Live query at send time.** The report runs its SQL against the warehouse at the moment it fires, so the data is as fresh as the source.
- **Read the cached result.** The report reads whatever the last scheduled refresh or extract produced. Fast and cheap, but only as fresh as that refresh.


If your dashboard reads a cached extract that refreshes at 7:00am and your report sends at 7:00am, you have a race. The report can grab yesterday’s numbers because the refresh has not finished. Stagger the two: refresh first, send with a buffer.


### 3. The render step (format)


The query results get turned into something deliverable. Common formats and when each fits:


- **PDF or PNG image:** a snapshot of the dashboard as it looks. Best for executives and board packs where layout matters and nobody will re-analyze.
- **CSV or Excel:** the underlying rows. Best when the recipient will pivot, filter, or paste into their own model.
- **Inline HTML / a link back to the live dashboard:** lightweight, always current when clicked. Best for internal teams who may want to drill in.


A frequent mistake is sending a static PDF to someone who needed the rows, or a 50,000-row CSV to someone who just wanted the headline number.


### 4. The delivery step (channel)


Finally the rendered report is pushed somewhere. The usual channels:


- **Email:** the universal default. Watch attachment size limits and whether the body shows an inline preview.
- **Slack or Microsoft Teams:** higher open rates for internal teams. Usually an inline image plus a link. Slack delivery is how many teams actually consume metrics; see[Basedash for Slack](https://www.basedash.com/blog/introducing-basedash-for-slack) .
- **Webhook:** posts the result (often JSON or a file URL) to an endpoint so you can route it into another system.
- **SFTP or cloud storage (S3, GCS):** drops a file on a schedule. Common for handing data to partners or feeding downstream jobs.


Push beats pull for routine reporting. The whole reason to schedule is that you stop relying on people remembering to look.


## Report bursting: one report, many tailored copies


Report bursting (sometimes called “data-driven delivery”) is the capability that separates a basic scheduler from a real reporting system. Instead of building one report per recipient, you build a single report and the tool sends each recipient a version filtered to only their data.


A regional sales lead gets only their region. A customer gets only their own account. An account manager gets only their book of business. One definition, many tailored sends.


Bursting only works if the filtering is enforced at the data layer, not bolted on at send time. It is the same machinery as[row-level security in a permission model](https://www.basedash.com/blog/how-to-design-a-bi-permission-model-for-a-saas-team) : the report inherits each recipient’s row-level filter, runs once per recipient (or once with a per-recipient parameter), and delivers the filtered slice. If a tool offers scheduling but no row-level filtering, “personalized” reports really mean building and maintaining one report per person, which does not scale past a handful of recipients.


## The parts that quietly break


Scheduled reports fail in ways that are hard to notice, because by definition nobody is watching when they run. These are the recurring problems.


- **Timing races.** The report fires before the refresh finishes and ships stale or empty data. Fix by sequencing: refresh, then send with a buffer, or trigger the send on refresh completion if the tool supports it.
- **Time zone drift.** Schedules defined in UTC land at odd local hours, and twice a year daylight saving shifts everything by an hour. Set zones explicitly.
- **Silent empties.** A column gets renamed upstream, the query breaks, and the report still sends, now with zeros or blanks. Recipients trust it because it looks normal. Add a non-empty check or a “no data” guard so a broken report fails loudly instead of lying quietly.
- **Alert and report fatigue.** Too many low-value scheduled emails train people to filter the whole sender to a folder they never open. Fewer, denser reports beat many thin ones.
- **Attachment bloat.** A growing dataset turns a tidy CSV into a file that bounces off mailbox size limits. Cap rows, summarize, or switch to a link.
- **Security of the payload.** A PDF with revenue sitting in an inbox or an S3 bucket is data outside your access controls. Prefer links that respect login over attachments for sensitive numbers, and confirm recipients are still authorized.
- **Orphaned schedules.** The analyst who built the Monday report leaves, the report keeps sending for a year, and nobody knows who owns it or whether it still matters. Give every schedule an owner and a review date.


## How different BI tools approach scheduled reports


Capabilities vary widely. This compares the concrete features that determine whether a tool can do what you need.


Tool Native channels Report bursting / per-recipient filtering Formats Notes


Basedash Email, Slack Via row-level access controls Link, image, table AI-assisted; schedules tied to workspace permissions


Power BI Email (subscriptions), Teams Yes, via data-driven subscriptions (Premium) PDF, image, link Paginated reports add SFTP and file delivery


Tableau Email (subscriptions) Limited; Data-Driven Alerts are separate Image, PDF, link Subscriptions push the view; bursting needs workarounds


Looker Email, Slack, webhook, S3/GCS, SFTP Yes, via “send as” with user attributes CSV, Excel, PDF, PNG, JSON Strong delivery options including cloud storage


Metabase Email, Slack Filtered by saved parameters, not true bursting CSV, Excel, PDF (dashboard) “Dashboard subscriptions” cover most needs


Sigma Email, Slack, webhook Yes, via row-level security on the schedule PDF, CSV, Excel Filters can be passed per recipient


Channels and formats change; confirm against each vendor’s current documentation before you rely on a specific capability, especially when bursting or cloud-storage delivery is a hard requirement.


## A checklist for setting up a scheduled report


Before you turn a schedule on, run through these. Most broken reports skipped one of them.


- **Owner.** A named person responsible when it breaks, not “the data team.”
- **Audience and channel.** Who receives it, and is email or Slack the right place for them?
- **Format fit.** A snapshot for readers, rows for analysts, a link for people who will drill in.
- **Freshness guarantee.** When does the data refresh, and does the send happen after it, with a buffer?
- **Time zone.** Set explicitly, and sane through daylight saving changes.
- **Failure path.** What happens on a query error or empty result? It should fail loudly, not send blanks.
- **Per-recipient scope.** If different people should see different slices, is row-level filtering enforced, or are you about to maintain 30 near-identical reports?
- **Security.** For sensitive numbers, prefer a link that respects login over an attachment that lives forever in an inbox.
- **Review date.** A date to confirm anyone still reads it. Unread scheduled reports are pure waste.


## When to use a scheduled report, an alert, or neither


Use a scheduled report when the cadence is the point: a recurring summary that people should see whether or not it changed, like a Monday pipeline review, a daily ops snapshot, or a monthly board pack.


Use an alert when only the exception matters: you do not want a daily “revenue is fine” email, you want a message the moment revenue drops below plan.


Use neither, and just keep a live dashboard, when the data is consulted irregularly and only by people who will open the tool anyway. Scheduling a report nobody asked for is the most common way to manufacture noise.


## FAQ


### What is the difference between a scheduled report and a dashboard subscription?


They are usually the same thing. “Dashboard subscription” is the term Power BI, Tableau, and Metabase use for subscribing a person or group to a recurring delivery of a dashboard. “Scheduled report” is the more generic term. Both run on a schedule and push output to a channel.


### How often should reports be scheduled?


Match the cadence to the decision, not to the data. A report that drives a daily standup should arrive daily before the standup. A board metric can be monthly. Sending more often than the underlying decision is made just adds noise and, with live queries, warehouse cost.


### Can scheduled reports send different data to different people?


Yes, if the tool supports report bursting or data-driven delivery backed by row-level filtering. One report definition is filtered per recipient so each person sees only their slice. Without row-level filtering, you are stuck building a separate report per recipient.


### Why is my scheduled report empty or stale?


The two usual causes are a timing race (the report fired before the data refresh finished) and a broken upstream query that returns no rows but does not error. Sequence the refresh before the send with a buffer, and add a non-empty guard so a broken report fails loudly instead of sending blanks.


### Should I send a PDF, a CSV, or a link?


Send a PDF or image when layout matters and the recipient will read, not re-analyze. Send a CSV or Excel file when they will pivot or model the rows themselves. Send a link when they may want to drill into the live dashboard and you want the data always current.


## Where Basedash fits


Basedash is a modern BI tool where teams query databases, build dashboards with AI assistance, and schedule those dashboards to email and Slack on a recurring basis, with delivery scoped by the same workspace permissions and access controls that govern who can see what. For a lean team that wants recurring reports in front of people without standing up a separate reporting stack, scheduling sits next to the dashboard rather than in a separate admin console. As with any tool, weigh it against the capabilities above, especially the format and bursting needs specific to your audience.
