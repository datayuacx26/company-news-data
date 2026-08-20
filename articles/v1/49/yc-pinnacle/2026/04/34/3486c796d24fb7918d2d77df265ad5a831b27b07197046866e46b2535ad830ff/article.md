---
schema_version: "1.0.0"
document_id: "3486c796d24fb7918d2d77df265ad5a831b27b07197046866e46b2535ad830ff"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/rcs-for-healthcare"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T09:08:45.431702+00:00"
fetched_at: "2026-07-28T22:15:36.111958+00:00"
content_hash: "sha256:34e2d1169c9bdbd5cf2121c63eaee214fb581ef6705003169d95a6572a70752c"
---

# RCS for Healthcare: Improve Patient Engagement Without Building an App

Healthcare has an engagement problem. You can build the best patient portal on the market, and 60% of your patients won't log in. You can send a reminder email, and it'll land in a tab nobody checks. You can call — and get voicemail.


Text messages have a 98% open rate. Most patients read them within minutes of delivery. The problem is that SMS is a blunt instrument: 160 characters of plain text, a bare link, no context, no interaction.


RCS changes the calculus. Rich Communication Services brings the interactive capability of a native app to the default messaging application already on your patient's phone. No download prompt. No login screen. Just a message they can act on in one tap.


## What Healthcare Gains from RCS


### Appointment Reminders That Reduce No-Shows


The average no-show rate in healthcare runs between 10–30%, depending on specialty. Each missed appointment costs time, revenue, and continuity of care.


An SMS reminder says: "Appointment tomorrow at 2pm. Reply C to confirm or R to reschedule."


An RCS reminder says the same thing — but with your practice name and logo in the thread header, a card showing the provider name and location, and two buttons: **Confirm** and **Reschedule** . No typing. One tap. Pinnacle logs the response as a button-tap event your system can act on immediately.


TypeScript


```text
await   client  .  messages  .  rcs  .  send  ({
from  :   "  your_agent_id  "  ,
to  :   patient  .  phone  ,
text  :   "  You have an appointment tomorrow.  "  ,
cards  :   [
{
title  :   `  Dr.   ${  provider  .  name  }  `  ,
subtitle  :   `  ${  format  (  appointment  .  time  ,   "  EEE, MMM d  "  )  }   at   ${  format  (  appointment  .  time  ,   "  h:mm a  "  )  }   —   ${  clinic  .  name  }  `  ,
media  :   clinic  .  logoUrl  ,
buttons  :   [
{
title  :   "  Confirm  "  ,
type  :   "  trigger  "  ,
payload  :   `  confirm_  ${  appointment  .  id  }  `  ,
},
{
title  :   "  Reschedule  "  ,
type  :   "  openUrl  "  ,
payload  :   `  https://portal.example.com/reschedule/  ${  appointment  .  id  }  `  ,
},
],
},
],
});
```


### Medication Adherence


Patients on long-term medication regimens have a documented adherence problem — roughly 50% of patients with chronic conditions don't take medications as prescribed ([WHO data](https://iris.who.int/handle/10665/42682) ). A daily or weekly RCS reminder reduces cognitive friction: instead of a text link to a portal page, the patient gets a refill card with a **Request Refill** button that fires a webhook directly into your pharmacy management system.


### Post-Discharge Instructions


After a procedure or hospital stay, patients often leave with a paper sheaf of instructions they lose before they reach the car. A structured RCS message sent 30 minutes post-discharge changes this:


- Care instructions formatted as a readable card
- Warning signs listed as a quick-scan section
- A **Call My Care Team** button that dials the on-call nurse directly
- A follow-up appointment booking link


### Lab Results and Clinical Updates


Lab results can come back as a branded card with a one-line summary and buttons to either view the full report in the patient portal or send a question to their provider. No PII in the message body — just enough context to prompt the right action.


### Care Coordination Between Providers


Outbound messaging doesn't have to be patient-facing. Care coordinators can use Pinnacle to send RCS messages to referring providers — a referral card with patient context, the requested service, and a callback button. Cleaner than fax. Faster than email.


## On Privacy


RCS messages are delivered over carrier infrastructure and are subject to the same security posture as SMS. For use cases involving PHI, evaluate your message design carefully: keep protected health information out of the message body.


RCS supports **in-thread webviews** — a browser window that opens inside the messaging app without ever leaving the thread. This is the right pattern for sensitive content: lab results, prescription details, and care summaries load in an authenticated webview, so patients see their information securely without being handed off to an external browser or forced to log in to a portal. Pinnacle encrypts data in transit (TLS 1.2+) and at rest. See the[trust center](https://trust.pinnacle.sh/) for the full security posture.


If your organization has a strict HIPAA BAA requirement,[contact us](https://www.pinnacle.sh/contact?message=We%27re%20a%20healthcare%20organization%20interested%20in%20RCS%20messaging%20and%20have%20HIPAA%20BAA%20requirements.) — we work with healthcare customers on a case-by-case basis.


## Book a Call


Most of your competitors are still sending plain SMS. RCS adoption in healthcare is early — the teams moving now are the ones that will own the patient experience before it becomes table stakes.


We've worked with healthcare organizations of all sizes, from early-stage digital health startups to established provider networks.[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+RCS+for+healthcare) and we'll walk through your patient engagement flows, handle RCS agent registration, and get you live fast.


## Key Takeaways


- RCS replaces plain SMS reminders with interactive cards: branded sender, visual context, and tap-to-respond buttons — all in the default messaging app.
- The highest-value healthcare flows for RCS are appointment confirmations, medication reminders, and post-discharge instructions.
- Button taps post back to your webhook in real time, enabling automated confirmation, escalation, and follow-up.
- For sensitive use cases, keep PHI out of message bodies — link to authenticated portals instead.
- Pinnacle handles SMS fallback automatically, so every patient gets the message regardless of device.


## FAQ


**1. How do button taps integrate with our EHR or scheduling system?** Pinnacle pushes every button tap to a webhook you configure. The payload includes the value you set on the button (e.g.,` confirm_appt_123` ). Your webhook handler can then call your scheduling API to update the appointment status.


**2. Can we send appointment reminders in bulk?** Yes. Create an[audience](https://app.pinnacle.sh/dashboard/audiences) with your appointment list for the day and use` blast_rcs` to send all reminders in one call. Pinnacle handles per-message personalization via dynamic fields.


**3. How long does RCS agent registration take for a healthcare organization?** Typically 1–2 weeks. Pinnacle manages the submission and communicates status — you don't interface with carriers directly.
