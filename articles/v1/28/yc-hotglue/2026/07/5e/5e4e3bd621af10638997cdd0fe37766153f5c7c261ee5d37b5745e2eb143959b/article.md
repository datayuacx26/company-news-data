---
schema_version: "1.0.0"
document_id: "5e4e3bd621af10638997cdd0fe37766153f5c7c261ee5d37b5745e2eb143959b"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/how-to-lower-your-coreplus-quickbooks-api-usage"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:376553852a444bbbcb6b98efd235cb405cbf90ec2079f3e2e0877d7d08ae3cfa"
---

# QuickBooks New API Fees Explained + How to Monitor & Minimize Usage

Starting July 28, 2025, QuickBooks is launching its new QuickBooks App Partner Program, which introduces **paid tiers** and **API fees.** Without careful monitoring, this new pricing model can get expensive fast.


This article goes over:


- how the new pricing works
- how you can monitor your usage in hotglue
- what you can do to minimize your API fees


# How the new App Partner Program pricing works


Not every request falls under the new pricing regime. QuickBooks calls the affected requests “CorePlus” requests—essentially, requests that are intended to pull data out of QuickBooks. This includes requests for:


- reports
- query operations
- batch requests
- regular REST resource GETs.


In hotglue, your CorePlus requests are mostly query operations and report queries.


Conversely, other requests are NOT metered—they are “free” to make at will:


- All token and authorization-related requests
- PUTs, PATCHes, and POSTs that exclusively push data into QuickBooks
- Any request that returns a non-success response, such as 429 rate limit


In the simplest terms, you can frame your monthly requests around two numbers:


1. Under **500,000 requests per month** , you are eligible for the free “Builder tier.” If you don’t need to be listed on the app store and don’t care about partner marketing, this allows you to make production requests without paying any fees.Up to
2. **1,000,000 requests per month** are included in the $300/month “Silver tier.” This gives you access to the app store and other partner resources, and as long as you stay within this range, you don’t need to worry about variable monthly costs. Past 1M, you stay in the Silver tier, but pay an overage fee per request ($0.0035/req)


There are additional tiers when you hit more than 500 connected accounts or more than 10 million requests—you can review Intuit’s full fee schedule[here](https://developer.intuit.com/partners/benefits) .


# How to monitor your usage in hotglue


From within hotglue, you can review your QuickBooks API usage by navigating to the[QuickBooks API usage dashboard](https://hotglue.com/app/api-usage-dashboard) . The dashboard is designed to help you visualize your current usage and identify opportunities to reduce usage.


💡hotglue offers an unofficial dashboard of the daily CorePlus requests that you run in hotglue, extrapolated out to total monthly costs. You can review the official tally from Intuit by going to your[QuickBooks Developer Dashboard](https://developer.intuit.com/workspaces) , then selecting the app that you are using in production, and going to the Analytics tab.


## Projected Requests + Costs


The first thing you’ll see on the dashboard is an estimate of your monthly requests and costs. This is extrapolated from the requests your past hotglue jobs have made.


Don’t be alarmed if you see a high monthly cost / request count! We will review strategies to optimize usage below. In many cases, simple tweaks can get you back down to the Silver tier ($300/mo) with limited overage costs.


## Daily API Usage


The first chart in the dashboard simply shows your daily API usage over time. We use these numbers to build an estimated monthly cost.


## Usage per Stream


The Usage per Stream section breaks down the number of requests that were made to sync each stream. In the example below, we can quickly see the` GeneralLedgerReport` streams account for most of the usage.


This can be immensely helpful for figuring out where to focus optimization efforts.


## Usage per Tenant


The last section breaks down the usage by tenant and attempts to estimate their projected impact on costs. In this example,` blue_walrus_labs` accounts for slightly more of the API requests compared to other tenants. This could indicate they have more data being generated on the QuickBooks side, or that their jobs are scheduled on a more frequent basis.


# How to optimize your QuickBooks API usage


## Ensure your read jobs are running incrementally


This is a must, particularly if you are projected to go over the` 1,000,000` request breakpoint. The easiest way to check whether you are running full syncs is to look at a random job’s log, and check the` SYNC TYPE` . If it shows` full_sync` , you have probably configured your environment to run full syncs in **Settings > Jobs > Historical Data.**


## Setting smarter schedules


Another easy way to make less requests is to decrease the frequency on which you run jobs. By running fewer, more spread out jobs, the hotglue connector’s incremental sync logic will be able to bundle more data together in single requests, thereby reducing your overall request count.


Check out relevant docs here:


- [https://docs.hotglue.com/api-reference/schedule-jobs/create-jobs-schedule](https://docs.hotglue.com/api-reference/schedule-jobs/create-jobs-schedule)
- [https://docs.hotglue.com/api-reference/manage-tenants/toggle-schedule](https://docs.hotglue.com/api-reference/manage-tenants/toggle-schedule)


## Syncing reports


The biggest offenders of QuickBooks’ limits are reports. While most QuickBooks resources are incremental by default, reports have no primary keys and usually need to be fetched in multiple batches.


In order to write this data to a database / data warehouse target without primary keys, we recommend setting the` replication_method` to` truncate` . This allows hotglue to send the full report and write it to your database without creating any duplicated data.


There are two ways to accomplish this with hotglue:


### Using full syncs (not recommended)


The simplest approach for working with reports is to set` "reports_full_sync": true` in the config. This will make the QuickBooks connector pull the entire report every single time a job runs.


As you can imagine, this can result in a large number of API calls. If you’re currently using this approach and rely heavily on report data, we highly recommend you switch to using incremental syncs.


### Using incremental syncs


Alternatively, the hotglue QuickBooks connector implements a “lookback” period which defaults to syncing the last three periods for a given report. This is much easier on your request limits. However, in order to sync this data to a database with` truncate` , you’ll need to leverage hotglue’s snapshot functionality.


Our team has put together a sample transformation script that demonstrates how you can accomplish this.[Check it out on GitHub](https://github.com/hotgluexyz/recipes/blob/master/src/qbo-optimization/qbo-report-snaps.ipynb) or preview below:


## Maintaining “fullsync-like” outputs for non-Report streams


If you write your data to a file-based destination in lieu of a database, like S3, you may prefer to have full datasets written to your file store after every job. You can replicate this behavior, while running incremental syncs, by utilizing hotglue’s built-in snapshot layer.


Here’s an example using the gluestick` snapshot_records` function, which upserts incremental` invoices` data, and assigns the full history of invoices, alongside the new invoices, to a dataframe named` all_invoices` :


` import


gluestick


as


gs


input


=


gs


.


Reader


(


)


#


Get


the incremental


Invoices


data


from


sync


-


output


inc_invoices


=


input


.


get


(


"Invoices"


)


#


Update


the snapshot


+


get


the full list


of


Invoices


all_invoices


=


gs


.


snapshot_records


(


inc_invoices


,


"Invoices"


,


SNAPSHOT_DIR


,


pk


=


"Id"


)


`


# Conclusion


The good news is you still have time to make changes before QuickBooks starts charging for API usage. Although the new program will go into effect on July 28th, actual billing for overages will not begin until **Nov 1, 2025.**


Making the tweaks above, primarily around setting smart schedules and using incremental syncs can make a huge difference in API usage. Plus, with hotglue’s API usage dashboard you will be able to quickly see if your changes are bringing your projected bill down.


hotglue is here to help! If you have questions on the upcoming QuickBooks API fees or how to reduce your usage, don’t hesitate to reach out via Slack oremail .


TABLE OF CONTENTS


- How the new App Partner Program pricing works
- How to monitor your usage in hotglue
- Projected Requests + Costs
- Daily API Usage
- Usage per Stream
- Usage per Tenant
- How to optimize your QuickBooks API usage
- Ensure your read jobs are running incrementally
- Setting smarter schedules
- Syncing reports
- Maintaining “fullsync-like” outputs for non-Report streams
- Conclusion


RECOMMENDED BLOGS


[hotglue melt: May 2025 Universal external id support, connector builder updates, subtenant symlinks, and more!](https://hotglue.com/blog/hotglue-melt-may-2025)


[hotglue melt: April 2025 Connector Builder, real-time write, streaming read jobs, accounting unified schema v2, and more!](https://hotglue.com/blog/hotglue-melt-april-2025)


[hotglue melt: March 2025 10x faster discovers for CRM connectors, dark mode, and more!](https://hotglue.com/blog/hotglue-melt-march-2025)
