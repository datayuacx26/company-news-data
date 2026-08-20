---
schema_version: "1.0.0"
document_id: "ce6c376c7ed6069a8fe3e84c31f0c2e2e90aa6d1b7066e9efef1b292210b2290"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/webhooks"
published_at: "2023-03-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:0d533d6fecf250e77a368850191f0a1175a7d24bfd5b886892c5bef897faceb3"
---

# Capture email events with Webhooks

We’re excited to announce one of our most requested features: Webhooks.


## What is a Webhook?


A webhook is a way for one application to provide other applications with real-time information. In our case, we send a notification to your application every time an email event occurs. This allows you to receive real-time and actionable updates on the delivery, open, bounce, and click events of your emails.


> "Working with Resend has been amazing. By using Webhooks, I'm able to track email opened/clicked events via Segment and log those events in LogSnag for visibility. I highly believe in the people behind Resend."
>
>
> Taylor Facen
>
>
> Founder of Finta


## What can you build with Webhooks?


Webhooks use HTTPS and deliver a JSON payload which includes metadata like the from, to, and status of your email event. You can use email webhooks to automate your workflows, like:


- Automatically remove bounced email addresses from mailing lists
- Create alerts in your messaging or incident tools based on event types
- Store all send events in your own database for custom reporting/retention
- Expose email events in your app


## Setting up


For how powerful webhooks are, they could not be more straightforward to get started.


> "Resend is super easy to set up. Loving the modern approach the team is taking with supercharging email. Never been a fan of other clunky tools."
>
>
> Brek Goin
>
>
> Founder of Hammr


First, create an endpoint to receive` POST` requests (or use a tool like[Zapier](https://zapier.com/features/webhooks) ).


For example, you can add an API route on Next.js:


```text
import   type   {   NextApiRequest  ,   NextApiResponse   }     from     'next'  ;
export     default     (  req  :   NextApiRequest  ,     res  :   NextApiResponse  )     =>     {        if     (  req  .  method   ===     'POST'  )     {          const   payload   =   req  .  body  ;        console  .  log  (  payload  )  ;        res  .  status  (  200  )  ;        }     }  ;
```


Then[register your webhook](https://resend.com/webhooks) endpoint on Resend.


Resend Webhooks


Now every time an email event occurs, Resend will send a` POST` request to your endpoint with the event payload.


## Check the full example


We built a[GitHub repository](https://github.com/resend/resend-examples/tree/main/with-webhooks) that shows a Slack notification being triggered from Resend webhooks using Next.js and TypeScript.


Slack Webhook Example


## What’s next?


We’re excited to see what you build with webhooks. For more information on each event type, retry schedule, and delivery attempts, check the[Webhooks docs](https://resend.com/docs/webhooks) .
