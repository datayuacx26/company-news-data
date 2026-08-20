---
schema_version: "1.0.0"
document_id: "38470abe7cdf9a4ad495d27b824fd8f97712bb72660e6744dcb400a33c22fef3"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/test-automation-a-key-enabler-for-cloud-migration-part-i"
published_at: "2024-02-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:9766e353082bfb041805b3f2354a0e02c9816c4c6f8fd20e9c409557b9409759"
---

# Test Automation – A Key Enabler for Cloud Migration, Part I

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


- Test Automation – A Key Enabler for Cloud Migration, Part I


*In this post, Guidewire PartnerConnect Solution partner*[Karate Labs](https://www.karatelabs.io/) *(Karate) explains the power of test automation in enabling a smoother transition to cloud. A comprehensive test-driven approach, API testing for validation, and specialized tools can help achieve lasting business agility.*


Before migrating to the cloud, a lack of automated tests in your development introduces risk and creates additional churn while attempting to iterate and stabilize the cutover. Yet there is an approach companies can take to reduce risk and realize business benefits – a test-driven approach. When implemented strategically, a test-driven approach can set your business up to release faster, more often, and with higher confidence.


Many large enterprises face challenges in implementing test automation for their key IT initiatives, due to several factors:


- New functionality is considered a higher priority
- Business stakeholders are unconvinced on the need
- Limited understanding of the benefits
- Manual testing teams have resistance to skill updates
- Hard to maintain or flaky tests
- Programming language barriers make test automation difficult
- Overfocus on the UI testing, neglecting APIs
- Absence of a cohesive test automation strategy


A prerequisite for a smooth and successful migration to the cloud is test automation. With automated tests in place, teams realize multiple benefits when tests adequately cover the key business functionalities of software delivered. The most important benefit is the ability to ship value faster, more frequently, and with higher confidence that the software meets expectations. Automated tests act as a safety net so that the team can add more features and innovate freely.


In short, test automation gives you a significant competitive advantage.


“As a way to prepare for the cloud, we encourage customers to invest in automating tests and to match our selected cloud testing technology stack, such as Karate for API testing”
– Zachary Griesbach, Director of Product Management for Updates and Testing, Guidewire Software


After migrating to the cloud, you need to keep up with the increased pace of platform updates. Business stakeholders will have higher expectations that the cloud enables the team to deliver more and release faster. The only way to achieve this is by having test automation in place.


## Validating of Migration Patterns


### Re-deploy As-Is


Migration is ideally as simple as re-deploying the application on the cloud without changes. This is only possible if the exact runtime capabilities available on-prem are available on the cloud with no changes in architecture or behavior. To validate this pattern:


1. The team can focus on just the infrastructure aspects of the migration instead of worrying about functionality changes.
2. The testing strategy becomes greatly simplified. The same test suite that was built for the application on-premise can be run against the cloud deployment. If the tests pass, the system is ready to go live on the cloud.


In reality, things are way more complicated and nuanced. A migration to the cloud involves moving to a newer version of the platform, which means changes in the runtime infrastructure, architecture, and functionality. This most common case is referred to as “Lift and Shift”, and requires a modified validation approach.


## Lift and Shift


The typical migration to the cloud involves changes at the following levels:


- Architecture
- Behavior
- Data


**Architecture changes** include components of the runtime stack and technology choices. For example, the UI rendering technology can be different which means existing UI automation tests will not work and have to be rewritten or heavily refactored. Another challenge from a test automation perspective is when the API layer is upgraded from e.g. SOAP to REST. A move to the cloud will typically involve the adoption of more cloud-native technologies, for example, managed database technology such as AWS DynamoDB. Messaging and integration protocols also can change.


**Behavior changes** , while rare, can happen during a platform upgrade. These changes increase business risk when validating a successful migration because you need to identify if a behavior change is a bug or an intended feature.


**Data changes** , including the way data is persisted and the shape of the data, increase as architecture and behavior change and add an extra dimension of complexity to a migration. Ideally, a migration would be as simple as exporting data out of the source and importing it into the target. Data changes present additional challenges to navigate, such as, should all data be migrated. Does the data need to be transformed? How fast can a migration be performed? Should the migration be a “big bang” or done in phases?


The additional complexity and nuance of a “Lift and Shift” means it is essential to have test automation established for the existing system as a baseline. With this baseline in place, the team has a reference point for how the system should behave in the cloud.


The cloud deployment will be a combination of components, where some have no changes, and where others have minor or major changes. This is summarized in Figure 1.


## Automated Validation


Most of the effort in a cloud migration goes into the second row of Figure 1. The challenge is to validate that the new system is functionally equivalent to the old system and to be able to automate this validation. We also must validate that all the data needed is present in the new system.


When the old system behaves differently, a transform must be applied to the legacy response to convert the shape to align with the new system. After the conversion, an equality match will suffice to validate that the new system is behaving as expected and enables a quick understanding when it does not match.


The validation approach depicted in Figure 2 can be summarized as (1) call the old system, (2) call the new system, and (3) compare the results.


This approach is especially well-suited for API testing of core business flows and logic:


- APIs surface most, if not all, of the core business logic, more than what is accessible via a UI
- APIs are well-suited to do data transformations and comparisons
- Transformation between SOAP/XML to REST/JSON is easy with modern tooling
- APIs are designed for speed of data exchange
- API test suites can be run many times faster and give quick feedback during the iterative phases of the migration
- After migration, most of the test scripts can be reused with minor changes


Figure 3 depicts the steady state, where the steps of calling the old system and performing a data transform have to be removed. The tests are now standard functional and regression tests where API responses are compared with an expected baseline.


Test automation emerges as a pivotal strategy for facilitating cloud migrations, mitigating risks, and enhancing business agility. In part two, we’ll focus on an example of validating SOAP to REST API migration, discuss regression testing post-migration, and best practices for running a subset of tests.


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
