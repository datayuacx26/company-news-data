---
schema_version: "1.0.0"
document_id: "3c2803700bbd0ee849e7925025e9ef3cdd94d4acc57095b5aee1101fb1e38713"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/introducing-fargate-playwright-browser-load-testing"
published_at: "2023-07-21T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:16ff3b4c92a09f1617d14a1573b79864b1acba23f8c174c63b7f1e51eb8a7988"
---

# Introducing full AWS Fargate + Playwright support

July 21st, 2023[Announcement](https://www.artillery.io/blog/tag/announcement)


# Introducing full AWS Fargate + Playwright support


Hassy Veldstra


Today we’re announcing two new features in Artillery.


1. **AWS Fargate support** : run distributed serverless load tests from 6 different AWS regions
2. **Playwright support on Fargate** : run high scale load tests using real headless browsers


## Run distributed tests on AWS Fargate


AWS Fargate is a serverless pay-as-you-go compute platform. Artillery can now make use of Fargate to run distributed load tests from 6 different geographical regions:


- ` us-east-1` (N. Virginia)
- ` us-west-1` (N. California)
- ` eu-west-1` (Ireland)
- ` eu-central-1` (Frankfurt)
- ` ap-south-1` (Mumbai)
- ` ap-northeast-1` (Tokyo)


Your tests will run from your team’s own AWS account and can scale out horizontally. Most existing Artillery test scripts just work with no modifications required, and performance metrics are collected and aggregated automatically.


Companies like[Auth0](https://auth0.com/blog/optimizing-cost-and-performance-of-auth0s-private-cloud/) ,[Amity](https://www.amity.co/) , and[Bedrock](https://bedrockstreaming.com/) run thousands of production-grade load tests with Artillery on Fargate as part of their delivery pipelines.


Unlike existing[AWS Lambda](https://www.artillery.io/blog/open-source-distributed-load-testing-with-lambda) support, load tests that run on AWS Fargate have no limitations on running time.


#### More information


- Our guide for[load testing on AWS Fargate](https://www.artillery.io/docs/load-testing-at-scale/aws-fargate)
- Docs for the new[run-fargate](https://www.artillery.io/docs/reference/cli/run-fargate) command
- Learn more about how[Auth0 use Artillery to optimize performance](https://auth0.com/blog/optimizing-cost-and-performance-of-auth0s-private-cloud/)


## Load test dynamic web apps with Playwright


Playwright is a modern browser automation framework created by Microsoft. We launched initial support for running Playwright scripts as load tests[back in late 2021](https://www.artillery.io/blog/load-testing-with-real-browsers) . Artillery can now run Playwright on Fargate to make it possible to **run Playwright load tests at scale** .


#### More information


- [Learn more about Playwright on playwright.dev](https://playwright.dev/)


### Why load test with real browsers?


Load testing dynamic web apps is difficult. The two common approaches are:


1. Emulating user activity with HTTP calls to backend APIs
2. Replaying pre-recorded requests with HAR files


Creating and updating test scripts with those approaches is time-consuming and brittle, and load tests that are run from those scripts usually fail to model realistic traffic patterns. Those approaches are also very backend-focused and do not provide good visibilty into user-perceived performance of the app under load.


With Playwright + AWS Fargate support in Artillery it’s now possible to load tests with real browsers in a way that’s fast, scalable, cost-efficient, and actionable.


#### More information


- See an[end-to-end example of using Playwright with Artillery on GitHub](https://github.com/artilleryio/artillery/tree/main/examples/browser-load-testing-playwright#load-testing-and-smoke-testing-with-real-browsers)


### Get insights into user-perceived performance


Artillery will track and report[Core Web Vitals](https://web.dev/vitals/#core-web-vitals) metrics for each load test. Core Web Vitals provide a way to measure and track user-centric experience of the web app under high load.


The following metrics are tracked for all pages in a test:


- FID - first input delay - a measurement of interactivity
- LCP - largest contentful paint - a measurement of loading performance
- CLS - cumulative layout shift - a measurement of visual stability


Additionally, TTFB (time to first byte) and FCP (first contentful paint) are also reported. Those metrics provide additional measurements of loading experience and can help debug issues with high LCP.


These metrics are reported as standard Artillery metrics, and can be automatically sent to an external monitoring system with Artillery’s publish-metrics plugin, and visualized in Artillery Cloud.


#### More information


- [Web Vitals](https://web.dev/vitals/) - learn more about Web Vitals
- [Publishing Artillery metrics to monitoring and observability systems](https://www.artillery.io/docs/observability)


### Visualize results with Artillery Cloud


Artillery CLI can be configured to send test reports to Artillery Cloud for visualization and analysis.


Aggregate Core Web Vitals values and scores are displayed for each page, as well as changes in those values throughout the load test.


[Sign up for early access to Artillery Cloud ›](https://app.artillery.io/login)


## Test at scale, cost-efficiently


Fargate support is available as part of the Artillery CLI at no charge. You only pay AWS for the resources used to run your load tests.


AWS Fargate is a pay-as-you-go serverless platform. The charges will vary depending on the duration and volume of your load tests, but are likely to be an order of magnitude less than the cost of using a hosted load testing platform, or building an in-house solution for distributed load testing.


#### More information


- See[AWS Fargate pricing](https://aws.amazon.com/fargate/pricing/)


## No DevOps needed


To run tests on AWS Fargate with Artillery, you will need an AWS account with configured AWS credentials. Artillery uses the official AWS SDK to create resources it needs to run your load tests on the fly. There is no long-lived infrastructure that needs to be set up or managed.


#### More information


- [Running Artillery on AWS Fargate](https://www.artillery.io/docs/load-testing-at-scale/aws-fargate)
