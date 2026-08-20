---
schema_version: "1.0.0"
document_id: "426d2a9b53e2cd73291775485332b522227d0bd6e052d8e20c42bd0b3c1a7ee4"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/vector-buckets"
published_at: "2025-12-01T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:25:06.703683+00:00"
content_hash: "sha256:fd18c836be9bd65df35f72736be87c35a229150473593df8487b473cbb165be3"
---

# Introducing Vector Buckets

Today, we're introducing[Vector Buckets](https://supabase.com/docs/guides/storage/vector/introduction) , a new storage option that gives you the durability and cost efficiency of Amazon S3 with built-in similarity search.


Vector search is becoming a core primitive for modern apps: semantic search, recommendations, RAG, image and audio similarity, and more.


Supabase already gives you powerful tools for vectors, such as` pgvector` in Postgres. With Vector Buckets, you now have more options for how you store vectors:


- Use pgvector for smaller, latency-sensitive datasets that belong tightly in your database.
- Use Vector Buckets when you need to store a large amount of vectors—up to tens of millions—on a durable storage layer with similarity search built in.


## What are Vector Buckets?#


**Vector Buckets** are a new bucket type in Supabase Storage.


Conceptually:


- A **Vector Bucket** is where your vector indexes live.
- Inside each bucket, you define one or more **vector indexes** (for example:` documents-openai` ).
- Each index stores high-dimensional vectors plus optional metadata.
- You query those indexes using Supabase clients or directly from Postgres via a foreign data wrapper.


## What do Vector Buckets bring to the table?#


### Scalable vector storage for large datasets#


Embeddings add up quickly: thousands of floats per vector, multiplied by millions of items.


Instead of putting everything in Postgres, Vector Buckets store your embeddings in S3-backed object storage, which gives you:


- Capacity for tens of millions of vectors per index
- A storage layer designed for large, durable datasets
- Room to keep full archives of vectors without over-optimising your Postgres schema or worrying about table bloat


Your vectors live in a storage layer built for large datasets, while you still query them through Postgres.


### Built-in similarity search#


Vector Buckets are not just blobs of float arrays. Each index supports similarity search out of the box.


Similarity search lets you find items that are conceptually related based on their vector representations, not just exact keyword matches. That’s what powers:


- Semantic document search (“find content about this topic, even if the keywords differ”)
- Product and content recommendations (“find items similar to this one”)
- Image, audio, or video similarity (“find assets that look or sound like this”)
- De-duplication and near-duplicate detection across large media libraries


With Vector Buckets, you can:


- Insert vectors with a key, a float32 vector, and metadata
- Run k-NN queries (for example, “return the 20 closest vectors to this embedding”)
- Use a familiar distance metric such as cosine similarity
- Ask for distances and metadata along with the results


No extra vector database to run, no new query language. Just vector indexes with search, available from the same Supabase SDKs you already use or directly via Postgres.


### Performance that fits most app workflows#


Vector Buckets are designed to provide sub-second similarity search over large datasets, which is more than enough for:


- Backend workflows and batch processing
- AI agents and background jobs
- Dashboards and internal tools
- Many user-facing features where “fast” means hundreds of milliseconds, not single-digit milliseconds


If you’re chasing ultra-low latency at very high QPS,` pgvector` in a tuned Postgres cluster (or a dedicated vector database) remains the best place to push performance. Vector Buckets focus on simple, scalable similarity search at large scale, not on being the absolute fastest option.


### Metadata filtering#


Each vector can include an arbitrary metadata object, for example:


`
_10


metadata: {


_10


title: 'Getting started with Vector Buckets',


_10


type: 'doc',


_10


language: 'en',


_10


project_id: '1234',


_10


}


`


You can:


- Filter by metadata during similarity search (e.g.` type = 'doc' AND language = 'en'` )
- Query through Postgres and join the results with your relational tables
- Build multi-tenant or multi-project search just by encoding tenant/project IDs into metadata


This makes it easy to build domain-aware, tenant-aware semantic search.


## When should you use Vector Buckets vs` pgvector` ?#


Vector Buckets and` pgvector` are complementary. They serve different roles and work best together.


### Use` pgvector` when…#


- You’re optimizing for **lowest possible latency** on user-facing queries
- Vectors are **part of your core relational model** (for example, a column on` documents` or` products` )
- You want **transactional guarantees** (data and embeddings written together)
- Your vector dataset is **small to medium** and you’re comfortable scaling Postgres specifically for vector workloads


### Use Vector Buckets when…#


- You want **S3-style durability and scale** for embeddings
- You’re dealing with a **large amount of vectors** (up to tens of millions) that you don’t want sitting in Postgres
- You’re building **AI-heavy Supabase apps** (semantic search, recommendations, RAG, media similarity) and want a managed vector storage tier
- You prefer a clear split between:


- **Hot vectors** in` pgvector` for the highest-traffic / most latency-sensitive queries
- **Warm or cold vectors** in Vector Buckets for everything else


In practice, many apps will use both:


- Keep your most frequently queried vectors (for example, current content, top products) in` pgvector` .
- Store the full archive (older content, long tail SKUs, historical embeddings, large media corpora) in Vector Buckets.


## How do Vector Buckets work?#


At a high level, here’s what happens under the hood:


**1. Vector Bucket in Supabase Storage**


You create a bucket of type Vector Bucket in the Dashboard or via API.


`
_10


import { createClient } from '@supabase/supabase-js'


_10


_10


const supabase = createClient('https://your-project.supabase.co', 'your-service-key')


_10


_10


await supabase.storage.vectors.createBucket('embeddings')


`


**2. Create Vector indexes inside the bucket**


Inside the Vector Bucket, you create one or more indexes.


`
_10


// Create an index in that bucket


_10


await supabase.storage.vectors.from('embeddings').createIndex('documents-openai', {


_10


dimension: 1536,


_10


distanceMetric: 'cosine',


_10


})


`


**3. Store vectors**


You can store vectors directly from the SDK, an Edge Function, or Postgres.


`
_34


// Postgres


_34


INSERT INTO s3_vectors.documents_openai (key, data, metadata)


_34


VALUES


_34


(


_34


'doc-1',


_34


'\[0.1, 0.2, 0.3, /* ... rest of embedding ... */\]'::embd,


_34


'{"title": "Getting Started with Vector Buckets", "source": "documentation"}'::jsonb


_34


),


_34


(


_34


'doc-2',


_34


'\[0.4, 0.5, 0.6, /* ... rest of embedding ... */\]'::embd,


_34


'{"title": "Advanced Vector Search", "source": "blog"}'::jsonb


_34


);


_34


_34


// JS-SDK (server only)


_34


const index = supabase.storage.vectors


_34


.from('embeddings')


_34


.index('documents-openai')


_34


_34


const { error } = await index.putVectors({


_34


vectors: \[


_34


{


_34


key: 'doc-1',


_34


data: {


_34


float32: \[0.1, 0.2, 0.3 /* ... */\],


_34


},


_34


metadata: {


_34


title: 'Getting started with Vector Buckets',


_34


type: 'doc',


_34


language: 'en',


_34


},


_34


},


_34


\],


_34


})


`


**4. Query vectors**


You can run similarity search queries against your indexes, either via the SDK or Postgres.


`
_24


// Postgres


_24


SELECT


_24


key,


_24


metadata->>'title' as title,


_24


embd_distance(data) as distance


_24


FROM s3_vectors.documents_openai


_24


WHERE data <==> '\[0.1, 0.2, 0.3, /* ... embedding ... */\]'::embd


_24


ORDER BY embd_distance(data) ASC


_24


LIMIT 5;


_24


_24


// JS-SDK (Server only)


_24


const index = supabase.storage.vectors


_24


.from('embeddings')


_24


.index('documents-openai')


_24


_24


// Query with a vector embedding


_24


const { data, error } = await index.queryVectors({


_24


queryVector: {


_24


float32: \[0.1, 0.2, 0.3 /* ... embedding of 1536 dimensions ... */\],


_24


},


_24


topK: 5,


_24


returnDistance: true,


_24


returnMetadata: true,


_24


})


`


## Designed for workloads up to tens of millions of vectors#


Vector Buckets currently can handle large-but-not-infinite workloads:


- Each vector index supports up to **tens of millions of vectors** (50M per index today).
- You can create multiple indexes per bucket (for tenants, models, or domains).


That makes Vector Buckets a great fit for:


- Multi-tenant SaaS apps
- Documentation and content libraries
- Product catalogues and recommendation systems
- Media libraries and image/video/audio similarity search
- AI builders who want semantic search without running their own vector infrastructure


## Example scenarios#


A few concrete ways to put Vector Buckets to work:


### 1. AI documentation search#


- Store all your documentation (including old versions, drafts, and translations) as embeddings in a Vector Bucket.
- Keep the most recent / highest-traffic docs in` pgvector` for instant in-app search.
- Implement a search endpoint that queries` pgvector` first and falls back to Vector Buckets when needed.


### 2. Long-tail product search and recommendations#


- Vectorise your entire catalogue and store it in a Vector Bucket.
- Include metadata for category, brand, stock status, and region.
- Use metadata filters to refine search (e.g. “in stock, in this region, same category”).
- Let recommendation jobs and AI agents work against the full set of products without bloating Postgres.


### 3. Media similarity and de-duplication#


- Store embeddings for images, audio or video frames in a Vector Bucket.
- Use similarity search to:


- Find visually similar assets for content discovery or recommendations
- Detect possible copyright issues by finding near-duplicate content
- Clean up your library by removing duplicate or near-duplicate media


## Availability#


Vector Buckets are currently available in **Public Alpha** for Pro projects and above.


Currently supported in the following regions:


- us-east-1
- us-east-2
- us-west-2
- eu-central-1
- ap-southeast-2


More regions will be added in the near future.


We’re using this phase to refine the APIs, scaling behaviour, and search experience based on real workloads. Limits may evolve as we learn from how you use the feature in production.


Vector Buckets are **free to use (fair use policy applies)** during Public Alpha. Egress costs still apply.


## Get started#


You can try Vector Buckets in your project today:


1.


**Create a Vector Bucket**


Dashboard → **Storage → Create bucket → Vector Bucket** .


2.


**Create an index**


Pick a dimension that matches your embedding model and choose a distance metric.


3.


**Store vectors**


Use Supabase clients to upsert vectors with metadata.


4.


**Query vectors**


Build endpoints for semantic search, recommendations, or retrieval-augmented generation.


5.


**Layer with` pgvector`**


Keep your hottest, most latency-sensitive vectors in` pgvector` , and store large archives and media-heavy datasets in Vector Buckets.


We’re excited to see what you build with this new vector storage tier.


As you try Vector Buckets during the Public Alpha, please send feedback—what works, what’s confusing, and what you’d like to see next will directly shape where we take this feature.
