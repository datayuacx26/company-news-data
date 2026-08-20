---
schema_version: "1.0.0"
document_id: "42fc4fad2a6507e902f8c0620bbb010b9a3ecac514d5751fb6ac4f9cd613ba31"
company_key: "yc-magicbell"
company: "MagicBell"
source_id: "yc-magicbell-news-import-9a6b19b555ea"
canonical_url: "https://www.magicbell.com/blog/what-is-aws-pinpoint"
published_at: "2026-03-21T00:00:00+00:00"
first_seen_at: "2026-07-22T03:05:45.790203+00:00"
fetched_at: "2026-07-28T22:18:15.193115+00:00"
content_hash: "sha256:d95bc7f7fcee6cfabe468a5425bb65837907650c217edc9fa8152a14c267b552"
---

# What Is AWS Pinpoint?

**AWS Pinpoint is a messaging platform from Amazon Web Services that sends targeted messages across push, email, SMS, voice, and in-app channels.** It brings audience segments, campaigns, and analytics into one service. AWS plans to end support for Pinpoint on October 30, 2026.


## How AWS Pinpoint Works


Pinpoint sits on top of other AWS messaging tools. It adds a layer for running campaigns across channels. You pick an audience, write a message, and choose when and how to send it. The service then tracks opens, clicks, and results.


The basic flow has three steps:


1. **Pick your audience.** Build segments from user data and behavior, or upload a list.
2. **Set up a campaign or journey.** Campaigns send one message to a group. Journeys let you build multi-step flows with branches and wait times.
3. **Send and track.** Pinpoint sends the message through the right channel and shows you how it did.


This works well for marketing teams that run planned campaigns. For real-time or event-based messages, Pinpoint also has an API you can call directly.


## What Channels Does Pinpoint Support?


Pinpoint covers five channels, plus a custom option.


### Push Notifications


Pinpoint works with Apple Push Notification service (APNs) and Firebase Cloud Messaging (FCM). It sends[push notifications](https://www.magicbell.com/blog/what-are-push-notifications) to iOS and Android phones. You store device tokens, group users into segments, and send pushes through campaigns or the API.


### Email


Pinpoint uses Amazon SES to send email. You get templates, custom fields, and delivery tracking. If email is all you need,[Amazon SES on its own](https://www.magicbell.com/blog/what-is-amazon-ses-guide) costs less and gives you more control.


### SMS


Pinpoint handles[SMS notifications](https://www.magicbell.com/blog/what-are-sms-notifications-and-why-are-they-important) for alerts and promos. You can send one-way texts or turn on two-way SMS for chats. Costs change based on the country and message type.


### In-App Messaging


[In-app notifications](https://www.magicbell.com/blog/what-are-in-app-notifications) show up inside your app. Pinpoint has an SDK that displays these based on user segments and events. Use this for onboarding, feature tips, or timely prompts.


### Voice


Pinpoint can call phone numbers and play a text-to-speech message. This is rare but useful for urgent alerts or users who need audio.


## AWS Pinpoint vs SNS vs SES


AWS has several messaging tools that do similar things. Here is how they compare.


Feature AWS Pinpoint Amazon SNS Amazon SES


Main use Multi-channel campaigns Pub/sub and app-to-app messaging Email at scale


Channels Push, email, SMS, voice, in-app Push, SMS, email, HTTP Email only


Segments Yes No No


Campaigns Yes, with A/B tests No No


Analytics Opens, clicks, results Delivery only Bounces and complaints


Pricing Per message + audience fee Per message Per message


**Pinpoint vs SNS:** SNS is a pub/sub tool. It fans out messages to apps and services. It can send push and SMS, but it does not know about user groups or campaigns. Pick SNS for system events. Pick Pinpoint when you need to target users and track results.


**Pinpoint vs SES:** SES is just for email. Pinpoint uses SES under the hood. If you only send email, SES costs less and gives more control. Pinpoint helps when you also need push, SMS, or in-app on top of email.


## Amazon Pinpoint Pricing


Pinpoint charges you based on what you use. There are no upfront fees.


**Audience fee:** The first 5,000 users are free each month. After that, you pay $0.0012 per user reached by a campaign, journey, or in-app message.


**Push:** The first 1,000,000 pushes per month are free. Each extra push costs $0.000001.


**Email:** Each email costs $0.0001.


**In-app:** The first 15,000 API calls per month are free. Each extra call costs $0.00011.


**SMS and voice:** Prices vary by country. AWS now manages these through End User Messaging.


**Events:** The first 100 million events per month are free. Each extra event costs $0.000001.


The free tiers are good for small apps. But costs add up fast at scale. The audience fee stacks on top of per-message costs, so high-volume senders pay twice.


## Where Pinpoint Falls Short


Pinpoint was a useful tool, but it has clear gaps.


### It Shuts Down in October 2026


AWS will end support for Pinpoint on October 30, 2026. After that date, you lose access to segments, campaigns, journeys, and analytics. The raw messaging APIs (SMS, push, voice) will live on as AWS End User Messaging. But the layer that ties them together goes away.


This matters most if you start building on Pinpoint now. You would need to migrate within months.


### It Locks You Into AWS


Pinpoint only runs on AWS. Your audience data, segments, and campaigns all live there. If you use more than one cloud, or want to avoid locking into a single vendor, this makes things harder. Moving off Pinpoint later takes real work.


### It Takes a Long Time to Set Up


You need to set up IAM roles, event streams, device tokens, and channel SDKs. Teams without deep AWS skills often spend weeks just getting started. That is a lot of effort for a service that is winding down.


### The Developer Experience Is Rough


Pinpoint was built for marketing teams, not developers. The APIs span many AWS services. There is no ready-made UI for user preferences. There is no inbox component you can drop into your app. You have to build these yourself.


### There Is No Notification Inbox


When Pinpoint sends a push, it is gone once the user swipes it away. There is no built-in place for users to find old messages. If you want an inbox, you have to build it from scratch. That is a lot of extra code and testing.


## AWS Pinpoint Alternatives


With Pinpoint going away, teams need other ways to handle multi-channel messages.


### MagicBell


MagicBell is a notification platform built for developers. It works with any cloud, not just AWS. It sends messages through push, email, SMS, in-app, Slack, and Teams.


Here is what makes it different from Pinpoint:


- **Works on any cloud.** No AWS lock-in. Use it with any stack.
- **Fast to set up.** Clean APIs, SDKs, and UI parts that plug in within minutes.
- **Built-in inbox.** Users get a place to view, manage, and act on past messages.
- **User preferences.** End users choose which channels they want. This cuts opt-outs and lifts engagement.
- **No sunset risk.** MagicBell is a growing platform, not a service facing shutdown.


If you are moving off Pinpoint or starting fresh, MagicBell handles the hard parts so you can focus on your product.


### Other Options


Other tools in this space include OneSignal (strong on push and in-app), Braze (full marketing suite), and custom builds on top of SNS and SES. Each has its own mix of cost, scope, and setup time.


## Wrapping Up


AWS Pinpoint packed segments, campaigns, and multi-channel delivery into one service. But it is going away in October 2026. Its AWS-only design and steep setup costs make it a tough sell for new projects.


If you need to send messages across push, email, SMS, and in-app today, pick a tool that will still be here next year.[MagicBell](https://www.magicbell.com/) gives you the same multi-channel reach without the lock-in.[Sign up for free](https://app.magicbell.com/) and start sending in minutes.
