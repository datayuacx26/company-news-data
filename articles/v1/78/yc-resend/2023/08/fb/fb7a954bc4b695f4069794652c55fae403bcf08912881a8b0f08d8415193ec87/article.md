---
schema_version: "1.0.0"
document_id: "fb7a954bc4b695f4069794652c55fae403bcf08912881a8b0f08d8415193ec87"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/how-to-configure-supabase-to-send-emails-from-your-domain"
published_at: "2023-08-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:ec01f4c1567bb21c8dbb1aaad1ea82565464933489954e3dd1441e3d033559ff"
---

# How to configure Supabase to send emails from your domain

One of the coolest things about[Supabase](https://supabase.com/) is that it has a built-in authentication system that allows you to send emails to your users.


These emails typically include:


- Confirm signup
- Invite user
- Magic link
- Reset password
- Change email address


As you get ready to move to production, you might want to consider sending emails from a custom SMTP service. Here's why.


## The challenge of sending emails via Supabase


All of the[Supabase Auth](https://supabase.com/docs/guides/auth) emails are sent from` noreply@mail.app.supabase.io` .


Although this is good enough to get started, you probably want these emails to be sent from your own domain for brand recognition.


Supabase Auth Email


Apart from that, you can only send up to **4 emails per hour** . That's a[recent change](https://github.com/orgs/supabase/discussions/15896) caused by an influx of spam messages.


> "The default email system we provide is only for testing purposes - it is not intended for production use. This means that the deliverability will not be great, because everyone is using it for testing and is a major reason we don't recommend using it for production."
>
>
> Paul Copplestone
>
>
> Founder & CEO at Supabase


Beyond rate limits, a managed SMTP server might also help with:


- Deliverability and IP reputation management
- Compliance and anti-spam practices
- Analytics and webhooks
- Scalability


## Integrate to have full control and visibility


Supporting existing tools is one of Supabase's product principles, so they have made it easy to configure a custom SMTP server, or any other tool you need.


On top of that, we've built an integration that allows you to connect your Supabase project to Resend.


All you need to do is navigate to the[Integrations page](https://resend.com/settings/integrations) and follow the steps.


[Supabase integration](https://resend.com/settings/integrations)


Once you're done, you'll be able to send emails from your own domain.


This will give you full control and visibility over your email delivery.


No more *"Hey, I didn't get your email"* from your users. Now you can see exactly what's happening with your emails - if they were sent, delivered, bounced, or marked as spam.


Supabase integration Email


Here's a video showing the integration end-to-end:


## Configure a custom SMTP server manually


If you prefer to configure your SMTP server manually, you can do that as well.


Navigate to your Supabase project, click on **Project Settings** , then navigate to **Auth** . Over there you'll find the **SMTP Settings** section.


Supabase - SMTP Settings


Once you toggle the **Enable Custom SMTP** option, you'll be able to configure your SMTP server. You can change the host, port number, username, password, sender email, and sender name.


Supabase - SMTP Configurations


You can copy and paste all of these values from the[Resend SMTP](https://resend.com/settings/smtp) page.


Check the[documentation](https://resend.com/docs/send-with-supabase-smtp) to learn more about integrating SMTP with Supabase.
