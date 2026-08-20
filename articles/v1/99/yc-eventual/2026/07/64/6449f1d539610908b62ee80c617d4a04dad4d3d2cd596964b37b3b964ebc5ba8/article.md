---
schema_version: "1.0.0"
document_id: "6449f1d539610908b62ee80c617d4a04dad4d3d2cd596964b37b3b964ebc5ba8"
company_key: "yc-eventual"
company: "Eventual"
source_id: "yc-eventual-news-import-1ca85739ec11"
canonical_url: "https://www.eventual.ai/blog/daft-v0-7-17"
published_at: "2026-07-07T08:00:00+00:00"
first_seen_at: "2026-07-25T03:47:26.048705+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:a17273ec37ff86a9062e5244733bcfe081170e2e0ebd850bd58bc23ec6abed51"
---

# Daft v0.7.17: LeRobot Datasets, HDF5 Files, and Local LLM Inference

Daft v0.7.17 rounds out the robotics data stack that started with DROID last release, adds a second modality-native file type, and gives you a way to run prompts against open-weight models without standing up an inference service.


**TLDR:**


- ` daft.datasets.lerobot` reads LeRobot v3 datasets — episode metadata, frame-level sensor data, and camera video — as a lazy DataFrame with joins and video-frame decoding handled for you
- ` daft.Hdf5File` gives you typed accessors (` hdf5_keys` ,` hdf5_metadata` ,` hdf5_attrs` ) for inspecting HDF5 files without hand-rolling` h5py` traversal code
- ` prompt(..., provider="transformers")` runs open-weight instruction-tuned models locally through Hugging Face` pipeline("text-generation")` — no API key, no separate service
- ` daft.datasets.common_crawl` can now read from Hugging Face buckets, a credential-free alternative to the AWS S3 mirror for anyone outside` us-east-1`


18 changes from 10 contributors.


> Most code examples in this post are self-contained[PEP 723](https://peps.python.org/pep-0723/) scripts — copy one into a file, run it with` uv run script.py` , and dependencies install automatically. A few examples that need a real dataset, model download, or network fetch are shown for shape rather than as runnable scripts. Don't have uv?[Install it here.](https://docs.astral.sh/uv/getting-started/installation/)


## LeRobot v3 Dataset Support


[LeRobot Dataset v3.0](https://huggingface.co/docs/lerobot/lerobot-dataset-v3) is Hugging Face's format for robot learning data: chunked Parquet for episode and frame metadata, plus per-camera MP4 shards for video. Reading it previously meant writing your own joins between episode and frame tables and your own timestamp math to line up decoded video frames with sensor readings.


` daft.datasets.lerobot.read()` gives you one lazy DataFrame with one row per frame — it joins the episode metadata to the per-frame sensor data, and when you pass` load_video_frames` , decodes the matching camera image from its MP4 shard by timestamp.


```text
import   daft
from   daft.datasets   import   lerobot


# One row per frame, with the primary camera decoded into an image column
frames   =   lerobot.read(
"  your-org/your-robot-dataset  "  ,
load_video_frames  =  "  observation.image  "  ,
)
frames.select(  "  episode_index  "  ,   "  frame_index  "  ,   "  observation.image  "  ).show(  3  )
```


Video decoding requires the optional` av` and` pillow` dependencies (` pip install av pillow` ). The dataset URI can be a local directory, an` hf://datasets/org/name` path, or a bare` org/name` Hugging Face repo ID.


Contributed by[@srilman](https://github.com/srilman) in[#7090](https://github.com/Eventual-Inc/Daft/pull/7090) .


## HDF5 File Support


HDF5 shows up constantly in robotics and scientific data — DROID's per-episode trajectories are HDF5 underneath — but reading it in Daft previously meant dropping to raw` h5py` and managing file handles yourself.


` daft.Hdf5File` wraps a lazy file reference around HDF5's group/dataset model, and` hdf5_keys` ,` hdf5_metadata` , and` hdf5_attrs` expose the common inspection operations — listing group members, walking metadata, and reading attributes — as DataFrame expressions instead of imperative` h5py` calls.


```text
# /// script
# description = "Inspect an HDF5 file's structure with daft.Hdf5File expressions"
# requires-python = ">=3.12"
# dependencies = ["daft[hdf5]==0.7.17", "h5py"]
# ///
import   daft
import   h5py
import   tempfile
import   os


from   daft.functions   import   hdf5_file, hdf5_keys, hdf5_metadata


tmpdir   =   tempfile.mkdtemp()
path   =   os.path.join(tmpdir,   "  trajectory.h5  "  )
with   h5py.File(path,   "  w  "  )   as   f:
grp   =   f.create_group(  "  robot  "  )
grp.create_dataset(  "  joint_positions  "  ,   data  =  [[  0.1  ,   0.2  ,   0.3  ], [  0.4  ,   0.5  ,   0.6  ]])
grp.attrs[  "  units  "  ]   =   "  radians  "


df   =   daft.from_pydict({  "  path  "  : [path]})
df   =   df.with_column(  "  file  "  , hdf5_file(daft.col(  "  path  "  )))
df   =   df.with_column(  "  keys  "  , hdf5_keys(daft.col(  "  file  "  ),   group  =  "  /robot  "  ))
df   =   df.with_column(  "  metadata  "  , hdf5_metadata(daft.col(  "  file  "  ),   group  =  "  /robot  "  ))
df.select(  "  keys  "  ,   "  metadata  "  ).show()
```


For known layouts like DROID trajectories, pair` Hdf5File` with a typed` daft.func` -decorated UDF that reads specific datasets directly — that path skips the generic traversal entirely and returns typed tensor columns.


Contributed by[@everettVT](https://github.com/everettVT) in[#7159](https://github.com/Eventual-Inc/Daft/pull/7159) and[#7160](https://github.com/Eventual-Inc/Daft/pull/7160) .


## Local LLM Inference with the Transformers Provider


Every other` prompt()` provider assumes you're calling out to a hosted API. Sometimes you want offline batch inference against an open-weight model instead — no API key, no rate limits, no network dependency.


` provider="transformers"` runs Hugging Face's` pipeline("text-generation")` locally, on whichever device is available (CUDA, then MPS, then CPU). Generation kwargs like` max_new_tokens` and` temperature` forward straight to the pipeline call; constructor-time options like` dtype` and` trust_remote_code` go through` pipeline_kwargs` .


```text
import   daft
from   daft.functions   import   prompt


df   =   daft.from_pydict({  "  quote  "  : [  "  I am going to be the king of the pirates!  "  ]})


df   =   df.with_column(
"  response  "  ,
prompt(
daft.col(  "  quote  "  ),
provider  =  "  transformers  "  ,
model  =  "  HuggingFaceTB/SmolLM2-360M-Instruct  "  ,
system_message  =  "  Classify the anime from the quote in one short sentence.  "  ,
max_new_tokens  =  100  ,
),
)
df.show()
```


The same provider also accepts text-file columns directly — pass a` daft.File` or` bytes` value with a` text/*` MIME type and the contents get read and inlined into the prompt, wrapped in a tag the model can distinguish from your instructions. Install with` pip install 'daft\[transformers\]'` .


Contributed by[@YuangGao](https://github.com/YuangGao) in[#7152](https://github.com/Eventual-Inc/Daft/pull/7152) and[#7174](https://github.com/Eventual-Inc/Daft/pull/7174) .


## Common Crawl via Hugging Face Buckets


Common Crawl backs a large share of the web-scale text used to pretrain language models, but the canonical access path — AWS S3 in` us-east-1` — charges egress fees to everyone reading from outside that region.


` daft.datasets.common_crawl` now reads from a Hugging Face-hosted mirror as a credential-free alternative:` source="hf"` (or no argument at all, for 2026-and-later crawls) routes through` hf://buckets/commoncrawl/commoncrawl/...` instead of S3, so you get a` daft.read_warc` -backed DataFrame without an AWS account or region constraint.


```text
import   daft


# Reads from the HuggingFace bucket automatically for 2026+ crawls;
# pass source="hf" explicitly to force it for earlier crawls.
df   =   daft.datasets.common_crawl(  "  CC-MAIN-2026-25  "  ,   source  =  "  hf  "  ,   num_files  =  1  )
df.select(  "  WARC-Target-URI  "  ,   "  WARC-Type  "  ,   "  WARC-Date  "  ).show(  3  )
```


The HTTPS endpoint remains the fallback when neither S3 credentials nor the Hugging Face mirror are a fit.


Contributed by[@lhoestq](https://github.com/lhoestq) in[#7103](https://github.com/Eventual-Inc/Daft/pull/7103) .


## Faster Mixed Aggregations with any_value


Grouped aggregations that mixed` any_value()` with` sum()` ,` min()` , or` max()` on numeric columns used to fall back to Daft's slower general aggregation path entirely — one` any_value` call downgraded the whole query, even when every other aggregate qualified for the fast inline path.


` any_value` now has its own inline accumulator for numeric dtypes (Int8 through Int64, UInt8 through UInt64, Float32/Float64), so it stays on the fast path alongside the aggregates it's usually paired with — grabbing one representative row's metadata alongside a numeric rollup.


```text
# /// script
# description = "Mix any_value with sum/min/max in one groupby aggregation"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.17"]
# ///
import   daft
from   daft.functions   import   any_value


df   =   daft.from_pydict({
"  sensor  "  : [  "  a  "  ,   "  a  "  ,   "  a  "  ,   "  b  "  ,   "  b  "  ],
"  reading  "  : [  10.0  ,   12.0  ,   9.0  ,   100.0  ,   110.0  ],
"  unit  "  : [  "  celsius  "  ,   "  celsius  "  ,   "  celsius  "  ,   "  celsius  "  ,   "  celsius  "  ],
})


result   =   df.groupby(  "  sensor  "  ).agg(
df[  "  reading  "  ].sum().alias(  "  total  "  ),
df[  "  reading  "  ].min().alias(  "  low  "  ),
df[  "  reading  "  ].max().alias(  "  high  "  ),
any_value(df[  "  unit  "  ]).alias(  "  unit  "  ),
)
result.sort(  "  sensor  "  ).show()
```


Non-numeric` any_value` targets (strings, binary) still use the general path — this change is scoped to the numeric dtypes that already qualified for inline` sum` /` min` /` max` .


Contributed by[@BABTUNA](https://github.com/BABTUNA) in[#7036](https://github.com/Eventual-Inc/Daft/pull/7036) .


## Catalog-Qualified Namespace Resolution


When you're attached to more than one catalog,` create_namespace` ,` drop_namespace` , and` has_namespace` previously ignored the catalog qualifier on identifiers like` "warehouse.staging"` — only` create_table` and` drop_table` routed correctly. Namespace operations on a non-current catalog either landed in the wrong place or failed outright.


All three now resolve the catalog portion of a qualified identifier the same way table operations already did, so namespace management works correctly across every attached catalog, not just the current one.


```text
# /// script
# description = "Create a namespace in a non-current attached catalog by qualified name"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.17"]
# ///
import   daft
from   daft.catalog   import   Catalog


primary   =   Catalog.from_pydict({  "  orders  "  : {  "  id  "  : [  1  ,   2  ,   3  ]}},   name  =  "  primary  "  )
warehouse   =   Catalog.from_pydict({  "  events  "  : {  "  id  "  : [  1  ,   2  ,   3  ]}},   name  =  "  warehouse  "  )
daft.attach_catalog(primary,   alias  =  "  primary  "  )
daft.attach_catalog(warehouse,   alias  =  "  warehouse  "  )


daft.create_namespace(  "  warehouse.staging  "  )
print  (  "  staging lives in warehouse:  "  , daft.has_namespace(  "  warehouse.staging  "  ))
print  (  "  primary is unaffected:  "  , daft.has_namespace(  "  primary.staging  "  ))
```


Contributed by[@Lucas61000](https://github.com/Lucas61000) in[#7171](https://github.com/Eventual-Inc/Daft/pull/7171) .


## Everything Else


**Xet Reads for Hugging Face IO** : Support for Hugging Face's chunk-based Xet storage backend, using the` hf-xet` crate directly instead of routing through OpenDAL ([@srilman](https://github.com/srilman)[#7183](https://github.com/Eventual-Inc/Daft/pull/7183) )


**GooseFS Storage Backend** :` goosefs://` paths work with every standard reader and writer via a new` GooseFSConfig` , built on OpenDAL's` services-goosefs` backend ([@XuQianJin-Stars](https://github.com/XuQianJin-Stars)[#7109](https://github.com/Eventual-Inc/Daft/pull/7109) )


**Distributed Aligned Asof Joins** : An internal shuffle-skipping execution path for asof joins over already-sorted, aligned partitions, plus a carryover-correctness fix in the existing shuffle path that the new tests caught ([@euanlimzx](https://github.com/euanlimzx)[#7177](https://github.com/Eventual-Inc/Daft/pull/7177) )


**ignore_corrupt_files for Iceberg Table Reads** : The` ignore_corrupt_files` option introduced last release now forwards through` Table.from_iceberg(...).read(...)` too, not just the standalone` read_iceberg` path ([@jackylee-ch](https://github.com/jackylee-ch)[#7147](https://github.com/Eventual-Inc/Daft/pull/7147) )


**Scheme-less S3 Endpoints Fixed** : Some Iceberg REST catalogs vend an` s3.endpoint` value with no scheme (` s3.example.com` instead of` https://s3.example.com` ), which previously failed URL parsing and broke every read while writes silently succeeded. Daft now defaults to` https` , matching PyArrow's behavior ([@plusplusjiajia](https://github.com/plusplusjiajia)[#7111](https://github.com/Eventual-Inc/Daft/pull/7111) )


**Cleaner Rust Error Messages** : Nested Rust errors surfaced in Python as walls of` ProviderError { source: ... }` text. They're now formatted as a short, readable chain:


```text
DaftCoreException: Unable to load credentials for IO backend `s3`


Caused by the following errors:
1: an error occurred while loading credentials
2: service error
3: UnauthorizedException: Session token not found or invalid
```


([@srilman](https://github.com/srilman)[#7178](https://github.com/Eventual-Inc/Daft/pull/7178) )


**pillow Added to the video Extra** :` VideoFile` now raises an informative error instead of an opaque import failure when Pillow is missing, and` pillow` is pinned into the` video` extra so` pip install 'daft\[video\]'` pulls it automatically ([@everettVT](https://github.com/everettVT)[#7213](https://github.com/Eventual-Inc/Daft/pull/7213) )


## Core Team Contributions


- [@srilman](https://github.com/srilman) built the LeRobot v3 dataset API, added Xet storage reads for Hugging Face IO, and reformatted nested Rust error chains into readable messages.
- [@everettVT](https://github.com/everettVT) shipped native HDF5 file support and fixed a missing Pillow dependency in the video extra.
- [@euanlimzx](https://github.com/euanlimzx) implemented the distributed execution path for aligned asof joins and fixed a carryover bug in the existing shuffle path along the way.


## Community Contributions


7 external contributors shipped features and fixes in v0.7.17:


- [@YuangGao](https://github.com/YuangGao) added the Transformers provider for` prompt()` and extended it to accept text-file inputs directly.
- [@lhoestq](https://github.com/lhoestq) added Hugging Face bucket access as a credential-free alternative source for Common Crawl.
- [@BABTUNA](https://github.com/BABTUNA) added the inline` any_value` accumulator and consolidated the Sum/Product/Min/Max inline accumulators into one macro.
- [@Lucas61000](https://github.com/Lucas61000) fixed catalog-qualified namespace resolution across attached catalogs.
- [@XuQianJin-Stars](https://github.com/XuQianJin-Stars) added GooseFS storage backend support via OpenDAL.
- [@jackylee-ch](https://github.com/jackylee-ch) forwarded` ignore_corrupt_files` through Iceberg catalog table reads.
- [@plusplusjiajia](https://github.com/plusplusjiajia) fixed scheme-less S3 endpoint URLs defaulting incorrectly.


## Upgrade


```text
uv   add   "  daft>=0.7.17  "
```


Or try the latest nightly:


```text
uv   pip   install   daft   --pre   --extra-index-url   https://nightly.daft.ai
```


Check the[full changelog](https://github.com/Eventual-Inc/Daft/releases/tag/v0.7.17) for the complete list of merged PRs.


## Join the Community


Questions about LeRobot datasets, HDF5 files, or running local LLM inference with the Transformers provider? Connect with the Daft community:


- [Slack](https://daft.ai/slack) — Daily discussions and real-time support
- [GitHub Discussions](https://github.com/Eventual-Inc/Daft/discussions) — Technical deep dives and feature requests
- [Documentation](https://docs.daft.ai/) — Complete API reference and tutorials
