---
schema_version: "1.0.0"
document_id: "263e279c7f64e9e1978f6e9265c951878ff45702035bd4588dacbcf483527979"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/datadog-integration-with-aws"
published_at: "2021-10-06T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:650ac543c4c62397127b98c527c9a792eb6805e14df444dddbaea6a6e5d5a32d"
---

# Datadog Integration with AWS

## Introduction


If you have ever tried to integrate Datadog with AWS, you might have been overwhelmed with the configuration required to stream your logs and metrics. If you're like us, you found yourself wondering which integration method to use. Do we use the \[Kinesis Firehose\](https://docs.datadoghq.com/logs/guide/send-aws-services-logs-with-the-datadog-kinesis-firehose-destination/) or \[Lambda Forwarder\](https://docs.datadoghq.com/logs/guide/send-aws-services-logs-with-the-datadog-lambda-function/) for log collection? Do we use \[Metric polling\](https://docs.datadoghq.com/integrations/faq/cloud-metric-delay/#aws) or \[Metric streams with Kinesis Firehose\](https://docs.datadoghq.com/integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/?tab=cloudformation) for metric collection? Wait, there's also a \[Datadog agent\](https://docs.datadoghq.com/getting_started/agent/)? In this post, we share our analysis for each method of integration and guide the use of each.


**tl;dr:** You will need multiple ways to achieve full observability of AWS in Datadog. If you are interested, skip to the end for easy integration with fine control that keeps your AWS and Datadog bills low while avoiding noisy logs/metrics.


## Methods


Before analyzing each technique, let's take a look at how each method works.


### 1-click integration (logs and metrics)


If you are looking to get started quickly, Datadog introduced \[1-click integration\](https://www.datadoghq.com/blog/aws-1-click-integration/). This integration configures your account with Lambda Forwarder and Metric Polling. See those methods for details on the 1-click integration.


### Lambda Forwarder (logs)


The Datadog Lambda Forwarder is a Lambda function that runs inside your account. As your applications record logs to Cloudwatch or save objects to s3, the Lambda relays these messages and s3 events to Datadog. You can configure the Lambda Forwarder in automatic mode, which will forward all logs to Datadog. Otherwise, you can manually configure each Cloudwatch Log Group to forward logs to Datadog.


### Kinesis Firehose (logs and metrics)


Within your AWS account, you configure a Kinesis Firehose Delivery Stream to relay to Datadog. Subsequently, you configure the Firehose to subscribe to Cloudwatch Log Groups and Cloudwatch Metrics. The subscriptions forward logs and metrics to the Firehose. The Firehose buffers data until a time limit expires or the buffer exceeds a specified size, at which point; the Firehose forwards the data to Datadog.


### Metric polling (metrics)


After configuring an IAM Role in your AWS account, you configure Datadog to crawl your account for Cloudwatch Metrics. The Datadog crawler runs every ~10 minutes and extracts new data points from Cloudwatch.


### Datadog Agent (logs and metrics)


The Datadog Agent is a daemon installed on an EC2 box or as a container in a docker cluster. The agent collects and receives logs and metrics from its machine (and cluster if desired) and sends them to Datadog.


### \[Bonus\] Firelens


Firelens is an AWS logging driver that allows you to route docker container logs running on ECS. This option requires running a sidecar, which is an additional docker container running alongside your main container. The Datadog docs make little mention of this option - making it a bonus technique.


## Comparison


To understand which options are best, we break down the pros and cons on different dimensions: speed of ingestion, flexibility, how much it will cost your team to develop, and how much it will cost in ongoing infrastructure.


### Ingestion Speed


We consider all the time needed for users to use the logs and metrics in Datadog as ingestion speed. This dimension includes polling delays, buffer delays, ingestion indexing, and sampling intervals.


| Method | Logs Available | Metrics Available |
|:--------|:----------------|:-------------------|
| Lambda Forwarder | Real-time | N/A |
| Kinesis Firehose | Low volumes: ~1 minute; High volumes: Real-time | ~5-10 minutes |
| Datadog Agent | Real-time | ~5-10 minutes |
| Metric polling | N/A | ~15-20 minutes |
| Firelens | Real-time | N/A |


### Flexibility


We consider flexibility to measure the ability to tune log and metric data easily. This dimension includes granularity of the data and metadata to filter out unwanted or noisy logs and metrics. This dimension also measures how easy it is to configure for Containers, Serverless, and Static Sites.


| Method | Granularity | Containers| Serverless | Static Sites |
|:--|:--|:--|:--|:--|
| Lambda Forwarder (Automatic) | Poor | Easy | Easy | N/A |
| Lambda Forwarder (Manual) | Great | Medium | Medium | N/A |
| Kinesis Firehose | Great | Medium | Medium | Hard |
| Datadog Agent | Great | Hard | N/A | N/A |
| Metric polling | Poor | Easy | Easy | Easy |
| Firelens | Great | Medium | N/A | N/A |


### Development Cost


We are biased toward automation since most engineering teams need to set up multiple environments and accounts. We are considering development costs to configure each technique and set it up for automation. Also, we have weighted development costs for expertise. If your team has significant cloud experience, you can assume smaller estimates.


| Method | Initial setup | Setup for new services | Level of Expertise |
|:--|:--|:--|:--|
| Lambda Forwarder (Automatic) | 1/2 day | 0 | None |
| Lambda Forwarder (Manual) | 1 day | 1/2 day | Low |
| Kinesis Firehose | 1 week | 1/2 day | Medium |
| Datadog Agent | 2 weeks | 1/2 day | High |
| Metric polling | 1/2 day | 0 | None |
| Firelens | 1 week | 1/2 day | Medium |


### Infrastructure Cost


We consider total infrastructure cost to fully utilize Datadog, which includes usage pricing to your Datadog bill as well as services running in your AWS account to emit logs and metrics.


| Method | AWS | Datadog |
|:--|:--|:--|
| Lambda Forwarder (Automatic) | $$ | $$ |
| Lambda Forwarder (Manual) | $$ | $ |
| Kinesis Firehose | $ | $ |
| Datadog Agent | $$$ | $ |
| Metric polling | $$ | $$$ |
| Firelens | $ | $ |


## Which option is best?


The one-click integration is excellent if you want to try out Datadog but lacks control and will become a burden to your team and wallet when you add environments and AWS accounts.


For most, we recommend a combination of 2 options to achieve full observability. Datadog Agent and Kinesis Firehose.


If you are running EC2 workloads, you need to install the Datadog Agent. However, if you are running an ECS or EKS cluster, it is best to run the Datadog Agent as a daemon container that runs on every worker node.


For everything else, we prefer the Kinesis option. To set up Kinesis Firehose requires in-house DevOps expertise to configure the subscriptions properly. However, the additional effort is worth the control you gain over noise and costs. In addition, there is a one-minute delay at low volumes; this is likely not enough to disrupt your workflow.


## Nullstone


Whether you have in-house DevOps expertise or not, it would be nice to use the best option without spending the development effort to configure every Container, Lambda, or Static Site we launch.


We have built Nullstone to solve problems like this across your entire engineering tech stack. We provide a repeatable, one-click integration with Datadog that keeps your infrastructure bills low with fast, reliable data delivery. Check out our \[guide\](https://docs.nullstone.io/getting-started/logs-metrics/datadog.html) in the docs for details.


## Conclusion


By incorporating a telemetry provider like Datadog, your team can dramatically improve reliability and reduce diagnostics. Rather than tracking down issues, your team will be able to focus on expanding features in your product.


Datadog integration is challenging to configure. However, equipped with the right tools, your team can achieve comprehensive observability over your cloud systems.


‍
