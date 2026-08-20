---
schema_version: "1.0.0"
document_id: "c7889f85df096ac8c2ac8d80f8795cb367feb0242ae8c77f34bc353f37aab5ab"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/hotglue-melt-april-2024"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:8997187bee7ad35e4247767015fb11bf2a8226a869bec2decb2b3c6dc0d66e21"
---

# hotglue melt: April 2024

Welcome to our April Melt! Here’s what you may have missed from the past month.


## **New Connectors ⛓️**


We added 5 new integrations to our library and the open-source Singer community in April.


**Logic4**


Keyna from our integrations team built a tap and target for Logic4 ([logic4.nl](https://hotglue.com/blog/logic4.nl) ), an e-commerce and ERP platform based in the Netherlands. You can use the connectors to fetch inventory, purchase, and sales information, and write back transactions like purchase orders.


**Bird (formerly MessageBird)**


Keyna built a target for Bird ([bird.com](https://bird.com/) ), an omnichannel communications platform and CRM. Built on Hotglue’s target SDK, the target lets you push any data, like new contacts, to your customers’ Bird CRM accounts.


**Appsflyer**


Adil from our integrations team built a target for Appsflyer ([appsflyer.com](https://www.appsflyer.com/) ), a mobile marketing analytics tool. Combined with our existing Appsflyer tap, the new target allows you to offer bi-directional connectivity with your customers’ Appsflyer apps.


**Connectwise**


Our partner, Plentive, built a connector to read data from Connectwise ([connectwise.com](https://www.connectwise.com/) ), an MSP management solution. The tap fetches data such as projects, activities, and time entries.


**ClientTether**


Chauncy from Plentive also built a connector for ClientTether ([clienttether.com](https://clienttether.com/) ), a CRM for franchises. The connector fetches general CRM data from your customers’ ClientTether accounts.


We also released a Confluence connector out of beta, thanks to a fork of[Edgar Ramírez Mondragón](https://github.com/edgarrmondragon) ’s Singer tap.


## **New API endpoints 🚥**


We released two new API endpoints last month.


#### ` catalog/search`


Run fast, granular searches across catalogs to search for streams, fetch types, and more. This is particularly useful for large catalogs that are too big to be served in a single linkedSource fetch.


#### ` discover/kill`


Just like the Kill Jobs endpoint, you can use this to end any running discovers for a linked connection.


## **Job log quality of life updates 🔁**


You may also notice some minor QoL updates to your log viewing experience:


- Logs now open with the erroring section loaded, rather than the first line of the log
- We added new navigation pills to quickly skip between the sync, ETL, and export sections
- Logs are now clickable as soon as jobs kick off


## **Job-level environment variable control ✅**


You have more control than ever over the behavior that your jobs run with.


#### ` POST /jobs`


When making a POST to \`/jobs\`, you can define override environment variables. This could be an existing reserved variable (max_record_count, default_export_format) or a custom environment variable used in your ETL.


#### **Schedules**


When setting schedules, you can now configure schedules to run with specific environment variables and job settings, just like manual job runs. Supported params in schedules now include:


- ` override_start_date`
- ` override_selected_tables`
- ` override_field_map`
- ` environment_variables`


Got questions, or have requests you’d like to see us build? Reach out to us in Slack or athello@hotglue.xyz .


See you next month 🙂


- Brendan


TABLE OF CONTENTS


RECOMMENDED BLOGS


[hotglue melt: March 2024 This is your monthly update for March!](https://hotglue.com/blog/hotglue-melt-march-2024)


[hotglue melt: February 2024 This is your monthly update for February!](https://hotglue.com/blog/hotglue-melt-february-2024)


[hotglue melt: 2023 Feature Roundup This is your 2023 hotglue feature update!](https://hotglue.com/blog/hotglue-melt-2023-Feature-Roundup)
