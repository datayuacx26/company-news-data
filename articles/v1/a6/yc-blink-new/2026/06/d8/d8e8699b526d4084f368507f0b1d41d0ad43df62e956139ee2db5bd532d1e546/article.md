---
schema_version: "1.0.0"
document_id: "d8e8699b526d4084f368507f0b1d41d0ad43df62e956139ee2db5bd532d1e546"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-feedback-tool"
published_at: "2026-06-07T00:50:08+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:f3a20ac05c38afc254a9451ba7adeb6fe37424fe3474d090c94b7e8a9eb953e4"
---

# How to Build a Feedback Tool

## Five feedback tools you can build today


### 1. NPS survey (0–10 scale, auto-categorized)


The NPS survey is the simplest feedback tool that still gives you actionable signal.


Describe it to Blink: *"Build an NPS survey. Show a 0–10 scale. Capture the score, an optional comment, and the user's email. Auto-categorize: 0–6 = detractors, 7–8 = passives, 9–10 = promoters. Show an admin dashboard with the current NPS score, a trend chart, and a list of recent responses filterable by category."*


Blink provisions the database schema, the survey form, the scoring logic, and the admin view from that single prompt. What you get:


- ` nps_responses` table:` user_email` ,` score` ,` category` ,` comment` ,` submitted_at`
- Admin view: current NPS score + 30-day trend + response list
- Email alert to the product team on every detractor score (0–6)


The most valuable signal isn't the aggregate NPS number. It's the verbatim comments from detractors. Build the search and filter for those first.


### 2. Feature voting board


Users suggest features and upvote the ones they want most. The product team sees a ranked list sorted by vote count.


Describe it to Blink: *"Build a feature voting board. Users can submit feature requests and upvote existing ones. Each request has a title, description, status (under review, planned, shipped), and vote count. Show a public board sorted by votes. Include an admin view where I can change status and add internal notes."*


What you get:


- Public board: anyone can submit and vote without an account
- Status workflow: Under Review → Planned → Shipped (with email notification to voters)
- Admin panel: change status, merge duplicate requests, add private notes


The status notifications matter. When you move a feature from "Planned" to "Shipped," every user who voted gets an email. That's the interaction that turns passive voters into vocal advocates.


### 3. Bug report tracker


A simple form that routes bug reports into a database with admin triage. Better than Jira for small teams; faster to query than a spreadsheet.


Describe it to Blink: *"Build a bug report form. Capture: title, description, steps to reproduce, severity (low/medium/high/critical), browser/OS, and an optional screenshot upload. Create an admin dashboard with a list of all bugs, filterable by severity and status. Statuses: New, In Progress, Fixed, Won't Fix."*


The screenshot upload is the part that takes 20+ minutes to wire up manually. Blink handles file storage automatically — no S3 bucket configuration, no signed URLs to write.


### 4. In-app feedback widget


A floating button in the corner of your app. User clicks it, a modal slides in, they type their feedback, and it submits to your database.


Describe it to Blink: *"Build an in-app feedback widget. Add a floating button in the bottom-right corner of the page. When clicked, open a modal with: a dropdown ('What kind of feedback?': Bug Report, Feature Request, General Feedback), a text area, and a submit button. On submit, save to the feedback table and send an email notification to the product team."*


The result is a reusable component. Add it to any page with a single import. All submissions flow to the same database — searchable, exportable, and queryable without leaving your own infrastructure.


Building an in-app feedback widget — the floating button, modal, and database wiring up automatically


Blink


*Building an in-app feedback widget — the floating button, modal, and database wiring up automatically*


### 5. Post-churn survey


Triggered when a user cancels their subscription. Captures why they left while the reason is still fresh.


Describe it to Blink: *"Build a post-churn survey. When a user cancels, send an email with a link to a short survey. Questions: 'Why are you canceling?' (multiple choice: Too expensive / Missing features / Not using it enough / Switching to competitor / Other) and 'What would bring you back?' (open text). Store results in a churn_responses table. Show an admin dashboard with a breakdown of cancellation reasons."*


The cancellation reason breakdown is where the product insight lives. If 60% of churned users cite "too expensive" and 5% cite "missing features," that's a pricing conversation, not a roadmap one.


## Build steps


1


#### Describe your feedback tool to Blink


Go to[blink.new](https://blink.new/) . Use one of the prompts above, or describe what you need in plain language. Be specific about what data you want to capture and what the admin view should show.


Blink generates the full stack — form, database, admin dashboard, and notification logic — from your description. Database is automatically included. No Supabase account, no separate billing.


2


#### Review the generated schema


Blink provisions the database automatically. Verify the columns match what you described. Common additions to request: a` metadata` JSON column for future flexibility, a` user_id` foreign key if you're tying feedback to authenticated users, and an` is_archived` flag for the admin view.


3


#### Customize the survey questions


Tell Blink to update the form: change question wording, add or remove fields, update the rating scale. For NPS specifically: confirm the 0–10 scale renders as a horizontal button row, not a dropdown — the visual format affects response rates.


4


#### Configure notifications


Tell Blink: "Send an email to \[your email\] every time a new response is submitted. For NPS, only send immediately for scores 0–6. Batch weekly digest for 7–10."


Auth is built in to Blink. If your feedback tool is tied to user accounts, Blink wires the` user_id` to the feedback record automatically — no Clerk or Firebase Auth to configure separately.


5


#### Deploy and share the URL


Blink handles hosting. Your feedback tool is live on a Blink-subdomain immediately. Point a custom domain when you're ready — no Vercel configuration, no DNS gymnastics beyond the CNAME. 200+ AI models available if you want to add AI-powered response analysis later.


## The thing you get for free: your data, your schema


The most underrated benefit of building your own feedback tool isn't the cost savings. It's data ownership.


With Canny, UserVoice, or Typeform, your data lives in their database. You can export CSVs, but you can't run` GROUP BY churn_reason WHERE plan = 'pro' AND tenure_days < 30` . You can't join it against your billing data to calculate the revenue impact of specific feature requests. You can't trigger a Slack message when a high-LTV customer submits a detractor NPS score.


When you build with Blink, the database is yours. You query it directly. The schema matches your product's domain model. The data doesn't disappear if you stop paying a monthly subscription.


That's what $149–$499/mo is actually buying: a managed database you don't control. The moment you outgrow the canned reports, you're doing SQL exports anyway.


Build it once, own it forever.


Querying your own feedback database directly — the data stays yours, the schema matches your product


Blink


*Querying your own feedback database directly — the data stays yours, the schema matches your product*


## Frequently Asked Questions


Under an hour for a functional NPS survey or bug tracker. The database, form, and admin dashboard generate from a single prompt. Add another 20–30 minutes to customize survey questions, tweak the admin view, and configure email notifications. Most teams have something deployable the same day they start.


Yes. Describe the public board, voting mechanics, and admin panel to Blink and it generates the full stack. The difference from Canny: your board runs on your domain, in your database, with your branding — and there's no per-tracked-user pricing. If you need Canny's enterprise features (SSO, CRM integrations, dedicated support), Canny is still the right choice. If you need the functionality without the pricing, build it.


No. The prompts in this guide are written in plain English and generate working code. You can describe changes in natural language — "add a priority field," "show only detractors in the default view," "send a Slack notification instead of email" — and Blink updates the code accordingly. Database automatically included. Auth built in.


Yes. The widget Blink generates is a standard web component. Copy the embed code into any HTML page, React app, or Next.js project. Blink handles the API endpoint, database write, and notification on submit. No backend changes to your existing app required.


It's in a standard Postgres database. Export to CSV, connect a BI tool directly, or migrate to another platform by exporting the schema and data. You're never locked into a proprietary format — the data is rows in a table, which is also true of Canny and Typeform, except you don't own their tables.


Yes. Tell Blink: "When a user's status changes to 'cancelled,' trigger the post-churn survey email automatically." Blink wires the webhook between your subscription system and the survey email. If you're using Stripe for billing, describe that integration and Blink handles the webhook handler.
