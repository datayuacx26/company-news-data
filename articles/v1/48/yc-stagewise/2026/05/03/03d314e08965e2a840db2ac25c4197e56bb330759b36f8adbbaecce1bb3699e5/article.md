---
schema_version: "1.0.0"
document_id: "03d314e08965e2a840db2ac25c4197e56bb330759b36f8adbbaecce1bb3699e5"
company_key: "yc-stagewise"
company: "stagewise"
source_id: "yc-stagewise-news-import-0e327623c986"
canonical_url: "https://stagewise.io/news/release-week-may-4-10"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-22T14:44:14.475585+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:e03d91601860680c928b6f901b0a439984aa058aa0f71467fcdc60ac4351671d"
---

# Release Week: May 4–10

## What Shipped Last Week


### Updated User Interface


The biggest change this week was the release of our updated UI, paving the way toward a more generalized yet more powerful agentic coding experience in stagewise.


We wrote more about that direction in[We're Becoming the Open Source Agentic IDE](https://stagewise.io/news/becoming-the-open-source-agentic-ide) . The short version: the browser-native workflow stays core to stagewise, but the product is expanding around it so agents can work across more of the codebase and the development process.


### Smart Approvals for safer autonomy


We also shipped **Smart Approvals** , a new approval mode for agent shell actions. It sits between "Always ask" and "Always allow": routine commands can run without interruption, while risky commands still require manual approval.


That makes agents more autonomous and safer at the same time. They can move faster through normal development work, but still stop before actions that look destructive, system-level, or out of scope. The full technical breakdown is available in[How Smart Approvals Work in stagewise](https://stagewise.io/news/smart-approvals-deep-dive) .


### Easier AWS inference setup


Users can now select AWS profiles stored on their machine when connecting stagewise to AWS inference. That makes setup smoother if you already manage AWS access through local profiles, instead of manually copying connection details into the app.


### MiniMax M2.7 support


We added support for **MiniMax M2.7** , giving users another current model option inside stagewise. This continues the broader push to make stagewise more open to the models and providers developers already want to use.


## What the Week Adds Up To


This was a week of making stagewise broader, faster, and easier to trust. The updated UI sets up the next phase of the product, while Smart Approvals make stagewise agents more autonomous.


We also shipped several smaller fixes across the app, including a random crash on close, attachment positioning in the input box, and issues with built-in diff history on deltas involving empty files.
