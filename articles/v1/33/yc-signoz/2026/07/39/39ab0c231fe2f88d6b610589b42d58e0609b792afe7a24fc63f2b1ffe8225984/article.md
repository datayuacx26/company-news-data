---
schema_version: "1.0.0"
document_id: "39ab0c231fe2f88d6b610589b42d58e0609b792afe7a24fc63f2b1ffe8225984"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/opentelemetry/python-overview"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:8be2beb429cd1e6ee8f2f6a872c776e9b90d7264627e5e9bc5d890e9346f9029"
---

# Overview - Implementing OpenTelemetry in Python applications [Tutorial Series]

# Overview - Implementing OpenTelemetry in Python applications \[Tutorial Series\]


Published on: June 20, 2024


Last Updated: July 14, 2026


3 min read


OpenTelemetry is an open-source observability framework that aims to standardize the generation, collection, and management of telemetry data(Logs, metrics, and traces). It is incubated under the Cloud Native Computing Foundation(Cloud Native Computing Foundation), the same foundation that incubated Kubernetes.


**


In this guide, you will learn how to implement OpenTelemetry in Python Applications. The following lessons cover everything you need to know about using OpenTelemetry to implement observability in Python applications.


## What you’ll learn


1.


[Setting up a basic Flask application](https://signoz.io/opentelemetry/setting-up-flask-application/)


In this tutorial, you will create a simple Flask to-do application with MongoDB.


2.


[Setting up SigNoz](https://signoz.io/opentelemetry/setting-up-signoz/)


OpenTelemetry does not provide a storage and analytics layer. In this tutorial, you will set up SigNoz to send your OpenTelemetry data.


3.


[Auto-instrumentation with OpenTelemetry](https://signoz.io/opentelemetry/python-auto-instrumentation/)


Set up automatic traces, metrics, and logs collection in the Flask application.


4.


[Manually configuring the agent for instrumentation with OpenTelemetry](https://signoz.io/opentelemetry/manually-configuring-opentelemetry-agent/)


Set up manual instrumentation with OpenTelemetry for more granular controls.


5.


[Create spans manually in your Python application](https://signoz.io/opentelemetry/manual-spans-in-python-application/)


Create manual spans and add metadata and attributes to them.


6.


[Create custom metrics with OpenTelemetry](https://signoz.io/opentelemetry/python-custom-metrics/)


Create custom metrics like counter, gauge, and histogram in your application.


7.


[Configure OpenTelemetry logging SDK in Python](https://signoz.io/opentelemetry/logging-in-python/)


Learn how to configure[OpenTelemetry logging](https://signoz.io/blog/opentelemetry-logs/) SDK in Python.


8.


[Customize metrics stream produced by OpenTelemetry SDK using views](https://signoz.io/opentelemetry/customize-metrics-streams-produced-by-opentelemetry-python-sdk/)


Learn how to configure the metrics stream produced by OpenTelemetry.


For a single-page reference instead of the chapter path, refer to the[OpenTelemetry Python guide](https://signoz.io/opentelemetry/python/) , which covers the setup end to end.


## Tutorial Scope


What you’ll do:


- Set up OpenTelemetry in a Python application
- Set up automatic and manual instrumentation
- Set up logging and custom metrics collection with OpenTelemetry
- Visualize all data collected with OpenTelemetry in[SigNoz](https://signoz.io/docs/introduction/)


This tutorial series is meant to introduce you to OpenTelemetry and provide practical examples of how to implement it in a Python application.


What we will not cover:


- In-depth basics of Node.js or Docker.
- General programming concepts.
- Non-OpenTelemetry related monitoring techniques.


## Pre-requisites


You are expected to have:


- Basic familiarity with Python/Flask.
- Basic knowledge of logging, tracing, and monitoring.


## Expected Outcomes


By the end of this series, you will be proficient in:


- Setting up and configuring OpenTelemetry for a Python application to collect traces, metrics, and logs
- Familiarize yourself with some advanced concepts in OpenTelemetry, like customizing the metrics stream
- Utilizing SigNoz to visualize OpenTelemetry data.
- Sample Repo for the project that we will build -[OpenTelemetry Python Example](https://github.com/SigNoz/opentelemetry-python-example)


## Getting Started


Let’s begin by setting up the sample Flask application that we will use to demonstrate the implementation of OpenTelemetry.


Read Next Article of OpenTelemetry Python series on[Setting up a basic Flask application](https://signoz.io/opentelemetry/setting-up-flask-application/)
