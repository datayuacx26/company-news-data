---
schema_version: "1.0.0"
document_id: "2d4e8071afc69d6b5654cabe79aa879fa2e8344309b84bd259be2eb44dcc371f"
company_key: "yc-hightouch"
company: "Hightouch"
source_id: "yc-hightouch-news-import-8089bfc075a8"
canonical_url: "https://hightouch.com/blog/composable-data-schema-advantage-financial-services"
published_at: null
first_seen_at: "2026-07-25T08:06:20.021758+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:a69b8fd054a0d87a25a82cc1974b9a72bd323f591e70e5568dceb36b8e7ccfe7"
---

# The composable advantage part three: Why most CDPs struggle with financial services personalization

*The following is a guest post from Dio Favatas, Head of Financial Services Adtech & Identity Strategy at Capgemini. Capgemini is an AI-powered global business and technology transformation partner that delivers end-to-end services and solutions across strategy, technology, design, engineering, and business operations.*


In parts[one](https://hightouch.com/blog/composable-cdp-advantage-financial-services) and[two](https://hightouch.com/blog/composable-identity-advantage-financial-services) of this series, we explored why customer data and identity resolution work best when they remain inside a financial institution's own environment.[Composable CDPs](https://hightouch.com/blog/composable-cdp) make this possible by activating data directly from the warehouse and resolving identities within the institution's existing security and governance framework.


In this post, we'll dive deeper into one of the most important, but perhaps least understood, advantages of a Composable CDP: its ability to adapt to your data schema, rather than forcing your business into a predefined customer model.


This may sound like a technical detail, but it has a direct impact on some of the most important outcomes financial institutions care about: personalization, cross-sell, customer retention, and increasingly, AI success.


Over the course of my career, I've seen financial institutions spend months – and sometimes years – trying to implement CDPs that promised a unified view of the customer. Too often, those projects become exercises in reshaping customer data to fit the platform rather than using it to create business value.


To understand why this happens, we must first look at what a data schema actually does.


## What is a data schema and why is it important for personalization?


A data schema defines how data is structured, including the entities your business tracks and the relationships between them. It defines what exists, how it is organized, and how different pieces connect.


If a platform cannot accommodate your specific schema, it cannot accurately represent your data, which severely limits the information you can use for personalization.


Every financial institution operates on a unique schema:


- A retail bank structures customer data differently than an asset manager.
- An insurance provider relies on different entities and relationships than a commercial lender.
- Even two institutions in the exact same vertical will organize data differently based on their proprietary internal models.


In a standard data schema, separate tables – such as Customers, Checking accounts, and Credit cards – are linked together by a common key, like a User ID. This relational connection is how a system knows that Sally Thompson, in the example below, holds both a checking account and a credit card.


## Why traditional CDPs struggle to support financial services data schemas


Most CDPs were built around a simplified view of the business, and assume two concepts are sufficient to represent customer data:


- A single customer profile represents each customer
- Events represent everything that happens around that customer at a point in time, such as page views, purchases, or email opens


The problem is that financial institutions operate in a much more complex reality.


### **Problem 1: Financial services entities don't fit the traditional users-and-events model**


Financial institutions operate on rich data models containing checking accounts, credit cards, mortgages, loan applications, households, advisors, businesses, and many other entities. Each has its own attributes, relationships, and current state. A loan application can be submitted, approved, denied, or withdrawn. A checking account balance changes continuously throughout the day. A household can contain multiple customers, accounts, and financial relationships.


To compensate, organizations typically resort to one of two workarounds, both of which require significant data transformation, create ongoing maintenance overhead, and only partially address the underlying limitations.


#### **Workaround #1: Turn entities into events**


One common approach is to represent every single update to a financial entity as an event. Instead of modeling a credit card as its own distinct entity, the institution must send a new event to the CDP every time a change occurs:


- *Credit card opened event* (Type: Platinum Rewards, Limit: $15,000, Status: Active)
- *Credit card suspended event* (Type: Platinum Rewards, Limit: $15,000, Status: Suspended)
- *Credit card reactivated event* (Type: Platinum Rewards, Limit: $15,000, Status: Active)


While these event triggers are useful for launching immediate, real-time campaigns, major challenges emerge when events become the *only* way to represent the underlying product.


**Routine personalization becomes overly complex**


Because events describe what happened, rather than represent the current state of the credit card, answering basic questions becomes surprisingly difficult.


Consider a simple query: which customers currently have an active credit card?


Because there is often no native concept of a credit card entity in a traditional CDP, the current state may need to be inferred from a sequence of historical events:


- Credit card opened
- Credit card suspended
- Credit card reactivated
- Credit card closed


The logic a marketer would have to build might look something like this:


**Has performed "Credit card opened"** AND **has not performed "Credit card closed"** AND either **has not performed "Credit card suspended"** OR **has performed "Credit card reactivated"** * after the most recent **"Credit card suspended" event**


Answering what should be a straightforward question now requires reconstructing the current state of every account from historical events, carefully accounting for edge cases, and trusting that every event arrived in the right order, with the right information attached. That requires more technical skill, creates a high margin for error, and makes audience building nearly impossible for marketers to do on their own.


That complicated logic gets even harder as the business becomes more complex. What happens if a customer closes one credit card and opens another? What if they hold multiple credit cards at the same time? What if one card is active and another is suspended?


As financial institutions add more products and customer relationships, a tool meant to empower marketer self-service increasingly struggles to fulfill that purpose.


**Every change becomes another event**


As more aspects of the credit card need to be exposed, the number of events grows quickly. Changes to credit limits, rewards balances, delinquency status, payment activity, fraud alerts, card replacements, and countless other attributes may each generate additional events. Many of those events also repeat the same contextual information – such as the card type, credit limit, and account status – simply to describe the latest change.


Over time, what began as a simple event stream turns into a sprawling collection of historical events. Every new event type must be generated, documented, stored, and maintained. As the event model grows, it becomes increasingly difficult to understand which events should be used, how they relate to one another, and whether they accurately reflect the current state of the business.


That growing complexity doesn't just make the data harder to manage, it also makes it harder to trust. As more event types, transformations, and historical logic are introduced, confidence in the data gradually erodes for both marketers and data teams.


At the same time, organizations incur the cost of generating, processing, and storing an ever-growing stream of events, often while making tradeoffs about how much historical data they can afford to retain for future segmentation and personalization.


### **Workaround #2: Flatten entities into customer attributes**


Another common workaround is to represent entity data as customer profile attributes.


For example, instead of modeling a credit card as its own entity, a bank might expose attributes on the user profile such as:


- Credit card status = Active
- Credit card type = Platinum Rewards
- Credit limit = $15,000
- Rewards balance = 24,500


On the surface, this approach may seem straightforward, but it quickly breaks down in several important ways.


**Customer profile fields get overwritten, creating an inaccurate view of the customer that isn’t reliable to use for personalization**


If a customer has multiple credit cards, checking accounts, loans, or applications, those values quickly begin to conflict and overwrite each other. Imagine a customer with both a Platinum Rewards card and a Cash Back card. If the Cash Back card is closed, a single ‘Credit card status’ field may now read as closed, even though the Platinum Rewards card remains active. What's more, from the marketer's perspective, there's no indication that the profile is incomplete or inaccurate – and no way to verify it.


**Maintaining customer profiles requires increasingly complex data modeling**


To work around this, teams may introduce increasingly complex profile structures, with fields such as ‘Card 1 status’, ‘Card 2 status’, and ‘Card 3 status’, along with matching fields for credit limits, rewards balances, product types, and dozens of other attributes.


As profile schemas grow, marketers are often left choosing between multiple fields with similar names, legacy attributes, or product-specific variants. It becomes increasingly difficult to know which field is current, which is deprecated, and which should be used for personalization.


The goal of a CDP is to make customer data more accessible to marketers. But as profile schemas grow more complex, marketers still rely on data teams to evolve the underlying profile schema before they can launch campaigns that rely on new products, attributes, or personalization use cases.


**Current state requires constant synchronization**


Keeping attributes current also becomes a significant operational challenge for fields that change frequently. Marketers might want to target customers whose:


- credit card utilization exceeds 80%
- who are within 500 points of a rewards redemption
- or whose checking account balance recently crossed a threshold


Credit utilization, rewards balances, and account balances update continuously. The current state must be synchronized back to the customer profile continuously, creating more pipelines to maintain and more opportunities for stale or inconsistent data.


**Complex relationships don't fit**


These challenges become even more pronounced for concepts like households or businesses. A household isn't simply an attribute of a customer – it has its own members, accounts, assets, and relationships. A marketer might want to:


- target households with more than $1 million in combined assets
- send only one direct mail piece per household
- or suppress every member of a household after one person accepts an offer


Representing those relationships as customer attributes quickly becomes impractical.


### **Problem #2: Financial institutions rarely have just one view of the customer**


Even if financial services entities could be represented perfectly, another architectural limitation remains: traditional CDPs assume there is a single representation of every customer.


Financial institutions rarely operate that way.


Different business units often need different representations of the same individual. A retail banking team, wealth management team, commercial banking team, and lending division may all interact with the same person, but each has different products, governance requirements, and marketing objectives.


The same is true for[identity resolution](https://hightouch.com/blog/composable-identity-advantage-financial-services) . Some communications – such as legal notices or regulatory disclosures – require an extremely strict, deterministic view of the customer. Others, such as paid media campaigns, benefit from a more flexible, probabilistic view that brings together additional context and identifiers. Both representations are valid, they simply serve different purposes.


Traditional CDPs force organizations into a single customer model. Institutions must either simplify those different representations into a single view or compensate by creating workspaces, or even separate CDP instances. The result is additional governance, synchronization, and maintenance overhead, making it harder to coordinate communications, identify cross-sell opportunities, and deliver consistent customer experiences.


### **The result: Personalization never becomes truly self-serve**


Taken together, these two architectural limitations explain why traditional CDPs have struggled to deliver on the promise of personalization in financial services. Personalization never becomes truly self-serve. Instead, it remains permanently dependent on data engineering to model, synchronize, and maintain customer data. Implementations take months – or even years – and many organizations ultimately discover they've spent significant time and resources reshaping and maintaining their data without ever fully enabling the personalization they set out to achieve.


## How a Composable CDP changes the model


[Composable CDPs](https://hightouch.com/blog/composable-cdp-advantage-financial-services) aren't bound by these limitations, which makes them uniquely well-suited for the complexity of a financial institution.


Customers, checking accounts, credit cards, mortgages, applications, households, businesses, and more remain first-class entities. There is no need to flatten them into customer attributes or reconstruct them from events. And organizations also aren't forced into a single customer model, allowing different teams to work from the customer representation that's most appropriate for their needs.


That has real consequences for how teams build, move, and scale.


That said, it is worth being precise about what a Composable CDP does and what it depends on. Because a Composable CDP works directly against the institution’s warehouse, the quality of what it delivers grows with the quality of the data underneath it. The data and entities in scope need to be modeled and reliable, but institutions with maturing data models can simply start with a limited set and expand over time.


That is not a criticism of the composable approach – it reflects the reality that financial institutions already invest in these capabilities because they are foundational to analytics, operations, risk, and compliance. A Composable CDP simply builds on that investment. Rather than conforming a second, flattened customer model to the CDP for every new marketing use case, institutions model their data once in the warehouse in a way that reflects how their business actually works. The result is less duplication, less ongoing maintenance, and a platform that can support far broader use cases over time.


**Faster implementations**


Because the platform works directly with the institution's existing warehouse schema, teams don't have to copy customer data into a separate system or remodel it to fit the CDP. By eliminating both steps, organizations can go live and start personalizing campaigns in weeks instead of months.


**Less maintenance over time**


As new products are introduced or business needs evolve, marketers don't need to submit a ticket and wait for the data team to add new fields, rebuild pipelines, or redesign customer profiles. New data in the warehouse is immediately available for segmentation and personalization.


**Simpler personalization**


Questions that previously required reconstructing history or maintaining thousands of profile attributes become simple filters on the underlying entities.


Instead of building complex audience logic, marketers can simply query:


- Credit card status = Active


The same applies to many other financial services use cases:


- Rewards balance > 50,000
- Checking account balance > $50,000
- Credit utilization > 80%
- Mortgage type = Conventional
- Estimated home equity = $256,928


Rather than adapting marketing questions to the limitations of the CDP, marketers work directly with the business concepts they already understand.


**Complex relationships remain intact**


Because households, businesses, advisors, and other entities remain part of the underlying data model, marketers can build audiences using those relationships directly instead of approximating them through customer attributes or event logic.


For example, marketers can target:


- Households with more than $1 million in combined assets
- Customers whose advisor manages more than $10 million in assets
- Businesses with multiple active lending products


**Multiple customer models supported**


Because the platform works directly with the warehouse, organizations aren't forced into a single customer model. Different business units can work from the customer representation that best fits their products, governance requirements, and marketing objectives, while still operating on the same underlying data.


The same flexibility applies to identity resolution. Teams can use deterministic identities where precision is required and[probabilistic identities](https://hightouch.com/blog/announcing-adaptive-identity-resolution) where greater reach is more valuable, without creating separate CDP instances.


## Why schema flexibility matters even more in the age of AI


Schema flexibility becomes even more important as financial institutions adopt AI.


An AI system can only reason about the data it has access to. Traditional CDPs often contain only a subset of customer information because every new entity, attribute, and relationship requires additional modeling work and adds additional storage costs. If information about accounts, products, applications, households, or advisor relationships never makes it into the CDP, the AI can't use it.


A Composable CDP changes this dynamic. Because it works directly with the warehouse schema, AI can reason over the same entities and relationships the business already uses to understand its customers.


Most financial institutions are approaching AI thoughtfully, and many are still evaluating where it fits. But the data foundation they build today will determine what future AI systems can see, understand, and act upon.


## Conclusion


Traditional CDPs promise a complete view of the customer, but in reality, they require financial institutions to leave much of their customer reality behind.


Composable CDPs don't ask you to reshape your business to fit the platform. They adapt to the way your business already works. Schema flexibility is what closes that gap, and it's what makes the original promise of the CDP finally achievable.
