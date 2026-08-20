---
schema_version: "1.0.0"
document_id: "a368ef001e419b1ce89ca3dbae600123a647140021140c1d667e49dbf56b0a18"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/sendblue-chat-sdk-adapter"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:56229c6909f3d27cf76fce036bbbbae0db03b30a426f7efd34374dd5ce2789e2"
---

# Sendblue Chat SDK Adapter - iMessage for Chat SDK Bots | Sendblue

[Home](https://www.sendblue.com/) /[Blog](https://www.sendblue.com/blog) / Chat SDK adapter


June 2, 2026


5 min read


Nikita Jerschow


# Sendblue Chat SDK Adapter: iMessage for Chat SDK Bots


Chat SDK gives developers one framework for building bots across messaging channels. The Sendblue adapter brings iMessage, SMS, and RCS into that same model, so teams can build one bot and reach customers in the Messages app they already use.


Today we are documenting support for the Sendblue Chat SDK adapter. It lets a Chat SDK bot receive inbound Sendblue webhooks, send outbound messages through Sendblue, and use iMessage-specific features such as typing indicators, tapback reactions, delivery callbacks, and message history.


**Maintenance commitment:** Sendblue is committed to maintaining the adapter integration path as Chat SDK and the Sendblue API evolve. We will keep the primary docs current, support the adapter for production builders, and route account or delivery issues through Sendblue support.


## Install the Adapter


Install Chat SDK and the Sendblue adapter package:


```text
npm install chat chat-adapter-sendblue @chat-adapter/state-memory
```


` @chat-adapter/state-memory` is useful for local development and examples. In production, use persistent Chat SDK state such as Redis or Postgres so subscribed threads and conversation state survive deploys.


Add your Sendblue credentials to the environment:


```text
SENDBLUE_API_KEY=your-api-key-id
SENDBLUE_API_SECRET=your-api-secret-key
SENDBLUE_FROM_NUMBER=+15551234567
SENDBLUE_WEBHOOK_SECRET=optional-shared-secret
```


## Create a Chat SDK Bot


The adapter plugs into Chat SDK the same way as other channel adapters:


```text
import { Chat } from "chat";
import { createSendblueAdapter } from "chat-adapter-sendblue";
import { createMemoryState } from "@chat-adapter/state-memory";


const chat = new Chat({
userName: "imessage-bot",
adapters: {
sendblue: createSendblueAdapter(),
},
state: createMemoryState(),
});


chat.onDirectMessage(async (thread, message) => {
await thread.post(`Got it: ${message.text}`);
});
```


## Handle Sendblue Webhooks


Point your Sendblue receive webhook at a route in your app, then pass requests to Chat SDK:


```text
// app/api/webhooks/sendblue/route.ts
import { chat } from "@/lib/chat";


export async function POST(request: Request) {
await chat.initialize();
return chat.webhooks.sendblue(request);
}
```


From there, inbound iMessages show up as Chat SDK messages and outbound replies are delivered through Sendblue. iPhone users get blue bubbles, while SMS and RCS can be accepted when you enable those services in the adapter config.


## Why This Matters


AI agents are moving from demos to real customer workflows. Those workflows need channels where people actually respond. Sendblue gives Chat SDK bots a native messaging surface on iPhone, with automatic fallback for non-iMessage recipients and webhooks for replies and status updates.


For teams already using Chat SDK, this means you can keep your bot logic in one place while adding iMessage as a production channel. For Sendblue developers, it means faster access to the broader Chat SDK adapter ecosystem.


## What Is Supported


- Inbound and outbound one-to-one messages
- iMessage-first delivery with SMS/RCS service filtering
- Delivery status callbacks
- Typing indicators for one-to-one conversations
- Tapback reactions through Sendblue reaction types
- Message history with cursor pagination
- Direct access to the official Sendblue TypeScript SDK for media, contacts, groups, and newer API features


## Start Building


[Read the adapter docs Set up credentials, webhooks, sending, reactions, and production notes.](https://docs.sendblue.com/guides/chat-sdk-adapter/)[Create a Sendblue account Use the CLI or dashboard to get a free sandbox and API keys.](https://docs.sendblue.com/getting-started/quickstart/)[Install from npm Add the adapter package to your Chat SDK application.](https://www.npmjs.com/package/chat-adapter-sendblue)


We are excited to make iMessage a first-class channel for Chat SDK builders. If you are building an agent that should talk to customers over text, this is the shortest path from bot logic to real blue-bubble conversations.


Chat SDK


iMessage API


AI Agents


Developer Tools
