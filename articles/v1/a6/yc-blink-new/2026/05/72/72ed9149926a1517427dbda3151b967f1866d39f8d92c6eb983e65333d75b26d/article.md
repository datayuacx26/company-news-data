---
schema_version: "1.0.0"
document_id: "72ed9149926a1517427dbda3151b967f1866d39f8d92c6eb983e65333d75b26d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-a-dashboard"
published_at: "2026-05-03T12:18:03+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:15:27.754583+00:00"
content_hash: "sha256:1ba5a7f5cd4090be9a8aedde8d95e522d4201ffe13670fb18ab25478ad4d2589"
---

# How to Build a Custom Business Dashboard Without Code

## The build prompt


Open[blink.new](https://blink.new/) and describe your specific dashboard:


```text
Build a business metrics dashboard with:


KPI Cards (top row):
- Monthly recurring revenue (sum of active subscriptions)
- New signups this month vs last month (with % change)
- Active users (logged in last 30 days)
- Churn rate this month (cancelled / active at start of month)


Charts:
- Line chart: daily signups over last 30 days
- Bar chart: revenue by plan tier (Starter, Pro, Max) this month
- Line chart: daily active users over last 30 days


Data Tables:
- Recent signups (last 50): name, email, plan, signup date
- At-risk customers: users who haven't logged in for 21+ days


Filters:
- Date range picker (default: last 30 days)
- Plan tier filter


Refresh: data updates every 5 minutes automatically


Design: clean, dark mode, professional


```


Adjust the metrics to match your business. Blink includes the database automatically — no Supabase setup. Auth is built in — only logged-in users can view the dashboard.


## Real-world dashboard types


### SaaS metrics dashboard


For subscription businesses, the key metrics are MRR, churn, LTV, and activation rate.


```text
SaaS dashboard with:
- MRR: total active subscription revenue
- New MRR: revenue from new customers this month
- Expansion MRR: upgrades this month
- Churned MRR: cancellations this month
- Net MRR movement: New + Expansion - Churned
- Average subscription age: mean days since signup for active users
- Activation rate: users who completed onboarding / total signups


```


### E-commerce operations dashboard


For product businesses, the key metrics are orders, revenue, and fulfillment.


```text
E-commerce dashboard with:
- Total orders today vs yesterday vs same day last week
- Revenue today with 7-day trend line
- Average order value this month
- Top 10 products by revenue this month
- Fulfillment queue: orders pending shipment
- Return rate this month


```


### Agency client dashboard


For service businesses, billable hours and project status matter most.


```text
Agency dashboard with:
- Billable hours this month by client
- Utilization rate by team member
- Active projects: name, client, status, deadline
- Revenue forecast: contracted value remaining this month
- Overdue tasks: tasks past deadline by project


```


## Connecting your data sources


A dashboard is only useful if it shows real data. Three approaches:


**1. Blink's built-in database (easiest)**


If you build your product on Blink, the dashboard queries the same database your app uses. Instant, real-time. No configuration.


**2. CSV import for historical data**


For dashboards built on existing data: "Add a CSV import button to the admin section. When I upload a CSV, map the columns to the existing schema and import." Upload your Stripe export, your CRM export, your spreadsheet.


**3. API integrations**


For live data from external tools:


- "Connect to Stripe API and pull subscription and revenue data — refresh every hour"
- "Connect to Google Analytics API and show page views and sessions"
- "Connect to HubSpot API and pull deal pipeline and contact data"


Describe the integration in plain English. Blink generates the API connection.


## Making dashboards that actually get used


Most dashboards fail because they show too much. Nobody opens a dashboard with 40 charts daily.


**One screen, one story.** Every dashboard should have a single narrative: "Is the business healthy this week?" Everything that doesn't answer that question belongs on a secondary page or not at all.


**Start with five metrics.** Every business has five metrics that determine success. Find those five. Build the dashboard around them. Add more only after you've validated they get used.


**Refresh rate matters.** Dashboards that require manual refresh don't get checked. Set the auto-refresh interval to match how often the underlying data changes: every 5 minutes for real-time operations, every 2 hours for daily business metrics, daily for weekly trends.


**Mobile access.** The executive checking numbers before a meeting is on their phone. Tell Blink: "Make the dashboard responsive — KPI cards stack on mobile, charts scroll horizontally on small screens."


For broader context on building production apps without code, see[vibe coding best practices](https://blink.new/blog/vibe-coding-best-practices-production-2026) and[what sales teams build with AI](https://blink.new/blog/what-sales-teams-build-with-ai) .


Any data stored in your Blink database, data from connected APIs (Stripe, HubSpot, Google Analytics, Shopify), and data imported via CSV. The dashboard can display metrics, trends, tables, and anything you can describe in plain English. If the data exists somewhere accessible, Blink can pull it and display it.


Auth is built in — only logged-in users can access it by default. Add role-based access: "Only users with the 'admin' role can view the finance section. Regular team members can see the operations metrics." Blink wires the permissions automatically.


Yes. Tell Blink: "Add a read-only view at /dashboard/embed that doesn't require login, protected by a secret token in the URL." Useful for displaying metrics in a TV display, a client portal, or another application.


Describe the report: "Every Monday morning at 8am, send a weekly summary email to the team with this week's KPI snapshot: total revenue, new users, churn rate." Blink generates the email template and the scheduled task. No external email service configuration required.


Yes. With Blink's built-in auth, multiple team members log in with their own credentials. You can add role-based access so different roles see different data, and every action is logged to the audit trail.
