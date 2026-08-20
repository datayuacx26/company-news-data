---
schema_version: "1.0.0"
document_id: "ed763a23c959010606ee50bf54c81bf4570434b5fc26037c58174c39baedd0c4"
company_key: "yc-emi-labs"
company: "Emi Labs"
source_id: "yc-emi-labs-rss-87232385bc09"
canonical_url: "https://medium.com/@EmiLabsTech/a-tale-about-observability-in-emi-0f9126e4807c"
published_at: "2023-12-04T18:44:34+00:00"
first_seen_at: "2026-07-27T09:02:34.241036+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:25564c6dce2a715821e7936bd2f348f88260397dbeed00b5d1e5d050c53b5b67"
---

# A tale about observability in Emi

Opentelemetry


Otel


Observability


Sre


DevOps


# A tale about observability in Emi


[Emi Labs Tech - Ravens](https://medium.com/@EmiLabsTech?source=post_page---byline--0f9126e4807c---------------------------------------)


4 min read


·


Dec 4, 2023


--


by[Rodrigo Sevil](https://www.linkedin.com/in/rodrigo-sevil/)


*In the ever-evolving landscape of modern software development, the need for efficient tracing, logging, and performance measurement systems has become increasingly vital.*


In this blog post, we will explore the issue we faced, the choices we had and the valuable lessons we learned along the way of implementing OpenTelemetry (OTEL) at the core of our micro-services observability stack.


## What Challenge Did We Encounter?


- Initially, our in-house tracing, logging, and metrics system, which relied on AWS CloudWatch (CW) logs and metrics, as well as Sentry for error tracking, proved adequate for our needs.
- However, as our services grew in complexity, it became increasingly challenging to identify and track communication between them.
- Moreover, we lacked a comprehensive performance measurement framework that could capture and analyze data across various areas, including API endpoints, database queries, and other critical components.


To address these challenges and enhance our system’s efficiency and visibility, started looking for a solution that could seamlessly trace and log service communication while also providing comprehensive performance metrics across all relevant areas.


## Exploring Our Options


When it came to selecting a solution, we considered the following options:


- **Adopt an all in one solution such as NewRelic, Datadog, or similar tools** : These popular monitoring tools offer a range of features for tracing, logging, and performance monitoring. They provide a user-friendly interface and integrations with various technologies. However, they may come with vendor lock-in and require additional costs.
- **Adopt OpenTelemetry (OTEL) and choose an observability backend** :[OTEL is an open-source observability project](https://opentelemetry.io/docs/what-is-opentelemetry/) that aims to provide a unified approach for collecting, processing, and exporting telemetry data.


In the end, we chose the second option because it offered the benefits of a standardized approach, flexibility, and extensibility:


- [A wide range of language-specific SDKs](https://opentelemetry.io/docs/instrumentation/) , making it easy to integrate with various programming languages and frameworks. These SDKs provide developers with the necessary tools to instrument their code and capture relevant telemetry data, such as traces, metrics, and logs.
- [OTEL supports various data collection methods](https://opentelemetry.io/docs/collector/) , including agent-less ([no-collector](https://opentelemetry.io/docs/collector/deployment/no-collector/) ) and agent-based ([sidecar](https://opentelemetry.io/docs/collector/deployment/agent/) or[gateway](https://opentelemetry.io/docs/collector/deployment/gateway/) ) approaches. This flexibility allows organizations to choose the most suitable data collection method based on their specific requirements and constraints.
- [Another key aspect of OTEL is its extensibility](https://opentelemetry.io/ecosystem/) . The project provides a plugin architecture that enables users to extend its functionality and integrate with additional data sources, visualization tools, and storage systems. This extensibility empowers organizations to customize their observability pipelines and adapt them to their unique monitoring needs.


## Implementing OpenTelemetry (OTEL)


Here are the key steps we took to deliver OTEL:


- We initiated the process by creating thin wrappers of the software development kits (SDKs) provided by OTEL. These wrappers facilitated easy setup and instrumentation of our applications and internal libraries with minimum configuration.
- In addition to the SDK wrappers, we also developed a thin wrapper for the otel-collector image. This wrapper allowed us to centralize all traces/metrics/logs manipulation configurations of the collector without having to duplicate logic in the SDKs, while at the same time reducing the risk of breaking production grade services and providing easier rollouts.
- We began with the agent-collector pattern to make the most of our current infrastructure and utilize its resource detection capabilities efficiently.
- To bolster our confidence in deployments and ensure the reliability of these lightweight layers, we introduced unit tests to validate our custom instrumentations within the SDKs. Additionally, we implemented integration tests to verify the accuracy of our custom configurations within the collector.
- Initiate the rollout process initially within staging environments, subsequently deploying it across production environments in a phased manner, with coordination facilitated by the respective service owners.


By implementing these steps, we effectively integrated OpenTelemetry into our systems, enabling us to gather comprehensive observability data and gain valuable insights into the performance and behavior of our applications.


The following diagram provides a quick overview of the end result:


Press enter or click to view image in full size


## Valuable Lessons Learned


Our journey with OpenTelemetry (OTEL) was enlightening and came with several key takeaways:


- Using OTEL allowed us to evaluate X-Ray and New Relic as backends when we initially launched. Ultimately, we were able to effortlessly switch off X-Ray with just a single configuration tweak, all thanks to OpenTelemetry. This is a crucial capability for the SRE/DevOps team, that is providing tools that are as transparent as possible for developers while enabling to take action across dozens of services.
- The adoption of OpenTelemetry (OTEL) has empowered our organization to seamlessly integrate and implement Service Level Objectives (SLOs) and Service Level Indicators (SLIs). This has significantly contributed to our ability to proactively manage and optimize the overall health and efficiency of our applications, enabling us to uphold and continually improve upon our service level commitments.
- With OTEL’s versatile instrumentation, we have achieved a cohesive framework that facilitates seamless communication and interoperability among various components, providing a holistic view of our applications behavior. This standardized approach not only optimizes our operational workflows but also lays a solid foundation for scalability and future enhancements, ultimately contributing to the resilience and performance of our systems
- While OTEL is well-documented, it may have a steep learning curve that may not be suitable for all teams. Therefore, it is crucial to allocate sufficient time for learning and training to fully benefit from its capabilities.
- Even though OTEL provides built-in instrumentation, we discovered that additional configuration or adaptation of trace/metrics output was necessary to ensure proper visualization in the observability backend. This extra effort was essential for obtaining accurate and meaningful insights.


We appreciate you taking the time to read this blog post, and we hope it provides valuable insights for your own observability solution.


If you have any doubts or specific topics you’d like us to delve deeper into in our next post, please don’t hesitate to leave a comment below.


Your feedback and questions are valuable to us, and we’re eager to address them in our future content.
