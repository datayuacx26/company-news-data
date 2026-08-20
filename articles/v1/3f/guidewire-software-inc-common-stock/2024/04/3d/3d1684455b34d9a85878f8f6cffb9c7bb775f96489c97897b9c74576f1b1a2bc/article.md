---
schema_version: "1.0.0"
document_id: "3d1684455b34d9a85878f8f6cffb9c7bb775f96489c97897b9c74576f1b1a2bc"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/how-to-streamline-business-process-automation-with-autopilot-workflow-service"
published_at: "2024-04-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:55d76249a15d2fdfd3b1d136bf270af1bcdae7bd353c4ced3d08a5c0f4edb105"
---

# How to Streamline Business Process Automation with Autopilot Workflow Service

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


- How to Streamline Business Process Automation with Autopilot Workflow Service


In P&C insurance, developers encounter significant hurdles when managing business processes the traditional way. Achieving complete automation in core processes, like claims handling, without human intervention is complex. Additionally, legacy system upgrades are complicated by the necessity for custom code, and changes in business logic require server restarts, leading to deployment delays and user frustration. Finally, the lack of collaboration between business and IT teams further complicates matters, often resulting in the need for rework.


In this article, we’ll introduce you to Autopilot Workflow Service, our new cutting-edge workflow management core service that uses developer-friendly tools to enable development teams, business analysts, and operational teams to streamline and fully automate business processes, bypassing the complexities of traditional coding.


## What is Autopilot Workflow Service?


Autopilot Workflow Service is a workflow management service on Guidewire Cloud Platform that externalizes business process logic within InsuranceSuite and powers the automation of P&C business processes. This core service provides the following:


- A way to orchestrate and integrate business process logic across InsuranceSuite, enterprise systems, and third-party services using Workflow Designer.
- Mechanisms to automate and streamline business processes while allowing human intervention when needed.
- A way for business processes to be defined in workflows external to InsuranceSuite.
- A way for you to respond to InsuranceSuite App Events.


Autopilot Workflow Service is comprised of:


- Workflow Designer: A user-friendly tool for orchestrating and modeling workflows.
- A runtime that manages the execution of workflow instances.
- Workflow Provisioner: A tool for onboarding the Autopilot Workflow Service for a planet.
- Workflow Console: A tool for managing runtime workflow instances.


## How Do Developers Use Autopilot Workflow Service?


Developers use Autopilot Workflow Service with ClaimCenter to fast-track development through user-friendly tooling and reusable artifacts, like templates, to enable rapid prototyping of automated workflows. They can also easily enhance, extend, or change core application logic without having to redeploy ClaimCenter. Finally, developers have experienced improved collaboration with business teams, reducing the likelihood of rework.


## Example: Personal Auto Glass Claim Flow


In[Guidewire Documentation](https://docs.guidewire.com/product-adoption-resources/latest/product-guidance/platform/autopilot-workflow-service/?_gl=1*ixrfou*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczNjI4Nzg0Ny4xNjEuMS4xNzM2Mjg4OTc3LjYwLjAuMA..) , we’ve provided a reference implementation that fully automates the OOTB Personal Auto line-of-business (LOB) based glass claim process: from opening a draft claim, to payment, and then to claim closure for eligible claims.


[Download the Reference Implementation](https://docs.guidewire.com/product-adoption-resources/latest/product-guidance/platform/autopilot-workflow-service/technical-solutions?_gl=1*1xwezb0*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczNjI4Nzg0Ny4xNjEuMS4xNzM2Mjg5MjgyLjYwLjAuMA..) (login required)


In the reference implementation, the workflows are implemented in a modular fashion for easy maintenance. One primary flow and seven subflows are created to achieve the desired automation. The debug timers are added to the workflows for debugging and tracing the workflow runtime execution; they can be removed before promoting to production.


Workflows provided in the reference implementation are shipped as a ZIP file, which can be imported directly into your Workflow designer. Once imported, the workflows can be configured to reflect the customer’s business needs.


## Autopilot Workflow Service Resources for Developers


[Autopilot Workflow Service Adoption Guide](https://docs.guidewire.com/product-adoption-resources/latest/product-guidance/platform/autopilot-workflow-service/adoption-guide?_gl=1*1qbey4k*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczNjI4Nzg0Ny4xNjEuMS4xNzM2Mjg5Mjk5LjQzLjAuMA..) (login required)
This guide is intended for implementation team business architects/analysts (BAs), developers, technical leads, quality analysts (QAs), and project managers interested in leveraging Autopilot Workflow Service to automate business processes.


[FAQs for Autopilot Workflow Service](https://docs.guidewire.com/product-adoption-resources/latest/product-guidance/platform/autopilot-workflow-service/frequently-asked-questions?_gl=1*1xx8rok*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczNjI4Nzg0Ny4xNjEuMS4xNzM2Mjg5MzU0LjYwLjAuMA..) (login required)
This document answers the frequently asked questions project teams may encounter when implementing Autopilot Workflow Service for a customer.


## Embracing the Future of P&C Automation Development


Autopilot Workflow Service represents a significant leap forward in P&C automation, offering a comprehensive solution for developers to streamline processes, quickly adapt to changes, and foster better collaboration across teams.


We encourage you to explore Autopilot Workflow Service further and identify opportunities within your organization to leverage its capabilities. By adopting a proactive approach to automation, you can drive efficiency, agility, and success in today’s competitive landscape.


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
