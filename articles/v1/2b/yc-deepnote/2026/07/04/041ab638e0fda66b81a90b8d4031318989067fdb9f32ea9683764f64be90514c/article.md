---
schema_version: "1.0.0"
document_id: "041ab638e0fda66b81a90b8d4031318989067fdb9f32ea9683764f64be90514c"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/codex-for-data-analytics-powered-by-deepnote"
published_at: null
first_seen_at: "2026-07-26T08:11:39.657869+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:403b079bce26c31f2bdc2b8b155e3e31953528b78f291cfc3240193c18b7e0f4"
---

# Codex for data analytics, powered by Deepnote

[← Back to all posts](https://deepnote.com/blog)


Today, OpenAI launched the Codex data analytics plugin with a native connection to Deepnote. The business context, stored in your Deepnote workspace - the notebooks, scheduled analyses, data apps, and integrations your team already maintains in Deepnote is available to Codex directly, so any data explorations in Codex can start from your real work.


The data analytics plugin packages connectors for data tools like Deepnote, with workflows to diagnose metric movements, answer product and business questions, and turn the results into documents and reports. Adding Deepnote brings your trusted business context into those workflows without leaving the tools you already use.


Codex connects to your Deepnote workspace via MCP and acts within the connected user’s existing permissions: it can search projects, read and write notebooks, run them, and inspect integrations. A viewer key can read, an editor or admin key can build.


Here is what that unlocks.


## **Your workspace as the context for every exploration**


Most real questions cross team boundaries. "Why did activation drop last month?" usually touches marketing attribution, product retention, and the sales handoff, and the answer often already lives in someone's notebook.


With the Deepnote MCP, Codex can search and read across all projects in your workspace. So instead of rebuilding an analysis from scratch, it pulls from the marketing notebook, the sales pipeline notebook, and the product retention notebook, and composes a single narrative across all three. The reporting reflects how your team actually defines and measures things because it is built on the analyses your team has already written.


## **Turn a Codex exploration into a published Deepnote app**


Because Codex can write to Deepnote via MCP, an exploration need not end with a chat thread. Share it as a notebook or a published app your team can open, rerun, and extend.


## **Build and refine real workflows**


With Deepnote, you can build and iterate on persistent, production-ready workflows instead of relying on transient, one-off queries. Codex interacts directly with individual blocks in your Deepnote notebooks to create and refine these long-running pipelines.


For example, in a fraud detection and credit card cancellation workflow, you can use Codex to improve the system through several targeted iterations:


-


Add new features to the scoring model to increase accuracy.


-


Tune the scoring threshold based on recent transaction data.


-


Insert a monitoring block to track false positive rates over time.


Each update is executed through the MCP, resulting in a robust, automated workflow that continues to deliver value long after the initial conversation.


## **Connect Deepnote to Codex today**


1.


Create a workspace API key in Deepnote under **Settings & members > Security > API keys** .


2.


In Codex, request a connection with a prompt like connect to deepnote mcp via https://deepnote.com/mcp.


3.


Paste your Deepnote API key when Codex prompts for it.


4.


Restart the session so the tools load, then start prompting against your workspace.


Deepnote is available in the Codex data science plugin today. Try it now and let us know what you think!


## **Useful links**


-


[Deepnote plugin in Codex](https://chatgpt.com/plugins/share/449d4e5659914849bfdf87e5c6d51960)


-


[Deepnote MCP](https://deepnote.com/docs/deepnote-mcp)


-


[OpenAI launch post](https://openai.com/index/codex-for-every-role-tool-workflow/)


-


[openai/plugins](https://github.com/openai/plugins)


Jakub Jurovych


CEO @ Deepnote


Follow Jakub on[LinkedIn](https://www.linkedin.com/in/jakubjurovych/)
