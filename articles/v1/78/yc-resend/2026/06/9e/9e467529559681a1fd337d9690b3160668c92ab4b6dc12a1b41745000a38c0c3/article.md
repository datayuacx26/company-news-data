---
schema_version: "1.0.0"
document_id: "9e467529559681a1fd337d9690b3160668c92ab4b6dc12a1b41745000a38c0c3"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/engineering-a-vercel-chat-adapter"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:40f96caa5cc9728877110598f7e4f4470ae6b9fe52f9b45cf51514c7b2ff7adc"
---

# Engineering a Vercel Chat Adapter

AI agents communicate through channels like Slack, chat apps, and SMS. Most of these channels are ephemeral. Messages disappear into feeds, threads get buried, and there's no real record of what happened.


Email is the one protocol that nearly everyone already uses, and conversations don't vanish. We built the[official Resend adapter](https://github.com/resend/resend-chat-sdk) for Vercel Chat SDK to bring email into the Chat SDK ecosystem.


Along the way, we faced challenges, made decisions, and weighed tradeoffs. Let's talk about the development process behind` @resend/chat-sdk-adapter` .


## About the adapter


Vercel's[Chat SDK](https://chat-sdk.dev/) is an open-source, TypeScript SDK for building chat bots across Slack, Microsoft Teams, Google Chat, Discord, and more. Write your bot logic once and deploy everywhere.


The Resend adapter extends the SDK, bringing email capabilities to your chat apps.


The Resend adapter treats email as a bidirectional chat channel. Inbound emails arrive via Resend webhooks and trigger Chat SDK event handlers. Outbound messages get delivered as emails through the Resend API.


Practically, this means that you can send emails from your chat interface, build proactive outbound notifications, and more.


## Threading without a database


Chat platforms give you thread IDs for free. A message arrives on Slack with a` thread_ts` . Discord has channel IDs.


**Email doesn't work like that.** There is no central thread ID, so threading is a client-side concern.


There are two headers that let mail clients reconstruct conversation threads:` In-Reply-To` and` References` . These require` Message-ID` s of prior emails to keep track of the thread. We needed to derive thread IDs from email headers alone. The format we chose looks like this:


```text
resend:{sender-address}:{sha256-first-16-chars-of-root-message-id}


```


When a new email arrives, the` ThreadResolver` checks whether` In-Reply-To` or` References` point to a message we've already seen. If so, it's part of an existing thread. If not, we hash the` Message-ID` to create a new thread ID.


```text
async     resolveThreadId  (  input  :   ResolveInput  )  :     Promise  <  string  >     {    	  const     {   toAddress  ,   messageId  ,   inReplyTo  ,   references   }     =   input  ;    	   	  if     (  inReplyTo   ||   references  )     {    		  const   candidateIds   =     this  .  extractMessageIds  (  inReplyTo  ,   references  )  ;    		   		  for     (  const   candidate   of   candidateIds  )     {    			  const   existingThread   =     this  .  messageToThread  .  get  (  candidate  )  ;    			   			  if     (  existingThread  )     {    				  this  .  trackMessage  (  existingThread  ,   messageId  )  ;    				  return   existingThread  ;    			  }    		  }    	  }    	   	  const   hash   =     await     hashMessageId  (  messageId  )  ;    	  const   threadId   =     this  .  encodeThreadId  (  {   toAddress  ,   rootMessageIdHash  :   hash   }  )  ;    	  this  .  trackMessage  (  threadId  ,   messageId  )  ;    	   	  return   threadId  ;     }
```


An in-memory` Map` is enough here. Email already carries its own history in the` References` header, so we don't need persistence. As long as the adapter process is running, it can track which messages belong to which threads.


## Mapping between email and chat


The Chat SDK expects adapters to implement` parseMessage` (inbound) and` postMessage` (outbound). Both have specific shapes.


On the **inbound** side, the SDK wants a` Message` with an author that has` userId` ,` userName` , and` isMe` values. For email, the identity fields are the sender's address.` isMe` is true if the sender matches the bot's` fromAddress` .


On the **outbound** side,` postMessage` accepts an` AdapterPostableMessage` . This is a union that can be a plain string, markdown, an AST, or a structured card. Each needs to become an email with HTML and plain text options. Strings and markdown are straightforward. Cards are not.


## Rendering cards as email


The Chat SDK has a` CardElement` type for structured messages: cards with titles, sections, fields, buttons, images, dividers. We had to turn them into email HTML.


A card rendered in an email inbox


To accomplish this, we turned to our open source package,[React Email](https://react.email/) .


Each` CardElement` type maps to a React Email component:


- **Text** (with a content property) becomes` <Text>`
- **Field** (with label and value) becomes a bolded label and a value
- **Link-button** (with label and url) becomes` <Button>`
- **Image** (with label and url) becomes` <Img>`
- **Divider** becomes` <Hr />`


## What I learned


The bulk of the work when building the Chat SDK adapter was translation. Each platform has its own version of threads, messages, authors, and some form of rich content. The work is figuring out how platforms represent those things differently and writing the conversion layer.


Email's representations are older and less convenient than all the other platforms. You have to reconstruct a thread from headers alone. No structured content format exists; you need to render archaic email HTML.


But **email reaches everyone** . There's no OAuth, no app installations, and no workspace invites. You simply send to an address and they receive it. It's the democratic protocol that the web was built on.


We're excited to see what you build with the Resend adapter. Check the[docs](https://resend.com/docs/chat-sdk) for the full setup guide, or view the[open source code](https://github.com/resend/resend-chat-sdk) .
