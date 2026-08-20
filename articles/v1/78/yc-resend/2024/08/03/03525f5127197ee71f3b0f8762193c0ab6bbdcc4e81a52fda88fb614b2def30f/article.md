---
schema_version: "1.0.0"
document_id: "03525f5127197ee71f3b0f8762193c0ab6bbdcc4e81a52fda88fb614b2def30f"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/introducing-the-schedule-email-api"
published_at: "2024-08-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:507e0a91d9ea395d1511d9fefc8c5ca0c6a052cd82a109dffc61f6db8df5781c"
---

# Introducing the Schedule Email API

While some emails need to be delivered as soon as possible, like password resets or magic links, others can be scheduled for a specific time.


Here are some examples of when you might want to schedule an email:


- Send a welcome email **5 minutes after** signup
- Trigger a reminder email **24 hours before** an event
- Schedule a weekly digest email for the **next day at 9am PST**


Before, you had to use external services to handle the scheduling logic, but now you can use the new Resend API to schedule emails.


## Schedule an email


You can use the various Resend SDKs to schedule emails.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const   oneMinuteFromNow   =     new     Date  (  Date  .  now  (  )     +     1000     *     60  )  .  toISOString  (  )  ;
await   resend  .  emails  .  send  (  {        from  :     'Acme <onboarding@resend.dev>'  ,        to  :     [  'delivered@resend.dev'  ]  ,        subject  :     'hello world'  ,        html  :     '<p>it works!</p>'  ,        scheduledAt  :   oneMinuteFromNow  ,     }  )  ;


```


The date must be in[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format (e.g.,` 2024-08-05T11:52:01.858Z` ).


Emails can be scheduled up to 72 hours in advance.


Once you schedule an email, you can see the scheduled time in the Resend dashboard.


## Reschedule an email


After scheduling an email, you might need to update the scheduled time.


You can do so with the following method:


```text
resend  .  emails  .  update  (  {        id  :     '49a3999c-0ce1-4ea6-ab68-afcd6dc2e794'  ,        scheduledAt  :   oneMinuteFromNow  ,     }  )  ;


```


You can also reschedule an email directly in the Resend dashboard.


## Cancel a scheduled email


If you need to cancel a scheduled email, you can do so with the following code:


```text
resend  .  emails  .  cancel  (  '49a3999c-0ce1-4ea6-ab68-afcd6dc2e794'  )  ;


```


Once an email is canceled, it cannot be rescheduled.


You can also cancel a scheduled email in the Resend dashboard.


## Limitations


There are some limitations to keep in mind when scheduling emails:


- Batch emails cannot be scheduled
- Emails sent via SMTP cannot be scheduled
- Emails with attachments cannot be scheduled


## Get started


We hope this new endpoint makes it easier for you to schedule emails in your application without having to introduce additional complexity.


You can see the[Resend OpenAPI spec](https://github.com/resend/resend-openapi) or the[Postman collection](https://www.postman.com/resend/workspace/resend-api/collection/78558-536fcef8-4f87-42a5-ae1a-57f891ef1404) for all parameters.


Feel free to check the[documentation](https://resend.com/docs/api-reference/emails/send-email) for more details.
