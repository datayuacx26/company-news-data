---
schema_version: "1.0.0"
document_id: "78594320c7a8d4e55dff155695f6dd11429969fed43e9a130b59fa2dbc797a3c"
company_key: "yc-taloflow"
company: "Taloflow"
source_id: "yc-taloflow-rss-cfae8c512c9e"
canonical_url: "https://www.taloflow.ai/blog/is-aws-down"
published_at: "2022-04-26T16:00:00+00:00"
first_seen_at: "2026-07-26T01:23:39.915767+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:8ba8033b47187dca71ace98ff85549c65bc0c2c6e101163fd8fad30ef970e273"
---

# Is AWS Down? Independent Health Checks for 10 Regions and 7 Services

At Taloflow, we depend on AWS, and so do many of our customers. If AWS is down, we've got to know! Unfortunately, the official AWS status site is a bit vague on details, and popular downtime detectors are just not suited for this purpose.


To provide the developer community with a better alternative, the Taloflow team is proud to present[Is AWS Down?](https://www.taloflow.ai/is-aws-down) , a free AWS outage/uptime checker that performs thousands of health-checks every day on the most popular AWS services and regions so you never have to wonder again: is AWS down?


## How does it work?


"Is AWS Down?" provides timely and more granular insights into AWS outages. **Using the AWS API, we invoke 7 different kinds of services directly, including Lambda, EC2, S3, SQS, DynamoDB, IAM, and API Gateway.** If the service does not respond in 30 seconds we mark it as failed. (Due to how AWS sets up their availability zones, we may experience issues that you do not, or vice versa.)


The service is **currently available in 10 AWS regions:** us-east-1, us-east-2, us-west-1, us-west-2, eu-west-1, eu-west-2, sa-east-1, ap-south-1, ap-southeast-2, ca-central-1.


Is AWS Down? available regions and services


We run this service on Google Cloud Platform so that the service is available when you need it most (i.e.: when AWS is down!). We plan to expand this free service we built for the developer community to other regions and cloud platforms soon.


**We’d love to hear your feedback and whether this is useful to you.**[\[email protected\]](https://www.taloflow.ai/cdn-cgi/l/email-protection#8bffeeeae6cbffeae7e4ede7e4fca5eae2)


Otherwise, happy debugging!


Taloflow Team


**P.S.:** There are some known issues, like the voting game has a lag, email alerts send for every available region (can’t customize this yet) and do not route to 6 different providers (will update the setup form).


‍
