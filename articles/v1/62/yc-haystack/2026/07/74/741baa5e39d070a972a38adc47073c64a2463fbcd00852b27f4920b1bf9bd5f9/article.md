---
schema_version: "1.0.0"
document_id: "741baa5e39d070a972a38adc47073c64a2463fbcd00852b27f4920b1bf9bd5f9"
company_key: "yc-haystack"
company: "Haystack"
source_id: "yc-haystack-news-import-51ea83245eb5"
canonical_url: "https://www.usehaystack.io/blog/measuring-devops-beyond-dora-accelerate-metrics"
published_at: null
first_seen_at: "2026-07-21T22:30:19.779153+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:729079c742947bb893492780199bfe846d0b6225ab4e75cd49fdac0391fb0208"
---

# Measuring DevOps: Beyond DORA Accelerate Metrics

The DORA Accelerate Metrics (sometimes called the “Four Key Metrics”) have played an important role in moving forward how the industry measures DevOps. Instead of focusing on poor activity-based metrics or metrics that promote micromanagement, DORA metrics provide an empirical view of how a team is performing at DevOps and how they can improve.


The DORA metrics measure DevOps performance using the following metrics:


- **Change Lead Time** - Time from first commit to a change (i.e. Pull Request) being deployed into production.
- **Deployment Frequency** - Number of deployments completed over a given time period, with the highest performing teams able to deploy on-demand.
- **Change Failure Rate** (CFR) - The percentage of deployments causing a failure in production.
- **(Mean) Time To Recovery** (MTTR/TTR) - How long it takes to recover from a failure in production.


Rigorous[appraisal](https://youtu.be/RBuPlMTXuFc) of these Four Key Metrics (in Google’s State of DevOps Reports) has shown that higher performers are 2x more likely to meet their commercial goals (productivity, profitability, market share, number of customers) and their non-commercial goals (quantity of products or services, operating efficiency, customer satisfaction, quality of products or services and achieving organisational or mission goals). Indeed, companies which do well under these DevOps metrics have a 50% higher market cap growth over 3 years.


These metrics are even correlated to engineering well-being; indeed,[the 2018 State of DevOps report](https://services.google.com/fh/files/misc/state-of-devops-2018.pdf) found that companies which were elite performers against the Four Key Metrics "are 1.8 times more likely to recommend their team as a great place to work".


Whilst these metrics are a step in the right direction, they are imperfect and incomplete. Below we’ve included some metrics that we think are good to track to get a better picture of your engineering team and potentially a metric you might want to exclude from your analysis.


## Swap out Deployment Frequency (for Change Lead Time)


The DORA State of DevOps metrics does include Deployment Frequency as a metric, but often this metric is measured from a scale of “on-demand” to a certain frequency (every day, every week, etc). In low-activity projects, it’s perfectly acceptable not to deploy overly frequently, so long as you’re able to deploy fast when you need to.


Change Lead Time is already part of the DORA Four Key Metrics and measures how long it takes to actually get a change into production. Providing you’re using a metrics tool that correctly measures Change Lead Time, you can instead use this as a measure of your deployment performance.


## Onboarding Time


For high growth customers, it is often important to ensure that developers can be set up and productive ASAP. By measuring the amount of time it takes for a developer to make their first commit from being added to a GitHub organisation, you can measure how quickly they are able to get onboard to your development stack and start being productive. Fast onboarding time optimises for flow allows developers to get up and running with minimal frustration.


## Full Resolution Time (for Bugs)


For excellent customer service, it’s important that bugs are resolved quickly after they are reported. Indeed, this metric can directly play a role in optimising customer satisfaction. By measuring the time it takes an engineering team to fully resolve a bug after it has been reported, you can ensure you are optimising for an outstanding customer experience. Unlike MTTR metrics, Full Resolution Time accounts for the fix time of all customer facing bugs instead of just those that cause an incident (however that is defined for your organisation).


## Developer Satisfaction/Net Promotor Score


Using Developer Satisfaction or Developer Net Promotor Score surveys, you are able to ensure your developers are satisfied with your development stack and identify areas for improvement. This helps ensure that your developers aren’t just able to ship work fast and safely, but are also satisfied with their developer platform.


## Conclusion


In this blog post we’ve considered a number of metrics that give you a more holistic view of the performance of your engineering team, beyond just what is offered in vanilla DORA metrics. To summarise, the following metrics provide great measures of engineering speed, quality and satisfaction:


- **Onboarding Time** - Time from joining an organisation to first commit.
- **Full Resolution Time for Bugs** - How long it takes to fix a bug once it is reported.
- **Developer Satisfaction/Net Promotor Score** - Satisfaction or NPS score for the developer experience of a product, with optional additional feedback.


‍
