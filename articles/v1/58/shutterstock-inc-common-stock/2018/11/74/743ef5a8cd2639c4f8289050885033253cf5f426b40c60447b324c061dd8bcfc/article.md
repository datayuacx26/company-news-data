---
schema_version: "1.0.0"
document_id: "743ef5a8cd2639c4f8289050885033253cf5f426b40c60447b324c061dd8bcfc"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2018/11/13/integration-testing-for-front-end-applications"
published_at: "2018-11-13T05:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:d6dc2045b05e35a2367c1d17041a15c98a4281b39964dfd7e4dbb5a04317880f"
---

# Integration Testing for Front-End Applications

## Introduction


At Shutterstock, we continuously work to add new features and improve functionality of our website for our users. In order to keep up with the rapid pace of development without compromising quality, we need to have a solid testing toolkit that allows developers to test the front-end application in isolation while gathering faster feedback on their code. In this post, we will talk about our integration testing toolkit and the benefits we have seen since implementing it.


## Why Do We Need Integration Tests?


The Shutterstock front-end application is powered by an array of microservices. If we were to rely solely on end-to-end tests for the front-end, then debugging test failures would be much harder and time-consuming due to the additional tracing that would be required. Additionally, end-to-end tests are long running—they require a given application and their underlying service dependencies to be deployed in test environments. As a result, the feedback loop on a developer’s code quality is necessarily delayed.


Integration tests allow developers to start their applications locally, mock-out all dependencies and focus specifically on the functionality they are developing/testing. The key benefits of integration tests, then, are:


- Reliability
- Allow isolated front-end, feature-focused testing
- Ability to simulatie upstream behavior and allow exploratory testing
- Provide early insight into application performance


## Integration Test Toolkit for Front-End Applications


Our integration test toolkit is comprised of[Nock](https://github.com/nock/nock) ,[Mocha](https://mochajs.org/) ,[Chai](https://www.chaijs.com/) and[Puppeteer](https://github.com/GoogleChrome/puppeteer) .


### Nock


Using Nock, we intercept and override HTTP calls made by our server to external services. This allows us to control the inputs to our tests, making them deterministic and reliable. It also enables developers to test their code in isolation. Additionally, because tests do not need to call any external services, they are more performant.


### Puppeteer


We use Puppeteer to test our client application. It is a feature-rich, well-documented library that lets us mimic user interactions on our website. In addition to its other capabilities, we use Puppeteer to intercept and override external calls made by the client. We also leverage the ability to capture artifacts such as console logs, request failure logs and screenshots to make debugging easier.


### Mocha & Chai


We opted to use Mocha as the test runner and Chai as the assertion library for our toolkit. Though either of these could be replaced with other available test runner/assertion libraries of your choice, we opted for Mocha as it is well-documented, intuitive, and easy to use.


## Integration Test Lifecycle


The diagram above shows the lifecycle of our integration tests using this toolkit.


Tests are executed using Mocha. Using Mocha’s before-hooks, the application is started locally and the test mode is determined. The test mode is defined by the environment variable. Depending of the mode of the test, the integration tests either record, play or simply execute.


Details about each of the modes are as below:


### ` RECORD`


In this mode, our tests make real calls to services while using Nock and use Puppeteer’s recording and intercepting feature to record these calls. Typically, when a test is first written, it is run in` RECORD` mode to capture all external request/responses. We post-process the recordings to eliminate any duplicates and add regex matches for ephemeral aspects of the recordings (such as timestamps) as a part of our test teardown.


### ` PLAY`


In this mode, the tests use the recordings generated from the` RECORD` mode. Again, using Nock and Puppeteer’s interception capabilities, tests detect if there is a matching recording for the outgoing call and if there is one, the outgoing call is overridden by the matching recording. We use this mode to run our tests in our CI/CD pipeline.


If a matching recording is not found for a given outgoing call, the test fails, indicating to the developer the need to re-` RECORD` the test.


### ` NO_MOCK_NO_PLAY`


This mode allows us to ensure that our recordings have not gone stale. When using the` NO_MOCK_NO_PLAY` mode, we do not record any outgoing calls, nor do we attempt to override them. We simply let the tests make real service calls. The test output is automatically monitored to ensure that there are no failures because of bad recordings. Tests in this mode mode are run on a regular cadence to ensure that the upstream services have not made breaking interface changes and that our tests are not operating under incorrect inputs.


## Wins


Since implementing this toolkit, we have seen the following positive outcomes


- Improved test runtime:


- The toolkit outperforms our previous Selenium based end-to-end test suite by 72%
- These integration tests run alongside unit tests in our CI/CD pipeline and provide feedback to developers with 8 minutes of PR creation


- Better coverage checks:


- By gathering data around JS and CSS code coverage, we can more easily measure our test completeness and effectiveness


- Early insights into performance:


- Since developers can now visualize the external calls in form of recordings, they can start implementing strategies to improve the performance of their applications


## Conclusion


Using the features of Nock, Mocha and Puppeteer, we have been able to implement a powerful integration test toolkit for our front end applications. These tests, in conjunction with some end-to-end tests, have allowed us to execute high-quality, high-coverage test strategies to ensure we provide the best possible experiences to our customers. For more on our journey of reaching and adopting this strategy, feel free to watch this video from our NYC NodeJS meetup:
