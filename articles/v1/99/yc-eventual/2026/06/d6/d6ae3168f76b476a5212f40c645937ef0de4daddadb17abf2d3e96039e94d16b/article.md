---
schema_version: "1.0.0"
document_id: "d6ae3168f76b476a5212f40c645937ef0de4daddadb17abf2d3e96039e94d16b"
company_key: "yc-eventual"
company: "Eventual"
source_id: "yc-eventual-news-import-1ca85739ec11"
canonical_url: "https://www.eventual.ai/blog/daft-v0-7-16"
published_at: "2026-06-30T08:00:00+00:00"
first_seen_at: "2026-07-25T03:47:26.048705+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:069aed909492a568e7203178dd869ca23447fc97c6ec09fc12732b3a4b6c00b7"
---

# Daft v0.7.16: DROID Robotics Dataset, PyTorch DataLoader, and Resilient File Reads

Daft v0.7.16 ships the building blocks for robotics data pipelines alongside resilience features that keep overnight batch jobs alive when files go bad.


**TLDR:**


- ` daft.datasets.droid` gives you a DataFrame API over the DROID robotics dataset — 76k demonstration episodes, camera feeds, and natural language annotations
- ` DataFrame.to_torch_dataloader` connects Daft directly to PyTorch training loops without an intermediate dataset abstraction
- ` ignore_corrupt_files` skips bad files instead of aborting your entire job, with structured observability so nothing gets silently dropped


44 improvements from 22 contributors across robotics data access, ML integration, and production resilience.


> Every code example in this post is a self-contained[PEP 723](https://peps.python.org/pep-0723/) script. Copy it into a file and run it with` uv run script.py` — dependencies install automatically. Don't have uv?[Install it here.](https://docs.astral.sh/uv/getting-started/installation/)


## DROID Robotics Dataset


The[DROID dataset](https://droid-dataset.github.io/) contains 76,000 robot manipulation demonstrations across 564 scenes and 86 tasks — one of the largest open-source robotics datasets available. Loading it previously meant writing custom HDF5 readers and stitching together camera feeds manually.


` daft.datasets.droid` wraps the dataset in Daft's lazy DataFrame API. You get distributed reads, predicate pushdown, and multimodal column handling out of the box.


```text
import   daft
from   daft.datasets   import   droid


df   =   droid.raw()    # lazy — no data fetched yet


(
df
.where(daft.col(  "  success  "  ))
.where(daft.col(  "  building  "  )   ==   "  Ross  "  )
.select(  "  uuid  "  ,   "  current_task  "  ,   "  trajectory_length  "  ,   "  wrist_video  "  )
.limit(  10  )
.show()
)
```


The dataset is ~8.7 TB on GCS with anonymous access to the public bucket. Daft's lazy evaluation means only the data you actually select and filter gets read. Columns include episode metadata (uuid, building, success, task, trajectory length), camera extrinsics, and video files (wrist, exterior cameras) as lazy` VideoFile` references.


Trajectory HDF5 reading (sensor, observation, and state data) and RLDS format support are coming in a follow-up release.


Contributed by[@srilman](https://github.com/srilman) in[#7089](https://github.com/Eventual-Inc/Daft/pull/7089) .


## Native PyTorch DataLoader


Training loops need data served through` torch.utils.data.DataLoader` . Until now, connecting Daft to PyTorch meant writing a custom` Dataset` wrapper that bridged two different iteration models.


` DataFrame.to_torch_dataloader` eliminates that glue code. Daft streams batched partitions directly as` dict\[str, torch.Tensor\]` , matching the streaming semantics that Daft already uses internally.


```text
# /// script
# description = "Stream a DataFrame into PyTorch as batched tensors"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.16", "torch"]
# ///
import   daft


df   =   daft.from_pydict({
"  feature_a  "  : [  1.0  ,   2.0  ,   3.0  ,   4.0  ,   5.0  ,   6.0  ],
"  feature_b  "  : [  10  ,   20  ,   30  ,   40  ,   50  ,   60  ],
"  label  "  : [  0  ,   1  ,   0  ,   1  ,   0  ,   1  ],
})


for   batch   in   df.to_torch_dataloader(  batch_size  =  2  ):
print  (  f  "features:   {  batch[  '  feature_a  '  ].shape  }  , labels:   {  batch[  '  label  '  ]  }  "  )
```


Numeric types become tensors automatically. Non-numeric columns come through as plain Python lists.` pin_memory=True` pins tensors to CUDA-accessible memory when a GPU is available.


This pairs naturally with the DROID dataset API — load robot demonstrations with` daft.datasets.droid` , transform them with Daft expressions, and feed the result directly into a training loop.


Contributed by[@srilman](https://github.com/srilman) in[#6997](https://github.com/Eventual-Inc/Daft/pull/6997) .


## Resilient File Reads with ignore_corrupt_files


A single corrupt Parquet file in a data lake can abort an overnight batch job that was hours into processing. The failure is correct behavior — but it's also expensive when 99.99% of the data is fine.


` ignore_corrupt_files=True` skips unreadable files (bad magic bytes, truncated footers, corrupt row-group data) and keeps processing. Every skipped file is recorded in` df.skipped_corrupt_files` — a list of` (path, reason, partial)` tuples available once you call` .collect()` .` partial` is` True` when a file emitted some valid batches before corruption was hit, so you know whether you lost a whole file or just the tail of one.


```text
# /// script
# description = "Demonstrate ignore_corrupt_files with a mix of valid and invalid parquet data"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.16"]
# ///
import   daft
import   tempfile, os


tmpdir   =   tempfile.mkdtemp()
bad_path   =   os.path.join(tmpdir,   "  bad.parquet  "  )


daft.from_pydict({  "  x  "  : [  1  ,   2  ,   3  ]}).write_parquet(tmpdir)
with   open  (bad_path,   "  wb  "  )   as   f:
f.write(  b  "  not a parquet file  "  )


df   =   daft.read_parquet(os.path.join(tmpdir,   "  *.parquet  "  ),   ignore_corrupt_files  =  True  )
df.collect()


for   path, reason, partial   in   df.skipped_corrupt_files:
tag   =   "   (partial)  "   if   partial   else   ""
print  (  f  "Skipped  {  tag  }   {  path  }  :   {  reason  }  "  )
```


Network errors, timeouts, and permission failures are never swallowed — those indicate infrastructure problems that need fixing, not data problems that can be skipped. If every matched file turns out to be corrupt, Daft still raises rather than returning an empty DataFrame. Available on` read_parquet` ,` read_csv` , and` read_iceberg` .


Full usage docs — what counts as corrupt per format, the` WARNING` -level log output, and production dead-letter-queue patterns — are at[Ignoring Corrupt Files](https://docs.daft.ai/en/stable/connectors/generic-file-source-options/#ignoring-corrupt-files) .


Contributed by[@chenghuichen](https://github.com/chenghuichen) in[#6520](https://github.com/Eventual-Inc/Daft/pull/6520) .


## Top-Level daft.concat()


Combining multiple DataFrames previously required chaining` .concat()` calls or writing a reduce loop.` daft.concat()` takes a list of DataFrames and concatenates them in one call.


```text
# /// script
# description = "Concatenate multiple DataFrames with daft.concat()"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.16"]
# ///
import   daft


jan   =   daft.from_pydict({  "  month  "  : [  "  Jan  "  ,   "  Jan  "  ],   "  revenue  "  : [  100  ,   120  ]})
feb   =   daft.from_pydict({  "  month  "  : [  "  Feb  "  ,   "  Feb  "  ],   "  revenue  "  : [  130  ,   110  ]})
mar   =   daft.from_pydict({  "  month  "  : [  "  Mar  "  ,   "  Mar  "  ],   "  revenue  "  : [  140  ,   150  ]})


combined   =   daft.concat([jan, feb, mar])
combined.show()
```


All input DataFrames must share a schema. Raises` ValueError` on empty input.


Contributed by[@Liusixuuu](https://github.com/Liusixuuu) in[#7105](https://github.com/Eventual-Inc/Daft/pull/7105) .


## Spark-Style Timezone Conversions


Migrating PySpark pipelines to Daft previously meant rewriting timezone conversion logic. Three new functions match Spark's temporal semantics directly:


```text
# /// script
# description = "Spark-style timezone conversions with from_utc_timestamp and to_utc_timestamp"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.16"]
# ///
import   daft
from   daft   import   col
from   daft.functions   import   from_utc_timestamp, to_utc_timestamp


df   =   daft.from_pydict({
"  event  "  : [  "  server_start  "  ,   "  deploy  "  ,   "  alert  "  ],
"  utc_time  "  : [  "  2026-06-26 14:00:00  "  ,   "  2026-06-26 18:30:00  "  ,   "  2026-06-27 02:15:00  "  ],
})


df   =   df.with_column(  "  utc_ts  "  , col(  "  utc_time  "  ).cast(daft.DataType.timestamp(  "  microseconds  "  )))
df   =   df.with_columns({
"  new_york  "  : from_utc_timestamp(col(  "  utc_ts  "  ),   "  America/New_York  "  ),
"  tokyo  "  : from_utc_timestamp(col(  "  utc_ts  "  ),   "  Asia/Tokyo  "  ),
})


df.select(  "  event  "  ,   "  utc_time  "  ,   "  new_york  "  ,   "  tokyo  "  ).show()
```


- ` from_utc_timestamp(ts, tz)` converts a UTC timestamp to local wall-clock time
- ` to_utc_timestamp(ts, tz)` converts local wall-clock time back to UTC
- ` convert_timezone(target_tz, ts)` matches Spark's reversed argument order


All three return timezone-naive timestamps, matching PySpark behavior. Available in both Python and SQL.


Contributed by[@BABTUNA](https://github.com/BABTUNA) in[#6919](https://github.com/Eventual-Inc/Daft/pull/6919) .


## String Distance and Similarity Functions


Fuzzy matching, deduplication, and entity resolution need string comparison metrics. Four new functions ship as pure Rust scalar UDFs with no external dependencies:


```text
# /// script
# description = "String distance and similarity functions for fuzzy matching"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.16"]
# ///
import   daft
from   daft   import   col
from   daft.functions   import   levenshtein_distance, jaro_similarity, jaro_winkler_similarity


df   =   daft.from_pydict({
"  name  "  : [  "  martha  "  ,   "  dwayne  "  ,   "  dixon  "  ],
"  candidate  "  : [  "  marhta  "  ,   "  duane  "  ,   "  dicksonx  "  ],
})


df   =   df.with_columns({
"  edit_dist  "  : levenshtein_distance(col(  "  name  "  ), col(  "  candidate  "  )),
"  jaro  "  : jaro_similarity(col(  "  name  "  ), col(  "  candidate  "  )),
"  jaro_winkler  "  : jaro_winkler_similarity(col(  "  name  "  ), col(  "  candidate  "  )),
})


df.show()
```


- ` levenshtein_distance` — minimum edit distance (Int64)
- ` jaro_similarity` — similarity score from 0.0 to 1.0 (Float64)
- ` jaro_winkler_similarity` — Jaro with a prefix bonus (Float64)
- ` damerau_levenshtein_distance` — Levenshtein plus transpositions (Int64)


All four are null-safe and available as expression methods (` col("a").levenshtein_distance(col("b"))` ).


Contributed by[@nish2292](https://github.com/nish2292) in[#7068](https://github.com/Eventual-Inc/Daft/pull/7068) .


## File Existence Checks


Unclean datasets reference files that may have been deleted, moved, or never uploaded. The` file_exists` expression checks whether a path resolves to an actual file before you try to read it.


```text
import   daft
from   daft   import   col
from   daft.functions   import   file, file_exists


df   =   daft.from_pydict({
"  path  "  : [  "  s3://bucket/data/good.parquet  "  ,   "  s3://bucket/data/missing.parquet  "  ],
})


df   =   df.with_column(  "  exists  "  , file_exists(file(col(  "  path  "  ))))
df   =   df.where(col(  "  exists  "  ))
df.show()
```


Filter out missing files before a batch read instead of failing halfway through the job. The DROID dataset module uses this internally to handle episodes with missing camera recordings.


Contributed by[@srilman](https://github.com/srilman) in[#7140](https://github.com/Eventual-Inc/Daft/pull/7140) .


## Everything Else


**Iceberg Enhancements** :` ignore_corrupt_files` support for` read_iceberg` in SQL, plus branch and tag reads for time-travel queries ([@jackylee-ch](https://github.com/jackylee-ch)[#7130](https://github.com/Eventual-Inc/Daft/pull/7130) ,[#7084](https://github.com/Eventual-Inc/Daft/pull/7084) )


**Delta Lake Column Mapping** : Read Delta tables that use column mapping for reads — a common pattern in Databricks-managed tables ([@aaron-ang](https://github.com/aaron-ang)[#7005](https://github.com/Eventual-Inc/Daft/pull/7005) )


**Spark-Compatible String Functions** :` translate` ,` substring_index` ,` soundex` ,` ascii` ,` chr` ,` space` — closing more Spark migration gaps ([@XuQianJin-Stars](https://github.com/XuQianJin-Stars)[#7070](https://github.com/Eventual-Inc/Daft/pull/7070) )


**Range Partition Hints** : Specify range-based partition hints in clustering specs for workloads where hash partitioning creates skew ([@euanlimzx](https://github.com/euanlimzx)[#7050](https://github.com/Eventual-Inc/Daft/pull/7050) )


**Ray Dynamic Scale-In** :` RaySwordfishActor` now supports dynamic scale-in, releasing idle workers back to the Ray cluster ([@huleilei](https://github.com/huleilei)[#5903](https://github.com/Eventual-Inc/Daft/pull/5903) )


**Inline Aggregation Performance** : Specialized` BoolAnd` ,` BoolOr` , and` Product` accumulator types for faster inline aggregations ([@BABTUNA](https://github.com/BABTUNA)[#6984](https://github.com/Eventual-Inc/Daft/pull/6984) ,[#6975](https://github.com/Eventual-Inc/Daft/pull/6975) )


**Scan Size Estimation** : Scan tasks now estimate size from materialized buffers and Parquet metadata instead of encoded size, improving partition planning ([@madvart](https://github.com/madvart)[#7161](https://github.com/Eventual-Inc/Daft/pull/7161) ,[@desmondcheongzx](https://github.com/desmondcheongzx)[#6542](https://github.com/Eventual-Inc/Daft/pull/6542) )


## Core Team Contributions


- [@srilman](https://github.com/srilman) built the DROID dataset API,` to_torch_dataloader` , and the` file_exists` expression.
- [@euanlimzx](https://github.com/euanlimzx) implemented range partition hints and ASOF join tests.
- [@rchowell](https://github.com/rchowell) added the scalar` #\[daft_func\]` proc macro for the extension system.
- [@madvart](https://github.com/madvart) improved scan size estimation from materialized buffers.
- [@desmondcheongzx](https://github.com/desmondcheongzx) fixed Parquet metadata scan estimates and flotilla node churn handling.
- [@rohitkulshreshtha](https://github.com/rohitkulshreshtha) gated vllm to Linux and bumped the Ray floor.
- [@colin-ho](https://github.com/colin-ho) collapsed the docs sidebar navigation.


## Community Contributions


13 external contributors shipped features and fixes in v0.7.16:


- [@BABTUNA](https://github.com/BABTUNA) added Spark-style timezone conversions and inline aggregation accumulators.
- [@chenghuichen](https://github.com/chenghuichen) implemented` ignore_corrupt_files` with structured observability.
- [@Liusixuuu](https://github.com/Liusixuuu) added top-level` daft.concat()` . First contribution.
- [@nish2292](https://github.com/nish2292) shipped four string distance/similarity functions. First contribution.
- [@XuQianJin-Stars](https://github.com/XuQianJin-Stars) added Spark-compatible string functions and upgraded OpenDAL.
- [@jackylee-ch](https://github.com/jackylee-ch) enhanced Iceberg with` ignore_corrupt_files` SQL support and branch/tag reads.
- [@aaron-ang](https://github.com/aaron-ang) added Delta Lake column mapping support.
- [@huleilei](https://github.com/huleilei) built dynamic scale-in for Ray Swordfish actors.
- [@RitwijParmar](https://github.com/RitwijParmar) added OTEL resource config support and MCAP HTTP URL handling. First contribution.
- [@XiaoHongbo-Hope](https://github.com/XiaoHongbo-Hope) fixed actor UDF deadlocks and unschedulable UDF detection. First contribution.
- [@TechyMT](https://github.com/TechyMT) fixed plan-cache fingerprint uniqueness. First contribution.
- [@YuangGao](https://github.com/YuangGao) fixed session namespace and table resolution.
- [@qingfeng-occ](https://github.com/qingfeng-occ) fixed PostgreSQL read options via Gravitino.
- [@mikedep333](https://github.com/mikedep333) upgraded arrow-rs from 57.1 to 59.0.


## Upgrade


```text
uv   add   "  daft>=0.7.16  "
```


Or try the latest nightly:


```text
uv   pip   install   daft   --pre   --extra-index-url   https://nightly.daft.ai
```


Check the[full changelog](https://github.com/Eventual-Inc/Daft/releases/tag/v0.7.16) for the complete list of merged PRs.


## Join the Community


Questions about the DROID dataset API, PyTorch DataLoader integration, or resilient file reads? Connect with the Daft community:


- [Slack](https://daft.ai/slack) — Daily discussions and real-time support
- [GitHub Discussions](https://github.com/Eventual-Inc/Daft/discussions) — Technical deep dives and feature requests
- [Documentation](https://docs.daft.ai/) — Complete API reference and tutorials
