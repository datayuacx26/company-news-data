---
schema_version: "1.0.0"
document_id: "a3f732c9418cc088e117f6949b921a0f8a276d5b87f16d8514049aea80098b52"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/send-sms-mms-rcs-with-one-sdk"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T09:08:45.431702+00:00"
fetched_at: "2026-07-28T22:15:36.111958+00:00"
content_hash: "sha256:ef3c3963f17fdb592a3bf6ee2e85852aa7c3fa6254149b1cae1a7ecc5f1edde2"
---

# Send SMS, MMS, and RCS with One SDK

You signed up for Pinnacle to send rich messages. Your backend might be TypeScript, Python, or Ruby. Either way, the SDK is the same idea: one package, one API key, all four channels. Here's the shortest path from zero to sending.


Choose your language


## Install


Bash


```text
npm   install   rcs-js
```


Package on[npm](https://www.npmjs.com/package/rcs-js) .


## Initialize the Client


TypeScript


```text
import   {   PinnacleClient   }   from   "  rcs-js  "  ;


const   client   =   new   PinnacleClient  ({   apiKey  :   "  pnclk_...  "   });
```


Your API key lives in the[Pinnacle dashboard](https://app.pinnacle.sh/) under **Settings → API Keys** . Keep it server-side — never expose it in client bundles.


## Send an SMS


Plain text. 160 characters per segment, delivered to every phone on the planet.


TypeScript


```text
await   client  .  messages  .  sms  .  send  ({
from  :   "  +12015550100  "  ,
to  :   "  +14155551234  "  ,
text  :   "  Your order #4821 has shipped.  "  ,
});
```


The` from` number must be a Pinnacle-provisioned phone number. You can buy one from the[dashboard](https://app.pinnacle.sh/dashboard/numbers?current=purchase) or via the API. Full reference:[Send SMS](https://docs.pinnacle.sh/api-reference/messages/send-sms) .


## Send an MMS


Add a media URL to attach images, GIFs, short videos, or vCards.


TypeScript


```text
await   client  .  messages  .  mms  .  send  ({
from  :   "  +12015550100  "  ,
to  :   "  +14155551234  "  ,
text  :   "  Here's your receipt.  "  ,
mediaUrls  :   [  "  https://cdn.example.com/receipts/4821.pdf  "  ],
});
```


MMS supports up to 4.5 MB per message and up to 10 files. Accepted formats include JPEG, PNG, GIF, MP4, PDF, and vCard. Full reference:[Send MMS](https://docs.pinnacle.sh/api-reference/messages/send-mms) .


## Send an RCS Message


RCS is where it gets interesting. You can send a plain text RCS message, attach rich cards, or add quick replies — all from the same send call.


### Text + Quick Replies


TypeScript


```text
await   client  .  messages  .  rcs  .  send  ({
from  :   "  your_rcs_agent_id  "  ,
to  :   "  +14155551234  "  ,
text  :   "  How was your delivery experience?  "  ,
quickReplies  :   [
{   type  :   "  trigger  "  ,   title  :   "  Great 👍  "  ,   payload  :   "  rating_good  "   },
{   type  :   "  trigger  "  ,   title  :   "  Could be better  "  ,   payload  :   "  rating_bad  "   },
],
});
```


### Rich Card


A rich card combines an image, title, subtitle, and action buttons in a single interactive tile.


TypeScript


```text
await   client  .  messages  .  rcs  .  send  ({
from  :   "  your_rcs_agent_id  "  ,
to  :   "  +14155551234  "  ,
cards  :   [
{
title  :   "  Winter Jacket — $129  "  ,
subtitle  :   "  Waterproof, rated to -20°C. Ships in 2 days.  "  ,
media  :   "  https://cdn.example.com/products/jacket.jpg  "  ,
buttons  :   [
{
title  :   "  Buy Now  "  ,
type  :   "  openUrl  "  ,
payload  :   "  https://shop.example.com/jacket  "  ,
},
{   title  :   "  Save for Later  "  ,   type  :   "  trigger  "  ,   payload  :   "  save_jacket  "   },
],
},
],
});
```


The` from` field for RCS is your RCS agent ID (not a phone number). You get this when your RCS agent is approved — Pinnacle handles the approval process for you. Full reference:[Send RCS](https://docs.pinnacle.sh/api-reference/messages/send-rcs) .


## Automatic Channel Fallback


Not every recipient supports RCS. Pinnacle gives you two ways to handle this.


### Option 1: the` fallback` field


Pass a` fallback` object directly in your RCS send call. If the recipient's device doesn't support RCS, Pinnacle automatically delivers the fallback as SMS or MMS — no extra logic on your end.


TypeScript


```text
await   client  .  messages  .  rcs  .  send  ({
from  :   "  your_rcs_agent_id  "  ,
to  :   "  +14155551234  "  ,
text  :   "  Your order #4821 has shipped.  "  ,
fallback  :   {
from  :   "  +12015550100  "  ,
text  :   "  Your order #4821 has shipped.  "  ,
},
});
```


The` fallback.from` must be a Pinnacle-provisioned phone number with SMS/MMS capabilities. You're only charged for whichever message is actually delivered.


### Option 2: check capabilities first


If you want to branch your logic — for example, send a rich card to RCS users and a plain SMS to everyone else — call` getCapabilities` before sending:


TypeScript


```text
const   capabilities   =   await   client  .  rcs  .  getCapabilities  ({
phoneNumbers  :   [  "  +14155551234  "  ],
});
// null means RCS not supported
if   (  capabilities  [  "  +14155551234  "  ])   {
await   client  .  messages  .  rcs  .  send  ({
from  :   "  your_rcs_agent_id  "  ,
to  :   "  +14155551234  "  ,
text  :   "  ...  "  ,
});
}   else   {
await   client  .  messages  .  sms  .  send  ({
from  :   "  +12015550100  "  ,
to  :   "  +14155551234  "  ,
text  :   "  ...  "  ,
});
}
```


For most use cases the` fallback` field is simpler — reach for` getCapabilities` when you need per-device branching logic.


## Key Takeaways


- Install one package for your language and you get SMS, MMS, and RCS from the same client.
- Python uses` from_` and Ruby uses` send_` as thin workarounds for reserved keywords — everything else is symmetric.
- RCS` from` is an agent ID, not a phone number; SMS/MMS` from` is a Pinnacle-provisioned E.164 number.
- Call` getCapabilities` before sending RCS, or rely on Pinnacle's automatic fallback chain.


## FAQ


**1. Where do I get an RCS agent ID?** After onboarding, your agent ID appears in the[dashboard](https://app.pinnacle.sh/dashboard) under **Agents** . Pinnacle registers your RCS agent with carriers on your behalf — typically 1–2 weeks for US carrier approval.


**2. Can I use the same phone number for SMS, MMS, and RCS?** SMS and MMS share a phone number. RCS uses a separate agent identity. They're linked in the Pinnacle dashboard.


**3. What happens if a message fails?** Every send returns a message ID. Check delivery status via webhook events (` message.delivered` ,` message.failed` ) or the[Messages API](https://docs.pinnacle.sh/api-reference/messages/get) .


**4. Is there a sandbox?** Yes. The Pinnacle[sandbox](https://app.pinnacle.sh/dashboard/development/sandbox-numbers) lets you send to whitelisted test numbers immediately, no carrier approvals required. Switch to production by swapping your API key.


**5. Do the SDKs support async?** TypeScript is fully async. The Python SDK supports both sync and async via` AsyncPinnacle` . Ruby is synchronous by default.


[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+the+Pinnacle+SDK) with the Pinnacle team — we'll walk through your use case, pick the right channels, and get you live fast.
