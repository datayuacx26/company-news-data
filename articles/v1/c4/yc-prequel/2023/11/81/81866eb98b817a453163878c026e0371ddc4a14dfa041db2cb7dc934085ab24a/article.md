---
schema_version: "1.0.0"
document_id: "81866eb98b817a453163878c026e0371ddc4a14dfa041db2cb7dc934085ab24a"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/schema-evolution/"
published_at: "2023-11-15T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:195f1ad6a27a82d299ace4e01a3f3cd55c3998a5e6d472988f55cd9cd7bd750c"
---

# Major Feature Release: Schema Evolution

As you add features to your application, the shape of data in your database is likely to change. You’ll add new columns, and deprecate old ones – it’s the circle of life of the db world. If you’re syncing data to other places (your customer’s data warehouse, for example), those changes need to be propagated there too.


Let’s make this concrete: say you offer a payment product, and you have a table called *transactions* . This table records *date* , *amount* , and *merchant* .


Now, folks on your engineering team do a bunch of hard work, and they’re able to tag each merchant with a category like food or apparel or airline. This is useful data to keep, so they add a column to the table called *category* . Of course, your customers want to get this category data too.


## Why it’s hard to build in-house


Propagating those changes to your customer’s warehouses is a thorny problem. First, you need to detect that a change has happened on the source side. Then, you need to check the state of existing destinations. And, in relevant cases, you need to propagate the change (ie create the new column) there. You need to be diligent about types while doing: this new column might be of type *text* in Snowflake, *varchar* in Redshift, and *string* in BigQuery.


These steps need to happen before you run any new data transfers: otherwise, the destination will complain that you’re trying to insert data into a column that doesn’t exist.


And then, assuming you got these basic steps right, you get into some fun edge-cases. What happens if the destination is offline and non-reachable when you try to propagate this schema change? Will syncs be broken moving forward or is your evolution engine capable of recovering gracefully? In that same offline database scenario, what happens if you add two columns in a row? How will you keep track of the last known state of the destination so that you can evolve it appropriately?


## Our solution


We’ve built all this functionality into the Prequel platform, so that you don’t have to. Whenever you make a change to your source config file, this change is automatically propagated to all your destinations. If you add columns, we’ll add them seamlessly. If you remove columns, we’ll flag them as deprecated in the destination. This allows your customers to leverage the new functionality without ever having to deal with a breaking change in their data pipeline. We handle the edge cases mentioned above and a whole host of other ones too.


You can read about the exact functionality[here](https://docs.prequel.co/docs/schema-evolution) . The feedback we’ve gotten from customers is that it feels a little bit like magic. To us, that’s the best compliment there is. If you’d like to learn more about the feature, or otherwise chat about how we could power your data exports and save you heaps of engineering time and headache, drop us a note.
