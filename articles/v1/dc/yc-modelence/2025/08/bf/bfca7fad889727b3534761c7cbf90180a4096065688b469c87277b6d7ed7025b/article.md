---
schema_version: "1.0.0"
document_id: "bfca7fad889727b3534761c7cbf90180a4096065688b469c87277b6d7ed7025b"
company_key: "yc-modelence"
company: "Modelence"
source_id: "yc-modelence-news-import-7e8ea9c35a32"
canonical_url: "https://modelence.com/blog/migrate-mongodb-data-api"
published_at: "2025-08-27T00:00:00+00:00"
first_seen_at: "2026-07-24T04:44:45.712461+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:72c9e1f8a277d6086b6371abcabdbbddbad440392ae5c36c82c1efd188ab8f25"
---

# Migrate your MongoDB Data API in 30 minutes

## Introduction


MongoDB has recently announced the deprecation of its[Atlas Data API](https://modelence.com/blog/modelence-mongodb-atlas) (as well as Custom HTTPS Endpoints) for Atlas App Services - set to become unavailable after September 30, 2025. This affects many developers and applications that rely on HTTP-based CRUD access to Atlas clusters.


[Official MongoDB deprecation notice](https://www.mongodb.com/docs/atlas/app-services/data-api/data-api-deprecation/)


## What is MongoDB Data API?


The Data API provided a simple, RESTful HTTP interface directly to your MongoDB Atlas cluster - developers could perform CRUD operations via HTTP requests without needing a full backend or client driver. It was especially useful for serverless workflows, quick integrations, and frontend-only applications.


## Who used it - and what's happening now?


Many serverless apps, low-code tools, and rapid prototyping platforms relied heavily on the Data API. These users are now facing pending disruptions as MongoDB phases it out. On September 30, 2025, both the Data API and Custom HTTPS Endpoints are planned to be removed.


MongoDB has published documentation to help users migrate, recommending alternatives like Express + Drivers, Cloud Functions, and partner solutions. But for most teams, adopting these options means a complex setup process, significant time investment, and, in many cases, the added challenge of migrating data.


## Introducing Modelence: Your drop-in replacement


Replacing the deprecated Data API is easy. With Modelence, you can provision a fully functional, production-ready MongoDB Data API replacement in about 30 minutes - no heavy backend work required.


## Step-by-step migration with Modelence


1. Create a new Modelence app and cloud environment by following the setup wizard. Register at[cloud.modelence.com](https://cloud.modelence.com/) .
2. Choose the **data-api** template from example apps.
3. Copy the generated command and run it in your terminal to deploy data-api to your Modelence Cloud.
4. In the Modelence Dashboard, view your app deployment status in the Deployments tab.
5. Once it's deployed (which will take 5-10 minutes), navigate to **Application → Custom Configuration** section and set your Data API key and MongoDB URI. You can always change them, and we recommend rotating API keys periodically for security purposes.
6. Your endpoint will have the following format:` http://<your-app>.prod.modelence.app`


Now you can use the following endpoints to invoke your Data API:


**Login example:** Use the API key configured in the previous step.


```text
curl --location 'http://<your-app>.prod.modelence.app/auth/providers/api-key/login' \
--header 'Content-Type: application/json' \
--data '{ "key": "123" }'


```


This will return an access token, which should be used in all API calls as an authorization bearer.


**Insert document example:**


```text
curl --location 'http://<your-app>.prod.modelence.app/data/v1/action/insertOne' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <access-token>' \
--data '{
"dataSource": "Cluster0",
"database": "dataApi",
"collection": "users",
"document": { "name": "John Sample", "age": 42 }
}'


```


For an interactive feel, use the Postman-like interface available at` http://<your-app>.prod.modelence.app` .
