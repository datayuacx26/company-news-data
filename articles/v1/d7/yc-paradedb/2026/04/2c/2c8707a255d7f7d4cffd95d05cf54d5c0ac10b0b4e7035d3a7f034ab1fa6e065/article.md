---
schema_version: "1.0.0"
document_id: "2c8707a255d7f7d4cffd95d05cf54d5c0ac10b0b4e7035d3a7f034ab1fa6e065"
company_key: "yc-paradedb"
company: "ParadeDB"
source_id: "yc-paradedb-rss-b460d8ada1d2"
canonical_url: "https://www.paradedb.com/blog/railway"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:48.722459+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:b76d9b7ac99da8e67efc660108ac0ce75ed32392c8a7b083cd70910b53d74219"
---

# ParadeDB is Officially on Railway

# ParadeDB is Officially on Railway


By Ming Ying on April 14, 2026


We're excited to announce that[Railway](https://railway.com/) now supports ParadeDB as an official integration. This means one-click deployment of ParadeDB onto Railway, allowing you to go from zero to a running ParadeDB instance in under a minute.


## Why Railway?


Railway is one of the leading platforms for deploying AI applications, databases, and backend services. Its developer experience is exceptional: push-to-deploy, automatic environment variables, and a clean dashboard for managing services.


## Deploy ParadeDB in One Click


Click the button below to deploy ParadeDB on Railway. Railway will spin up a Docker container running the latest ParadeDB image with sensible defaults.


Once deployed, Railway automatically configures the following environment variables for you:


Variable Default


` POSTGRES_USER`` postgres`


` POSTGRES_PASSWORD` Auto-generated


` POSTGRES_DB`` paradedb`


` PGPORT`` 5432`


` DATABASE_URL` Private connection string


` DATABASE_PUBLIC_URL` Public connection string


## Connect to Your Instance


After deployment, navigate to the **Variables** tab in your Railway service dashboard to find your connection strings.


To connect from another Railway service on the same project, use the private connection string:


```text
psql  $DATABASE_URL


```


To connect from your local machine, use the public connection string:


```text
psql  $DATABASE_PUBLIC_URL


```


## Try a Quick Search Query


Once connected, you can immediately start using ParadeDB's search features. Load the sample table, create a BM25 index, and run a full-text search:


```text
CALL   paradedb.create_bm25_test_table(
schema_name  =  >    'public'  ,
table_name  =  >    'mock_items'
);


CREATE   INDEX search_idx  ON   mock_items
USING   bm25 (id, description, category, rating, in_stock, created_at, metadata, weight_range)
WITH   (key_field =  'id'  );


SELECT   description, pdb.score(id)
FROM   mock_items
WHERE   description  ||  |    'running shoes'    AND   rating  >    2
ORDER    BY   score  DESC
LIMIT  5  ;


```


## What's Next


To learn more about deploying ParadeDB on Railway, visit our[documentation](https://docs.paradedb.com/deploy/cloud-platforms/railway) .
