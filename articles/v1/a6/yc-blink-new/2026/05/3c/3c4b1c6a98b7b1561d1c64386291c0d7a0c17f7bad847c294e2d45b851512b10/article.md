---
schema_version: "1.0.0"
document_id: "3c4b1c6a98b7b1561d1c64386291c0d7a0c17f7bad847c294e2d45b851512b10"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-time-tracker"
published_at: "2026-05-24T01:41:06+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:55ba866cbd9b72dd710c4987491b1dcd8d3ce689370d855dcc80899aadedd396"
---

# How to Build a Time Tracking App With AI (Toggl Alternative in Hours)

## Step 1: Define Your Projects and Billing Rates


Open[blink.new](https://blink.new/) and describe what you're building. Specificity matters here:


> "Build a time tracking app for a freelance agency. We need: client and project setup where each project belongs to a client, a configurable hourly billing rate per project, user accounts for up to 25 team members, and an admin role that can see all team entries."


Blink generates the full-stack app — database, auth, and UI in one pass. No Supabase account to configure. No Vercel setup. Auth is built in, not something you wire up later.


The database will include tables for clients, projects (with a` billing_rate` column), users, and time entries linked to both a project and a user.


## Step 2: Build the Timer and Time Entry Screen


Add the core tracking UI with a follow-up prompt:


> "Add a timer screen with a one-click Start/Stop button. When stopped, automatically create a time entry with: project, start time, end time, duration, and a billable/non-billable toggle. Also allow manual time entry by typing start and end times."


The billable/non-billable toggle is not optional. You need it to separate client-facing work from internal meetings and admin so your reports show accurate billable hours — not inflated ones.


## Step 3: Add Team Members and Role-Based Access


Prompt next:


> "Add team member management. Admins can invite users by email. Regular users see only their own time entries. Admins see all team entries. Show a weekly hours total per team member on the admin dashboard."


Blink's built-in auth handles the role separation — you're not writing middleware or configuring permission layers. You describe the access model; Blink implements it.


Teams that track time accurately are[20–30% more productive](https://punchly.work/blog/hidden-costs-of-not-tracking-time-in-business/) than those using manual methods. The admin dashboard makes that difference visible: you see who's logging hours, who isn't, and which projects are consuming more time than they should.


## Step 4: Wire Up Invoice Generation


This is the feature that turns a time tracker into a billing tool. Prompt:


> "Add invoice generation. Admin selects a client and a date range. The app pulls all billable time entries for that client, calculates total hours × billing rate, and generates a PDF invoice with: company name, client name, line items by project, total due, and invoice number. Allow download as PDF."


For a standalone deep dive on invoice building, see our[invoice generator app guide](https://blink.new/blog/how-to-build-invoice-generator-app) . This article treats invoice generation as a core module of the time tracker — not a separate export step.


One data point worth building around: clients who receive invoices with task-level time breakdowns are 3× more likely to pay on time than clients who get a lump-sum total. Detailed line items eliminate disputes before they happen.


## Step 5: Add the Reports Dashboard


Prompt:


> "Add a reports page. Show: total billable hours by client (this month vs last month), total billable hours by project, hours by team member, and which projects are over or under their estimated hour budget. Include a profit indicator — billed hours vs target hours per project."


This is the profitability reporting that Toggl locks behind $18/seat. You're building it directly into your tool, with the exact columns your team needs.


You'll probably need one iteration on this prompt. Ask for a chart on the second pass if the table view isn't enough.


## Step 6: Deploy and Share with Your Team


Add timesheet approval before you ship:


> "Add a timesheet approval flow. Team members submit their weekly hours for manager review. Admins see a queue of pending submissions, can approve or request changes with a note. Approved timesheets are locked — entries can no longer be edited."


Then deploy. With Blink, there's no Vercel config, no environment variables to manage, no DNS to wire manually. One click. Share the URL with your team.


The whole build — from first prompt to live URL with five team members — is under two hours. The feature that took most iterations in my own build was the reports dashboard. Budget 30 minutes for that section alone.


## What to Build Next


Three extensions that turn the tracker into a full billing suite:


**Connect Stripe for invoice payments.** One sentence in your next Blink prompt: "Add a Stripe payment link to each generated invoice so clients can pay online." Blink handles checkout, webhooks, and payment status. Clients click Pay Now; the invoice marks itself paid.


**Slack alerts for budget overruns.** Ask Blink to send a Slack notification when a project hits 80% of its hour budget. Catch it before you're underwater, not after the invoice goes out.


**Client reporting portal.** Build a read-only view where clients log in and see their project hours in real time. Cuts "how many hours do we have left?" emails — and positions your agency as the transparent one.


For the full foundational tutorial covering every feature from scratch, see our[step-by-step time tracking app guide](https://blink.new/blog/how-to-build-a-time-tracking-app) .


## Frequently Asked Questions


Toggl is built for the average workflow. A custom build gives you your specific billing rates per client, your invoice format, your approval flow — and profitability reporting without the $18/seat Premium upgrade. You build it once and own it at any team size.


No. Every step in this guide is a prompt. Blink generates the full-stack app — database schema, auth, and UI — in one pass. You iterate by describing what to change, not by writing code.


The core app — timer, projects, billable rates, user accounts — takes under an hour. Invoice generation and timesheet approval add another 30–45 minutes of iteration. Full build is under two hours for most people starting from scratch.


No per-seat fee. You built it once — add users as team members in the app. For comparison: 50 users × $9 Toggl Starter = $5,400/year. 50 users × $18 Toggl Premium = $10,800/year. Your custom build: same flat cost at any team size.


Yes. Add one line to your Blink prompt: "Add a Stripe payment link to each generated invoice so clients can pay online." Blink wires up Stripe checkout, handles the webhook, and updates the invoice status when payment clears.


Ask Blink to add a CSV export formatted for QuickBooks import — that's a five-minute addition. For a native sync, describe the integration behavior in plain English and Blink builds the API connection. Both routes work; CSV is faster to ship.
