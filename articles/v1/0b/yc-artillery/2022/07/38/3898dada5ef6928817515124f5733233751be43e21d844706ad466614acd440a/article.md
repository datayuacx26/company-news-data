---
schema_version: "1.0.0"
document_id: "3898dada5ef6928817515124f5733233751be43e21d844706ad466614acd440a"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/open-source-distributed-load-testing-with-lambda"
published_at: "2022-07-15T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:7e993a2345e330a8f30a6aac321fd474ded38c91089dbc15d6fb975ca68860a4"
---

# Blitz it with Lambda: open-source, cloud-native distributed load testing

July 15th, 2022[Announcement](https://www.artillery.io/blog/tag/announcement)


# Blitz it with Lambda: open-source, cloud-native distributed load testing


Hassy Veldstra


Say hello to open-source serverless highly-distributed load testing at scale.


What we have here is a load test that starts at 25k requests per second and ramps up to >200k requests per second over about 3 minutes. That’s not the interesting part though. What’s more interesting than the raw numbers is that:


- The test is highly distributed and scalable - we can **scale out horizontally** to 400k RPS, 1m RPS or more with little effort
- It runs with Artillery so you have access to all of the powerful[features of Artillery](https://www.artillery.io/docs/guides/overview/why-artillery#features) like scenarios with multiple steps and request chains, multiple protocols, plugins and extensions and so on
- All of the **metrics are aggregated automatically** and in a statistically-sound way
- It’s **running on AWS Lambda** and is fully serverless and AWS-native. There is no infra to manage
- Running this for 10 minutes costs about $4


Pretty neat, isn’t it?


Oh and the best thing - it’s completely **open source** , and built right into the Artillery CLI.


## Open-source serverless highly-distributed load testing at scale


**This is load testing the way it should be in 2022.** We’re very excited to be releasing this update to Artillery - the biggest one in a while!


There is a lot of[accidental complexity](https://en.wikipedia.org/wiki/No_Silver_Bullet) in load testing, and the biggest barrier to running more load tests up until now has been a lack of options for **running distributed tests easily** and in a **modern cloud-native way** .


Ease-of-use and being cloud-native matter because together they **unlock the ability to load test at higher frequency & volume** . This combination is missing in other load testing tools currently available.


> This is load testing the way it should be in 2022 by @artilleryio. Fully open-source, serverless, and highly distributed.


### Existing hosted solutions fall short on pricing


There’s a lot of hosted load testing solutions out there. If you’re currently using one of those, your load testing spend is going to drop by orders of magnitude if you move to Artillery. You will no longer need to navigate complex pricing models based on[VU](https://www.artillery.io/docs/resources/glossary#virtual-user-vu) concurrency, VU minutes, and numbers of requests, and deal with often laughably small caps on those. You won’t be discouraged by outdated pricing models from running load tests more frequently, and at higher scale, possibly from from CICD pipelines to help increase confidence in new releases going out to production.


You can try a quick comparison yourself — the test above made a total of 58 million requests and created 530k virtual users for a total AWS cost of ~$4. How does that compare to the hosted solution you’re using now?


### Other open source solutions are not cloud-native


When it comes to other popular open-source tools, none of them are cloud-native, and running a distributed test requires wrangling with infrastructure at pretty low level of abstraction. Some tools have third-party solutions for running on Kubernetes, which is okay, but if you just want to run some tests, why do you need to figure out what the heck a Helm chart is, and how to download and install it? Or have to figure out how to build and host a custom Docker image? Oh and if you want to aggregate and present metrics from every node automatically[in a statistically-sound way](https://www.circonus.com/2018/11/the-problem-with-percentiles-aggregation-brings-aggravation/) you can forget it.


## Getting started


You will need to install the latest version of Artillery, and have an AWS account with an[AWS profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) configured locally. Artillery takes care of everything else, as long as you have the permissions required for Artillery to create and run AWS Lambda functions for your tests.


An IAM policy with all required permissions is available in the docs at[https://docs.art/aws-lambda](https://docs.art/aws-lambda) .


```text
npm   install   -g   artillery
```


```text
pnpm   add   -g   artillery
```


```text
yarn   global   add   artillery
```


```text
bun   add   --global   artillery
```


```text
# Grab an example test script from GitHub
curl   -o   blitz.yml   \
https://gist.githubusercontent.com/hassy/c723c93e2319449dd76f1602d81603da/raw/c6e71bc6e0041c095ef781e7c096192450c85410/blitz.yml


# Make sure you have an AWS profile set up
aws   sts   get-caller-identity


# Run the test script locally
artillery   run   blitz.yml


# And now run it from your own AWS account
artillery   run   \
--platform   aws:lambda   \
--platform-opt   region=us-east-1   \
--count   25   \
blitz.yml
```


## Limitations & caveats


### This is a preview release


**AWS Lambda support is in preview** . There are some limitations to what’s possible, and you may run into bugs. Please report any issues via[GitHub issues](https://github.com/artilleryio/artillery/issues) .


### AWS Lambda is great for short bursts of traffic


Each AWS Lambda is[limited to 15 minutes](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html#function-configuration-deployment-and-execution) of running time, which means that the entire load test cannot run for longer than 15 minutes. There may be ways to get around this limit, but for now running Artillery on Lambda is best suited to short 10-15m bursts of high traffic.


### You cannot stop a running test


Once an AWS Lambda starts running, **there is no way to stop it** . Neither the AWS SDK, nor the AWS Console provide that ability. This means that once a load test is kicked off, it will run to completion. Be mindful of this, and ramp up load on your applications gradually.


### Unavailable Artillery features


The following Artillery features are not supported yet, but we’re aiming for full coverage as soon as possible:


- Only built-in engines (HTTP, WebSocket and Socket.io) may be used
- Only built-in` publish-metrics` and` metrics-by-endpoint` plugins may be used
- Loading data from external CSV files with` payload`
- Loading and running custom code via` processor`
- Using any third-party plugins or engines
- ` --config` ,` --dotenv` ,` --target` , and` --insecure` flags for the` run` command
- ` ensure` specs
- ` before` and` after` hooks run once in each Lambda worker


### Do not test without consent


Lastly, it goes without saying that load testing systems you have no permission to load test is a crime in many jurisdictions. Pointing Artillery at any target that does not belong to you could result in prosecution and liability for any damages caused.


## What’s next?


AWS Lambda is only the beginning. Artillery’s goal is to be completely platform-agnostic and let developers run load tests wherever it makes sense. We’re launching support for the following platforms next:


- AWS ECS (Fargate and EC2 types)
- Kubernetes on AWS


With Kubernetes on Google Cloud and Azure to follow.


Stay up-to-date and be the first to know about new releases by[joining our mailing list](https://www.artillery.io/#newsletter) or following us on Twitter at[@artilleryio](https://twitter.com/artilleryio?ref_src=twsrc%5Etfw)


> Load testing at scale by @artilleryio. Fully open-source, serverless, and highly-distributed.
