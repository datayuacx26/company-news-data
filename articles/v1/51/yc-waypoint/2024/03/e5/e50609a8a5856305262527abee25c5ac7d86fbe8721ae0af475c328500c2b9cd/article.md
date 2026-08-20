---
schema_version: "1.0.0"
document_id: "e50609a8a5856305262527abee25c5ac7d86fbe8721ae0af475c328500c2b9cd"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-news-import-ffde4d661374"
canonical_url: "https://www.usewaypoint.com/blog/mailchimp-transactional-email-templates-builder"
published_at: "2024-03-21T00:00:00+00:00"
first_seen_at: "2026-07-22T19:31:17.685303+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:46cadcf0c9e4dc8f4cc5662f4d2eee1eafc9ae4fdc8a8ed6bd2d5e560c6de2a6"
---

# Building Mailchimp transactional email templates without code

While MailChimp is primarily used for email marketing campaigns, MailChimp also offers an email API product for one-to-one 'transactional emails'. This email API service, originally called Mandrill, was introduced in 2012 and turned into an[add-on service](https://www.cmswire.com/digital-marketing/mailchimps-mandrill-move-enrages-email-users/) in 2016. Although this add-on service is integrated in many ways, many have found it challenging to build and collaborate around transactional email templates on MailChimp.


## Problem


While MailChimp has a no-code builder for campaign emails, it does not offer a similar one for their API product.


This can provide a disjointed experience as it forces teams to have to hand-code HTML email templates. Which, if you are reading this article, you probably already know that HTML emails are filled with[quirks](https://www.caniemail.com/) and 90s style HTML tables in order for them to render well across clients and devices.


## Solution


Enter[EmailBuilder.js](https://emailbuilderjs.com/) – a free and completely[open-source](https://github.com/usewaypoint/email-builder-js) block-based email template builder.


*Free and open-source EmailBuilder.js*[playground](https://usewaypoint.github.io/email-builder-js/#sample/reservation-reminder) *.*


Sample templates on the playground:


-


[Welcome email](https://usewaypoint.github.io/email-builder-js/#sample/welcome)


-


[One-time passcode (OTP)](https://usewaypoint.github.io/email-builder-js/#sample/one-time-password)


-


[Reset password](https://usewaypoint.github.io/email-builder-js/#sample/reset-password)


-


[E-commerce receipt](https://usewaypoint.github.io/email-builder-js/#sample/order-ecomerce)


-


[SaaS subscription receipt](https://usewaypoint.github.io/email-builder-js/#sample/subscription-receipt)


-


[Reservation reminder](https://usewaypoint.github.io/email-builder-js/#sample/reservation-reminder)


-


[Post metrics](https://usewaypoint.github.io/email-builder-js/#sample/post-metrics-report)


-


[Respond to inquiry](https://usewaypoint.github.io/email-builder-js/#sample/respond-to-message)


By using EmailBuilder.js (either self-hosted or on the[playground](https://usewaypoint.github.io/email-builder-js/#sample/reservation-reminder) ), teams can build email templates and output to them to clean JSON or HTML that can be copy and pasted into MailChimp's transactional email template.


*Raw HTML output that can be copy and pasted into MailChimp's transactional email template.*


Additionally, all blocks automatically look great across clients and devices as they are[tested and supported](https://www.usewaypoint.com/docs/email-client-support) by modern email clients (on both desktop and mobile) including: Gmail, Apple Mail, Outlook, Yahoo! Mail, HEY and Superhuman.


## Looking for a more collaborative solution?


EmailBuilder.js is the free community version of our much-loved no-code template builder on[Waypoint](https://www.usewaypoint.com/) . If you are looking for a more integrated and collaborative experience for your team, check out[Waypoint's email API](https://usewaypoint.com/) (an alternative to MailChimp's email API) with a hosted template builder with dynamic LiquidJS variables, drag and drop, Markdown formatting, reusable layouts, loops, and additional blocks.


*Waypoint's collaborative 'pro builder' that is tightly integrated with Waypoint's email API.*


## Wrap up


If you need a no-code builder for your HTML emails, take advantage of our free and open-source community builder:[EmailBuilder.js](https://emailbuilderjs.com/) .


Further reading:


-


[MailChimp email API docs](https://mailchimp.com/developer/transactional/docs/templates-dynamic-content/#creating-templates)


-


[EmailBuilder.js open-source GitHub project](https://github.com/usewaypoint/email-builder-js)


-


[EmailBuilder.js playground](https://usewaypoint.github.io/email-builder-js/#sample/reservation-reminder)


-


[Waypoint home](https://www.usewaypoint.com/)


-


[Waypoint quick start guide](https://www.usewaypoint.com/docs/quickstart)
