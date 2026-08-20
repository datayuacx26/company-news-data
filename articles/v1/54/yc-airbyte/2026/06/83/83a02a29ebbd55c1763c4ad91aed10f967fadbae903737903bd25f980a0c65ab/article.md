---
schema_version: "1.0.0"
document_id: "83a02a29ebbd55c1763c4ad91aed10f967fadbae903737903bd25f980a0c65ab"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/motherduck-certified-destination"
published_at: "2026-06-11T05:03:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:23:21.398471+00:00"
content_hash: "sha256:7326744fd0c25f14aa41814f62a955200acbbef75dfb304e69947d64e7950b7e"
---

# MotherDuck Is Now a Certified Destination in Airbyte

If you're using DuckDB and need a reliable way to integrate all your data sources, we've got you covered.


The MotherDuck destination connector in Airbyte is now officially certified. We’re excited about this partnership and future opportunities to integrate Airbyte and MotherDuck.


## **What is MotherDuck?**


MotherDuck is the serverless data warehouse built on DuckDB. Uniquely, MotherDuck uses a *[hypertenancy](http://motherduck.com/product/hypertenancy)* architecture: each user, service, or agent gets a dedicated compute instance called a Duckling. Queries run on these isolated instances, ensuring millisecond performance and complete isolation of compute, without resource contention.


MotherDuck is tightly integrated with the DuckDB ecosystem. You can connect instantly using a DuckDB extension, then download data locally to test and develop using the same workflows you’ll deploy to the cloud. With user interfaces from an MCP Server, to Python, to the DuckDB CLI, MotherDuck makes it trivial to query manually or with an agent at the wheel.


## **What data sources can I sync to MotherDuck?**


Any of the 600+ data sources available on Airbyte. Once you've set up the MotherDuck destination, you can pull data from sources like HubSpot, Stripe, Salesforce, GitHub, Slack, and hundreds more into a single queryable destination.


## **Destination features**


The MotherDuck destination implements the Destinations V2 specification. This means your tables are properly typed and automatically deduplicated. That means no raw JSON blobs to deal with downstream.


All sync modes are supported:


- Full Refresh (Overwrite)
- Full Refresh (Append)
- Full Refresh (Overwrite + Deduped)
- Incremental Sync (Append)
- Incremental Sync (Append + Deduped)


All tables automatically include three system-generated columns: _airbyte_raw_id, _airbyte_extracted_at, and _airbyte_meta. These contain metadata about the sync operation. The connector also supports namespaces, so you can configure where Airbyte writes tables..


## **How to set it up**


Adding the MotherDuck destination takes about two minutes.


Click the Add destination button on your Airbyte instance. Find MotherDuck and click the Add button.


Enter the database where you want your data written. The database path uses the md: prefix. For example, if you want the data in a database named my_database, type md:my_database.


Click the API key field and paste in your MotherDuck access token. Make sure that you do not include the access token in the database path as it may end up in your logs.


Click the Save button and you’re ready to go!


Select the data sources that you would like to sync with MotherDuck and choose the sync schedule for your data.


## **Who should use this**


**Teams building with AI**


Once your data lands in MotherDuck, it's instantly available to your AI tools and agents. Query in natural language through our MCP Server — from Claude, ChatGPT, or Cursor, no SQL required. And because every user, service, and agent gets its own compute instance, a busy agent firing thousands of queries never slows anyone else down.


**Teams with low-latency query needs**


MotherDuck's hypertenancy architecture gives every user and service their own dedicated compute instance against shared data, so queries return in milliseconds and no one waits on a shared cluster. Ideal for customer-facing analytics and dashboards that have to feel instant.


**Teams that want visualizations built in**


No need to bolt on a separate BI tool. Turn your synced data into a[Dive](http://motherduck.com/product/dives) — an embeddable data app you can build in minutes and drop straight into your product or share with your team.


## **What does "certified" mean?**


Certified connectors get direct maintenance from Airbyte engineers, are tested and production ready, and have pre-planned version upgrade grace periods.


The MotherDuck connector has been live since late 2024. It has gone through 20+ releases addressing DuckDB version upgrades (now on v1.4.2), special character handling in stream names, and unicode support in database and table names.


## **Try it out**


Set up the MotherDuck destination in[Airbyte Cloud](https://cloud.airbyte.com/) or read about all the configuration options in the[connector documentation](https://docs.airbyte.com/integrations/destinations/motherduck) . If you have DuckDB installed locally,[MotherDuck](https://airbyte.com/connectors/motherduck) offers a free tier for getting started with its cloud platform.
