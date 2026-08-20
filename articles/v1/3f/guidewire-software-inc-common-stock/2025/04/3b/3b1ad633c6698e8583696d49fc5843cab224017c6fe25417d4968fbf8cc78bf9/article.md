---
schema_version: "1.0.0"
document_id: "3b1ad633c6698e8583696d49fc5843cab224017c6fe25417d4968fbf8cc78bf9"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/get-to-know-bring-your-own-testing-and-quality-gates"
published_at: "2025-04-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:33775500b0087c5a1d6ee364a9244f2a8b915d8b5e700fa5a9e9e439c56a19b9"
---

# Get to Know: Bring-Your-Own Testing and Quality Gates

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


- Get to Know: Bring-Your-Own Testing and Quality Gates


In this edited conversation from our Get to Know the Platform Better webinar series, Brady Haggstrom, Product Marketer at Guidewire, interviews Krzysztof Szczurek, Product Manager for CI/CD Experience, about Guidewire's new Bring-Your-Own Testing and Quality Gates features.


### Get to Know the Platform Better: “Bring-Your-Own” Testing and Quality Gates


---


**Brady** : Welcome to our "Get to Know the Platform Better" on-demand webinar series. Today, we're focusing on Bring-Your-Own Testing and Quality Gates. We'll provide background, articulate the challenges for customers and feature benefits, share future investments Guidewire is making, and wrap up with a demo.


I'm Brady Haggstrom, a Product Marketer at Guidewire, and I'll be moderating today's webinar. I'm excited to have Krzysztof join me as our expert. Krzysztof, would you like to introduce yourself?


**Krzysztof** : My name is Krzysztof Szczurek. I'm the Product Manager for CI/CD Experience for insurance applications on Guidewire platform. I'm happy to be here and shed more light on Bring-Your-Own Testing and Quality Gates. Thank you for inviting me, Brady.


**Brady** : Let's kick it off. Krzysztof, can you tell us about the background and context?


**Krzysztof** : Sure. Guidewire has CI/CD running on Guidewire Cloud Platform where we already support you with different tools for code, build, and deployment. We've built capabilities within Guidewire Home, a set of self-service tooling with an abstraction layer over Bitbucket configuration settings and TeamCity configuration settings. We also have Guidewire Testing Framework that supports you in writing your own tests.


So we're in a pretty good place - the full end-to-end DevOps flow is supported from the first commit up to deployment in production. Now, we've listened to your feedback and decided we want to be more flexible. We want you to bring your own tools that can support your unique use cases in different areas. One of those areas is testing, which is why we decided to start working on Bring-Your-Own Testing and Quality Gates.


**Brady** : Would you be able to drill into that now and tell us specifically what problem we're aiming to solve for customers?


**Krzysztof** : Yes. It's a common challenge for both existing and new Guidewire Cloud Platform customers, along with the SI partners supporting them.


Let’s start with customers migrating to the cloud or still considering it. These customers typically accumulate a lot of resources during their on-premises time. One of those resources usually relates to testing—different testing pipelines they've invested in over the years.


It's possible today to migrate all these resources to Guidewire Cloud Platform, but we know it's a challenging process. We want to make it easier for those customers migrating and also for customers already with us on the platform.


Each company is different, and sometimes there are quality requirements very specific to your business or your current situation. We know sometimes you want to ensure quality in your own way, and there are many partners on the market who can help by creating testing pipelines specific to you in your own tools. We want to make it easier for you to integrate all testing resources with Guidewire Cloud Platform CI/CD pipelines.


**Brady** : That makes sense. Now, let's introduce Guidewire's approach to solving this.


**Krzysztof** : When we started designing the “Bring-Your-Own Testing” framework, we had some core principles in mind. We wanted to make it automated, efficient, and scalable - basically a simple framework for integrating external tools with our platform.


That's why we created the framework based on three different pillars:


1. Notifications


: We started with notifications, deciding to send you a set of notifications you can use as triggers for your external test pipelines.
2. Integrated Pipelines


: Once tests are performed, we wanted to give you a way to propagate the results back into our CI/CD pipelines—that's the second pillar, integrated pipelines.
3. Quality Gates


: Finally, we want the test results to be useful in our CI/CD workflow, so we created a set of quality gates—pre-configured criteria based on your test results that help you make informed decisions about whether to progress an artifact in the pipeline.


**Brady** : Great, Krzysztof. How does it work?


**Krzysztof** : Let me explain with an interaction diagram—visuals are the best way to understand different concepts.


**Fig 1** : Guidewire supports 2 types of the quality gates: pre-merge and pre-promote.


As I mentioned, everything starts with notifications. We support different kinds: notifications related to Bitbucket operations, InsuranceSuite deployments, and others related to build lifecycle and promotion. These notifications serve as triggers to automatically start your testing pipeline.


When you're running your tests, it's a black box from Guidewire's perspective. You can use whatever tool you want as long as the tests you're running fall within one of the six types supported in the Quality Gates framework.


After running your tests, you can propagate the results back to Guidewire Cloud Platform via a set of dedicated APIs we manage for you. Then we can show information about the test results as an additional build in Bitbucket or as additional information in the build promotion UI.


We support two different levels: pre-merge for merge operations and pre-promote for promotion operations in the Build Promotion UI in Guidewire Cloud Console.


The last step is setting up quality gates so that if tests fail, we can block merging or promotion for you


I should mention what we're not doing


with this solution: We're not providing any cloud infrastructure to run third-party tests, and we're not setting up any direct connection to TeamCity. Both decisions were made so we wouldn't limit you. Without these limitations, you need to set up your own cloud infrastructure, but you can do it in your own unique way that meets your specific use cases.


...


**Brady** : Now, Krzysztof, can you show us how Bring-Your-Own Testing and Quality Gates works?


**Krzysztof** : Sure. You can check out the demo, or let me give you an overview of how it works in practice.


**Fig 2** : Check out the full demo via thewebinar link above.


In the demo, I showed how to create a new quality gate through the Guidewire Home interface. You start by naming your quality gate, selecting the application it applies to, and choosing which stage it should be active for—either before merging or before promoting to pre-production or production environments.


I demonstrated setting up quality gates for both scenarios: a "Quality Gates Demo" for the pre-merge stage in BillingCenter, and a "Performance Test" quality gate for the pre-promotion stage in Contact Manager.


Next, I showed how to set up webhooks in the AppEvents Webhooks application, where you configure endpoints and subscriptions to receive notifications when relevant events occur in your development pipeline.


To demonstrate the pre-merge quality gate in action, I created a pull request in Bitbucket. The system prevented me from merging because the quality gate requirements weren't met, showing a message that "a successful build of Quality Gates Demo for the latest commit is required before this pull request can be merged."


For the pre-promotion quality gate, I showed the build promotion view where you can see the status of your quality gates. I demonstrated how to use the CI/CD Manager API to send test verification results back to Guidewire Cloud Platform. Once I sent the passing verification, the quality gate status updated from "pending" to "passing," allowing the build to be promoted to the next environment.


This entire flow demonstrates how you can integrate your external testing tools with Guidewire Cloud Platform, using webhooks to trigger your tests and APIs to report results back, with quality gates ensuring that only builds that meet your quality criteria progress through your pipeline.


...


**Brady** : That’s great, Krzysztof. Can you tell us which customers this is available for?


**Krzysztof** : I'm very happy to say it's available for every InsuranceSuite customer on Guidewire Cloud Platform. It's not available for on-premises customers, but as long as you migrate to Guidewire Cloud Platform, it's already available since Kufri—one of our releases from last calendar year.


**Brady** : Krzysztof, to conclude, if you were a customer and you wanted to get started right away, what resources would you point them to?


**Krzysztof** : As I showed in the demo, there are a lot of great docs around this topic. I encourage you to try it out and actually try the feature. It's already there—you can use the APIs and set up your own quality gates.


**Read Guidewire Documentation** :


- [Bring-Your-Own Testing](https://docs.guidewire.com/cloud/gcc-guide/insurer-developer/latest/qg-byo-testing/?utm_source=Guidewire&utm_medium=Developer-Blog&utm_campaign=platform-webinar-series-bring-your-own-testing-and-quality-gates)
- [Quality Gates](https://docs.guidewire.com/cloud/gcc-guide/insurer-developer/latest/quality-gates/?utm_source=Guidewire&utm_medium=Developer-Blog&utm_campaign=platform-webinar-series-bring-your-own-testing-and-quality-gates)


Go ahead, try it out, and if you need any kind of support, please don't hesitate to contact your Guidewire Customer Success Manager or Alliance Manager.


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
