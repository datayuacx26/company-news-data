---
schema_version: "1.0.0"
document_id: "b23b0da9b1dc5c54f0c3c2a6ed8f4fe7f8df46fde1d98597479017f58be56281"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/data-api-nearest-read-replica"
published_at: "2025-04-04T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:bc81a63230b9f86a4049b8bb8b87116a1765639f82d3b93b4d2e9d7e5c82d2c2"
---

# Data API Routes to Nearest Read Replica

Today we’re releasing Data API requests routing to the nearest Read Replica by extending our[API load balancer](https://supabase.com/docs/guides/platform/read-replicas#api-load-balancer) to handle geo-routing.


It’s an impactful improvement that will minimize request latency for your globally distributed applications. It’s available by default when using a load balancer endpoint.


## What is geo-routing?#


Geo-routing automatically directs your Data API requests to the geographically closest read replica of your database, reducing latency and improving response times for your users around the world.


Previously, if you had read replicas in Frankfurt, Singapore, and Virginia, a user located in Europe may experience dramatically different latencies because they could be making requests to any of the replicas.


Our new geo-routing automatically connects users to the nearest read replica so the same user would only make requests to the read replica in the Frankfurt region.


## How geo-routing works#


Our geo-routing system uses geospatial algorithms to determine the optimal read replica for each request:


1. Each incoming API request includes geolocation data from the network edge (specifically the` cf.colo` property, which provides the IATA airport code of the datacenter that received the request).
2. We maintain a coordinate mapping system that associates each region where read replicas can be deployed with precise geospatial coordinates.
3. When a request arrives, we calculate the distance between the network edge and each available read replica using the[Haversine formula](https://en.wikipedia.org/wiki/Haversine_formula) (which determines the great-circle distance between two points on a sphere using their longitudes and latitudes).
4. The system automatically routes the request to the geographically closest read replica, minimizing network latency without requiring any configuration on your part.
5. In cases where multiple databases exist in the same region we implement a round-robin strategy to ensure balanced load distribution.


The entire process is completely seamless to your application and users, requiring no changes to your code or configuration besides updating your project URL (` <project_ref>-all.supabase.co` ) today.


To get the most from geo-routing, deploy read replicas in regions where your users are concentrated. The more strategically you place your read replicas, the more your users will benefit from reduced latency and improved response times.


## Initial release and roadmap#


As an initial release, geo-routing is available with the following limitations:


- Currently limited to read-only Data API (PostgREST) requests


If you're already using our API load balancer there's nothing you need to do; geo-routing is automatically applied to your Data API requests.


Otherwise, you can enable this feature by ensuring your project is using the API load balancer endpoint (` <project_ref>-all.supabase.co` )


We're actively working on expanding geo-routing support to other Supabase products, such as Auth, Storage, and Realtime. Stay tuned for updates.


## Get started today#


As always, we welcome your feedback, let us know what you think!


- [Sign up for Supabase](https://supabase.com/dashboard/sign-up) and get started today
