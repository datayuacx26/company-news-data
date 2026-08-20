---
schema_version: "1.0.0"
document_id: "944a346f369c9a2933eba30a2e3ac8fd798a229cd32cdb3870961978359e5c6d"
company_key: "yc-honeydew"
company: "Honeydew"
source_id: "yc-honeydew-rss-6ec5327dfb7a"
canonical_url: "https://honeydew.ai/blog/why-did-we-build-a-native-app-for-honeydew-semantic-layer-on-snowflake/"
published_at: "2023-11-14T12:18:38+00:00"
first_seen_at: "2026-07-26T14:25:40.247020+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:4ade4a1a5c3de6b0278ab8c2324a907ac78823ec48dc4ba54ff70b67411080b5"
---

# Why did we build a Native App for Honeydew Semantic Layer on Snowflake?

## Introduction


Semantic layers are[awesome](https://honeydew.ai/blog/so-lets-talk-about-semantic-layers/) . But they are even more awesome when they are tightly integrated into your Snowflake, and right at your fingertips in your Snowflake Web Interface or Snowflake application.


Enter … 🥁


The Honeydew[Native App](https://app.snowflake.com/marketplace/listing/GZTSZ14KQ9/honeydew-semantic-layer) .


## Our users demand a semantic layer in their Snowflake Workbooks


A semantic layer serves many purposes: it enables data democratization, provides consistency, ensures governance, and saves countless hours of engineering. With a semantic layer, analytics engineers can enjoy a weekend, and business users can trust a dashboard.


Users access our semantic layer with a to ol of their choice: their Looker, Tableau, Jupyter, or even DBeaver. But, alas, one very important tool was left behind: the[Snowflake Web Interface](https://docs.snowflake.com/en/user-guide/ui-snowsight) , Snowsight.


No one likes switching tabs.


With a Native App, there is no need.


## Applications built on Snowflake want a semantic layer


Snowflake enables[building applications](https://www.snowflake.com/en/data-cloud/workloads/applications/) powered by the Data Cloud. Many of those applications include custom-developed query generators, to bridge between application code and the data warehouse. For an application developer, a semantic layer is a query generator on steroids: it understands data structure and relations and can construct correct fast queries for ad-hoc needs.


But how to integrate it into an application? Right, the Native App.


The Native App encapsulates and secures access to the semantic layer by any means of connection to Snowflake:[JDBC](https://docs.snowflake.com/en/developer-guide/jdbc/jdbc) ,[ODBC](https://docs.snowflake.com/en/developer-guide/odbc/odbc) ,[Node.js](https://docs.snowflake.com/en/developer-guide/node-js/nodejs-driver) ,[PHP](https://docs.snowflake.com/en/developer-guide/php-pdo/php-pdo-driver) ,[Go](https://docs.snowflake.com/en/developer-guide/golang/go-driver) ,[.NET](https://docs.snowflake.com/en/developer-guide/golang/go-driver) , or[Python](https://docs.snowflake.com/en/developer-guide/python-connector/python-connector) . A semantic layer on Snowflake can now be used to generate queries from any application built on Snowflake.


## More security


Snowflake provides user management, access control, encryption, and network separation for every query and connection. Organizations invest in security design ensuring sensitive or private data is always protected.


Now with a Native App, the semantic layer gets the same protection of the Snowflake environment. The access to it is confined to the Snowflake connection, whether it comes from Snowsight or any other Snowflake client.


## What can the users do with the Honeydew Native App?


The Native App provides access to the Honeydew semantic layer:


1. **Business metadata** for governance and cataloging, such as metrics and relationships.
2. **Query generator** for ad-hoc data access based on shared semantics.
3. **Management APIs** for automation and management purposes.


A few examples are below - covering just a small part of its abilities!


### Metadata access to shared semantics


List the available semantic entities in a workspace:


```text
select   name   from   table  (  HONEYDEW_APP  .  API  .LIST_ENTITIES(  'tpch'  ));
```


Explore the relations between different entities, to see how they are connected:


```text
select   source,   target  ,   type  ,   connection   from   table  (  HONEYDEW_APP  .  API  .LIST_RELATIONS(  'tpch'  ))   where   source   =   'orders'  ;
```


Find all metrics that are related to “revenue”:


```text
select   name  , entity,   sql   from   table  (  HONEYDEW_APP  .  API  .LIST_FIELDS(  'tpch'  ))   WHERE   TYPE   =   'Metric'   and   NAME   LIKE   '%revenue%'  ;
```


### Data access powered by shared semantics


One of the advantages of using the Honeydew Semantic Layer on Snowflake is that the users do not have to worry about how to join tables while querying data.


Just combine attributes and metrics, and the Semantic Layer will generate the right query:


```text
select   HONEYDEW_APP  .  API  .GET_SQL(  'tpch'  ,   'select "orders.order_month" as order_month, AGG("customers.count") as customers_count from world.world order by order_month asc'  );
```


Users can also run the generated query directly in Snowflake, and put the results in a transient table. Then that table can be used to build subsequent analysis steps.


```text
call   HONEYDEW_APP  .  API  .RUN_SQL(  'tpch'  ,   'select "orders.order_month" as order_month, AGG("customers.count") as customers_count from world.world order by order_month asc'  ,   'customers_by_month'  );
select   *   from   HONEYDEW_APP  .  QUERY_RESULTS  .customers_by_month   where   year  (order_month)   =   1997  ;
```


#### I only use Tableau/ Looker/ PowerBI - do I need the Native App?


Nope. The Honeydew semantic layer works without it as well,[securely and reliably.](https://honeydew.ai/blog/honeydew-soc-2-announcement/)


However, the native app benefits few scenarios:


1. Using the semantic layer directly from Snowsight, the Snowflake Web Interface.
2. Using the semantic layer from full-stack application code built on Snowflake.
3. Using the semantic layer where security policies are highly custom and conservative, such as in government environments.


## Semantic Layer becomes a native platform capability


This Honeydew Native App integration allows organizations to embed a semantic layer deeper into Snowflake, making it a native platform capability.


Users can access the semantic layer directly within the familiar Snowflake environment, enhancing their productivity and efficiency. The native app helps streamline data security management, ensuring that only authorized users can access and analyze sensitive data. Additionally, Honeydew enables users to create more complex workflows, leveraging the semantic layer as a single source of truth.


## Now what?


You can[install the Honeydew Native App for Snowflake](https://app.snowflake.com/marketplace/listing/GZTSZ14KQ9/honeydew-semantic-layer) .


Or[talk to us to set up a demo](https://honeydew.ai/get-started/) .
