---
schema_version: "1.0.0"
document_id: "fb66d28f7a692d01d3dfd791bea363f772d2116f2ec07f9e24c8d52cdf628fa7"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/introducing-magic-link"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:f883610869ab745e68258a4d2d6067e85bc6150f0ebf1ed4521a6e2349ea9ac7"
---

# Introducing Magic Link

Imagine if you could just send a link to your users to connect all their integrations… now you can using our new **Magic Link** feature 🪄


TL;DR: Instead of doing the eng work to setup auth workflows for each of your integrations (or embedding hotglue’s widget into your frontend), Magic Link enables you to create a completely white-labeled and shareable link with your users that prompts them to connect their integrations!


In the demo below I walk through all the white-labeling options we have available and how the Magic Link works, take a look:


## Why would you need Magic Link?


There are a lot of different scenarios where you may want to use a Magic Link instead of having your users connect their integrations directly in your frontend.


In some cases, the person managing the integration (for example Salesforce), isn’t the user of your product. Many orgs have dedicated Salesforce admins – instead of getting that admin into your product so they can connect Salesforce, you can simply give your user a Magic Link that they can forward to the relevant person in the org to authorize. Nice and simple!


Another common case is dealing with a user who needs a new integration, and you’re not ready to roll it out to everyone yet. For example, let’s say this user is on Dynamics Business Central or NetSuite, while most of your users are on QuickBooks Online. You still want the deal, so you generate a Magic Link with those “enterprisey” connectors, giving your Customer Success team an easy way to onboard that user.


The list goes on, but there are a lot of scenarios where a Magic Link can come in handy.


## How does it work?


Generating a Magic Link is really simple. All you need to define is a **tenant id** (a unique ID to identify the user that is going to connect) and you can optionally filter which flows or connectors you want to be visible for linking:


This gives you a unique link to share with the user, ready to go! Notice that the URL is whitelabeled in my example for my dummy company called “gluestick” (if you don’t whitelabel, it would be` connect.hotglue.com` ).


You can now share that link, and when the user accesses it they’ll be prompted to connect Salesforce:


## How customizable is the Magic Link?


You can customize the look and feel of the Magic Link in your hotglue environment settings: change the logo, favicon, descriptions, buttons, and even add a link to the help docs. The Magic Link will also inherit your widget customizations to set the primary color and more!


Of course the Magic Link wouldn’t be truly whitelabeled unless could customize the URL, which you can also do directly within your environment settings!


Thanks for reading! If you have any questions about this feature or want to try hotglue,[book a demo](https://hotglue.com/demo) .


TABLE OF CONTENTS


- Why would you need Magic Link?
- How does it work?
- How customizable is the Magic Link?


RECOMMENDED BLOGS


[Announcing hotglue’s $4M Seed Round Announcing our $4 million seed round led by 8VC with participation from Y Combinator, Correlation Ventures, and others.](https://hotglue.com/blog/announcing-hotglues-4m-seed-round)


[The Stigma of Embedded iPaaS Too many embedded iPaaS tools underperform, creating skepticism in the market. The negative stigma isn't impacting hotglue. Read on to learn why.](https://hotglue.com/blog/the-stigma-of-embedded-ipaas)


[hotglue Leads The Embedded Integration Category G2 released their latest badges and hotglue leads in multiple categories.](https://hotglue.com/blog/hoglue-leads-the-embedded-integration-category)
