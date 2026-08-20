---
schema_version: "1.0.0"
document_id: "d2ae0f84e2e3cd4096e7f27fdeb93290b366ef2ebb94d984257d3a04966e6ed7"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/niseko-release-whats-new-and-whats-next-for-guidewire-integration-gateway"
published_at: "2025-09-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:8279a1bc1267a6d5efd78e206017ba95ca4b334cadf78a3e275038f0d0b79cdc"
---

# Niseko Release: What's New and What's Next for Guidewire Integration Gateway

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


- Niseko Release: What's New and What's Next for Guidewire Integration Gateway


Developers working on Guidewire know that seamless and secure integration is the bedrock of every successful project, whether you’re connecting InsuranceSuite to external systems, modernizing legacy apps, or building new digital experiences. **Integration Gateway** is Guidewire’s cloud-native service for building secure, scalable connections across your ecosystem. With each release, Integration Gateway delivers new features to help you move faster, reduce manual work, and tackle real-world integration challenges.


The Niseko release continues this momentum, introducing tools and features designed to make your integration projects more efficient and future-ready. Here we will take a closer look at what’s new in the Niseko release and how these updates set the stage for the forward-looking roadmap of Integration Gateway.


**Want to see these powerful new tools in action?** Watch our recent webinar for a comprehensive walkthrough of the Niseko release and its benefits for your projects.


### Integration Gateway Product Highlights & Roadmap


## What is Guidewire Integration Gateway?


Integration Gateway is Guidewire's cloud-native integration service. It's a bridge that connects InsuranceSuite to a wide range of internal and third-party systems. With each major release, Integration Gateway evolves to empower insurers, partners, and developers to accelerate integration projects and improve operational efficiency.


## How the Niseko Release Improves Your Workflow


The Niseko release introduces several high-impact features that directly address developer pain points and enhance productivity on Integration Gateway.


1.


Integration Gateway **REST Client Generator**
A built-in tool that automates the generation of REST clients for outbound integrations, especially for InsuranceSuite Cloud APIs and any OpenAPI 3.0-compliant service.


You can now quickly generate, configure, and use REST clients directly within their Integration Gateway apps, reducing manual effort, errors, and time-to-market for new integrations.


2.


**Guidewire Property Service Embedded UI**
An embedded user interface within Integration Gateway Admin Service (IAS) UI that provides direct visibility and management of Helios property configurations for Integration Gateway apps.


You can now view configuration properties (such as secrets, endpoints, and environment variables) for their Integration Gateway apps without leaving Integration Gateway UI, improving transparency and operational control.


3.


Integration Gateway **App Test Coverage Reporting**
A new feature that enables customers to view test coverage reports for their Integration Gateway apps, specifically for Camel routes and integration logic.


You can now draw actionable insights about the quality and reliability of integration apps, helping your teams identify untested code paths and improve overall test coverage. Test coverage results are available in TeamCity.


4.


**Increased Payload Limit (50MB)**
The maximum allowed http payload size for inbound requests to Integration Gateway apps has been enhanced from 10MB to 50MB.


This addresses real-world integration scenarios where large files or messages must be processed, such as document management, Electronic Data Interchange (EDI), and London Market claims. The new limit supports more complex integrations and removes a key adoption barrier for customers.


5.


**Apache Camel 4.8.8 LTS**
Integration Gateway now runs on[Apache Camel 4.8.8 LTS](https://camel.apache.org/releases/release-4.8.8/) , the latest long-term support version.


This brings performance improvements, new features (including contract-first REST DSL), and enhanced security. Integration Gateway’s update strategy ensures customers benefit from the latest Camel features while minimizing upgrade effort.


6.


**Improved CORS Settings for** Integration Gateway **Apps**
Support enhancement for configuring Cross-Origin Resource Sharing (CORS) in Integration Gateway apps.


This simplifies secure connectivity between web applications and Integration Gateway apps across regions, addressing common security and usability requirements for modern digital experiences.


7.


**Marketplace Growth**
[Guidewire Marketplace](https://marketplace.guidewire.com/) now features over 118 integration apps.


You have access to a growing library of ready-to-use integrations, accelerating project delivery and reducing custom development.


## The Future of Integration Gateway Addresses 5 Key Needs


Guidewire’s investment in Integration Gateway is guided by five strategic themes, each addressing key customer and partner needs:


1.


### Automatic Updates & Updatability


-


**Automated update delivery:** Integration Gateway will further automate the update process, minimizing manual intervention and downtime. The Snowcat integration will enable automated build, test, and validation of customer Integration Gateway apps before each release, ensuring zero breaking changes and seamless upgrades.


-


**Regression testing:** Automated regression testing for each release/patch, with test coverage reporting and update previews for customers.


2.


### Improved Runtime Administration


-


**Failed Route Management:** Enhanced tools for managing failed routes, including UI templates for SQS/DLQ (dead-letter queue) message handling, failure resubmission, and centralized failure administration.


-


**Runtime observability:** Continued improvements in monitoring, logging, and metrics (e.g., Datadog integration, deployment history, and route-level visibility).


3.


### Self-Service


-


**Expanded self-service controls:** More self-service options for app creation, upgrades, scope setting (Keti integration), and administration security (Guard integration).


-


**Bulk operations:** Features like “Update All Apps” and bulk deprovisioning to streamline management at scale.


4.


### Cloud Apps (Packaging)


-


**Full Guidewire package integration:** Unified packaging and lifecycle management for Integration Gateway, InsuranceSuite, and other Guidewire solutions, including support for Lifecycle Manager (LCM).


-


**Templates and pre-packaged integrations:** Introduction of IIntegration Gateway Templates and Marketplace expansion to deliver more pre-built, reusable integration assets.


5.


### Developer Productivity


-


**AI-powered development:** Expansion of CodeLift for Integrations, including GenAI-based test generation, code recommendations, and update assistance.


-


**Low-code/visual tooling:** Visual route construction tools (e.g., Kaoto) for low-code integration development.


-


**Bring Your Own CI/CD and SCM:** Support for custom CI/CD pipelines and Source Code Management (SCM) via APIs/CLI, increasing flexibility for enterprise DevOps teams.


## What Else to Expect in the Integration Gateway Roadmap


-


**Inbound mTLS Support:** Enhanced security for inbound integrations.


-


**Support for InsuranceNow Integrations:** Extending Integration Gateway’s reach to InsuranceNow customers.


-


**Cloud Assurance Integration:** Gradle plugin for code quality inspections and SonarQube integration.


-


**Knative and Functions:** Exploration of event-driven and serverless integration patterns.


-


**Continued Alignment with Guidewire Standards:** Ongoing updates to ensure Integration Gateway remains compatible with GWCP UX, infrastructure, and security best practices.


## Release Compliance and Support Policy


Integration Gateway apps are supported for the current (N), previous (N-1), and second previous (N-2) releases. Visual indicators in Integration Gateway user interface alert users when an app is out of support or approaching end-of-support.


Integration Gateway and InsuranceSuite can be upgraded independently, providing flexibility for customers to adopt new features at their own pace.


**Ready to get started?** Visit the official[Integration Gateway Release Notes](https://docs.guidewire.com/is/integgatewayfw/niseko/Integration/integration-gateway-books/ig-framework/release-notes/topics/c_ig_rn_niseko.html) and[Developer Documentation](http://docs.guidewire.com/) for more details, or reach out to the Product Manager of Integration Gateway - Akshay Kumar atakskumar@guidewire.com with any questions.


Subscribe to our Developer Newsletter for release highlights delivered directly to your inbox!


[Subscribe Here](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
