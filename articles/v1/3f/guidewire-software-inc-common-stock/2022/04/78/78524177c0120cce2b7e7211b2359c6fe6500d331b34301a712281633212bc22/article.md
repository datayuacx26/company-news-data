---
schema_version: "1.0.0"
document_id: "78524177c0120cce2b7e7211b2359c6fe6500d331b34301a712281633212bc22"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/using-advanced-product-designer-with-development-teams"
published_at: "2022-04-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:dfe4f9deda647a34332fb2a06c200c8c47d819f4dc700ef5cccfbcdc48c4739a"
---

# Using Advanced Product Designer with Development Teams

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- Using Advanced Product Designer with Development Teams


As a Senior Business Architect at Guidewire, I help customers implement and use our software every day. Recently, I’ve been able to see first-hand how[Advanced Product Designer (APD)](https://www.guidewire.com/developers/developer-tools-and-guides/advanced-product-designer) helps business owners and development teams build better, more complete lines of business.


I’d like to share a few examples of what it’s like to use APD to keep business owners and development teams aligned — from the first conceptualization of the line of business all the way through to implementation.


## What is Advanced Product Designer?


APD is a business tool that helps design, simulate, and deploy insurance products. It enables delivery teams to bring products to market using a dynamic, collaborative methodology that minimizes the initial effort to create a new line of business.


## What Does APD Mean for Developers?


As a developer, you produce code that needs to meet the expectations of the business to deliver value.


Whenever I’m working on a new line, it’s important that I keep in mind the balance between the complexity of the work I’m doing and the impact it will have on the project. Both high- and low-complexity work can have a big impact, but we need balance between the two.


In projects, identifying more complex requirements is often hindered by the need to configure the basics — coverages, entities, and screens required — as a foundation to confirm that we understand the requirements so we can gain business acceptance.


With the introduction of Advanced Product Designer, I can achieve a deeper understanding of the product and business expectations faster because APD automates much of the low-value, high-impact development required to implement a new line. It frees me up to focus on high-complexity, high-value development activities that create a richer product offering.


## APD in Action


Here’s how I’ve used APD in recent projects with Guidewire customers.


### Conceptualization


The process for delivering a new line of business with APD starts with a good design. This design is achieved through workshops led by a business architect that also include the product owner, subject matter experts, and the development team. Having developers involved early in the process is extremely helpful for ensuring team alignment throughout the project. During these workshops, we ask the insurer to answer questions like the following:


- What exposures will your line protect?
- How will you protect these exposures?
- How will you price your policies?
- What forms will you need to produce and what data will be required to generate the forms?
- How will you evaluate risk?


In a recent project, I was involved in a series of workshops with a customer that was building a modernized Personal Auto line. During these workshops, the development team got a comprehensive overview of the business expectations for the new line of business.


As the workshops proceeded, the developers worked with the business architect to create a mind map — a conceptual model that supported the business expectations and provided recommendations on ways to improve the design.


### Visualization


One of the advantages of Advanced Product Designer is the ability to import the mind map (conceptual model) and receive immediate feedback from the product owner and SMEs. Here’s an example of that mind map after it’s imported to Advanced Product Designer:


It can be very challenging for developers to quickly incorporate feedback from the business once development starts. However, by visualizing the line in PolicyCenter first, before any development had started, the team was able to execute the submission workflow and obtain feedback confirming that the requirements gathered in the workshop met the needs of the business.


As we implemented the Personal Auto line, the visualization phase proved critical. Testing how things would really function in PolicyCenter after the product was deployed often resulted in clarification of requirements that may not have been caught until development. This gave the whole team an opportunity to answer questions about the PolicyCenter experience. The visualization process led to fewer missed requirements and limited the need for rework once development started.


### Finalization


The conceptual model and visualization gave the development team a strong understanding of the business expectations for the new line of business. With this alignment, the developers were able to proceed with development and successfully generate a new line of business.


## Implementing a New Line of Business


Advanced Product Designer aids in the development process by automating the generation of a new line of business. This doesn’t mean we have a complete product. Rather, we’re able to automate many of the files that would traditionally require a developer to spend time coding as part of a new line of business. These automated files can include new entities, attributes, typelists, coverages, UI, and APIs.


### Automated Code Generation


Advanced Product Designer’s code generation process automates much of the high-effort, low-value work for which we traditionally use developers to implement. Of course, this doesn’t negate the need for developers. Instead, it enables them to focus on the high-value development necessary to configure a new line of business.


When we were implementing the Personal Auto line with our customer, we didn’t need a developer to spend time creating new coverages, entities, or typelists. Creating those manually is time consuming and often prone to error. That’s because implementation typically requires copying information between a requirements document and the application.


Instead, APD’s automated code generation enabled our customer’s developers to reduce the number of hours required for this work, avoid errors, and redirect their focus to high-value tasks like rating, form definition, and integration.


### API Generation


Advanced Product Designer’s code generation automates the creation of the Cloud REST APIs and App Events based on the conceptual model designed in the workshop.


This alleviates an enormous amount of work for developers, including generation of the schemas, mappings, and updaters. After finalization, integration teams can immediately use the APIs and App Events for integrations with internal Guidewire systems or vendor solutions via our Integration Gateway. This helps our development team begin exploring gaps between the integration requirements and the APIs, rather than spending hours coding APIs prior to, or in coordination with, a gap analysis.


When you consider the benefits we gained from using Advanced Product Designer at each stage of creating a new Personal Auto line, it’s clear why this insurer has now made APD a foundation of its development process. We were able to save time, focus resources on the most complex product design and implementation challenges, and create a more complete product offering with less effort than other approaches require.


## Related Guidewire Resources


- [Advanced Product Designer Guidebook](https://community.guidewire.com/s/article/Advanced-Product-Designer-Guidebook?_gl=1*18jyf3w*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczMzk0NjU5NC4xMjcuMS4xNzMzOTQ3MTg2LjQ0LjAuMA..) (Community login required)
- [Advanced Product Designer for Guidewire Implementations – FAQ](https://community.guidewire.com/s/article/Advanced-Product-Designer-for-Guidewire-Implementations-FAQ?_gl=1*14to3dy*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczMzk0NjU5NC4xMjcuMS4xNzMzOTQ3MjAwLjMwLjAuMA..) (Community login required)
- [Guidewire Marketplace: Advanced Product Designer templates & GO Products](https://marketplace.guidewire.com/s/search?collection=GO%20Products&_gl=1*1l20goi*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczMzk0NjU5NC4xMjcuMS4xNzMzOTQ3MjE0LjE2LjAuMA..#q=(%40sfMP_Product_Type__c%3D%22GO%20Products%22)&t=Marketplace&sort=%40sfmp_downloaded__c%20ascending)


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
