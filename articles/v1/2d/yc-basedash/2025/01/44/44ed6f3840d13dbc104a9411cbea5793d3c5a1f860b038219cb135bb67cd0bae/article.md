---
schema_version: "1.0.0"
document_id: "44ed6f3840d13dbc104a9411cbea5793d3c5a1f860b038219cb135bb67cd0bae"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-create-a-materialized-view-in-mysql/"
published_at: "2025-01-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:02cb1f07d82ffd875c2272dbdfdcddfb2d78b7bfaaec5f9472592ac2810fe013"
---

# How to Create a Materialized View in MySQL

A materialized view is a database object that contains the results of a query. It’s similar to a regular view, except that the data is physically stored. This is good because it makes it faster to retrieve data, which helps for stuff like complex queries. This guide walks you through how to create one in MySQL.


## What is a materialized view?


Full disclosure: MySQL doesn’t natively support materialized views. However, you can simulate one using a combination of a regular view and a physical table. You have to create a table to store the query result and then periodically refresh that table to keep the data up to date.


## Create a base table and view


### Step 1: Create a base table


Start by creating a base table that will store the materialized data. This table’s structure should match the result of the query you intend to materialize.


```text
CREATE   TABLE   materialized_view_table   (
-- Define columns here
);
```


### Step 2: Create a view


Next, create a view that defines the query you want to materialize.


```text
CREATE   VIEW   my_view   AS
SELECT
-- Define your query here
;
```


## Populate the materialized view


### Refresh the materialized view


Since MySQL doesn’t automatically refresh materialized views, you need to implement a mechanism to periodically update the base table with the latest data.


```text
TRUNCATE   TABLE   materialized_view_table;


INSERT INTO   materialized_view_table
SELECT   *   FROM   my_view;
```


You can automate this process using events or triggers.


## Query the materialized view


To retrieve data, simply query the base table.


```text
SELECT   *   FROM   materialized_view_table;
```


The advantage of this approach is that it’s faster to retrieve data than if you just queried the view directly.


## Managing updates


To keep the materialized view up-to-date, consider implementing a scheduled job that runs the refresh query at regular intervals. You can do this with MySQL events or external schedulers.


---


[Basedash](https://www.basedash.com/) is built as an AI-native BI platform, so teams can go from ad hoc SQL to trusted answers and dashboards quickly, without the overhead of traditional BI setup.
