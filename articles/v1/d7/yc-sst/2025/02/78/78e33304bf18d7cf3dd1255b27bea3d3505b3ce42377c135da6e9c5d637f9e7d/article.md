---
schema_version: "1.0.0"
document_id: "78e33304bf18d7cf3dd1255b27bea3d3505b3ce42377c135da6e9c5d637f9e7d"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/update-permalinks/"
published_at: "2025-02-18T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:1c009d85096c4d1d351372da1a7e24fb1a856d3151ef8a9061b99ed6cc60aa52"
---

# Update Permalinks

Moving forward, every update made to an SST v3 app will get a unique URL; a ***permalink*** . This is printed out by the SST CLI.


sst deploy


```text
↗      Permalink       https://sst.dev/u/318d3879
```


These permalinks redirect to a page in the Console.[Check out a short video](https://x.com/thdxr/status/1891595772937863283) of this in action.


Here you can view:


1. Full list of **all the resources** that were modified
2. Changes in their **inputs and outputs**
3. Any Docker or site **builds logs**
4. **CLI command** that triggered the update
5. **Git commit** , if it was an auto-deploy


You’ll need to have your AWS account connected to the Console to view these details.


---


### How it helps


The permalinks are useful for.


1. **Debugging deploys** : The changes in the inputs of a resource let you see if the changes to your` sst.config.ts` have been applied correctly.
2. **Speeding up deploys** : You can check which resources are taking long. Or why a certain resource is being updated in the first place.
3. **Sharing with your team** : Say you run into an error while running` sst dev` locally; now you can send this link to a teammate. And they can view the full event log instead of asking you to send them the log file from your local machine.


---


### How it works


In addition to the state updates, the SST CLI also uploads the event log from every update to the[S3 state bucket](https://sst.dev/docs/state) in your AWS account. It also generates a globally unique id for the update.


If your AWS account is connected to the Console, it pulls this state and generates the details for the update permalink. This also means that the Console can only show you updates after your AWS account has been connected.


When you visit the permalink, the Console looks up the id of the update and redirects you to the right app in your workspace.


---


You can learn more about[updates](https://sst.dev/docs/console/#updates) in the Console and how[state](https://sst.dev/docs/state) works over on the[docs](https://sst.dev/docs) .
