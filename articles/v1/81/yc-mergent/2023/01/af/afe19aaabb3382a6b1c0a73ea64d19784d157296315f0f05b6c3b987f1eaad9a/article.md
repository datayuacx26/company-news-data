---
schema_version: "1.0.0"
document_id: "afe19aaabb3382a6b1c0a73ea64d19784d157296315f0f05b6c3b987f1eaad9a"
company_key: "yc-mergent"
company: "Mergent"
source_id: "yc-mergent-rss-c4a67b378e23"
canonical_url: "https://blog.mergent.co/the-true-cost-of-building-vs-buying-software-infrastructure"
published_at: "2023-01-17T20:01:13+00:00"
first_seen_at: "2026-07-25T13:51:58.649676+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:07a52f7a9018e83a28c941471394f381209369712bdab3520ea4dced3c010705"
---

# The True Cost of Building vs. Buying Software Infrastructure

When it comes to software infrastructure, teams have two main options: building from scratch, ideally on top of open-source tooling, or buying from a vendor. Both options have their own pros and cons, and the decision ultimately comes down to your specific needs and goals.


In this blog post, we'll explore the pros and cons of building vs. buying software infrastructure to help you decide which option would be best for your situation, along with a list you can use to calculate your software's total cost of ownership (TCO) and an example using Apache Kafka.


The real meat of this post is in the TCO calculation, so if that's what you're here for, feel free to skip the following two sections.


## Building - Pros and Cons


Building software infrastructure from scratch can be an excellent option for teams with unique or specialized needs that off-the-shelf solutions cannot meet. By building your own software, you can tailor it precisely to your needs, ensuring that it does exactly what is needed and integrates seamlessly with existing systems.


Additionally, building gives you complete control over the development process and allows you to make changes and updates quickly and easily.


However, building software infrastructure from scratch also has its downsides. One of the biggest cons is cost. Developing software from scratch can be a time-consuming and expensive process. Building your own software also has a higher risk of bugs and security vulnerabilities.


Before we dive into calculating costs, let's discuss the alternative: buying.


## Buying - Pros and Cons


Buying software infrastructure off-the-shelf can be an excellent option for teams that want a proven solution that has been tested and validated by other organizations. Off-the-shelf software is generally less expensive and less time-consuming to implement than building your own software from scratch. Additionally, buying software from a vendor can provide access to technical support and updates, which can be essential for ensuring the ongoing security and performance of the software.


However, like building, buying software infrastructure off-the-shelf also has its downsides. One of the biggest cons is that off-the-shelf software may not be tailored to your specific needs and may require additional customization to work effectively. Additionally, buying software off-the-shelf limits your control over the development process and makes it more challenging to make changes and updates to the software.


## Total Cost of Ownership (TCO)


Whether building or buying, the TCO of software is often miscalculated or even overlooked entirely. We've put together a short "checklist" you can use to calculate the total cost of ownership.


*When calculating the TCO, be sure you calculate the cost for each team involved (e.g., if you have separate infrastructure and development teams, consider the TCO for both independently).*


#### Up-front costs


-


software cost & licensing, if applicable


-


learning & education


-


implementation & testing (including data migration costs)


-


documentation & knowledge sharing


-


customization


#### Ongoing costs


-


direct infrastructure costs (e.g., hosting & storage)


-


backup infrastructure costs (e.g., failover & additional AZs)


-


supporting infrastructure costs (e.g., monitoring & alerting)


-


maintenance, patches/upgrades, & support


-


feature additions


#### Team & opportunity costs


-


hiring to replace the engineers now working with the new software


-


time spent on infrastructure that could otherwise be spent on core product


### TCO Example: Apache Kafka


Using a simplified version of the above list, let's dig into an example with Apache Kafka, a popular open-source distributed streaming platform many companies use to build data pipelines and send data between services.


To remain unbiased, we'll compare it to[Heroku's Apache Kafka as a service](https://www.heroku.com/kafka) Standard plan. This means we are looking for the following specs:


-


Capacity: 300GB


-


Retention: 2 weeks


-


vCPU: 4


-


Ram: 16GB


-


Brokers: 3 Kafka, 5 Zookeeper


In this example, we assume both the infrastructure engineer and the software engineer have all-in costs of $200k/yr, or $3,846/mo ( *note: $200k is very low in the United States when including benefits, shares, bonuses, & perks* )


Building (on AWS) Buying (Heroku)


software cost & licensing $0 (OSS) $21,600


learning & education $7,692 (2 eng * 1 week) $3,846 (1 eng * 1 week)


implementation & testing $15,384 (2 eng * 2 weeks) $7,692 (1 eng * 1 week)


infrastructure costs $12,117.60 (3x kafka.m5.xlarge + storage) $0 (included in software cost)


supporting infrastructure costs $1,200 ($15 per host per month, 5 hosts) $1,200 ($15 per host per month, 5 hosts)


maintenance, patches/upgrades $15,384 (2 eng * 2 weeks spread throughout the year) $7,692 (1 eng * 2 weeks spread throughout the year)


**Year 1 TCO** **$51,777.6** **$42,030**


Yes, you're reading that right: **buying your Kafka cluster from Heroku is cheaper than building with AWS.**


## Conclusion


When it comes to software infrastructure, teams have the option of building it from scratch or buying it off the shelf. Both options have their own set of pros and cons, and the decision ultimately comes down to the specific needs and goals of the organization.


Teams should carefully consider their specific needs and goals, as well as the cost and resources required for each option. While building software infrastructure can be a good choice for teams with unique or specialized needs, buying off-the-shelf software can be a more cost-effective and low-risk option for organizations that want a proven and supported solution.
