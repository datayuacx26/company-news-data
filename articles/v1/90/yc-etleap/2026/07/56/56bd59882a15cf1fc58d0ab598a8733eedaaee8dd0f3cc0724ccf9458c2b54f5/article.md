---
schema_version: "1.0.0"
document_id: "56bd59882a15cf1fc58d0ab598a8733eedaaee8dd0f3cc0724ccf9458c2b54f5"
company_key: "yc-etleap"
company: "Etleap"
source_id: "yc-etleap-news-import-75d796be5177"
canonical_url: "https://etleap.com/blog/writing-dbt-models-to-iceberg-with-snowflake"
published_at: null
first_seen_at: "2026-07-21T18:50:49.183689+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:6c4f2ad244ab140424156199c55bbf760a970e1bdf982217a09c132a0da0e658"
---

# Writing dbt Models to Iceberg with Snowflake

*Tested with dbt-core 1.11.8 and dbt-snowflake 1.11.4*


Snowflake's Catalog-Linked Database (CLD) feature works with dbt Core 1.11's Iceberg materializations, so you can read and write Iceberg tables while using Snowflake only for compute. The output stays in your S3 bucket and your Glue catalog, queryable from any Iceberg-compatible engine (Athena, Spark, Trino, DuckDB, etc.).


This post is a practical guide to setting that up, including a case-sensitivity issue you'll hit on your second` dbt run` .


## Why Use This Pattern?


Materializing dbt models to Iceberg makes sense when the transformed output has consumers beyond Snowflake. A few things this gets you:


-


**One copy of the data.** The tables dbt produces live in your S3 bucket and Glue catalog, not in Snowflake-managed storage.


-


**Portability of the transformed layer.** Most Iceberg-on-Snowflake setups stop at ingestion: raw data lands in Iceberg, but the moment dbt runs, the output becomes a native Snowflake table. Writing dbt models back to Iceberg keeps the entire pipeline open.


This pattern isn't the right choice for every project, and there are a few things worth knowing before you start:


-


**dbt views are not supported in a CLD.** Only` table` and` incremental` materializations work. Models you'd normally express as views need to either move to native Snowflake or be rewritten as tables.


-


**Quoting and case sensitivity can become a problem.** The Glue–Snowflake case-sensitivity mismatch (covered later in this post) is the most common source of friction.


-


**You need to be on a recent dbt version.** Iceberg materializations require dbt-core 1.11+ and a current` dbt-snowflake` adapter.


-


**If Snowflake is the only consumer, this is the wrong pattern.** Native Snowflake tables are simpler, faster to query, and have fewer moving parts.


## Snowflake Setup


The following setup assumes that you are using S3 for storage and Glue as your catalog for your Iceberg tables already. To let Snowflake write dbt models back to that same Iceberg environment, you need four things on the Snowflake side.


### **1. Iceberg REST Catalog Integration**


Follow[Snowflake’s steps to create a catalog integration](https://docs.snowflake.com/en/user-guide/tables-iceberg-configure-catalog-integration-rest-glue) **** of type` ICEBERG_REST` pointing at your Glue catalog.


Since we’ve found that Snowflake's example read-write policy leaves out some permissions, here's a combined policy covering both the catalog integration and the external volume (step 2 below):


```text
{
"Version"  :    "2012-10-17"  ,
"Statement"  :    [
{
"Sid"  :    "AllowGlueCatalogAccess"  ,
"Effect"  :    "Allow"  ,
"Action"  :    [
"glue:GetCatalog"  ,
"glue:GetDatabase"  ,
"glue:GetDatabases"  ,
"glue:CreateDatabase"  ,
"glue:DeleteDatabase"  ,
"glue:GetTable"  ,
"glue:GetTables"  ,
"glue:CreateTable"  ,
"glue:UpdateTable"  ,
"glue:DeleteTable"
]  ,
"Resource"  :    [
"arn:aws:glue:*:{accountid}:table/*/*"  ,
"arn:aws:glue:*:{accountid}:catalog"  ,
"arn:aws:glue:*:{accountid}:database/*"  ,
"arn:aws:glue:*:{accountid}:userDefinedFunction/*/*"
]
}  ,
{
"Sid"  :    "AllowS3ExternalVolumeAccess"  ,
"Effect"  :    "Allow"  ,
"Action"  :    [
"s3:GetObject"  ,
"s3:PutObject"  ,
"s3:DeleteObject"  ,
"s3:ListBucket"
]  ,
"Resource"  :    [
"arn:aws:s3:::{bucket-name}"  ,
"arn:aws:s3:::{bucket-name}/*"
]
}
]
}
```


### **2. External Volume**


Follow[Snowflake’s steps to create an external volume](https://docs.snowflake.com/en/user-guide/tables-iceberg-configure-external-volume-s3) **** that points Snowflake at the S3 location where Iceberg data and metadata will be written. This must allow writes.


### **3. Catalog-Linked Database (CLD)**


A catalog-linked database is a Snowflake database connected to an external Iceberg REST catalog. Once created, Snowflake treats the external catalog's tables as if they were native: you can query, create, and drop them in a similar manner to your native Snowflake tables.


Without a CLD, dbt can't manage the Iceberg tables in both Snowflake and Glue. The CLD gives dbt a single interface to create, query, and drop Iceberg tables.


Create the CLD using the Catalog Integration and External Volume from the prior steps:


```text
CREATE   DATABASE dbt_iceberg_cld
LINKED_CATALOG =  (
CATALOG   =  'glue_rest_catalog_int'
)


```


Snowflake exposes Glue objects through a three-part name:` cld_name.schema.table` . The CLD maps to your Glue catalog: each Glue database appears as a schema, and each Glue table appears as a table within that schema. The CLD name is independent of your Glue catalog name.


### **4. User Grants**


Your dbt user needs read/write access on the CLD in addition to any permissions it has on your regular Snowflake database.


### Validate your CLD


At this point, when you view the` dbt_iceberg_cld` database in Snowflake’s database explorer, you should see that your existing Glue Databases appear as schemas and that your Iceberg tables are queryable as` dbt_iceberg_cld.glue_database_name.iceberg_table_name` .


If you are still not seeing the Iceberg tables that you expect to be in the CLD, or are unable to create new tables, the following queries can help to diagnose where the problem is originating:


```text
SELECT   SYSTEM$VERIFY_CATALOG_INTEGRATION (    'glue_rest_catalog_int'    )  ;
SELECT   SYSTEM$VERIFY_EXTERNAL_VOLUME (    'iceberg_external_volume'    )  ;
SELECT   SYSTEM$GET_CATALOG_LINKED_DATABASE_CONFIG (    'dbt_iceberg_cld'    )


```


## Configure your dbt Project


You can now configure dbt to read and write Iceberg tables through the CLD.


### Create a` catalogs.yml` File


This file lives in your dbt project root and defines the write integration.


```text
# catalogs.yml
catalogs  :
-  name  : snowflake_cld
active_write_integration  : glue_rest_integration
write_integrations  :
-  name  : glue_rest_integration
catalog_type  : iceberg_rest
external_volume  : iceberg_external_volume
table_format  : iceberg
adapter_properties  :
catalog_linked_database  : dbt_iceberg_cld
catalog_linked_database_type


```


### Enable Iceberg Materializations for your Project


Enable Iceberg materializations by adding the` enable_iceberg_materializations` flag in your project. See dbt’s[Snowflake and Apache Iceberg topic’s limitations section](https://docs.getdbt.com/docs/mesh/iceberg/snowflake-iceberg-support?version=1.11#limitations-1) for more information on this flag.


```text
# dbt_project.yml
flags  :
enable_iceberg_materializations  :  true
```


### Configure your Models to write to Iceberg


Apply` catalog_name` to any model you want materialized as an Iceberg table. You can do this per-model or in` dbt_project.yml` for an entire path:


```text
# dbt_project.yml
models  :
your_project  :
iceberg_models  :
+materialized  : table
+catalog_name


```


Or per-model:


```text
# model. sql
{  {
config (
materialized= 'table'  ,
catalog_name= 'snowflake_cld'
)
}  }


select   *
from    {  {   source (  'raw'  ,    'orders'  )    }  }
```


The only supported materializations for Iceberg currently are` table` and` incremental` . Views cannot be created in a Catalog Linked Database.


With this in place,` dbt run` will create Iceberg tables in your S3 bucket and register them in your Glue catalog. You can verify the tables appear in Snowflake's database explorer under your CLD, and that the underlying files exist in S3.


## Case Sensitivity and Quoting


One final piece of configuration is required to avoid case-sensitivity errors on re-runs. To understand why, it helps to know how each layer handles casing:


-


**Glue** enforces that everything is lowercase.


-


**Snowflake** treats everything as uppercase unless wrapped in double quotes by default.


-


**Snowflake CLDs** that are set to be case-insensitive resolve queries written with uppercase identifiers to the lowercase Glue identifiers.


-


**dbt** provides a project-level option to support quoted or unquoted identifiers.


Here’s where it breaks: your first` dbt run` against a CLD succeeds. dbt runs statements like` CREATE ICEBERG TABLE MY_MODEL` and the CLD properly translates these to be` create iceberg table "my_model"` .


However, your second run fails with:


```text


```


By default, Snowflake's Catalog Linked Database is case-insensitive for queries, but dbt's` get_relation()` compares identifiers case-sensitively before issuing any SQL. The CLD surfaces Glue's lowercase identifiers as-is (e.g.,` "your_schema"."your_model"` ), while dbt expects the unquoted uppercase form (` YOUR_SCHEMA.YOUR_MODEL` ). dbt sees these as an "approximate match" rather than an exact match and refuses to proceed.


To avoid this issue, you can enable quoting for schema and identifier names in` dbt_project.yml` :


```text
# dbt_project.yml
quoting  :
database  :  false
schema  :  true
identifier  :  true
```


With quoting enabled, dbt sends lowercase quoted identifiers, which match what the CLD returns, and the relation lookup succeeds.


One caveat: this is a project-wide setting. Existing models in the same project that reference or materialize as native Snowflake tables with unquoted (uppercase) identifiers may fail to resolve after quoting is enabled, since dbt will now look up lowercase quoted identifiers. If you're adding Iceberg materializations to an existing dbt project on` 1.11.x` , plan for a rename pass or a project split.


## Mixing CLD and Native Snowflake Models


Most dbt projects using a CLD will also have models that write to a regular Snowflake database. A few things to keep in mind when both live in the same project.


### 1. A model cannot write to a CLD without setting` catalog_name`


When you add the` catalog_name` config, a lot happens under the hood that makes Iceberg materializations work, notably:


-


dbt uses` CREATE ICEBERG TABLE...` instead of` CREATE TABLE`


-


dbt uses the database defined in` catalogs.yml` instead of the` target.database`


With this in mind, it is typically unnecessary to set your` target.database` to your CLD. If dbt attempts to write a model without a` catalog_name` defined to the CLD, it will fail.


### 2. Different environments may require a different catalog in` catalogs.yml`


If you write models to different databases depending on the environment (e.g., dev vs. prod), you'll need a separate CLD for each environment. You will need to define an additional entry in` catalogs.yml` for each one; you can then use a variable to set` catalog_name` conditionally in your model or` dbt_project.yml` :


```text
# dbt_project.yml
models  :
your_project  :
iceberg_models  :
+materialized  : table
+catalog_name  :  "{{ var('iceberg_catalog', 'snowflake_cld_dev') }}"
```


## Querying from Outside Snowflake


The tables dbt writes through the CLD are standard Iceberg tables registered in your Glue catalog and stored on S3. Nothing about the output depends on Snowflake remaining your query engine.


For example, in Athena:


```text
SELECT   *  FROM   your_glue_database.your_model  LIMIT    10
```


The same tables are readable from Spark, Trino, Flink, and DuckDB without copying data.
