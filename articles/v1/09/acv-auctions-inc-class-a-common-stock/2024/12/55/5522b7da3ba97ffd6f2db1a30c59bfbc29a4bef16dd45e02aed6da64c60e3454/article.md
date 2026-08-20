---
schema_version: "1.0.0"
document_id: "5522b7da3ba97ffd6f2db1a30c59bfbc29a4bef16dd45e02aed6da64c60e3454"
company_key: "acv-auctions-inc-class-a-common-stock"
company: "ACV Auctions Inc."
source_id: "acv-auctions-inc-class-a-common-stock-rss-d00f1f014d5a"
canonical_url: "https://acv.engineering/posts/elasticsearch-percolate/"
published_at: "2024-12-10T06:00:00+00:00"
first_seen_at: "2026-07-20T23:17:32.344046+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:ae2cba0f33293b922aa0746b6035008c5e694929fdc60a4ff20a91c2c2937708"
---

# Using Elasticsearch Percolate for User Notifications

## Introduction


The world of search and filtering is vast, and one of the most important concepts in it is “reverse filtering,” i.e. figuring out what set of filters would have matched a particular result. This is sometimes called Percolate, i.e. the opposite of filtering. The most common business use case for this is notifications - for example, if users have their filters saved as saved searches, and something happens to a document they would have cared about, we want to notify them somehow.


At ACV, selling dealerships are able to launch a car auction, right from their mobile app or from the web app. Once the auction launches, we notify potential buyers whose saved searches match the auction. The buyers are able to control exactly how they want to be notified - via a push notification on their device, via text, via email or any combination of those.


This is where Elasticsearch comes in! Elasticsearch has Percolate defined as a built-in query type, giving out-of-the-box Percolate functionality.


## How To Use Elasticsearch’s Percolate


To use Percolate in Elasticsearch, you first need to define a percolator field. This field will store the query that would have matched the target document. You can store the percolator field in documents in the same index as the target document, or you can use a separate index. We will be using a separate index, since that allows for the performance to be tuned properly.


The first step will be to create our target document index, which has the documents for which we’ll want to find saved searches for. Let’s call it` auctions-percolate-blog` . Hopefully I’ve chosen a unique name, so you can follow along at home.


Create the index like this:


```text
PUT /auctions-percolate-blog
{
"mappings": {
"properties": {
"make": {"type": "keyword"},
"model": {"type": "keyword"},
"year": {"type": "keyword"},
"vin": {"type": "keyword"}
}
}
}


```


As you can see, we have 4 fields, all keyword types. Let’s store our document that has ID` auction-1` that look like this:


```text
PUT /auctions-percolate-blog/_doc/auction-1
{
"make": "Ford",
"model": "F-150",
"year": 2023,
"vin": "insert_vin_here"
}


```


Separately, we’ll have an index called` saved-searches-percolate-blog` . This index has the same fields as the target one, but also a percolate field type we’re calling` query` .


```text
PUT /saved-searches-percolate-blog
{
"mappings": {
"properties": {
"make": {"type": "keyword"},
"model": {"type": "keyword"},
"year": {"type": "keyword"},
"vin": {"type": "keyword"},
"query": {"type": "percolator"}
}
}
}


```


This index will have a document with ID` saved-search-1` , which looks like this:


```text
PUT /saved-searches-percolate-blog/_doc/saved-search-1
{
"make": "Ford",
"query": {
"bool": {
"filter": [
{
"term": {"make": "Ford"}
}
]
}
}
}


```


Now, we can run this Percolate Query:


```text
GET /saved-searches-percolate-blog/_search
{
"query": {
"percolate": {
"field": "query",
"index": "auctions-percolate-blog",
"id": "auction-1"
}
}
}


```


The result (just the` hits.hits` component):


```text
[
{
"_index": "saved-searches-percolate-blog",
"_id": "saved-search-1",
"_score": 0,
"_source": {
"make": "Ford",
"query": {
"bool": {
"filter": [
{
"term": {
"make": "Ford"
}
}
]
}
}
},
"fields": {
"_percolator_document_slot": [
0
]
}
}
]


```


## How It Works


Here is a quote from the[official Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-percolate-query.html#how-it-works) :


> At search time, the document specified in the request gets parsed into a Lucene document and is stored in a in-memory temporary Lucene index. This in-memory index can just hold this one document and it is optimized for that. After this a special query is built based on the terms in the in-memory index that select candidate percolator queries based on their indexed query terms. These queries are then evaluated by the in-memory index if they actually match.


Essentially, Elasticsearch pulls the auction document into memory, and goes through the` saved-searches-percolate-blog` index, and evaluates all the percolator (query) field’s queries against that document. This can use significant CPU cycles, depending on the size of the saved search index.


## Performance Considerations


In practice, as the saved search index scales up, it is difficult to maintain “real-time” performance. If the system needs to notify users in real-time, for example if a bid is placed and we want to notify users within 1-2 seconds so they can counter-bid, this is the wrong approach. Instead, it may be more useful to[use GraphQL](https://acv.engineering/posts/graphQL-subscriptions-in-go/) and have the user’s client subscribed to the updates they want. In our use case, we want to notify users of auction launch, as that works fine even with a 5-10 second delay.


-
-
-
-
