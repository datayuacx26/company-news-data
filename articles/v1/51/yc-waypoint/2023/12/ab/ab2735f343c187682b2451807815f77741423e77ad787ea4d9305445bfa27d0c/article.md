---
schema_version: "1.0.0"
document_id: "ab2735f343c187682b2451807815f77741423e77ad787ea4d9305445bfa27d0c"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-news-import-ffde4d661374"
canonical_url: "https://www.usewaypoint.com/blog/the-case-for-a-transactional-email-template-builder"
published_at: "2023-12-20T00:00:00+00:00"
first_seen_at: "2026-07-22T19:31:17.685303+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:d84040f779e28f07100317745bb585ebfa644866716e235fcae1fba67e0be70c"
---

# The case for a transactional email template builder

## Why do software teams still hand-code email templates?


This became one of the key questions during our initial research for[Waypoint](https://www.usewaypoint.com/) . Of course, coding templates will never go away – just like coding websites has a time and place. However, as no-code platforms have become much more prevalent for building websites, applications, and internal tools, we were curious why email hasn't followed a similar path.


You might be thinking, but what about those email template builders on services like Mailchimp and SendGrid that have been around for over a decade?


It turns out, these template builders were simply not designed for the kinds of emails that software teams typically send. They were designed for the marketing team to create email newsletters that contain little to no dynamic content (outside of a simple variable or two). So while these existing builders work for email campaign blasts, they would quickly fall flat when working with product-triggered '[transactional emails](https://www.usewaypoint.com/templates) ' like receipts, confirmations, alerts, reports.


This results in teams continuing to fall back on engineers hand-coding email templates (either using[straight HTML](https://www.usewaypoint.com/templates) or with a component library like[MJML](https://mjml.io/) ) within their codebase.


While the code approach is certainly a flexible solution, **the big problem with this approach is that it results in engineers having to maintain these emails** . As the team and product grows, this leads to endless engineering bottlenecks and a backlog of changes that never get prioritized.


## It's time for a transactional email template builder


What if there was a better way that could streamline this collaboration process?


Enter a template builder designed for transactional email templates.


At[Waypoint](https://www.usewaypoint.com/) , we built exactly this and bundled it with our email API service. This approach enables teams to create data-rich templates and then once the template is setup, developers can[send data](https://www.usewaypoint.com/) through the API to send the email.


*Waypoint's*[sample template gallery](https://www.usewaypoint.com/templates) *.*


### The template building experience


Since transactional email templates are much more complicated than marketing emails, we've built a suite of features within our builder that makes for a great experience between the designer and developer – all without code.


-


Template can can be immediately previewed with test data.


-


Advanced blocks like loops, progress bars, and avatars can be created without custom HTML.


-


Text can be formatted in Markdown and can reference LiquidJS variables and LiquidJS filters.


-


Blocks can be conditionally hidden based on variables.


-


Test data scenarios can be created to simplify testing different template states.


-


Option to prevent sending if referenced template data is missing.


-


Navigator panel with drag and drop.


See some of these features in action:


*Video showing the template building experience on Waypoint.*


As it turns out, this approach has been a breath of fresh air for collaborative product teams:


### Compared to existing builders


When comparing Waypoint's building experience to existing dynamic template builders[similar to SendGrid](https://www.usewaypoint.com/) , the differences are easy to spot when working with data-rich templates.


*Screenshot of Waypoint's template builder (top) vs SendGrid's template builder (bottom).*


These differences are even more pronounced when doing more advanced templating. In the example below, Waypoint has a way to visually build loops and conditionals while SendGrid requires HTML with Handlebar templating code.


*Screenshot of Waypoint’s template builder visually editing a template with a 'loop' block (top) vs having to use custom HTML and Handlebar templating on SendGrid (bottom).*


## Conclusion


If you're on a product team and your application sends transactional emails, it's worth considering a new approach to emails that reduce bottlenecks. You may already be using powerful no-code builders for the inherit collaboration benefits – why not do the same the same for your product emails?


Further reading:


-


[Waypoint home](https://www.usewaypoint.com/)


-


[Waypoint template gallery](https://www.usewaypoint.com/templates)


-


[Waypoint template basics](https://www.usewaypoint.com/)
