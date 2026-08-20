---
schema_version: "1.0.0"
document_id: "59a6e515bc54236d463071e9ef2d9c6b6dfd248f66d90ad412f214c5a4d4dd56"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/resend-forward-3-wrap-up"
published_at: "2024-08-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:e029706e189e6609eff19fd802a9f56e163b042a784da14ec3c47d9dd9475316"
---

# Resend Forward 3: Wrap Up

Today marks the end of[Resend Forward](https://resend.com/forward) , our third launch week.


In the last seven days, we shipped six new features.


Here's a quick recap of what we launched:


- Day 1: Schedule Email API
- Day 2: Vercel Integration
- Day 3: Marketing Analytics
- Day 4: Deliverability Insights
- Day 5: React Email 3.0
- Day 6: Broadcast Schedule


If you prefer watching instead of reading, check out this video:


## Day 1: Schedule email API


On Monday, we introduced a new API to schedule emails for a specific time.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const   oneMinuteFromNow   =     new     Date  (  Date  .  now  (  )     +     1000     *     60  )  .  toISOString  (  )  ;
await   resend  .  emails  .  send  (  {        from  :     'Acme <onboarding@resend.dev>'  ,        to  :     [  'delivered@resend.dev'  ]  ,        subject  :     'hello world'  ,        html  :     '<p>it works!</p>'  ,        scheduledAt  :   oneMinuteFromNow  ,     }  )  ;


```


Before, you had to use external services to handle the scheduling logic, but now you can use the new Resend API to schedule emails without having to introduce additional complexity.


[Schedule email API Send emails at a specific time without additional complexity. resend.com/blog/introducing-the-schedule-email-api](https://resend.com/blog/introducing-the-schedule-email-api)


## Day 2: Vercel Integration


On Tuesday, we announced a new integration with[Vercel](https://vercel.com/) , designed to speed up the process of managing API keys and environment variables in your Vercel projects.


Now, you can integrate your Vercel projects directly within the Resend dashboard. This means that with just a few clicks, you can automatically create a new API key linked to the domain of your choice and have it instantly added as an environment variable in your Vercel project.


[Vercel Integration Connect your Resend API keys with Vercel environment variables in a few clicks. resend.com/blog/vercel-integration](https://resend.com/blog/vercel-integration)


## Day 3: Marketing Analytics


On Wednesday, we launched a new view for marketing users. The goal is to understand how your emails deliver, who is engaging with them, and where users opt-out.


Earlier this year, we focused on creating the easiest way to write and send email blasts to your users.


But what happens *after* you send an email? How do you know if your users liked it?


To help answer that question, we are unveiling Marketing Analytics, giving you both aggregated metrics along with itemized breakdowns of how your broadcasts performed.


[Marketing Analytics Understand how your emails deliver, who is engaging with them, and where users opt-out. resend.com/blog/introducing-marketing-analytics](https://resend.com/blog/introducing-marketing-analytics)


## Day 4: Deliverability Insights


On Thursday, we released a new feature to improve your chances of landing in the inbox instead of the spam folder.


As we've helped companies of all sizes improve their deliverability, we realized a pattern in our advice. We often recommended the same best practices over and over. Knowing these best practices requires you to be an email expert or read different articles on deliverability, which can be overwhelming.


That's why we created Deliverability Insights.


[Deliverability Insights Improve email deliverability by identifying issues and applying best practices. resend.com/blog/deliverability-insights](https://resend.com/blog/deliverability-insights)


## Day 5: React Email 3.0


On Friday, we announced a new major version of[react.email](https://react.email/) with tons of bug fixes and an 11x performance improvement.


This version also includes **54 components** for you to create beautiful emails inspired by[Tailwind UI](https://tailwindui.com/) and[shadcn/ui](https://ui.shadcn.com/) .


[React Email 3.0 An entire collection of pre-built components with a much faster development experience. resend.com/blog/react-email-3](https://resend.com/blog/react-email-3)


## Day 6: Broadcast Schedule


On the last day, we returned to the topic of scheduling emails, but this time, focused on broadcasts.


Now, you can use natural language to schedule emails to multiple contacts using broadcasts.


[Broadcast Schedule Use natural language to schedule emails to multiple contacts. resend.com/blog/broadcast-schedule](https://resend.com/blog/broadcast-schedule)


## Looking Forward


We hope you enjoyed the new features we launched last week.


See you in the next one.
