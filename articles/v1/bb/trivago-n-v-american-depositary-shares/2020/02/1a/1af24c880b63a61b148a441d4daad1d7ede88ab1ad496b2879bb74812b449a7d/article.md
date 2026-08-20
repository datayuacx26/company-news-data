---
schema_version: "1.0.0"
document_id: "1af24c880b63a61b148a441d4daad1d7ede88ab1ad496b2879bb74812b449a7d"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2020-02-11-betterurlsearchwithelasticsearch/"
published_at: "2020-02-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:9bd14c098b30368cb7d602fae338db36b337cd1bda64b833725a60b72810ed0e"
---

# Better URL Search with Elasticsearch

At trivago, we generate a huge amount of logs and we have our[own custom setup for shipping logs](https://tech.trivago.com/2016/01/19/logstash_protobuf_codec/) using mostly[Protocol Buffers](https://developers.google.com/protocol-buffers) . Eventually we end up with some fields in Elasticsearch (ES) that contain partial (or full) URLs. For instance, in our specific case we store the[query component](https://en.wikipedia.org/wiki/URL#Syntax) of the URL in a field called` query` and the[path component](https://en.wikipedia.org/wiki/URL#Syntax) in a field named` url_path` . Sample values for these fields could be:


```text
url_path:   "/webservice/search/hotels/43326/rates"


query:   "from_date=2020-06-01T00:00:00%2B02:00&to_date=2020-06-10T00:00:00%2B02:00&currency=EUR&room_type=9&room_0=2a&fixed_status=1"
```


We use the[ELK stack (Elasticsearch, Logstash, Kibana)](https://www.elastic.co/what-is/elk-stack) as the core of our logging pipeline. Since Elasticsearch’s primary use case was that of a search engine, it comes equipped with a diverse assortment of tools to process data. Searching on these URL-like texts is not the same as trying to search in a summary of a book. When a field is defined as` text` in Elasticsearch, it will apply the[Standard](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-standard-analyzer.html)[Analyzer](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-standard-analyzer.html) by default.


The Standard Analyzer uses the[Standard](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-standard-tokenizer.html)[Tokenizer](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-standard-tokenizer.html) , which provides grammar-based tokenization. Put simply: if the value of the field would be an English sentence written using ASCII characters, the tokenizer will split the text based on punctuation signs, spaces and some special characters (like` /` for instance).


This tokenizer works quite well for our` url_path` field:


```text


POST   _analyze
{
"tokenizer"  :   "standard",
"text"  :   "/webservice/search/hotels/43326/rates"
}


```


Producing the following list of tokens:


```text
[  "webservice"  ,   "search"  ,   "hotels"  ,   "43326"  ,   "rates"  ]
```


If we test the value of the` query` field against this tokenizer, we can see that it produces a lot of useless tokens:


```text
[
"from_date"  ,
"2020"  ,
"06"  ,
"01T00"  ,
"00"  ,
"00"  ,
"2B02"  ,
"00"  ,
"to_date"  ,
"2020"  ,
"06"  ,
"10T00"  ,
"00"  ,
"00"  ,
"2B02"  ,
"00"  ,
"currency"  ,
"EUR"  ,
"room_type"  ,
"9"  ,
"room_0"  ,
"2a"  ,
"fixed_status"  ,
"1"
]
```


Although it detected the` from_date` field, it fails to tokenize the value of the query parameters as a single token, which makes searching very difficult.


It is more likely for a user to want to find documents where` currency` is set to` EUR` or` room_type` equal to` 9` . Generalizing this means that the users are interested in matching on the key/value pairs present in the query string.


Let’s go over a couple of ways we could approach this.


We could pre-process the data and make our Logstash pipeline split the data into multiple fields (using the[kv filter](https://www.elastic.co/guide/en/logstash/current/plugins-filters-kv.html) for instance). Creating a new field for each attribute in the query string could lead to a cardinality explosion in our indexes, considering that any user could create random key/value pairs.


We could work around the cardinality issue by flattening the structure and having a couple of nested fields (` name` and` value` ):


- ` query.name` the field that could hold the attribute name
- ` query.value` that would hold the value


This approach would introduce yet another problem. The` query` field would have to be an array of objects and as such, it would lead to queries matching in unexpected ways. Let me explain:


If we have the following values for our` query` field (as an array of objects) in a couple of documents:


```text
"query"  : [
{   "name"  :  "currency"  ,   "value"  :  "EUR"   },
{   "name"  :  "room_type"  ,   "value"  :  "9"   },
]
```


A query such as this one:


```text
query.name:  "currency"   AND   query.value:  "9"
```


would match our document although it would be matching for the “wrong reasons”. In our example,` currency` doesn’t have a value of` 9` , but since both boolean conditions are evaluated as` true` , the given document would produce a match. It is more likely that the user firing this query wants to match on` currency` having the value` 9` , which should not produce any matches in our sample data.


## Our Solution


Our end goal is to match by attribute name/value pair, so if we could make these pairs a single token, we would accomplish our goal with the benefit of having a single field and not strange matches. With this approach, each key/value pair of the query string would be a single token in the form of` name1=value1` and` name2=value2` . This means that we could then write a query


like:


```text
query  :   'currency=EUR'  ;
```


This changes how we can query the data, but it guarantees that it would not produce false matches. Since we don’t generate new fields dynamically, there is also no risk of having cardinality issues.


The tokenization can be implemented in different places in the pipeline using the[split](https://www.elastic.co/guide/en/logstash/current/plugins-filters-split.html)[filter](https://www.elastic.co/guide/en/logstash/current/plugins-filters-split.html) or the previously mentioned[kv](https://www.elastic.co/guide/en/logstash/current/plugins-filters-kv.html)[filter](https://www.elastic.co/guide/en/logstash/current/plugins-filters-kv.html) . We decided to use a[custom pattern analyzer](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-pattern-analyzer.html) on the Elasticsearch side.


Our` pattern_analyzer` uses a custom tokenizer defined as:


```text
"url_pattern"  : {
"pattern"  :   "&"  ,
"type"  :   "pattern"
}
```


We also use a custom` char_filter` to handle the decoding of some special characters into their ASCII equivalent, which makes the queries more user-friendly:


```text
"char_filter"  : {
"url_escape_filter_mapping"  : {
"type"  :   "mapping"  ,
"mappings"  : [
"%20:> +"  ,
"%2B:> +"  ,
"%2C:> ,"  ,
"%3A:> :"  ,
"%5E:> ^"  ,
"%7C:> |"  ,
"%3D:>:"  ,
"%5B:> ["  ,
"%5D:> ]"
]
}
}
```


Finally, we define a custom analyzer called` pattern_analyzer` :


```text
"pattern_analyzer"  : {
"filter"  : [
"Lowercase"  ,
"Asciifolding"  ,
"Stop"  ,
"Unique"
],
"char_filter"  : [
"Html_strip"  ,
"Url_escape_filter_mapping"
],
"type"  :   "custom"  ,
"tokenizer"  :   "url_pattern"
}
```


This analyzer is used in our mapping templates:


```text
"query"  : {
"norms"  :   false  ,
"analyzer"  :   "pattern_analyzer"  ,
"type"  :   "text"
},
```


If we test the initial value of our` query` field against the field using the custom analyzer:


```text
POST   accesslogs/_analyze
{
"field"  :   "query",
"Text"  :    "from_date=2020-06-01T00:00:00%2B02:00&to_date=2020-06-10T00:00:00%2B02:00&currency=EUR&room_type=9&room_0=2a&fixed_status=1"
}
```


We get a more useful list of tokens:


```text
[
"from_date=2020-06-01T00:00:00+02:00"  ,
"to_date=2020-06-10T00:00:00+02:00"  ,
"currency=eur"  ,
"room_type=9"  ,
"room_0=2a"  ,
"fixed_status=1"
]
```


Using this list of tokens, it is easier to find those specific requests that we’re looking for. It is even more intuitive to realize what is going on if we need to share the query with a colleague.


We could’ve decided to write our own tokenizer to deal with URLs. It would have provided us with full control over the Token Stream (i.e tokens) produced by Elasticsearch. Still, dealing with custom analyzers involves writing and maintaining custom plugins, which would have been definitively more difficult to support in the long run. Instead, we chose to leverage the already quite flexible toolbox provided by Elasticsearch.


## Thanks


I want to thank my colleague[🦄 Dario Segger](https://github.com/unidario) (currently ex-teammate) for the initial implementation described in this post. We’ve been using this approach for some time now.
