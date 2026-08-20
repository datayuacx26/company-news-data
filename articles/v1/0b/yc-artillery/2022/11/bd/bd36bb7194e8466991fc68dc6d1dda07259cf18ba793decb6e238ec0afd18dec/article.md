---
schema_version: "1.0.0"
document_id: "bd36bb7194e8466991fc68dc6d1dda07259cf18ba793decb6e238ec0afd18dec"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/this-load-test-cost-us-how-much"
published_at: "2022-11-09T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:e68abf6fae0007f3fb82e16db02ea602070b516ccf8438f5cf1193ac63f82bff"
---

# This load test cost us… how much!?

November 9th, 2022[Announcement](https://www.artillery.io/blog/tag/announcement)


# This load test cost us… how much!?


Hassy Veldstra


Running load tests at scale can get expensive very quickly, **unless you use Artillery** . If you’re not familiar with Artillery - we do cloud-native load testing. Your tests run in your own AWS account, from AWS Lambda or AWS Fargate, and the transition from running a test from a local machine to scaling it out and running it from hundreds of workers in the cloud is seamless.


## New in Artillery - cost reporting for AWS Lambda


We added built-in cost reporting for AWS Lambda in the latest release of Artillery. The way it works is straightforward: you run a load test on AWS Lambda, and Artillery tells you how much that cost in AWS Lambda fees. It works across all regions and takes Lambda size into account. There’s nothing for you to configure.


### Load testing at scale does not have to be be ridiculously expensive


Running directly on AWS also means an orders-of-magnitude cost advantage over using hosted load testing solutions, which unlocks entire new use-cases for load testing. As a developer, you no longer worry about cost and can focus on running load tests to improve and optimize your systems.


Running tests on AWS Lambda is completely open-source and free ([see the initial launch announcement here](https://www.artillery.io/blog/open-source-distributed-load-testing-with-lambda) ), and we just shipped a few more updates, including **cost reporting** .


### Demo: Serverless Load testing


See what running a serverless load test with Artillery looks like in this short demo. We’ll run a sustained load of ~100k RPS for a few minutes, and Artillery will tell us how much it cost (spoiler: not very much).


We’ll be running the test script we shared in[the initial launch announcement](https://www.artillery.io/blog/open-source-distributed-load-testing-with-lambda) .


You can try running the test yourself with:


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
# Grab an example test script from GitHub:
curl   -o   blitz.yml   \
https://gist.githubusercontent.com/hassy/c723c93e2319449dd76f1602d81603da/raw/c6e71bc6e0041c095ef781e7c096192450c85410/blitz.yml


# Make sure you have an AWS profile set up:
aws   sts   get-caller-identity


# Run the test script locally:
artillery   run   blitz.yml


# And now run it from your own AWS account:
artillery   run   \
--platform   aws:lambda   \
--platform-opt   region=us-east-1   \
--count   30   \
blitz.yml
```


> 100k RPS load test for less than a cup of coffee! Open source serverless load testing with @artilleryio.


## Start load testing with Artillery and AWS Lambda


All of this available in the most recent release of Artillery. Install Artillery with` npm install artillery@latest` and start load testing at scale. All you need is an AWS account, Artillery will do the rest.


Learn more in our guide on[load testing with AWS Lambda](https://www.artillery.io/docs/guides/guides/distributed-load-tests-on-aws-lambda) , and check the project out on GitHub:[https://github.com/artilleryio/artillery](https://github.com/artilleryio/artillery)
