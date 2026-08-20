---
schema_version: "1.0.0"
document_id: "7e4ccf0c3bdc864ed61b256ddceb04f4f8febd00e9365aab240706c1d3d10f45"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-a-crm-with-ai"
published_at: "2026-06-13T00:23:53+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:48:58.638835+00:00"
content_hash: "sha256:602f823e605c39b94889dc6149d2a1c505ed311f5b90c29bdc3abf7ac02da048"
---

# How to Build a CRM from Scratch with AI (No Code Required)

## Building Your CRM with Blink


1


#### Write your first CRM prompt


The more specific your prompt, the more accurate your first build. Give Blink the exact fields and stages your team uses:


> "Build me a CRM with a contacts table (name, email, phone, company, status: lead/prospect/customer/churned), a deals pipeline with stages (New Lead, Qualified, Proposal Sent, Negotiation, Closed Won, Closed Lost), and a notes section linked to each contact and deal. Include user authentication and a dashboard showing total deals by stage and total pipeline value."


Go to[blink.new](https://blink.new/) and paste that prompt. Blink generates the database schema, the UI, the backend routes, and the auth layer in one session.


The database is auto-provisioned — no Supabase account needed. Your contacts are stored in a real SQL database from the first prompt.


2


#### Set up contacts and companies


After the first build, verify the contacts table has what your team actually tracks. Most sales teams need:


- Full name and email (required fields with validation)
- Phone number, company name, job title
- Status field with defined options (lead, prospect, customer, churned)
- Owner field to assign contacts to individual salespeople
- Created date for sorting and filtering


If you track company accounts separately:


> "Add a companies table with company name, website, industry, and employee count. Link each contact to a company. Show all contacts at a company on the company detail page."


Blink updates the data model and UI. No schema migration script required.


3


#### Build the deal pipeline


Your pipeline is where revenue visibility lives. Customize the stages to match how your team actually sells:


> "Show the deals pipeline as a Kanban board. Each deal card should display: deal name, value, contact name, assigned salesperson, and days in stage. Make cards draggable between stages. Add a 'reason lost' dropdown on Closed Lost cards."


The reason-lost field pays off in 90 days. You'll have real data on why deals don't close — that insight shapes your next quarter's process.


Add a probability field to each stage (New Lead: 10%, Qualified: 30%, Proposal Sent: 50%, Negotiation: 70%). Your dashboard can then show a weighted pipeline forecast automatically.


4


#### Add activity logging and notes


Every touchpoint with a prospect needs a record. Notes keep your team aligned; activity logs prevent "I thought you were following up" conversations.


> "Add an activity log where users can record calls, emails, and meetings. Each entry needs: activity type, date, a text note, and the user who logged it. Display the activity feed chronologically on each contact and deal page. Add a notes section with author name and timestamp."


With Blink, auth is built in — every note and activity entry automatically captures which user created it. No Firebase Auth to configure.


5


#### Connect reporting and dashboards


A CRM without reporting is a contact book. The reporting dashboard is where your sales manager actually works:


> "Add a reporting dashboard with: total open deals by stage, total pipeline value by stage, deals closed this month vs last month, win rate percentage, and average days to close. Show stat cards at the top and a bar chart of deals by stage below."


After two weeks of real usage, you'll know which metrics matter most. Follow-up prompts add date filters, salesperson filters, or trend lines in one conversation.


6


#### Invite your team


A CRM only works if everyone uses it. Blink includes user authentication out of the box:


> "Add user roles: admins can view and edit all contacts, deals, and users. Sales reps can only see and edit contacts and deals assigned to them. Show a 'My Deals' view that filters to the logged-in user's pipeline by default."


Once deployed, share the URL. Your team creates accounts and logs in immediately. Hosting is included — no Vercel config, no deployment pipeline. Your CRM goes live when you click publish.


## The 6 Features Your CRM Needs


A working CRM covers these six capabilities:


**Contact list with search.** Name, email, company, status, and last activity date — all searchable in under 300ms. Contacts without activity older than 30 days get a visual flag: follow up now or lose them.


**Deal pipeline with stages.** Kanban view, draggable cards, probability weighting. Your pipeline value at a glance — not buried in a spreadsheet.


**Activity log.** Every call, email, and meeting recorded with the author and timestamp. The full interaction history for any contact in one scroll.


**Task assignments.** Assign follow-up tasks to specific team members with a due date. A "my tasks" view shows what's due today. Nothing falls through the cracks.


**Email logging.** Gmail and Outlook integrations let your team log emails with one click. The email body, subject, and timestamp write to the contact's activity feed automatically.


**Basic reporting dashboard.** Win rate, pipeline value by stage, deals closed this month vs last month, and average days to close. Four numbers that answer "how is the team doing?" instantly.


## Integrating With Tools You Already Use


Your CRM connects to existing tools through follow-up prompts:


**Gmail and Outlook.** Add a "Log to CRM" button in your email client. One click records the email to the relevant contact's activity feed.


**Slack notifications.** When a deal moves to Closed Won, post a message to #sales-wins automatically. When a deal sits in Negotiation for 14 days, send an alert to the assigned salesperson.


**Stripe payment data.** Pull Stripe customer data into your CRM contacts. See payment history, subscription status, and lifetime value alongside the contact's activity feed.


**CSV import.** Export your contacts from Salesforce, HubSpot, or a spreadsheet, then upload. Most migrations complete in under 10 minutes.


## What You Give Up


Be honest about the tradeoffs. A custom CRM built in an afternoon is not a Salesforce replacement for every team:


**Enterprise compliance.** Salesforce has SOC 2 Type II, HIPAA, and FedRAMP certifications for regulated industries. Verify Blink's compliance posture if your customers require enterprise security certifications.


**Mobile app.** A Blink-built CRM is a web app. Salesforce has a native iOS and Android app with offline capability. If your reps work in areas without reliable internet, a native app matters.


**Advanced AI forecasting.** Salesforce's opportunity scoring and predictive forecasting train on years of your data. A custom CRM built today starts with zero historical training data.


For most teams under 50 people that need contacts, a pipeline, notes, and basic reporting: the custom build wins on cost, speed, and fit.


Custom CRM built with AI and Blink — contact list, deal pipeline, and reporting dashboard fully deployed after one afternoon


blink


*Custom CRM built with AI and Blink — contact list, deal pipeline, and reporting dashboard fully deployed after one afternoon*


Most teams have a working CRM — contacts, pipeline, activity log, basic reporting — in under 2 hours using Blink. The database, auth, and hosting are included automatically, so the entire session focuses on building features. Compare that to hiring a developer, which runs $5,000-15,000 and 2-4 weeks minimum.


Yes. Blink generates full-stack applications with a real persistent database and real user authentication. Your contacts, deals, and activity logs survive page refreshes. Your team logs in with accounts. The app runs at a real URL your team can bookmark. It's not a prototype.


Yes. Ask Blink to add a CSV import feature. Export your contacts from Salesforce or HubSpot as a CSV file, then upload. Most migrations finish in under 10 minutes. Field mapping — matching column names from your export to your CRM's fields — takes one follow-up prompt.


Salesforce Pro Suite is $100/user/month billed annually. For a 10-person team, that's $12,000 per year. Blink is free to start with one project. Custom domains and more projects start at $24.95/month. There are no per-seat fees — every team member can log in for the same flat cost.


Write a follow-up prompt describing what you need. New fields, new views, new reports, new integrations — each typically takes one or two prompts. The app is a real codebase, so there's no artificial limit on what you can add over time.


Only users with accounts in your CRM can access it. Blink's built-in authentication lets you control exactly who gets an account. Role-based access (admin vs sales rep vs read-only) is configurable with one prompt. Your data is isolated to your application.
