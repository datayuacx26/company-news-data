---
schema_version: "1.0.0"
document_id: "324d41f4246bc48c78ede03368b5de30c6d9a0bbfcc5168e722ed8c66e70a737"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/how-to-share-data-with-snowflake-data-sharing/"
published_at: "2023-01-31T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:90fe48d53dddc9c6b5bd7a963a924ce9f7a86772f22ccd527b0c57dec7f42700"
---

# How to Share Data with Snowflake Data Sharing (with examples)

Snowflake is a popular cloud data warehouse, and if you’re in the business of generating or working with large amounts of data, it provides a powerful storage and computing engine for handling this data.


If your company sells to (or partners with) other businesses, you’ve likely encountered the request from customers to transfer data (or “share data”) directly to that customer to enable analytics or reporting in their own infrastructure. If the data already resides within your Snowflake — either because you copy it there, or use Snowflake as your primary data warehouse — you can leverage the built-in sharing features to share with other Snowflake accounts.


This walkthrough provides an overview of the steps required to configure Snowflake Data Sharing, and the options available to extend this sharing to all other regions and warehouses using Prequel.


## Defining the Data Model


Arguably the most time intensive step, defining the data model (i.e., determining the tables and columns to share with customers) involves understanding what data your customers might need and finding the balance between the ease and flexibility of analysis.


If you’re planning to turn this into a scaled product, it’s worth being very intentional at this stage. After all, you can think of this schema as a new API with a lot of the same inherent promises to the customer around consistency and reliability.


A good starting point is to look at the data model exposed by your public API (if you have one) or to evaluate whether the existing data model from the source is already in a good state. Like all products, this will likely require iteration, and keeping the surface area small and/or starting with a small cohort of beta customers can be a good way to start.


Once you’ve aligned on the data model you want to expose to the customer, you’ll need to prepare the data to be shared.


An optional step at this point is to think about relabeling or adding metadata (e.g., comments) to the tables/columns to guide the analysts on the other end. Many companies even pre-write some views (SQL queries) to help customers get started with some of the more predictable or popular analyses.


## Preparing the Data and Filtering by Customer


To share the right data with your customer, you’ll need to filter that specific customer’s data with secure views, and add those to the customer-specific share. *Note: secure views (vs. regular views) need to be used to avoid unintentionally exposing data from other customers.*


For this example, we will create one database per customer, each with a single schema. We will then create secure views in that schema to filter data for the specific customer from the commingled source database.


```text
-- For this example, we will partition data using customer-specific databases
create   database   if   not   exists   db_customer_00001;
create   schema   if   not   exists   db_customer_00001  .  schema_name  ;


-- Create a secure view (or views) to add to the share
create   secure view   db_customer_00001  .  schema_name  .transactions   as   (
select   *   from   db_customer_data  .  schema_all_customers  .transactions
where   organization_id   =   'customer_00001'
);
```


Now that the separate database has been created with the specific customer’s data, we can create the share.


## Setting up the Share


With the customer’s Snowflake account identifier available (e.g.,` ab00001` ,` xy56789` , etc.) you’ll now set up the share and initiate the connection between the Snowflake accounts.


In Snowflake, confirm that you’re logged in with a role that has the` CREATE SHARE` privilege, or ask an admin to grant it to you:


```text
-- Check for the "CREATE SHARE" privilege
show grants   to   role   data_share_admin;
```


Next, in Snowflake, you will create the share, add objects to the share, and finally, add the customer’s Snowflake account to the created share.


```text
-- Create a customer specific share. The customer will rename this when they accept the share
create   or   replace   share share_customer_00001;


-- Add objects to the share
grant   usage   on   database   db_customer_00001   to   share share_customer_00001;
grant   usage   on   schema   db_customer_00001  .  schema_name   to   share share_customer_00001;
grant   select   on   view   db_customer_00001  .  schema_name  .transactions   to   share share_customer_00001;


-- Add the customer's Snowflake account to the share
alter   share share_customer_00001   add   accounts  =  ab00001;
```


**Note: Sharing across regions.** By default, you will only be able to add Snowflake accounts to the share if their Snowflake account is in the same region. See “Scaling Data Sharing to other Regions” below for options to share data to other locations.


## Onboarding your Customer


Once the share is created, you’ll need your customer to follow a few steps in their Snowflake account to get access to the shared data.


- Navigate to the Snowflake webapp to accept the data share invitation.
- In Snowflake, have the` ACCOUNTADMIN` navigate to “Data” → “Private Sharing”, and find the share called` SHARE_CUSTOMER_00001` which will appear shared from the provider Snowflake account. Alternatively, the` ACCOUNTADMIN` can grant the ability to accept incoming shares using:` grant import share on account to <role>;`
- Click “Get shared data” to accept the share.
- Give the database a name to identify the database, select the roles to grant access to, then click “Get Data”.


Your customer should now see this data share directly in “Data” → “Databases”!


## Scaling Data Sharing to other Regions


If you’d like to share data with customers who have Snowflake accounts in other regions (e.g., us-west-1, us-east-1, us-east-2, etc.), you’ll need to first copy over the data to that specific cloud region. To do so, you can enable replication of the source data (in other words, the database that the secure view points to) to the target region.


Be aware that naively replicating this data to all regions where your customers have Snowflake accounts will incur additional data transfer and compute costs for every region the data is replicated.


To avoid runaway costs associated with replicating full datasets to many regions at once, you may want to explore moving each tenant’s data to their local cloud region more explicitly. For example, you can use a dbt transformation to incrementally replicate data to a region-specific database-share for all customers within a region.


Once you complete your Snowflake data sharing setup, you may discover that many customers use other data warehouses (Redshift, BigQuery, Databricks, etc.) or destination types. Snowflake doesn’t support sharing data to other warehouses or data destinations. If you’d like to share data to customers on other destinations and abstract away the complications of dynamic region replication altogether, you can use Prequel to expand your available destinations to all warehouses and all regions.


## Scaling Data Sharing to other Warehouses with Prequel


Prequel is a data sharing platform that makes it easy to connect to any customer data warehouse to share and sync data. Prequel handles complicated cross-region and cross-warehouse sharing out of the box, and includes customer onboarding forms that can be whitelabeled so you can ship data sharing in a matter of days.


To share data using Prequel, your first step is to create a Prequel account and connect your data source. Once your source is connected and you’ve selected the tables and columns you’d like to share, you can use Prequel to connect to any customer destination in any cloud region to begin sharing data.


If you’d prefer to let your customers onboard themselves, Prequel also provides a fully featured API with a set of SDKs to help your customers self-serve onto their data share directly in your app. These endpoints and SDKs are well documented in the Prequel API documentation.


## Conclusion


In this walkthrough, you’ve learned how to utilize the built-in Snowflake Data Share feature to share data directly to your customers, and we’ve touched on how to extend this to other regions and warehouses using Prequel.


Prequel is a data sharing platform that makes it easy to use your data source (Snowflake or otherwise) to sync data to all of your customers’ data warehouses and other data destinations.
