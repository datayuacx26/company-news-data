---
schema_version: "1.0.0"
document_id: "d43c342c45997e72873bdf3f62c44009dd86799a54a1a01aa6e16fe290549284"
company_key: "yc-factiq"
company: "FactIQ"
source_id: "yc-factiq-news-import-bb59a353b5d0"
canonical_url: "https://factiq.com/blog/the-receipts-are-in-software-productivity-outpaced-other-sectors-since-chatgpt-launch"
published_at: "2026-01-26T16:01:04.766+00:00"
first_seen_at: "2026-07-21T19:29:26.784531+00:00"
fetched_at: "2026-07-28T22:23:07.215004+00:00"
content_hash: "sha256:ce2ed89d24b6c9290e380148a96b9c76ea9b9ed04ba40e9882b12056ba206c95"
---

# The Receipts are In: Software Productivity Outpaced Other Sectors Since ChatGPT Launch

### TL;DR


-


Software Publishers' revenue per employee rose 28% since ChatGPT's public launch - significantly higher than in other industries


-


Headcount among software publishers remained flat in this period


-


Official government data likely undercounts Software Publisher labor productivity due to measurement technicalities.


###


BLS and Census data1 show that productivity in software significantly outpaced that in traditional sectors like manufacturing and accounting since Q1 2023.


In this post, we use *revenue per employee* as a simple proxy for output per worker in software publishing. It’s not the same as BLS “labor productivity”, but is a useful lens for whether the sector is generating more dollars with the same headcount.


###


## Revenue Per Employee in Software Publishers increased faster than other industries since ChatGPT launched


Percentage change in 4-quarter rolling average revenue per employee (Q1 2023 to Q3 2025)


Bureau of Labor Statistics, Census Bureau


Updated 26 Jan 2026


After six years of stagnation, revenue per employee surged 28% after the launch of ChatGPT - while headcount remained entirely flat.


## Revenue per employee (rolling 4q average) in the Software Publishers industry rose sharply after Q1 2023, after remaining flat for years


Quarterly revenue divided by average quarterly employment (Nominal USD)


Data Source:


Census Bureau · Bureau of Labor Statistics


How we built this


· loading


Updated 26 Jan 2026


Rolling Average (4 quarters)


## Employment in software publishing has stayed flat during the post-AI productivity boom, while nearly doubling from 2016 to 2023


Seasonally adjusted monthly data


Data Source:


Bureau of Labor Statistics


How we built this


· loading


Updated 26 Jan 2026


### Why this doesn't show up in official data


The official BLS data for measuring labor productivity shows that growth in software publishing productivity was very low in 2023, and was higher than broader economy (though still below the numbers we calculated) in 2024.


## BLS Labor Productivity data shows software publisher productivity growth below the general economy in recent years


Year-over-year percentage change in labor productivity


Data Source:


Bureau of Labor Statistics


How we built this


· loading


Updated 26 Jan 2026


If these gains are so obvious in the revenue data, why aren't they showing up in the government's official productivity reports? The answer lies in a technicality of how we measure Producer Price Inflation.


The lack of improved productivity in 2023 and 2024 is largely because of intricacies in the way BLS calculates productivity. The BLS uses a "Producer Price Index" for software to "deflate" price increases from labor productivity. This approach - while useful for things like commodity goods - doesn't quite make sense for software (where increased labor productivity might lead to better quality software - for which a producer can charge higher prices).


For instance, BLS determined that software prices spiked between 2023 and 2024 - and then discounted the increase in revenue per employee by this factor. Their implicit reasoning was "revenue increased because the publisher increased their prices, not because labor was more productive".


## As measured by BLS' Producer Price Index, software prices faced a high "inflation" in 2023 and 2024, after being flat for years


Year-over-year percentage change


Data Source:


Bureau of Labor Statistics


How we built this


· loading


Updated 27 Jan 2026


Deflating by software PPI is arguably the wrong metric for software, where productivity gains manifest as improved quality rather than lower prices. If a firm’s revenue rises 20% but the price index says ‘software prices’ rose 15%, BLS counts only ~5% real output growth. If that price index is really capturing quality improvements, the deflation step understates productivity.


### **Does this mean job losses for Software Developers in the future?**


It depends. If software developers are 30% more productive, does that mean a company needs 30% fewer people, or can they now build 30% more products that were previously too expensive to develop?


Agriculture is a classic example of an industry where increase in labor productivity meant far fewer jobs (in the long-term) in that industry. Demand for agricultural products is limited by population size. Beyond a certain point, you just don't need to produce more and more agricultural products. So fewer humans are needed to cater to the limited/finite demand for such products.


But if demand for software increases substantially as a result of cheaper and easier to maintain software - demand for human builders that can gather user requirements and iterate (with AI and other humans) to build a product might actually increase. At least as long as they have better taste and judgement about user preferences than AI.


1.


Quarterly revenue numbers for specific NAICS industries were obtained from the Census Bureau. Monthly employment numbers for these industries were obtained from BLS, and then averaged to get quarterly estimates. Revenue was then divided by total employment to get revenue per employee.
