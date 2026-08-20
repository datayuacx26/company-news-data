---
schema_version: "1.0.0"
document_id: "93c16cbff5a6438613c649063f56b7f70b387fe4bdde73207da015e24dff55c3"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/why-smtp-still-matters-today"
published_at: "2023-08-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:9ef932c912377404117d41d2fdfbbde27ea7aeb4fe912167f7e2275d5d7ff8e5"
---

# Why SMTP still matters today

**We want email to feel like the web** , modern and cutting edge. This is why we support modern technologies like[React for email building](https://react.email/) and[Serverless for sending](https://resend.com/docs/send-with-vercel-functions) .


Part of that approach led us to believe all we needed was a great REST API.


Here's a bit of background on why we invested in support of a 40-year-old protocol.


## How we got here


In 1971, Ray Tomlinson sent the first email using ARPANET. This became one of the foundational building blocks of the internet as we know it.


10 years later, SMTP was born and has persisted as the standard protocol for sending emails for the last 40 years. Though much of SMTP has been abstracted into various interfaces, it still remains the standard protocol underlying the[347 billion emails sent every day](https://www.statista.com/statistics/456500/daily-number-of-e-mails-worldwide/) .


## Why SMTP is so special


One reason SMTP integrations are still popular today is because they are provider agnostic, allowing the integration point to be based on the protocol rather than a specific email API.


We noticed quickly that there are still a large number of systems that rely on SMTP for their integrations. This is true for legacy systems, but also for popular frameworks like[Laravel](https://resend.com/docs/send-with-laravel-smtp) ,[Rails](https://resend.com/docs/send-with-rails-smtp) ,[Liferay](https://resend.com/docs/send-with-liferay-smtp) , and[Django](https://resend.com/docs/send-with-django-smtp) as well as modern systems like[Retool](https://resend.com/docs/send-with-retool-smtp) and[Supabase](https://resend.com/docs/send-with-supabase-smtp) that want to provide a quick and standardized way for their customers to send emails.


[Supabase SMTP](https://resend.com/docs/send-with-supabase-smtp)


It was evident that SMTP was not something to avoid, but rather something to embrace.


## Send with SMTP today


Today, we are announcing SMTP with Resend.


[SMTP on the Dashboard](https://resend.com/settings/smtp)


To configure an SMTP integration, you can simply use these credentials:


- Host: **smtp.resend.com**
- Port: **465**
- Username: **resend**
- Password: **YOUR_API_KEY**


See how simple it is to send an email with[SMTP and Nodemailer](https://resend.com/docs/send-with-nodemailer-smtp) :


For more help with integrating,[check our documentation](https://resend.com/docs/send-with-smtp) .
