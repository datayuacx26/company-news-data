---
schema_version: "1.0.0"
document_id: "b494b61196d62c17f09550091162d678ac964ba5ccf28721c66fba1bd0249429"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/mysql-transpose-rows-to-columns/"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:3c6409e0aa1a15a7975ae738d8add47ea95ae916044cbabfaca7a4c895cf3977"
---

# MySQL: Transpose Rows to Columns

Transposing rows to columns in MySQL involves reshaping data so that rows become columns, often for improved readability and data analysis. This guide explains how to achieve this using SQL queries, focusing on the use of conditional aggregation and the` CASE` statement.


## Understanding the transpose operation


Transposing data means converting rows into columns, creating a pivot table effect. This operation is useful when you want to compare data across different rows in a more horizontal format.


## Sample dataset


Consider a simple dataset in a table` sales_data` :


```text
CREATE   TABLE   sales_data   (
year   INT  ,
product   VARCHAR  (  50  ),
sales   INT
);
```


## Using CASE and GROUP BY


One common approach to transposing rows to columns in MySQL is using the` CASE` statement with` GROUP BY` . This method works well for known and limited distinct values.


### Transposing specific rows to columns


Here’s how to transpose sales data for different products into separate columns:


```text
SELECT
year  ,
SUM  (  CASE   WHEN   product   =   'Product A'   THEN   sales   ELSE   0   END  )   AS   ProductA_sales,
SUM  (  CASE   WHEN   product   =   'Product B'   THEN   sales   ELSE   0   END  )   AS   ProductB_sales
FROM
sales_data
GROUP BY
year  ;
```


### Dynamic column generation


For dynamic column generation based on unknown or numerous distinct values, a more complex approach involving prepared statements is required.


## Using prepared statements for dynamic transposing


Dynamic transposing is a two-step process: first, dynamically creating a list of columns; second, constructing a query using this list.


### Generating the column list


Extract distinct values to be used as column names:


```text
SET   @sql   =   NULL  ;
SELECT
GROUP_CONCAT(  DISTINCT
CONCAT  (
'SUM(CASE WHEN product = '''  ,
product,
''' THEN sales ELSE 0 END) AS '  ,
CONCAT  (  '`'  ,product,  '_sales`'  )
)
)   INTO   @sql
FROM
sales_data;
```


### Building the dynamic query


Construct and execute a dynamic query with the generated column list:


```text
SET   @sql   =   CONCAT  (  'SELECT year, '  , @sql,   ' FROM sales_data GROUP BY year'  );


PREPARE stmt   FROM   @sql;
EXECUTE   stmt;
DEALLOCATE   PREPARE stmt;
```


If this query pattern is part of recurring reporting,[Basedash](https://www.basedash.com/) helps you turn it into reusable, AI-native BI workflows: prompt-to-SQL, shared dashboards, and trusted answers that stay aligned with your data model.
