---
schema_version: "1.0.0"
document_id: "433f68b1eb1c03cb5deb7b133f9e8097de6743c089c8151e12d05471d0282266"
company_key: "yc-signoz"
company: "SigNoz"
source_id: "yc-signoz-rss-564a62b873f8"
canonical_url: "https://signoz.io/guides/understanding-prometheus-rate-vs-increase-functions-correctly"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.602972+00:00"
fetched_at: "2026-07-28T20:48:25.229040+00:00"
content_hash: "sha256:b058a82a761eda6a04fd4e50363b0e02b76919a7194a5a4a3cec0228563ceb11"
---

# What is the difference between Prometheus Rate vs Increase Functions

# What is the difference between Prometheus Rate vs Increase Functions


Published on: July 25, 2024


Last Updated: June 23, 2026


5 min read


Prometheus, a powerful open-source monitoring system, offers two essential functions for analyzing counter metrics: rate() and increase(). Understanding these functions correctly is crucial for effective data analysis and monitoring. This article delves into the key differences between rate() and increase(), their use cases, and best practices for implementation.


## Understanding Prometheus Rate and Increase Functions


Prometheus serves as a robust monitoring and alerting toolkit, collecting and storing time series data as metrics. Counter metrics, which only increase over time, form a significant part of Prometheus' data collection. The rate() and increase() functions help you analyze these counter metrics effectively.


Rate() calculates the per-second average rate of increase for a counter over a specified time range. It's particularly useful for determining how quickly a value is changing. Increase(), on the other hand, computes the total increase in a counter's value over a given time period. Both functions play vital roles in data analysis, offering different perspectives on metric behavior.


## How the Rate() Function Works


The rate() function calculates the per-second average rate of increase for a counter metric. It uses the following formula:


```text
rate  (  counter [  time_range ]  )    =    (  last_value  -   first_value )    /    (  last_timestamp  -   first_timestamp )


```


This calculation provides a smoothed-out rate of change, which is especially useful for volatile metrics. The syntax for using rate() is straightforward:


```text
rate  (  metric_name [  time_range ]  )


```


Rate() is ideal for analyzing metrics that change frequently, such as requests per second or CPU usage. However, it's important to note that rate() assumes the counter increases linearly between samples — which may not always be the case in real-world scenarios.


### Real-World Examples of Rate() Usage


1.


**Calculating request rates in web servers** :


```text
rate  (  http_requests_total [  5m ]  )


```


This query shows the average number of HTTP requests per second over the last 5 minutes.


2.


**Monitoring network traffic** :


```text
rate  (  network_transmit_bytes_total [  1h ]  )


```


This displays the average network transmission rate in bytes per second over the last hour.


3.


**Analyzing CPU usage trends** :


```text
rate  (  node_cpu_seconds_total {  mode =  "user"  }  [  10m ]  )


```


This query reveals the average CPU usage in user mode over the last 10 minutes.


## Deep Dive into the Increase() Function


The increase() function calculates the total increase in a counter's value over a specified time range. It uses this formula:


```text
increase  (  counter [  time_range ]  )    =   last_value  -   first_value


```


The syntax for increase() is similar to rate():


```text
increase  (  metric_name [  time_range ]  )


```


Increase() is particularly useful when you need to know the total change in a metric over time, rather than the rate of change. It's ideal for analyzing cumulative metrics or when you need to understand the absolute change in a value.


### Practical Applications of Increase()


1.


**Measuring total requests over a specific time period** :


```text
increase  (  http_requests_total [  24h ]  )


```


This query shows the total number of HTTP requests received in the last 24 hours.


2.


**Analyzing cumulative errors in a system** :


```text
increase  (  error_count_total [  1h ]  )


```


This displays the total number of errors that occurred in the last hour.


3.


**Tracking resource consumption in cloud environments** :


```text
increase  (  cloud_cpu_usage_seconds_total [  7d ]  )


```


This query reveals the total CPU time consumed over the past week.


## Rate vs Increase: When to Use Each


Choosing between rate() and increase() depends on your specific monitoring needs:


- Use rate() when you need to understand the speed of change or when working with metrics that are typically expressed as rates (e.g., requests per second).
- Opt for increase() when you need to know the total change over time or when working with cumulative metrics.


For alerting, rate() is often more suitable for detecting sudden spikes or drops in activity. Increase() is better for setting thresholds on total values over time.


## Best Practices for Using Rate() and Increase()


1. **Choose appropriate time ranges** : Shorter ranges provide more granular data but may be noisier. Longer ranges smooth out fluctuations but may miss short-term issues.
2. **Handle counter resets** : Both functions automatically handle counter resets, but be aware of potential inaccuracies immediately after a reset.
3. **Combine with other[PromQL functions](https://signoz.io/guides/promql-cheat-sheet/)** : Use functions like sum(), avg(), or max() with rate() or increase() for more complex analyses.
4. **Optimize query performance** : Avoid using unnecessarily long time ranges, as this can impact query performance.


## Key Takeaways


- Rate() calculates per-second average change; increase() shows total change over time.
- Both functions are essential for analyzing counter metrics in Prometheus.
- Choose between rate() and increase() based on your specific monitoring needs.
- Proper use of these functions is crucial for accurate data interpretation and alerting.


For more on Prometheus, the[Prometheus alternatives](https://signoz.io/comparisons/prometheus-alternatives/) roundup goes broader, and the[Prometheus vs Grafana](https://signoz.io/comparisons/prometheus-vs-grafana/) ,[OpenTelemetry vs Prometheus](https://signoz.io/blog/opentelemetry-vs-prometheus/) ,[Loki vs Prometheus](https://signoz.io/blog/loki-vs-prometheus/) , and[Prometheus vs InfluxDB](https://signoz.io/blog/prometheus-vs-influxdb/) comparisons cover adjacent matchups.


## FAQs


### What is the main difference between rate() and increase() in Prometheus?


Rate() calculates the per-second rate of change, while increase() computes the total change over a specified time range.


### Can I use rate() and increase() with gauge metrics?


No, these functions are designed for counter metrics.[Gauge metrics](https://signoz.io/guides/what-is-the-difference-between-a-gauge-and-a-counter/) should be analyzed using different PromQL functions.


### How do rate() and increase() handle counter resets?


Both functions automatically handle counter resets by calculating the rate or increase based on the available data points.


### What time range should I use with rate() and increase() functions?


The ideal time range depends on your specific use case. Generally, use shorter ranges (e.g., 5m) for more granular data and longer ranges (e.g., 1h) for smoother trends.


## Enhance Your Monitoring with SigNoz


While Prometheus offers powerful monitoring capabilities, managing retention and scaling can become challenging as your infrastructure grows. SigNoz provides a comprehensive monitoring solution that builds upon Prometheus' strengths while addressing its limitations.


SigNoz Cloud is the easiest way to run SigNoz.[Sign up](https://signoz.io/teams/) for a free account and get 30 days of unlimited access to all features.


You can also install and self-host SigNoz yourself since it is open-source. With 24,000+ GitHub stars,[open-source SigNoz](https://github.com/signoz/signoz) is loved by developers. Find the[instructions](https://signoz.io/docs/install/) to self-host SigNoz.


With SigNoz, you can:


- Scale your monitoring infrastructure effortlessly
- Access advanced querying and visualization capabilities
- Benefit from integrated tracing and logging alongside metrics.
- Get high performance with the clickhouse database
- Take advantage of[SigNoz](https://signoz.io/docs/introduction/) 's exceptional exception monitoring capabilities
