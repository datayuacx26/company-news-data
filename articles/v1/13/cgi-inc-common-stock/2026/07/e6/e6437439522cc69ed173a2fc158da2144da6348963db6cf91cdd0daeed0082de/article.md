---
schema_version: "1.0.0"
document_id: "e6437439522cc69ed173a2fc158da2144da6348963db6cf91cdd0daeed0082de"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/big-data-analytics/nissan-europe-customer-master-data-management"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:00d58b68a910383ad22f90ef5fa7a5cc63728215e433e9a635779322496293a8"
---

# Managing customer data as an asset at Nissan Europe

- Improved campaign conversion rates using preferred channels
- 1.5X more effective campaign conversions by targeting less…to spend less
- 2.5X more effective campaign conversions by targeting better…to win more


Nissan Europe achieved these inspiring results by partnering with CGI to establish a customer master data management platform.


At Informatica World 2015,[Valérie Clert](https://fr.linkedin.com/in/valerieclert) from Nissan and[Christophe Jeandidier](https://fr.linkedin.com/pub/christophe-jeandidier/2/b95/194) from CGI presented these results to a packed audience. Let me share them with you now.


**The challenge**


Nissan Europe was facing a declining customer retention rate and a customer renewal rate that was lagging the competition. The shortfalls were driven by several factors, including poor customer data management leading to poor customer communications. Since personalized communication is a key to customer loyalty, Nissan’s strategy was to “move from a car manufacturer to a service provider.” A 360° view of the customer was needed to support a personalized dialog.


The Nissan Customer Database project was started to create that 360° customer view. The challenges included:


- 100+ data sources managed in silos¾each holding information around the customer life cycle
- Very heterogeneous data models
- Lack of visibility into Nissan’s end-to-end processes
- Operations in 24 countries with different systems and required customizations


**The solution**


Let’s take a closer look at the top challenges the project team faced and how they were solved:


*Selecting the right data to persist in the customer database*
Since concurrent information was being provided on the same customer entities, how would they choose the right data to persist? The first step was to analyze for accuracy and reliability the data coming from the different source systems. Based on this analysis, a source priority score was defined to determine the leading system for each of the entities in the customer database. Also, the latency of the data had to be taken into account, with more recent data receiving a higher score. The combination of both source priority and latency score determines which source data is kept in the Nissan Customer Database.


*Creating a single customer view of data*
Imagine hundreds of source systems providing potential data for the same customer, the same company, and the same vehicle. Each system provides only part of the information and in different formats. A single, unique version of the customer was needed in the Nissan Customer Database, and it needed to be the best version possible. The team used advanced clustering and matching techniques to first identify groups of data that most likely belonged to the same customer, then cleansed the data, and then applied matching algorithms to identify the likelihood of two entities being the same customer.


*Ensuring quality of postal address data*
A key success factor in a marketing campaign tool is ensuring the quality of postal addresses used to decrease the costs of undelivered mail, and to increase the rates of efficiency of the campaign. Addresses have different formats in different countries, posing a tough challenge when designing a common solution. The team leveraged the Informatica toolset to validate, enhance and correct the addresses received from the different source systems, taking into account local differences for each country.


Today, 16 countries of Nissan Europe are live with the Nissan Customer Database and experiencing the benefits of improved customer insights about: who are their customers, who is most likely to be loyal to the brand, when will they likely change their cars, and what should be their next Nissan car.


A single view of each customer allows Nissan to communicate in a personalized way, but also allows them to define patterns and predict key triggers requiring specific actions. CGI is proud to be of service to Nissan based on our[customer intelligence services](http://www.cgi.com/en/business-intelligence-services/customer-intelligence) and in-depth experience in customer analytics.
