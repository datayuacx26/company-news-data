---
schema_version: "1.0.0"
document_id: "0c8bd247c88c9c2853c39c31cdc80fceafdebe70c84040e31e4d1575e68b94de"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/how-gumroad-migrated-100m-emails-to-resend"
published_at: "2025-03-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:ce51bd076249df5bc945810ea838f6d258b4028e61409f7bb435292f4a3f6e84"
---

# How Gumroad Migrated 100M Emails to Resend

Gumroad is a platform that allows creators to sell digital products.


They rely heavily on emails both to communicate with creators and to allow creators to communicate with their audiences.


This is the story of how Gumroad transformed a potential crisis into an opportunity for better email infrastructure.


## The Problem


During a recent busy holiday season, their email provider suddenly blocked their account. They couldn't get access to support and were shut down for multiple days.


Gumroad was already using Resend for some smaller projects, but their experience prompted a full switch to Resend as their primary email provider.


> The Resend team moved very quickly and helped us when we needed it most. In the very same day, we had a pricing quote, and a team of engineers looking at our case.
>
>
> Sahil Lavingia
>
>
> CEO of Gumroad


## The Resend Experience


Gumroad reached out over our shared Slack channel during the Christmas Break at 4:50am PT on a Saturday. By 6:58am, we were deep in talks about how we could help, and by 4:05pm, they'd signed a contract.


Within hours we were debugging their setup to unblock them. Within two days,[Brian Kerr](https://resend.com/humans/brian-kerr) , one of our Customer Success Engineers, had reached out on Slack to set up a custom migration plan based on their needs.


From the very beginning, we've been focused on Developer Experience and that starts with getting out of the way so developers can do their thing. Help should be easy to access and should operate more like a team member and not like a ticket system.


## How Gumroad Works


Creators on Gumroad can send emails to their audience to communicate new releases, updates, and more. To do this, they use Gumroad's email builder that allows them to write and format beautiful emails.


Once Gumroad users hit send, the email is sent and delivered to their audience.


## A Custom Migration Plan


Our goal is to help you land in inboxes, and with large senders, we often create custom plans to guide you through the migration process.


### Key Considerations


Each sender is different, but for Gumroad we focused on the following key considerations.


**1. Domain segmentation**


Domain reputation is critical to achieve and maintain high deliverability. We encouraged segmentation of their audience into different subdomains to help protect against abuse (e.g.,` pro.gumroad.com` ,` vip.gumroad.com` , etc.).


**2. Sending rate**


Landing in the inbox requires appearing trustworthy to inbox providers (e.g. Google, Yahoo, etc.). We recommended spacing larger newsletter spikes to avoid being rate limited or blacklisted.


**3. Improved Queueing**


[Resend's Batch API](https://resend.com/docs/api-reference/emails/send-batch-emails) handles the infrastructure and orchestration complexity so you can focus on your product.


Queue System Architecture


**4. Internal monitoring**


We helped Gumroad[set up webhooks](https://resend.com/docs/dashboard/webhooks/introduction) to capture bounces and complaints. Capturing this data allowed them to create alerts per sender based on industry Acceptable Usage Policy (AUP) thresholds.


- Complaint rate <0.08%.
- Bounce rate <4%.


### Suppression Lists


One of the most valuable resources for planning a migration is a customer's suppression lists (i.e., known bounces). With larger senders, these lists can often contain millions of email addresses.


We imported Gumroad's suppression lists from their previous provider to ensure they didn't unnecessarily damage their sending reputation during the migration.


### Transition Plan


For best deliverability during a migration, you should slowly increase your email volume at a consistent rate and avoid "spikes". The goal is predictability and consistency to avoid surprising email providers.


Migrating millions of monthly emails requires coordination and planning, so during our early talks, we set up a transition plan to fit their needs across each of their domains.


Here's an example of[how that may look](https://resend.com/docs/knowledge-base/warming-up) for a smaller sender:


Day Messages per day Messages per hour


1 Up to 5,000 emails 100 Maximum


2 Up to 2,500 emails 300 Maximum


3 Up to 5,000 emails 600 Maximum


4 Up to 5,000 emails 800 Maximum


5 Up to 7,500 emails 1,000 Maximum


6 Up to 7,500 emails 1,500 Maximum


7 Up to 10,000 emails 2,000 Maximum


## True Customer Success


As Gumroad migrated their sending to Resend, we ensured our team was available to help with any questions. Scale and Enterprise customers get access to a dedicated Slack channel with our engineers to help with any issues that arise.


As an example, early on they asked about e2e testing.


> How do you suggest we go about testing emails? In Rails we had e2e tests which tested whether the correct email had been enqueued in the ActionMailer queue. Is it possible to do the same with Resend and an e2e testing tool like Playwright?


We created a sample repository and recorded an internal video to help walk them through the process of setting it up.


We also actively monitor and reach out to customers who are experiencing issues. As an example, we contacted Gumroad via Slack when:


- we noticed large sending changes
- potential issues may impact their account
- new features may benefit them


## Continuously Improving


During a recent team offsite, we blocked off time for a private call with the Gumroad team to discuss their experience with Resend.


They walked us through their use of Resend, pointed out some pain points and features we could improve, and shared their top priority needs.


## Why Resend?


> From the very beginning, Resend has excelled at customer experience. They not only responded to our urgent needs, but also proactively guided us through the migration process. Working with Resend means getting email expertise, not just email service. We're able to focus on building our core product and growing our business because we're confident that emails will land in our customers' inboxes.
>
>
> Connor Mann
>
>
> Staff Software Engineer at Gumroad


Sending at scale requires an email provider who is reliable, communicative, and responsive. See how Resend can power your next big idea.


Learn more about our[Enterprise offering](https://resend.com/enterprise) .
