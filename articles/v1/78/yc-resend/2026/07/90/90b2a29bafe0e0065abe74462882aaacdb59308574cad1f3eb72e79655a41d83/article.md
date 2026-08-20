---
schema_version: "1.0.0"
document_id: "90b2a29bafe0e0065abe74462882aaacdb59308574cad1f3eb72e79655a41d83"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/message-id-for-sent-emails"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:e965236e408a12edd198d155b4f4c557179348fe9bd17b44321937c3572f2b03"
---

# Email Threading with Message-ID

Every email carries a` Message-ID` header, the unique identifier that mail clients and servers use to recognize a message and group conversations into threads.


Starting today, we're including` message_id` in **all email webhooks** and in the **retrieve and list email endpoints** . This has been one of our most requested features, and it unlocks new use cases without any new primitives.


## What you can build with it


- **Threaded replies** : pass the` message_id` as the` In-Reply-To` and` References` headers on your next send, so Gmail, Outlook, and Apple Mail group it into the same conversation.
- **Reply matching** : when an inbound reply arrives, its headers reference the original Message-ID, so you can link the reply back to the exact email that started the thread.
- **Cross-system correlation** : reconcile emails sent through Resend with mail server logs, support tools, or your own database using an identifier the whole email ecosystem understands.
- **Deliverability investigations** : when working with a recipient's mail admin, the Message-ID is how they find the message on their side.


## Included in every email webhook


All` email.*` webhook events now include` message_id` in the payload. This includes` email.sent` , which fires as soon as the email is accepted for delivery, so you can capture the Message-ID immediately.


```text
{        "type"  :     "email.sent"  ,        "created_at"  :     "2026-07-08T23:41:12.126Z"  ,        "data"  :     {          "email_id"  :     "56761188-7520-42d8-8898-ff6fc54ce618"  ,             "message_id"  :     "<111-222-333@email.example.com>"  ,          "created_at"  :     "2026-07-08T23:41:11.894719+00:00"  ,          "from"  :     "Acme <onboarding@resend.dev>"  ,          "to"  :     [  "delivered@resend.dev"  ]  ,          "subject"  :     "Sending this example"  ,          "tags"  :     {            "category"  :     "confirm_email"          }        }     }
```


## Included when retrieving emails


The[GET /emails/:email_id](https://resend.com/docs/api-reference/emails/retrieve-email) endpoint now returns` message_id` for every sent email, so you can look it up at any time, not just when a webhook fires. The[GET /emails](https://resend.com/docs/api-reference/emails/list-emails) list endpoint returns it for every email in the page too, so you can backfill Message-IDs in bulk.


```text
{        "object"  :     "email"  ,        "id"  :     "4ef9a417-02e9-4d39-ad75-9611e0fcc33c"  ,           "message_id"  :     "<111-222-333@email.example.com>"  ,        "to"  :     [  "delivered@resend.dev"  ]  ,        "from"  :     "Acme <onboarding@resend.dev>"  ,        "created_at"  :     "2026-04-03 22:13:42.674981+00"  ,        "subject"  :     "Hello World"  ,        "html"  :     "Congrats on sending your <strong>first email</strong>!"  ,        "text"  :     null  ,        "bcc"  :     [  ]  ,        "cc"  :     [  ]  ,        "reply_to"  :     [  ]  ,        "last_event"  :     "delivered"  ,        "scheduled_at"  :     null     }
```


## Conclusion


Threading, reply matching, and correlation are the foundation of building conversations on top of email. Exposing the Message-ID is the first step, and we're working on more primitives to make replies even easier.


If you have any questions, please reach out to us and we'll be happy to help.
