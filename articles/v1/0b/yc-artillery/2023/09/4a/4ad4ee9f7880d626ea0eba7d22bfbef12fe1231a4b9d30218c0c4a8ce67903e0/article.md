---
schema_version: "1.0.0"
document_id: "4ad4ee9f7880d626ea0eba7d22bfbef12fe1231a4b9d30218c0c4a8ce67903e0"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/github-action-fargate-lambda"
published_at: "2023-09-25T12:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:108b8d933ff6a549ce13c847a78e2c807f877ed1979701fa8d969bc9f7af1116"
---

# Using GitHub Actions to run Fargate and Lambda tests

September 25th, 2023[How to](https://www.artillery.io/blog/tag/howto)


# Using GitHub Actions to run Fargate and Lambda tests


Bernardo Guerreiro


Earlier this year we announced the[release of our official GitHub Action](https://www.artillery.io/blog/announcing-the-artillery-github-action) for running Artillery tests.


In this blog post we’ll walk you through setting up a load test using Fargate and Lambda and our official GitHub Action, including some AWS IAM permissions you may have to go set up.


## Why Fargate or Lambda?


While the GitHub Action with the` run` command is perfectly fine for a small *smoke* or *acceptance* test, any meaningful or large load test will likely have a big resource impact on the host machine running it.


By leveraging[Lambda](https://www.artillery.io/docs/load-testing-at-scale/aws-lambda) and[Fargate](https://www.artillery.io/docs/load-testing-at-scale/aws-fargate) horizontal scaling capabilities, you can scale your test as much as you want, while still keeping all reporting going to the GitHub Action. This use-case is exactly why we[open-sourced access to Fargate and Lambda](https://www.artillery.io/blog/open-source-distributed-load-testing-with-lambda) , so that anyone can run distributed load tests easily from anywhere.


## Setting up your test


Running tests with GitHub Actions in Fargate or Lambda is very similar from a test scenario point of view. The main difference will be in your config, as each worker in Fargate/Lambda will run a copy of the test, so we need to decide how many workers we want.


For instance, let’s say our goal is to setup a nightly test to gather a baseline of our application with 500 users per second. Take the following` scenario.yml` and config as an example:


```text
config  :
target  :   http://asciiart.artillery.io:8080
environments  :
ci  :
phases  :
-   arrivalRate  :   100
duration  :   10m
smoke  :
phases  :
-   arrivalRate  :   10
duration  :   60s
scenarios  :
-   name  :   main flow
flow  :
-   get  :
url  :   '/'
```


*Note: in the example above, the **ci** phase was set with an` arrivalRate` of 100, because we will use a` --count` of 5 workers to get to the desired 500 virtual users.*


## Plugging it into a GitHub Action


Now that we have our test, we can create a skeleton of a GitHub Action with it:


```text
name  :   Running Artillery in Lambda or Fargate


on  :
workflow_dispatch  :
schedule  :
-   cron  :   '0 12 * * *'   #setup a nightly cron for midnight


permissions  :
contents  :   read   #this is needed so that checkout of repository can happen


jobs  :
artillery  :
runs-on  :   ubuntu-latest


steps  :
-   name  :   Checkout repository
uses  :   actions/checkout@v3


-   name  :   Run Fargate Test
uses  :   artilleryio/action-cli@v1
timeout-minutes  :   15   # best practice: adding timeout in case something goes wrong with the test, so that GitHub Action won’t run for the default 6h
with  :
command  :   run-fargate ./scenario.yml --count 5 --output ./report.json


-   name  :   Run Lambda Test
uses  :   artilleryio/action-cli@v1
with  :
command  :   run-lambda ./scenario.yml --count 5 ./report.json
```


We’re almost there, but not quite yet! The above action would work for a` run` command, but it needs to have the right AWS permissions setup in order to run Lambda/Fargate tests. Let’s do that next.


## AWS Permissions


### Set up an IAM Role and Policy


Usually when running commands that require access to AWS from a CI/CD system, there will be a role scoped to the correct permissions needed.


We’ve tried to make this easy for you by providing guides for[AWS Fargate](https://www.artillery.io/docs/load-testing-at-scale/aws-fargate#iam-permissions) and[AWS Lambda](https://www.artillery.io/docs/load-testing-at-scale/aws-lambda#iam-permissions) on the permissions needed, and how to create the roles and policies.


Having a role and policy created is a pre-requisite, so make sure you do that or work with people in your organization who have the ability to do so.


### Configure the GitHub Action to use them


We recommend you use AWS’s official[configure-aws-credentials GitHub Action](https://github.com/aws-actions/configure-aws-credentials) for this purpose.


The exact configuration of the GitHub Action will depend on the method used for authentication. We’ll provide you with the recommended mechanism below, but please check the documentation above for more details.


#### Using OIDC (Recommended)


First, update your` permissions` in the action:


```text
permissions  :
contents  :   read   # needed so that checkout of repository can happen
id-token  :   write   # additionally needed now for OIDC
```


Then, add this **before Artillery tests run** :


```text
-   name  :   Configure AWS Credentials
uses  :   aws-actions/configure-aws-credentials@v2
env  :
SHOW_STACK_TRACE  :   true   # in case there are issues, this can help with debugging
with  :
aws-region  :   eu-west-1
role-to-assume  :   ${{ secrets.ARTILLERY_ROLE_ARN }}   # arn of the role created in AWS, should be kept in GitHub Secrets
role-session-name  :   OIDCSession
```


*Note: this assumes that OIDC has been setup in the AWS account, as described in our guides. GitHub has a[guide here](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services#configuring-the-role-and-trust-policy) .*


If you want to use something other than OIDC, you should check the documentation on how to do it. However, it is not recommended, as otherwise you have to store long-lived credentials in GitHub Secrets. For instance, here is a guide for[using static IAM credentials ›](https://github.com/aws-actions/configure-aws-credentials#assumerole-with-static-iam-credentials-in-repository-secrets)


## Some Extras


### Playwright Tests


As our Fargate image officially supports Playwright, by running your tests in Fargate, you can easily run your Playwright Tests from GitHub Actions!


### Reporting to Artillery Cloud Dashboard


While it’s currently in beta, our Artillery Cloud dashboard is the best way to complement running tests on CI, as you’ll be able to visualise runs over time easily, check historical results, and more!


Once again, there’s no changes needed to the scenario and config to report to the Dashboard, so once you have an API Key, you’ll need to pass` --record` to the command to report to cloud. For example:


```text
-   name  :   Run Fargate Test
uses  :   artilleryio/action-cli@v1
with  :
command  :   run-fargate ./scenario.yml --count 5 --output ./report.json --record --tags ci:true,owner:qa,type:baseline   # we add `--record` and a few tags to identify the test
env  :
ARTILLERY_CLOUD_API_KEY  :   ${{ secrets.ARTILLERY_CLOUD_KEY }}   # cloud key set in gh action secrets
```


> **[Join the waitlist for Artillery Cloud](https://www.artillery.io/cloud?tf=1) to get early access to the platform!**


### Saving Reports


You can use the official` upload-artifact` GitHub Action for this:


```text
-   name  :   Upload artifact
uses  :   actions/upload-artifact@v3
if  :   always()
with  :
name  :   artillery-report
path  :   ./report.json   # reference the generated report in the file system
```


### Failure Notifications to Slack


There are many actions in the Marketplace for this. For example, you can use[action-slack](https://github.com/marketplace/actions/action-slack) , and simply add something like this to the workflow:


```text
-   name  :   Notify about failures
if  :   failure()
uses  :   8398a7/action-slack@v3.15.1
with  :
status  :   ${{ job.status }}
fields  :   repo,message,commit,author,eventName,job,took,pullRequest
env  :
SLACK_WEBHOOK_URL  :   ${{ secrets.SLACK_WEBHOOK_URL }}   # required in secrets
```


## Conclusion


That’s all! Once you do this setup, your workflows will be able to run against Fargate and Lambda, and give you meaningful insights about your system performance.


Happy load testing!
