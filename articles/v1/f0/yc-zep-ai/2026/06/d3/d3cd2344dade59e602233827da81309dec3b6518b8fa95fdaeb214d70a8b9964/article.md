---
schema_version: "1.0.0"
document_id: "d3cd2344dade59e602233827da81309dec3b6518b8fa95fdaeb214d70a8b9964"
company_key: "yc-zep-ai"
company: "Zep AI"
source_id: "yc-zep-ai-rss-0db2565bfdf1"
canonical_url: "https://blog.getzep.com/the-batch-api-load-large-datasets/"
published_at: "2026-06-09T15:20:48+00:00"
first_seen_at: "2026-07-24T08:02:00.467869+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:f74f30fbf10313d9750513101a1da31492114d3c0dda884a28df4ec50147e408"
---

# The Batch API: Load Large Datasets into Agent Memory

Today we're shipping the **Batch API** , the new way to load large datasets into agent memory. It ingests data faster than sending the same operations one at a time, runs in batches of up to 50,000 items, tracks progress in a new dashboard, and stays separate from real-time ingestion. You create a batch, add your items across any graphs, users, and threads, then start processing. The Batch API replaces our earlier batch methods and is available to all Zep customers starting today.


## What the Batch API is


The[Batch API](https://help.getzep.com/adding-batch-data?ref=blog.getzep.com) is the recommended way to load large datasets into your[Context Graphs](https://help.getzep.com/graph-overview?ref=blog.getzep.com) : a backfill, a migration from another system, a document collection, or any other bulk import. Instead of calling` graph.add` or` thread.add_messages` once per item, you group everything into one batch and hand it to Zep as a single job. A batch follows a three-step lifecycle: create an empty batch, add items to it, then start processing. Items can be graph episodes or thread messages, and one batch can target any number of graphs, users, and threads.


## Why it matters


The Batch API ingests large datasets faster than the same operations sent one at a time, and a single batch holds up to 50,000 items, enough to submit a full migration as one job. Its processing runs separately from real-time ingestion, so a big backfill won't affect ongoing ingestion.


You also get visibility into how that work is going. The new batch dashboard shows every batch in your project: its status, a live progress bar, processed-item counts, and any per-item errors. When a batch completes, Zep can send a[webhook](https://help.getzep.com/webhooks?ref=blog.getzep.com#batch-api-payload) .


The batch dashboard: every batch in your project with its status, progress, processed-item counts, and completion time.


## How to use it


Create a batch, add your items across one or more calls (up to 500 per call, 50,000 per batch), then start processing. Zep returns immediately and ingests the batch asynchronously, grouping items by destination graph and processing them in the order you added them.


```text
# Create a batch
batch = client.batch.create(metadata={"description": "Customer support backfill"})


# Add items: graph episodes and thread messages, any destination
client.batch.add(batch_id=batch.batch_id, items=[
BatchAddItem(type="graph_episode", user_id="alice",
data="Alice signed up for the Pro plan on 2024-06-15.", data_type="text"),
BatchAddItem(type="thread_message", thread_id="alice_support_thread_42",
content="My dashboard isn't loading.", role="user", name="Alice"),
])


# Start processing (Zep ingests the batch asynchronously)
client.batch.process(batch_id=batch.batch_id)


```


When you're loading historical data, set a` created_at` timestamp on each item. Zep uses it to track when facts extracted from that data are valid or invalid.


## Getting started


To get started, see the[Batch API docs](https://help.getzep.com/adding-batch-data?ref=blog.getzep.com) . The Batch API is available to all Zep customers starting today. It replaces the older[graph.add_batch and thread.add_messages_batch](https://help.getzep.com/adding-batch-data?ref=blog.getzep.com#deprecated-batch-methods) methods, which are now deprecated and will be removed in a future release.
