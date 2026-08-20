---
schema_version: "1.0.0"
document_id: "383bf18e7878a6ba230247654ccb4920415eb88b225bff20717b50a89f4703fa"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/inbound-emails"
published_at: "2025-11-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:9e92c1a8bd14697754c1e6ea31b63501955e5294dbaeb00b782cfdc1d15190f5"
---

# Inbound Emails

Resend started by modernizing the way you **send emails** .


Today, we're making it easy for you to **receive emails** as well.


Inbound unlocks entirely new use cases like:


- Replying to in-app emails
- Processing forwarded attachments
- Receiving support emails from users


## How does it work?


First, determine the email address you want to use. You can either:


- Use your team's default inbound domain (e.g.` <your-alias>@<id>.resend.app` )
- [Set up a custom domain](https://resend.com/docs/dashboard/receiving/custom-domains) (e.g.` example.com` )


Diagram of how inbound emails work


Resend processes all incoming emails to the provided address and then:


1. Parses the email content as JSON
2. Stores the attachment file(s)
3. Sends a JSON payload to an endpoint of your choice


## Getting started


Here's how to start receiving emails using Resend.


### 1. Get your email address


To help you get started quickly, Resend provides a` .resend.app` address, which is automatically created for you.


To find your Resend domain:


- Go to the[Receiving Emails](https://resend.com/emails/receiving) page.
- Click the **more options button** and select **Inbound address** .


Send emails to your inbound address and Resend will process them and send a JSON payload to your webhook.


Alternatively, for a more branded experience, you can also[set up a custom domain](https://resend.com/docs/dashboard/receiving/custom-domains) to receive emails.


### 2. Configure webhook


In your application, create a new route that can accept` POST` requests.


Next,[create a new webhook](https://resend.com/docs/dashboard/receiving/introduction#2-configure-webhooks) to receive events.


Enter your endpoint URL and select the` email.received` event.


### 3. Receive email events


Once you receive the email event, you can process the email and attachment metadata.


```text
{        "type"  :     "email.received"  ,        "created_at"  :     "2024-02-22T23:41:12.126Z"  ,        "data"  :     {          "email_id"  :     "56761188-7520-42d8-8898-ff6fc54ce618"  ,          "created_at"  :     "2024-02-22T23:41:11.894719+00:00"  ,          "from"  :     "Acme <onboarding@resend.dev>"  ,          "to"  :     [  "delivered@resend.dev"  ]  ,          "bcc"  :     [  ]  ,          "cc"  :     [  ]  ,          "message_id"  :     "<example+123>"  ,          "subject"  :     "Sending this example"  ,          "attachments"  :     [            {              "id"  :     "2a0c9ce0-3112-4728-976e-47ddcd16a318"  ,              "filename"  :     "avatar.png"  ,              "content_type"  :     "image/png"  ,              "content_disposition"  :     "inline"  ,              "content_id"  :     "img001"            }          ]        }     }
```


## View received emails


All inbound emails are visible on the[emails](https://resend.com/emails/receiving) page. Resend will store your emails even if you don't configure a webhook, or if your webhook endpoint is down.


Emails can be filtered by their` to` ,` from` , and` subject` fields. Click on an email to see its full details, including the HTML, Plain Text, and any attachments.


## Process forwarded attachments


Inbound email flows often involve processing attachments.


Users can forward airplane tickets, receipts, and expenses to you. Then, you can extract key information from attachments and use that data.


To do this, call the[Attachments API](https://resend.com/docs/api-reference/emails/list-received-email-attachments) when you receive the webhook event. That API will return a list of attachments with their metadata and a` download_url` that you can use to download the actual content.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  emails  .  receiving  .  attachments  .  list  (  {        emailId  :     '4ef9a417-02e9-4d39-ad75-9611e0fcc33c'  ,     }  )  ;


```


## Get started today


We're excited to see the new features Inbound will empower you to build.


Visit the[Inbound API docs](https://resend.com/docs/dashboard/receiving/introduction) to get started.
