---
schema_version: "1.0.0"
document_id: "6185a1f2aa1ce7fed817b9d473567e197587a3680f65f28fb36a562e287e4f12"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/open-next-v1/"
published_at: "2023-04-17T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:4d2bc14d6778d6271dc1eee9821a832ff7314a8ac831cf1623ad2fc2f2abeddb"
---

# OpenNext 1.0

OpenNext 1.0 is out. You can try it now with the` NextjsSite` construct.


## What is OpenNext


[OpenNext](https://open-next.js.org/) is an open-source Next.js serverless adapter created by the SST team. It allows you to self-host Next.js *serverlessly* .


To celebrate the 1.0 release, we created a fun video.


[Play](https://youtube.com/watch?v=k_8lBdPC3gk)


## Background


Next.js does not support self-hosting your app using serverless. There have been several projects to try and fix this. However, this is not easy to do, as it requires reverse engineering how Vercel deploys Next.js internally. As a result most of these attempts have failed or have ended up as closed source paid implementations.


The goal of OpenNext is to create a free open source, framework agnostic, serverless adapter for Next.js.


## OpenNext 1.0


OpenNext was created back in December, 2022. Since then the repo has grown to over 1000 stars on GitHub. The community has really come together to ensure that OpenNext supports all of Next.js 13’s features. The OpenNext channel is also one of the most active channels on our Discord.


While OpenNext is already being used in production, the[1.0 release](https://github.com/sst/open-next/releases/tag/v1.0.0) marks a significant milestone in terms of stability and performance.


We’d love for you to take it for a spin.


## Get started


Here’s how you can use OpenNext to deploy your Next.js app to AWS with SST.


Terminal window


```text
npx     create-next-app    npx     create-sst    npx     sst     deploy
```


[Check out the full tutorial](https://sst.dev/docs/start/aws/nextjs/) . If you are using a different framework, check out the[other deployment options](https://github.com/sst/open-next/blob/main/README.md#deployment) .


---


We could use your support in maintaining OpenNext, make sure to join` #open-next` on Discord and[star us on GitHub](https://github.com/sst/open-next) .
