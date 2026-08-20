---
schema_version: "1.0.0"
document_id: "f7f4c7f19eb733e15b6d20f2e80aeea003b3f5486ac164a1c191ba35e2689231"
company_key: "yc-kaya"
company: "Kaya"
source_id: "yc-kaya-news-import-23b0e279a9c9"
canonical_url: "https://www.usekaya.com/blog/how-to-plan-for-10x-growth-in-2023"
published_at: "2022-12-14T00:00:00+00:00"
first_seen_at: "2026-07-22T01:21:51.947140+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:907e55d5f94d72c2fe6978921835476c04a0a904882f0eab3b02074e1655565a"
---

# How to Plan for 10x Growth in 2023

With the year winding down, now is an important time to take stock of how your marketing has performed and plot a path toward 10x growth. In this post, we discuss the top 5 things you need to prioritise and how to do it right:


1. Google Analytics UA to GA4 migration - ideally by Jan ‘23
2. Review CAC and growth metrics by acquisition channel
3. Set Growth KPIs for the new year
4. Resource planning: budget & people
5. Reporting infrastructure: Minimum viable marketing analytics stack


## Google Analytics UA to GA4 migration


Google will officially sunset the older version of Google Analytics, aka Universal Analytics (UA), leaving us with Google Analytics 4 (GA4). As of July 1st, UA will stop collecting new data.


Even though the deadline is ~7 months away, we encourage you to get GA4 ready now because:


- GA4 only starts tracking once you’ve completed the installation. Your historical data will not be migrated to GA4.
- Having it ready before Jan ensures you have complete 2023 data for future reporting in GA4.
- Year-end tends to be a quiet time for most businesses. This allows you to make changes when your website has low traffic - easier to test your setup and prevent any mistake from causing any detrimental impact (eg duplicated tracking)


Here are some tips:


- Keep both UA and GA4 running in parallel as UA is arguably more user-friendly and has some metrics that are no longer tracked in GA4.
- Instead of directly sending events from your website to GA4, we recommend installing GTM on your website and sending events from GTM to GA4.
- Without GTM, any change in event tracking will require efforts from your engineering team. This creates bottlenecks for your marketers who often need to set up new events for marketing campaigns.
- Here are the guides to set up[GTM](https://measureschool.com/install-google-tag-manager/) and[GA4](https://www.kristaseiden.com/the-ultimate-guide-for-setting-up-a-google-analytics-4-property/) .


What can you do to preserve your historical UA data if they’ll not be migrated to GA4?


- Export your data to BigQuery ([more info](https://support.google.com/analytics/answer/3416092?hl=en#zippy=%2Cin-this-article) )
- Export static reports from UA


## Review CAC and growth metrics by acquisition channel


Before setting any new growth KPIs for the new year, take some time to evaluate how well your marketing efforts performed in 2022. Here are the main metrics we recommend including in your evaluation:


- Customer Acquisition Cost - This is a great article by Andrew Chen on


[how to calculate CAC the right way](https://andrewchen.com/how-to-actually-calculate-cac/) .
- Cost per Conversion - Conversion here refers to some entity that is not a paying customer (yet). In other words, it’s a leading indicator for CAC eg Lead, Webinar Registration, etc.
- Conversion rate at each major step of your funnel - These metrics should help you identify which part of the funnel is leaky.
- (If you are running performance marketing campaigns) Return on ads spend, Cost per Click, Cost per Mille Impressions, Click Through Rate


For each metric, you should break them down by channel and document any outliers, what has worked well and didn’t. Your marketing logbook should contain:


- Start and end dates of the incident
- What metrics are impacted and by how much
- Point of contact
- Links or screenshots


## Set Growth KPIs for the new year


Once you have a good understanding of your current performance and identify where the bottleneck is, you can use this information to set new growth KPIs for the new year. To achieve these goals, you will need to develop a plan of action that outlines the steps you will take to reach them. This plan should include tactics such as creating more engaging content, improving your website design, and running more targeted ads.


For example, if you find that your website conversion rate is much lower compared to the industry benchmark while other metrics are on par, your first goal could be to improve your conversion rate by X%. This could be achieved by:


- Improving website quality (eg by improving copywriting, UI UX design) and/or
- Traffic quality (eg by reviewing your main acquisition channels and evaluating if they’re attracting the right customer profile).


## Resource planning: Budget & People


First, let’s talk about budget planning. Each business has a preferred approach and we'll highlight the most 2 common ones:


- Top-down: Start by allocating a percentage of your revenue (or profit)
- Bottom-up: Start by calculating your CAC and multiply it by the number of customers you wish to acquire


In any case, we recommend performing scenario planning down to the channel level to make sure your plan and KPIs are financially viable and realistic. The screenshot below shows an example showing how the metrics from the previous step are used in our planning.


Example budget calculator we use with our users


Don't forget to:


- Include all the expenses such as agency fees, software, assets, and content creation fee
- Set aside budget for experimentation. You can start with 15-20% of your monthly budget.


This


[guide from Brian Balfour](https://brianbalfour.com/essays/5-steps-to-choose-your-customer-acquisition-channel)


provides a good framework to determine if there’s any channel we should start or stop pursuing.


Next, you may consider hiring freelancers or marketing agencies to execute your campaigns. Here’s a


[checklist you can use to vet agencies](https://docs.google.com/spreadsheets/d/18gEPpa_Absawl4N48dQY4iWo4G9kmB1ZaCsBVhbBbao/edit#gid=0) .


## FAQ


### How do I choose a growth marketing agency?


Consider technical expertise, industry knowledge, past records, adaptability, and willingness to collaborate. Look for agencies experienced in your industry and have a solid understanding of your startup dynamics.


### How does AI promote business growth?


AI offers smart insights that founders rely on to make decisions. It automates marketing processes, which increases the speed of experiments, iterations, and product releases. AI also enables brands to optimize and tailor content to the specific needs of their target audience.


### How to create a growth marketing strategy?


To create a growth marketing strategy, outline your vision and identify growth marketing metrics. Also, map out the customer journey and decide on a focus area based on the data. Finally, outline potential tests and experiments, and create an action plan.


## Reporting infrastructure: Minimum viable marketing analytics stack


Tracking CAC and growth metrics shouldn’t feel like a chore. Most startups feel this way because they don’t have a good marketing analytics setup in place.


We recommend setting up a minimum viable marketing analytics stack that will automatically (and reliably) extract data from various data sources and load them to a reporting tool or spreadsheet where you can calculate and visualise your growth metrics in (near) real-time.


### **1. Data sources**


- A web analytics platform: Google Analytics (free) / Mixpanel / Posthog / Amplitude / Heap
You should make sure


[UTM tagging is done correctly](https://support.google.com/analytics/answer/1033863?hl=en#zippy=%2Cin-this-article)


at every customer touch points so you measure channel or even ads-level performance accurately.
- Acquisition channels: Google Ads, Facebook Ads, etc
- CRM / Sales app: Hubspot, Shopify, etc


### **2. ETL tools to transfer data from source to destination**


- No-code ETL: Airbyte (open source), Fivetran
- Marketing specific: Supermetrics, Funnel IO


### **3. Destination**


- Visualisation tool: Google Data Studio, Metabase (open source), PowerBI
- Good ol’ spreadsheet


## How can Kaya help?


Think of Kaya as your extended growth team, we exist to help you tackle these pesky marketing tasks:


- We launch and manage your marketing campaigns cheaper and faster than traditional agencies.
- Track CAC, ROI and other growth metrics without any manual work. All you need to do is connect your data sources.
- You don’t need to worry about losing historical UA data - we collate data from UA and GA4 and present them in standardised reports.
