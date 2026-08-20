---
schema_version: "1.0.0"
document_id: "8d6d61dff0e0b8823d105cf3f4e38e5be961bc2a0591277c5e47f81e87eb43e0"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-appointment-booking-app"
published_at: "2026-05-27T12:49:37+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:1876af468312ecac913cdd0ecd7c0800822adee0934553062f92c2f27209a011"
---

# How to Build an Appointment Booking App (Like Calendly) With AI

## The Infrastructure a Booking App Needs


A booking system has six technical layers: a database to store appointments, auth to protect the admin view, a backend to handle scheduling logic, email for confirmations, hosting for the frontend, and a public booking page.


Building this manually means wiring together at least four separate services — and then paying for all of them monthly:


Manual Stack Blink


Database Supabase ($25/mo) Included


Auth Clerk ($25/mo) Included


Backend API Vercel Functions ($20/mo) Included


Email Resend ($20/mo) Included


Hosting Vercel ($20/mo) Included


**Monthly cost** **$70–90/mo** **$0 (free tier)**


**Setup time** 8–16 hours Under 2 hours


Blink handles all five layers. You describe what you want; it builds and deploys the app. No config, no DevOps.


## Building the App, Step by Step


1


#### Define your booking types


Start by describing your appointment structure:


> "Build me an appointment booking app. I need two booking types: a 30-minute intro call and a 60-minute consulting session. Each type should have a title, duration, price (optional), and description."


Blink creates a` booking_types` table with those fields and a settings panel where the admin can add, edit, or disable booking types without touching any code. Start with two types — you can always add more later.


2


#### Set up the availability system


Tell Blink how your scheduling rules work:


> "Add an availability system. The host sets working hours for each day of the week — for example, Monday through Friday, 9am to 5pm. Add the ability to block specific dates. Add a 15-minute buffer between bookings so there's no back-to-back stacking."


Blink generates an` availability_rules` table and the backend logic to compute open time slots based on your rules, existing bookings, and buffer times. All times are stored in UTC and converted to the guest's local time zone on the booking page.


3


#### Build the public booking page


Describe the guest-facing interface:


> "Create a public booking page at /book. The guest picks a booking type, then sees available time slots in a calendar view. After picking a slot, they fill in their name, email, and an optional message. Show a confirmation screen after they book."


Blink builds the calendar UI, the slot-picker, and the booking form. No login required for guests. Each booking is saved to a` bookings` table with fields for guest info, selected slot, booking type, and status. Default status is` confirmed` — you can add an approval step if your workflow needs one.


4


#### Add automated email confirmations


Tell Blink what to send and when:


> "When a booking is confirmed, send a confirmation email to both the host and the guest. Include the booking date and time, a link to add it to their calendar, and unique one-click links for rescheduling and cancellation."


Blink adds transactional email logic using Resend. The confirmation includes an` .ics` calendar attachment compatible with Google Calendar, Outlook, and Apple Calendar. Cancellation and rescheduling links are signed URLs — guests click once, no account needed.


If you want reminder emails 24 hours before, add: "Also send a reminder email to the guest 24 hours before their booking."


5


#### Build the admin dashboard


Add the management layer:


> "Build a password-protected admin dashboard at /admin. Show upcoming bookings sorted by date. Let the admin cancel or reschedule any booking. Include a weekly calendar view. Protect the route with email and password login."


Blink adds auth to the` /admin` route using its built-in auth system — no Clerk setup, no Firebase config. The dashboard shows the booking queue, a calendar view, and action buttons per booking. Only authenticated users can access it; the public booking page stays open.


6


#### Deploy and share your booking link


Your app is already live. Blink deploys to a public URL from the first prompt — no build pipeline, no deploy config, no server to provision.


Go to your Blink dashboard, set a custom domain if you want one, and copy your booking link. Share it in your email signature, your website, your LinkedIn profile, wherever.


Done.


## Add-On Features


The core app is live. Here's what to layer on next, ordered by how often teams add them:


**Stripe payment collection**


For paid services — coaching calls, consulting hours, tutoring sessions — tell Blink:


> "Add Stripe Checkout to the booking flow. Require payment before the slot is confirmed. If the payment fails or is abandoned, release the slot."


Blink integrates Stripe and holds the time slot during checkout. The booking confirms only on successful payment. For recurring billing patterns — like a monthly coaching retainer — the[Stripe subscription guide](https://blink.new/blog/how-to-build-stripe-subscription-app) covers how to add subscription payments to a Blink app.


**Google Calendar sync**


> "When a booking is confirmed, create a Google Calendar event on the host's calendar. When a booking is cancelled, delete it."


This requires a Google OAuth app (free to create in Google Cloud Console). Blink walks through the setup. Once connected, the host's Google Calendar stays in sync automatically — no manual entry.


**SMS reminders**


> "Send an SMS reminder to the guest 24 hours before their booking using Twilio."


You'll need a Twilio account and a phone number ($1/month for the number). Blink handles the reminder scheduling — you provide the credentials in the app's environment settings.


**Round-robin team scheduling**


If multiple people share a booking pool:


> "Add a round-robin booking type. When a guest books, assign them to whichever team member has the next available slot. Send the assigned team member a confirmation email."


Blink creates a` team_members` table, links members to the booking type, and distributes new bookings evenly. The assigned team member gets the confirmation; the guest gets a confirmation with that person's name.


A custom appointment booking app with availability management and automated confirmations


Blink


For a deeper look at the full build process — including how to structure the data model and handle edge cases like overlapping bookings —[how to build a scheduling app](https://blink.new/blog/how-to-build-a-scheduling-app) goes into more detail on the backend logic.


## Frequently Asked Questions


The core app — availability rules, public booking page, email confirmations, and admin dashboard — takes 2 to 4 hours with Blink. Each feature is added by describing it in plain language. Adding Stripe payments takes another 30 to 45 minutes. Google Calendar sync takes about 20 minutes once you have a Google OAuth app set up.


Yes. You need a Google OAuth client ID and secret from Google Cloud Console (free). Once you add those to your Blink app's environment settings, Blink can create and delete calendar events automatically when bookings are confirmed or cancelled. The setup takes about 20 minutes. iCal-compatible calendar invites (.ics files) are also included in confirmation emails by default, which covers Apple Calendar and Outlook without any extra setup.


No. Blink is a full-stack AI app builder — you describe what you want in plain language and it writes the code, creates the database schema, and deploys the app. You don't write SQL, configure a server, or use a terminal. If you want to customize the app beyond what's in the chat, Blink lets you edit the generated code directly — useful if you have a developer on the team who wants to extend it.


Blink stores all booking times in UTC in the database. The public booking page detects the guest's browser time zone using the Intl API and displays available slots in local time. Confirmation emails show the booking time in both the guest's time zone and the host's time zone. This prevents the most common international scheduling mistake — both parties showing up an hour apart because someone forgot daylight saving time.


Yes. Blink integrates Stripe Checkout into the booking flow. The time slot is temporarily held when the guest starts checkout and confirmed only after successful payment. Failed or abandoned payments release the slot back to the available pool. You can also set minimum booking notice (e.g. no same-day bookings) and cancellation policies (e.g. no refunds within 24 hours) as additional rules in the app.


Three things, in order of impact: calendar add links in confirmation emails (reduces forgotten appointments significantly), SMS reminders 24 hours before (Twilio integration, about 30 minutes to add), and upfront payment. Guests who paid almost never ghost. If you're booking free calls, the calendar link plus a 24-hour reminder covers most cases. Require payment only when the no-show cost is high enough to justify adding friction to the booking flow.


Yes. Blink deploys your app to a public URL, which you can embed as an iframe on any website — WordPress, Webflow, Framer, plain HTML, anywhere. The iframe approach takes about 5 minutes. If you want deeper integration (e.g. showing available slots inline without an iframe), Blink exposes an API that your existing frontend can query directly to pull slots and submit bookings programmatically.
