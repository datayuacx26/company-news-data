---
schema_version: "1.0.0"
document_id: "eebf6d90509fa6efb234b182a463d7614373970667bd56db8bf31ecb2a523bf9"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/new-sst-console/"
published_at: "2023-09-22T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:7ae006d330908e253054600295a156d6bcf924b92ca5574b168d276fee7d7978"
---

# New SST Console

The new SST Console is out. You can[learn more](https://sst.dev/docs/console/) about it over on our docs.


### What is the SST Console


The SST Console is a web based dashboard to manage your SST apps. With it you can invoke functions, debug issues, view logs, and manage all your apps with your team —[console.sst.dev](https://console.sst.dev/)


### Features


#### Logs


With the SST Console, you don’t need to go to CloudWatch to look at the logs for your functions. It also supports a couple of modes; *recent* , *live* , and *time intervals* .


#### Issues


The SST Console will automatically show you any errors in your Lambda functions in real-time. There is:


- **Nothing to setup** , no code to instrument
- **Source maps** are supported **automatically** , no need to upload
- **No impact on performance** or cold starts, since the functions aren’t modified


#### Local logs


When the Console starts up, it checks if you are running` sst dev` locally. If so, then it’ll show you real-time logs from your local terminal.


### Open source


The Console is built with SST, deployed with[Seed](https://seed.run/) , and you can[view the source on GitHub](https://github.com/sst/console) .


The codebase is also a good example of what a production SST app looks like.


### Your team and AWS accounts


You can create a workspace in the Console, invite your team, and connect all your AWS accounts. It’ll automatically discover your SST apps.


### Pricing


The SST Console pricing is based on the number of times the Lambda functions in your SST apps are invoked per month. It comes with a free tier of 1M invocations.


You can[read more about it over on the docs](https://sst.dev/docs/console/#pricing) .


### Old Console


The old SST Console is still accessible at[old.console.sst.dev](https://old.console.sst.dev/) . But we’ll be moving away from it in the future.


### Learn more


[Learn more about the new SST Console](https://sst.dev/docs/console/) . And if you have any questions, join us over on Discord.
