---
schema_version: "1.0.0"
document_id: "c50c1363bf7a584c95e84d87f5090c397be5c7c7a00e1e1aea554c5e4943bbac"
company_key: "doubleverify-holdings-inc-common-stock"
company: "DoubleVerify Holdings Inc."
source_id: "doubleverify-holdings-inc-common-stock-rss-f6469e95d005"
canonical_url: "https://medium.com/doubleverify-engineering/improving-lakehouse-storage-cost-visibility-to-reduce-total-cost-of-ownership-tco-9f678e1735b9"
published_at: "2026-02-10T11:57:35+00:00"
first_seen_at: "2026-07-20T23:17:33.321656+00:00"
fetched_at: "2026-07-28T22:21:02.085623+00:00"
content_hash: "sha256:a55f350f6c4fd6e8c811c4e473f4ccba503a800f3e22bc12cc2230b85948c875"
---

# Improving Lakehouse Storage Cost Visibility to Reduce Total Cost of Ownership (TCO)

# Improving Lakehouse Storage Cost Visibility to Reduce Total Cost of Ownership (TCO)


[DV Engineering](https://medium.com/@dv-engineering?source=post_page---byline--9f678e1735b9---------------------------------------)


7 min read


·


Feb 10, 2026


--


*Written By:*[Saul Tawil](https://www.linkedin.com/in/saultawil/)


> Disclaimer: The views expressed in this post are based on the author’s engineering experience. Code snippets are simplified for illustration purposes. Results reflect one internal implementation and may vary by environment.
>
>
> Note: The discussion and code below sometimes reference Databricks Lakehouse terminology, using the Delta table format in GCP, but the principles should apply to any lakehouse, table format or cloud.


Lakehouse architectures are often sold on the premise of low TCO. Because storage is decoupled from compute, there is a tendency to view Lakehouses as “unlimited and cheap,” leading teams to throw data into the lake without thinking twice.


However, for large-scale data platforms, these costs can add up to significant monthly charges if not managed properly. We found that the hidden storage costs of Lakehouse footprints — what we internally refer to as “orphaned” files (unreferenced data) and inefficient column storage — were an area ripe for optimization.


Intelligent management of storage costs starts with **visibility** , followed by accountability and leadership support. This post details two concrete engineering strategies we developed to expose true table sizes and column-level footprints. **In our case, these visibility tools helped surface opportunities to drive double-digit percentage reductions in storage costs.**


## Part 1: True Table Size and Cost/Read Visibility


### The Challenge: Logical vs. Physical Size


Lakehouses don’t always display a table’s *true* size. A DESCRIBE command typically shows the size of the *referenced* data (the current snapshot). It does not account for:


1. **Unreferenced Files:** Files deleted by retention policies or OPTIMIZE operations that are no longer directly accessible via the Delta log’s current checkpoints but physically exist in storage (e.g., files waiting for a VACUUM command).
2. **Time Travel & Retention:** Data retained for historical queries or streaming replays, which bloats the physical footprint.
3. **Metadata:** Delta logs and parquet metadata like footers and dictionaries.


Furthermore, even if we know the cost, how do we determine the *value* ? A generic table cost doesn’t tell us if the dataset provides enough business value to justify its price tag.


### The Solution


To solve this, we moved beyond metadata summaries and built a “Physical Storage” calculator combined with a “Cost/Read” metric.


1. **Calculate Physical Storage** : We utilize cloud storage libraries to recursively calculate the size of the actual table path. This captures the full directory size, including active data, history, and unreferenced files.


*Code Snippet: Recursive scanning using cloud libraries (Illustrative)*


```text
# Note: Code simplified for illustration.  # Ensure all scanning complies with provider rate limits.   def get_storage_folder_size(bucket_name, folder_prefix):      """Get total size of objects in a storage folder recursively."""      if bucket_name and folder_prefix:          # Example using storage client, adaptable to S3/ADLS/GCS          blobs = client.list_blobs(bucket_name, prefix=folder_prefix)          total_size = sum(blob.size for blob in blobs)          return total_size   def process_row(row):      return {          "table_name": row["table_name"],          "physical_size": get_storage_folder_size(row["bucket"], row["prefix"]),          # ... timestamps and metadata      }
```


**2. Calculate Downstream Usage (Lineage)** : To determine value, we leverage system lineage tables. We don’t just look at direct reads; we’ve built a recursive query to find *downstream* dependencies. We have found some tables fueling dependencies multiple layers deep.


*Code Snippet: Recursive lineage analysis*


```text
# Loop to iteratively find all lineage paths  while True:      initial_df.createOrReplaceTempView("initial_df")      new_df = spark.sql(f"""          with lineage as (          {query}          )          select distinct              rl.source_table_full_name,              tl.target_table_full_name,              rl.level + 1 as level          from initial_df rl          join lineage tl          on rl.target_table_full_name = tl.source_table_full_name      """)      # ... Break if no new records found ...
```


**3. The “Cost/Read” Metric** : By combining the *Physical Storage Cost* with the *Total Read Count* (direct & downstream), we derive a **Cost/Read** metric. This allows us to sort tables by “inefficiency.” A table consuming meaningful monthly resources with zero reads is an immediate candidate for deletion.


### Table Size Dashboard


Visualize table sizes and Cost/Read for the entire lakehouse with a dashboard like this *(Note: Data in this dashboard has been mocked up and does not reflect any real company data):*


Press enter or click to view image in full size


*Figure 1: Our dashboard highlighting the delta between Logical Table Size and Physical Storage Size.*


**Key Findings:**


- **The “Unreferenced” Delta:** For tables with consistently small files (streaming sources) and standard retention settings, we found that physical storage was often **notably higher** than logical size due to files waiting for retention expiration.
- **Actionable Data:** We empowered teams to review retention settings and optimize VACUUM schedules on high-churn, low-value tables.


## Part 2: Column Size and Parquet Metadata Visibility


### The Challenge: Parquet Internals


While knowing the table cost is helpful, knowing *why* a table is expensive is better. The next step in our process was determining the storage footprint of individual columns.


This was difficult because Parquet is a semi-columnar format. You cannot simply ask the file system for the size of “Column A.” To get accurate data, we had to scan the Parquet footers of large volumes of files across our environment.


We faced several engineering hurdles:


1. **Catalog Restrictions:** Direct access to cloud files is often restricted by governance layers.
2. **Scale:** Efficiently listing large volumes of files requires handling cloud provider rate limits carefully.
3. **Complex Types:** Nested structs and arrays make simple sizing impossible without inspecting file metadata.


### The Solution: A Distributed Rust-Based Scanner


We built an internal scanning tool that inspects the physical layer using delta-rs (Rust bindings for Delta Lake).


> ***Note on Cloud Compatibility:*** *While our implementation example utilizes gcsfs for Google Cloud Storage, this logic is platform-agnostic. The same approach applies to other clouds.*


**Why Rust (delta-rs)?** Using standard frameworks to parse the transaction logs of thousands of tables can be computationally heavy. The JVM startup overhead and metadata parsing latency were bottlenecks. By switching to delta-rs, we could parse millions of log entries efficiently with minimal memory overhead, allowing us to “fan out” these checks in a read-only manner without spinning up massive compute clusters.


**Phase 1: Fast Metadata Extraction with Delta-RS** ; We used delta-rs in a distributed function to efficiently parse the Delta Transaction Log to retrieve the table’s referenced files and the Delta log file size.


*Code Snippet: Using delta-rs for log inspection (Illustrative)*


```text
def get_delta_files_worker(iterator):      """Worker function to extract Delta metadata using delta-rs library."""      from deltalake import DeltaTable as DeltaRsTable       for pdf in iterator:          results = []          for idx, row in pdf.iterrows():              storage_path = row['storage_path']              try:                  # Use Rust-based reader for speed                  dt = DeltaRsTable(storage_path, storage_options=storage_options)                   # Get active files directly from the log                  actions = dt.get_add_actions(flatten=True)                  if actions.num_rows > 0:                      file_paths = actions.column('path').to_pylist()                      # Calculate sizes...                   results.append({                      "table_name": row['table_name'],                      "referenced_files_json": json.dumps(file_paths),                      # ...                  })              except Exception as e:                  # ... handle errors ...
```


**Phase 2: The “Unreferenced File” Hunt** : We compared the “Referenced Files” list from the delta log with the “Physical Files” list from storage to identify unreferenced data.


*Code Snippet: Parallel storage scanning (Illustrative)*


```text
def scan_partition_worker(iterator):      # ... setup fs ...      for table in tables:          # Get all physical files (read-only)          all_files = fs.find(path, detail=True)           # Compare against Delta Log (referenced) files          orphaned_files = {fs_normalized[norm] for norm in orphaned_normalized}           # Calculate size of the "Ghost Data"          orphaned_parquet_bytes = sum(all_files[f]['size'] for f in orphaned_files)
```


**Phase 3: Parquet Footer Inspection** : Finally, to get the column sizes, we read the footer metadata of both the referenced and orphaned files and put them in their respective buckets. This allowed us to see the compressed size of every column, including nested fields.


*Code Snippet: Reading Parquet Metadata (Illustrative)*


```text
def scan_files_optimized(iterator):      # ... setup ...      def process_single_file(row):          pq_file = pq.ParquetFile(full_path, filesystem=fs)          local_sizes = {}           # Iterate over row group metadata footers to sum compressed column sizes          for rg_idx in range(pq_file.metadata.num_row_groups):              rg = pq_file.metadata.row_group(rg_idx)              for c_idx in range(rg.num_columns):                  chunk = rg.column(c_idx)                  # Map physical column to logical name for delta column mapping                  root_col = chunk.path_in_schema.split('.')[0]                  local_sizes[root_col] = local_sizes.get(root_col, 0) + chunk.total_compressed_size          return local_sizes
```


*Reference*[https://parquet.apache.org/docs/file-format/](https://parquet.apache.org/docs/file-format/) *for more on the Parquet file format.*


### Column Size Cost Dashboard — Main Tab


Visualize column sizes, cost, and table metadata with dashboards like these:


Press enter or click to view image in full size


*Figure 2: Our Column Cost Dashboard, grouping costs by column name across the entire organization. Note: The next step here is to add column usage (AKA reads) as well.*


### Column Size Cost Dashboard — Table Metadata Tab


Press enter or click to view image in full size


*Figure 3: Detailed view showing Parquet Overhead vs. Actual Data.*


**What this shows:** While Figure 2 focuses on the *logical* data (columns), this tab reveals the *physical* “packaging” costs. It separates the actual column data from the **Parquet Overhead** (file footers, bloom filters, and statistics) and **Delta Log** storage.


**Why it matters:** This view allows us to detect “inefficient storage” rather than just “large data.” For example, if the **Storage Type Metadata Distribution** (middle chart) shows a high percentage of Parquet Metadata relative to actual data, it is a clear indicator of the “small file problem” — where we are paying more to store file headers than the data itself. This signal immediately tells our engineers which tables require an OPTIMIZE job to compact files and reclaim that wasted budget.


### The Results


- **Duplicate Data:** We discovered specific heavy columns that were duplicated or minimally modified across Silver and Gold tables, consuming significant capacity.
- **Cost Savings:** By exposing that specific columns were consuming meaningful resources, teams immediately prioritized schema changes that led to sustainable monthly savings.


## Conclusion


Lakehouses offer incredible scalability, but “unlimited storage” should not mean “unlimited cost.”


Data platform teams must drive visibility. By looking deeper than standard metadata — analyzing the physical files and inspecting Parquet footers — we shifted the conversation from “We need more budget” to “We are optimizing our architecture.”


Most Lakehouse vendors today, being relatively young, do not yet provide these storage insights as we think they should or may do in the future. So, it is up to data platform teams to step up and provide this level of visibility.


These backend and dashboard tools, along with dashboard-based push alerts, empower teams to make data-driven decisions about their footprints, ensuring storage spend maps directly to business value.
