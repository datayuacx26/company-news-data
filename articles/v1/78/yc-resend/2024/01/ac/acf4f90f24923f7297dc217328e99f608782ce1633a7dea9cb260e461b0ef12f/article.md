---
schema_version: "1.0.0"
document_id: "acf4f90f24923f7297dc217328e99f608782ce1633a7dea9cb260e461b0ef12f"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/introducing-the-batch-emails-api"
published_at: "2024-01-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:26:19.240569+00:00"
content_hash: "sha256:3a692c0191cc730a8090739626e09f975d30956de2c8e5b10827cd20f8137daa"
---

# Introducing the Batch Emails API

Today, we're reaching GA for Batch Emails. This new endpoint allows you to send up to **100 emails in a single API call** .


We know that many of our customers are sending multiple emails to multiple recipients at once. So, we wanted to make it simple for them to do so without writing custom code, hitting the rate limit, or using a third-party service.


Before, you would need to set up a queueing system to make that work. Now, we handle all the infrastructure and orchestration complexity so you can focus on your product.


Queue System Architecture


## Sending Batch Emails


You can use the various Resend SDKs to send batch emails.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
await   resend  .  batch  .  send  (  [        {          from  :     'Acme <onboarding@resend.dev>'  ,          to  :     [  'foo@gmail.com'  ]  ,          subject  :     'hello world'  ,          html  :     '<h1>it works!</h1>'  ,        }  ,        {          from  :     'Acme <onboarding@resend.dev>'  ,          to  :     [  'bar@outlook.com'  ]  ,          subject  :     'world hello'  ,          html  :     '<p>it works!</p>'  ,        }  ,     ]  )  ;


```


You can check the[Resend OpenAPI spec](https://github.com/resend/resend-openapi) or the[Postman collection](https://www.postman.com/resend/workspace/resend-api/collection/78558-536fcef8-4f87-42a5-ae1a-57f891ef1404) for all parameters.


Note that the` attachments` field is not supported yet.


## Get started


We hope this new endpoint makes it easier for you to send emails to multiple recipients, all while ensuring that every message is delivered to each contact individually.


Keep in mind that the number of emails you can send in a single batch (currently 100) is customizable, so if you have specific needs, please[contact support](https://resend.com/contact) .


You can check the[documentation](https://resend.com/docs/api-reference/emails/send-batch-emails) for more details.
