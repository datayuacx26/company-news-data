---
schema_version: "1.0.0"
document_id: "4f692205d4175098926f1ff23447c81dbd967abb5600d7bdd35c226afe1d500b"
company_key: "yc-lancedb"
company: "LanceDB"
source_id: "yc-lancedb-news-import-bc01535eeacf"
canonical_url: "https://www.lancedb.com/blog/scalable-computer-vision-with-lancedb-voxel51-d8b65066d5f6"
published_at: null
first_seen_at: "2026-07-22T01:53:54.773349+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:6047e0d8bef518ea3818f4ccfee79e51517206dc51d4b32017d6e6e0c3078467"
---

# Scalable Computer Vision with LanceDB & Voxel51

## Importance of dataset analysis in CV


Dataset analysis is an essential step in any computer vision project. It allows us to understand the data that we are working with, identify any potential problems, and make sure that the data is as representative as possible of the real world. This is especially important once the model architecture is fixed due to deployment constraints, as the best way to improve performance is via iteration on datasets.


[Voxel51](https://voxel51.com/) is a set of open-source tools for computer vision. The FiftyOne query language allows you to explore and analyze your data in a variety of ways. You can use the query language to find specific images, objects, or features.


With this integration with LanceDB, Voxel51 can leverage its fast, robust, and setup-free vector search capabilities.


## LanceDB: The serverless vectorDB


LanceDB is an open-source, serverless, multi-modal vector database.


- It is built with persistent storage, which greatly simplifies retrieval, filtering, and management of embeddings.
- It supports vector similarity search, full-text search, and SQL.
- Native support for JavaScript/TypeScript


To enable the LanceDB backend in Voxel51, simply pass` backend="lancedb"` in supported operations


```text
import   fiftyone.brain   as   fob


fob.compute_similarity(
...
backend=  "lancedb"  ,
)
```


## **Example use cases**


### Sort dataset by similarity


### Get similar images to a given id


```text
import   fiftyone   as   fo
import   fiftyone.brain   as   fob


fob.compute_similarity(
...
backend=  "lancedb"  ,
)
id   = dataset.first().  id
view = dataset.sort_by_similarity(  id  , k=  30  )


session = fo.launch_app(view)
session.wait()
```


### Search by Text queries


```text
query =   "a photo of a dog"
view = dataset.sort_by_similarity(query, k=  10  , brain_key=  "lancedb_index"  )
```


The integration allows various customizations that can be found in the docs.


## LanceDB interface integrates into the Python data ecosystem


LanceDB is compatible with the Python data ecosystem and can be used with pandas, NumPy, and Arrow.


```text
lancedb_index = fob.compute_similarity(…)
table = lancedb_index.table


# Integration with Python data ecosystem
df = table.to_pandas()    # get the table as a pandas dataframe
pa = table.to_arrow()     # get the table as an arrow table
```


More use cases and examples using voxel51 with LanceDB backend are available in the[integration docs](https://docs.voxel51.com/integrations/lancedb.html#lancedb-integration) .


Visit LanceDB[repo](https://github.com/lancedb/lancedb) and[docs](https://lancedb.github.io/lancedb/) to see more examples and integration.
