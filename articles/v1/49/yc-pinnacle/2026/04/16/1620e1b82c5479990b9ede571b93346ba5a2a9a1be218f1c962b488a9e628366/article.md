---
schema_version: "1.0.0"
document_id: "1620e1b82c5479990b9ede571b93346ba5a2a9a1be218f1c962b488a9e628366"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/slothwise-case-study"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T09:08:45.431702+00:00"
fetched_at: "2026-07-28T22:15:36.111958+00:00"
content_hash: "sha256:5c9b90287b4e5f0ca1af66491cfe124bad5cc3d5b6979e621b65f787ffe2d8ea"
---

# How Slothwise Built a Health Platform That Lives in Your Texts

Most health apps have the same problem: nobody opens them.


You download one after a doctor's visit, enter your medications, maybe log a meal, and then open it once more three weeks later before deleting it. The app required too much of a behavior change. It wasn't where you already were.


[Slothwise](https://www.slothwise.com/) built from the opposite assumption: the interface that people already use — their text messaging app — should be the product.


## What Slothwise Built


Slothwise is a personal health management platform that consolidates your medical records, tracks your health metrics, and answers your health questions. The interface is a phone number.


Text what you ate. Text a photo of a label for nutritional analysis. Text "how are my labs looking?" and get a response grounded in your actual health history. Everything from appointment prep to insurance claim analysis — handled through a back-and-forth with a number your patients already have saved.


The platform also syncs with wearable devices and maintains a unified health record over time. But the access point is always the same: a text message.


## The Messaging Problem


Building on SMS has real limits. Plain text can't show a structured lab result with context. It can't attach a clickable button to book a follow-up visit. It can't visually distinguish a nutrition summary from a medication alert. And critically, it can't carry your brand — an SMS from a health platform looks identical to an SMS from a delivery service.


The Slothwise team needed rich media, interactive elements, and a sender identity that patients would recognize and trust. RCS delivered all three.


## How Pinnacle Powers the Platform


Slothwise integrated Pinnacle's messaging API to power the two-way conversation at the core of their product.


**Inbound** : Patients text their dedicated number. Pinnacle receives the inbound message via webhook and routes it to the Slothwise AI layer for processing. The platform analyzes the message against the patient's health history, generates a response, and sends it back — all within seconds.


**Outbound** : Responses arrive as structured RCS messages when the patient's device supports it. A nutrition summary might come as a card with a breakdown table and a quick reply to log it. A lab result update arrives as a rich card with a **View Full Report** button and a **Message My Doctor** reply option.


For patients on devices that don't support RCS, Pinnacle's automatic fallback delivers the same information as SMS — no special handling required on Slothwise's end. The platform sends once, Pinnacle handles channel selection.


**Verification** : The Slothwise sender identity — name, icon, verified badge — appears in the RCS thread header. Patients aren't guessing whether a health nudge at 9am is legitimate or spam. The brand is explicit in the interface.


## The Technical Setup


Slothwise uses the Pinnacle TypeScript SDK:


TypeScript


```text
import   {   PinnacleClient   }   from   "  rcs-js  "  ;


const   client   =   new   PinnacleClient  ({   apiKey  :   process  .  env  .  PINNACLE_API_KEY  !   });
```


Inbound messages post to a webhook. Slothwise processes the message, queries the health record, generates a response, and sends it back in the same handler:


TypeScript


```text
// Webhook handler (simplified)
app  .  post  (  "  /pinnacle/inbound  "  ,   async   (  req  ,   res  )   =>   {
const   {   from  ,   text  ,   mediaUrls   }   =   req  .  body  ;


const   response   =   await   healthAI  .  respond  ({
patientPhone  :   from  ,
message  :   text  ,
attachments  :   mediaUrls  ,
});


await   client  .  messages  .  rcs  .  send  ({
from  :   "  slothwise_agent  "  ,
to  :   from  ,
...  response  .  rcsPayload  ,
});


res  .  sendStatus  (  200  );
});
```


For scheduled health check-ins and proactive reminders, Slothwise uses Pinnacle's scheduled send with[audience blasts](https://app.pinnacle.sh/dashboard/audiences) — segmenting patients by their health program and sending personalized reminders at optimal times.


## Why Text Works for Health


The friction argument is the product thesis: every additional step between a health insight and a patient acting on it is a percentage point of engagement lost. SMS and RCS have no login screen. No download prompt. No notification permission dialog.


Most patients check their texts within minutes of delivery. A prompt to log lunch arrives at 12:30pm and gets answered. The same prompt sent as an in-app notification gets dismissed and forgotten.


RCS adds something plain SMS can't: enough visual context to make the response meaningful. A calorie summary card with a quick reply is actionable. A line of plain text is noise.


## What It Means for Healthcare Products


Slothwise demonstrates a design pattern that applies broadly to any health-adjacent product: use the messaging layer as your primary interface, not a notification channel. The platform doesn't send push notifications to drive people back to an app. The messaging app *is* the app.


This changes how you think about architecture. Your backend needs to be conversational — it receives a message, processes context, responds. Pinnacle handles the transport layer: channel selection, fallback, delivery confirmation, and brand identity. You focus on the intelligence layer.


For teams building patient engagement tools, chronic disease management platforms, or health coaching products, the Slothwise approach is worth studying: lowest-friction interface, richest possible content, zero app install required.


## Getting Started


If you're building in the healthcare or health-tech space and want to explore what a text-first product looks like:


- Sign up at[app.pinnacle.sh](https://app.pinnacle.sh/)
- Test your inbound/outbound loop in the[sandbox](https://app.pinnacle.sh/dashboard) — no carrier approvals needed
- Configure your webhook in the[Pinnacle dashboard](https://app.pinnacle.sh/dashboard/development/webhooks) and check the[Pinnacle docs](https://docs.pinnacle.sh/) for the full webhook reference and RCS payload guide
- Reach out atfounders@pinnacle.sh if you have compliance requirements or need a custom arrangement


[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+building+a+text-first+product) with the Pinnacle team — we'd love to help you build something similar.


---


*Slothwise is available at[slothwise.com](https://www.slothwise.com/) starting at $7.99/month.*
