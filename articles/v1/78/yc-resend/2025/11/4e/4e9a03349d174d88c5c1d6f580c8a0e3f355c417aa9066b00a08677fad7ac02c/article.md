---
schema_version: "1.0.0"
document_id: "4e9a03349d174d88c5c1d6f580c8a0e3f355c417aa9066b00a08677fad7ac02c"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/how-dub-uses-webhooks-to-power-features"
published_at: "2025-11-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:a7e133c4a4446162bdeace2ff229b01bca6e0642694e0e52adee5594f2090ab0"
---

# How Dub Uses Webhooks to Power Features

How do you build a product packed with features that heavily rely on email—without having to manage email infrastructure?


**[Dub](https://dub.co/) is a link attribution platform for modern teams** that provides quick links, conversion tracking, affiliate programs, and more.


It's quickly become known for its **world-class user experience** . Every detail is crafted to be quick, delightful, and intuitive. And for many companies, Dub is a central tool for marketing and growth.


Like many communication platforms, Dub relies on email, although email is not their core product. The quality of Dub isn't accidental. It's the result of relentless focus.


## The Power of Focus


When Dub set out to build a modern affiliate network, communication between brands and partners was core to the experience. Email powers key features, like:


- Partner onboarding
- Conversion tracking
- Campaign communications
- Reporting and analytics
- Affiliate payout tracking


They faced the same question that many companies face: *"What tools will support us at scale so we can focus on building our product?"*


Dub needed a platform that could scale with their growing user base. Over two years later, Dub now sends **millions of emails each month** with Resend. What started as a reliable email provider has become a key piece of Dub's product architecture.


> We went from pretty much zero to now doing about **5 million emails per month** . The ability to build at scale **without needing to worry** about infrastructure or deliverability has been incredibly helpful. Soon, we anticipate doubling or even tripling that number.
>
>
> Steven Tey
>
>
> Founder of Dub


Resend’s developer-first[API](https://resend.com/docs/api-reference/) and[reliability](https://resend-status.com/) meant Dub could focus on building features rather than maintaining queues, handling retries, or fussing over reputation metrics.


The value of a trusted partner that can scale with your needs is the power to focus.


## Turning webhooks into product features


Where many teams turn to complex and over-engineered solutions, **Dub excels at simplicity** . It's the superpower that lets them build faster and with more creativity.


As one example, Dub uses webhooks in creative ways to power features.


Dub’s early use of Resend[webhooks](https://resend.com/docs/dashboard/webhooks/) was straightforward: syncing unsubscribes between Resend and their database. But as the platform grew, those same webhooks became the building blocks for entirely new features.


When Dub launched[Partners](https://dub.co/partners) , its modern affiliate management platform, the team wanted in-app messaging between brands and affiliates—with real-time visibility into read statuses.


Instead of building a full email tracking system from scratch, Dub extended Resend’s webhook primitives to power that feature. What could have been a complex, time-consuming build became a seamless integration.


As Steven put it, "Most people see webhooks as notifications. For us, they became the foundation of a new feature." The[email.opened](https://resend.com/docs/dashboard/webhooks/event-types#email-opened) webhook now drives read-status updates for Dub’s messaging system.


Combined with the[batch sending API](https://resend.com/docs/api-reference/emails/send-batch-emails) and message[tags](https://resend.com/docs/dashboard/emails/tags) , it allows Dub to handle complex many-to-many relationships—where multiple brand owners send messages to multiple partners—all without needing to maintain their own email infrastructure or analytics pipeline.


For Dub, Resend isn’t just a vendor. It’s a collaborator in their growth. Steven keeps new Resend features in the back of his mind—ready to connect what Dub needs with what’s possible.


> It’s always been a combination of what we need and what’s possible. **Resend feels more like a partner in our growth than just another vendor** .
>
>
> Steven Tey
>
>
> Founder of Dub


That mindset has inspired new experiments, from leveraging custom domains for deliverability and branding, to exploring the[Inbound Email](https://resend.com/docs/dashboard/receiving/introduction) feature to enable two-way conversations between brands and affiliates.


Every new Resend release becomes an opportunity to push Dub’s product forward.


## Looking ahead


Dub’s journey shows how a developer-first email platform can evolve from a backend utility into a true product enabler.


As Steven put it, the partnership has allowed Dub to move faster and build smarter:


> When you have a tool that’s **this reliable and this flexible** , you start to see all the new ways it can shape your product.
>
>
> Steven Tey
>
>
> Founder of Dub


Dub’s story is a reminder that the right primitives can unlock powerful new features and enable you to build products that you couldn't have imagined before.
