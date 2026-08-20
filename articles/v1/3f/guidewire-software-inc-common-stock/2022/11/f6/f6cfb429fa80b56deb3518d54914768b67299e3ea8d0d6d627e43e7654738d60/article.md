---
schema_version: "1.0.0"
document_id: "f6cfb429fa80b56deb3518d54914768b67299e3ea8d0d6d627e43e7654738d60"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/technology/simplify-event-driven-integrations-on-guidewire-cloud-with-app-events"
published_at: "2022-11-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:92618fba6869909f489851e9357156de70ce0e4a1251cda1a598a60503f349bd"
---

# Simplify Event-Driven Integrations on Guidewire Cloud with App Events

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


- [Technology](https://www.guidewire.com/resources/blog/technology)


- Simplify Event-Driven Integrations on Guidewire Cloud with App Events


I was watching the 2022 Connections presentation on App Events, and I laughed when Simon Reading mentioned that with enterprise software, integrations are hard. He’s right! Most customers I’ve worked with have dozens of integrations to external systems, and all of them have their own languages and levels of complexity. Those external systems also weren’t designed for straightforward integration with other systems. Guidewire has been working hard with customers to solve this challenge with the Cloud Integration Framework, and with Flaine I’m thrilled to tell you more about a key component.


I’m excited to announce the General Availability of App Events for[ClaimCenter](https://www.guidewire.com/products/core-products/insurancesuite/claimcenter-claims-management-software) and[PolicyCenter](https://www.guidewire.com/products/core-products/insurancesuite/policycenter-insurance-policy-administration) customers on[Guidewire Cloud](https://www.guidewire.com/products/technology/guidewire-cloud) . App Events streamlines integrations and updates and increases developer efficiency by publishing business events to downstream systems without writing code.


App Events will help insurers:


-


Streamline integrations and platform updates with integration logic independent of the[InsuranceSuite](https://www.guidewire.com/products/core-products/insurancesuite) application.


-


Enable cross-application workflows with actions in downstream systems triggered in response to claim and policy lifecycle events in InsuranceSuite.


-


Simplify development by allowing systems to subscribe to InsuranceSuite business events configured using an admin UI or Integration Gateway.


App Events are best understood as part of Guidewire’s[Integration Framework](https://developer.guidewire.com/cloud-integration-framework-the-right-tools-for-the-job/) . Here are a few key components:


1.


InsuranceSuite Cloud API – An inbound API that allows external systems to deliver inbound data and trigger business actions inside of InsuranceSuite using an API.


2.


App Events – Makes it easy to get events and related data out of InsuranceSuite for integration purposes. App Events is an evolution of Guidewire’s event-messaging feature for self-managed InsuranceSuite deployments.


3.


Integration Gateway – Allows customers to write integration applications which bridge between InsuranceSuite and their downstream systems.


Next, let’s discuss why App Events are so important. In every insurer’s technology ecosystem, there are downstream systems that need to be notified when business events occur in InsuranceSuite. These systems need to be alerted for a couple common reasons, including keeping data in sync or triggering an automated action in the downstream system based on a business action in InsuranceSuite.


There are hundreds of downstream system examples; here are two straightforward scenarios. In a fraud scoring event, the third-party scoring system needs to know when there are claim or policy lifecycle events to trigger rescoring of the claim or the policy in order to trigger a remediation action. Or, in a situation where you’d like to notify policyholders (such as via SMS texts), App Events offers a way to subscribe to events happening in the Guidewire system to be able to trigger these notifications.


This just scratches the surface of the power of App Events. To learn more, check out the[2022 Connections session](https://connections.guidewire.com/event/2517e7e4-611e-4ecd-b9fb-7f5b88c28352/websitePage:9509b03b-96d8-4127-805a-bbd41c33716f) , “Simplifying Cloud Integrations with App Events” under the “Innovation Showcase” Track. Customers can also read App Events[developer documentation](https://docs.guidewire.com/gw-login) or take the training course on the[Guidewire Education](http://education.guidewire.com/) site. Just search for “App Events” when you get there.


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
