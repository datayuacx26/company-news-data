---
schema_version: "1.0.0"
document_id: "81546b7718c00f449ab90b847f87172b16978081a14d7781f0fa7a32b423e16f"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/container-spot-capacity/"
published_at: "2025-01-13T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:c6453fa1c4e84da2ed9ed4c01b31426c0f3a6d0ea8bc19477859d8a16402576a"
---

# Container Spot capacity

We are adding support for using Spot instances when you create a Fargate service with the[Cluster](https://sst.dev/docs/component/aws/cluster) component through the[capacity](https://sst.dev/docs/component/aws/cluster#capacity) prop. Spot instances can be around 50% cheaper and we talk about how they work here.


[Play](https://youtube.com/watch?v=2RrAZiTZoeA)


## Background


You can create container services with ECS and Fargate in SST with the[Cluster](https://sst.dev/docs/component/aws/cluster) component.


You are charged per hour of vCPU and GB of memory used. With our base config, this works out to around $12 per month.


Spot instances are spare capacity that AWS has and it’s available at a discounted rate, around $6 per month.


---


## Spot


You can enable this using the new[capacity](https://sst.dev/docs/component/aws/cluster#capacity) prop.


sst.config.ts


```text
const   cluster   =   new     sst  .  aws  .  Cluster  (  "  MyCluster  "  , {   vpc   }  );
new   sst  .  aws  .  Service  (  "  MyService  "  , {         cluster,         loadBalancer: {           ports: [{ listen:   "  80/http  "   }],         },         capacity:   "  spot  "  ,    });
```


You can also configure the % of regular Fargate vs Spot capacity you want to use.


sst.config.ts


```text
capacity: {         fargate: { weight:   1   },         spot: { weight:   1   }    }
```


Learn more about the[capacity](https://sst.dev/docs/component/aws/cluster#capacity) prop.


---


#### Caveats


There are a couple of caveats.


1. AWS may reclaim this capacity and turn off your service after a two-minute warning. This is rare, but it can happen.
2. If there’s no spare capacity, you’ll get an error.


This makes Fargate Spot a good option for dev or PR environments.


sst.config.ts


```text
capacity: $app  .  stage     ===     "  production  "     ?     undefined     :     "  spot  "  ;
```


---


## Get started


Get started by checking out the[Fargate Spot capacity example](https://sst.dev/docs/examples/#aws-cluster-spot-capacity) .


You can also check out our container service quick starts.


- [Bun](https://sst.dev/docs/start/aws/bun)
- [Deno](https://sst.dev/docs/start/aws/deno)
- [NestJS](https://sst.dev/docs/start/aws/nestjs)
- [Express](https://sst.dev/docs/start/aws/express)


These will help you get started with building container services.
