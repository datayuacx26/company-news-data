---
schema_version: "1.0.0"
document_id: "91836d9435bedf2db2f4f36fb26f081f1293e9b59c995b8504be2591f91dc454"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/what-operations-teams-build-with-blink"
published_at: "2026-05-05T00:43:11+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:40.928893+00:00"
content_hash: "sha256:8666e86cf9f27c3bb907e47c86f0c04cd11fbe8df2ef61fc284c2872b843f211"
---

# What Operations Teams Build With Blink: Dashboards, Trackers, and Approval Flows

Operations team using AI to build custom approval workflows and dashboards


Blink


The vendor invoice sat in an email thread for three days. The original requestor tagged the manager. The manager tagged finance. Finance replied asking for the purchase order number. The PO number was in a different email thread. Nobody approved it. The vendor followed up.


This is not a people problem. It's a tooling problem.


[58% of operations managers say tool fragmentation causes missed approvals](https://asana.com/resources/state-of-work) , according to Asana's State of Work report. The average ops team runs 4–8 different SaaS tools — and those tools don't talk to each other, so approvals bounce through email and Slack anyway.


The fix isn't another SaaS subscription. It's a custom tool that matches your actual workflow.


Operations teams are building those tools with[Blink](https://blink.new/) — an AI app builder where database, auth, and hosting are included. Here's what they're building and why it's working.


## The Per-Seat SaaS Problem


[Monday.com](https://monday.com/pricing) runs $12–20 per seat per month.[Asana](https://asana.com/pricing) is $13 per seat.[Airtable](https://airtable.com/pricing) hits $20 per seat. Combined, a 10-person ops team spends $450–530 per month on tools — and still builds approval flows in email.


Per-seat pricing scales painfully. A company that grows from 10 to 50 people triples or quadruples its SaaS bill without the tool changing at all.


Custom tools built with Blink have no per-seat fee. You build it once. The whole team uses it. The cost stays flat.


## 6 Tools Operations Teams Are Building With Blink


### 1. Vendor Approval Flow


**The problem:** Vendor invoices, software purchases, and budget requests move through email. Nobody knows who's approved what or what's still pending.


**What teams build:** A three-stage approval workflow — requestor submits with amount, vendor, description, and supporting docs; manager approves or rejects with a comment; finance marks as paid and attaches the receipt. Every stage sends an automated notification. The dashboard shows all pending requests by status, requester, and department.


**What it replaces:** Email chains, Slack threads, and disconnected spreadsheets. Some teams replace a Monday.com workflow they never customized past the default template.


### 2. Company Resource Tracker


**The problem:** No one knows who has the MacBook, which software licenses are active, or when the SaaS subscriptions renew.


**What teams build:** A searchable inventory of equipment, software licenses, and subscriptions. Each record has an owner, status, renewal date, and cost. Ops gets an automated alert 30 days before any renewal. Employees can request resources through the same app, and requests flow through a lightweight approval.


**What it replaces:** A shared Airtable that one person maintains, or a Google Sheet that's always out of date.


### 3. New Hire Onboarding Checklist App


**The problem:** Onboarding steps live in a Google Doc that someone updates once a year. New hires don't know what they've missed. Ops doesn't know who's completed what.


**What teams build:** A per-employee onboarding tracker. Each new hire has their own checklist — IT setup, policy acknowledgments, tool access requests, intro meetings. Managers see completion status across all new hires. Ops can see which steps are consistently delayed and fix the bottleneck.


**What it replaces:** The Google Doc, plus the Slack messages asking "did you get your laptop access set up?"


### 4. Budget Request Tool


**The problem:** Budget requests go into a generic form or email. Finance has no visibility until month end. Ops can't see total committed spend versus budget.


**What teams build:** A budget request app with department-level tracking. Requestors submit with cost, rationale, department, and budget code. Approvers see the running total against each department's budget before approving. Finance sees all approved requests for the current period in one view.


**What it replaces:** Spreadsheets emailed to finance, and the guesswork about remaining budget.


### 5. Operations Dashboard


**The problem:** Ops metrics live in five different places. Building a view requires exporting CSVs and pasting them together.


**What teams build:** A real-time dashboard pulling from the team's existing tools — headcount, open reqs, budget utilization, vendor contract statuses, and onboarding completion rate. Blink can pull from APIs or receive webhooks from existing systems and display everything in one place.


**What it replaces:** The weekly PowerPoint that someone builds manually every Friday afternoon.


### 6. SOC Tracker


**The problem:** SOC 2 compliance requires tracking evidence across dozens of controls. Most teams manage this in a spreadsheet with columns for control status, owner, evidence link, and review date.


**What teams build:** A SOC tracker with one row per control, assignable owners, status (in progress, complete, needs review), evidence attachments, and automated reminders before audit dates. Auditors get read-only access to specific sections without seeing the whole system.


**What it replaces:** A Google Sheet that seven people edit simultaneously, and the anxiety before every audit cycle.


Custom Blink tools vs per-seat SaaS cost comparison


Blink
