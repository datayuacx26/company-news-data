---
schema_version: "1.0.0"
document_id: "42d8317dfbb108702fdd8a04e401db3571d0e59c1c3b519a518181ecfed849fb"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/overview-mysql-last-30-days/"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:a2b5ca9c16652f0ddc6718bc3acde947c6d78e9baa3cb81ebec8f410c925dab4"
---

# Overview: MySQL Last 30 Days

MySQL, a widely-used open-source relational database management system, often requires querying data based on time intervals. This guide focuses on extracting data from the last 30 days, a common requirement for reports and data analysis.


## Understanding Date and Time Functions in MySQL


MySQL provides several functions to work with date and time values. The` CURDATE()` and` NOW()` functions are particularly useful for current date and time operations. To query data from the last 30 days, these functions can be combined with the` DATE_SUB()` function.


## Querying Data from the Last 30 Days


To fetch records from the last 30 days, use the` DATE_SUB()` function in your` WHERE` clause. This function subtracts a specified time interval from a date.


```text
SELECT   *
FROM   your_table
WHERE   your_date_column   >=   DATE_SUB(CURDATE(), INTERVAL   30   DAY  );
```


In this query,` your_table` should be replaced with your actual table name and` your_date_column` with the column that contains the date information.


## Handling Time with` NOW()` and` CURDATE()`


There’s a subtle difference between` NOW()` and` CURDATE()` in MySQL:


- ` NOW()` returns the current date and time.
- ` CURDATE()` returns only the current date.


Depending on your data’s nature and your requirements, choose the one that suits your needs. For precise time calculations,` NOW()` is preferable.


```text
SELECT   *
FROM   your_table
WHERE   your_date_column   >=   DATE_SUB(  NOW  (), INTERVAL   30   DAY  );
```


## Accounting for Time Zones


When dealing with multiple time zones, consider converting your dates to UTC or using the` CONVERT_TZ()` function for accurate results.


```text
SELECT   *
FROM   your_table
WHERE   CONVERT_TZ(your_date_column,   'System'  ,   'UTC'  )   >=   DATE_SUB(UTC_DATE(), INTERVAL   30   DAY  );
```


## Using` BETWEEN` for Range Queries


The` BETWEEN` operator can also be used for querying data within a specific range. This is particularly useful when you need data between two dates.


```text
SELECT   *
FROM   your_table
WHERE   your_date_column   BETWEEN   DATE_SUB(CURDATE(), INTERVAL   30   DAY  )   AND   CURDATE();
```


## Tips for Performance Optimization


- Ensure that the date column used in the` WHERE` clause is indexed.
- For large datasets, consider narrowing down the query with additional conditions to reduce the result set.


[Basedash](https://www.basedash.com/) is built as an AI-native BI platform, so teams can go from ad hoc SQL to trusted answers and dashboards quickly, without the overhead of traditional BI setup.


This guide provides the essentials for querying MySQL data from the last 30 days. By understanding and utilizing MySQL’s date and time functions effectively, you can extract meaningful insights from your data in a time-efficient manner.
