---
schema_version: "1.0.0"
document_id: "9ee4f40aba1e769bcfa8e8c059aaa4f61d4961e368dc7803bd6705512a0cd2be"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/seamlessly-integrate-external-testing-tools-with-quality-gates"
published_at: "2024-04-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:bb2ddb13cde4a64dab28a220357b3144085f35efdbf15c0ad6c1eaa2065b21e5"
---

# Seamlessly Integrate External Testing Tools with Quality Gates

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


- Seamlessly Integrate External Testing Tools with Quality Gates


Many of our customers have accumulated large existing testing investments on their own infrastructure. As they migrate to Guidewire Cloud Platform, they have asked us to help retain their investments and accelerate their cloud migrations momentum and adoption. We want to answers those needs by assisting customers in their transition to the cloud by giving them an ability to integrate their tests results with CI/CD pipeline for InsuranceSuite applications.


With the upcoming addition to the Guidewire Cloud Console tooling offering called Quality Gates, Guidewire provides a standardized flow to enable approved third-party testing frameworks and a way to propagate the execution results into Guidewire Cloud Platform for quality gating. Customers can add the execution results directly in the Guidewire CI/CD pipeline to enhance developer experience, provide better transparency, and reduce the time and resources needed to identify and fix issues.


## What are Quality Gates?


By quality gate, we mean a specific, pre-configured criterion that must be fulfilled before an artifact can pass a certain CI/CD step.


Currently, there are two stages that will be supported:


- Stage: What operation will it impact?
- Pre-merge: Merging pull request
- Pre-promotion: Promoting build to a higher planet class


Quality gates can be split into two different types – default and custom. Guidewire will provide default quality gates as part of the Guidewire Cloud Console CI/CD offering. Custom quality gates are dedicated to reflect third-party testing tooling owned by Guidewire customers outside Guidewire Cloud Platform.


The quality gates framework provides a diversified set of APIs that allow you to perform maintenance operations on quality gates, check their status, and send testing results.


## How do custom quality gates work?


Custom quality gates that enable you to bring your own testing tool into InsuranceSuite CI/CD pipeline are split into three parts:


- **Notifications** – an option to configure a callback to a given URL in response to a particular event in the Guidewire Cloud Platform CI/CD pipeline for InsuranceSuite applications.
- **Gating mechanism** – an option to prevent a commit from being merged or a build from being promoted to a higher-level environment based on configured criteria (for example, the outcome of external tests).
- **API to propagate test results** – a set of standardized APIs implemented in Guidewire Cloud Console microservices that enable users to propagate the pass/fail result for a specific quality gate.


## How can you integrate your own testing with Guidewire Cloud Console via the custom quality gates framework?


In order to integrate your testing with Guidewire Cloud Console using a custom quality gates framework, you will need to set up your quality gates first. It is a quick and simple process that can be done through the GCC UI or API. During quality gate configuration, you will need to provide information about your tests. This will give you better transparency and visibility into your testing process.


Test results can be programmatically provided to the Guidewire Cloud Console using APIs. You can send these results in the form of a “verification,” which is a logical object that contains details about your test run. Depending on the quality gate stage, results can be viewed in Bitbucket or the Build Promotion UI. Additionally, you can use APIs to programmatically check the status of quality gates.


In order to fully automate the process, you can leverage CI/CD events, a new addition to AppEvents Webhooks. These notifications can be used as triggers to initiate tests with third-party testing frameworks. Currently, the following notifications are available:


- PullRequestOpenedEvent,
- PullRequestUpdatedEvent,
- RepositoryChangedEvent,
- IsAppDeployedEvent.


By combining the three components, you can create a fully automated “bring your own testing” solution. This solution will automatically initiate your tests based on the GCC CI/CD pipeline, and send the results back to GCC, provides you with better transparency and visibility into your testing process, and reducing time and resources required to identify and resolve issues.


## What is the future?


The initiative is currently in the Early Access program. The scope of the program is limited to custom quality gates. We plan to release the feature globally with the addition of default quality gates in the summer of 2024, as part of the Kufri release.


Looking ahead, we have several exciting features in the pipeline. We aim to extend the scope of quality gates to additional applications (e.g. EnterpriseEngage). In addition, we plan to introduce additional notification types, auditing features and additional default quality gates including manual signoffs. The roadmap for the initiative is flexible and regularly updated based on customer feedback.


## Want to Learn More?


I hope you’ve found this sneak peek into the incoming Bring Your Own Testing feature helpful.


The feature is available in Early Access for Guidewire customers. To request more information about our Early Access program for Bring Your Own Testing, please contacteaprogram@guidewire.com .


If you want to learn more about the features and functionalities of GCC, you can also check out[Guidewire Cloud Console documentation](https://docs.guidewire.com/cloud/gcc-guide/insurer-developer/latest/?_gl=1*1odisfk*_gcl_au*MTUwMTA3Nzg0NC4xNzI1MDQ1MDUz*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczMjI5ODE5My43OC4xLjE3MzIyOTk0NjYuNTkuMC4w) (login required).


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
