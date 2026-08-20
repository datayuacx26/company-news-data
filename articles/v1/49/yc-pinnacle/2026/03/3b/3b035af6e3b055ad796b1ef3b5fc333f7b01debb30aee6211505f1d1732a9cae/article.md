---
schema_version: "1.0.0"
document_id: "3b035af6e3b055ad796b1ef3b5fc333f7b01debb30aee6211505f1d1732a9cae"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/message-reactions-send-and-receive-emoji-reactions-sms-mms-rcs"
published_at: "2026-03-31T00:00:00+00:00"
first_seen_at: "2026-07-22T09:08:45.431702+00:00"
fetched_at: "2026-07-28T22:16:23.597827+00:00"
content_hash: "sha256:74aa9ac8e836a8761fa9f14586ea529afd719e469bb444b99c47a67c4aa0cf71"
---

# Message Reactions: Send and Receive Emoji Reactions via SMS, MMS, and RCS

## Reactions Shouldn't Be an RCS-Only Feature


Every consumer messaging app has reactions. iMessage, WhatsApp, Telegram — a thumbs up on a message is the universal "acknowledged." It's how people communicate now. Quick, expressive, zero friction.


But in business messaging? Reactions have been locked behind RCS, leaving the vast majority of SMS and MMS conversations without one of the most natural interaction patterns in modern communication.


Pinnacle changes that. We're the only messaging API that supports emoji reactions across **SMS, MMS, and RCS** . One endpoint, any emoji, every channel. React to a customer's message with ✅ to signal resolution. Receive a 👍 from a user confirming their appointment. Track 🔥 reactions on a promotional blast as a real-time engagement signal.


It works the way you'd expect — and on channels where nobody else offers it.


---


## Sending a Reaction


One API call:


TypeScript


```text
import   {   PinnacleClient   }   from   "  rcs-js  "  ;


const   client   =   new   PinnacleClient  ({
apiKey  :   process  .  env  .  PINNACLE_API_KEY  ,
});


await   client  .  messages  .  react  ({
messageId  :   "  msg_abc123  "  ,
reaction  :   "  👍  "  ,
});
```


Pass any Unicode emoji — 👍 ❤️ 😂 😮 😢 ✅ 🔥 🙌 — and it lands in the conversation thread. Works on SMS, MMS, and RCS messages.


To remove a reaction:


TypeScript


```text
await   client  .  messages  .  react  ({
messageId  :   "  msg_abc123  "  ,
reaction  :   null  ,
});
```


---


## Message Tracking: Precision Reactions


When you react to a message, Pinnacle needs to identify which specific message in the thread to attach the reaction to. This is where **message tracking** comes in.


When sending a message, enable tracking to get precise reaction targeting:


TypeScript


```text
await   client  .  messages  .  sms  .  send  ({
to  :   "  +14155551234  "  ,
from  :   "  +18005550001  "  ,
text  :   "  Your order has shipped! Expected delivery: Friday.  "  ,
options  :   {
tracking  :   "  HIDDEN  "  ,
},
});
```


Tracking has two modes:


- **` HIDDEN`** — Invisible identifiers are embedded in the message. Users never see them, but Pinnacle can precisely target the message for reactions. Best for most use cases.
- **` ID`** — An explicit ID is appended to the message. Visible to the user, but useful for debugging or cases where transparency matters.


If you need to react to a message that was sent *without* tracking, use the` force` option:


TypeScript


```text
await   client  .  messages  .  react  ({
messageId  :   "  msg_abc123  "  ,
reaction  :   "  👍  "  ,
options  :   {   force  :   true   },
});
```


The` force` flag bypasses the tracking requirement — though without tracking, the reaction may attach to a nearby message in the thread rather than the exact one you intended. For production flows where precision matters, enable` HIDDEN` tracking on send.


---


## Receiving Reactions From Users


When a user reacts to one of your messages, Pinnacle delivers it via your webhook. Inbound reactions arrive as standard webhook events — the same endpoint that handles text replies:


TypeScript


```text
app  .  post  (  "  /webhooks/pinnacle  "  ,   async   (  req  ,   res  )   =>   {
res  .  status  (  200  ).  send  ();


const   {   event  ,   data   }   =   req  .  body  ;


if   (  event   ===   "  MESSAGE.RECEIVED  "  )   {
const   {   from  ,   content   }   =   data  ;


if   (  content  .  reaction  )   {
console  .  log  (
`  ${  from  }   reacted with   ${  content  .  reaction  .  emoji  }  `   +
`   to message   ${  content  .  reaction  .  messageId  }  `  ,
);
await   handleReaction  (  from  ,   content  .  reaction  );
}   else   if   (  content  .  text  )   {
await   handleReply  (  from  ,   content  .  text  );
}
}
});
```


The` content.reaction` object gives you:


- ` emoji` — the emoji the user reacted with
- ` messageId` — the ID of the message they reacted to


No separate webhook setup. No additional configuration. Reactions flow through the same pipeline as every other inbound message.


---


## What You Can Build With Reactions


### Confirmation Without Conversation


Replace multi-message confirmation flows with a single reaction prompt:


TypeScript


```text
await   client  .  messages  .  sms  .  send  ({
to  :   "  +14155551234  "  ,
from  :   "  +18005550001  "  ,
text  :   "  Your appointment is confirmed for tomorrow at 2pm. React 👍 to confirm or reply to reschedule.  "  ,
options  :   {   tracking  :   "  HIDDEN  "   },
});
```


When the 👍 arrives via webhook, log the confirmation and move on — no back-and-forth needed.


### Real-Time Sentiment Signals


Reactions are lightweight sentiment data. A 🔥 on a promotional message is engagement. A ❤️ on an order confirmation means the experience landed. Track them:


TypeScript


```text
if   (  content  .  reaction  )   {
await   analytics  .  track  (  "  message_reaction  "  ,   {
from  :   data  .  from  ,
emoji  :   content  .  reaction  .  emoji  ,
messageId  :   content  .  reaction  .  messageId  ,
timestamp  :   data  .  timestamp  ,
});
}
```


Aggregate reaction data across campaigns and you have a real-time pulse on what resonates — without surveys, without reply parsing, without NLP.


### Support Closure


A customer sends "thanks, that fixed it!" — react with ✅. It's a human signal that the conversation is complete, without forcing the customer to read another message:


TypeScript


```text
await   client  .  messages  .  react  ({
messageId  :   lastCustomerMessageId  ,
reaction  :   "  ✅  "  ,
});
```


### Reaction-Based Opt-In


"React ❤️ for early access" is a lower bar than "reply YES" — and more expressive. When the reaction arrives, add them to your list:


TypeScript


```text
if   (
content  .  reaction  ?.  emoji   ===   "  ❤️  "   &&
campaignMessages  .  has  (  content  .  reaction  .  messageId  )
)   {
await   addToEarlyAccess  (  data  .  from  );
await   client  .  messages  .  sms  .  send  ({
to  :   data  .  from  ,
from  :   "  +18005550001  "  ,
text  :   "  You're in! We'll reach out when early access opens.  "  ,
});
}
```


---


## Reactions in the Conversations Dashboard


The Pinnacle[conversations dashboard](https://app.pinnacle.sh/) renders reactions inline in the message thread — the same way consumer messaging apps display them. When a customer reacts to one of your messages, the emoji appears directly beneath the message in the thread as it arrives.


Customer reactions appear inline in the conversations dashboard — visible to your team in real time.


---


## Frequently Asked Questions


### 1. Do reactions work on SMS, or just RCS?


Reactions work on **SMS, MMS, and RCS** — Pinnacle is the only messaging API that supports reactions across all three channels. This isn't an RCS-exclusive feature.


### 2. Can I react with any emoji?


Yes. Any standard Unicode emoji works. For best compatibility across devices, stick to commonly supported emoji — 👍 ❤️ 😂 ✅ 🔥 🙌 😮.


### 3. What's the difference between HIDDEN and ID tracking?


` HIDDEN` embeds invisible identifiers in your message — users don't see them, but Pinnacle can precisely target the message for reactions.` ID` appends a visible ID to the message text.` HIDDEN` is recommended for most production use cases.


### 4. What happens if I react without tracking enabled?


You can use` options.force: true` to react without tracking, but the reaction may attach to a nearby message in the thread rather than the exact target. For precision, enable tracking on send.


### 5. Can I receive reactions from users?


Yes. Inbound reactions arrive via the same` MESSAGE.RECEIVED`[webhook event](https://docs.pinnacle.sh/guides/messages/receiving) as text replies. The` content.reaction` object contains the emoji and the ID of the message that was reacted to.


### 6. Is there a rate limit on reactions?


Reactions are standard API calls — the same rate limits that apply to sending messages apply to reactions.


### 7. Can a message have multiple reactions?


Yes. Messages support multiple reactions.


---


## Key Takeaways


- **Cross-channel** : Reactions work on SMS, MMS, and RCS — not just RCS. Pinnacle is the only API that offers this.
- **Send reactions** :` client.messages.react({ messageId, reaction: "👍" })` — any Unicode emoji.
- **Remove reactions** : Pass` reaction: null` to remove.
- **Precision tracking** : Enable` HIDDEN` or` ID` tracking on send for exact reaction targeting. Use` force: true` as a fallback.
- **Receive reactions** : Inbound reactions arrive via` MESSAGE.RECEIVED` webhook with` content.reaction` containing` emoji` and` messageId` .
- **Dashboard support** : Send and view reactions directly from the Pinnacle conversations UI — no code required.


---


## Get Started


The reactions endpoint is available via the[Pinnacle API](https://docs.pinnacle.sh/api-reference/messages/react) and all SDKs ([TypeScript](https://docs.pinnacle.sh/quickstart/rcs/typescript/send) ,[Python](https://docs.pinnacle.sh/quickstart/rcs/python/send) ,[Ruby](https://docs.pinnacle.sh/quickstart/rcs/ruby/send) ). Enable[message tracking](https://docs.pinnacle.sh/guides/messages/sending) for precise reactions, and set up[webhooks](https://docs.pinnacle.sh/guides/messages/receiving) to receive reactions from users.


Want to see how Pinnacle fits your stack?[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+RCS+message+reactions) with one of our engineers — we'll walk through your use case and get you live.
