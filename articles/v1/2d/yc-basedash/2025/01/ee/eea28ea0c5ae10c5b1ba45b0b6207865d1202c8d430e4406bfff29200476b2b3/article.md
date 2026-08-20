---
schema_version: "1.0.0"
document_id: "eea28ea0c5ae10c5b1ba45b0b6207865d1202c8d430e4406bfff29200476b2b3"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/pivot-tables-in-mysql/"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:03ce6ebf0b4b24c358091108a0dfe5b617b6a4f7bc049188182ec08f86000f59"
---

# Pivot Tables in MySQL

A pivot table in MySQL will transform rows into columns. It’s a solid way to generate reports. This post dives into how it works so you can display cleaner data.


## How to pivot a table in MySQL?


You can pivot in MySQL by using the` CASE` or` IF` statements within aggregation functions like` SUM()` or` AVG()` . This process turns unique values from a column into multiple columns in the output, effectively aggregating the data based on your conditions.


### Example


Consider a sales table with` product_id` ,` month` , and` sales_amount` columns. To create a report that shows each product’s total sales for each month on a single row, you would use the following query:


```text
SELECT
product_id,
SUM  (  CASE   WHEN   month   =   'January'   THEN   sales_amount   ELSE   0   END  )   AS   January,
SUM  (  CASE   WHEN   month   =   'February'   THEN   sales_amount   ELSE   0   END  )   AS   February,
SUM  (  CASE   WHEN   month   =   'March'   THEN   sales_amount   ELSE   0   END  )   AS   March
FROM   sales
GROUP BY   product_id;
```


This query assigns the sales to the correct month by checking each row. It sums the` sales_amount` for that month, ensuring rows without sales in a month display 0 through the` ELSE 0` part of the` CASE` statement.


## Dynamic pivot tables


To create a pivot table that automatically adjusts to new data, such as additional months or categories, without manual updates, you’ll need to use dynamic SQL. This method involves:


1. Identifying unique values for column headers.
2. Building a SQL string with these values as conditional statements.
3. Executing the constructed SQL query.


Here’s how to pivot dynamically using prepared statements in MySQL:


```text
SET   @sql   =   NULL  ;
SELECT   GROUP_CONCAT(  DISTINCT
CONCAT  (
'SUM(CASE WHEN month = '''  ,
month  ,
''' THEN sales_amount ELSE 0 END) AS '  ,
CONCAT  (  '`'  ,   month  ,   '`'  )
)
)   INTO   @sql
FROM   sales;


SET   @sql   =   CONCAT  (  'SELECT product_id, '  , @sql,   ' FROM sales GROUP BY product_id'  );


PREPARE stmt   FROM   @sql;
EXECUTE   stmt;
DEALLOCATE   PREPARE stmt;
```


This approach dynamically generates a SQL query by collating all unique months from the` sales` table into a conditional sum for each, then executing the SQL.
