---
schema_version: "1.0.0"
document_id: "d89aea4d3c8eccd840db3a5972f1da0acb96454dc524ae593ec837348071ccbb"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-expense-tracker"
published_at: "2026-05-28T12:36:49+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:03.604597+00:00"
content_hash: "sha256:b9ae2976c4ce5fee2013f17d7b310bff8a3f4d6c21440ae2eecfe3bb6147f668"
---

# How to Build an Expense Tracker

## Step 1: Describe Your Expense Tracker


Open[blink.new](https://blink.new/) and describe what you need in plain English.


For a team tracker, start with:


> "Build a team expense tracker. Employees log expenses with amount, date, category, description, and a receipt photo. Managers can see all expenses and approve or reject them with a note. Employees see only their own submissions. Add a reporting dashboard that filters by employee, category, and date range."


Blink reads this and generates the full application: database schema, auth system, user interface, and business logic. You don't write a single line of code.


## Step 2: Review the Expense Form


The generated app includes an expense entry form. Check that it has:


- Amount (numeric, required)
- Date (picker, defaults to today)
- Category (dropdown with your categories)
- Description (text field)
- Receipt (file upload — photo or PDF)


If a field is missing, describe the change: "Add a receipt upload field to the expense entry form." Blink updates the form and the database schema in seconds.


## Step 3: Set Up Categories and Budgets


Categories organize expenses and power budget tracking. Blink creates defaults, but you define your actual categories.


Prompt: "Add a settings page where admins can add and edit expense categories. Each category should have a name and a monthly budget amount."


This creates a categories table in the database linked to expenses. The budget dashboard reads both tables to calculate remaining budget per category — the core of any useful expense tool.


## Step 4: Build the Reporting Dashboard


Good reporting is where expense trackers earn their keep. The right dashboard shows you where money goes, by whom, and when.


Prompt: "Build a reporting dashboard showing: total expenses by category this month, expenses by employee with subtotals, and a filter panel for date range, category, and employee. Export the filtered view as CSV."


Blink generates charts and filterable tables. Queries run against your PostgreSQL database — no analytics SaaS, no extra bill.


## Step 5: Add the Approval Workflow


The approval workflow is the most critical piece for team tools. Without it, managers have no visibility before reimbursements happen.


Prompt: "Add an approval workflow. Submitted expenses appear in a Pending queue for managers. Managers approve or reject with a note. Employees see the status of each submission in their view."


This adds a status field on each expense record (pending, approved, rejected) and a manager dashboard for pending items. Auth is built in — Blink's auth system handles the role-based views automatically, so no Clerk configuration is needed.


## Step 6: Deploy


Click Deploy in Blink. Your expense tracker goes live at a public URL within minutes. No Vercel config, no CI/CD pipeline, no domain complications.


Share the URL with your team. Each employee creates an account through Blink's built-in auth and lands on their own expense view. Managers see the full approval dashboard.


## The Cost Comparison


Custom-built expense tracker vs SaaS subscription cost — building your own eliminates per-user fees


Blink


[Zoho Expense charges $4–6/user/month](https://www.zoho.com/us/expense/pricing/) billed monthly — $40–60/month for a 10-person team. Expensify charges approximately $5–9/user/month depending on the plan — $50–90/month for 10 people.


Both tools require you to adapt your workflow to their structure. Approval chains, category names, and reimbursement policies follow the SaaS defaults. Changing them means upgrading your plan.


Expensify Zoho Expense Custom Blink Build


Monthly (10 users) $50–90/mo $40–60/mo $0 (free tier)


Monthly (20 users) $100–180/mo $80–120/mo $0–20/mo


Categories Fixed tiers Fixed tiers Fully custom


Approval workflow Built-in Built-in Fully custom


Data ownership Expensify Zoho You


Setup time 1–2 hours 1–2 hours 1.5–2 hours


A custom build matches your workflow exactly. You define the categories, the approval chain, the budget periods, and the reporting logic.


## What Blink Includes That You'd Otherwise Configure


Component DIY Stack Blink


Database Supabase ($25/mo) Included


Authentication Clerk ($25/mo) Included


Hosting Vercel ($20/mo) Included


Backend API Custom code (8+ hours) Generated


**Total** **$70+/mo + 8 hours** **$0 to start**


Blink includes the database automatically — expense records persist in a PostgreSQL database without any Supabase setup. Auth is built in — each employee logs in with their account and sees only their own expenses, no Clerk configuration required. Hosting is included — your expense tracker deploys to a live URL instantly, no Vercel config needed.


If you need a broader context for how this compares to other tools on the market, see[the best AI app builders](https://blink.new/blog/best-ai-app-builders) for a full breakdown.


## What to Add Next


Once your base tracker is live, these are the most common extensions:


- **Receipt OCR** — prompt Blink to add text extraction from uploaded receipt images
- **Recurring expenses** — flag subscriptions separately from one-time costs
- **Mileage tracking** — add a miles field with auto-calculation at IRS rate
- **Multi-currency** — add a currency selector and conversion rate field for international teams
- **Slack or email notifications** — trigger a notification when an expense is approved or rejected


Each of these is a one-prompt addition in Blink.


## Frequently Asked Questions


A personal expense tracker takes under an hour. A team expense tracker with approval workflows and role-based views takes 1.5–2 hours. Blink provisions the database, auth, and hosting automatically — you're not configuring infrastructure.


No. You describe what you need in plain English and Blink generates the application. You modify the app the same way — by describing the change. Blink supports 200+ AI models, so you're not locked into one underlying engine.


Yes. Blink includes file storage — no S3 bucket or Cloudinary account to configure. Add a receipt upload field to the expense form by describing it, and Blink adds the field, storage wiring, and display logic.


A spreadsheet breaks with multiple users, has no approval workflow, and requires manual data entry with no validation. Your Blink expense tracker is a real web app with user accounts, structured database records, and server-side logic. It scales to 50 employees without breaking.


Blink's free tier covers you while you're getting started. Paid plans start at $20/month — still a fraction of per-user SaaS pricing at scale. Your expense data stays in your PostgreSQL database and is never locked in a vendor's schema.


The CSV export works with most accounting import workflows. For direct integrations, describe the connection you need: "Push approved expenses to QuickBooks via webhook." Blink can generate the backend logic for that.
