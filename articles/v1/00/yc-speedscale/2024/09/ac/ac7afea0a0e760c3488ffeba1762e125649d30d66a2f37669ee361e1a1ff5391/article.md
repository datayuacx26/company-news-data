---
schema_version: "1.0.0"
document_id: "ac7afea0a0e760c3488ffeba1762e125649d30d66a2f37669ee361e1a1ff5391"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/testing-with-production-traffic/"
published_at: "2024-09-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:a87af3cb94f537ab4311891afe6386a21a3964b5b144992876b653887834c181"
---

# Seasonal Holiday Readiness Testing with Production Traffic

Many businesses depend on seasonal products, offerings, and campaigns. Seasonal traffic usually refers to high traffic levels because of festival seasons like Thanksgiving and Christmas and sales events like Black Friday, Cyber Monday, and Boxing Day. These impact a wide range of industries, such as travel, hospitality, social activities, and, most prominently, e-commerce. If your business is primarily run online, you’ll need your applications to be elastic and flexible enough to handle this significant increase in traffic to cater to demand and also maintain a good user experience for your customers.


Taking a DIY approach to addressing the challenges associated with high production traffic bursts can be tricky because it involves load testing, stress testing, spike testing, and performance testing, among other things. The result of these testing exercises usually leads to optimizations in application infrastructure and code.[Speedscale](https://speedscale.com/) is a tool that can integrate with your applications and simulate realistic production workloads and patterns so that you can improve your application design and architecture and be better prepared for seasonal spikes in production traffic.


This article explores the effects of seasonal traffic and how to design your applications for handling it.


## What Is Seasonal Traffic?


E-commerce is one of the most prominent examples of industries that experience seasonal traffic spikes during events. To give you an idea of the scale, Amazon’s Prime Day in 2023 generated[15.35 trillion requests on EBS](https://aws.amazon.com/blogs/aws/prime-day-2023-powered-by-aws-all-the-numbers/) . While only a few companies in the world get to that scale, many companies handle hundreds of thousands of requirements in a second, which is not unheard of.


Seasonal traffic spikes occur around specific times of the year, such as regional travel seasons, festivals, and sporting events. Holiday seasons vary from one country to another, even within different regions of a country. Travel seasons are similar. There are also[significant sales events](https://blog.cloudflare.com/cyber-week-analyzing-internet-traffic-and-e-commerce-trends) , many of which align with holidays, such as Thanksgiving, Easter, Christmas, Diwali, and so on.


Several subpatterns emerge within these variations of seasonal traffic, such as gradual increases in traffic over some time, short-lived spikes in traffic, through-the-roof traffic spikes, and so on. These changes in production traffic patterns can significantly impact the application’s performance, resulting in slower load times, unexpected crashes, and transaction failures, all of which lead to bad user experience and lost revenue. This makes it extremely important for a business to predict and handle these changes in traffic efficiently.


## Preparing for Production Traffic Spikes with Speedscale


Speedscale offers a range of features that allow you not only to mock traffic spikes but also monitor the performance of your applications during those spikes to get a fuller view of what’s happening to your systems and pinpoint any performance-related issues. Speedscale’s three-phase process of[observing](https://docs.speedscale.com/#observe-) ,[replaying](https://docs.speedscale.com/#replay-) , and[analyzing](https://docs.speedscale.com/#analyze-) aligns with three key load testing features it provides: load generation, scenario scripting, and analysis and optimization.


You can get started with setting up Speedscale for your application using the[platform-agnostic tutorial](https://docs.speedscale.com/tutorial/) provided in the official documentation. If you want to follow along with this guide, you should also[create a Speedscale account](https://app.speedscale.com/signup) , as the examples use the console to create[test configs](https://docs.speedscale.com/concepts/test_config/) and[load patterns](https://docs.speedscale.com/reference/configuration/load-pattern/) , among other things.


To design an application that can handle different variations of loads in terms of variety, volume, intermittence, and rate of fluctuation, you’ll need to test your application with specifically designed loads. The three most common types of tests to prepare for these high-traffic workloads are the following:


- [Stress tests](https://queue-it.com/blog/load-vs-stress-testing/) ****:**** These involve continuously increasing the traffic to determine breaking points for your systems.
- [Breakpoint tests](https://grafana.com/blog/2024/01/30/breakpoint-testing/) ****:**** This kind of test sets an estimated breakpoint for your system and gradually increases the load to gauge how it handles traffic, identifying the point at which it breaks or experiences significant performance degradation.
- [Spike tests](https://k6.io/docs/test-types/spike-testing/) ****:**** These involve testing an application for sudden spikes and surges in traffic.


These and other tests, such as smoke tests and soak tests, help you understand how your systems perform under heavy traffic situations and how this impacts your application performance, availability, reliability, and user experience.


You’ll first need to create a test configuration in the console to get started with these tests. Test configs help you control how the traffic is replayed for your system. Creating test configs involves configuring[generators](https://docs.speedscale.com/reference/glossary/#generator) ,[responders](https://docs.speedscale.com/reference/glossary/#responder) ,[assertions](https://docs.speedscale.com/reference/glossary/#assertion) , and[goals](https://docs.speedscale.com/reference/configuration/goals/) , as shown in the image below:


Once you have the basic configuration in place, you can start designing tests for specific scenarios.


## Simulating Specific Scenarios Using Scenario Scripting


To ensure that your application can handle real-world conditions, you need to test it with data that closely resembles production data and ensure it interacts with your application in the same way. It’s essential to simulate not just data but also product scenarios and workflows that generate the data. This approach closely mimics your actual production workloads, providing confidence in your application’s ability to handle traffic spikes.


Speedscale allows you to[mock services](https://docs.speedscale.com/concepts/service_mocking/) and also use[transforms](https://docs.speedscale.com/concepts/transforms/) to modify the data before or during a replay so that the[service under test](https://docs.speedscale.com/reference/glossary/#sut) (SUT) has as realistic a load as possible. Doing this is essential to mimic the complexity of various scenarios, such as payment processing and inventory management.


### Payment Processing


Processing payments in an application usually involves several moving pieces, including backend services, a payment gateway, a reconciliation engine, and more. To run a realistic test on your system, you need realistic data. You can either write code that acts as a service that interacts with other mock services and generates mock data, or you can use third-party integrations to do that. That’s where Speedscale’s[service mocking](https://docs.speedscale.com/concepts/service_mocking/) comes in.


Speedscale enables you to configure realistic response times for external services and their response sequencing to mimic accurate production data and scenarios. You can also mock a third-party API authentication layer to get a fully realistic load to test.


As payment processing is a multicomponent, multistep process involving third parties, it’s expected to have non-idempotent responses to API requests. For instance, the status of a payment might change every time you hit the API. Speedscale can automatically mock such dependencies even without specific training. To see it in action, check out[this demo](https://play-dev.speedscale.com/analyze?service=payment&startTime=now-15minutes&endTime=now) that mimics payments data from HubSpot, Plaid, and DynamoDB, among other internal services.


### Heavy User-Login Periods


Industries like e-commerce and event ticketing often see prolonged user login periods when end users are deciding what to buy. This means that there is a high number of concurrent users at any given time. You could script this scenario in Speedscale using[load pattern stages](https://docs.speedscale.com/reference/configuration/load-pattern/) that allow you to define how you want to ramp up the traffic to test your system. You have the option to configure the number of requests over a period of time and can also define the number of virtual users ([vUsers](https://docs.speedscale.com/reference/glossary/#vuser) ). Combining this configuration with an authentication service or any other external dependencies will provide a complete workflow for testing.


Every logged-in user takes a chunk of memory and CPU in an application and the associated database. You may have inactive logged-in users and active users that have been logged in for a long time. Although it’s not a good idea to allow inactive users to occupy resources, you should also design your application to be flexible enough to accommodate them if that happens. You’ll need to design and architect your application to address these cases and manage your resources.


### Inventory Management


Inventory management gets complicated when traffic spikes. You need to ensure that your product stocks displayed on an e-commerce website, for example, are fully updated before someone places an order. Such an application would most likely involve a caching layer that maintains the inventory so that the application doesn’t have to go back to the database whenever an order is placed.


You can mimic the application and the caching layer during periods of high demand using Speedscale’s service mocking and load patterns in test configs. You can configure a high number of requests per second with a load that ramps up quickly to put the system under stress.


## Analyzing Results and Optimizing


To understand how your application performed in the tests, you can look at[reports](https://docs.speedscale.com/guides/reports/) and[snapshots](https://docs.speedscale.com/concepts/capture/#snapshots) . You can see the list of reports associated with different services and configs in the image below:


You can also access detailed reports covering performance metrics such as CPU and memory usage, success rates, and mock responses:


These reports also provide an endpoint-level latency summary with P50, P95, and P99 metrics, along with other metrics like requests per second, total requests, error count, and error percentage. You can use these metrics to guide various application optimization strategies to ensure quick scaling, better load times, and infrastructure optimizations. The following are some strategies you could use:


- ****Right-sizing resources:**** Before optimizing the application, you need to ensure that you have the right infrastructure in place to handle seasonal spikes. Bear in mind that high bursts of seasonal traffic also mean that there are times during the offseason when traffic is lower than usual.
- ****Code optimization:**** When you configure tests in Speedscale to run later, you can define load patterns, assertions, and goals. These make identifying issues and bottlenecks easier.
- ****Leveraging caching:**** If your application is experiencing higher latency than the acceptable service levels, you need to ensure that a significant number of requests are being served from the cache. Otherwise, increased database calls will increase latencies and degrade user experience. The Speedscale dashboard’s[CPU and latency metrics](https://docs.speedscale.com/guides/reports/performance-details/) help identify these issues.
- ****Utilizing content delivery networks (CDNs):**** If you’re seeing very high latencies on the Speedscale report, an inefficient or missing CDN is often the culprit. You can use this report to your advantage and identify the services behind the endpoints with high latency to ensure you have everything cached at the CDN close to the end user.
- ****Prerendering static HTML pages:**** Prerendering static HTML pages can be really beneficial when you are receiving bursts of requests. It reduces the load on the application and also improves the end-user experience significantly.


## Conclusion


In this article, you learned about seasonal production traffic, how it impacts a business, and how[Speedscale](https://speedscale.com/) can help you design and architect your application to handle the complexities that come with it. You also saw several use cases in various industries where testing for seasonal traffic is important for the business.


Companies like[Cimpress](https://speedscale.com/customers/cimpress/) and[Sephora](https://speedscale.com/customers/sephora/) have already used Speedscale’s load testing capabilities for better seasonal traffic handling.[Sign up for a free Speedscale account](https://app.speedscale.com/signup) to try it out!
