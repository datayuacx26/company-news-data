---
schema_version: "1.0.0"
document_id: "b217da101e0ef127686423473a75e9f794cd244cd19f68cbec5b895ac30fb969"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/improving-trust-with-datadog-log-management/"
published_at: "2018-11-01T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:e5c0a8e45197c0ce21d5b2425039684cdc4bea06f557c127c4c33c25e2b39d24"
---

# Improving trust with Datadog Log Management

Jules Denardou


At Datadog, we send hundreds of thousands of emails per day, and the number continues to grow. To accommodate the delivery and analytics for this volume of outbound emails, we use a number of third-party mail providers. While all of this mail is important, some emails are more vital than others, such as password resets. These messages require the highest trust in the applications and infrastructure used to send them.


At a high level, the problem is fairly simple. We want to send certain emails to customers using services that require little to no maintenance. Additionally—and this is where the challenge lies when using third-party services—our support team needs easy access to email-related metrics, so they can properly assist customers and troubleshoot login issues.


This blog post explains how Datadog uses a managed service from one of our cloud providers as well as[Datadog’s Log Management product](https://www.datadoghq.com/product/log-management/) to create a trusted, observable, and serverless solution for password reset emails.


## Exporting Amazon SES events


Rather than creating our own self-hosted email service, one of the services we use is Amazon Simple Email Service (SES) because AWS is one of our cloud providers. Although Amazon SES provides some metrics, the default way they are collected and displayed did not meet our needs. While the data exists within SES and CloudWatch, it is not readily exposed in a manner that would help answer common support questions, such as whether a specific recipient recently received a password reset email. More importantly, our security team strives to build solutions that meet fellow engineers where they work, without impacting their workflow. In the case of password-related events, this means using Datadog’s Log Management product to make it easy to search password reset emails via predefined facets.


We designed a solution where the metrics would be directly accessible from Datadog and easily readable. We use Amazon SES configuration sets to publish to an SNS topic whenever an event is triggered on an email. This SNS topic then triggers a Lambda that transfers the email event to Datadog Log Management. Here is the global overview of the logic and infrastructure involved in the process:


The first step is to create an Amazon SES configuration set. In this configuration set, you can specify an event destination. An event destination defines which event will generate logs and where they are sent. The different event types you can monitor are: bounce, click, complaint, delivery, open, reject, and send. Events can only be sent as Cloudwatch Metrics, Firehose data, or Simple Notification Service (SNS) messages. Using SNS is the easiest solution, as it integrates naturally with the Datadog Logs Lambda function to forward the event to Datadog Log Management.


```text
1   resource     "aws_sns_topic"     "ses_to_datadog_logs"   {    2       name           =     "ses-to-datadog-logs"    3       display_name   =     "ses-to-datadog-logs"    4       policy         =     "  ${  file  ("sns_policy.json")  }  "    5   }    6
7   resource     "aws_ses_configuration_set"     "ses_to_datadog_logs"   {    8       name   =     "ses-to-datadog-logs"    9   }    10
11   resource     "aws_ses_event_destination"     "ses_to_datadog_logs"   {    12       name                     =     "ses-to-datadog-logs"    13       configuration_set_name   =     "  ${  aws_ses_configuration_set  .  ses_to_datadog_logs  .  name  }  "    14       enabled                  =     true    15       matching_types           =     [  "send"  ,   "reject"  ,   "bounce"  ,   "complaint"  ,   "delivery"  ,   "open"  ,   "click"  ]    16
17       sns_destination   =     {    18         topic_arn   =   "  ${  aws_sns_topic  .  ses_to_datadog_logs  .  arn  }  "    19        }    20
21       depends_on   =     [  "aws_ses_configuration_set.ses_to_datadog_logs"  ]    22   }
```


The next step is to create the Lambda that will be triggered by every event sent to SNS. The code of the Lambda can be found[in our Github project repository](https://github.com/DataDog/dd-aws-lambda-functions/blob/master/Log/lambda_function.py) . You can create the Lambda with this Terraform snippet by running` terraform apply` .


```text
1   resource     "aws_iam_role"     "ses-to-datadog-logs-role"   {    2       name   =     "ses-to-datadog-logs-role"    3
4       assume_role_policy   =     <<  EOF    5   {    6        "Version": "2012-10-17",    7        "Statement": [    8          {    9            "Action": "sts:AssumeRole",    10            "Principal": {    11              "Service": "lambda.amazonaws.com"    12            },    13            "Effect": "Allow",    14            "Sid": "AllowAssumeRoleByLambda"    15          }    16        ]    17   }    18   EOF    19   }    20
21   resource     "aws_lambda_function"     "ses-to-datadog-logs"   {    22       filename           =     "ses-to-datadog-logs.zip"    23       function_name      =     "ses-to-datadog-logs"    24       role               =     "  ${  aws_iam_role  .  ses-to-datadog-logs-role  .  arn  }  "    25       handler            =     "lambda_function.lambda_handler"    26       source_code_hash   =     "  ${  base64sha256  (  file  ("ses-to-datadog-logs.zip"))  }  "    27       runtime            =     "python2.7"    28
29       environment   {    30         variables   =     {    31           DD_API_KEY   =   "<your_api_key>"    32          }    33        }    34   }
```


In production we would recommend the API Key be encrypted, but that implementation is outside the scope of this post.


With the serverless infrastructure in place, the full pipeline is complete, and SES logs are sent to Datadog whenever an email is processed using the previously defined configuration set. The last step is to configure Datadog to make it easier for users to read the logs.


## Viewing SES events in Datadog


Logs are sent from AWS to Datadog as JSON, and thus are processed automatically at intake via a pre-existing[integration pipeline](https://docs.datadoghq.com/logs/processing/pipelines/#integration-pipelines) . To make email events more easily searchable, you can simply open an email log line, and transform the parameters that are important to you into facets in a single click. For example, here at Datadog, we use the mail event type and the subject of the email as facets to ensure the corresponding values are indexed.


## Conclusion


By adding Datadog Log Management to Amazon SES, we have simple, low-maintenance infrastructure for password reset emails, but with increased trust and built-in observability for our support team. Since we can easily create monitors on top of our setup, we are alerted through normal escalation channels if the service is impacted on any part of the pipeline.


-
-
-
