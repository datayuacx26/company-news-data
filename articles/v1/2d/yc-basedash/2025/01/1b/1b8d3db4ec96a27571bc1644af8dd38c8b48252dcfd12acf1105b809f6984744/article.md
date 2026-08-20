---
schema_version: "1.0.0"
document_id: "1b8d3db4ec96a27571bc1644af8dd38c8b48252dcfd12acf1105b809f6984744"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-do-an-unpivot-in-mysql/"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:ba03149fe1e8e60f3d379ce6fa47aee4002e042f605b88e78995202244bd58c1"
---

# How to Do an Unpivot in MySQL

Unpivoting in MySQL involves transforming columns into rows, effectively normalizing data that was previously denormalized. This guide demonstrates how to perform an unpivot operation in MySQL, converting columnar data into a more query-friendly row format.


## Understanding the unpivot operation


In MySQL, unpivoting is not a direct, single-command process as it is in some other SQL databases. Instead, it requires a combination of SQL operations to achieve the same result. The goal is to transform data from a wide format (with many columns) to a long format (with more rows and fewer columns).


## Preparing your data


Consider a table named` sales_data` with the following structure:


```text
| product_id | Jan_sales | Feb_sales | Mar_sales |
|  ------------|-----------|-----------|-----------|
|   1            |   150         |   200         |   250         |
|   2            |   300         |   350         |   400         |
```


Our aim is to unpivot the monthly sales columns into a format with two columns:` month` and` sales` .


## Creating the unpivot query


The following query uses a combination of` UNION ALL` and` SELECT` statements to unpivot the data:


```text
SELECT   product_id,   'Jan'   as   month  , Jan_sales   as   sales   FROM   sales_data
UNION ALL
SELECT   product_id,   'Feb'  , Feb_sales   FROM   sales_data
UNION ALL
SELECT   product_id,   'Mar'  , Mar_sales   FROM   sales_data;
```


This query produces an output like:


```text
| product_id |   month   | sales |
|  ------------|-------|-------|
|   1            | Jan   |   150     |
|   1            | Feb   |   200     |
|   1            | Mar   |   250     |
|   2            | Jan   |   300     |
|   2            | Feb   |   350     |
|   2            | Mar   |   400     |
```


## Handling large numbers of columns


If your table has a large number of columns, manually writing a` UNION` for each one can be impractical. In such cases, consider automating this process with a dynamic SQL query or using an external tool to generate the query for you.


## Enhancing query efficiency


- Index relevant columns to speed up the query, especially if the original table is large.
- Use` UNION ALL` instead of` UNION` to avoid the overhead of removing duplicate rows, assuming your data does not have duplicates.


If this query pattern is part of recurring reporting,[Basedash](https://www.basedash.com/) helps you turn it into reusable, AI-native BI workflows: prompt-to-SQL, shared dashboards, and trusted answers that stay aligned with your data model.
