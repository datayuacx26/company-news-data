---
schema_version: "1.0.0"
document_id: "3bb9d4de9f74e6a72f5a3354dc58b50473cff8109b870b0b6a615fa6ebc6f660"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-group-by-hour-in-mysql/"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:de11112ad94f2af705c21855cdf1f64da83dc474eb5cfec22b9d3dbccf6ccd65"
---

# How to Group By Hour in MySQL

Grouping data by hour in MySQL involves extracting the hour part from a datetime or timestamp column and using it in a` GROUP BY` clause. This technique is commonly used in time series data analysis, allowing you to aggregate data based on hourly intervals.


## Understanding the HOUR function


MySQL’s` HOUR()` function extracts the hour from a time value. It’s crucial in grouping data by hour. Here’s a basic syntax:


```text
SELECT   HOUR  (time_column)   AS   hour
FROM   your_table;
```


## Simple hourly grouping


To group records by hour, use the` HOUR()` function within the` GROUP BY` clause. This approach aggregates data for each hour of the day (0-23).


```text
SELECT   HOUR  (datetime_column)   AS   hour  ,   COUNT  (  *  )
FROM   your_table
GROUP BY   HOUR  (datetime_column);
```


## Grouping by date and hour


When dealing with timestamps, you often need to group by both the date and the hour to avoid mixing data from different days.


```text
SELECT   DATE  (datetime_column)   AS   date  ,   HOUR  (datetime_column)   AS   hour  ,   COUNT  (  *  )
FROM   your_table
GROUP BY   DATE  (datetime_column),   HOUR  (datetime_column);
```


## Handling time zones


If your data involves different time zones, convert the datetime to a consistent time zone before grouping.


```text
SELECT   CONVERT_TZ(datetime_column,   '+00:00'  , your_timezone)   AS   converted_time
FROM   your_table;
```


Then, use` converted_time` for grouping.


## Aggregating results


Combine the` GROUP BY` clause with aggregate functions like` SUM()` ,` AVG()` , or` COUNT()` to summarize data.


```text
SELECT   HOUR  (datetime_column)   AS   hour  ,   SUM  (value_column)
FROM   your_table
GROUP BY   HOUR  (datetime_column);
```


## Advanced usage with date ranges


For more complex scenarios, such as filtering within specific date ranges, combine` WHERE` with` GROUP BY` .


```text
SELECT   HOUR  (datetime_column)   AS   hour  ,   COUNT  (  *  )
FROM   your_table
WHERE   datetime_column   BETWEEN   '2023-01-01'   AND   '2023-01-31'
GROUP BY   HOUR  (datetime_column);
```


## Visualizing results


If this query pattern is part of recurring reporting,[Basedash](https://www.basedash.com/) helps you turn it into reusable, AI-native BI workflows: prompt-to-SQL, shared dashboards, and trusted answers that stay aligned with your data model.
