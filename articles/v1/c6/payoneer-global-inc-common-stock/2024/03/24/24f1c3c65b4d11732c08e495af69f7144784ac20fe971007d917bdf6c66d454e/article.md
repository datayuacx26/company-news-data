---
schema_version: "1.0.0"
document_id: "24f1c3c65b4d11732c08e495af69f7144784ac20fe971007d917bdf6c66d454e"
company_key: "payoneer-global-inc-common-stock"
company: "Payoneer Global Inc."
source_id: "payoneer-global-inc-common-stock-rss-9fa92a296e72"
canonical_url: "https://engineering.payoneer.com/coding-with-purpose-becoming-kpi-driven-9b520abcd5e9"
published_at: "2024-03-25T07:02:28+00:00"
first_seen_at: "2026-07-20T23:18:22.924920+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:07ab2621407f708e41a6af49c6cf485c046411c379c5e6372da93eba552ba310"
---

# Coding with Purpose: Becoming KPI-Driven

# Coding with purpose: Becoming KPI-driven


[Avi Thalker](https://medium.com/@avithalker?source=post_page---byline--9b520abcd5e9---------------------------------------)


4 min read


·


Mar 25, 2024


--


Press enter or click to view image in full size


Have you ever stopped to think about the impact of a new feature you just developed? Do you even know why you were requested to develop it? Are you familiar with your customer’s most urgent needs?


If these questions leave you with an uncertain “I don’t know”, you better change it ASAP!


Knowing the impact of your development on the business and understanding the numbers behind a business requirement are essential for a healthy development lifecycle. You must know what your main goal is and how to measure your success.


Now, let’s dive into the strategies for bringing this vision to reality.


## Define your KPI


A **K** ey **P** erformance **I** ndicator (KPI) is a measurable value that demonstrates how effectively your system is achieving the business goal.


Take, for instance, a system with the goal of automating the review process of customer’s documents. In this scenario, the KPI can be defined as: “Automation percentage out of all documents per month” and your main effort will be to increase the automation rate.


The power of a well-defined KPI lies in its ability to offer clarity, guiding your focus and providing a concrete measure of success.


## Visualize your KPI


Create a dashboard to visualize the progress of your KPI over time. Using time series graphs can be great choice for achieving this goal. You will be able to correlate the trends on the graph to your previous developments to see their impact.


Press enter or click to view image in full size


Left metric to present the performance of the KPI in the last 2 weeks as an absolute number. Right metric to present the performance of the KPI as a time series graph over time.


There are various platforms to create dashboards, analyze data, and visualize metrics, including Grafana, PowerBI, Tableau, etc.


## Get Avi Thalker’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


In my team we utilized Grafana for visualizing our metrics.
We used a “time scale” of a sprint (2 weeks) for our time axis of the graph in order to present progress between sprints.


You can enhance your dashboard by having cross sections of your KPI focus on specific populations. This reveals more insights, that can guide you where to put the efforts in order to maximize the business impact.


I recommend that you build a dashboard that’s automatically updated and doesn’t require manual effort in order to generate up to date reports. This will streamline the tracking process of your KPI and provide real-time insights.


## Continuously monitor your KPI


It’s crucial to monitor the performance of your KPI over time, just as you would when technically monitoring your services for errors or performance degradation.
**Degradation of your KPI needs to be considered the same as bug in production.** Sometimes it will occur due to actual bugs! **** You must become aware of it as quickly as possible, understand the root cause, and figure out how to bring it back to normal.


Without monitoring the KPI, there might be bugs in the system or in the business flow that you aren’t aware of, as they are not “yelling” error or exception.


In one of our systems, we had an issue with a third party that was responsible to identify if a file is authentic or not. It responded with the following: authentic, fraud, or indecisive. The third party had a bug that increased the “indecisive” response rate. Although this is a legitimate response, it was not seen as an error, and it had a direct impact on our KPI as it caused the files to go to manual review while our KPI focus was on increasing the file review automation.


Establish a routine with the team to review the KPI dashboard. This can be done daily, weekly, or at the end of each sprint.
Additionally, set up automatic alerts for the KPI and its related metrics whenever there is a drop or spike. For alerts, my team utilizes Grafana and Anodot.


## Prioritize based on business impact


When you have a clear KPI, you can more easily know where to place your focus. You can decide if each requirement serves your KPI or not. For the requirements that serve your KPI, **** make sure to have analysis that shows estimation of the future impact on the KPI. For example: “We have X cases like this in a month. Fixing it will increase the KPI by Y%”. Knowing the impact on the KPI is an integral part of the requirement and will help you prioritize by business value. This way, you will know what to anticipate while deploying to production.


It’s true that not all requirements are KPI related, for example refactoring, technical debts, or bug fixing tasks. These are essential for “keeping the light on”. **Strike a balance between KPI-related features and addressing technical debts.** Neglecting technical debts will eventually damage your ability to improve your KPI. Make sure to dedicated part of your sprint to these tasks.


Working with KPIs will create a positive effect on the team. They will truly understand the impact of their development, which will automatically increase their engagement and sense of responsibility.


Using and monitoring KPIs will create great sense of satisfaction upon success and will increase the accountability level upon failure.


Knowing the numbers behind the requirement will strengthen the connection of the team to the business. It will encourage them to use their own initiatives to come up with new solutions.


Defining a KPI not only clarifies your goal but also empowers you with the means to measure and enhance your success. Visualize it in order to monitor and see progress over time. Prioritization becomes the strategic lever, ensuring efficient resource allocation. Embrace these principles, and witness your team’s efforts flourish, maximizing the value you bring over time.
