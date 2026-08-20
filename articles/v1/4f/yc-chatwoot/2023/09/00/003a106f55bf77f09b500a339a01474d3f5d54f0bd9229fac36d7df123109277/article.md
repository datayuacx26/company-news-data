---
schema_version: "1.0.0"
document_id: "003a106f55bf77f09b500a339a01474d3f5d54f0bd9229fac36d7df123109277"
company_key: "yc-chatwoot"
company: "Chatwoot"
source_id: "yc-chatwoot-news-import-2810aa3d6c74"
canonical_url: "https://www.chatwoot.com/blog/what-is-coming-in-chatwoot-v3"
published_at: "2023-09-18T00:00:00+00:00"
first_seen_at: "2026-07-24T01:18:13.394314+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:5118ba687ec8ffec4e1c8eabf0f542c18e42ca52fa03fb23e8bf7935f203e90c"
---

# What is coming in Chatwoot V3?

Chatwoot V3.0 is on the horizon, slowly making its entrance. Our theme for this release is enhancing prioritization and boosting efficiency. In this article, we talk about the main changes we are introducing.


## Features in V3.0


### Robin AI 🐦: Our native AI assistant (Beta, invite only)


We are introducing a native AI assistant as our flagship feature in this release. This is a powerful addition designed to smoothen your entire customer support lifecycle. Here's how:


- **Talk to customers:** Whenever a customer messages you, Robin can immediately respond to them, hold a complete conversation, and resolve the queries––without ever needing your interaction.
- **Triage conversations:** It helps you prioritize conversations better by automatically suggesting labels and assisting with replies and summarizing conversations.
- **Tailor content:** The AI assistant learns from your interactions with customers and generates articles that were not available on your website. This helps you identify missing pieces and add more contextual content.


This new addition is set to significantly reduce the volume of conversations, making it easier to prioritize and manage customer inquiries effectively.


### Improved Messenger (Live in v3.1.0)


We've gone back to the drawing board and completely redesigned our website widget for a more seamless user experience. Key enhancements include faster loading times and integration with the Help Center. The redesign includes additional branding elements to prioritize your brand’s perception. Additionally, we've tackled long-standing issues such as continuity problems and support for multiple thread conversations.


### Improvements in Reporting (GA)


We have redesigned our reports and introduced a new metric in our Conversations, Agents, Labels, Inbox, and Team Reports – ‘Customer waiting time’. This helps you measure the period a customer waited for a response from an agent. We consider this metric, coupled with the First Response Time, to be crucial for monitoring customer service performance. In future releases, we plan to include automation and SLA features to further enhance this metric's usefulness.


### Dark mode (GA)


There has been a lot of discussion within the community regarding dark mode – which is now available within your dashboard. We also took this opportunity to clean up the old codebase, and thanks to TailwindCSS, we were able to get this up and running in a short amount of time. We are in the process of fully migrating from FoundationCSS.


0:00


/


### Experimental: Sentiment Analysis on conversations (TBA)


We're experimenting with sentiment analysis on conversations to explore whether it can be utilized to prioritize customer inquiries. This feature has the potential to revamp how you manage and respond to customer feedback.


### Fixes for commonly reported issues


We are addressing some of the outstanding community issues. These include improvements in permissions, email channel stability, and clarifications around the WhatsApp token authentication flow.


## The release process


You might be wondering how to get these updates on your instances. Let's delve into that.


We have released the first candidate with the aforementioned changes. Over the next few weeks, we'll continue rigorous testing and follow up with stable releases.


As you may have seen in some of the commits, we have many dependency updates, including the Postgres pg_vector extension. Although upgrading to version 15 should work without issues, we have not tested it thoroughly on all platforms, and some planned updates are still awaiting quality checks.


The changes will not be available in the master branch immediately. To get the changes to your instance, please follow the[manual upgrade process](https://www.chatwoot.com/docs/self-hosted/deployment/linux-vm?ref=www-internal-blog.chatwoot.com#upgrading-to-a-newer-version-of-chatwoot) with the branch name` 3.0.0-rc1` . Please note that we do not have the documentation for upgrading instances or fixing common issues. Therefore, we advise against switching your production instances immediately.


We're excited about the possibilities this V3 release brings to your customer support operations. If you have any questions or need assistance with the upgrade process, please don't hesitate to reach out to our support team.
