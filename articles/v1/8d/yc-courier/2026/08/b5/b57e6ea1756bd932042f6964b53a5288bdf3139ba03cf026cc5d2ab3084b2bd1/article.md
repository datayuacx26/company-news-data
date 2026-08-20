---
schema_version: "1.0.0"
document_id: "b57e6ea1756bd932042f6964b53a5288bdf3139ba03cf026cc5d2ab3084b2bd1"
company_key: "yc-courier"
company: "Courier"
source_id: "yc-courier-news-import-df9818472bef"
canonical_url: "https://www.courier.com/blog/product-notifications-api"
published_at: null
first_seen_at: "2026-08-18T05:15:40.729814+00:00"
fetched_at: "2026-08-18T05:15:41.892507+00:00"
content_hash: "sha256:6bb756cdb0eda62b9c96255305cd4ab63ef7370aa15a0b3aa4eb70d548dd83f7"
---

# Product notifications API: what it handles and how to build

## Key takeaways


**A product notifications API is a single endpoint you send product events to, which then decides what message gets sent, to whom, on which channel, at what time, with that user's preferences already applied.** It sits between your application and your delivery providers.


It has ten jobs: every channel from one call, an in-app notification center, mobile push, preferences enforced at send time, one identity per user, transactional sends and journeys on one system, fatigue controls, copy changes without a deploy, one delivery log, and per-tenant scoping if you sell to companies.


With Courier, you post your event feed to one endpoint and Courier handles routing, preferences, timing, rendering, and delivery across email, SMS, push, Slack, Microsoft Teams, and a drop-in in-app inbox. Courier's free tier covers 10,000 sends a month, and paid usage is $0.005 per send with no per-seat or per-channel fees.


*Last updated: August 17, 2026.*


---


## What a product notifications API is


A **product notifications API** turns "this happened in the product" into "this person got told, on a channel they agreed to."


Most teams already own two of the three layers below: a delivery provider for transactional mail, and a marketing tool for campaigns. The product notifications API is the middle one, and the question worth answering is whether you need it when you already pay for the other two.


Delivery provider (SendGrid, Twilio, FCM) Product notifications API Marketing automation (Customer.io, Braze)


What you send it A recipient, a channel, and finished content An event with a user ID in it A segment and a campaign


What it decides Nothing. You already decided Which channel, when, and whether to send at all Who enters a campaign, and when


Preferences Per channel, siloed from each other Stored once, updated as needed, enforced on every send Marketing consent and unsubscribe only


Transactional and lifecycle Delivers transactional, one channel per integration Both, on the same profiles and templates Lifecycle is its core job. Not built for password resets


Who operates it Engineering Engineering, product, and growth marketing Marketing


The outer two columns aren't missing preference tooling. SendGrid has[unsubscribe groups](https://www.twilio.com/docs/sendgrid/ui/sending-email/unsubscribe-groups) , and Twilio[handles STOP, UNSUBSCRIBE, CANCEL, and five other keywords](https://www.twilio.com/docs/messaging/tutorials/advanced-opt-out) on long codes by default. The problem is that neither knows the other exists: a user who texts STOP is opted out at Twilio and still fully subscribed at SendGrid, and nothing reconciles that except code you write. That coordination is what the middle column is actually for.


### What changes in your code


Without a notification layer, your application code makes every decision:


```text
// Someone was mentioned in a comment. Now what?      if     (  user  .  emailOptIn     &&     !  inQuietHours  (  user  )  )     {         await   sendgrid  .  send  (  buildEmail  (  user  ,   comment  )  )  ;      }      if     (  user  .  pushToken     &&   user  .  pushOptIn  )     {         await   fcm  .  send  (  user  .  pushToken  ,     buildPush  (  comment  )  )  ;      }      await   db  .  notifications  .  insert  (  {   userId  :   user  .  id  ,   body  :     buildInApp  (  comment  )     }  )  ;
// Still unhandled: what if the email bounced? Did we already send this      // person 11 other mention alerts in the last five minutes? What time      // is it where they live? Which of these three did they actually get?
```


With one, your code states what happened and stops there:


```text
await   courier  .  send  .  message  (  {       message  :     {         to  :     {   user_id  :   user  .  id     }  ,         template  :     "comment-mention"  ,         data  :     {   comment_body  :   comment  .  body  ,   author  :   comment  .  author  .  name     }         }      }  )  ;
```


Every decision from the first block still gets made. Channel choice, opt-outs, quiet hours, deduplication, retries, and the record of what was delivered. They stop being your code's problem.


## Why product notifications matter


Everything you ship is invisible to someone who isn't looking at your app. If a task got assigned, a build failed, or a teammate left a comment, the notification *is* the feature. And the messages nobody notices are the ones people trust you for: a verification code four minutes late is a signup that didn't happen, and a failed-payment notice in spam is a customer who thinks you cancelled them.


The counterweight matters as much, because it sets up the requirements below. Notifications are also the fastest way to make someone resent your product. Five emails for five comments on one document. A push at 3 a.m. in the user's real time zone. Every one of those is a system failure rather than a copy problem, which is why preferences and fatigue controls are requirements and not polish.


## Ten things a product notifications API has to handle


Ten requirements, in three groups: getting a message out, deciding what to send, and living with the thing after launch. Build instead of buy and you own all ten, plus their edge cases, indefinitely.


### Getting a message out


**1. Every channel from one call.** Email, SMS, Slack, Microsoft Teams, in-app, and webhooks, plus mobile push if you have an app. One payload describing the notification, not one integration per channel with its own auth, retry semantics, rate limits, and error shapes. Two channels means a routing decision. Three means that decision needs to live in one place instead of scattered across your services.


**2. An[in-app notification center](https://www.courier.com/solutions/in-app-channel) .** A real-time feed inside your product with unread counts, read and archive state, and sync across a user's devices and tabs. An in-app feed is the channel users check on their own terms, and the one teams consistently underestimate. It is not a list endpoint. It is a persistent connection, a per-user message store, read-state reconciliation across sessions, pagination, and a UI that matches your design system.


**3. Mobile push, including the token lifecycle.**[APNs](https://developer.apple.com/documentation/usernotifications) for iOS and[FCM](https://firebase.google.com/docs/cloud-messaging) for Android, plus the part nobody scopes: registering tokens, mapping several devices to one user, invalidating dead tokens after an uninstall, and interpreting the silent failures both services return. Courier covers this as[multi-channel routing](https://www.courier.com/platform/multi-channel-routing) across providers.


### Deciding what to send, and when


**4.[Preferences](https://www.courier.com/solutions/user-preferences) enforced at send time.** Per-channel and per-topic opt-outs, quiet hours in the user's time zone, and digest schedules. The load-bearing word is *enforced* . If preferences are a table your application code is supposed to consult, some caller will forget, and that caller will be the one sending at 2 a.m. Preferences must be applied by whatever sends, not by everything that asks it to send.


**5. One identity per user.** A single profile holding email, phone number, device tokens, Slack ID, locale, and time zone, so a send call takes a user ID and the system resolves how to reach them. Passing contact methods into every send means every service in your stack needs to know how to contact a user, and each will have a slightly different answer.


**6.[Transactional sends](https://www.courier.com/solutions/transactional-notifications) and[journeys](https://www.courier.com/solutions/customer-journeys) running on one system.** A receipt is a send with no delay. An onboarding sequence is a send with a three-day delay and a branch on whether the user did the thing. There is no architectural difference between them worth splitting across two vendors, and splitting them is how you end up unable to answer "what did we send this person last week."


**7. Fatigue controls built into the system.** Batching a burst of events into one message, throttling how often a user can hit the same notification, rolling activity into a daily or weekly digest, and capping total volume per user. These have to be settings the system applies on its own. A rule someone remembers to add to a code path is not a fatigue control, because the one time it gets forgotten is the send that annoys everyone.


### Living with it after launch


**8. Copy changes that don't need a deploy.** Whoever writes the copy should be able to ship the copy. When templates live in your repo, fixing one word in a subject line takes a pull request, a review, and a deploy, so it competes with feature work and loses. Nobody decides to let notifications go stale. They go stale because filing the ticket costs more than the fix is worth.


**9. One delivery log across every provider.** Per-message status for every channel: queued, sent, delivered, opened, clicked, bounced, failed, and why. The test is how long it takes to answer "did this user get their receipt?" With one log it's a search. With logs spread across four vendor dashboards it's an investigation, and support is doing it while the customer waits.


**10. Per-tenant scoping, if you sell to companies.** Per-tenant branding, template overrides, and preference scoping, so one customer's admin can disable a notification type for their whole organization without affecting anyone else. Retrofitting tenant scoping into a system built single-tenant is one of the more painful migrations in this category.


## Why engineering, product, and growth marketing need the same system


Most comparisons of notification tools miss this. A product notifications API is not only developer infrastructure. Three groups need to reach the same users, and they need to do it from the same profiles, the same preferences, and the same event feed.


Team What they need What the API gives them


Engineering An event contract and an SDK, with no channel logic in the application One endpoint, typed SDKs, provider abstraction, and per-message delivery logs


Product To change what gets sent and when, without shipping a deploy A visual journey editor and a template editor reading the same data the code sends


Growth To run onboarding, activation, and retention against real product behavior The same event feed, with product events as triggers instead of email engagement


Growth is the row that gets stranded. Give that team a marketing tool that can't see product events and it will target on the only signal it has, which is email engagement. That's how you get a win-back campaign aimed at someone who logged in an hour ago.


What makes one system work for all three is parity between the UI and the API. If the visual editor does less than the API does, engineering builds around it and product is locked out again, which is the situation you were trying to leave.


## What building notification infrastructure yourself costs


You can build all ten. Teams do. The build is rarely what they regret.


What they regret is the bill that arrives afterward, and it arrives in places the original ticket never mentioned:


The part nobody scoped Why it keeps costing you


Durable delays A three-day delay has to survive deploys, restarts, and cancellations, and stay correct when the user does the thing on day two


Device tokens Users have three devices, uninstall on one, and APNs and FCM report that failure differently, sometimes silently


Preference migrations Every new notification type you add needs a schema change and a backfill for existing users


One timeline from many providers Joining SendGrid's event schema to Twilio's to Firebase's into a per-user history is a data engineering project unrelated to your product


Provider churn Every vendor API version bump, every failover path, every new channel someone asks for


Read state An inbox that agrees with itself across a laptop, a phone, and four open tabs


Two of those are where I'd expect a build to actually come apart. **Durable delays** , because a cron job over a jobs table looks correct until the first deploy mid-delay, and you tend to learn that in production. And **the unified timeline** , because it's the thing you need most on the day a customer asks whether they got their receipt, and it's the thing you'll have built least.


**Build it yourself if** you have one channel, one provider, no preference management, no in-app feed, and no plan to add any of them. A CLI that emails errors doesn't need infrastructure. A product with users does.


## How the main notification platforms compare


Six platforms cover most shortlists. The last column is the one to read, because that's where each one stops fitting:


Platform What it is Strongest fit Where it stops fitting


[Courier](https://www.courier.com/platform/notification-infrastructure) Multi-channel notification API plus lifecycle journeys in one platform Teams running transactional and lifecycle messages off the same event feed, with a drop-in inbox and preferences included More platform than a single-channel app needs on day one


Novu Open-source notification infrastructure, self-hostable Teams with a compliance or control requirement to run it themselves You operate it, including scaling and upgrades


Customer.io Product-led lifecycle marketing with published entry pricing Mid-market teams whose messaging is campaigns driven by event data Built around profiles and campaigns rather than product events. No in-app feed, and not a fit for transactional sends


Iterable Enterprise cross-channel marketing automation Large marketing organizations running high-volume campaigns across many channels Quote-only with no free tier, so evaluating it means a sales cycle. Not built for transactional sends or an in-app feed


OneSignal Push-first engagement platform Mobile and web push at high volume, with segmentation and analytics Not built around product events or per-topic preferences


Firebase Cloud Messaging Push transport from Google, free Raw Android, iOS, and web push delivery Transport only. Preferences, workflows, templates, and logs are yours to build


For a fuller breakdown including pricing and developer experience, see Courier's[notification infrastructure comparison](https://www.courier.com/blog/best-notification-infrastructure-software) . For push specifically, the[push platform roundup](https://www.courier.com/blog/top-push-notification-platforms) compares the main options and the[push provider comparison](https://www.courier.com/blog/top-push-notification-providers) goes deeper on delivery and reach.


## How to build product notifications on Courier


The architecture in one sentence: **you send your product's events to Courier, and Courier decides what message gets sent, to whom, on which channel, at what time, with that user's preferences applied.**


```text
Your app       |       |  events (user.mentioned, payment.failed, export.ready, trial.day_three)       v    Courier       |-- Send API      -> immediate transactional messages       |-- Journeys      -> delayed, branching, multi-step sequences       |       |-- applies preferences, quiet hours, throttles, batches, and digests       |-- renders per channel from one template       v    Email   SMS   Push   In-app inbox   Slack   Teams   Webhook       |       v    One delivery log
```


Six steps, and the first two are enough to send real messages.


### 1. Identify each user once


Create a profile holding every way to reach the user. Do this at signup and on profile updates, then stop thinking about contact methods.


```text
import     Courier     from     "@trycourier/courier"  ;
const   courier   =     new     Courier  (  {   apiKey  :   process  .  env  .  COURIER_API_KEY     }  )  ;
await   courier  .  profiles  .  replace  (  "user_8fa2"  ,     {       profile  :     {         email  :     "maya@acme.com"  ,         phone_number  :     "+15558675309"  ,         locale  :     "en-US"  ,         timezone  :     "America/Denver"  ,         slack  :     {   user_id  :     "U024BE7LH"     }         }      }  )  ;
```


Push device tokens register separately from your mobile client, so a user with three devices resolves to three tokens under one ID. Courier's[user management docs](https://www.courier.com/docs/platform/users/users-overview) cover lists, audiences, and tenant scoping.


### 2. Send the immediate messages directly


For anything transactional, one call to Courier's Send API does routing, rendering, preference enforcement, and delivery.


```text
const     {   requestId   }     =     await   courier  .  send  .  message  (  {       message  :     {         to  :     {   user_id  :     "user_8fa2"     }  ,         template  :     "export-ready"  ,         data  :     {           file_name  :     "q3-pipeline.csv"  ,           download_url  :     "https://app.acme.com/exports/9042"           }  ,         routing  :     {           method  :     "single"  ,           channels  :     [  "inbox"  ,     "push"  ,     "email"  ]           }         }      }  )  ;
```


` method: "single"` walks the channel list in order and stops at the first channel that can reach the user, so an active user gets it in the in-app inbox and an absent one gets an email.` method: "all"` sends everywhere. Courier's[Send API reference](https://www.courier.com/docs/platform/sending/send-message) covers content, attachments, and per-channel overrides, and[choosing a sending strategy](https://www.courier.com/docs/platform/sending/choosing-your-sending-strategy) covers when a journey is the better call.


The same request in cURL, since that's usually what gets tested first:


```text
curl   -X POST https://api.courier.com/send   \       -H   "Authorization: Bearer   $COURIER_API_KEY  "     \       -H   "Content-Type: application/json"     \       -d   '{        "message": {          "to": { "user_id": "user_8fa2" },          "template": "export-ready",          "data": { "file_name": "q3-pipeline.csv" },          "routing": { "method": "single", "channels": ["inbox", "push", "email"] }        }      }'
```


### 3. Point your event feed at one endpoint


This is the step that changes how much code you write. Register an inbound webhook in Courier under Settings, and Courier generates a URL of the form` https://api.courier.com/inbound/webhook/<token>` . Post your product events to it. No auth header is needed, because the token in the URL identifies your workspace, and Courier accepts payloads up to 6 MB.


```text
curl   -X POST https://api.courier.com/inbound/webhook/YOUR_WEBHOOK_TOKEN   \       -H   "Content-Type: application/json"     \       -d   '{        "event": "trial.day_three_no_project",        "userId": "user_8fa2",        "properties": {          "plan": "trial",          "projects_created": 0,          "days_remaining": 11        }      }'
```


Treat that URL as a secret. Anyone holding it can post events into your workspace.


If you already run Twilio Segment, skip the webhook. Courier consumes your Segment event stream directly as a journey trigger, so the events you're already tracking become your notification triggers with no new instrumentation. Full setup in Courier's[inbound webhooks docs](https://www.courier.com/docs/platform/workspaces/inbound-webhooks) .


### 4. Trigger journeys from that same feed


A Courier[journey](https://www.courier.com/docs/platform/journeys/journeys-overview) is a visual workflow that starts from a trigger and runs nodes in sequence. Courier supports four trigger types:


- **API invoke** , when your code posts to the journey's invoke endpoint
- **Webhook** , when an event arrives on one of your inbound webhooks
- **Twilio Segment** , when a matching event arrives from your Segment stream
- **Audience** , when a user joins an audience


So the same` trial.day_three_no_project` event that hits your webhook can start an onboarding journey. Or invoke one explicitly:


```text
curl   -X POST https://api.courier.com/journeys/JOURNEY_ID/invoke   \       -H   "Authorization: Bearer   $COURIER_API_KEY  "     \       -H   "Content-Type: application/json"     \       -d   '{        "user_id": "user_8fa2",        "data": { "plan": "trial", "projects_created": 0, "days_remaining": 11 }      }'
```


From the Node SDK:


```text
const     {   runId   }     =     await   courier  .  journeys  .  invoke  (  "JOURNEY_ID"  ,     {       user_id  :     "user_8fa2"  ,       data  :     {   plan  :     "trial"  ,   projects_created  :     0  ,   days_remaining  :     11     }      }  )  ;
```


That` runId` identifies the individual execution. Inside a journey, the nodes that matter for product notifications:


- [Delay](https://www.courier.com/docs/platform/journeys/nodes/delay) pauses for a duration or until a specific time
- [Branch](https://www.courier.com/docs/platform/journeys/nodes/branch) splits on trigger data, profile fields, or upstream results
- [Fetch Data](https://www.courier.com/docs/platform/journeys/nodes/fetch-data) calls your API mid-run, so a nudge can check whether the user already did the thing
- [Throttle](https://www.courier.com/docs/platform/journeys/nodes/throttle) caps how often a user passes a given point
- [Batch](https://www.courier.com/docs/platform/journeys/nodes/batch) collects a burst of events and releases them as one payload
- [Add to Digest](https://www.courier.com/docs/platform/journeys/nodes/digest) accumulates events per user against a subscription topic and releases them on a schedule
- [AI](https://www.courier.com/docs/platform/journeys/nodes/ai) runs an LLM prompt inside the run to classify or generate structured data that downstream nodes branch on


Batch and digest are what turn "12 emails about one document" into one message. That's the fatigue-control requirement solved by configuration instead of by a scheduler you maintain.


When a send looks wrong, Courier's[run inspection](https://www.courier.com/docs/platform/journeys/run-inspection) steps through one user's execution node by node against real production data, showing payloads, branch decisions, and delivery status.


### 5. Drop in the in-app notification center


The in-app inbox is a component, not a project. Your backend mints a JWT for the signed-in user, and Courier's SDK handles the connection and read state.


```text
import     {   useEffect   }     from     "react"  ;      import     {   CourierInbox  ,   useCourier   }     from     "@trycourier/courier-react"  ;
function     Notifications  (  {   userId  ,   jwt   }  )     {         const   courier   =     useCourier  (  )  ;
useEffect  (  (  )     =>     {         courier  .  shared  .  signIn  (  {   userId  ,   jwt   }  )  ;         }  ,     [  userId  ,   jwt  ]  )  ;
return     <  CourierInbox     />  ;      }
```


Anything sent to the` inbox` channel appears in real time, with unread counts and archive state synced across the user's devices.[Toasts](https://www.courier.com/docs/platform/inbox/notify-with-toasts) share the same connection for in-session pop-ups. Courier publishes Inbox SDKs for React, Vue, Angular, web components, React Native, Flutter, iOS, and Android. Start at the[inbox overview](https://www.courier.com/docs/platform/inbox/inbox-overview) , or see what[Courier Inbox](https://www.courier.com/platform/inbox) covers as a product.


### 6. Ship preferences alongside it


Same SDK, same session:


```text
import     {   CourierPreferences   }     from     "@trycourier/courier-react"  ;
function     PreferencePage  (  )     {         return     <  CourierPreferences     />  ;      }
```


That renders subscription topics, channel selection, and digest schedules. Non-React apps use the` <courier-preferences>` web component, and Courier's[embedding guide](https://www.courier.com/docs/platform/preferences/embedding-preferences) covers headless hooks if you want to build the UI yourself. There's also a[hosted preference center](https://www.courier.com/docs/platform/preferences/hosted-page) if you'd rather not build the page, which is what email footers should point at.


The UI is not the important part. What matters is that Courier applies[preferences](https://www.courier.com/docs/platform/preferences/preferences-overview) at send time. Every send passes through them, including sends written by someone who has never read your preferences schema.


### Who owns what


Concern Your responsibility Courier's responsibility


Product events Emit them Receive, filter, and route them


User records Source of truth in your database Profile with contact methods, locale, and time zone


Client SDK auth Mint JWTs on your backend Verify them and scope the session


Which notifications exist Decide them Store and version the templates


Channel intent Set the routing strategy Resolve it against reachability and preferences


Copy and design Edit in[Design Studio](https://www.courier.com/docs/platform/content/design-studio/design-studio-overview) , no deploy Render per channel


Timing Configure delays and send windows Execute them durably across deploys and restarts


Fatigue rules Configure batch, throttle, and digest Enforce them


Preferences Expose the UI Store them and enforce at send time


Provider accounts Bring your own, or use Courier's Auth, retries, failover, and normalized errors


Delivery status Read[message logs](https://www.courier.com/docs/platform/analytics/message-logs) or subscribe to outbound webhooks Track every message across every provider


Per-customer branding Define tenants Scope templates, brands, and preferences per[tenant](https://www.courier.com/docs/platform/tenants/tenants-overview)


Short version: you own your events and your user data. Courier owns everything between the event and the delivered message.


## How AI coding agents speed up a Courier integration


Your agent can write this integration faster than you can scope it. What slows an agent down is not writing code. It's not knowing which payload shape an endpoint accepts, which field is required, and what a generic` Invalid input` actually means. Each unknown becomes a guess, and each guess costs a round trip.


Courier ships four things for this, and they do different jobs.


**Courier Skills, for knowledge.** An open-source agent skill that teaches your assistant the right primitive for each use case, the exact payload shapes, and the mistakes that produce unhelpful errors. Every reference file opens with rules and common mistakes before any explanation, because a stuck agent searches before it reads. The error strings Courier's API returns are indexed back to their causes, so an agent can search with the string it already has.


```text
npx skills   add   trycourier/courier-skills
```


No API key and no config file. It works with Claude Code, Cursor, Codex, GitHub Copilot, Gemini CLI, Windsurf, Amp, Zed, Cline, and OpenCode, and the installer detects what you have. In Claude Code you can install it as a self-updating plugin instead:


```text
/plugin marketplace   add   trycourier/courier-skills    /plugin   install   courier@courier-skills
```


**The MCP server, for actions.** Courier runs a hosted[MCP server](https://www.courier.com/docs/tools/mcp) at` https://mcp.courier.com` with no local setup. It exposes the Courier API as typed tools, so while your agent writes the integration it can also send a test message, read the delivery log, check a user's preferences, and publish a template.


```text
claude mcp   add   --transport http courier https://mcp.courier.com --header api_key:YOUR_COURIER_API_KEY
```


For Cursor, Windsurf, VS Code, or the OpenAI Responses API, point the client at the same URL with your key in the` api_key` header.


**Docs built for agents, for grounding.** Courier publishes machine-readable documentation indexes at[llms.txt](https://www.courier.com/docs/llms.txt) and` llms-full.txt` , so an agent can discover every page and endpoint with no configuration. The[agent quickstart](https://www.courier.com/docs/tools/agent-quickstart) is the shortest path from an empty workspace to a sent email and a working inbox.


**The CLI, for automation.**` npm install -g @trycourier/cli` gives shell access to the same API for seeding test data, publishing templates from a pipeline, or debugging without a browser. See the[CLI docs](https://www.courier.com/docs/tools/cli) .


Pick by job rather than installing all four. Skills teach, MCP acts, docs ground, the CLI automates. Most teams want skills plus MCP, and Courier's[Build with AI](https://www.courier.com/docs/tools/ai-onboarding) page has setup for whichever agent you run.


## What Courier costs


Courier's Developer plan is free and includes 10,000 sends per month, journeys, broadcasts, the MCP server, the CLI, and the SDKs. The Business plan is pay-as-you-go at $0.005 per send with no per-seat or per-channel fees, and adds AI translations and the AI journey node. Enterprise covers EU data residency, role-based access control, observability integrations, and an SLA. Current numbers are on Courier's[pricing page](https://www.courier.com/pricing) .


One note for comparisons, because this is where most pricing analysis goes wrong: per-send pricing behaves nothing like per-monthly-active-user pricing. A product sending two notifications per user per month pays very little per send and a lot per MAU. A product sending fifty flips that completely. Model your own sends-per-user ratio before comparing sticker prices, because the pricing model matters more than the unit rate.


## Frequently asked questions


### What is a product notifications API?


A product notifications API is a single endpoint you send product events to, which resolves each event into messages on the right channels for the right user at the right time. It handles channel routing, template rendering, preference enforcement, timing, provider delivery, and logging, so your application code emits an event instead of calling SendGrid, Twilio, and Firebase separately.


### Do I need a notification API if I only send email?


Probably not yet. One channel with one provider and no preference management is a case where your provider's own SDK is enough. The calculation changes as soon as you add a second channel, an in-app feed, or per-topic preferences, because that is when routing and preference enforcement need somewhere central to live.


### Can one API handle both transactional and marketing messages?


Yes, and it should. They are the same send with different timing and different consent rules. Running them in separate tools produces two copies of your user profiles, two preference stores that will eventually disagree, and two delivery logs you have to join by hand to answer basic questions about what a user received.


### How long does it take to add an in-app notification center?


Building one from scratch is typically a multi-week project once you account for real-time sync, read state across devices, and pagination. With a drop-in SDK it's a JWT endpoint on your backend plus a component in your frontend, so a working feed in a day is realistic.


### Should I build or buy notification infrastructure?


Build if you have one channel, one provider, no preferences, no in-app feed, and no plan to add them. Buy otherwise. The initial build is not the expensive part. Durable delayed sends, device token lifecycle, preference migrations, provider failover, and a unified delivery log are what keep costing you after launch.


### Can AI agents send notifications through the Courier API?


Yes. Courier's hosted MCP server at` https://mcp.courier.com` exposes the API as typed tools, so an agent can send messages, manage profiles, read delivery logs, and publish templates directly. Courier Skills, installed with` npx skills add trycourier/courier-skills` , gives the same agent the payload shapes and failure modes it needs to write the integration code correctly.


### What channels does Courier support?


Email, SMS, mobile and web push, in-app inbox, Slack, Microsoft Teams, and webhooks, across 50+ provider integrations. Courier abstracts the provider, so switching from one email or SMS vendor to another is a configuration change rather than a code change.


## Next steps


Send your first message with Courier's[quickstart](https://www.courier.com/docs/getting-started/quickstart) , which takes one API call and no dashboard setup. The free Developer plan covers 10,000 sends a month, so you can wire up a real event feed before talking to anyone.


Teach your agent the platform first:


```text
npx skills   add   trycourier/courier-skills
```


Then hand it the[agent quickstart](https://www.courier.com/docs/tools/agent-quickstart) and let it wire up your first event.


If you'd rather see the platform side first,[Courier for SaaS](https://www.courier.com/solutions/saas) covers how product, engineering, and growth teams share one notification system.
