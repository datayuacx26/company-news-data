---
schema_version: "1.0.0"
document_id: "46ef44a566dd2a9734335931cb0def3c052484d18f4cc788debc5a2c5e657034"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/audience-blast-two-lines"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T09:08:45.431702+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:5da1608bbc4779cea7a5e53fdd82f57c1c59f0920d2c1cfabddf1ebddf72a0d4"
---

# Add an Audience and Send a Blast in Two Lines of Code

Most messaging platforms make bulk sends complicated. You upload a CSV, wait for validation, configure a campaign, set a schedule, click through three confirmation screens, and hope nothing fails silently.


Pinnacle has two API calls.


## The Two Lines


TypeScript


```text
const   {   id   }   =   await   client  .  audiences  .  create  ({
name  :   "  Spring Launch  "  ,
contacts  :   [  "  +14155551234  "  ,   "  +16505550101  "  ,   "  +12125559876  "  ],
});


await   client  .  messages  .  blast  .  rcs  ({   audienceId  :   id  ,   senders  :   [  "  your_agent_id  "  ],   message  :   {   text  :   "  Our spring collection is live.  "   }   });
```


That's it. Audience created, blast sent.


## Breaking It Down


### Creating the Audience


TypeScript


```text
const   {   id   }   =   await   client  .  audiences  .  create  ({
name  :   "  Spring Launch  "  ,
});
```


` contacts` takes an array of E.164 phone numbers. The audience persists in your account — you can reuse it for future campaigns, append to it, or remove contacts from it later. Full reference:[Create Audience](https://docs.pinnacle.sh/api-reference/audiences/create) .


To append to an existing audience:


TypeScript


```text
await   client  .  audiences  .  contacts  .  add  ({
id  ,
contacts  :   [  "  +17185554321  "  ],
});
```


### Sending the Blast


TypeScript


```text
await   client  .  messages  .  blast  .  rcs  ({
audienceId  :   id  ,
senders  :   [  "  your_agent_id  "  ],
message  :   {   text  :   "  Our spring collection is live.  "   },
});
```


The` senders` array specifies which RCS agent(s) to send from. Pinnacle distributes messages across senders automatically for throughput. Every contact in the audience receives the message. Full reference:[Blast RCS](https://docs.pinnacle.sh/api-reference/messages/blast-rcs) ·[Blast SMS](https://docs.pinnacle.sh/api-reference/messages/blast-sms) ·[Blast MMS](https://docs.pinnacle.sh/api-reference/messages/blast-mms) .


## Add Rich Content


The blast endpoint supports the same rich payload as a single RCS send — cards, quick replies, and carousels.


TypeScript


```text
await   client  .  messages  .  blast  .  rcs  ({
audienceId  :   id  ,
senders  :   [  "  your_agent_id  "  ],
message  :   {
cards  :   [
{
title  :   "  Spring Collection 2026  "  ,
subtitle  :   "  New arrivals — shop the drop before it's gone.  "  ,
media  :   "  https://cdn.example.com/spring-drop.jpg  "  ,
buttons  :   [
{   title  :   "  Shop Now  "  ,   type  :   "  openUrl  "  ,   payload  :   "  https://shop.example.com/spring  "   },
{   title  :   "  Save for Later  "  ,   type  :   "  trigger  "  ,   payload  :   "  save_spring  "   },
],
},
],
},
});
```


## SMS and MMS Blasts


The same pattern works for SMS and MMS:


TypeScript


```text
// SMS blast
await   client  .  messages  .  blast  .  sms  ({
audienceId  :   id  ,
senders  :   [  "  +12015550100  "  ],
message  :   {   text  :   "  Our spring collection is live: https://shop.example.com/spring  "   },
});


// MMS blast
await   client  .  messages  .  blast  .  mms  ({
audienceId  :   id  ,
senders  :   [  "  +12015550100  "  ],
message  :   {
text  :   "  Spring drop is here.  "  ,
mediaUrls  :   [  "  https://cdn.example.com/spring-drop.jpg  "  ],
},
});
```


Pinnacle applies automatic channel fallback at the per-recipient level — if a contact's device doesn't support RCS, they get SMS.


## Schedule a Blast


Pass` options.schedule.sendAt` to send at a future time:


TypeScript


```text
await   client  .  messages  .  blast  .  rcs  ({
audienceId  :   id  ,
senders  :   [  "  your_agent_id  "  ],
message  :   {   text  :   "  Flash sale ends in 2 hours.  "   },
options  :   {   schedule  :   {   sendAt  :   "  2026-05-01T09:00:00Z  "  ,   timezone  :   "  UTC  "   }   },
});
```


Scheduled blasts appear in the[dashboard](https://app.pinnacle.sh/dashboard) under **Blasts → Scheduled** and can be cancelled before they fire via the[cancelScheduledMessage](https://docs.pinnacle.sh/api-reference/messages/cancel-scheduled-message) endpoint or the` cancel_scheduled_message` MCP tool.


## Track Delivery


Every blast is tracked at the message level. The Pinnacle[analytics dashboard](https://app.pinnacle.sh/dashboard/analytics) shows:


- Total sent / delivered / failed per blast
- Per-recipient delivery status
- Button tap counts and payload breakdown
- Cost breakdown by channel


Delivery events also post to your[configured webhook](https://app.pinnacle.sh/dashboard/development/webhooks) as they happen, so you can update your CRM or trigger follow-up flows in real time.


## Python and Ruby


Python


```text
# Python
from   rcs   import   Pinnacle  ,   RichText


audience   =   client  .  audiences  .  create  (
name  =  "  Spring Launch  "  ,
contacts  =  [  "  +14155551234  "  ,   "  +16505550101  "  ],
)


client  .  messages  .  blast  .  rcs  (
audience_id  =  audience  .  id  ,
senders  =  [  "  your_agent_id  "  ],
message  =  RichText  (  text  =  "  Our spring collection is live.  "  ),
)
```


Ruby


```text
# Ruby
audience   =   client  .  audiences  .  create  (
name  :   "  Spring Launch  "  ,
contacts  :   [  "  +14155551234  "  ,   "  +16505550101  "  ]
)


client  .  messages  .  blast  .  rcs  (
audience_id  :   audience  .  id  ,
senders  :   [  "  your_agent_id  "  ],
message  :   {   text  :   "  Our spring collection is live.  "   }
)
```


## Key Takeaways


- Create an audience with` client.audiences.create` and pass an array of phone numbers.
- Send to the entire audience in one call with` client.messages.blast.rcs` ,` .sms` , or` .mms` .
- Rich content (cards, quick replies, carousels) is supported on RCS blasts.
- Schedule future blasts with` options.schedule.sendAt` .
- Delivery stats and button tap events are available in the dashboard and via webhooks.


## FAQ


**1. Is there a limit on audience size?** Pinnacle handles large audiences with automatic throttling and multi-sender distribution. Contactfounders@pinnacle.sh for enterprise volume requirements.


**2. Can I send personalized messages to each contact in the blast?** Individual personalization (per-contact dynamic fields) is available via the batch send API. Contact the team for details on per-recipient variable substitution.


**3. How does fallback work for blasts?** If RCS is unavailable for a specific contact, Pinnacle falls back to SMS automatically. The fallback behavior is configurable per blast.


**4. Can I cancel a scheduled blast?** Yes — use the` cancelScheduledMessage` endpoint before the scheduled send time, or cancel directly from the[dashboard](https://app.pinnacle.sh/dashboard) .


**5. Where do button tap events go?** Button taps from blast messages are delivered to your configured webhook exactly like single-message interactions — with the button payload, message ID, and recipient phone number.


[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+audience+blasts) with the Pinnacle team — we'll walk through your audience setup and campaign strategy and get you live fast.
