---
schema_version: "1.0.0"
document_id: "94cc1a53a41992673c344a0a6e0cc7dbcf3a62d8c2b222d70eb07f999eb18d47"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/test-automation-a-key-enabler-for-cloud-migration-part-ii"
published_at: "2024-03-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:26:13.235024+00:00"
content_hash: "sha256:4b3c6521697cb6dbcbc82d99429b79826ac59f67ed1d3a00f655620fae69caac"
---

# Test Automation – A Key Enabler for Cloud Migration, Part II

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


- Test Automation – A Key Enabler for Cloud Migration, Part II


*This is part two of two.*[Read part one here](https://www.guidewire.com/resources/blog/developers/test-automation-a-key-enabler-for-cloud-migration-part-i) *. In this article, Guidewire PartnerConnect Solution partner*[Karate Labs](https://www.karatelabs.io/) *(Karate) explains the power of test automation in enabling a smoother transition to cloud. A comprehensive test-driven approach, API testing for validation, and specialized tools can help achieve lasting business agility.*


In part one, we emphasized the crucial role of test automation in facilitating smooth cloud migrations. In part two, we delve deeper into the complexities of migration, focusing on API testing. Building upon the principles discussed earlier, let’s consider a practical example where we transition from SOAP to REST APIs, highlighting the intricacies involved and the importance of a robust test automation strategy to navigate these changes seamlessly.


## Example: Validating SOAP to REST API Migration


Consider an example where the architecture has been changed, enabling the use of REST and JSON instead of SOAP and XML. This requires the following activities:


1. Write tests for the existing SOAP APIs that validate XML payloads to establish a baseline
2. Write the test for the new REST APIs that validate JSON payloads against expected JSON examples to establish the test automation that will be used going forward, even post-migration
3. Add a step in the new test that calls the “old” SOAP API. It will be possible to reuse the script from the first step. We may also need to convert the JSON request data to the equivalent XML payload.
4. Add a step in the new test that converts the XML response from the “old” API to the same shape as the expected JSON response in the new API
5. Now do a JSON to JSON comparison. If this passes, this means that a use-case working in the “old” system also works accurately in the new system, and all required data and business logic are in place
6. After migration, remove the calls to the “old” system and remove the transformation steps


Depending on the risk and criticality of the test scenario, a decision can be taken on a case-to-case basis to only write test-automation for the new system (the second step) and skip the other steps.


For the above strategy to work, it requires a test automation tool that:


- Supports both JSON and XML
- Supports the conversion from JSON to XML and vice-versa
- Supports the comparison of two JSON or XML payloads (assertions)


## Post Migration


The investment in comprehensive test automation pays off well into the future. Regression tests act as a safety net for the team to release more often, with higher confidence. Choosing a test automation tool that is specialized for functional regression testing is key. For example – the quality of reports can provide insights into which areas of your application need more tests. Furthermore, the flexibility to run only specific tests when needed is important when tracking platform updates.


## Validating Updates with Focused Regression Testing


When it comes to updates to the cloud platform, simply running the test suite against the application deployed to the new (N+1) version can indicate update adoption readiness.


One of the advantages of the cloud is that provisioning a new instance on the new platform for the sake of testing is easy. Setting up and tearing down test environments can be automated as well.


## Running a Subset of Tests


The simplest way to structure a test suite where you have the flexibility of running a subset of tests on demand is to use tags. Good test automation tools support the concept of adding metadata to individual tests that identify which tags are associated. For example, you could mark a set of tests as being related to a specific business functionality such as “quote” or “First Notice Of Loss”.


“Invest in automated testing to support the migration, and then slightly tailor it for maximum reuse and streamline validation of continuous updates. Thereafter you can start to optimize the automated regression to focus on areas touched by the update” – Zachary Griesbach, Director of Product Management for Updates and Testing, Guidewire Software


When adopting an update it will be made clear which modular parts of the application are impacted. This means that you can focus on executing the subset of tests that are relevant to the changes and save time. For complex systems, running the entire set of regression tests is time-consuming, and being able to reduce the time needed for release validation is a competitive advantage.


In the future, we expect enhanced techniques to determine the subset of tests needed for validating a new release or platform update, such as AI-assisted code analysis and intelligent test suite selection.


## Summary


With a test-driven approach, you greatly reduce the risk and time of a business-critical migration to the cloud. API test automation is well-suited to address the complexities of a complex migration even when application logic and behavior changes are involved. The investment in test automation will continue to enable business agility long after the migration is achieved.


## Next Steps


Learn how a comprehensive test automation solution can address HTTP and REST API testing, UI automation, and inter-application integration testing where Webhooks and Kafka are involved. Solutions for exploring APIs can empower business analysts to validate core functionality and collaborate with developers without needing to write code. You can read more in this Karate Labs white paper,[Navigating the Brave New World of API Testing (PDF)](https://www.karatelabs.io/media/uzmkmpvo/navigating-the-brave-new-world-of-api-testing.pdf) .


## Learn More about the Guidewire Testing Framework


The Guidewire Testing Framework simplifies testing and enables behavior-driven development (BDD), helping teams complete updates and releases faster. To learn more, and to find Guidewire Documentation resources, visit our[Guidewire Testing Framework](https://www.guidewire.com/developers/developer-tools-and-guides/guidewire-testing-framework) page.


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
