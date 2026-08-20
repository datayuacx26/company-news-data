---
schema_version: "1.0.0"
document_id: "10baeab2ede04370a9d204f7fd69f820b5feccc68638653c5e1e7949197a72dd"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-feedback-and-feedback-analytics"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T10:43:55.310513+00:00"
fetched_at: "2026-08-04T11:44:39.476301+00:00"
content_hash: "sha256:e686a91f834306b91c6100e4f850c2d6fcc745b5e8b0d216dac97b7e2d48db12"
---

# Introducing Agent Feedback and Feedback Analytics

You can now capture human[feedback](https://mastra.ai/docs/observability/feedback) — thumbs, ratings, comments, or corrections — on any agent response. Tag each record by source (` user` ,` SME` , or` qa` ) so you can visualize, filter and compare later.


Every feedback record can be anchored to a` threadId` ,` traceId` , or` spanId` . Reviewers can see agent responses in the context of a conversation and grade each one accordingly.


Feedback records can be queried per agent — average rating, thumb split, or comment volume. Compare user ratings versus QA ratings, and open the trace in Studio to inspect the model calls, tool calls, and outputs behind any feedback record.


Your browser does not support the video tag.


Before feedback, ratings and reviewer comments lived in external storage with no link back to the trace that produced them. Now they sit alongside spans, metrics, and logs in the same observability store, and can be forwarded to[PostHog](https://mastra.ai/reference/observability/tracing/exporters/posthog) ,[Braintrust](https://mastra.ai/reference/observability/tracing/exporters/braintrust) ,[Arize](https://mastra.ai/reference/observability/tracing/exporters/arize) , and other supported exporters.


## Get started


Install Mastra observability and and storage backends for memory and observability:


Terminal


```text
npm   install   @mastra/core   @mastra/observability   @mastra/libsql   @mastra/duckdb
```


note


Requires` @mastra/core@1.18.0` or later, added in[PR #14842](https://github.com/mastra-ai/mastra/pull/14842) .


Feedback lives in the observability domain. The example below routes the observability domain to DuckDB with default storage on LibSQL using a[MastraCompositeStore](https://mastra.ai/reference/storage/composite) :


src/mastra/index.ts


```text
import   {   Mastra   }   from   "  @mastra/core/mastra  "  ;
import   {   MastraCompositeStore   }   from   "  @mastra/core/storage  "  ;
import   {   LibSQLStore   }   from   "  @mastra/libsql  "  ;
import   {   DuckDBStore   }   from   "  @mastra/duckdb  "  ;
import   {   Observability,   MastraStorageExporter   }   from   "  @mastra/observability  "  ;


export   const   mastra   =   new   Mastra  ({
agents: {
/* ... */
},
storage:   new   MastraCompositeStore  ({
id:   "  composite-storage  "  ,
default:   new   LibSQLStore  ({ url:   "  file:./mastra.db  "   }),
domains: {
observability:   await   new   DuckDBStore  ().  getStore  (  "  observability  "  )
}
}),
observability:   new   Observability  ({
configs: {
default: {
serviceName:   "  mastra-feedback  "  ,
exporters: [  new   MastraStorageExporter  ()]
}
}
})
});
```


## Capturing feedback


Feedback records are available via` mastra.observability` , which publishes events through Mastra's[observability pipeline](https://mastra.ai/docs/observability/overview) : your span processors run, the record is enriched with context pulled from the trace, and every exporter you've configured receives the record.


```text
import   {   mastra   }   from   "  ../src/mastra  "  ;


if   (  !  mastra.observability.addFeedback)   {
throw   new   Error  (  "  Feedback not supported.  "  );
}


await   mastra.observability.  addFeedback  ({
traceId,
feedback: {
feedbackType:   "  rating  "  ,
value:   4  ,
feedbackSource:   "  qa  "  ,
comment:   "  Solid answer, would ship as-is.  "  ,
metadata: { messageId }
}
});
```


### List feedback


Read raw feedback records back from the store. Filter by any anchor or context field —` threadId` ,` traceId` ,` spanId` ,` feedbackType` ,` feedbackSource` ,` entityName` — and paginate.


```text
const   {   feedback   }   =   await   observability  !  .  listFeedback  ({
filters: { threadId },
pagination: { perPage:   100   }
});
```


### Query analytics


Numeric feedback (` rating` , or thumbs coded as` 1` /` -1` ) queries through the same OLAP surface as metrics. Aggregate with an optional period-over-period comparison:


```text
const   avg   =   await   observability  !  .  getFeedbackAggregate  ({
feedbackType:   "  rating  "  ,
aggregation:   "  avg  "  ,
comparePeriod:   "  previous_week  "
});
// → { value: 4.2, previousValue: 3.8, changePercent: 10.5 }
```


Break down by any dimension —` feedbackSource` ,` entityName` ,` environment` :


```text
const   bySource   =   await   observability  !  .  getFeedbackBreakdown  ({
feedbackType:   "  rating  "  ,
groupBy: [  "  feedbackSource  "  ],
aggregation:   "  avg  "
});
// → { groups: [{ dimensions: { feedbackSource: "admin" }, value: 4.1 }, ...] }
```


Bucket by interval for time series:


```text
const   overTime   =   await   observability  !  .  getFeedbackTimeSeries  ({
feedbackType:   "  rating  "  ,
interval:   "  1h  "  ,
aggregation:   "  avg  "
});
// → { series: [{ name: "rating", points: [{ timestamp, value }, ...] }] }
```


Same shape for` getFeedbackPercentiles({ percentiles: \[0.5, 0.95\], interval: "1d" })` .


### From the browser


Every method above (excluding` addFeedback` ) is also available using the[Mastra client SDK](https://mastra.ai/docs/server/mastra-client) :


```text
import   {   MastraClient   }   from   "  @mastra/client-js  "  ;


export   const   client   =   new   MastraClient  ({   baseUrl:   "  http://localhost:4111  "   });


const   {   feedback   }   =   await   client.  listFeedback  ({
filters: { threadId },
pagination: { perPage:   100   }
});
```


For more information and full configuration options, see:


- [Feedback overview](https://mastra.ai/docs/observability/feedback)
- [Feedback reference](https://mastra.ai/reference/observability/feedback)
- [Composite storage](https://mastra.ai/reference/storage/composite)
- [Mastra Client SDK — observability API](https://mastra.ai/reference/client-js/observability#feedback)
