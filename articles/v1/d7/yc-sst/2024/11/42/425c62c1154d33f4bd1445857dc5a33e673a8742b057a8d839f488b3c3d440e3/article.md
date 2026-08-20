---
schema_version: "1.0.0"
document_id: "425c62c1154d33f4bd1445857dc5a33e673a8742b057a8d839f488b3c3d440e3"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/container-support/"
published_at: "2024-11-08T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:e3a918a8b8b79d91904f03e36d71dd7274ab45c4a5547562546d4a65c022afdf"
---

# Container support

Historically, SST has primarily supported deploying serverless applications. But over the last month we’ve slowly expanded native support for containers on AWS.


[Play](https://youtube.com/watch?v=sg4CnvoI7eg)


This includes changes across the entire SST platform.


---


### 1. Components


There’s a new family of components that’ll help you build with containers.


-


[Cluster](https://sst.dev/docs/component/aws/cluster) &[Service](https://sst.dev/docs/component/aws/service)


These help you deploy your containerized applications to AWS using ECS and Fargate.


sst.config.ts


```text
const   cluster   =   new     sst  .  aws  .  Cluster  (  "  MyCluster  "  , {   vpc   }  );
new   sst  .  aws  .  Service  (  "  MyService  "  , {         cluster,         loadBalancer: {           ports: [{ listen:   "  80/http  "  , forward:   "  3000/http  "   }],         },         dev: {           command:   "  npm run dev  "  ,         },    });
```


In addition to configuring ECS and Fargate, this also configures[service discovery](https://x.com/jayair/status/1853848336538673606) for your applications.


-


[Vpc](https://sst.dev/docs/component/aws/vpc)


Container applications are usually deployed in a VPC. So this component makes it easy to create a VPC. And optionally add a bastion host or a NAT gateway.


sst.config.ts


```text
new   sst  .  aws  .  Vpc  (  "  MyVpc  "  , { bastion:   true  , nat:   "  managed  "   });
```


-


[Postgres](https://sst.dev/docs/component/aws/postgres) ,[Redis](https://sst.dev/docs/component/aws/redis) , &[Efs](https://sst.dev/docs/component/aws/efs)


While these components are not specifically for containers, they’ve been designed to work well with the above` Cluster` and` Vpc` components.


---


#### Cost


Unlike our serverless components, that are pay-per-use, these components have a more traditional pricing structure. We’ve taken special care to ensure that these components are as cost effective as possible to get started with. While still allowing you to scale with them.


Unfortunately, AWS’ pricing pages for these services is not great. So the above components have a new *Cost* section in their docs. For example, here’s what the[cost of using the Vpc component looks like](https://sst.dev/docs/component/aws/vpc#cost) .


You can[read more about what we’ve done here](https://x.com/jayair/status/1851019182122652125) .


---


### 2. CLI


There are two big things we’ve done with our CLI to support containers.


1.


The` dev` prop allows you to run your application locally in a new tab in the` sst dev` multiplexer.


2.


The new[sst tunnel](https://sst.dev/docs/reference/cli#tunnel) command allows your local machine to connect to resources that’ve been deployed in a VPC. This is helpful because most of the container related components need a VPC. You can[check it out in action here](https://x.com/jayair/status/1844055259729007084) .


---


### 3. Console


The[SST Console](https://sst.dev/docs/console) now shows you logs for your containers. And[Autodeploy](https://sst.dev/docs/console#autodeploy) will support running in the same VPC as your app. This will allow your deploy process to have access to all the resources in your app.


---


## Get started


We’ve updated all our tutorials to help you get started with the new containers.


- [Bun](https://sst.dev/docs/start/aws/bun)
- [Nuxt](https://sst.dev/docs/start/aws/nuxt)
- [Solid](https://sst.dev/docs/start/aws/solid)
- [Deno](https://sst.dev/docs/start/aws/deno)
- [Hono](https://sst.dev/docs/start/aws/hono)
- [Astro](https://sst.dev/docs/start/aws/astro)
- [Remix](https://sst.dev/docs/start/aws/remix)
- [Svelte](https://sst.dev/docs/start/aws/svelte)
- [Next.js](https://sst.dev/docs/start/aws/nextjs)
- [Drizzle](https://sst.dev/docs/start/aws/drizzle)
- [Prisma](https://sst.dev/docs/start/aws/prisma)
- [Express](https://sst.dev/docs/start/aws/express)


The frontends now support deploying to both serverless and containers.


---


## What’s next


Over the next few weeks we’ll extend support to other languages and frameworks. Like Rails, Laravel, Python, Elixir, Go, and more.
