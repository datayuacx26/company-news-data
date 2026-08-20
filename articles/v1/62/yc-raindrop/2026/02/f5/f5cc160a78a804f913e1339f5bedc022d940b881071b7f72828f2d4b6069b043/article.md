---
schema_version: "1.0.0"
document_id: "f5cc160a78a804f913e1339f5bedc022d940b881071b7f72828f2d4b6069b043"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/announcing-the-query-sdk/"
published_at: "2026-02-17T00:00:00+00:00"
first_seen_at: "2026-07-22T10:48:02.200992+00:00"
fetched_at: "2026-07-28T22:19:59.143850+00:00"
content_hash: "sha256:57b107ec4f52e853f581249d63a271ff6c6f28df30776d758104be5e6b94a503"
---

# Announcing the Query SDK

Today we're releasing the Query SDK. A powerful, agent-native toolkit for your AI observability data, available in both TypeScript and Python.


Most observability platforms give you a query language. Some give you SQL. Others give you a proprietary DSL that looks like SQL but isn't. The Query SDK is something different: a complete toolkit for building dashboards, Claude Code skills, and automated pipelines on top of your production AI data.


[Read the docs to get started.](https://www.raindrop.ai/docs/query-api/sdk)


> "The Query SDK enables us to close the feedback loop, quickly turning chat errors into Linear tickets."
>
>
> Brian Rhindress
>
>
> Senior Engineer at GC.AI


## More than a query language


Traditional trace stores and data warehouses, like Langfuse and LangSmith, offer query capabilities that are fundamentally just filters. You can slice data by timestamp, model, or metadata. You can count things. You can export CSVs.


That's fine if all you need is a dashboard. But teams building serious AI products need more. They need to


*understand* what's happening in production, not just count it. And they need to automate ways to close the loop between what they observe and what they fix.


The Query SDK gives you:


-


**Signals** to programmatically access the signals you've configured in Raindrop. Instantly track user frustration, link users to internal dashboards, and build custom alerting or triage workflows.


-


**Semantic search** across user inputs and assistant outputs. Find conversations where users expressed frustration about a topic, not just conversations that contain a keyword.


-


**Timeseries analysis** with flexible bucketing (minute, hour, day, week, month) to spot trends and regressions the moment they appear.


## Signals


Raindrop's signal system is what separates it from a traditional data warehouse. Signals are how you define what matters in your product, and the Query SDK gives you full programmatic access to all of them.


typescript


```text
const     frustrationSignalId     =     "  d2f3a1b8-4e7c-4a91-b6d0-9f8e7c6d5a4b  "  ;
const     frustration     =     await     client  .  events  .  count  (  {
filters  :     {     signal  :     frustrationSignalId     }  ,
}  )  ;
const     trend     =     await     client  .  events  .  timeseries  (  {
bucket  :     "  hour  "  ,
filters  :     {     signal  :     frustrationSignalId     }  ,
}  )  ;
```


This makes it possible to build things that traditional query languages simply can't support. Automated triage pipelines that route issues the moment they're detected. Regression detectors that compare signal distributions before and after a deployment. Custom alerting that triggers when a specific signal spikes in a specific user cohort.


Raindrop already has built-in alerting. But the Query SDK lets you go further, integrating Raindrop's semantic analysis with the traditional tools you know and love. Here's an example that sends a Sentry alert whenever a frustration signal spikes:


typescript


```text
const     baseline     =     await     client  .  events  .  count  (  {
filters  :     {     signal  :     frustrationSignalId     }  ,
start  :     "  24h_ago  "  ,
end  :     "  1h_ago  "  ,
}  )  ;
const     lastHour     =     await     client  .  events  .  count  (  {
filters  :     {     signal  :     frustrationSignalId     }  ,
start  :     "  1h_ago  "  ,
}  )  ;
if     (  lastHour  .  count     >     baseline  .  count     /     24     *     3  )     {
Sentry  .  captureMessage  (
`  Frustration signal 3x above baseline (  ${  lastHour  .  count  }   events in the last hour)  `  ,
"  warning  "
)  ;
}
```


## Semantic search


One of the most important capabilities in the SDK is semantic search. Instead of writing regex patterns or hoping users typed the exact keyword you're looking for, you can search by


*meaning* .


typescript


```text
import     {     RaindropQuery     }     from     "  @raindrop-ai/query  "  ;
const     client     =     new     RaindropQuery  (  {
apiKey  :     process  .  env  .  RAINDROP_QUERY_API_KEY  ,
}  )  ;
const     results     =     await     client  .  events  .  search  (  {
query  :     "  user confused about how to export their data  "  ,
mode  :     "  semantic  "  ,
searchIn  :     "  user_input  "  ,
}  )  ;
```


This finds conversations where users were confused about exporting, even if they never used the word "export" or "confused." Under the hood, we use quantized vector embeddings and a custom scoring function to rank results by relevance, not just string similarity.


You can search across assistant outputs too. Find cases where your agent gave incorrect instructions, hallucinated, or went off-track.


## Getting started


Install the SDK:


bash


```text
npm install @raindrop-ai/query
```


bash


```text
pip install raindrop-query
```


Get a Query API key from your Raindrop dashboard and start building.


python


```text
from     raindrop_query     import     RaindropQuery
client     =     RaindropQuery  (  api_key  =  "  your-api-key  "  )
count     =     client  .  events  .  count  (
filters  =  {  "  signal  "  :     "  d2f3a1b8-4e7c-4a91-b6d0-9f8e7c6d5a4b  "  }
)
similar     =     client  .  events  .  search  (
query  =  "  agent stuck in a loop  "  ,
mode  =  "  semantic  "  ,
search_in  =  "  assistant_output  "  ,
limit  =  20  ,
)
```


Check out the


[full documentation](https://www.raindrop.ai/docs/query-api/sdk) to see everything the SDK can do.
