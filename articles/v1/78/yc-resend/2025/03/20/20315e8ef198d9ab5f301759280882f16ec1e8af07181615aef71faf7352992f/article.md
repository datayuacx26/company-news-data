---
schema_version: "1.0.0"
document_id: "20315e8ef198d9ab5f301759280882f16ec1e8af07181615aef71faf7352992f"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/broadcast-api"
published_at: "2025-03-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:73fcf8b41f6abc9b9200dedd1b585082d7a43dbac8701fd4bdaaa339348d07b7"
---

# Broadcast API

When we first launched our no-code email editor,[Broadcasts](https://resend.com/features/broadcasts) , we focused on building the best user experience. We introduced familiar modern editor patterns and have continued adding new components ([Section](https://resend.com/changelog/new-section-component) ,[Code Block](https://resend.com/changelog/new-code-block-component) ,[YouTube](https://resend.com/changelog/new-youtube-component) , and[𝕏](https://resend.com/changelog/new-x-twitter-component) ) and details.


Behind Broadcasts lies a quietly powerful system that can queue, throttle, and send millions of emails, gather and display engagement metrics, and more.


## Enabling developers


Today, we're excited to expose the **full broadcast experience to developers** . Send HTML, plain text, or React (with the Node.js SDK) to any audience.


## Available in all languages


As with all endpoints, the Broadcast API enjoys **full SDK coverage** .


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
await   resend  .  broadcasts  .  create  (  {        audienceId  :     '78261eea-8f8b-4381-83c6-79fa7120f1cf'  ,        from  :     'Acme <onboarding@resend.dev>'  ,        subject  :     'hello world'  ,        html  :     'Hi {{{FIRST_NAME|there}}}, you can unsubscribe here: {{{RESEND_UNSUBSCRIBE_URL}}}'  ,     }  )  ;


```


The Broadcast API offers[6 endpoints](https://resend.com/docs/api-reference/broadcasts/create-broadcast) for creating, updating, and sending broadcasts.


## Review in the no-code editor


All broadcasts created with the API can be reviewed in the no-code editor before sending.


## Building broadcast integrations


Full API coverage means you can build integrations with your product or web apps:


- Enable your users to create and send broadcasts without leaving your product
- Generate broadcasts programmatically for internal use
- Trigger broadcasts based on events in your app


You can build and send broadcasts with the API and let the Broadcast infrastructure handle the queue, throttle, and scheduling implementation.


## Audience management


Just like the visual editor, the Broadcast API uses[Audiences](https://resend.com/docs/api-reference/audiences/create-audience) to manage your recipients.


[Audience endpoints](https://resend.com/docs/api-reference/audiences/create-audience) let you control audiences and contacts programmatically.


When creating broadcasts, you can include dynamic audience data to personalize the email content.


- ` {{{FIRST_NAME|fallback}}}`
- ` {{{LAST_NAME|fallback}}}`
- ` {{{EMAIL}}}`
- ` {{{RESEND_UNSUBSCRIBE_URL}}}`


When you include the` {{{RESEND_UNSUBSCRIBE_URL}}}` placeholder in the call, Resend includes an unsubscribe link in the email to automatically handle unsubscribe requests.


## Limitations


Broadcasts created with the API are distinct from those created in the visual editor. While you can view all Broadcasts using the API or the visual editor, you can only edit and send broadcasts from the location where they were created.


## Get started today


We're excited to see what you'll create and send.


Visit the[Broadcasts API docs](https://resend.com/docs/api-reference/broadcasts/create-broadcast) to get started.
