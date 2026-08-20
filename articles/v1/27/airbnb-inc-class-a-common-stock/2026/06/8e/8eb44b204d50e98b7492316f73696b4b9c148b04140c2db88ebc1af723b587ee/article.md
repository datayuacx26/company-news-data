---
schema_version: "1.0.0"
document_id: "8eb44b204d50e98b7492316f73696b4b9c148b04140c2db88ebc1af723b587ee"
company_key: "airbnb-inc-class-a-common-stock"
company: "Airbnb Inc."
source_id: "airbnb-inc-class-a-common-stock-rss-d1a7958eece3"
canonical_url: "https://medium.com/airbnb-engineering/scaling-beyond-one-how-airbnb-evolved-its-data-architecture-for-a-multi-product-world-6125645d470c"
published_at: "2026-06-09T17:01:02+00:00"
first_seen_at: "2026-07-20T04:35:41.377728+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:e30accc202192308d4f96bc4bf9fd9912a59d8fff614e80012b62af5e46c1ce8"
---

# Scaling beyond one: How Airbnb evolved its data architecture for a multi-product world

# **Scaling beyond one: How Airbnb evolved its data architecture for a multi-product world**


## How Airbnb’s data engineers and analytics engineers built a consistent and flexible data modeling framework to support the expansion into Homes, Experiences, and Services.


[Patrick Lam](https://medium.com/@patrick_lam?source=post_page---byline--6125645d470c---------------------------------------)


9 min read


·


Jun 9, 2026


--


Press enter or click to view image in full size


**By** :[Patrick Lam](https://www.linkedin.com/in/pkphlam/) ,[Namrata Lamba](https://www.linkedin.com/in/namrata-lamba/) ,[Jamie Stober](https://www.linkedin.com/in/jamiestober/)


With the May 2025[Summer Release,](https://news.airbnb.com/product-releases/airbnb-2025-summer-release/) Airbnb redesigned its app, relaunched Experiences, and debuted Services, pushing us beyond our traditional Homes focus. For the data teams, this meant rapidly evolving a decade-old infrastructure to integrate two brand-new product pillars. Our data engineers and analytics engineers rose to the challenge by building a consistent and flexible framework to serve as a robust and scalable data foundation for the next decade of growth.


But getting there wasn’t straightforward. This fundamental shift surfaced a critical question for our data organization: **How do you evolve your offline data architecture to support new product lines without introducing disorder in vital analytics services?**


We knew the approach we took would have long-lasting implications. A fragmented strategy risked creating data silos, inconsistent analytics, and a tangled web of technical debt that would likely slow down future innovation. In this post, we’ll take you behind the scenes to share key decisions that we made, the framework that emerged, and the lessons that helped reshape our offline data warehouse for the future. Note that we focus specifically on our offline data warehouse (the analytics-oriented data infrastructure owned by our data engineers and analytics engineers) rather than the online data systems that serve the app directly, as the two domains have fundamentally different requirements, constraints, and design philosophies that warrant separate treatment.


## The core dilemma: separate vs. monolithic


The first and most critical question was how to structure offline data for the new, three-product world, with Homes, a refreshed Experiences product, and the new Services offering. This involved a trade-off between two main approaches:


1. **Separate data models:** This approach creates distinct sets of tables for each product line, keeping the data for each business clean and highly tailored, but incurring a higher incidence of duplicated logic across models.
2. **Monolithic model:** This approach combines all product lines into a single, unified set of tables to maximize code reusability and ensure consistency, but risks becoming unwieldy and less well-suited to the unique attributes of each product.


It became clear that neither approach was universally superior. The optimal choice depended heavily on the specific business domain. A model that was perfect for guest data, for example, would be suboptimal for payments data.


We chose a path that balanced consistency with flexibility. We established a framework that combined firm, centralized principles with decentralized modeling guidelines, empowering each data team to make the right choice for its domain.


## Our three foundational principles


To ensure a baseline of consistency across all teams, and to keep the door open for any new product categories that emerge in the future, we established three foundational principles. These principles ensured that no matter which modeling path a team chose, the results would be consistent, scalable, and easy for all data consumers to understand:


- **Principle 1: No hybrid data models.** A domain’s data model had to be either completely separate by product type or completely monolithic. We viewed this choice between two distinct paths as key for future scalability. It was important to avoid confusing, inconsistent situations down the road where some products might use combined data tables while others do not.
- **Principle 2: Consistent identifier naming.** To ensure reliable table joins and prevent confusion, we established a strict convention where the structure of primary identifiers was directly dependent on the modeling choice. Teams using separate models were required to use product-specific IDs (e.g., *id_experience* , *id_service* ). In contrast, teams using a monolithic model had to use a generic product descriptor ID (e.g., *id_product_listing* ) and include a product type column (e.g., *dim_product_type* ) to differentiate between Homes, Experiences, and Services.
- **Principle 3: Clear namespace organization.** We used namespaces to define clear placement for every table. Core, product-specific tables were placed in dedicated product namespaces while monolithic, cross-cutting tables lived in a global namespace. This structure was supplemented by team-specific namespaces, giving individual teams the flexibility to manage their own assets and intermediate tables.


These principles set firm boundaries for our modeling efforts and ensured a consistent foundation across the company.


## The modeling guidelines


With the foundational principles in place, we gave each team a set of guidelines to help them decide how to model their data. This empowered them to pick the right model for their specific domain, using a common set of considerations.


- **Shared vs. unique product attributes:** Do the different product lines (Homes, Experiences, and Services) share a common set of data attributes? Or are there many unique attributes for each product line that would make a unified model impractical?
- **Future evolution:** Will this model scale if we add a fourth product line? A fifth?
- **Upstream alignment:** How does the online production database structure its data?
- **Downstream consumers:** How will analysts and data scientists query this offline data?
- **Code maintainability:** Does one path lead to cleaner, more modular code?
- **Data volume & performance:** Can the model handle the scale of the new data that’s being added efficiently?
- **Compatibility:** How can we maximize backward and forward compatibility with the existing data and ensure future changes for Experiences and Services do not impact resiliency of the models powering the Homes product line?
- **Business Continuity** : How can we ensure accurate reporting for existing key metrics after introducing new data into the models, especially considering the relatively low volume of data that is processed initially and the need to handle unexpected values?


These guidelines provided a framework for every domain team to use in analyzing their specific situation.


## Putting the framework into action


When teams applied this framework to their specific data, a clear pattern emerged. While every guideline played a role, one question in particular proved to be the most decisive factor: Do the product lines share mostly common data attributes, or do they have significant unique attributes?


### Separate data models: for product-specific logic


Teams working on features closest to the user experience found that the product attributes were too distinct to combine. They overwhelmingly chose to build separate models to capture the unique nature of each product, particularly as we introduced several new concepts with our Services product. Some examples include:


- **Listings:** The biggest conceptual change came from the introduction of Service *offerings* . These are distinct activities or variants nested under a single parent Service listing. For example, a fitness host might offer yoga, pilates, and strength training, or a chef might offer several different menus. This many-to-one relationship between offerings and a parent listing was a new core concept that had no parallel in our Homes or Experiences products, making a separate data model necessary.
- **Availability:** For Service providers, we introduced the concept of business hours. Previously, hosts set availability with specific calendar dates (for Homes) or specific start times (for Experiences). Business hours allowed a guest to book at any time within a set timeframe (e.g., Mon-Fri, 9am-5pm). This required us to change our data models to translate these flexible hours into discrete, bookable time slots for guests.
- **Location:** Prior to this launch, listings for Homes and Experiences were generally at a single, fixed location specified by the host. With Services, a host could have guests travel to them, or they could choose to travel to their guests. This introduced the need for a Service area, allowing hosts to designate the geographic areas they were willing to travel to. This flexible, radius-based location concept required its own data model.
- **Guests:** The redesign of the app created entirely new user journeys. The way a guest interacted with the app, from the homepage and search all the way through the checkout steps, differed across product types. By its nature, interaction data has very high volume, which risks performance degradation. To accurately capture these distinct funnels and user behaviors, and ensure scalability and performance, separate data models were necessary.


### Monolithic data model: for cross-cutting concepts


While product-facing domains chose separation, other teams managing more foundational, cross-cutting concepts found that a monolithic model was a much better fit.


- **Messaging:** On Airbnb, a message thread is the common unit of analysis, and it can occur between guests and hosts, guests and customer support, or even between guests. Messaging can span multiple product types within a single thread. Separating the data models by product type would fragment these conversations, create operational challenges, and make it impossible to get a holistic view, so a monolithic approach was the choice.
- **Payments:** Our payments data models are largely product-agnostic by design. They are built to handle transactions, refunds, and payouts, regardless of what product is being purchased. New product types like Services could be onboarded within the existing monolithic data models with minimal changes.
- **Customer support:** Much like messaging, customer support requests can span multiple product types. A guest might contact support with an issue that involves both a Home they stayed in and a Service they booked. A monolithic model was the only way to maintain a single, unified view of a user’s support history.


This clear delineation, choosing the separate models approach for product-specific logic and monolithic models for cross-cutting concepts, allowed us to successfully model our new, complex business landscape. The framework gave us the flexibility we needed within a consistent and scalable structure.


## Navigating the challenges of standardization


Designing a framework on paper and implementing it across a large, fast-moving organization are two very different things. To make this initiative successful, we would need to navigate two significant, real-world challenges.


### **Creating the foundation of actionable insights**


Our offline data warehouse doesn’t exist in a vacuum. Upstream online data models and their corresponding event logging were rightly optimized for the immediate needs of running the app, such as transactional speed and stability, rather than the structural clarity that is ideal for offline analytics.


As a result, the raw data flowing into our warehouse was often structured in ways that were not ideal for analytics. This reality underscores a core tenet of our data strategy: the offline data warehouse must act as a crucial translation layer. Taking the raw production data and transforming it into a standardized, reliable source of truth is a key function performed by our data engineers and analytics engineers, enabling our downstream consumers to surface insights quickly and accurately.


### **Managing data debt while making progress**


The new standards gave us a clear vision, but they also highlighted legacy tables and dashboards, especially from the older version of Experiences, that no longer met the criteria. Migrating and deprecating these assets is always a massive undertaking. These tables often have hundreds of downstream consumers, so the process requires extreme care, involving extensive communication, dual pipeline runs for validation, and a painstakingly slow deprecation cycle to avoid breaking the business processes that depend on each asset.


These challenges are the reality for any data organization supporting rapid product innovation. Months after our initial launch, we continue to refine our translation layers and to carefully migrate legacy assets.


## The takeaway


The journey to a multi-product data architecture was as much about people and process as it was about technology. By establishing clear principles while empowering teams with a flexible set of modeling guidelines, we successfully navigated the data complexity of launching the new Services product line and overhauling the existing Experiences product line, while ensuring that functionality within the core Homes product line was not placed at risk.


This journey reinforced a key principle of data modeling at scale. The best answer is rarely “one size fits all.” It’s about creating a system that balances central consistency with domain-specific flexibility, and having the discipline to not only build for the future but also to thoughtfully address the past. It’s a continuous effort, one that has already paid dividends in scalability, clarity, and our ability to deliver insights quickly and accurately.


If this type of work interests you, check out some of our[related positions](https://careers.airbnb.com/) !


## Acknowledgments


This was one of Airbnb’s biggest product releases ever and the data work behind it was a true team effort. Thanks to all who contributed big and small.


*All product names, logos, and brands are property of their respective owners. All company, product, and service names used in this website are for identification purposes only. Use of these names, logos, and brands does not imply endorsement.*
