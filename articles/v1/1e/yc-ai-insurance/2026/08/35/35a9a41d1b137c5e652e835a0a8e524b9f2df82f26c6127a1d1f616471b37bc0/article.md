---
schema_version: "1.0.0"
document_id: "35a9a41d1b137c5e652e835a0a8e524b9f2df82f26c6127a1d1f616471b37bc0"
company_key: "yc-ai-insurance"
company: "AI Insurance"
source_id: "yc-ai-insurance-rss-f853efdc5ed7"
canonical_url: "https://blog.aiinsurance.io/product-updates-july-2026-an-api-first-ai-native-platform"
published_at: "2026-08-14T22:44:53+00:00"
first_seen_at: "2026-08-14T23:02:07.041998+00:00"
fetched_at: "2026-08-14T23:02:08.115828+00:00"
content_hash: "sha256:ff15a05788e98103596851cb0bfb6581ed7213140cd545494d31395b3a8a0045"
---

# Product Updates | July 2026: API-First, AI-Native

For this month’s product update we have some pretty big announcements. As we continue our journey towards building the best insurance management platform we will be launching a big set of improvements to our platform over the next six months.


Prefer to listen? Here’s this update as a podcast episode. Your browser doesn’t support embedded audio.[Download the episode](https://blog.aiinsurance.io/hubfs/AI_Insurance_Rebuilds_for_API_First_Speed.m4a) instead.


The updates we’re rolling out required real architectural changes to our platform. So, unlike typical updates, we won’t be rolling these out immediately, instead we’ll gradually move instances over to the new architecture a few at a time. We’ll get in touch with each of you to let you know when we plan to transition your instance to the new architecture.


Most of the improvements that we’re making are behind the scenes. These improvements will allow us to more quickly and efficiently respond to your needs. We’re also hoping that once you start using the new system you’ll notice a lot of smaller quality of life improvements that we were able to make along the way.


Please reach out if you'd like to know when your migration is planned for.


#### 💡 Releasing an AI Native Platform


Improving our core architecture also allowed us to make our platform even more AI friendly. The new architecture is API-first: Every piece of data in your instance is accessible via api and, most importantly for building robust integrations, we now support detailed, machine readable apis for your instance configuration. This means that all of the customization we build for your instance is legible to tooling, automation, and agents interacting with our apis on your behalf. In addition we’re building an MCP server, by providing a structured layer for your AI agents to interact with we can reduce token usage and agent errors and improve agent task success rates. The MCP server is still in beta but you can get an idea of what it will be able to do by reading[the docs](https://docs.go.aiinsurance.io/api-reference/mcp-connector) .


#### 📊 A Re-Architected Data Model


We’ve re-architected our data model from the ground up to make it more consistent, flexible, and faster to configure. This is part of how we can complete onboardings so much faster and you’ll notice that you no longer need to ask for us to add particular fields as available filters, columns, or for export. Smart tags are a few clicks away for any field anywhere in the system. And complex reporting requirements are easily satisfied with our new powerful calculated fields to operate on and transform any data in your data model into the format you need.


Another benefit is you should see that pages load faster. Exports and rating are faster as well. And some exports (such as exporting a very large number of events with financial data) are more than 100x faster.


### What does this mean for you?


#### 🛠️ Reduce the Effort Necessary to Maintain an Insurance Program on our Platform


Insurance systems evolve and change over time. New requirements arrive, you find opportunities for improved efficiency, etc. You need your insurance platform to change with you. In the past changes required weeks or sometimes months of lead time to land. With our new system the vast majority of changes should take only days, and many changes will be ready in minutes with just a few mouse clicks.


#### ⚡ Spin Up New Programs At Lightning Speed


We’ve vastly improved the process for setting up new programs. We’ve built improved tooling and core architecture around every step necessary to onboard a new program. We’re confident we can fully onboard a new program in 60 days, faster depending on how ready you are. We’re already seeing great results from this new architecture with our first program on the new architecture onboarded in less than 30 days.


#### 📊 Improved Endorsements and Bordereau


Another big improvement we’ve made to the data model is how we handle endorsement data. We now support exporting policies as of a given effective date and the bordereau export feature now supports exporting **every** policy field exactly as it appeared when the corresponding policy transaction was bound. You no longer have to worry about which fields are supported by bordereau or respect the as-of-date when exporting.


This more robust endorsement data model powers our improved policy timeline & endorsement history UI


And policy activity log


If you need even more insight into how your policies have changed over time you can use our[policy transactions api](https://docs.go.aiinsurance.io/api-reference/policies/overview) to dig even deeper.


### More To Come


Everything we’ve talked about in this update is available today for new programs and programs that have been migrated to our new architecture. We have a lot more in store that we’re excited to share in future updates.


---


We hope you find these updates helpful. We will continue to improve the product as we move forward and are excited to keep sharing these updates with you! Please reach out if you'd like to know when these updates will be coming to your instance.


The AI Insurance Team
