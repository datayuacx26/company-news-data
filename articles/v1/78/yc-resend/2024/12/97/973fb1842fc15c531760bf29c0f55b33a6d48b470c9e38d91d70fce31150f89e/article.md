---
schema_version: "1.0.0"
document_id: "973fb1842fc15c531760bf29c0f55b33a6d48b470c9e38d91d70fce31150f89e"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/new-features-in-2024"
published_at: "2024-12-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:0732326a929663176bdb35021915440c2f68f06317453e5b19d7df87deda4983"
---

# Top 10 new features in 2024

Today marks the end of 2024. Before launching into 2025, let's recap some of the best features we launched this year.


Here's a quick overview:


1. Broadcasts
2. React Email 3.0
3. Audiences
4. Batch API
5. New SDKs
6. Email Scheduling
7. Marketing Analytics
8. Deliverability Insights
9. New webhooks
10. Platform Integrations


## 1. Broadcasts


In January, we expanded Resend from being an email API only to a no-code tool for technical and non-technical team members to engage with their audience.


Broadcasts let you send email blasts using our WYSIWYG editor. It comes with email testing, markdown support, performance tracking, and 1,000 contacts for free.


We continued improving the editor, like our **new style editor** .


Additionally, we released **four new components** :[Section](https://resend.com/changelog/new-section-component) ,[Code Block](https://resend.com/changelog/new-code-block-component) ,[YouTube](https://resend.com/changelog/new-youtube-component) , and[𝕏 (formerly Twitter)](https://resend.com/changelog/new-x-twitter-component) .


[Broadcasts Enabling anyone to send email campaigns without code. resend.com/blog/send-marketing-emails-with-resend-broadcasts](https://resend.com/blog/send-marketing-emails-with-resend-broadcasts)


## 2. React Email 3.0


This year, we launched **two major versions** of our open-source project[React Email](https://react.email/) , which included numerous bug fixes, new features and components, and a 40x performance improvement.


Version 3.0, the latest version, included **54 components** , along with several big performance improvements and new integrations.


[React Email 3.0 An entire collection of pre-built components with a much faster development experience. resend.com/blog/react-email-3](https://resend.com/blog/react-email-3)


## 3. Audiences


In January, we announced a new way to manage recipients.


Audiences let you add, update, retrieve, and remove contacts without having to worry about the entire unsubscribe flow. The new feature comes with[9 new API endpoints](https://resend.com/docs/api-reference/audiences/create-audience) . We also introduced a CSV wizard for easily importing contacts.


[Audiences Add, update, retrieve, and remove contacts without having to worry about the entire unsubscribe flow. resend.com/blog/manage-subscribers-using-resend-audiences](https://resend.com/blog/manage-subscribers-using-resend-audiences)


## 4. Batch API


This year, we released a new batch API to send up to 100 emails in one request.


The Batch API has full SDK support, including:[Python](https://github.com/resend/resend-python) ,[Ruby](https://github.com/resend/resend-ruby) ,[PHP](https://github.com/resend/resend-php) ,[Go](https://github.com/resend/resend-go) , and[Java](https://github.com/resend/resend-java) .


[Batch API Process a high volume of emails at once with the new bulk endpoint. resend.com/blog/introducing-the-batch-emails-api](https://resend.com/blog/introducing-the-batch-emails-api)


## 5. New SDK


Rust has seen a rapid rise in popularity in the last few years, and one of the driving forces behind it has been the attention paid to type safety and developer ergonomics.


With those values in mind, this year we released our new[Rust SDK](https://github.com/resend/resend-rust) . The new SDK joins a[dozen other SDKs](https://resend.com/docs/sdks) available for other languages, including Python, Ruby, PHP, Go, and Java.


[New Rust SDK You can now send emails with the official Resend SDK in your Rust projects. resend.com/changelog/announcing-the-rust-sdk](https://resend.com/changelog/announcing-the-rust-sdk)


## 6. Email Scheduling


We introduced two ways to schedule emails:transactional andbroadcasts .


### Transactional Schedule API


We introduced a new API to schedule transactional emails for a specific time.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const   oneMinuteFromNow   =     new     Date  (  Date  .  now  (  )     +     1000     *     60  )  .  toISOString  (  )  ;
await   resend  .  emails  .  send  (  {       from  :     'Acme <onboarding@resend.dev>'  ,       to  :     [  'delivered@resend.dev'  ]  ,       subject  :     'hello world'  ,       html  :     '<p>it works!</p>'  ,       scheduledAt  :   oneMinuteFromNow  ,     }  )  ;


```


Later in the year, we also added[natural language scheduling](https://resend.com/changelog/schedule-api-with-natural-language) to the API.


[Schedule email API Send emails at a specific time without additional complexity. resend.com/blog/introducing-the-schedule-email-api](https://resend.com/blog/introducing-the-schedule-email-api)


### Broadcast Scheduling


Using broadcasts, you can use natural language to schedule emails to multiple contacts.


[Broadcast Schedule Use natural language to schedule emails to multiple contacts. resend.com/blog/broadcast-schedule](https://resend.com/blog/broadcast-schedule)


## 7. Marketing Analytics


To help marketing users understand deliverability, engagement, and when users opt out, we released Marketing Analytics.


Marketing Analytics provides aggregated metrics and itemized breakdowns of how your broadcasts performed.


[Marketing Analytics Understand how your emails deliver, who is engaging with them, and where users opt-out. resend.com/blog/introducing-marketing-analytics](https://resend.com/blog/introducing-marketing-analytics)


## 8. Deliverability Insights


We often recommend the same best practices to our users and compiled these findings into Deliverability Insights, a new feature that provides best practices for email deliverability.


[Deliverability Insights Improve email deliverability by identifying issues and applying best practices. resend.com/blog/deliverability-insights](https://resend.com/blog/deliverability-insights)


## 9. New Webhooks


We introduced several new webhooks to provide more control and insight into events.


### Domain Webhooks


Domains are critical to email deliverability. Some companies allow their users to configure emails in their own apps using custom domains, which is essential for deliverability, white-labeling, and branding purposes.


We added real-time notifications when domains are created, updated, or deleted to assist these users.


[Domain Webhooks Receive real-time notifications when domains are created, updated, or deleted. resend.com/changelog/new-domain-webhooks](https://resend.com/changelog/new-domain-webhooks)


### Contact Webhooks


You can receive real-time notifications when contacts are created, updated, or deleted. These webhooks are helpful for sending welcome emails, storing contact updates, creating Slack alerts, syncing contacts with your CRM, and more.


[Contact Webhooks Receive real-time notifications when contacts are created, updated, or deleted. resend.com/changelog/new-contact-webhooks](https://resend.com/changelog/new-contact-webhooks)


## 10. Platform Integrations


This year, we introduced several new platform integrations, includingVercel andZapier .


### Vercel Integration


We announced a new integration with[Vercel](https://vercel.com/) , designed to speed up managing API keys and environment variables in your Vercel projects.


Now, you can integrate your Vercel projects directly within the Resend dashboard. With just a few clicks, create a new API key linked to the domain of your choice and have it instantly added as an environment variable in your Vercel project.


[Vercel Integration Connect your Resend API keys with Vercel environment variables in a few clicks. resend.com/blog/vercel-integration](https://resend.com/blog/vercel-integration)


### Zapier Integration


We launched a[new Zapier integration](https://zapier.com/apps/resend/integrations) that allows you to connect Resend with over 6,000+ apps.


Populating the email fields[Zapier Integration Automate workflows without using code. resend.com/changelog/zapier-integration](https://resend.com/changelog/zapier-integration)


## Looking Forward


We hope you enjoyed the new features we launched this year.


See you in the next one.
