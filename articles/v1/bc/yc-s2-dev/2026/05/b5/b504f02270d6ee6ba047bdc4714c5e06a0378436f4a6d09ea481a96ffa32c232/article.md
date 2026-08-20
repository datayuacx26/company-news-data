---
schema_version: "1.0.0"
document_id: "b504f02270d6ee6ba047bdc4714c5e06a0378436f4a6d09ea481a96ffa32c232"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/locations-us-west-eu-north"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:540fd6aa9e5eb5a1a62bdcdcfcd7e69e4b5a2fe1ed032dc23dbaf3cbcdb6be4a"
---

# Two new locations for your streams: US West and EU North

Until today, S2 ran in one place - **US East** (` aws:us-east-1` ). Every basin you created was placed there, so its streams were too.


Today, that changes.


Location is now a choice when you create a basin. Two new locations are live, alongside US East:


- **US West** (` aws:us-west-2` )
- **EU North** (` aws:eu-north-1` )


With the EU North location, stream data is stored in and served from the EU.


## How it works


[Basins](https://s2.dev/docs/concepts/basins) are placed in a *location* . Basin-specific DNS[endpoints](https://s2.dev/docs/api/endpoints) route stream requests to a gateway service in that location. Appends write to object storage in that location, and reads are served from there too.


[Under the hood](https://s2.dev/docs/platform/architecture) , we spun up new cells and made many infrastructure improvements. S2 continues to make every single write durable in multiple availability zones. PrivateLink is also[supported](https://s2.dev/docs/platform/private-networking) for AWS clients.


The closer your basin is to your readers and writers, the lower your latency, which usually also translates into higher throughput.


Existing basins keep their current location – to use a different location, create a new basin there.


## Try it


*Requires[CLI](https://s2.dev/docs/cli)` s2 >= 0.36` or a current[SDK](https://s2.dev/docs/sdk) with location support, e.g.` @s2-dev/streamstore >= 0.24` .*


Pick a location when you create a basin:


```text
$   s2   create-basin   european-agents   --location   aws:eu-north-1
```


Or from the SDK:


```text
import   { S2 }   from   '@s2-dev/streamstore'  ;


const   s2   =   new   S2  ({ accessToken: process.env.  S2_ACCESS_TOKEN  !   });


await   s2.basins.  create  ({
basin:   'european-agents'  ,
location:   'aws:eu-north-1'  ,
});
```


Or in the dashboard's` Create Basin` dialog, where the **Location** dropdown lists everywhere you can land a basin.


Set a default once, and every new basin lands there unless you say otherwise:


```text
$   s2   set-default-location   aws:eu-north-1
```


See what's available:


```text
$   s2   list-locations
aws:us-east-1
aws:us-west-2
aws:eu-north-1
```


All 3 locations are generally available today on the s2.dev[cloud service](https://s2.dev/dashboard) , with the same transparent[pricing](https://s2.dev/pricing) .


Want to see it in action? Open the[playground](https://s2.dev/playground) to create a stream in the nearest location and start reading and writing records.
