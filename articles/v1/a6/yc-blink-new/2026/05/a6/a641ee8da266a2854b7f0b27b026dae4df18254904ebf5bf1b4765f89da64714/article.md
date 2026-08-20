---
schema_version: "1.0.0"
document_id: "a641ee8da266a2854b7f0b27b026dae4df18254904ebf5bf1b4765f89da64714"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-expense-tracker-app"
published_at: "2026-05-15T00:56:02+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:13:07.113521+00:00"
content_hash: "sha256:4137493000d5524d6f441c9fd9444283c09db50a3c92ca1811f94bb93370f3e3"
---

# How to Build an Expense Tracker App (Without Code) in 2026

## How to Build Your Expense Tracker with Blink


1


#### Define your expense categories and approval flow


Before generating anything, decide: (1) Which expense categories matter to your team? (Meals, Travel, Software, Equipment, Office Supplies, Other), (2) Is approval required for all expenses or only above a dollar threshold?, and (3) Who are the approvers — managers by department, or a single finance admin? These decisions shape the database schema. Specificity here saves 2–3 revision cycles later.


2


#### Generate the full-stack app with Blink


Go to[blink.new](https://blink.new/) and describe your expense tracker: "Build an expense tracker where team members can submit expenses with amount, date, category, description, and receipt photo upload. Managers can approve or reject submissions. Include a dashboard showing expenses by category, monthly totals, and a searchable history." Blink generates the database schema, the submission form, the approval workflow, and the reporting dashboard from this description. Database automatically included. Auth is built in so users have individual accounts.


3


#### Configure receipt upload


Receipt capture is the feature that makes expense reports credible. Tell Blink: "Add a photo upload field to the expense form. Users should be able to take a photo from their phone or upload a file from their computer. Store the receipt and link it to the expense record." Blink handles file storage and generates secure URLs for each uploaded image. No AWS S3 configuration needed. The receipt thumbnail appears in the expense list and approval views automatically.


4


#### Set up the approval workflow


An approval workflow has three states: submitted, approved, rejected. Each transition needs to be logged with a timestamp and the approver's identity. Tell Blink: "Add an approval workflow where submitted expenses show in a manager's review queue. Managers can approve or reject with an optional note. Submitters see the status change and note on their expense." With Blink, auth is built in — the manager's identity is automatically captured on each approval action. No manual session handling.


5


#### Build the reporting dashboard


Reports are what turn an expense tracker into a finance tool. Describe to Blink: "Build a reports page showing: total spending by category this month as a bar chart, spending over the last 12 months as a line chart, per-person spending breakdown for the current month, and a table of all expenses filterable by date range, category, and status." These are aggregation queries — Blink generates the SQL and the chart components from your description. Ships in minutes — no DevOps.


6


#### Add CSV export for accounting


Every expense app needs an export for accountants who live in Excel. Tell Blink: "Add a CSV export button that downloads all approved expenses in a date range, with columns for date, submitter, amount, category, description, and approval status." The export is a database query formatted as CSV. With Blink, this is a single API endpoint. Accountants get their spreadsheet; the app keeps the authoritative data. No more emailing spreadsheets back and forth.


## Key Features to Add After Launch


**Category budget limits.** "Travel spending is over $2,000 this month" alerts prevent end-of-month surprises. This is a comparison of monthly aggregate spend against a configured budget per category. Add a budget configuration table to the database, run the comparison query on each new expense submission, and send an alert if the running total crosses 80% or 100% of the budget. With Blink, the database is automatically included — the alert logic is a background function.


**Mobile-first receipt capture.** 70% of expense submissions happen on mobile, immediately after spending. The app needs to work on mobile browsers with the device camera accessible for receipt photos. Blink generates mobile-responsive UIs by default. Test the receipt upload on iOS Safari and Android Chrome specifically — file upload inputs work differently on mobile.


**Recurring expenses.** Subscriptions (AWS, Figma, Notion) recur on a fixed schedule. Adding a "recurring" flag to an expense lets the app auto-generate next month's entry with the same details. This is a scheduled job that runs on the first of each month. Tell Blink: "Add a recurring option to expenses. Recurring expenses auto-create next month." Blink handles the database and the scheduling.


**Mileage tracking.** If your team drives for work, IRS mileage reimbursement ($0.70/mile in 2026) is a significant expense category that spreadsheets handle badly. A mileage log — date, origin, destination, miles, purpose — with automatic dollar calculation is a standard expense tracker feature. Add it as a separate entry type with its own form fields.


**Team spending limits.** Per-user monthly spending caps give finance teams control without micromanagement. An employee with a $500/month limit gets a soft warning at $400 and a block at $500 (or a flag for manager review). This is a role-based configuration in the database, checked on each submission. Auth is built in with Blink, so the user identity and their spending limit are accessible on every request.


For more on building finance and business tools without a developer, see[Vibe Coding for Beginners](https://blink.new/blog/vibe-coding-for-beginners) and[Build a SaaS Without Coding](https://blink.new/blog/build-saas-without-coding) .


Expensify's pricing is[publicly listed](https://www.expensify.com/pricing) at $5/active user/month for their Collect plan. For a 20-person team submitting expenses monthly, that's $100/month ongoing. A custom Blink app starts free. The database, auth, and hosting are all included. The only ongoing cost is if you exceed the free tier as the team grows.


## Frequently Asked Questions


Most teams have a working expense tracker — with submission form, receipt upload, approval workflow, and reports — in under 2 hours. The initial Blink generation takes 15–20 minutes. Configuring the approval workflow, testing the receipt upload, and customizing the reports dashboard takes another 60–90 minutes. Hosting is included, so the app is live on a real URL the moment generation finishes. Ships in minutes — no DevOps.


No. Blink generates the full-stack application from a plain-English description — the frontend forms and dashboard, the backend API, the database schema, and the file storage for receipts. You describe what you want, Blink builds it. If something isn't right, you describe the change. Auth is built in so users have individual accounts automatically.


Receipt photos are uploaded to file storage when the expense is submitted. The file URL is saved in the expense database record. When viewing an expense, the app fetches the receipt URL and displays the image. With Blink, file storage is included — no AWS S3 account needed. Receipts are stored securely and accessible only to the submitter, their manager, and admin users. Auth is built in to enforce these access rules.


Yes. Blink's built-in auth supports multiple user accounts with different roles — submitters and approvers. Each user logs in with their own account and sees only their expenses (plus the approval queue for managers). There's no extra configuration for multi-user support. The database schema Blink generates includes a user_id column on every expense record, which enforces data isolation between users automatically.


The app includes a CSV export that downloads all approved expenses in a selected date range. Columns include date, submitter name, amount, category, description, approval status, and approver name. Import this into Excel, QuickBooks, or your accounting tool. For automated sync (daily export to accounting software), Blink supports API webhooks that fire on expense approval events.


Expensify is a mature product with features most small teams don't use — corporate cards, multi-entity accounting, Concur integration. A custom expense tracker has exactly the features your team needs and none it doesn't. It's also permanently free at small scale, versus Expensify's $5–9/user/month ongoing cost. The tradeoff: Expensify is ready immediately, while a Blink app takes 2 hours to build. For teams with specific workflows — custom approval chains, unusual categories, integration with internal tools — a custom app usually wins within 3–6 months.


Yes. QuickBooks has an API that accepts expense imports. After building the expense tracker, add a sync function: on expense approval, send the expense data to the QuickBooks API. Tell Blink: "When an expense is approved, send it to my QuickBooks account via their API." You'll need your QuickBooks API credentials. Blink stores them securely and makes the API call from the backend on each approval. No client-side credentials needed.
