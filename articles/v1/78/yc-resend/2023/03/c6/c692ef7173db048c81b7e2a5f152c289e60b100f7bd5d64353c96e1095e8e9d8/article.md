---
schema_version: "1.0.0"
document_id: "c692ef7173db048c81b7e2a5f152c289e60b100f7bd5d64353c96e1095e8e9d8"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/open-and-click-tracking"
published_at: "2023-03-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:713abada36fadc16dde7af9d8f4b44f1b74f16d682458922dd3dfe80825ac42e"
---

# Open and Click Tracking

> "How are my emails doing?"
>
>
> "Is anyone engaging with my CTA?"
>
>
> "How many users saw my announcement?"


These are the questions that come when you start sending emails at scale.


Today, Resend helps you not just answer, but take action on those questions.


## More than curiosity


We believe open and click tracking offers practical and actionable insights to better engage with your users.


Ultimately, opens and clicks show you which customers are the most interested in receiving the messages you send.


By knowing this you can:


- Measure results of your emails
- Have a better understanding of your audience
- Transform these insights into action by refining your email templates


Click and Opened Events in Resend Dashboard


## How open tracking works


When open tracking is enabled, we insert a 1 pixel by 1 pixel transparent GIF image at the bottom of each message.


Each email includes a unique link to this image file; when the image is opened, we can tell exactly which message was opened and by which user.


> "If you're a developer or working on a startup, you're going to love Resend's approach to emailing."
>
>
> Joe DeMaria
>
>
> Co-founder & CEO of SpecCheck


## How click tracking works


When click tracking is enabled, we set up a redirect for each link in the message. When your user clicks a link, they are routed to a Resend server and then immediately forwarded to the address you provided. This handoff is imperceptible to the recipient in normal circumstances.


As with open tracking, each of these redirect links is unique, allowing you to easily determine which person clicked which link at what time.


> "We were up and running with Resend in no time. It was seamless to integrate into our existing workflow and gave us a tremendous amount of visibility into our email capabilities. Simple to say, it was a no-brainer."
>
>
> Ty Sharp
>
>
> Co-founder & CEO of InBuild


## Perfectly paired with Webhooks


[We announced Webhooks](https://resend.com/blog/webhooks) earlier this week, perfect timing for open and click tracking.


Creating a webhook stream of your tracking events gives you a live pulse of your contact list so you can take action immediately.


Click and Opened Events in Resend Dashboard


You can also feed these data points into an external storage system to provide more context alongside your other user events.


Click webhook event


## How to enable tracking


Tracking is configured on the domain level.


You can enable tracking today by navigating to Domains, selecting the domain you want to enable and looking for the "Configuration" section.


Enable Open/Click tracking
