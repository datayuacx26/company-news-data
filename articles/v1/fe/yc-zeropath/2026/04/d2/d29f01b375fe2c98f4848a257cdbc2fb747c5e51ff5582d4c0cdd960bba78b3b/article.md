---
schema_version: "1.0.0"
document_id: "d29f01b375fe2c98f4848a257cdbc2fb747c5e51ff5582d4c0cdd960bba78b3b"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/ragflow-rce-unpatched-vulnerability"
published_at: "2026-04-09T00:00:00+00:00"
first_seen_at: "2026-07-24T06:50:52.708520+00:00"
fetched_at: "2026-07-28T22:15:57.305389+00:00"
content_hash: "sha256:93a9f47dcb310966f2fb84835a084707f621f4640c7b4fd7370a58e32d5f7a00"
---

# Unpatched RAGFlow Vulnerability Allows Post-Auth RCE

# Unpatched RAGFlow Vulnerability Allows Post-Auth RCE


## Bottom Line


A currently-unpatched vulnerability in the most recent version of RAGFlow (0.24) allows low-privilege authenticated users to run arbitrary code. Only RAGFlow instances using Infinity for chunk storage are vulnerable. We have submitted a PR and expect that the issue will be patched soon.


## Impacted Software


RAGFlow 0.24.0 (current version as of April 8th 2026)


## Video Walkthrough


## RAGFlow Background


RAGFlow is a wildly-popular project for Retrieval Augmented Generation. It gives LLMs a structured library of documents they can refer to when responding to prompts. As of early April 8th 2026, the project had 77.5k stars on Github and was widely adopted across many companies.


RAGFlow has over 77k stars on Github


Most users configure the service to listen on an internal network, but at least 1,918 instances are directly accessible on the public internet according to Shodan.


At least 1,918 RAGFlow instances are exposed on the public internet.


## Disclosure Process


We created a github security report ([GHSA-vw46-rrp3-c99v](https://github.com/infiniflow/ragflow/security/advisories/GHSA-vw46-rrp3-c99v) ) on March 3rd, 2026, and attempted to follow up with the project maintainers several times via email without success.


Our initial bug report, filed March 3rd, 2026


Given how easily-discoverable the flaw is, and in keeping with our[outbound disclosure policy](https://zeropath.com/outbound-disclosure) , after a month we decided that the most effective way to get the issue fixed was to submit a patch ourselves ([https://github.com/infiniflow/ragflow/pull/14091](https://github.com/infiniflow/ragflow/pull/14091) ). Unfortunately, creating this public PR means that any attackers monitoring the project are now aware of the vulnerability. We hope that this blog gets defenders up to speed as well, so that they can take appropriate countermeasures.


Releasing this post before remediation was not a decision we took lightly. Given how rapidly vulnerability discovery is accelerating because of LLM-powered research flows, we believe that it will become more and more common for known, reported vulnerabilities to escape the attention of project maintainers, and that in these cases it's better to make the issue public as responsibly as possible instead of letting attackers quietly discover and exploit it without legitimate users being aware they're running vulnerable software.


## The Flaw


### Original Sin


While researching RAGFlow, the function` _rank_feature_scores()` initially caught our attention. It's invoked during the rerank phase of document retrieval and converts a value from database search to a python dict using` eval()` like this:


```text
# rag/nlp/search.py


def     _rank_feature_scores  (  self  ,   query_rfea  ,   search_res  )  :
# ...
for   t  ,   sc   in     eval  (  search_res  .  field  [  i  ]  .  get  (  TAG_FLD  ,     "{}"  )  )  .  items  (  )  :
```


` eval()` will execute any python code. If the value of TAG_FLD is a typical dictionary declaration (` { "foo": "bar"}` ) it works as expected, but if there was a way to corrupt its value in the datastore, we'd have an easy RCE.


### Corrupting tag_feas


#### Anatomy of Retrieval


The property we were interested in is accessed during chunk retrieval through public API endpoints like /api/v1/retrieval, which take a question and return relevant documents.


Retrieval starts with a query to the configured datastore to find a broad set of documents that may be relevant. This initial query can be things like a vector search or a fulltext search.


Depending on configuration, the engine sometimes then "reranks" the results from the initial search -- that is, performs potentially more expensive computations on them to better score relevance.


The vulnerable code we were interested in is in this re-ranking process.


This means that we needed to trace the application data flow to find a way to get malicious data into the` TAG_FLD` property, and then craft a search that would access this malicious data during reranking.


#### Data Flow: Datastore To Eval()


We started by tracing the data flow, looking for a public-facing vector to corrupt the data, and trying to understand any sanitization or validation we'd have to dodge.


It turns out` .get(TAG_FLD, "{}")` reads the "` tag_feas` " property of a document chunk (where "feas" is short for "features"). This field is supposed to be an object that captures how relevant pre-defined tags are to the chunk, e.g.:


```text
{
"tag1"  :     0.1  ,
"tag2"     :     0.3
}
```


When the retrieval API is invoked, if Infinity is the backend, RAGFlow searches for chunks like this:


```text
# rag/nlp/search.py, Dealer.search()):
# Fields to retrieve from chunks as part of search -- includes TAG_FLD, which is our target tag_feas property


src   =   req  .  get  (  "fields"  ,
[  .  .  .  ,   PAGERANK_FLD  ,   TAG_FLD  ,     "row_id()"  ]  )


# rag/nlp/search.py, Dealer.retrieval()):
# Actual search invocation -- includes field list above (req)


sres   =     await   self  .  search  (  req  ,     [  index_name  (  tid  )     for   tid   in   tenant_ids  ]  ,   kb_ids  ,   embd_mdl  ,   highlight  ,  rank_feature  =  rank_feature  )


# Infinity-specific processing of tag_feas column in search result
# rag/utils/infinity_conn.py:


elif   re  .  search  (  r"_feas$"  ,   k  )  :
res2  [  column  ]     =   res2  [  column  ]  .  apply  (  lambda   v  :   json  .  loads  (  v  )     if   v   else     {  }  )


# rag/nlp/search.py
# sres (search result) ultimately passed to eval here during re-ranking


def     _rank_feature_scores  (  self  ,   query_rfea  ,   search_res  )  :
# ...
```


Based on this, we can tell that the only notable sanitization or transformation on the` tag_feas` column is that it's deserialized to a python type via` json.loads()` .


Fortunately for us, a plain string is valid json. As long as our corrupted value is a valid JSON string, the load will succeed with arbitrary python code.


(Interestingly, this also means the` eval()` is completely unnecessary when Infinity is the backend -- under normal circumstances,` json.loads()` converts the database value to a dict, it's implicitly converted back to a str for` eval()` , which then turns it right back into a dict.)


#### Data Flow: Datastore-Level Validation/Transformation


At this point we knew that a malicious value in the datastore would be faithfully piped through to` eval()` . But would any of RAGFlow's storage backends support storing an invalid property?


Unfortunately for attackers, ElasticSearch defines a mapping that fixes the type of` tag_feas` :


```text
{
"rank_features"  :     {
"match"  :     "*_feas"  ,
"mapping"  :     {
"type"  :     "rank_features"
}
}
}
```


To be accepted and written to the ElasticSearch index, items of rank_features type must be a JSON object with string values for keys and floating point numbers for values. This makes it difficult to sneak malicious code in. In our exploration at least, it didn't seem possible.


Unlike Elastic though, Infinity does not put any constraints on` tag_feas` at the datastore level, representing it as a simple varchar. This meant that as long as our target RAGFlow installation uses Infinity, the datastore wouldn't block our malicious code.


#### Data Flow: Public API To Datastore


We now just needed a way to get a user-controlled string into the` tag_feas` property of a chunk in an Infinity-backed RAGFlow instance.


The most obvious public endpoints to attempt to corrupt a chunk's` tag_feas` value are:


- POST /v1/chunk/create
- POST /v1/chunk/set
- POST /api/v1/datasets/{id}/documents/{id}/chunks
- PUT /api/v1/datasets/{id}/documents/{id}/chunks/{id}


Surprisingly, all these store the user-supplied` tag_feas` directly to the target chunk without validation. It's possible that this oversight occurred because ElasticSearch enforces the type itself.


Whatever the reason, we now had an end to end flow from our user input to the` eval()` statement, at least for RAGFlow instances using the Infinity backend.


#### Control Flow: Triggering Our Malicious Code


Our malicious property doesn't do us any good unless we're able to get RAGFlow to retrieve it. The most obvious vector was to trigger a search against the knowledge base with our corrupted chunk using /api/v1/retrieval.


However, the vulnerable code exists in a special phase of the retrieval process called re-ranking, where relevance scores and so on are adjusted after an initial search. By default with Infinity, re-ranking is skipped:


```text
# rag/nlp/search.py retrieval()


if   settings  .  DOC_ENGINE_INFINITY  :
# Don't need rerank here since Infinity normalizes each way score before fusion.
sim   =     [  sres  .  field  [  id  ]  .  get  (  "_score"  ,     0.0  )     for     id     in   sres  .  ids  ]
sim   =     [  s   if   s   is     not     None     else     0.0     for   s   in   sim  ]
tsim   =   sim
vsim   =   sim
```


If the user specifies an explicit re-rank model as a parameter to their retrieval query though, re-ranking still occurs:


```text
# rag/nlp/search.py retrieval()


if   rerank_mdl   and   sres  .  total   >     0  :
for   _id   in   sres  .  ids  :
# rerank_by_model ultimately calls the problematic eval()
sim  ,   tsim  ,   vsim   =   self  .  rerank_by_model  (
rerank_mdl  ,
sres  ,
question  ,
1     -   vector_similarity_weight  ,
vector_similarity_weight  ,
rank_feature  =  rank_feature  ,
)
```


At this point, we had the complete picture:


- Create a malicious Knowledge Base
- Corrupt` tag_feas` property of a chunk in that Knowledge Base using public API
- Use retrieval API to find that malicious chunk, forcing vulnerable rerank code to run with the rerank_id parameter


#### POC


Our fully-working POC is available here:


[https://github.com/ZeroPathAI/ragflow-poc](https://github.com/ZeroPathAI/ragflow-poc)


It includes a setup script and docker compose to stand up a test RAGFlow instance configured to be vulnerable.


NOTE: In testing with Infinity in docker with very few documents in it, we found that full text caches sometimes meant that it could take awhile for our malicious chunk to start appearing in search results. This would be less of an issue on a busy production deployment, and would just mean an attacker would need to retry retrieval until it worked. However, to make the issue easy to demonstrate, the POC optionally lets you specify the URL for the Infinity Thrift endpoint, so that it can explicitly flush caches so you don't have to wait around for that to happen naturally.


Overall exploit flow:


- Create a Knowledge Base that defines tags (tag_kb)
- Create a second Knowledge Base with the tag_kb_ids parser_config property set to the tag_kb knowledge base (target_kb)


- We need a tag kb configured, because without one, the vulnerable code doesn't get executed. The` eval()` happens when the re-ranker evaluates relevance of tags, which only happens if the target kb has tags.


- Add a chunk to target_kb with its` tag_feas` property set to a string containing python code
- Trigger a search of target_kb using the /api/v1/retrieval endpoint. Use a search question guaranteed to match our target chunk.


- Critically: specify that a re-rank model should be used during re-ranking with` rerank_id` . If Infinity is being used as the backend without a rerank model, vulnerable code is bypassed.


- Result: during re-ranking of results returned from Infinity, eval() gets triggered to try to parse` tag_feas` , resulting in RCE.


## Mitigation


### Apply Unofficial Patch


The RAGFlow project has not yet released a patched version, but you can apply this patch manually to your local RAGFlow instances:


[https://github.com/infiniflow/ragflow/pull/14091](https://github.com/infiniflow/ragflow/pull/14091)


### Reduce Exposure


Ensure RAGFlow instances are not accessible via the public internet, that only trusted users have RAGFlow accounts, and that those accounts are protected by strong passwords and properly controlled in password managers and vaults.


### Monitor Closely


Add proactive monitoring to RAGFlow pods and servers -- especially watch for suspicious processes spawning underneath the RAGFlow process.


## Lessons


While using` eval()` to convert a string representation of a dictionary to an actual dictionary is not uncommon, it's bad practice in production code. We'd strongly recommend avoiding this technique unless absolutely necessary, even when it seems like the input to eval can't be user influenced. It's in most cases an unnecessary foot gun.
