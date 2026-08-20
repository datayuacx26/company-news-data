---
schema_version: "1.0.0"
document_id: "5bfd767d4fe1e6e93a37afddae4706f9f249face6530ccaadbd2dc5882f637ed"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/microsoft-bulk-sending-requirements-2025"
published_at: "2025-05-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:bdf1c048ca4aeb517f2581b89ca6a17897177a7777dc70fd0e95c669b21d96ba"
---

# Microsoft’s bulk sending requirements for 2025

Back in early 2024,[Gmail](https://support.google.com/a/answer/81126?hl=en) and[Yahoo](https://senders.yahooinc.com/best-practices/) set the standard for bulk email senders. Next[Apple](https://support.apple.com/en-us/102322) switched up their own recommendations to strict requirements this past February.[Now Microsoft is joining the party.](https://techcommunity.microsoft.com/blog/microsoftdefenderforoffice365blog/strengthening-email-ecosystem-outlook%E2%80%99s-new-requirements-for-high%E2%80%90volume-senders/4399730)


Starting **May 5, 2025** , Microsoft enforced new requirements for high-volume senders. If you're already following best practices, this won’t be a big adjustment. But there are a few Microsoft-specific quirks you’ll want to double-check.


Let’s walk through who this applies to, what’s changing, and how to make sure you’re covered.


## Who Does This Impact?


These new rules are designed to target **bulk messages** like newsletters, promotional mail, sales emails, or other non-triggered campaigns.


More specifically, this applies to anyone sending **5,000 or more messages per day** to “consumer” Microsoft domains like outlook.com, hotmail.com, and live.com. (Gmail and Yahoo use the same 5,000/day threshold for their own policies.)


> Remember, even if you're not sending bulk mail, following these rules helps everyone.


No matter how much mail you send, strong authentication and a well-managed database help your mail land in the inbox. And if you’re growing, it’s worth getting compliant now before deliverability issues creep in.


## Microsoft Must-Haves


### SPF, DKIM, and DMARC are required


If your messages fail SPF, DKIM, or DMARC checks, Microsoft may flat-out reject them at the SMTP level. You’ll see errors like this:


> 550; 5.7.515 Access denied, sending domain \[SendingDomain\] does not meet the required authentication level.


Resend Tip


You’re prompted to add SPF, DKIM, and DMARC records during your initial domain setup with Resend. Once verified, you’re fully authenticated and DMARC-aligned automatically.


### One-click unsubscribe with proper headers


All bulk messages must include a clearly visible, easy-to-use unsubscribe link in the body of the email. Unlike other receivers, Microsoft doesn’t specify whether that link needs to be a one-click experience, but it should be obvious and work reliably.


To achieve one-click unsubscribe:


**1. Add unsubscribe headers to your email headers**


```text
List  -  Unsubscribe  :     <  https  :  /  /  example  .  com  /  unsubscribe  >    List  -  Unsubscribe  -  Post  :   List  -  Unsubscribe  =  One  -  Click
```


**2. Handle` POST` requests properly**


Return a blank page with a **200 (OK)** or **202 (Accepted)** when receiving a` POST` request with the` List-Unsubscribe=One-Click` key pair.


**3. Handle` GET` requests properly**


Show a regular unsubscribe page if your unsubscribe page is directly accessed.


Resend Tip


Add your own List-Unsubscribe header and link[via the API](https://resend.com/docs/dashboard/emails/add-unsubscribe-to-transactional-emails) , or let Resend handle it all automatically by using` {{{RESEND_UNSUBSCRIBE_URL}}}`[in a Broadcast](https://resend.com/docs/api-reference/broadcasts/create-broadcast) .


### Low spam complaint rates (under 0.3%)


This is the[same threshold as Google and Yahoo](https://resend.com/blog/gmail-and-yahoo-bulk-sending-requirements-for-2024/) .


Resend Tip


Monitor complaint rates in[the Metrics tab](https://resend.com/metrics) or stream specific user events (complaints, bounces, unsubscribes, etc.)[via webhook](https://resend.com/docs/dashboard/webhooks/event-types) .


### Valid Sender Addresses


Your “From” and “Reply-To” addresses can’t bounce. At least one should be able to accept messages, even if you’re not monitoring replies.


Resend Tip


At Resend, we love using[Slack channel email addresses](https://resend.com/docs/knowledge-base/how-can-i-receive-emails-with-resend) for the Reply-To field to manage replies efficiently.


## Final Thoughts


These new requirements from Microsoft are part of a broader shift across the email ecosystem, and **that’s a good thing** . Stronger authentication, better unsubscribe handling, and lower spam rates make email safer and more reliable for everyone involved: senders, receivers, and the people reading your messages.


If you’re using Resend, you’re already in a great spot. We built the platform to make this stuff easy including guided domain setup, built-in unsubscribe tools, and real-time visibility into complaints and bounces.


So May 5th is a checkpoint to review your mail setup. It doesn’t take much to get ahead of the curve, and[we’re here if you need help](https://resend.com/contact) .
