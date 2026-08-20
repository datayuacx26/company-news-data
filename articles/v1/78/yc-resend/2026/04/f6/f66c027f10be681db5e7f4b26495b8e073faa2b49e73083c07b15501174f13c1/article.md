---
schema_version: "1.0.0"
document_id: "f66c027f10be681db5e7f4b26495b8e073faa2b49e73083c07b15501174f13c1"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/introducing-automations"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:52:42.510574+00:00"
content_hash: "sha256:2fb70a02600cbb70df1f569f95ddc9595a7cc3f3c24bdc10adca85429d1a75a1"
---

# Introducing Automations

Resend makes it easy to **send and receive emails** .


Today, we're making it easy to **orchestrate them** .


**Automations** let you build lifecycle sequences triggered by events in your app:


- Onboard new users with timed sequences
- Re-engage users who've gone quiet
- Send trial expiration reminders


Here's how to create and manage Automations.


## 1. Define a trigger


Every Automation starts with an event.


- User signed up
- Order placed
- Trial expired


You create these events to fit your own needs. Each event can include an optional schema, so you always know what data is available.


## 2. Personalize each step


Once an event fires, you control what happens next:


- **Time delays** : hold the Automation for a set amount of time
- **Wait for events** : move to the next step after a certain condition is met
- **Conditions** : branch the flow based on contact or event data


Design sequences visually with the drag-and-drop editor, or describe what you want in plain language and let AI build it for you.


Conditions support` equals` ,` contains` ,` greater than` , and` and` /` or` logic. Each email step uses your existing[Templates](https://resend.com/blog/introducing-templates) , so copy stays consistent and your team can edit it without touching code.


This personalization can be used to:


- Send a welcome email immediately, then a tips email 3 days later
- Branch based on plan and send different content to free vs. paid users
- Wait for a user to complete onboarding before sending the next message


## 3. Go live with confidence


Before going live, fire a test event and watch it flow through each step.


When you're ready, publish your Automation and start firing events.


## 4. Send events from your app


When something happens in your app, fire the event from your application to trigger the Automation.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  events  .  send  (  {        event  :     'user.created'  ,        email  :     'steve.wozniak@gmail.com'  ,        payload  :     {          plan  :     'pro'  ,        }  ,     }  )  ;


```


As soon as the event is fired, your published Automation runs.


## 5. Full visibility into each run


Once running, the observability panel shows every run in real time: what's in progress, what completed, what failed. Debugging a stuck sequence takes seconds, not hours.


You can also[inspect runs programmatically through the API](https://resend.com/docs/api-reference/automations/list-automation-runs) .


## Developer-first design


We designed Automations to be developer-first. The entire Automation experience is available via the API or through the Dashboard, to let you build in the way that best fits your team.


You can[create Automations](https://resend.com/docs/api-reference/automations/create-automation) ,[define Events](https://resend.com/docs/api-reference/events/create-event) , and[inspect Runs](https://resend.com/docs/api-reference/automations/list-automation-runs) programmatically using the API, our SDKs, the[MCP Server](https://resend.com/docs/mcp-server) , or the[CLI](https://resend.com/docs/cli) .


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  automations  .  create  (  {        name  :     'Welcome series'  ,        status  :     'disabled'  ,        steps  :     [          {            key  :     'start'  ,            type  :     'trigger'  ,            config  :     {     eventName  :     'user.created'     }  ,          }  ,          {            key  :     'welcome'  ,            type  :     'send_email'  ,            config  :     {              template  :     {     id  :     '34a080c9-b17d-4187-ad80-5af20266e535'     }  ,            }  ,          }  ,        ]  ,        connections  :     [  {     from  :     'start'  ,     to  :     'welcome'     }  ]  ,     }  )  ;


```


## Get started today


Automations unlock entirely new ways to engage your audience by building lifecycle sequences triggered by events in your own app.


You can get started with **10,000 Automation Runs for free** , and scale as you need. See[Pricing](https://resend.com/pricing#automations) for all the details.


Visit the[Automations page](https://resend.com/docs/dashboard/automations/introduction) or read the[API docs](https://resend.com/docs/api-reference/automations/create-automation) to build your first Automation.
