---
schema_version: "1.0.0"
document_id: "75d10485c02f469098ab04615a1c74eba99286875ca4b9da2b2a1200c90918d5"
company_key: "yc-cargo"
company: "Cargo"
source_id: "yc-cargo-news-import-f4a8864e899b"
canonical_url: "https://www.getcargo.ai/blog/business-entities-the-foundation-of-your-revenue-architecture"
published_at: "2024-04-08T00:00:00+00:00"
first_seen_at: "2026-07-21T12:34:39.888673+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:706d1f1d90ed8110890b892a0df7322578cecdf1e39b773ebdcfb93a1ad54322"
---

# Business entities: the foundation of your revenue architecture

In a world where everyone claims you need to be data-driven, you must “think data” to perform at the best level.‍


For business teams, this brings us to the concept of “business entities,” often misunderstood.


In general, entities are governed centrally by the data team, who ensures that business workflows & processes are powered by the best data.


What’s a customer? What’s a product-qualified lead? How do they relate to the subscription status?


If you ask the same questions to different departments in your company, you may have as many definitions as the number of people you ask.


Understanding the different types of business entities and their sub-entities is essential for[business alignment](https://www.getcargo.ai/blog/sales-and-marketing-alignment-the-role-of-a-single-source-of-truth) to make internal teams talk the same language, streamline operations, and power effective growth strategies.


Let’s jump in!


## What are business entities?#


Essentially, an entity represents an object and its related attributes and relationships. If you ever did some Object Oriented Programming, the concept is close to defining a Class. In the example below, you need to define a car with the main properties that belong to the object “Car.”


OOP: Object Definition


This is the same for entities. These distinct concepts are highly relevant to the operations of any organization. Common examples of entities include a company, a person, a transaction, or a workspace. Each has its own attributes and its relationships to other entities.‍


You have three types of relationships:‍


- **One-to-One** : In one-to-one relationships, only two entities can map onto each other; no other elements can. A real-world example would be Social Security numbers, which can only be assigned to one person at a time.
- **One-to-Many** : Imagine you have two tables in your Google Sheets - one for customers and one for their orders. In this scenario, each customer can have multiple orders, but each order can only be associated with one customer. This is a one-to-many relationship between the two tables.
- **Many to many** : Imagine another scenario where you have two tables in your Google Sheets - one for employees and one for Workspace. In this scenario, each employee can belong to multiple workspaces, and each workspace can have multiple employees. This is a many-to-many relationship between the two tables.


You might be wondering, “Okay, but how do I define the relationship between two entities?”


You need to have a common attribute in the two different entities, an ID most of the time, used to map them together. You can think about it as a Vlookup function in excel where you need to provide a common column to do the matching.


For instance, if you are looking to combine all your company data from different sources like CRM, lead forms, freemium signups, Clearbit enrichment, and Dealroom data into a single account?


Well, relying on just the domain name won’t cut it.


This calls for tailor-made logic that fits your unique setup. This is what we call “Identity Resolution”, the process of unifying different data sets to build a single definition of your customers, the famous 360° view.


Data Schema


Let’s take Slack Enterprise example.


An organization in Slack is the central entity. An org can group multiple workspaces and has multiple teams that group multiple people. People can also belong to multiple workspaces outside their organization, like slack communities.


Slack data schema


Slack data schema


Beyond the architecture and relationships, **each entity has inner properties. You can see it as a data schema that defines how data is organized and represented in a database. This will be helpful in getting rid of duplicate and redundant data (often referred to as “Normalization”)**


Now, Let’s dig a little deeper on why business entities are an important element of revenue operations.


## Why are business entities important?‍#


One of the most important outputs for revenue teams is that entities make marketing/sales segmentation far more powerful, enabling marketers to slice, dice, and build subsets of data rapidly.‍


Plus, leveraging the relationships between your entities allows you to create granular segments that cross-reference different entities.


For example, you could create segments of users based on specific criteria, such as those who work for companies with multiple headquarters or who have subscribed to your service within the last year but further narrowed down to a specific industry.


With this level of granularity, you can tailor your outreach and messaging to specific segments and ultimately increase the effectiveness of your go-to-market strategy.


1.


**Understanding of the organization** : Identifying and defining the various entities within a business allows everyone to understand the different components that make up the organization. This can help better allocate resources, define scope accountability and metrics ownership, set goals and priorities, and make informed decisions.


2.


**Improved communication** : When everyone uses the same language and definitions, it’s easier to ensure internal teams are on the same page and working towards common goals. It also helps bridge the gap between business & data teams.


3.


**More efficient operations & strategy** : A thorough understanding of business entities can help identify inefficiencies and streamline operations. For example, identifying sub-entities within users (dormant, active, product-qualified) can help identify funnel bottlenecks.


4.


**Let you own your business definitions** : Instead of being locked up by the definition of third-party tools, as every tool has its own data model, you can finally define your entities and their related properties for what matters for your business-specific need.


## Limitations of rigid data models#


The last point around owning your business definitions requires additional attention.‍


Let’s dig a little deeper again.


Third-party data models are typically designed to serve a wide range of businesses across different industries, which means they may not be tailored to the specific needs or requirements of your company.


If you need a “workspace” entity, you will struggle to find a tool that supports this definition. I don’t even mention the case where you work for an association, so you don’t have customers but “donors” - Good luck to find a CRM that will be able to deal with your custom business definitions.


Remember when we talked about identity resolution - How would you build the 360 vision if your tools are not using the same data model?‍


Let’s review two examples of B2B software and how their data models can be a limit for you. This is mainly about doing cross-entity segmentation.


- **Braze** : Braze is the customer engagement platform. Their data model is “user-based” only, which means that all customer interactions are tracked at the individual user level rather than aggregated at the account. This makes it challenging to segment customers based on account or workspace attributes, or to have a complete view of a customer’s company journey, as interactions may be spread across multiple users.
- **Pardot** : Now called “Salesforce Account Engagement” is surprisingly” user-based” as well, the name is kind of misleading haha. You are expected to do scoring & routing & nurturing on a “lead level”, a pretty tight angle to do it well.
- **Segment** : Segment offers a data model limited to two objects: users and accounts, and in most cases, a user can belong to only one account. Just imagine if you are Slack as we discussed earlier, You won’t be able to define the workspace op team entities, which can hurt the field of possibilities.


This is where data warehouses make sense!


Identity resolution


Contrary to CRM and marketing automation softwares, Data warehouses are‍


- **Flexible** : you can represent any entity you want within a warehouse, with their dedicated properties and relationships that matter for your business. The unopinionated DNA of data warehouses is a killer feature.‍
- **Exhaustive** : The data warehouse contains all the other data from your tech stack, including product data, Customer success, chat, CRM, product analytics, etc.. The 360° view of your customers already resides in your house‍
- **Secure** : You introduce your customer data to further risk by letting providers keep it while keeping them in your own private cloud can significantly reduce your security risk. You also benefit from greater observability of the health and state of your customer data compared to the hidden wall of external platforms.‍
- **Resilient** : The robustness of the modern data stack is designed to handle the large volumes of data that organizations generate and manage on a daily basis, making it the core data infrastructure used by scale-ups and mature companies.


This makes data warehouses the perfect candidate to be the[single source of trust](https://www.getcargo.ai/blog/sales-and-marketing-alignment-the-role-of-a-single-source-of-trust) for modern businesses. That’s why we decided at Cargo to sit on top.


## Most common entities definitions in B2B#


An entity is a versatile concept that encapsulates various elements within an organization.


B2B entities


Nearly everything can be considered an entity, from deals that bring in revenue to the contracts that solidify relationships to the invoices that keep the lights on.‍


However, we’ll be honing in on the most common Go-to-Market entities - particularly pertinent to the revenue generation process.‍


You might be wondering: Should I know the exact attributes related to an entity in advance?


Nope! You can start with the most important data and gradually expand with new relevant properties.‍


Of course, each business will have different entities depending on its business model, industry, and go-to-market motion.


Now, let’s have a look at the common entities with basic definitions:‍


1. **Users** : This refers to individuals who interact with a business’s products or services, such as customers or leads. This is the most common entity across any business type.


-


Sub-entities: MQL, SQL, PQL


-


Relationships: Has many (ie: belong to multiple related entities)‍


-


Related Entities: account, workspace, events, intents


-


Properties


User entity


1. **Accounts** : This refers to a record of financial transactions for a particular customer or business, often used for billing or invoicing purposes.


-


Sub-entities: Customer, Dormant, Lost


-


Relationships: Has many


-


Related Entities: Account, workspace, intents, Transactions‍


-


Properties


Account entity


1. **Workspaces** : This refers to a designated area where users can belong to


-


Sub-entities: Marketing, Sales, Operations


-


Relationships: Has many


-


Related Entities: User, account


-


Properties


Workspace entity


‍4. **Events** : Behavioral events attached to a user


- Sub-entities: Website activity, product usage
- Relationships: Has one
- Related Entities: User
- Properties


Events entity


1. Intents: B2B signals (ie:business-friendly momentum) attached to a person or a company.


-


Sub-entities: New hire, Job offers, Fundraising


-


Relationships: Has many


-


Related Entities: Account, User


-


Properties


Intens entity


1. **Transactions** : An exchange of value between two entities, such as a purchase or sale.


-


Sub-entities: Orders, subscriptions


-


Relationships: Has many


-


Related Entities: Account, Team, User


-


Properties


Transaction entity


## The link between the semantic layer and entities.#


Ever heard about the semantic layer?


You can see it as a reference used as source of truth in a company.


The semantic layer defines a standardized set of business terms and concepts used to describe the data and provides a consistent and easy-to-understand view of the data to end users.


It indirectly bridges the data literacy gap by providing a consistent way for your teams to interact with and analyze the data.


The link between entities and the semantic layer lies in the semantic layer being built on top of the entities you’ve defined for your company, most of the time from the data warehouse. The semantic layer is the abstracted layer that democratizes data access by providing one common language in your organization.


Semantic Layer


The semantic layer is today what APIs were in 2010. It acts as the cement that binds your tech stack, ensuring seamless integration and synchronization of all your systems and applications.


Say goodbye to the complexity and inefficiencies of manual integrations and API sync.


This concept brings us to the final point, the rise of the modern revenue stack.‍


## The Rise of the Modern Revenue Stack#


Data has become crucial for businesses to thrive and stay competitive in today’s digital landscape. However, traditional data architectures have faced challenges in keeping up with the increasing demands of data-driven enterprises.‍


As a result, the modern data stack has emerged as a game-changer for established companies and scale-ups. This term has become prevalent in the tech industry as businesses worldwide strive to harness the power of data.


Vala Afshar, the chief digital evangelist at Salesforce, believes that we should view data as a vital resource like water. The success of the world’s largest and most valuable companies is built on data. We need good systems to move data efficiently, just like how plumbing facilitates water movement.


He is right!


Many well-known companies like Coca-Cola, Deliveroo, and Canva have already embraced this modern data infrastructure to streamline their operations. And more and more companies are adopting this set of best-of-breed tools and technologies that enable them to create a more agile and scalable data infrastructure.


modern data stack


‍According to Gartner, 90% of business professionals identify data as a vital route to innovation, utilizing data to do everything from engaging new customers using data-driven marketing to selling more products using recommendation engines and personalized messaging.


data warehouse as source of truth


The data ecosystem has been revamped, but what about the Martech stack?


Still operating in silos with rigid proprietary architecture.


But…


A new era, “the modern growth stack” or “modern revenue stack,” emerged. A direct response to the complexification of go-to-market strategies in B2B, usually a hybrid approach of inbound, outbound, cross-sell/upsell, and account-based.


That also explains the rise of revenue operations as the function that will be the glue between people, processes, and technology. By 2025, 75% of the highest-growth companies in the world will deploy[a revenue operations (RevOps) model](https://www.gartner.com/en/articles/revops-the-modern-operating-model-for-fast-forward-organizations) .


What if we could bridge the data literacy gap by making data available easily to business folks, as again they are the biggest data consumers.‍


Introducing the **modern revenue architecture** on top of the Modern Data Stack:


‍


Revenue architecture


New software built from the ground up to leverage data warehouse potential (among other[systems of records](https://www.getcargo.ai/blog/the-role-of-cloud-data-warehouses-as-the-new-system-of-record) ), bringing the benefits of the MDS in their DNA, such as greater scalability, flexibility, security, and robustness.‍


The modern revenue stack will use data to optimize and orchestrate, focusing on workflows that drive the business forward. It will help ****streamline your processes, break down silos, and finally provide you the 360° customer view we have heard for 10 years now but have never been able to see truly.


Let’s merge disparate stacks and adopt a centralized[system of record](https://www.getcargo.ai/blog/the-role-of-cloud-data-warehouses-as-the-new-system-of-record) that powers all the go-to-market efforts, letting you orchestrate engaging customer journeys, improve customer acquisition efficiency, and maximize sales pipelines.


‍


Gartner modern revenue stack


Even though the modern data stack shows a lot of promise, it has created a lot of technical dependencies for business teams.


Enter Cargo, which wants to reconcile the best of both worlds: the reliability of the data infrastructure at the service of business missions (beyond reporting).


To accomplish it, we’ve made it easy for modern revenue teams to build workflows on top of the data warehouse that can automate go-to-market strategies and drive revenue more efficiently.


👉 Interested to learn more? Signup[here](https://www.getcargo.ai/) and we’ll get you onboard.


## Key Takeaways#


- **Business entities are the “classes” of your revenue data model** : Just like object-oriented programming, entities (User, Account, Workspace, Event, Intent, Transaction) define objects with attributes and relationships, the foundation for consistent definitions across teams
- **Three relationship types govern entity connections** : One-to-One (Social Security to person), One-to-Many (customer to orders), Many-to-Many (employees to workspaces), understanding these unlocks cross-entity segmentation and granular targeting
- **Third-party tools lock you into rigid data models** : Braze is user-only, Pardot is lead-only, Segment limits you to users + accounts, none support custom entities like “workspace” or “donor,” forcing you to conform to their definitions
- **Data warehouses enable entity ownership and flexibility** : Unlike CRMs, warehouses let you define any entity with custom properties and relationships, contain 360° customer data across all systems, and provide security, resilience, and observability
- **The semantic layer bridges entities and business users** : Built on top of your entity definitions, the semantic layer provides a consistent business vocabulary that democratizes data access, it’s the “API of 2025” that binds your revenue stack together


## Frequently Asked Questions#


‍
