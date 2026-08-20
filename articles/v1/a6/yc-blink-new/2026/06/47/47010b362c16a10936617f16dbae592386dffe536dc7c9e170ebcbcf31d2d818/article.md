---
schema_version: "1.0.0"
document_id: "47010b362c16a10936617f16dbae592386dffe536dc7c9e170ebcbcf31d2d818"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-scheduling-app"
published_at: "2026-06-11T12:30:21+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:fca2fd850887e25f32545c21b0dba9f061d57cfaf3c9c5084fe238ea0d70b09f"
---

# How to Build a Scheduling App Without Code (Replace Calendly in an Afternoon)

## The Blink approach: describe the app, get the stack


[Blink](https://blink.new/) generates the full application from a prompt — React frontend, API routes, database schema, and auth. The database is included automatically, no Supabase account needed. Auth is built in, no Clerk setup required. Hosting is included, no Vercel config to touch.


You describe what you want. Blink builds it. You own the result.


The entire stack runs in one place, on one bill.


## Step-by-step: build a scheduling app with Blink


This walkthrough takes a scheduling app from zero to live. Each step includes the actual prompt to paste into Blink's chat.


**Step 1: Describe the core booking flow**


Go to[blink.new](https://blink.new/) and start a new project. Paste this prompt:


> *"Build a scheduling app where businesses can set their availability and clients can book appointments. Include a booking calendar that shows available time slots, a booking form collecting name, email, and reason for the meeting, and email confirmations sent to both parties after a booking is made."*


Blink generates the database schema (users, appointments, time_slots, availability tables), the booking calendar component, the booking form, and the email notification logic. No configuration needed — the database is auto-created and wired up.


**Step 2: Add availability management**


Once the MVP renders, prompt Blink to add admin controls:


> *"Add an admin dashboard where the business owner can set weekly availability — working hours per day, days off, and buffer time between appointments. The booking calendar should only show slots that fit within these rules."*


Blink adds the availability editor UI, updates the database schema to store availability rules, and adds the filtering logic so only open slots appear to bookers.


**Step 3: Add timezone support**


> *"Update the booking calendar to detect the visitor's timezone automatically and display all times in their local time. Store all appointments in UTC in the database."*


This single prompt handles the most painful part of any scheduling system. With Blink, the database schema update happens automatically — no migration scripts to write by hand.


**Step 4: Add payment collection**


> *"Add a payment step to the booking flow using Stripe. After the booker fills in their details, show a payment form to collect a $25 deposit before the booking is confirmed."*


Blink wires in the Stripe integration, adds the payment step between the booking form and confirmation screen, and only marks a booking as confirmed after payment succeeds.


**Step 5: Deploy**


Click Deploy in Blink. Your scheduling app goes live on a public URL — hosting included, no Vercel account needed. Share the link and start taking bookings.


The whole process takes an afternoon. Not a weekend, not a sprint.


Custom scheduling app vs Calendly: own your tool instead of paying per seat, forever


Blink


## What to add after the MVP


The five-step build above gives you a working scheduling app. Here's what to add next, based on what real scheduling tools charge extra for:


- **Recurring availability templates** — set "Tuesday/Thursday 9am–5pm" once, apply forever
- **Multiple services** — "30-min consult" vs "60-min deep dive" with different durations and prices
- **Google Calendar sync** — write confirmed bookings to Google Calendar, block off busy times
- **Cancellation and rescheduling** — let bookers cancel with a link in the confirmation email, within a policy window
- **Team scheduling** — assign bookings to specific team members based on their availability
- **Intake forms** — collect custom fields per service type (intake questions, file uploads)
- **No-show protection** — charge a cancellation fee if the booker cancels within 24 hours


Each of these is a Blink prompt away. Describe the feature, get the code, deploy. No DevOps, no separate service accounts.


## Cost comparison


Calendly Acuity Scheduling Build with Blink


**Monthly cost (10 users)** $100–$200/month $49/month (flat) Free to start


**Per-seat pricing** Yes — $10–20/seat No No


**Payments included** Professional+ only All plans You wire Stripe once


**Custom branding** Paid plans only Paid plans only Full control


**You own the code** No No Yes


**Custom features** Not possible Not possible Add any feature via prompt


**Annual cost (10 users)** $1,200–$2,400/year $588/year Free to start


Calendly and Acuity are services you rent. Blink gives you an app you own. There's no per-seat fee because it's your database, your hosting, your code.


The math is straightforward: a 10-person team at Calendly's $20/seat Professional plan pays $2,400 a year. That's the recurring cost of not building it once.


A full scheduling app — calendar, admin panel, payments, and notifications — ready in an afternoon with Blink


Blink


## Frequently asked questions


No. Blink includes the database automatically — no Supabase account, no PostgreSQL setup, no connection strings to manage. The database schema is generated from your prompts and wired up without any configuration. You describe the features; Blink handles the data layer.


Auth is built in. No Clerk account, no Firebase Auth setup, no JWT configuration. When you prompt Blink to add a login page or an admin dashboard, authentication is included automatically. Bookers don't need to log in — the admin panel is protected behind auth that Blink configures for you.


Yes. Prompt Blink to "sync confirmed bookings to Google Calendar" and it generates the Google Calendar API integration. You connect your Google account via OAuth, and every booking writes to your calendar automatically. You can also block off busy times to prevent double-booking.


Calendly and Acuity are SaaS tools you subscribe to. You don't own them, can't modify them, and pay forever. With Blink, you build an app you own — the code, the database, the hosting. You can add any feature, connect any service, and change any behavior. You're not paying per seat or waiting for a product roadmap to catch up.


Yes. Blink isn't a one-time export. Your app lives in Blink with its database and hosting included. Keep prompting to add features — more service types, team scheduling, intake forms, SMS reminders. Every update deploys in seconds. No DevOps, no staging environments to manage.


---


The scheduling app problem is solved. The only question is whether you keep renting it at $100/month or build it once.


Build this with Blink — database, auth, and hosting included. No config needed →[blink.new](https://blink.new/)
