---
schema_version: "1.0.0"
document_id: "b770b9ec5009d08cd60a934f44305af9250e4c6f8d035dcb571cc9cd43bfe403"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/aurora-serverless-in-v3/"
published_at: "2025-01-05T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:a0686091d473ea6f6346cb0580a6336d7c75e0479735333ac49159d8218e0d1f"
---

# Aurora Serverless in v3

We are adding[Aurora](https://sst.dev/docs/component/aws/aurora) , a new component for[Amazon Aurora Serverless v2](https://aws.amazon.com/rds/aurora/serverless/) . Recently, AWS announced that Aurora Serverless v2 can[scale to 0](https://aws.amazon.com/blogs/database/introducing-scaling-to-0-capacity-with-amazon-aurora-serverless-v2/) and auto-pause. This is good for dev or PR stages.


There are some differences between this and the[Postgres](https://sst.dev/docs/component/aws/postgres) RDS component. We talk about it here in this video.


[Play](https://youtube.com/watch?v=xt2PoZDcwxY)


---


## Getting started


To get started, you can add the` Aurora` component to your app.


sst.config.ts


```text
const   vpc   =   new     sst  .  aws  .  Vpc  (  "  MyVpc  "  );
const   database   =   new     sst  .  aws  .  Aurora  (  "  MyDatabase  "  , {         engine:   "  postgres  "  ,        vpc    }  );
```


Read more about the[Aurora](https://sst.dev/docs/component/aws/aurora) component.


---


#### Scaling


By default, this has a` min` of 0 ACUs and a` max` of 4 ACUs.


An ACU or Aurora Capacity Unit is roughly equivalent to 2 GB of memory. So pick the minimum and maximum based on the baseline and peak memory usage of your app.


sst.config.ts


```text
new   sst  .  aws  .  Aurora  (  "  MyDatabase  "  , {         engine:   "  postgres  "  ,         scaling: {           min:   "  2 ACU  "  ,           max:   "  128 ACU  "         },         vpc    });
```


If you set a min of 0 ACUs, the database will be paused when there are no active connections in the` pauseAfter` specified time period.


Read more about the[scaling](https://sst.dev/docs/component/aws/aurora#scaling) config.


---


#### Dev mode


Aside from scaling to 0, you can also configure the` Aurora` component to not deploy the database in` sst dev` . Instead it can link to your locally running database, if you enable the` dev` prop.


sst.config.ts


```text
new   sst  .  aws  .  Aurora  (  "  MyDatabase  "  , {         engine:   "  postgres  "  ,         dev: {           username:   "  postgres  "  ,           password:   "  password  "  ,           database:   "  local  "  ,           host:   "  localhost  "  ,           port:   5432         },         vpc    });
```


Read more about the[dev](https://sst.dev/docs/component/aws/aurora#dev) config.


---


## Cost


Each ACU costs $0.12 per hour for both` postgres` and` mysql` engine. The storage costs $0.01 per GB per month for standard storage.


So if your database is constantly using 1GB of memory or 0.5 ACUs, then you are charged $0.12 x 0.5 x 24 x 30 or **$43 per month** . And add the storage costs to this as well.


If your database scales to 0 ACUs and is auto-paused, you are not charged for the ACUs.


Read more about the[cost of using Aurora](https://sst.dev/docs/component/aws/aurora#cost) .


---


## Examples


We also have a few examples that you can check out.


- [Aurora Postgres](https://sst.dev/docs/examples/#aws-aurora-postgres)
- [Aurora MySQL](https://sst.dev/docs/examples/#aws-aurora-mysql)
- [Aurora local](https://sst.dev/docs/examples/#aws-aurora-local)
