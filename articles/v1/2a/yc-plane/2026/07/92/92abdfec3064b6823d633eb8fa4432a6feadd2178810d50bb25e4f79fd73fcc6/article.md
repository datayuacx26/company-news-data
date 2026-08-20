---
schema_version: "1.0.0"
document_id: "92abdfec3064b6823d633eb8fa4432a6feadd2178810d50bb25e4f79fd73fcc6"
company_key: "yc-plane"
company: "Plane"
source_id: "yc-plane-news-import-c2c9290ea736"
canonical_url: "https://plane.com/blog/sandbox"
published_at: "2026-07-10T20:37:44.169+00:00"
first_seen_at: "2026-07-23T20:33:38.186399+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:ab24984e43f016bef0a53555f9a8a9bf8a7d790c0b767e12044ee9d6cdbefeef"
---

# Sandbox: A safe way to build and test payroll and HR automations

Payroll and HR should work like infrastructure: programmable, API-first, built to fit your workflows instead of dictating them. But anyone who’s set up a payroll or HR automation knows the scariest part is running it the first time against real data. One bad loop and you’ve paid 40 contractors twice.


That’s why we built the new Plane sandbox, an isolated workspace where you can run anything against the full Plane API. Onboard contractors, send payments, push time entries, and more — without moving real money or affecting real people. Experiment confidently.


## What the Plane API covers


The[Plane API](https://docs.plane.com/) is included for free for every plan level, with no extra usage fees. We believe your data is yours, and you (and your agents) should be able to freely access and work with it.


What can you do with the API? Here are some common available tasks:


-


Onboard workers


-


Send payments and approve, decline, or pay payment requests


-


Submit, approve, decline, or cancel time off and leave requests


-


Create, update, and delete time entries


-


Receive real-time events from Plane via webhooks


For a full list of what you can do in the Plane API, see our[Working with the API](https://docs.plane.com/reference/introduction) documentation.


If there are any features you’re interested in that are not available, we’d love tohear from you . Our goal is to enable you and your agents to perform any action through the API that you can perform within the Plane platform, and we are constantly adding more features to Plane API.


## How the sandbox works


[Sandboxes](https://docs.plane.com/reference/sandboxes) are isolated workspaces you can use to test your Plane integration or try features without affecting real data. They act like regular workspaces in the UI and API, but with all side effects blocked.


-


Isolated data: You start with a clean workspace, without any live data copied into the sandbox


-


Non-billable: Sandbox activity doesn’t count toward billing


-


Protected side effects: Money movement, stateful providers (e.g., tracking of information), and outbound email delivery are disabled


-


Test credentials: Sandbox API keys can only access the sandbox they belong to


Sandboxes are available for admins. To create your sandbox, go to Developers > Sandboxes > Create sandbox. As with the API, sandboxes are included for free for every plan level.
