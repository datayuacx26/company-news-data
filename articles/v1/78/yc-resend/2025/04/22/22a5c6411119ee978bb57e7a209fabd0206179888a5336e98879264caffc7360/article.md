---
schema_version: "1.0.0"
document_id: "22a5c6411119ee978bb57e7a209fabd0206179888a5336e98879264caffc7360"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/resend-acquires-mergent"
published_at: "2025-04-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:02d19216ca423e5629d1009866f560a8fa1c79c50985fb2a87967d62829da430"
---

# Resend acquires Mergent

Today, we are thrilled to announce that Resend has acquired[Mergent](https://mergent.co/) , the serverless background job service.


Mergent made it easy for developers to schedule and manage tasks like data processing, third-party integrations, and email sending.


Now, we are excited to welcome the Mergent team, and invest even more in three pillars:


1. **Uptime**
2. **Scalability**
3. **Developer Experience**


## The story of Mergent


Four years ago,[James Martinez](https://resend.com/humans/james-martinez) joined the[Y Combinator S21 batch](https://www.ycombinator.com/companies/mergent) to fix the problem of scheduling background tasks.


James' obsession with durability, scalability, and developer experience drove him to build a platform that was super **easy to use, and extremely reliable** .


> Fun fact: Mergent was the first scheduling service our team at Resend used to power domain verifications.


In their first year,[Mergent's 99.997% uptime](https://blog.mergent.co/how-mergent-stacked-up-against-amazon-sqs-sla-in-2022) **raised the bar for an entire industry** by beating AWS SQS's SLA of 99.9% uptime.


In 2023,[Mergent surpassed 99.995% uptime](https://blog.mergent.co/mergents-apis-surpass-99995-uptime-for-the-second-year) . And, in the following year, they surpassed AWS SQS's SLA once again.


Mergent uptime graph


By the end of 2024, Mergent continued to grow, and they added[support for 100 billion invocations per month](https://blog.mergent.co/mergent-now-supports-100b-invocations-per-month) to help large customers.


In total, Mergent has processed more than **370.35 billion tasks** .


## What will happen to Mergent


Now, the Mergent team is joining Resend to continue to improve reliability at scale.


We will continue to support Mergent for existing customers during the **next 90 days** .


Mergent will be sunset on **July 28th, 2025** .


## How to transition from Mergent


If you're using Mergent to schedule emails like this:


```text
{        "scheduled_for"  :     "2025-04-24T19:55:08.623Z"  ,        "request"  :     {          "url"  :     "https://api.resend.com/emails"  ,          "headers"  :     {            "Authorization"  :     "Bearer re_xxxxxxxxx"          }  ,          "body"  :     {            "from"  :     "Acme <onboarding@resend.dev>"  ,            "to"  :     [  "delivered@resend.dev"  ]  ,            "subject"  :     "hello world"  ,            "html"  :     "<p>it works!</p>"  ,            "scheduledAt"  :     "in 1 min"          }        }     }
```


You can now use one of the Resend SDKs to schedule emails using natural language:


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
await   resend  .  emails  .  send  (  {        from  :     'Acme <onboarding@resend.dev>'  ,        to  :     [  'delivered@resend.dev'  ]  ,        subject  :     'hello world'  ,        html  :     '<p>it works!</p>'  ,        scheduledAt  :     'in 1 min'  ,     }  )  ;


```


If you're using Mergent for non-email tasks, there are a few options you can consider:


1. [Inngest](https://www.inngest.com/)
2. [Google Cloud Tasks](https://cloud.google.com/tasks/docs)
3. [AWS Simple Queue Service](https://aws.amazon.com/sqs/)


Each one of these options will come with its pros and cons, but we believe Inngest is the best drop-in replacement to Mergent.


Here's a guide on[how to migrate from Mergent to Inngest](https://innge.st/mergent-migration) .


## What does this mean for Resend


Resend is growing fast, extremely fast.


There is a ton of demand, and we're now sending millions of emails every single day.


Infrastructure is one of our top priorities, and we're incredibly excited to join forces with Mergent to invest even more in it.


We're just getting started.
