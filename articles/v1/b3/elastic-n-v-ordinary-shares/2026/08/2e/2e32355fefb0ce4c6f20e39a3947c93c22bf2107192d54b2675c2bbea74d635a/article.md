---
schema_version: "1.0.0"
document_id: "2e32355fefb0ce4c6f20e39a3947c93c22bf2107192d54b2675c2bbea74d635a"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/full-text-search-unindexed-data"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T22:34:05.631455+00:00"
fetched_at: "2026-08-05T22:34:06.828213+00:00"
content_hash: "sha256:2a8df1e4f2e0cbd53bf573c1976ecb2c6b764d84105036b7b4619a7dd749b69e"
---

# Elasticsearch ES|QL brings full-text search to data you never indexed

ES|QL` MATCH` now runs full-text search on data you never indexed. Computed columns, unmapped fields, strings assembled on the fly, even federated data sitting in S3. The new` TO_TEXT` function tells ES|QL to treat any string as analyzable text, so` MATCH` can tokenize, case-fold and term-match values that exist only for the lifetime of a query. This goes beyond the` LIKE` and` RLIKE` pattern matching that most query engines offer for unindexed strings: it's real analysis. Available now in Elastic Cloud Serverless and as a technical preview in Elasticsearch 9.5.


## How MATCH and TO_TEXT enable full-text search on any ES|QL expression


Let's start with a query that was impossible in Elasticsearch 9.4, which uses[the EVAL command](https://www.elastic.co/docs/reference/query-languages/esql/commands/eval) :


```text
FROM cooking_blog
| EVAL summary = TO_TEXT(CONCAT(title, description))
| WHERE MATCH(summary, "pancakes")
| KEEP title, author
```


In this example,` summary` has no mapping or analyzer configuration. It’s also not associated with any inverted index. It exists only for the lifetime of this query, but now you can search it anyway. Two additions make this work.


First,[MATCH](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/search-functions/match) now accepts any expression as its first argument, not just a mapped field. That includes columns produced by` EVAL` and function results used inline. It also includes unmapped fields loaded directly from the original document. Furthermore, all data types normally accepted by` MATCH` are supported in this new use case.


The second part of this is the new[TO_TEXT](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/type-conversion-functions/to_text) function, which is the first ES|QL conversion function that produces output of type` text` . Until now,` text` columns could only come from indexed mapped fields, and all strings produced by ES|QL expressions were` keyword` values rather than` text` . The distinction matters because` MATCH` treats the two differently:` text` values are analyzed, while` keyword` values are compared exactly, mirroring how a` MATCH` query on an indexed keyword field rewrites to a term query.` TO_TEXT(x)` is how you tell ES|QL: *treat this string as full text* .


This ships as a technical preview in Elasticsearch 9.5, and as such, it has some limitations:


-


It’s currently filtering only. A` MATCH` on an expression doesn't contribute to the relevance score yet; only matches on indexed fields affect the score.


-


Querying options like` fuzziness` and others aren't yet supported when matching an expression.


-


Runtime text is analyzed with the standard analyzer. This isn’t configurable yet.


Work is underway to address these limitations.


## Why use full-text search instead of LIKE or RLIKE in ES|QL?


ES|QL already had two ways to search strings without an index:` LIKE` (wildcard patterns) and` RLIKE` (regular expressions). Both work on any string expression, so it's fair to ask what` MATCH` adds. The answer is[analysis](https://www.elastic.co/docs/manage-data/data-store/text-analysis) , a more advanced form of search which uses techniques such as stemming and synonyms. It also uses stopword handling.


` LIKE` is simple substring matching, without any understanding of the words that comprise a string. Say, for example, you're looking for log messages about a fox:


```text
FROM app_logs
| WHERE message LIKE "*fox*"
```


This misses` "Fox spotted near the henhouse"` due to the capitalization, while matching` "Outfoxed by the competition"` , which isn't about a fox at all. It fails in both directions, with false negatives on capitalization and false positives on substrings buried inside other words.


Regular expressions can patch the case problem, but the word-boundary problem gets ugly fast. Something like:


```text
FROM app_logs
| WHERE message RLIKE "(.* )?[Ff][Oo][Xx]([ ,.:;].*)?"
```


And even that's not right yet. It misses a fox at the end of a sentence followed by` !` or` ?` , and it says nothing about tabs, quotes, or parentheses. Each fix makes the pattern longer, and the next person to read the query has to reverse-engineer what it’s actually doing.


` MATCH` makes the problem go away, because it runs both the query and the value through an[analyzer](https://www.elastic.co/docs/reference/text-analysis/analyzer-reference) , which tokenizes text into lowercase terms and then matches term against term:


```text
FROM app_logs
| WHERE MATCH(TO_TEXT(message), "fox")
```


This query will match values like` "The quick brown fox"` and` "FOX spotted near the henhouse"` but not` "Outfoxed by the competition"` or` “FOXTROT protocol enabled"` , regardless of any punctuation surrounding the words. Of course, this all works for multi-term queries, like` MATCH(TO_TEXT(message), "brown fox")` , too, just the way you’d expect it to.


Work is underway to enable the use of the[36 dedicated language analyzers](https://www.elastic.co/docs/reference/text-analysis/analysis-lang-analyzer) , with support for natural languages on data that was never indexed or mapped.


## Full-text search use cases for unindexed and unmapped data


The examples above searched values computed from[mapped fields](https://www.elastic.co/docs/manage-data/data-store/mapping) . . The more interesting use cases for ES|QL` MATCH` on expressions involve data that was never searchable at all. Let's walk through a few.


### How to search unmapped fields in ES|QL without adding a mapping


Sometimes you deliberately leave a field out of your mappings, such as a verbose stack trace or a raw request payload. You might even leave out a debug blob. Indexing one of these would cost disk and heap space on every document and wouldn’t be worth it for a field you might query once a quarter.


That decision has always been final, because[unmapped fields](https://www.elastic.co/search-labs/blog/esql-unmapped-fields) were invisible to queries entirely. In Elasticsearch 9.5, you can use` SET unmapped_fields="load"` to make ES|QL load unmapped fields directly from the source document as keywords. Follow that up by wrapping it in` TO_TEXT` , and now you can run full-text search on it:


```text
SET unmapped_fields="load";
FROM app_logs
| WHERE MATCH(TO_TEXT(stack_trace), "java.lang.NullPointerException")
| KEEP @timestamp, service.name, message
```


Here,` stack_trace` was never mapped. Every value is fetched from the original documents and analyzed on the fly. They’re matched row by row. That’s real work, and it will never be as fast as an[inverted index](https://www.elastic.co/docs/manage-data/data-store/index-basics) lookup. But now, that field you didn't index is no longer unsearchable. You get to keep the mapping small for the everyday case and still answer the once-a-quarter question when it matters.


### Full-text search on a keyword field without reindexing


Keyword fields can do a lot. They give you exact matching, fast aggregations, and sorting, which is why so many fields end up mapped that way. But mappings are decided when data arrives, and it’s easy to end up in a situation where you want to do something different with your data than you had originally intended. Maybe` product_name` was mapped as a` keyword` because the dashboards aggregate on it, and then, after receiving a year’s worth of product data, someone wants to be able to search within` product_name` values.


The old answer was to change the mapping to` text` (or add a multi-field) and reindex everything. This can be both time-consuming and costly, and in many cases, users simply won’t want to bother with it. The new answer is one function call:


```text
FROM products
| WHERE MATCH(TO_TEXT(product_name), "wireless noise cancelling headphones")
| KEEP product_name, brand, price
```


` TO_TEXT` converts the` keyword` values to` text` on the fly, so` MATCH` analyzes them instead of comparing them exactly. This allows you to query a` keyword` field without creating a mapping or reindexing the source document. If the search becomes an everyday query, indexing the field as` text` is still the right long-term move, but` TO_TEXT` gets you an answer today, without any extra work.


### Searching the same field across indices with different mappings


[ES|QL can span many indices](https://www.elastic.co/docs/reference/query-languages/esql/esql-multi-index) , and the same field doesn't need to always look the same in all of them. When the same field has different types in different indices, ES|QL treats it as a union type, and a conversion function resolves the conflict. Let’s consider an example in which the` message` field has type` text` in this year's index template but was a` keyword` in last year's:


```text
FROM logs-2025, logs-2026
| EVAL msg = TO_TEXT(message)
| WHERE MATCH(msg, "connection reset")
```


Every value is analyzed at query time, whether it came from the` text` index or the` keyword` one. The keyword values from the older indices are tokenized and lowercased like everything else, so "connection reset" finds “Connection RESET by peer”, no matter which index it lives in.


Another interesting case is when a field is mapped in only one index but also present (and unmapped) in the other:


```text
SET unmapped_fields="load";
FROM logs-2025, logs-2026
| WHERE MATCH(TO_TEXT(error_details), "timeout")
```


There's a nuance worth calling out here. If` error_details` is mapped in` logs-2026` but not in` logs-2025` , Elasticsearch cannot push this query down to[Lucene](https://lucene.apache.org/) , because the indices where the field is unmapped would silently return no matches. Instead, the planner notices that the field is potentially unmapped and evaluates the whole` MATCH` row by row, wherever the rows came from. You don't have to know which of your indices have the field mapped; the query just answers the question.


## How ES|QL analyzes text at query time without an inverted index


When ES|QL plans a` MATCH` against an expression, it analyzes the query string once, up front, into a set of terms. How each row is then evaluated depends on the expression's type:


**Expression type**


**Processing**


**Matching behavior**


` text` (via` TO_TEXT` )


Analyzer tokenizes value into lowercase terms


Token-against-token comparison; a row matches if any token equals any query term (` OR` semantics)


` keyword` ,` ip` ,` date` , numeric


No analysis; query constant converted once to the native type


Exact comparison per row


Both paths bypass Lucene entirely and evaluate values row by row. The non-text path mirrors exactly what a match query does when pushed down to Lucene against those field types, so the semantics stay consistent regardless of whether your query hits an index.


An inverted-index lookup does its work at ingest time and never touches non-matching documents at query time. A runtime` MATCH` does that analysis at query time, for every row that reaches it. One is fast because the work already happened; the other is flexible because the data doesn't need to have been indexed at all.


## What's next for ES|QL full-text search


Everything in this post is the first installment of a larger effort to make search in ES|QL work on anything, not just on what you indexed ahead of time. The limitations called out earlier are actively being worked on, and the roadmap goes further:


-


**Scoring.** Runtime matches will contribute to` _score` , so you can sort by relevance even when the data was never indexed.


-


` **MATCH_PHRASE**` **on expressions.** Already available in Elastic Cloud Serverless, and coming to the Elastic Stack in 9.6.


-


**Configurable analyzers.** Analyzer support for` MATCH` and` MATCH_PHRASE` on expressions, enabling language analyzers, stemming, and synonyms at query time.


-


**Match options.** Options like` fuzziness` and` operator` for runtime matches.


-


**Vector search.** Generating embeddings per row and running k-nearest neighbors (kNN) on runtime` dense_vector` expressions, bringing semantic search to unindexed data, too.


## Try ES|QL full-text search on expressions today


You can try runtime search today. It's available now in Elastic Cloud Serverless, where new ES|QL capabilities land first, and it ships as a technical preview in Elasticsearch 9.5. Start with the[search functions](https://www.elastic.co/docs/reference/query-languages/esql/functions-operators/search-functions) reference, and check the[ES|QL limitations](https://www.elastic.co/docs/reference/query-languages/esql/limitations) page for the current boundaries. It's a technical preview because we want your feedback: If you search something that was never indexed and it surprises you, either positively or negatively,[we'd love to hear about it](https://www.elastic.co/community) .
