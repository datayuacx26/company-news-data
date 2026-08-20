---
schema_version: "1.0.0"
document_id: "ac19526a7be8947a718e8650445e520b6b086dc28d87cdb633339d8d7f8f66a9"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/rcs-test-agents-send-your-first-rich-message-in-under-two-minutes"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-22T09:08:45.431702+00:00"
fetched_at: "2026-07-28T22:16:03.530620+00:00"
content_hash: "sha256:0e9aa3d59d3df5320ab488c3e7f78c0da832e3833daa675fdd3feea587e7ca5f"
---

# RCS Test Agents: Send Your First Rich Message in Under Two Minutes

## From Zero to Rich Message in Under Two Minutes


Most RCS platforms make you wait weeks to send your first message. Register a brand. Submit a campaign. Wait for carrier review. Get rejected. Resubmit. Wait again. By the time you send your first rich card, you've invested days of bureaucratic overhead before writing a single line of integration code.


Pinnacle test agents skip all of that. Create a test agent, whitelist your phone number, accept the tester invite on your device, and send — rich cards, buttons, carousels, media, the full RCS experience. No carrier review. No campaign submission. No waiting.


The entire process takes less than two minutes.


---


## What Is an RCS Test Agent?


To understand test agents, you need to understand how RCS Business Messaging works under the hood.


Every RCS message you receive from a business — a branded card from an airline, a shipping notification with a tracking button, a promotional carousel from a retailer — comes from an **RCS agent** . An agent is the business's identity in the RCS ecosystem: it has a verified name, logo, brand color, hero image, and contact details. When a customer opens a conversation, they see the agent's branding, not a raw phone number.


In production, creating an agent requires **carrier approval** . You submit your brand, define your use case, provide compliance documentation, and wait for Google's RBM (RCS Business Messaging) platform and the carrier network to review and verify your agent. This process exists for good reason — it ensures recipients can trust the businesses messaging them — but it takes days to weeks, and it's a significant barrier to getting started.


**Test agents** are the RBM platform's answer to this. A test agent is a fully functional RCS agent identity that bypasses carrier review entirely. It has its own brand name, logo, hero image, brand color, and contact details — it looks and behaves exactly like a production agent. The only restriction: it can only send messages to **whitelisted phone numbers** that have explicitly opted in as testers.


### Why Most Platforms Don't Expose This


Here's the thing: test agents are a capability of Google's RBM platform, but most messaging providers don't surface them to developers. The typical onboarding for RCS on other platforms looks like this:


1. Contact sales
2. Go through a multi-week onboarding process
3. Submit brand verification documents
4. Wait for carrier approval
5. *Then* send your first message


Pinnacle exposes test agent creation as a **self-service API call** . No sales call. No onboarding process. No waiting. You sign up, you hit the API (or click a button in the dashboard), and you have a working RCS agent in seconds. This is one of the few platforms — if not the only one — where you can go from zero to a branded RCS message on a real device in under two minutes.


### What Makes Test Agents Useful


- **Prototyping RCS experiences** before committing to a production campaign
- **Development and QA** — build and test your messaging integration against real devices
- **Demos** — show stakeholders exactly what your RCS messages will look like on a real phone
- **AI agent development** — test MCP-driven workflows end-to-end with real RCS delivery
- **Evaluation** — try RCS for your use case before investing in the production registration process


You can create up to **5 test agents per account** , each with their own branding and configuration.


---


## Three Ways to Create a Test Agent


### 1. The SDK (TypeScript)


The fastest path if you're already building with code:


TypeScript


```text
import   {   PinnacleClient   }   from   "  rcs-js  "  ;
const   client   =   new   PinnacleClient  ({   apiKey  :   process  .  env  .  PINNACLE_API_KEY   });


// Step 1: Create the test agent
const   agent   =   await   client  .  rcs  .  test  .  createAgent  ({
displayName  :   "  Pinnacle  "  ,
description  :   "  RCS messaging by Pinnacle  "  ,
logoUrl  :   "  https://cdn.pinnacle.sh/logo.png  "  ,
heroUrl  :   "  https://cdn.pinnacle.sh/hero.png  "  ,
phoneNumbers  :   [{   number  :   "  +18005550001  "  ,   label  :   "  Support  "   }],
emails  :   [{   address  :   "  founders@pinnacle.sh  "  ,   label  :   "  Support  "   }],
websites  :   [{   url  :   "  https://pinnacle.sh  "  ,   label  :   "  Website  "   }],
privacyUrl  :   "  https://pinnacle.sh/privacy  "  ,
termsUrl  :   "  https://pinnacle.sh/terms  "  ,
color  :   "  #6B4EFF  "  ,
isConversational  :   true  ,
agentUseCase  :   "  MULTI_USE  "  ,
});


console  .  log  (  `  Agent created:   ${  agent  .  body  .  id  }  `  );
// Output: Agent created: agent_abc123def456
```


After creation, there's a **2-minute cooldown** (a requirement from Google's RBM platform as your agent is being set up) before you can whitelist phone numbers. Then:


TypeScript


```text
// Step 2: Whitelist your phone number
const   whitelist   =   await   client  .  rcs  .  test  .  whitelistNumber  (  agent  .  body  .  id  ,   {
phoneNumber  :   "  +14155551234  "  ,
});


console  .  log  (  `  Whitelist status:   ${  whitelist  .  body  .  status  }  `  );
// Output: Whitelist status: PENDING
```


The recipient gets a message from "RBM Tester Management" — tap **"Make me a tester"** to accept. Once accepted:


TypeScript


```text
// Step 3: Send your first RCS message
await   client  .  messages  .  rcs  .  send  ({
from  :   agent  .  body  .  id  ,
to  :   "  +14155551234  "  ,
cards  :   [
{
title  :   "  Your order has been delivered!  "  ,
subtitle  :   "  Rate the dropoff  "  ,
media  :   "  https://cdn.pinnacle.sh/delivery-photo.jpg  "  ,
buttons  :   [
{
type  :   "  trigger  "  ,
title  :   "  Rate  "  ,
payload  :   "  rate  "  ,
},
],
},
],
quickReplies  :   [],
});
```


That's it. Three steps, three API calls, and you have a branded RCS message on a real device.


A real RCS message from a test agent — branded, interactive, and delivered in seconds.


---


### 2. The MCP Server (AI-Powered)


If you're using the[Pinnacle MCP server](https://docs.pinnacle.sh/mcp) with Claude, Cursor, or any MCP-compatible AI tool, you can create a test agent with natural language:


> "Create an RCS test agent called 'Acme Support' with a purple brand color, my company logo, and conversational mode enabled. Then whitelist my phone number +14155551234."


The MCP server's` create_test_agent` and` whitelist_test_agent` tools handle the full setup. Once the tester invite is accepted, use` send_rcs` to send your first message — all from the same AI conversation.


This is particularly powerful for iterating on message design. Describe the card layout you want, send it, see it on your device, adjust, and send again — all without leaving your AI workflow.


---


### 3. The Dashboard (No Code Required)


For teams that want a visual setup experience, the Pinnacle dashboard at[app.pinnacle.sh](https://app.pinnacle.sh/) provides a complete test agent management UI:


1. Navigate to the **RCS Agents** section
2. Click **Create Test Agent**
3. Fill in your brand details — name, logo, hero image, color, contact info
4. Save the agent and whitelist your phone number from the agent detail page
5. Accept the tester invite on your device
6. Send a test message from the **Conversations** dashboard


No code, no CLI, no API calls. The dashboard handles everything visually.


Create a test agent from the dashboard — fill in your branding and you're ready to go.


---


## The Whitelist Flow


After creating a test agent, you whitelist specific phone numbers that can receive messages from it. Here's what happens:


1. **You whitelist a number** — via SDK, MCP, or dashboard
2. **The recipient gets an invite** — a message from "RBM Tester Management" appears on their device
3. **The recipient taps "Make me a tester"** — this accepts the invite
4. **Status transitions to` ACCEPTED`** — the number can now receive RCS messages from your test agent


The tester invite from RBM Tester Management — tap "Make me a tester" to accept and start receiving RCS messages.


You can check the whitelist status at any time:


TypeScript


```text
const   status   =   await   client  .  rcs  .  test  .  getWhitelistStatus  (  agent  .  body  .  id  ,   {
phoneNumber  :   "  +14155551234  "  ,
});


console  .  log  (  status  .  body  .  status  );   // "PENDING" | "ACCEPTED" | "REJECTED"
```


Or list all whitelisted numbers and their statuses:


TypeScript


```text
const   numbers   =   await   client  .  rcs  .  whitelistedNumbers  .  list  ();
```


**Important timing note:** There is a mandatory **2-minute cooldown** after creating or updating a test agent before you can whitelist numbers. This is a requirement imposed by Google's RBM platform, not a Pinnacle limitation. If you attempt to whitelist during the cooldown, you'll receive an error with the remaining wait time.


---


## What Can a Test Agent Do?


Everything a production agent can do. Test agents are fully functional RCS senders — the only restriction is that they can only message whitelisted numbers. This means you can test:


- **Rich cards** with titles, subtitles, media, and buttons
- **Carousels** with multiple swipeable cards
- **Quick replies** for interactive conversation flows
- **Media messages** — images, GIFs, and videos
- **Typing indicators** — show the animated dots while your agent processes a response
- **Webhooks** — receive inbound messages, delivery receipts, and typing events from test recipients
- **Fallback** — test SMS/MMS fallback behavior for non-RCS devices
- **Scheduling** — schedule test messages for future delivery


TypeScript


```text
// Send a carousel to your test device
await   client  .  messages  .  rcs  .  send  ({
from  :   agent  .  body  .  id  ,
to  :   "  +14155551234  "  ,
cards  :   [
{
title  :   "  Classic Sneaker  "  ,
subtitle  :   "  $89.99  "  ,
media  :   "  https://cdn.yourapp.com/sneaker-classic.jpg  "  ,
buttons  :   [
{
type  :   "  openUrl  "  ,
title  :   "  Buy Now  "  ,
payload  :   "  https://yourapp.com/classic  "  ,
},
],
},
{
title  :   "  Limited Edition  "  ,
subtitle  :   "  $149.99 — Only 50 left  "  ,
media  :   "  https://cdn.yourapp.com/sneaker-limited.jpg  "  ,
buttons  :   [
{
type  :   "  openUrl  "  ,
title  :   "  Buy Now  "  ,
payload  :   "  https://yourapp.com/limited  "  ,
},
],
},
{
title  :   "  Kids Collection  "  ,
subtitle  :   "  Starting at $49.99  "  ,
media  :   "  https://cdn.yourapp.com/sneaker-kids.jpg  "  ,
buttons  :   [
{
type  :   "  openUrl  "  ,
title  :   "  Browse All  "  ,
payload  :   "  https://yourapp.com/kids  "  ,
},
],
},
],
quickReplies  :   [
{   type  :   "  trigger  "  ,   title  :   "  View Cart 🛒  "  ,   payload  :   "  cart  "   },
{   type  :   "  trigger  "  ,   title  :   "  Help  "  ,   payload  :   "  help  "   },
],
});
```


Carousels, buttons, quick replies — test every RCS feature before going to production.


---


## Updating Your Test Agent


Branding evolves. You can update any field on a test agent without recreating it:


TypeScript


```text
await   client  .  rcs  .  test  .  updateAgent  (  agent  .  body  .  id  ,   {
displayName  :   "  Acme Premium Support  "  ,
color  :   "  #0066FF  "  ,
description  :   "  Premium support for Acme Pro members  "  ,
});
```


Only pass the fields you want to change — everything else stays the same. Note that updating triggers the same **2-minute cooldown** before you can whitelist new numbers.


---


## From Test Agent to Production


When your RCS experience is validated and you're ready to go live, the path forward is:


1. **Create a brand** — register your business with Pinnacle
2. **Submit an RCS campaign** — define your production agent with the branding you refined during testing
3. **Pass carrier review** — Pinnacle handles the submission to Google's RBM platform
4. **Launch** — your production agent can message any RCS-capable device, no whitelisting required


The test agent is your staging environment. The production campaign is your release. Everything you built and validated during testing — message templates, conversation flows, webhook handlers, AI agent logic — carries over directly.


For detailed guidance on the production registration process, see our[RCS verification guide](https://www.pinnacle.sh/blog/how-to-get-your-business-verified-for-rcs) and the[RCS campaign documentation](https://docs.pinnacle.sh/guides/campaigns/rcs) .


---


## Frequently Asked Questions


### How many test agents can I create?


Up to 5 per account. Each test agent has its own branding, configuration, and whitelisted numbers.


### How many phone numbers can I whitelist per test agent?


There is no hard limit on whitelisted numbers per agent. Whitelist as many test devices as your team needs.


### Can I use test agents for blasts?


No. Test agents can only send to individually whitelisted numbers. For bulk messaging, you need a production RCS campaign. See the[bulk messaging guide](https://www.pinnacle.sh/blog/bulk-messaging-send-sms-mms-rcs-to-thousands-at-once) .


### What happens if the tester invite is rejected?


The whitelist status transitions to` REJECTED` . The recipient can re-accept by tapping "Make me a tester" in the original invite message from "RBM Tester Management". Some carriers may also reject the invite if they don't support test agents — in that case, try a different device or carrier.


### Do test agent messages cost credits?


Test messages use the same RCS rate as production messages. They're real messages delivered through the carrier network.


### Can I attach webhooks to a test agent?


Yes. Attach webhooks the same way you would for any sender — use the agent ID (e.g.,` agent_abc123def456` ) as the sender identifier. You'll receive` MESSAGE.STATUS` ,` MESSAGE.RECEIVED` , and` USER.TYPING` events for your test conversations.


---


## Key Takeaways


- **Under 2 minutes** : Create a test agent, whitelist your phone, accept the invite, and send — the entire flow takes less than two minutes
- **Full RCS feature set** : Rich cards, carousels, buttons, quick replies, media, typing indicators, webhooks, fallback — everything works in test mode
- **Three paths** : SDK, MCP server, or dashboard — pick whichever fits your workflow
- **Up to 5 agents** : Create multiple test agents with different branding for different projects or clients
- **Seamless production path** : Everything you build with test agents carries over to production — same SDK methods, same message format, same webhook handlers


---


## Get Started


Create your first test agent now:


- **SDK** :` client.rcs.test.createAgent({...})` —[full API reference](https://docs.pinnacle.sh/api-reference/rcs-agents/test/create-agent)
- **MCP** :` create_test_agent` tool —[MCP server docs](https://docs.pinnacle.sh/mcp)
- **Dashboard** :[app.pinnacle.sh](https://app.pinnacle.sh/) — create visually, no code required


Want to see how Pinnacle fits your stack?[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+getting+started+with+RCS) with one of our engineers — we'll walk through your use case and get you live.
