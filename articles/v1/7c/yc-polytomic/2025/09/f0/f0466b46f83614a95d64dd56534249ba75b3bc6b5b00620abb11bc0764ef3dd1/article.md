---
schema_version: "1.0.0"
document_id: "f0466b46f83614a95d64dd56534249ba75b3bc6b5b00620abb11bc0764ef3dd1"
company_key: "yc-polytomic"
company: "Polytomic"
source_id: "yc-polytomic-news-import-c5d7ea331448"
canonical_url: "https://www.polytomic.com/blog-posts/polytomic-connect-new-release"
published_at: "2025-09-18T00:00:00+00:00"
first_seen_at: "2026-07-25T19:31:15.738590+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:d14516fcb55dc511b37aadca79184e84d8e4dd0550cece68349d57b6b378a85e"
---

# Polytomic Connect API: New Release

Today we are releasing a new version of[Polytomic Connect](https://apidocs.polytomic.com/) : version` 2025-09-18` .


Polytomic Connect is Polytomic's embed API, where you can white-label Polytomic's full sync and ETL capabilities in your own product to sync and ETL to/from your customers' systems.


Today's release is a major one with breaking changes (see our[migration guide](https://apidocs.polytomic.com/guides/2025-09-18-migration-guide) ). Here are the major new features:


### Connection Proxy


Although Polytomic provides a single consistent API for pulling or pushing data, once in a while you may want to access an integration's native API to carry out an operation that has nothing to do with data syncing.


The[connection proxy interface](https://apidocs.polytomic.com/guides/2025-09-18-migration-guide#connection-proxy-api) allows you to do this. It provides access to every single native API endpoint for each integration supported by Polytomic. Yet because these native API calls happen through the Polytomic Connection object, you still keep the advantage of Polytomic handling all authentication, token management, and rate limits for you.


### Shared Connections


When pulling from/pushing to your customers' systems, you often have a central data warehouse that all your data syncing runs on. Previously, every Organization required a separate connection to this data warehouse, resulting in duplicated Polytomic connection health checks, cache refreshes, and so on that caused unnecessarily duplicated load on these systems.


Now, with[shared connections](https://apidocs.polytomic.com/api-reference/connections/create-shared-connection) , you can create your parent connection once in your parent Organization then share it to each of your customer Organizations.


The shared connections can be used just like real ones, except that all health check operations happen once on the parent connection, massively cutting down on connection load.


### Console Log API


Should you want to display diagnostic sync console logs to in your app such as the screenshot below, you can now do so by accessing our new console log APIs (separated by[Model Sync](https://apidocs.polytomic.com/api-reference/model-sync/executions/get-console-logs) and[Bulk Sync](https://apidocs.polytomic.com/api-reference/bulk-sync/executions/get-console-logs) console logs).


### Universal Entity Lookup


Using our new[Entity Lookup API](https://apidocs.polytomic.com/api-reference/entities/get) , you can now resolve any Polytomic UUID without knowing what type of object it is.


### Other changes


These are a few of the major changes in this new version of Polytomic Connect. For the full list, see the[What's New](https://apidocs.polytomic.com/guides/2025-09-18-migration-guide#whats-new) section of our documentation.


As always, please write to support@polytomic.com with any questions!


[Back to blog](https://www.polytomic.com/blog)
