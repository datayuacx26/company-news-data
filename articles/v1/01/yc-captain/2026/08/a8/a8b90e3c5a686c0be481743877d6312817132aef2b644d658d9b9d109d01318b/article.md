---
schema_version: "1.0.0"
document_id: "a8b90e3c5a686c0be481743877d6312817132aef2b644d658d9b9d109d01318b"
company_key: "yc-captain"
company: "Captain"
source_id: "yc-captain-rss-30d4a88671b2"
canonical_url: "https://www.runcaptain.com/blog/copy-collections"
published_at: "2026-08-18T12:00:00+00:00"
first_seen_at: "2026-08-20T00:24:19.602650+00:00"
fetched_at: "2026-08-20T00:24:21.208142+00:00"
content_hash: "sha256:bd480f3d9a1d56434cb4e328dbaebcb2e54667970a97ad76507a82ad70f63c70"
---

# Copy Collections

# Copy Collections


**You can now copy collections in Captain.** One API call duplicates the documents, chunks, embeddings, and custom metadata under a new collection name, reusing the index you already built.


bash


1 curl


-X


POST


https://api.runcaptain.com/v2/collections/shipping_manuals/copy


\\


2


-H


"Authorization: Bearer $CAPTAIN_API_KEY"


\\


3


-H


"Content-Type: application/json"


\\


4


-d


'{"target_name": "shipping_manuals_staging"}'


Copied collections are fully independent. Writes and deletes stay scoped to the collection they were issued against.


## Why we built it


Three requests kept coming up:


- **One collection per customer.**
If you run retrieval for many end clients, each client's data belongs in its own collection: separate indexes and separate deletes. Standing up client number forty used to mean re-indexing a shared corpus for the fortieth time. Now you can index the base corpus once, then easily copy it per client.
- **Staging copies.**
Sometimes a collection needs to branch. Production stays on the settings your users depend on while a copy absorbs the change you're not sure about yet: a different parsing configuration, a new chunk relationship setup, a different[PII masking policy](https://captain.dev/pii) . Test against the copy with real data, and when the branch proves out, cut over. When it drifts too far, you can delete it and copy again.
- **Experiments.**
Snapshot a collection before a risky change. If the experiment fails, delete the copy and the original is untouched.


## What it used to cost


Before this endpoint, duplicating a collection meant re-indexing it from the source files: paying indexing credits on every document again, then waiting on the job queue, sometimes for hours, before the duplicate was usable.


A copy is a flat **10 credits** , billed once the copy succeeds. Because the vectors are copied rather than rebuilt, the operation completes **in about a second** . Both numbers hold at any collection size.


Here it is against a real collection:


POST


/v2/collections/ :collection_name


/copy


[Try it](https://docs.captain.dev/reference/collections/copy)


1 curl


-X


POST


https://api.runcaptain.com/v2/collections/blog_demo_shipping_manuals/copy


\\


2


-H


"Authorization: Bearer $CAPTAIN_API_KEY"


\\


3


-H


"Content-Type: application/json"


\\


4


-d


'{"target_name": "blog_demo_shipping_manuals_staging"}'


200


Copied


1.28s


1 {


2


"success"


:


true


,


3


"message"


:


"Copied 'blog_demo_shipping_manuals' to 'blog_demo_shipping_manuals_staging' (6 documents)"


,


4


"source_collection"


:


"blog_demo_shipping_manuals"


,


5


"collection_name"


:


"blog_demo_shipping_manuals_staging"


,


6


"database_id"


:


"01a01749-7d73-7401-89ec-8261383d631c"


,


7


"documents_copied"


:


6


,


8


"files_copied"


:


6


,


9


"namespaces_copied"


:


1


10 }


## Details


- ` POST /v2/collections/{collection_name}/copy` with a` target_name` in the body.
- The target name must be unused in the collection's environment; a taken name returns a 409.
- Copies land in the same environment as the source and behave like any other collection: query them, index into them, delete them.


Reference documentation for the endpoint is at[docs.captain.dev/reference/collections/copy](https://docs.captain.dev/reference/collections/copy) .


Lewis Polansky
CEO & Co-Founder
Aug 18, 2026
