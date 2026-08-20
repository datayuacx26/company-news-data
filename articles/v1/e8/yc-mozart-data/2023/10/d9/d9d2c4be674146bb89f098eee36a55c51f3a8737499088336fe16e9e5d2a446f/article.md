---
schema_version: "1.0.0"
document_id: "d9d2c4be674146bb89f098eee36a55c51f3a8737499088336fe16e9e5d2a446f"
company_key: "yc-mozart-data"
company: "Mozart Data"
source_id: "yc-mozart-data-rss-80d5f072b9ee"
canonical_url: "https://mozartdata.com/mozart-data-braze-customer-engagement/"
published_at: "2023-10-03T22:54:26+00:00"
first_seen_at: "2026-07-24T11:43:21.667443+00:00"
fetched_at: "2026-08-20T02:02:30.556401+00:00"
content_hash: "sha256:6f9cdf914ac71375b277c6aabb548647f80141ef7f530826576d598cf36d2b33"
---

# How We Used Mozart Data and Braze to Boost Customer Engagement at Tempo

*Doo Shim is the Product Manager at Mozart Data. In his previous role, he worked on the Data team at Tempo, where he leveraged Mozart Data and Braze for a data-driven approach to increasing customer engagement.*


## Problem


**Are you struggling to structure your outreach campaigns to target the right customers at the right time with the right messaging?**


Companies collect various types of data about their customers from different SaaS applications and product engagement. This information is valuable in helping brands customize messages and engagement activities — but all too often, data is siloed within applications or databases and not fully utilized to its potential. And as a result, lifecycle marketing campaigns are an organized mess created by patches of data, which can lead to poor segmentation of customers, delayed campaigns, and campaigns being sent to incorrect customers. Thankfully, I discovered a solution through the combined power of Mozart and Braze.


## So, let’s begin: My Journey.


When I led the Data team at[Tempo](https://tempo.fit/) , an AI-powered home fitness technology company, we faced similar challenges. While various data was being collected, we didn’t have the solution to organize it to create useful customer segmentation. We found ourselves dealing with a very manual process of creating customer segmentation and outreach logic, resulting in lost sales and dormant users with no timely messages to re-engage them and keep them working out. This negatively impacted our long-term engagement and revenue. **That’s when we discovered we could combine Mozart Data and Braze as a scalable solution to segment and target customers with highly customized messaging based on their workout goals and habits.**


## How did we fix this?


### **Step 1: Define the goals and metrics to impact.**


The first step we took was defining the metrics we wanted to measure and impact, and clarify the goal of the outreach campaigns. For example, as a consumer fitness product, we wanted to increase the average number of workouts that a user completes each week. With the highly-customized engagement campaigns, we wanted to influence user behaviors and move them from the Dormant category to more engaged user categories.


### **Step 2: Segment users.**


Next, using the centralized and integrated data from multiple SaaS tools and source systems, we were able to segment the customers based on various factors. Fortunately, we used Mozart Data to centralize a broad range of data, such as user demographic information, fitness goals, fitness experience, product usage frequency, and purchasing behaviors. This helped us create a user-level transform table that segmented users into three broad categories: Dormant (least engaged), At-Risk, and Engaged (most engaged) based on their data, and further segmented them by their fitness habits.


### **Step 3:** **Send personalized messages to user segments.**


With the new user segmentation, the Lifecycle Marketing team created highly customized outreach campaigns in Braze to drive user engagement and retention. We connected Braze to our Snowflake data warehouse managed by Mozart and sent customized messages based on segment. For example, users might receive recommendations for new workout classes or coaches based on their previous engagement data or equipment purchases.


### **Step 4: Experiment, Measure, Iterate. Repeat.**


At this point users were receiving interesting engagement emails or push notifications on their app. But it didn’t stop there. We wanted to understand whether the new messaging experiences helped drive user engagement, so we identified the control and experimental groups to test the outreach campaigns. Additionally, we set up reporting around our experiments to monitor the impact before deploying the campaigns to the larger group.


## Outcomes


Through this process, we were able to send personalized engagement messages through Braze, including workout reminders to users who were at risk of lapsing. **The result?** Increased overall user engagement and a better customer experience. Additionally, we were able to continue optimizing our user engagement strategies over time. So, if you’re facing similar challenges with organizing your customer data and creating effective outreach campaigns, don’t hesitate to take the first step and define your metrics and goals. With the right tools and best practices, you can create highly targeted campaigns that engage your customers and drive long-term revenue.
