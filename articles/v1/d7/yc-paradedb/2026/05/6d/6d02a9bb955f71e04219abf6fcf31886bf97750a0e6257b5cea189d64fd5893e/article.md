---
schema_version: "1.0.0"
document_id: "6d02a9bb955f71e04219abf6fcf31886bf97750a0e6257b5cea189d64fd5893e"
company_key: "yc-paradedb"
company: "ParadeDB"
source_id: "yc-paradedb-rss-b460d8ada1d2"
canonical_url: "https://www.paradedb.com/blog/render"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:48.722459+00:00"
fetched_at: "2026-07-28T22:14:43.061733+00:00"
content_hash: "sha256:b8a677c528848c5a06fafe3c2cd673a4ddfc5b175990a54edce75a90c2a67b31"
---

# ParadeDB is Officially on Render

# ParadeDB is Officially on Render


By Ming Ying on May 12, 2026


We're excited to announce that[Render](https://render.com/) now supports ParadeDB as an official integration. This means one-click deployment of ParadeDB onto Render, allowing you to go from zero to a running ParadeDB instance in under a minute.


## Why Render?


Render is one of the leading platforms for deploying AI applications, databases, and backend services. Its developer experience is exceptional: push-to-deploy, automatic environment variables, and a clean dashboard for managing services.


## Deploy ParadeDB in One Click


The ParadeDB Render Blueprint deploys ParadeDB Community as a private service with persistent SSD storage. Click the button below to provision it in your account, or use Render's[official ParadeDB template](https://render.com/docs/deploy-paradedb) .


Behind the scenes, the Blueprint will:


- Create a private service named` paradedb` running the official ParadeDB Docker image.
- Attach a 10 GB persistent disk for your database data, mounted at` /var/lib/postgresql` .
- Configure the following environment variables for you:


Variable Default


` POSTGRES_USER`` postgres`


` POSTGRES_PASSWORD` Auto-generated


` POSTGRES_DB`` paradedb`


## Connect to Your Instance


ParadeDB runs as a private service on Render, which means it is not exposed to the public internet.


To connect from another service in your Render private network, use the service name as the host:


```text
psql -h paradedb -U postgres -d paradedb


```


To connect from your local machine, SSH into the service and run` psql` from inside the container:


```text
ssh srv-XXXXXXXXXXXXX@ssh.<region>.render.com
psql -U postgres -d paradedb


```


Replace` srv-XXXXXXXXXXXXX` with your service ID from the Render dashboard, and make sure you've added an SSH key to your Render account first.


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


To learn more about deploying ParadeDB on Render, visit our[documentation](https://docs.paradedb.com/deploy/cloud-platforms/render) .
