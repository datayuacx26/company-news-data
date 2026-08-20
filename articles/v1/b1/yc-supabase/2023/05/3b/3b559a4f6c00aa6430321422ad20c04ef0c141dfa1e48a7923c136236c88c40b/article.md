---
schema_version: "1.0.0"
document_id: "3b559a4f6c00aa6430321422ad20c04ef0c141dfa1e48a7923c136236c88c40b"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/vecs"
published_at: "2023-05-29T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:26478d3aef99c56974bf59c2a1636eddef4440dbe886968ca13a76caf60e76f7"
---

# Supabase Vecs: a vector client for Postgres

[Vecs](https://github.com/supabase/vecs) is a new Python library for managing embeddings in your Postgres database with the pgvector extension.


It handles:


- Creating and indexing tables
- Querying vectors by cosine distance, l2 distance, and max inner dot product
- Filtering based on user-defined metadata


## Goals#


Our goal for` vecs` is to provide an interface that lets Postgres + pgvector look and feel like a dedicated vector store. It works with any Postgres database (or platform) that[supports pgvector](https://github.com/pgvector/pgvector/issues/54) . It was designed with ease-of-use, interactivity, and exploratory data analysis in mind, but works equally well as a search workhorse.


If you're interested in the nuts and bolts of what's going on, it's trivial to drop into the SQL layer and see what's happening. Alternatively, folks who don't want to know what's happening in the database, don't need to care.


## Usage#


` Vecs` makes it easy to create a collection (table) and insert a few records - just 5 lines of code.


### Connecting#


`
_10


import vecs


_10


_10


DB_CONNECTION = "postgresql://<user>:<password>@<host>:<port>/<db_name>"


_10


_10


# create vector store client


_10


vx = vecs.create_client(DB_CONNECTION)


_10


_10


# create a collection of vectors with 3 dimensions


_10


docs = vx.get_or_create_collection(name="docs", dimension=3)


`


The` get_or_create_collection` call sets up a table in the Postgres database specified by` DB_CONNECTION` in a schema named` vecs` with the user defined name` docs` .


Or, more specifically:


`
_10


create table vecs.docs (


_10


id text primary key,


_10


vec vector(3) not null,


_10


metadata jsonb not null default '{}'::jsonb


_10


);


`


### Insert/Update#


We can insert a few records in that new SQL table/vecs collection using` Collection.upsert` .


`
_10


# add records to the collection


_10


docs.upsert(


_10


vectors=\[


_10


(


_10


"vec0", # the records user defined identifier


_10


\[0.1, 0.2, 0.3\], # the vector. A list or np.array


_10


{"year": 1973} # associated metadata


_10


)


_10


\]


_10


)


`


which will add the records to our table if the` id`` "vec0"` does not exist, or updates the existing record if it does exist.


### Query#


You can query a vecs collection at any time without an index, but it's a best practices to create an index on your collection after inserting data.


`
_10


docs.index()


`


Where` index` optionally takes an argument for the distance measure to index.


Finally, we can search the collection for similar vectors using the` query` method:


`
_10


docs.query(


_10


query_vector=\[0.10,0.21,0.29\], # required


_10


limit=1, # (optional) number of records to return


_10


filters={"year": {"$eq": 1973}}, # (optional) metadata filters


_10


measure="cosine_distance", # (optional) distance measure to use


_10


include_value=False, # (optional) should distance measure values be returned?


_10


include_metadata=False, # (optional) should record metadata be returned?


_10


)


`


Which returns:


`
_10


\[("vec1", 0.000697, {"year": 1973})\]


`


Since all metadata is stored in a` jsonb` column, there's a[lightweight but flexible DSL](https://supabase.github.io/vecs/concepts_metadata/) wrapped around it for filtering.


When you're done, disconnect with:


`
_10


vx.disconnect()


`


And 90% of the time, that minimal interface is all you'll need to touch.


For more in-depth information about` vecs` , checkout the[API Quickstart](https://supabase.github.io/vecs/api/) ,[celebrity look-alike demo](https://github.com/supabase/supabase/blob/be1f4ea85e10cc8134e389dcdbe6a092b08612f1/examples/ai/face_similarity.ipynb) , or[OpenAI integration example](https://supabase.github.io/vecs/integrations_openai/)


## Deploying with Supabase#


As usual, if you combine[supabase/vecs](https://github.com/supabase/vecs) with the rest of Supabase, you get more than the sum of the parts. Once you're happy with your vecs collection, you can make it accessible to your front-end through a supabase client library by exposing the collection as a view in your public schema.


For example, you could create a view


`
_10


create view public.docs as


_10


select


_10


id,


_10


embedding,


_10


metadata, # Expose the metadata as JSON


_10


(metadata->>'url')::text as url # Extract the URL as a string


_10


from


_10


vecs.docs


`


And then access it with the supabase-js client library within your applications:


`
_10


const { data, error } = await supabase


_10


.from('docs')


_10


.select('id, embedding, metadata')


_10


.eq('url', '/hello-world')


`


For more deployment options, including enterprise scalable architecture, check out the[engineering for scale guide](https://supabase.com/docs/guides/ai/engineering-for-scale#simple-workloads) .


## Future ideas#


Currently,` vecs` is unopinionated about where vectors come from or how they're produced. While there will always be a need for generic vector storage and querying, it's becoming clear that text and image vectorization make up +95% of usage. That gives us the opportunity to streamline those workflows for users.


One option we're exploring is to optionally assign transformation pipelines to collections along the lines of:


`
_14


# This is mock code only, not currently functional


_14


_14


docs: Collection =vx.get_or_create_collection(


_14


docs='docs',


_14


dimension=512,


_14


transform = TextPreprocessor( # this is new


_14


model="sentence-transformers/all-Mini-L6-v2"


_14


)


_14


)


_14


_14


docs.upsert(\[


_14


("id_0", "# Some markdown", {}),


_14


("id_1", "# Some more markdown", {})


_14


\])


`


so users can choose to work with their preferred media type without ever thinking about vectors.


Another direction we're considering is adding an async client to avoid blocking when waiting on the database or network i.e.


`
_10


# This is mock code only, not currently functional


_10


_10


await docs.upsert(\[


_10


("id_0", \[0.1, 0.2, 0.3\], {}),


_10


\])


`


Both possibilities are still up for debate. If you have view on either, feel free to weigh in on the[Feature Request: Preprocessing Transform](https://github.com/supabase/vecs/issues/5) and[Feature Request: Async Client](https://github.com/supabase/vecs/issues/6) GitHub issues.


## More info#


- Source code:[github.com/supabase/vecs](https://github.com/supabase/vecs)
- Vecs Docs:[supabase.github.io/vecs/](https://supabase.github.io/vecs/)
- Supabase Vector Toolkit:[supabase.com/docs/guides/ai](https://supabase.com/docs/guides/ai)
