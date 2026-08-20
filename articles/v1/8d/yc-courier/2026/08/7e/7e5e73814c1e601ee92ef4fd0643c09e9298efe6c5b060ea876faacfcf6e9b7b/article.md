---
schema_version: "1.0.0"
document_id: "7e5e73814c1e601ee92ef4fd0643c09e9298efe6c5b060ea876faacfcf6e9b7b"
company_key: "yc-courier"
company: "Courier"
source_id: "yc-courier-news-import-df9818472bef"
canonical_url: "https://www.courier.com/blog/intercom-alternative"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T10:39:34.414934+00:00"
fetched_at: "2026-08-19T10:39:36.844729+00:00"
content_hash: "sha256:fda6b31dda06b5440c1bfebabb919d177b9f793065c31964bf457fbb7900b8f0"
---

# A developer's alternative to Intercom for product notifications

**Intercom is a customer support platform. Courier is notification infrastructure.** If you're using Intercom to send your product's transactional and marketing messages, you're using a help desk for a job its API was not built to do.


If you're weighing an Intercom alternative right now, the timing isn't a coincidence. In May 2026, Intercom renamed itself Fin, after its AI support agent. A month later, Salesforce signed a definitive agreement to acquire the company for about $3.6 billion, planning to fold that agent technology into Agentforce. The deal is signed, not closed, with Salesforce expecting it to complete in the final quarter of its fiscal 2027.


The rename and the acquisition point the same direction, and it isn't lifecycle messaging. If you're deciding where your product's notifications will live for the next three years, one option is a support suite mid-acquisition whose roadmap is aimed at agent automation. That's a bet on someone else's priorities matching yours.


## Intercom vs Courier for product notifications


**What Intercom's API cannot do.**


- No push or SMS send endpoint. Intercom's send endpoint accepts only` in_app` ,` email` , and` whatsapp` .
- No way to reference a template you designed in the composer from code.
- No delivery status for email, push, or in-app messages.
- No API at all for Intercom Series, its campaign builder.


**What Courier does.** Courier is notification infrastructure covering both transactional sends and marketing campaigns.


- One API call reaches email, SMS, push, in-app, and chat.
- Sends go through your own provider accounts, so your domains and sender reputation stay yours.
- Preferences are enforced at send time on every channel.
- Courier Journeys replaces Intercom Series for multi-step campaigns, adding batch, digest, and throttle nodes Series has no equivalent for.
- Courier Broadcasts handles one-off campaigns to an audience.
- The UI, the API, and an MCP server expose the same capabilities, so engineers, marketers, and coding agents work one platform.


**Cost.** Courier charges $0.005 per message sent on any channel, with no per-seat or per-channel fees and 10,000 free sends per month. At published rates, sending 2,000 push notifications a month costs $223 on Intercom (one seat, the required $99 add-on, and metered usage) and $0 on Courier.


**Migration.** Both transactional and marketing sends move to Courier. Do transactional first, since those calls are already API-driven, then rebuild Intercom Series as Courier Journeys. Intercom keeps the help desk. Courier's[migration guide](https://www.courier.com/docs/tutorials/migrate/from-intercom) has the full concept mapping and a copy-pasteable prompt for running the work with a coding agent.


## Why Intercom isn't a fit


Intercom hasn't announced any plan to sunset outbound messaging, and this post isn't claiming it has. The question is fit, and the clearest evidence isn't the strategy. It's the API. Everything below comes from Intercom's own published OpenAPI spec, version 2.16, read in August 2026.


` POST /messages` is the only send endpoint, and this is its type enum:


```text
message_type: in_app | email | whatsapp
```


No push. No SMS. The` template` field on the same call looks like a way out until you read its description: "The style of the outgoing message. Possible values` plain` or` personal` ." It's a style, not a reference, so content you built in the composer is unreachable from code and the` body` you send is HTML your backend assembled.` from` must be an admin, so every API send is attributed to a seat-holding human. And a flag called` create_conversation_without_contact_reply` exists because outbound sends are modeled as support conversations.


Intercom is a support platform, and its send API is built like one. The consequence is that the only way to trigger a designed, multichannel message from your backend is indirect: post an event, and let a message you configured in the UI match against it. That path has documented ceilings.


- **120 active event names** per workspace, and each event drives at most 50 live messages.
- **No nested JSON.** An order with line items doesn't fit.
- **Deduplication** on workspace, contact, event name, and timestamp to the second, so two legitimate identical sends in the same second collapse into one.
- **Up to 24 hours** before a new metadata field is usable in the composer.
- **One event rule per message** , and event-triggered messages aren't eligible for re-notification.


The event returns` 202 Accepted` with an empty body. No message ID, and no delivery status endpoint for email, push, or in-app; the one that looks general is WhatsApp-only. So "did the password reset actually send?" has no programmatic answer.


## Why Courier fits


Courier is notification infrastructure: transactional sends and marketing campaigns on one platform. That's the entire product rather than a module beside a help desk, and the difference shows up in the same place Intercom's focus does: what the API lets you do.


What a product notification needs Intercom Courier


Send on any channel from your backend` in_app` ,` email` ,` whatsapp` One call, every channel


Reference content you designed Style only (` plain` or` personal` ) Name a template


Know whether it sent` 202` , no ID, no status endpoint Message ID and delivery history


Own your sending reputation Shared IPs, no outside provider Your provider accounts and IPs


Consent across channels` email` and` sms_message` Every channel, topics enforced at send


Roll-ups and frequency caps Precompute in your backend Digest, batch, throttle, send limits


Version campaigns as code No Series API Journeys API


Price that tracks sending Seats, add-on, per-channel metering $0.005 per send, flat


Four of those need more than a table row.


**One send call covers every channel.** You pass a recipient, a template, and data. Courier resolves which channels to use from the user's preferences and your routing rules, renders the content per channel, and returns an ID you can trace. Adding SMS to a notification that was email-only is a routing change, not a new integration.


**Your providers stay yours.** Courier sends through your SendGrid, SES, Postmark, Twilio, or FCM accounts, on your domains and your IPs. Your deliverability is a thing you own and can fix, not a shared pool you're a tenant in. Configure more than one provider per channel and Courier[fails over](https://www.courier.com/docs/platform/sending/failover) between them.


**Preferences are enforced, not advisory.** Topics span every channel including push and in-app, and Courier checks them at send time rather than trusting each campaign to remember. Mark a topic required and receipts still reach someone who has opted out of marketing. A hosted preference page and embeddable components ship with it, so the unsubscribe experience exists on day one.


**Engineers, marketers, and agents use the same platform.** Courier has full parity between the UI and the API, so nobody gets the degraded surface:


- **Templates** are built visually in[Design Studio](https://www.courier.com/platform/design-studio) , one template holding email, SMS, push, in-app, and chat.
- **Audiences** are rule-built on profile attributes and update automatically as data changes.
- **One-time sends** run through[Broadcasts](https://www.courier.com/docs/platform/broadcasts/broadcasts-overview) : pick a template, pick an audience, send or schedule.
- **Journeys** replace Series for multi-step campaigns, with a visual editor over the same nodes engineers drive through the API, including[experiments](https://www.courier.com/docs/platform/journeys/experiments) for testing message variants.
- **An in-app inbox** comes as a channel with drop-in components for React, iOS, Android, Flutter, and vanilla JS.
- **Coding agents** work the platform through the[API](https://www.courier.com/docs/reference/api-overview) and an[MCP server](https://www.courier.com/docs/tools/mcp) , so an agent can build templates, wire journeys, and read delivery logs.


Agent access is why these migrations move faster than they used to. The mechanical work, reading a campaign definition and creating its equivalent, is exactly what an agent is good at, and Courier exposes every capability it needs to do it.


## What changes for transactional sends


Transactional is where a support platform's assumptions hurt most, because the requirements are strict.


Intercom's documentation says dedicated IP addresses aren't available and all email goes over shared IPs, with no option to send through your own provider, so your password resets share reputation with other companies' campaigns. Event-driven email usually sends in seconds but can take up to 30 minutes under load. When activity restrictions trigger, email only reaches contacts seen or engaged within the last 180 days, which is exactly the wrong constraint for a dormant user asking for a password reset. And Subscription Types accept only` email` or` sms_message` , so there's no push or in-app consent model.


Courier changes all four, because you send through[your own provider accounts](https://www.courier.com/docs/external-integrations/integrations-overview) rather than a shared pool. Your domains, your reputation, your dedicated IP if you have one, and no engagement-recency rule standing between a dormant user and their login code.


## What changes for marketing sends


Series does real work, and rebuilding it takes real effort. Three things change shape.


**Roll-ups move into the platform.** Intercom has no digest primitive, so "one summary instead of nine notifications" gets precomputed in your backend before you fire the triggering event. Courier has[batch](https://www.courier.com/docs/platform/journeys/nodes/batch) and[digest](https://www.courier.com/docs/platform/journeys/nodes/digest) nodes that collapse many events into one message inside the journey.


**Frequency capping stops being per-message.** Intercom caps occurrences on each message individually, so nothing coordinates across campaigns. Courier's[send limits](https://www.courier.com/docs/platform/sending/send-limits) cap volume globally, per user, per topic, or per tenant, holding across every journey and broadcast at once.


**Localization stops being copy-paste.** Intercom's documented approach is to duplicate the message per language and target each copy with an audience rule on the contact's language. Eight locales, eight messages to keep in sync. Courier keeps locales on one template: set` locale` on the profile or the send, and the right version renders.[AI Translations](https://www.courier.com/docs/platform/content/design-studio/ai-translations) generates those locale versions from the template you already wrote, so adding a language is a step rather than a project.


## What Courier costs compared to Intercom


Comparing Intercom and Courier isn't license against license. Intercom prices notifications like a support tool: per seat, plus a $99 add-on for the journey builder and transactional sending, plus per-message metering on the channels notifications actually use. Courier's pricing page states its model in one line: "based on notifications sent, not team size or channels."


Intercom Courier


**Pricing basis** Per seat, plus usage per channel Per message sent, volume pricing at scale


**Published rates cover** Up to 2,000 messages/month Any volume


**Entry price** $29 to $132 per seat per month Free tier (10,000 sends/month), then $0.005 per message


**Push** $0.06 to $0.07 after a 500/month allowance $0.005


**Email** From $0.045, tiered $0.005


**SMS** $0.06 to $0.09 per segment, carrier included $0.005 plus your own carrier rate


**Provider choice** Intercom's shared IPs only Your own provider accounts


**Delivery status API** WhatsApp only Every message


Here's the smallest complete comparison, built only from what both companies publish. Send 2,000 push notifications a month, which is the top of Intercom's published rate card:


Intercom Courier


Plan minimum 1 Essential seat, $29 Free Developer tier, $0


Journey builder Proactive Support Plus, $99 Included


Transactional sending Same $99 add-on Included


2,000 push messages 500 included, then $0.07 and $0.06 by tier: $95 Inside the 10,000 free sends


**Monthly total** **$223** **$0**


Courier stays free to 10,000 sends a month, five times that volume, before you pay anything. After that it's $0.005 a send on every channel, with volume pricing available as you scale.


Above 2,000 messages Intercom's rates aren't published, so a like-for-like number at real volume doesn't exist. Courier publishes its rate up front instead, and once you're sending more than 100,000 messages a month, come talk to us about volume pricing. Intercom discounts heavily too, especially for startups. The difference is where the conversation starts: with a published number, or without one.


## When Intercom is still the right call


- **Support is the job.** If what you need is a help desk, an AI agent that resolves tickets, a help center, and a shared inbox, Intercom is a strong product and this post isn't an argument against it. Most teams migrating notifications to Courier keep Intercom for support.
- **Your outbound is onboarding UI, not messages.** Product tours, tooltips, checklists, and banners are in-product guidance. Courier doesn't replace those. In-app banners are on our roadmap; the rest belong in Intercom or a product-adoption tool.
- **Your volume is tiny and your channels are email-only.** If you send a few hundred messages a month, all email, entirely to recently active users, the ceilings above are theoretical and the add-on may be cheaper than a migration.


## How do you migrate off Intercom?


Transactional first. Those sends are already API-driven, they carry the most risk if they break, and shared IPs and a 30-minute delay ceiling serve them least well.


The order that works: inventory every live outbound message and Series, split it into transactional, marketing, and in-product guidance, then find the events in your code that exist only to trigger a message. Those become direct send calls, which is usually a small diff. Connect your providers, sync profiles, rebuild the highest-volume templates, and run both systems in parallel until the output matches. Map Subscription Types to preference topics and import opt-outs before any marketing send. Then rebuild Series as journeys, one at a time.


Push tokens are the one item you can't rush. Intercom collects them through its SDK, so they live inside Intercom rather than in your data. Start registering tokens with Courier from your app while Intercom is still live and let coverage build across a release cycle or two.


The full concept mapping, the documented constraints with citations, and a copy-pasteable prompt for running the work with a coding agent are in our guide to[migrating from Intercom to Courier](https://www.courier.com/docs/tutorials/migrate/from-intercom) .


## Frequently asked questions


### Is Courier a replacement for Intercom?


Not for support. Courier replaces the notification layer: transactional sends, campaigns, journeys, templates, preferences, and an in-app inbox across email, SMS, push, in-app, and chat. Intercom's help desk, Messenger, tickets, Help Center, and Fin have no Courier equivalent, and most teams keep them. The split that works is Intercom for conversations with your support team, Courier for what your product sends.


### Can I send a push notification through the Intercom API?


No.` POST /messages` accepts only` in_app` ,` email` , and` whatsapp` . To send a push from your backend you post an event to` POST /events` and rely on a message you configured in the UI matching against it, which returns` 202 Accepted` with no message ID and no delivery status endpoint to check afterward.


### What replaces Intercom Series in Courier?


[Journeys](https://www.courier.com/docs/platform/journeys/journeys-overview) . Entry rules become triggers (API invoke, inbound webhook, Segment event, or audience membership), waits become delay nodes, rules branches become branch nodes, and messages become send nodes. Journeys add batch, digest, throttle, and fetch-data nodes that Series has no equivalent for, and unlike Series they can be built through the API.


### Can Courier handle marketing campaigns, or only transactional messages?


Courier handles both.[Courier Journeys](https://www.courier.com/docs/platform/journeys/journeys-overview) is a visual campaign builder for multi-step lifecycle and onboarding sequences, and it replaces Intercom Series directly.[Broadcasts](https://www.courier.com/docs/platform/broadcasts/broadcasts-overview) handles one-off sends to an audience. Marketers get a UI for templates, audiences, broadcasts, and journey editing, and engineers get the same capabilities through the API. Journeys also add batch, digest, and throttle nodes that Series has no equivalent for.


### How much does Intercom cost for notifications?


Intercom pricing for notifications has three layers. Intercom charges per seat, $29 to $132 per seat per month, plus $99 a month for the Proactive Support Plus add-on that gates Series and transactional messaging, plus per-message usage: $0.06 to $0.07 for push and other "Messages Sent" channels after a 500/month allowance, from $0.045 for bulk email, and $0.06 to $0.09 per SMS segment. Those published tiers only run to 2,000 messages a month; above that, pricing is negotiated. Courier charges a flat $0.005 per message on any channel at any volume, with 10,000 free per month. On SMS the two aren't directly comparable: Intercom's per-segment rate includes the carrier cost, while Courier's $0.005 sits on top of your own carrier bill.


### Does Intercom support transactional email?


Yes, with four caveats. It requires the Proactive Support Plus add-on. All email goes over Intercom's shared IPs, with no dedicated IP option and no way to use your own provider. Intercom documents that event-driven email can be delayed up to 30 minutes under load. And activity-based restrictions can limit sending to contacts active within the last 180 days, which is exactly the wrong constraint for a password reset.


### What happens to my Intercom contacts and custom attributes?


Contacts become Courier user profiles keyed by` user_id` , with the contact's` external_id` as the identifier and channel addresses carried across. Custom attributes become profile attributes, and because profiles accept nested JSON, structures you had to flatten for Intercom can keep their shape. Segments become audiences that recalculate as attributes change.


### Is Intercom being shut down after the Salesforce acquisition?


No, and nothing reported suggests outbound messaging is being discontinued. Focus is the more useful question. Intercom pivoted to AI in 2023, renamed itself after its AI support agent in May 2026, and Salesforce's stated rationale for the $3.6 billion acquisition is folding that agent technology into Agentforce. The deal is signed, not closed, with Salesforce expecting it to complete in the final quarter of its fiscal 2027.


The API shows where the investment has already gone. Intercom's help desk surface is deep and well maintained: conversations, tickets, macros, teams, help center, voice. Outbound has one send endpoint that covers three message types, no push or SMS endpoint, no delivery status, and no campaign API at all. Notifications and lifecycle messaging are not a focus area for a company that has committed itself entirely to AI customer support. Choosing Intercom to send your product's messages for the next three years means betting that changes under new ownership.


## Where to start


If you are evaluating an Intercom alternative, start with the guide to[migrating from Intercom to Courier](https://www.courier.com/docs/tutorials/migrate/from-intercom) , or[create a free account](https://app.courier.com/signup) and send your first message on the free tier before committing to anything.
