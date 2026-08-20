---
schema_version: "1.0.0"
document_id: "52a062fbb77e5861318ac63ff3cacd316775cf1845e55dea3c19a825fa146ac9"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-news-import-2dd1f24c69b5"
canonical_url: "https://embrace.io/blog/sending-metrics-with-embraces-data-destinations/"
published_at: "2024-12-18T01:35:43+00:00"
first_seen_at: "2026-07-21T17:58:27.596992+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:c195953228e335b2515592a5d0165acd5f354343f19496a0f9b1b178c79e5323"
---

# Sending metrics to third-party platforms with Embrace’s Data Destinations

Embrace's Data Destinations work by collecting data through the Embrace SDK, generating structured metrics, and transmitting them using OpenTelemetry. Learn how the Embrace engineering team built this system and how it addresses several key challenges in mobile metrics collection.


At Embrace, we understand that the complexity of modern application architectures can make it challenging for developers and operations teams to gain a comprehensive view of their systems. With diverse backend services, infrastructure, and mobile applications, having a unified observability strategy is vital for effective monitoring and troubleshooting.


To address this challenge, we have developed a solution that seamlessly transmits vital metrics—such as traces, logs, sessions, and network data—from our platform to popular destination platforms like Grafana Cloud, New Relic, and Datadog. By integrating Embrace’s mobile metrics with backend metrics, we empower our customers to visualize and analyze critical performance indicators from both backend services and mobile applications within a single, centralized interface. This streamlined approach not only simplifies the observability process but also enhances decision-making capabilities by providing real-time insights into the health and performance of their applications.


In this post, we’ll explore the details of our Data Destinations system, starting with an overview of how Embrace collects, ingests, and transmits data to destination platforms through protocols like OTLP.


**TL;DR** : Embrace’s Data Destinations make it easy to send mobile metrics—such as session counts, crash rates, and network performance—directly to destinations platforms like Grafana Cloud, Datadog, and New Relic. The process involves collecting data through the Embrace SDK, generating structured metrics, and transmitting them using OpenTelemetry. Key challenges addressed include managing varied authentication methods, ensuring the correct order of time series data, and handling delayed data from mobile devices. By integrating mobile and backend data in one place, Embrace simplifies monitoring and troubleshooting for developers and operations teams.


## Overview of Embrace Data Destinations


To understand how Embrace’s Data Destinations product works, we can broadly split it up into two key pieces. The first is the ingestion pipeline, which is the mechanism through which we actually collect and process the raw data from devices. The second is the metric generation and transmission process, which allows us to transform that data into metrics that can be received by different backend observability destinations – all of which typically have their own additional requirements and unique quirks, adding complexities to this process which we’ll discuss.


### Ingestion pipeline


High-level overview of Embrace's data ingestion pipeline


Our architecture starts with the Embrace SDK, installed on mobile devices, which collects raw data and sends it to our ingestion pipeline using OpenTelemetry. This data first lands in Kafka, and from there, a set of loaders—deployed as pods within our Kubernetes cluster—consume the data, process it, and store the refined information in our internal storage system.


### Metric generation and transmission


High-level overview of Embrace's metric transmission process


#### Metric generation process


The metric generation process is driven by a Kubernetes pod that runs periodically, leveraging the **Configuration API** to retrieve essential configuration data for each application. This API defines critical parameters, including:


- **Application name** : Identifies the application for which metrics are being generated.
- **Metrics to generate** : Specifies which metrics to track, such as Session Count and Failed Network Count and its relative filtering and grouping configurations.
- **Reporting interval** : Determines how frequently metrics should be processed—options include 5 minutes, 1 hour, or 1 day.
- **Target destination platform** : Destination platform where we will send the data.


Based on this configuration, the pod generates **Runs** , which are structured data blocks that encapsulate key details, including the application name, the specified metrics, and crucially, the start and end timestamps relevant to the metrics being generated.


Each run is represented as a JSON object before being sent to Kafka. Here’s an example of how a run might look:


```text
{
"application_id": "123FB",
"start_time": "2024-10-31T10:00:00Z", // interval is every 5 minutes
"end_time": "2024-10-31T10:05:00Z",
"metrics": [
{
"name": "Session Count",
"internal_metric": "session.count",
"filters": {
"key": "device_model",
"operator": "eq",
"value": "iPhone 14",
},
"group_by": ["app_version"]
}
],
"data_destination": {
"platform": "grafana_cloud",
"configuration": {
"instance_id": "1234"
}
}
}
```


Once created, each run is stored in MySQL and queued in Kafka. This queuing process preserves the order of data windows, ensuring that metrics are transmitted in sequence and preventing any duplicate entries. This architecture guarantees that the metric is accurately staged for calculation and timely transmission to the destination platforms.


#### Metric transmission process


A set of Kubernetes pods consume messages from Kafka in order and calculate the required metrics using our internal libraries and storage. Once calculated, the metrics are dispatched to the specified destination platforms via HTTP requests, using the OpenTelemetry library in Golang. Although the targeted platforms claim adherence to standards, we encounter variations in authentication methods, headers, and payload structures. In addition, managing responses can be challenging, as status codes often lack clarity, requiring tailored handling for each platform to ensure reliable data delivery.


## Supported connectors


Embrace customers can send metrics to a variety of destination platforms:


- **Chronosphere**
- **Datadog**
- **Elastic**
- **Grafana Cloud**
- **Honeycomb**
- **New Relic**
- **Splunk**


Each connector is designed to accommodate specific authentication methods, data formats, and API requirements, ensuring smooth and reliable metric transmission.


## Key challenges in sending data to destination platforms


At Embrace, we use **OpenTelemetry (OTel)** as a standardized framework for collecting and transmitting metrics and traces to destination platforms. OTel serves as a bridge between our applications and external tools, ensuring that the telemetry data we send is consistent, reliable, and easily interpretable.


While OTel provides a standardized framework, each destination platform has its unique characteristics, capabilities, and limitations when it comes to processing and interpreting this data. Understanding these differences is critical in effectively integrating these platforms. Below, we outline key distinctions among popular observability platforms:


### Authentication


One of the primary challenges we face in transmitting data to destination platforms is authentication. Each platform employs its own authentication mechanisms, which can vary significantly. This necessitates the implementation of diverse approaches for managing authentication tokens, API keys, and other credentials.


- **Varied authentication methods** : Platforms may require different types of authentication, such as Bearer tokens, Basic Auth, or OAuth. This diversity complicates the implementation, as we must create tailored solutions for each platform to ensure secure access.
- **Token management** : Handling the lifecycle of authentication tokens adds another layer of complexity. Tokens may expire and require refreshing, necessitating robust management processes to ensure uninterrupted data transmission.
- **Error handling** : Platforms differ in how they provide feedback for authentication failures, making it essential to implement custom error handling for each case. This includes interpreting status codes and error messages, which can differ in clarity and format.


To address these authentication challenges, we developed dedicated Go modules within our codebase that are responsible for connecting to, and managing authentication with each destination platform.


### Handling out of order time series


A time series metric is a set of data points collected at regular intervals, each associated with a timestamp. This allows for the analysis of trends and patterns over time, such as monitoring the number of active sessions. Sending time series data in the correct order is critical for maintaining data integrity and accuracy.


As an example, consider a scenario where we are monitoring the number of mobile app sessions that occur within each 5-minute window. The data points collected/sent at regular intervals might look like this:


- **2024-10-18 08:55:00 to 2024-10-18 09:00:00** : 150 sessions.
- **2024-10-18 09:00:00 to 2024-10-18 09:05:00** : 160 sessions.
- **2024-10-18 09:05:00 to 2024-10-18 09:10:00** : 170 sessions.


While generating and sending metrics may seem straightforward, it becomes significantly more complex when dealing with hundreds of metrics across thousands of applications, all reporting at different intervals to multiple destination platforms.


One key challenge is ensuring the correct order of metrics. This requires not only generating the runs in sequence, but also ensuring that these runs are consumed and transmitted in the same order. In an ideal world without delays, errors, or retries, this would be relatively simple. However, in practice, we must be meticulous in confirming that each data point is sent only after the previous one has been successfully transmitted.


Another challenge is that each destination platform has its own methods for managing uniqueness and order. For instance, in Grafana Cloud, all time series metrics belonging to a tenant (in our case, an application) must be sent in order. This means that if we have the Sessions Count metric and the Network Count metric, the data points belonging to those metrics must be in order. Although it allows for some flexibility by permitting data to be sent out of order within a two-hour window, it is critical to maintain sequence. In contrast, Datadog treats each metric as an isolated time series, which makes less strict than Grafana Cloud because you can send the entire time series for one metric in order and then another metric’s time series independently in order.


As previously mentioned, our solution was building dedicated Go modules for each destination platform to effectively manage these distinctions and ensure compliance with their unique requirements.


### Mobile delayed data


[Mobile delayed data](https://www.cncf.io/blog/2024/03/25/why-you-may-be-dropping-key-mobile-data-from-your-observability-solution/) refers to the time gap between when an event occurs and when the mobile device is able to send that information for processing through the ingestion pipeline.


This delay in data transmission can arise from several factors, including network latency, which is the time it takes for data to travel between the mobile device and the server. Additionally, if the device doesn’t have an internet connection, the information can’t be sent until connectivity is restored. Another cause can be app crashes, where data is not transmitted until the app is relaunched. Each of these issues contributes to the overall delay in processing and reflecting real-time events.


In the graph below, you can see how much visibility you would lose from discarding delayed data.


This data is pulled from a customer that has above average data delays. Note how 25% of data does not arrive for at least 2 days after it’s collected on the mobile device, and 100% of data does not arrive for approximately 1 week. Every customer has its own unique pattern with respect to delayed data.


To generate metrics with the most complete information, we can collect data at different intervals:


- **5-minute metrics** : These are designed to provide nearly real-time data. However, because they focus on short and very recent time frames, they do not incorporate delayed data from previously recorded events. This ensures that the metrics reflect the most current state of the application, but it means that any events sent after this window closes will not be included.
- **1-hour metrics** : These metrics fall in the middle ground, offering a balance between timeliness and data comprehensiveness. While they do not provide real-time insights, they capture delayed data, allowing for a more complete view of application performance over a slightly longer period.
- **Daily metrics** : In contrast, daily metrics do not offer real-time insights but can capture delayed data. By aggregating data over a longer period, these metrics ensure that most of the relevant events are accounted for, even if they are sent later. This approach helps provide a more comprehensive view of application performance, albeit with less immediacy.


By generating metrics in this way, we aim to balance the need for timely insights with the necessity of including all relevant data, ensuring that our users receive accurate and meaningful metrics for their observability needs.


## What does this look like in practice?


Using Grafana Cloud as an example, you can now visualize key performance metrics over specific time periods. For instance, you can monitor **session counts** to understand how user engagement levels fluctuate over time.


Similarly, the **crash counts** graph provides insights into application stability, allowing you to track the frequency of crashes over the same timeframe. These visualizations enable you to quickly identify patterns, anomalies, and areas for improvement, facilitating data-driven decision-making to enhance user experience and app performance.


Grafana Cloud visualization showing sessions count calculated every 5 minutes


Grafana Cloud visualization showing crashes count calculated every one hour


Grafana Cloud visualization showing sessions count calculated every one hour


Grafana Cloud visualization showing crash count calculated every five minutes


Additionally, you can create a comprehensive dashboard that integrates Embrace metrics alongside metrics from your backend or infrastructure. This holistic view allows you gain valuable insights into user experience, app stability, and infrastructure health, all within a single observability platform.


Finally, you can leverage the various tools provided by Grafana Cloud to maximize the value of your metrics. For instance, you can set up alerts based on predefined thresholds for key metrics such as session counts or crash rates.


## Modernize your mobile observability with Embrace


Engineering teams want end-to-end visibility across their apps and systems so they can collaborate more effectively. In this post, we’ve covered how Embrace’s Data Destinations is a powerful way to send mobile metrics to your existing observability platform. That way, you can connect technical failures in your services and infrastructure directly to the user and business impact.


To learn more about Data Destinations, including how you can easily create and send custom metrics to your existing observability platforms, check out[our documentation](https://embrace.io/docs/data-destinations/) .


Send mobile metrics to your system today


Integrating Embrace’s mobile metrics with backend metrics allows you to visualize and analyze critical performance indicators from both backend services and mobile applications within a single, centralized interface.


[Read the docs](https://embrace.io/docs/data-destinations/)


Author


[Santiago Leira](https://embrace.io/author/santiago-leira/) Santiago Leira is a backend engineer at Embrace. He has nine years of total engineering experience, with seven years as a backend engineer and two years as a full-stack engineer. Prior to Embrace, Santiago worked at Wildlife Studios. He has been with Embrace since 2022, providing the team with ample expertise in mobile games.


Related Content


metrics


1 July 2025


• 2 min read


### [Your Apdex score is great, so why are users complaining?](https://embrace.io/blog/your-apdex-score-is-great-so-why-are-users-complaining/)


Embrace builds metrics out of the individual technical operations that make up user activity, so you can solve real issues that impact the people in front of the screens.


embrace metrics api


15 August 2024


• 3 min read


### [See the big picture with Embrace Custom Metrics](https://embrace.io/blog/see-the-big-picture-with-embrace-custom-metrics/)


Embrace captures all the data from all the sessions of an app. Your Error Logs, successful and failing Network Requests, and screen tap locations are all recorded by the mobile app client. That’s a lot of data. Too much data to be constantly monitoring.


integrations


24 August 2023


• 3 min read


### [Custom Metrics Forwarding gives you ultimate freedom and flexibility](https://embrace.io/blog/custom-metrics-forwarding/)


Embrace's suite of Data Forwarding tools now supports forwarding your own Custom Metrics, so you can measure exactly what matters.
