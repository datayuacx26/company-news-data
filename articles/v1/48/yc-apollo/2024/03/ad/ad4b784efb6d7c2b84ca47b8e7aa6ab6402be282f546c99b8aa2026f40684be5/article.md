---
schema_version: "1.0.0"
document_id: "ad4b784efb6d7c2b84ca47b8e7aa6ab6402be282f546c99b8aa2026f40684be5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/efficient-data-collection-and-reporting-with-apollo-routers-telemetry-events-and-instruments"
published_at: "2024-03-07T09:45:31+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:5fa5ace0c1c7dda6ff5402c9da75a180e8e6258ae872e361f861b92b1352f62c"
---

# Efficient Data Collection and Reporting with Apollo Router’s Telemetry Events and Instruments

In today’s world of modern application development, monitoring and analyzing the performance of your services is crucial. Whether you have a single application or a platform that you’re trying to support, you are going to need to customize events and reporting to properly understand what is going on.


By choosing an architecture with a graph router exposing multiple GraphQL APIs, you can have a clear understanding of everything by instrumenting your graph router instance. The Apollo Router offers a powerful toolkit for collecting and reporting data from your application’s request lifecycle. In this tutorial, we’ll explore how to use telemetry events and instruments together to gain valuable insights into your application’s behavior.


## Prerequisites


Before we get started, make sure you have the following in place:


- A working knowledge of Apollo Router.
- Access to your application’s router.yaml configuration file.
- Basic understanding of telemetry concepts.


## Telemetry Events


Telemetry events are used to signal significant occurrences in the Apollo Router’s request lifecycle. These events are logged and traced, providing visibility into what happens during the processing of a request. You can configure both standard and custom events to capture specific moments of interest.


## Configuring Telemetry Events


To configure telemetry events in your router’s config file, locate or add the telemetry section, and under instrumentation, add an events section:


```text
telemetry:
instrumentation:
events:
router:
request: info
response: info
error: error
```


In this example, we’ve configured standard events (` request` ,` response` , and` error` ) with different logging levels. You can adjust these levels based on your monitoring requirements.


You can also define custom events tailored to your application’s specific needs. Here’s an example of how to configure a custom event:


```text
telemetry:
instrumentation:
events:
router:
custom.event:
message: "Custom event occurred"
level: info
on: request
attributes:
http.response.status_code: true
"my_attribute":
response_header: "x-my-header"
```


In this case, we’ve defined a custom event called` custom.event` that will be triggered on request. We’ve also specified attributes to include in the event, providing additional context for analysis. You can see a more details example of attributes in our[docs](https://www.apollographql.com/docs/router/configuration/telemetry/instrumentation/spans#span-configuration-example) and[view](https://opentelemetry.io/docs/specs/semconv/) OpenTelemetry conventions for attributes available.


## Telemetry Instruments


Telemetry events are great for reporting an event based on some conditions, but what about reporting metrics to a backend? Telemetry instruments allow you to collect data from every request to report measurements to where you are exporting metrics to. Apollo Router supports standard and custom instruments, giving you flexibility in monitoring various aspects of your application.


To configure telemetry instruments in your router’s config file, add an` instruments` section under` instrumentation` if it doesn’t already exist.


```text
telemetry:
instrumentation:
instruments:
router:
http.server.active_requests: true
http.server.request.body.size: true
http.server.request.duration: true
http.server.response.body.size: true
```


Here, we’ve configured standard instruments that track metrics such as active requests, request body size, request duration, and response body size. You can enable or disable these instruments as needed.


For more tailored monitoring, you can define[custom instruments](https://www.apollographql.com/docs/router/configuration/telemetry/instrumentation/instruments/#custom-instruments) . These instruments allow you to collect specific metrics relevant to your application. Here’s an example of configuring a custom instrument:


```text
telemetry:
instrumentation:
instruments:
router:
acme.request.duration:
value: duration
type: counter
unit: s
description: "Custom request duration metric"
attributes:
http.response.status_code: true
"my_attribute":
response_header: "x-my-header"
```


In this example, we’ve defined a custom instrument called` acme.request.duration` . It collects the duration of requests, specifying its type as a counter. We’ve also included attributes and a description for better context.


## Combining Events and Instruments


Now that we’ve configured telemetry events and instruments, let’s see how to use them together effectively.


- **Event Triggering** : Use telemetry events to signal significant moments in your request lifecycle. For example, trigger a custom event when a specific condition is met, such as a failed request with a status code of 500.
- **Instrument Data Collection** : Configure telemetry instruments to collect data related to the events you trigger. In our example, the custom event` custom.event` triggered on request can be associated with a custom instrument like` acme.request.duration` . This instrument collects and reports the request duration along with relevant attributes.
- **Analyze and Monitor** : With telemetry events and instruments working together, you can now monitor and analyze your application’s performance. Use your preferred metrics backend to visualize and gain insights into request durations, response sizes, error rates, and more.


## Conclusion


In this tutorial, we’ve explored how to leverage telemetry events and[instruments](https://www.apollographql.com/docs/router/configuration/telemetry/instrumentation/) in the Apollo Router to efficiently collect and report data from your application’s request lifecycle. By combining these powerful tools, you can gain valuable insights into your application’s behavior, troubleshoot issues, and optimize performance.


Monitoring your application is crucial for delivering a reliable and high-performing service to your users. With Apollo Router’s telemetry capabilities, you’re well-equipped to achieve just that.


Happy monitoring and optimizing your Apollo Router-powered platform!


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
