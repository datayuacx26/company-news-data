---
schema_version: "1.0.0"
document_id: "df1347ddbc8d099b97bc64d115d49f5bdc18516df3fbb898a7e9bfff93b73c8b"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-customer-portal"
published_at: "2026-06-06T12:42:05+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:cbb15422dc598ad2217ed2c391a57800060c6184a3c016eadde3406490d12abb"
---

# How to Build a Customer Portal With AI (No Developer Required)

## Step 2: Build the Client View


The client-facing side needs four things:


**Project list with status.** When a client logs in, they see their active projects sorted by recent activity. Status should be specific to your workflow: "In Review," "Pending Client Input," "Delivered" — not generic.


**File area per project.** Files should be organized by project. Clients upload documents you need; you upload deliverables. Keep it simple: name, date, who uploaded, download link.


**Comment thread.** A simple threaded comment on each project eliminates email for status questions. "What's the timeline on the logo?" becomes a comment on the project, not an email to search for.


**Invoice view.** Invoice date, description, amount, status (Paid/Outstanding). Read-only for clients. You update it from the admin side.


## Step 3: Build the Admin View


Your admin view is the operational layer:


- **All projects list.** Every client's projects with status and last update date.
- **Project update form.** Change status, add a comment, upload files.
- **Client manager.** Add new clients, set their login credentials or invite by email.
- **Invoice creator.** Add invoice entries to a client's account.


The admin and client views share the same database — they just show different things based on who is logged in. Blink handles this with role-based access control automatically when you describe it.


**Refinement prompt:**


> "Add an admin view where I can manage all clients, update any project, and add invoice records. Clients should only see their own data."


## Step 4: Client Onboarding


How clients get access matters. Two options:


**Option A: You create accounts.** You add each client's email in the admin panel. They get a login invite. Simple, controlled.


**Option B: Self-registration with approval.** Clients register themselves; you approve new accounts before they see any data. More scalable.


For most service businesses, Option A is better — you onboard clients at the start of every engagement anyway.


**What to tell new clients:**


> "You have access to \[company\] portal at \[your-domain\]. Log in with \[email\] to see your project status, files, and invoices. Add questions as comments on any project — I respond within one business day."


## Step 5: Deploy on a Custom Domain


Blink deploys your portal to a public URL automatically. To put it on your own domain (portal.yourcompany.com):


1. In Blink settings, add your custom domain
2. Add the DNS record it shows you (a CNAME entry at your domain registrar)
3. Done — your portal is live at your domain


This takes five minutes and makes the portal look like yours, not a third-party tool.


## What a Customer Portal Replaces


The tools people typically pay for and stop needing after building a portal:


- **Notion for client sharing.** Notion is a great internal tool; it is clunky for client portals (permission management is non-obvious, URLs are confusing for clients).
- **Dropbox/Google Drive shared folders.** Fine for file sharing; zero status visibility.
- **Client portal SaaS (Copilot, HoneyBook, Dubsado).** $25–80/month, per-seat, template-constrained.
- **Project management tools used as portals.** Asana, Monday — too complex for clients, not built for it.


With Blink, the database, auth, and hosting are included. One bill instead of five tools.


## Build This With Your AI Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a customer portal where clients log in and see their projects, shared files, and invoice history. I manage everything from an admin dashboard — update project status, upload files, add invoice records."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


A basic customer portal with login, project status, file sharing, and invoices takes 2–4 hours to build with Blink. More complex portals with custom workflows, notifications, or integrations take a day. No developer required.


Yes. Blink's auth supports multiple roles. Your whole team can have admin access, each with their own login. Role-based permissions let you control who can edit vs who can view only.


Blink includes file storage. Standard use cases — documents, PDFs, images, small videos — work without configuration. For large video files (100MB+), you may want to link to external file storage.


No. Describe what you want in plain language. Blink generates the database, the views, the auth, and the hosting. You adjust via conversation: "make the status dropdown include 'In Review' and 'Delivered'" and Blink updates it.


Blink has a free tier that covers small teams. Paid plans start from $20/month — compared to $30–80/month per client seat on dedicated portal SaaS tools.
