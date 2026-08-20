---
schema_version: "1.0.0"
document_id: "fe82456bc8e73975d2a134be15b361dd86328d3bf1707d4a0c8d1cd0aac16a87"
company_key: "yc-exa"
company: "Exa"
source_id: "yc-exa-news-import-e15579f9a79b"
canonical_url: "https://exa.ai/blog/exa-d"
published_at: "2026-01-13T00:00:00+00:00"
first_seen_at: "2026-07-25T04:05:34.244049+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:e9b5a2ea13d974898ee109235eb1dc9be3f30e23b1d3a09cc4b4c92f6b3ce84a"
---

# exa-d: Data Framework to Process the Web

Building a modern search engine requires ingesting the entire web and ensuring it is queryable as it changes in real-time. The web has a few properties that make this challenging:


- **Many outputs for every page:** each page produces dozens of artifacts such as extracted text, metadata, and search signals such as embeddings, expanding the surface area for updates
- **Heterogeneous content:** HTML pages, PDFs, JavaScript-rendered apps, multimedia each have different structure and parsing requirements
- **Varying update frequency:** news articles may change hourly, academic papers may never change at all
- **Sheer volume:** hundreds of billions of pages, petabytes of raw content before any processing


To ensure our index stays current, our crawlers must detect changes from the web, reprocess pages, and regenerate embeddings before the query arrives. Each change triggers a messy cascade of derived features (embeddings, extracted text, metadata) with their own dependencies and update logic.


**How do you store and retrieve information from the web in a database?**


In this post, we will walk through exa-d, our inhouse data processing framework, designed to handle this complexity at scale.


## # Constraints to optimize for


Before building exa-d, we evaluated traditional data management stacks: data warehouses, SQL transformation layers, and orchestrators before ultimately deciding to build our own data framework optimized around a specific set of priorities:


### # 1. Typed columns and declarative dependencies


At Exa, many team members need to simultaneously iterate on new search signals derived from existing data. If each team member wrote bespoke scripts for calculating and updating different columns, this would not only lead to excessive code duplication, but also hamper iteration speed by making it difficult to predict the downstream impact of a change.


A core design choice for exa-d was that engineers interact by declaring relationships between data, not the steps to update them. A good analogy here is to spreadsheets where formulas reference other cells. In exa-d, engineers can focus on making sure their formulas are correct, and trust the framework to handle other concerns such as states, retries, and scheduling. This declarative pattern also allows columns and their relationships to be strictly typed, catching invalid transformations immediately as the code is written.


exa-d was built with this developer ergonomics in mind to declare the dependency graph between artifacts and handle execution automatically.


### # 2. Surgical Updates and Full Rebuilds


The dynamic nature of content on the web and the need for rapid iteration means that our data cannot just be stored as a static record, but should be able to support many kinds of flexible updates and augmentations.


Some parts of the web update daily or even hourly, requiring precise replacement of small sections of the index. If a bug gets introduced into our update pipeline, we want to repair exactly the rows that were affected. Other operations occur at a much larger scale, such as when we ship a new model and calculate new embeddings over the entire index, or test out a search signal candidate over a billion rows or more.


This need to modify massive datasets dynamically is particularly prominent in the ML age, and existing data frameworks are still catching up, often requiring inefficient write patterns such as rewriting all rows when modifying any single column.


exa-d was built to be able to identify the specific rows and columns that are affected by a change, without needing large scans or unnecessary rewrites.


### # 3. Efficient + Parallel Execution


Processing the web's data entails running complex jobs over petabytes of data. Two capabilities are essential for data processing at this scale:


**a. Workflows must be parallelized:** broken down and distributed across CPUs, nodes, and clusters so work runs concurrently rather than sequentially.


**b. Parallel work must be efficient:** machines should only compute what actually needs computing, skipping anything that's cached or recoverable from a previous run.


exa-d was designed to handle both. To be effective, parallel work must scale from tens to thousands of nodes. Work is distributed across heterogeneous resources (CPU, GPU, memory, network), and is scheduled to minimize waste.


## # Designing exa-d


To handle these challenges, we built exa-d: a data framework that uses S3 to store the web. The code below roughly outlines what it does:


```text
# tokenization converts text into tokens    documents   =   Column  (  name  =  "documents"  ,     type  =  str  )    tokenized   =   Column  (  name  =  "documents_tokenized"  ,     type  =  torch  .  Tensor  )  .  derive  (  )  .  _from  (  documents  )  .  impl  (  Tokenizer  )     # embedding model converts tokens into embedding vectors    embeddings   =   Column  (  name  =  "embeddings"  )  .  derive  (  )  .  _from  (  tokenized  ,     type  =  torch  .  Tensor  )  .  impl  (  EmbeddingModel  )
dataset   =   Dataset  (  location  =  "s3://exa-data/documents/"  )
# make sure all tokens and embeddings are present for the dataset    execute_columns  (  dataset  ,     [  tokenized  ,   embeddings  ]  )
```


## # The Logical Layer: The Dependency Graph


Data gets transformed in a production web index not as a linear sequence but as a system of independently evolving derived fields. Each field has its own update schedule and dependency surface, such as multiple embedding versions or derived signals like structured extractions. exa-d represents the index as typed columns with declared dependencies. Base columns are ingested data, while derived columns declare intent, forming an explicit dependency graph.


**Figure 1:** Column dependencies for a singular fragmenta in sample row 2. For each input, an output of a defined type is produced via a specific function


This does two practical things immediately:


-


**Execution order is determined by the dependency graph** itself vs hardcoded scripts. If embeddings depend on tokenized output, the column declares that dependency and the system determines execution order automatically. Otherwise, a separate script specifying that order would need to be written and maintained for each pipeline variant.


-


**Column definitions are contracts.** The builder pattern enforces type guarantees, for example Tokenizer: str → Tensorb b Tokenization: This function takes a string as input and outputs an array of numbers. Take a string like "dog eats bone" and split it into tokens, then map each token to an integer from the model's vocabulary. The output is an array of integers: e.g. \[482, 9104, 512\]. The tensor references the array of numbers. This array of integers is fed into an embedding model that outputs a vector of floats (e.g. \[0.023, -0.847, 0.412, ...\]) that represents the semantic meaning of the text.


, and makes column definitions reusable instead of relying on string names and ad hoc assumptions about shapes and schemas.


The graph determines what needs to be computed. For each derived column, the system checks whether its inputs exist and whether its output is already computed. Adding a new derived field means adding a node and its edges, not duplicating a pipeline and manually keeping them in sync.


## # The Storage Layer: Structuring Data for Precise Updates


While we need to process a lot of data, the index is vast. This means that we are appending relatively small sets of data or replacing a minor fraction of the index. If modifying data required rewriting every column on every interaction or scanning large blocks of rows, this would result in significant write amplification.


exa-d's storage model was designed to account for this with a simple idea: track completeness at the granularity you want to update.


Data lives in[Lance](https://lancedb.com/) on S3. Lance stores the dataset as a collection of fragments with partial schemas. Not every fragment needs the same columns and missing derived columns are expected as updates occur incrementally across the dataset.


This is the core storage operation exa-d relies on: **writing or deleting a single column for a specific fragment without rewriting the rest of the fragment.**


```text
def     write_column_to_fragment  (  ds  :   LanceDataset  ,   frag_id  :     int  ,   col  :     str  ,   data  :   pa  .  Array  )  :        frag   =   ds  .  get_fragment  (  frag_id  )        new_file   =   write_lance_file  (            path  =  f"s3://bucket/  {  ds  .  name  }  /  {  frag_id  }  /  {  col  }  .lance"  ,            schema  =  pa  .  schema  (  [  (  col  ,   data  .  type  )  ]  )  ,            data  =  data  ,          )        patched_frag   =   bind_file_to_fragment  (            frag  .  metadata  ,            new_file  ,            ds  .  schema  ,          )          return   patched_frag
patched_frags   =     [        write_column_to_fragment  (  dataset  ,   fid  ,     "embedding_v2"  ,   embeddings  [  fid  ]  )          for   fid   in   missing_frag_ids    ]    commit_to_lance  (  dataset  ,   patched_frags  )
```


Incremental fragment updates lend themselves to a few advantageous properties:


-


**Updates at precise granularity.** Adding a new derived field or fixing a bug only affects files containing impacted columns. Patching a fragment doesn't rewrite unaffected columns, so efficiency is maintained as the number of columns increases.


-


**Global view of column validity.** Auxiliary tables, NULL-filled results or external backfill bookkeeping are not required because the fragment metadata records which columns are present. Using the dataset state directly as an atomic source of truth sidesteps tricky transactional logic and state managementc c Lance uses a global manifest to define the contents of a Lance dataset, and updating the manifest is an atomic operation on S3. If a process makes a change to the dataset, it must race to commit to the manifest: if the manifest has since been modified, the changes have to be rebased onto the latest version. For the most common sort of fragment patching operation, this rebase process is very easy. Using the manifest as the source of truth makes reasoning about distributed interactions much simpler.


.


-


**Targeted debugging.** If a handful of fragments have incorrect values for a derived field, you can delete or invalidate that column for those fragments. The storage format could allow us to modify only the missing or invalid outputs.


## # The Execution Layer: Compute Only What is Necessary


Now that we have a dependency graph that declares the workflow we want to execute and the Lance physical layout that shows us what data is already materialized, the last step before workflow execution is query planning: determining what to compute and where.


The bird's eye view provided by Lance allows us to build a detailed query plan with a simple algorithm: We take the difference between the ideal state (all columns are fully populated) and the actual state of the dataset.


A B C D


Fragment 0 1


2


3


-2


Fragment 1 2


4


6


-4


Fragment 2 4


8


93


284


Fragment 3 3


6


9


-6


Fragment 4 5


10


15


-10


Initial state


Step 1 of 6


Hover to explore


With the dependency graph and Lance's view of materialized data, query planning becomes a diff: compare the ideal state (all columns populated) against actual state to find what's missing. A topological sort algorithm ensures each column computes after its dependencies, and per-fragment granularity means execution can parallelize across cores or machines. Checkpoints after each fragment avoid redoing work if interrupted.


This gives exa-d a single execution rule: compute missing or invalid columns. Whether a column is missing because it's a new document or because the embedding model changed, the codepath is the same. Backfills and incremental updates follow the same codepath.


## # Pipelined Execution on Ray Data


Under the hood, exa-d translates the topologically sorted column graph into[Ray Data](https://docs.ray.io/en/latest/data/data.html)1 jobs. Scheduling is gated by fragment completeness, so Ray only sees work items that actually need computation. Expressing each node in the dependency graph as a Ray Data pipeline stage creates separate workers for each Column.


Ray Actor


Model


Fragment 0


Fragment 1


Fragment 2


Fragment 3


Fragment 4


Fragment 5


Fragment 6


Fragment 7


Initial state: All fragments materialized


Step 1 of 5


Hover to explore


Loading an embedding model into GPU memory can take seconds to minutes depending on model size and latency stacks across the scale of updated fragments. exa-d uses Ray Actorsd d Actor = a stateful worker. It's a class instance that gets initialized once and stays alive to process multiple items. The opposite is a stateless task, which spins up, does one thing, and dies.


to load the embedding model once and wait in memory for the next batch of fragments that needs to be updated. Since scheduling is gated by fragment completeness, actors only receive fragments that require recomputation, avoiding redundant inference on already-materialized data.


Separate Actor stages give us pipeline parallelism. If a single worker computed all Columns, the GPU would sit idle during S3 downloads and tokenization. With separate Actors, each resource runs at capacity: the GPU embeds one fragment while the CPU tokenizes the next and the network fetches a third.


## # DAG Example


A small synthetic example makes the execution model concrete: define a dependency DAG of derived columns, point it at a dataset where fragments have only some of those columns, and the system materializes only what's missing.


```text
A   =   Column  (  "A"  ,     int  )      # base column already in the dataset
B   =   Column  (  "B"  ,     int  )  .  derive  (  )  .  impl_from  (  A  ,     lambda   a  :   a   *     2  )                # row-wise    C   =   Column  (  "C"  ,     int  )  .  derive  (  )  .  impl_actor_from  (  A  ,   TimesThreeActor  )          # stateful worker (cached model)    D   =   Column  (  "D"  ,     int  )  .  derive  (  )  .  impl_batch_from  (  B  ,   negate_batch  )             # batch map    E   =   Column  (  "E"  ,     int  )  .  derive  (  )  .  from_  (  B  )  .  from_  (  C  )  .  impl  (  lambda   b  ,   c  :   b  +  c  )     # multi-dependency
ds   =   Dataset  (  "s3://bucket/index.lance"  )
execute_columns  (        dataset  =  ds  ,        output_columns  =  [  B  ,   C  ,   D  ,   E  ]  ,     )
```


The important property is convergence: if execution is rerun after a partial failure, it will eventually reach the same end state where all outputs are computed correctly. Same as usual, exa-d observes missing and valid outputs, recomputes the diff and picks up where it left off.


## # Where we're going from here


The web's properties shaped exa-d's design: heterogeneous content, varying update frequencies, compounding derived artifacts. Typed columns, surgical patching, a declared dependency graph. Each choice followed from the constraints we were working within.


But constraints change as scale and workloads evolve, and our approach is evolving with them. We are in the process of building new iterations of this framework. For now, exa-d remains our answer to the core challenge: maintaining derived state over an index with billions of documents for storing and retrieving information from the entire web in a database.


If you are excited about working on data systems at web scale, contribute to the next iterations of Exa's pipelines.


[See open roles](https://exa.ai/careers)


## # Acknowledgements


We thank Jan van der Vegt and Summer Devlin for lending their expertise across drafts of this post.


## # Citations


1. 1.


[Ray Data](https://docs.ray.io/en/latest/data/data.html) is a scalable data processing library for AI workloads built on Ray.
