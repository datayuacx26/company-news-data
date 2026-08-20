---
schema_version: "1.0.0"
document_id: "0cbd6e0fc4784711e95a855dc8d6d987532389fead2f20eddf0bdec3f36e7253"
company_key: "yc-eventual"
company: "Eventual"
source_id: "yc-eventual-news-import-1ca85739ec11"
canonical_url: "https://www.eventual.ai/blog/daft-v0-7-15"
published_at: "2026-06-07T08:00:00+00:00"
first_seen_at: "2026-07-25T03:47:26.048705+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:8e61dce2abaa46f9ea42fda04b041cbb4f78a42bbb55195187523dfeb0a10e5f"
---

# Daft v0.7.15: Safe Type Conversions, Flight Shuffle Optimizations, and PostgreSQL Support

**TLDR:**


- try_cast() brings safe type conversion with null fallbacks instead of runtime errors
- Flight shuffle now defaults to LZ4 compression — frames stay compressed across disk and the wire, ~2.3x faster shuffle on EBS gp3
- UUIDv7 timestamp extraction enables time-based partitioning on UUID columns


Data transformations fail when type conversions encounter unexpected values. Shuffle performance becomes a bottleneck when workers exchange data across the network. Time-based queries struggle with UUID primary keys that embed temporal information.


Daft v0.7.15 addresses each of these pain points with 55+ improvements from 21 contributors, spanning safe type handling, shuffle optimizations, and expanded data source support.


## Safe Type Conversion with try_cast()


Type conversion errors halt pipelines when source data doesn't match schema expectations. The new` try_cast()` function converts types safely, returning null for invalid values instead of throwing exceptions.


```text
# /// script
# description = "Safe type conversion with try_cast function"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.15"]
# ///
import   daft


df   =   daft.from_pydict({
"  values  "  : [  "  123  "  ,   "  invalid  "  ,   "  456  "  ,   ""  ,   "  789  "  ],
"  timestamps  "  : [  "  2024-01-01  "  ,   "  not-a-date  "  ,   "  2024-12-31  "  ,   None  ,   "  2024-06-15  "  ]
})


df   =   df.with_columns({
"  safe_ints  "  : daft.col(  "  values  "  ).try_cast(daft.DataType.int64()),
"  safe_dates  "  : daft.col(  "  timestamps  "  ).try_cast(daft.DataType.date())
})


df.show()
```


try_cast() succeeds where cast() would fail, converting "123" and "789" to integers while gracefully handling "invalid" and empty strings as null values. Critical for ETL pipelines processing messy real-world data.


Contributed by[@XuQianJin-Stars](https://github.com/XuQianJin-Stars) in[#6960](https://github.com/Eventual-Inc/Daft/pull/6960) .


## Flight Shuffle LZ4 Compression


Distributed joins and aggregations shuffle data between workers. Disk and network overhead dominated execution time for data-intensive operations. Daft v0.7.15 defaults Flight shuffle to LZ4 compression — frames are compressed once on the map side and stay compressed across disk and the wire. A 1 TB TPC-H repartition sweep shows LZ4 winning at every partition count: ~10% faster on local NVMe and ~2.3x faster on EBS gp3.


```text
# /// script
# description = "Use Flight Shuffle with LZ4 compression for a distributed shuffle"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.15"]
# ///
import   daft


daft.set_execution_config(
shuffle_algorithm  =  "  flight_shuffle  "  ,
flight_shuffle_compression  =  "  lz4  "  ,
)
cfg   =   daft.context.get_context().daft_execution_config
print  (  "  shuffle_algorithm =  "  , cfg.shuffle_algorithm)
print  (  "  flight_shuffle_compression =  "  , cfg.flight_shuffle_compression)


df   =   daft.from_pydict({
"  user_id  "  : [i   %   1000   for   i   in   range  (  100_000  )],
"  amount  "  :   list  (  range  (  100_000  )),
})


df.groupby(  "  user_id  "  ).agg(daft.col(  "  amount  "  ).sum().alias(  "  total  "  )).sort(  "  user_id  "  ).show()
```


LZ4 is cheap enough to encode that the compression pays for itself even on fast local disks, and the win grows as storage gets slower. Set` flight_shuffle_compression="none"` to restore the previous uncompressed behavior.


Contributed by[@colin-ho](https://github.com/colin-ho) in[#7071](https://github.com/Eventual-Inc/Daft/pull/7071) and[#6979](https://github.com/Eventual-Inc/Daft/pull/6979) .


## UUIDv7 Timestamp Extraction


UUIDv7 embeds timestamps in the first 48 bits, enabling time-based partitioning and filtering on UUID columns. The new timestamp extraction functions unlock temporal analytics on UUID primary keys.


```text
# /// script
# description = "Extract the timestamp embedded in UUIDv7 values for partitioning"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.15"]
# ///
import   daft
from   daft   import   col
from   daft.functions   import   extract_day_uuid7, extract_hour_uuid7, uuid


df   =   daft.from_pydict({
"  event  "  : [  "  login  "  ,   "  purchase  "  ,   "  refund  "  ],
})
df   =   df.with_column(  "  id  "  , uuid(  version  =  "  v7  "  ))
df   =   df.with_columns({
"  hour_bucket  "  : extract_hour_uuid7(col(  "  id  "  )),
"  day_bucket  "  : extract_day_uuid7(col(  "  id  "  )),
})
df.select(  "  event  "  ,   "  id  "  ,   "  hour_bucket  "  ,   "  day_bucket  "  ).show()
```


UUIDv7 timestamp extraction enables efficient time-based partitioning schemes without additional timestamp columns. Particularly valuable for event sourcing and audit log systems using UUID primary keys.


Contributed by[@jaychia](https://github.com/jaychia) in[#7032](https://github.com/Eventual-Inc/Daft/pull/7032) .


## PostgreSQL Data Source Support


Daft now reads PostgreSQL tables directly via Gravitino catalog integration, expanding structured data source support beyond cloud object stores.


```text
from   daft.catalog   import   Catalog


catalog   =   Catalog.from_gravitino(
endpoint  =  "  http://localhost:8090  "  ,
metalake_name  =  "  your_metalake  "  ,
)


df   =   catalog.read_table(  "  postgres_catalog.public.users  "  )
df.show()
```


Query a PostgreSQL table and a Parquet dataset in S3 in the same job, without an export step in between.


Contributed by[@qingfeng-occ](https://github.com/qingfeng-occ) in[#6989](https://github.com/Eventual-Inc/Daft/pull/6989) .


## ASOF Join Enhancements


ASOF (As-Of) joins match records based on temporal proximity rather than exact equality. Daft v0.7.15 adds nearest ASOF joins and aligned partition assumptions for financial time series and sensor data analytics.


```text
# /// script
# description = "Match each trade to the nearest quote with an as-of join"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.15"]
# ///
import   daft


trades   =   daft.from_pydict({
"  ts  "  : [  100  ,   250  ,   400  ],
"  symbol  "  : [  "  AAPL  "  ,   "  AAPL  "  ,   "  MSFT  "  ],
"  price  "  : [  150.0  ,   155.0  ,   200.0  ],
}).sort(  "  ts  "  )


quotes   =   daft.from_pydict({
"  ts  "  : [  90  ,   260  ,   395  ,   500  ],
"  bid  "  : [  149.5  ,   154.8  ,   199.7  ,   153.0  ],
}).sort(  "  ts  "  )


trades.join_asof(quotes,   on  =  "  ts  "  ,   strategy  =  "  nearest  "  ).sort(  "  ts  "  ).show()
```


Nearest ASOF joins find the temporally closest match regardless of whether it occurs before or after the target timestamp. Assumes sorted and aligned partitions boost performance for pre-sorted time series data.


Contributed by[@euanlimzx](https://github.com/euanlimzx) in[#6953](https://github.com/Eventual-Inc/Daft/pull/6953) and[#7067](https://github.com/Eventual-Inc/Daft/pull/7067) .


## Window Function Performance


Window operations now use specialized Series return paths, reducing memory copies during finalization. first_value() and last_value() aggregations join the window function library for ranking and analytical queries.


```text
# /// script
# description = "first_value and last_value over an ordered window"
# requires-python = ">=3.12"
# dependencies = ["daft==0.7.15"]
# ///
import   daft
from   daft   import   Window, col


sales   =   daft.from_pydict({
"  region  "  : [  "  North  "  ,   "  North  "  ,   "  North  "  ,   "  South  "  ,   "  South  "  ],
"  month  "  : [  1  ,   2  ,   3  ,   1  ,   2  ],
"  revenue  "  : [  100  ,   120  ,   90  ,   80  ,   95  ],
})


w   =   (
Window().partition_by(  "  region  "  ).order_by(  "  month  "  )
.rows_between(Window.unbounded_preceding, Window.unbounded_following)
)
sales   =   sales.with_columns({
"  first_month_rev  "  : col(  "  revenue  "  ).first_value().over(w),
"  last_month_rev  "  : col(  "  revenue  "  ).last_value().over(w),
})
sales.sort([  "  region  "  ,   "  month  "  ]).show()
```


Returning Series from window ops instead of full RecordBatches cuts peak memory on partition-heavy window queries by up to ~39% (benchmarked across partition and frame configurations). first_value() and last_value() enable lead/lag analytics without manual offset calculations.


Contributed by[@euanlimzx](https://github.com/euanlimzx) in[#6974](https://github.com/Eventual-Inc/Daft/pull/6974) ,[#7006](https://github.com/Eventual-Inc/Daft/pull/7006) , and[#7011](https://github.com/Eventual-Inc/Daft/pull/7011) .


## Everything Else


**Video Processing** : video_frames() supports configurable sampling intervals via sample_interval_seconds parameter ([@TheR1sing3un](https://github.com/TheR1sing3un)[#6832](https://github.com/Eventual-Inc/Daft/pull/6832) )


**Iceberg Integration** : Branch and tag reads for Iceberg tables, plus auto-configuration for Alibaba Cloud OSS ([@jackylee-ch](https://github.com/jackylee-ch)[#7042](https://github.com/Eventual-Inc/Daft/pull/7042) ,[@plusplusjiajia](https://github.com/plusplusjiajia)[#6993](https://github.com/Eventual-Inc/Daft/pull/6993) )


**Join Optimization** : A specialized code path for hash joins on integer keys — roughly 2x faster dedupe at small scale ([@srilman](https://github.com/srilman)[#6644](https://github.com/Eventual-Inc/Daft/pull/6644) )


**Observability** : Distributed checkpoint counters and cross-sink helpers for production monitoring ([@rohitkulshreshtha](https://github.com/rohitkulshreshtha)[#7026](https://github.com/Eventual-Inc/Daft/pull/7026) ,[#6932](https://github.com/Eventual-Inc/Daft/pull/6932) )


**Cloud Storage** : GCS object store delete operations and per-column compression configuration ([@daiping8](https://github.com/daiping8)[#6958](https://github.com/Eventual-Inc/Daft/pull/6958) ,[@rchowell](https://github.com/rchowell)[#6884](https://github.com/Eventual-Inc/Daft/pull/6884) )


## Community Contributions


12 external contributors expanded Daft's capabilities across type safety, data source integration, and cloud storage:


- [@XuQianJin-Stars](https://github.com/XuQianJin-Stars) implemented try_cast safe type conversion.
- [@qingfeng-occ](https://github.com/qingfeng-occ) integrated PostgreSQL via Gravitino.
- [@jackylee-ch](https://github.com/jackylee-ch) enhanced Iceberg with branch/tag reads.
- [@TheR1sing3un](https://github.com/TheR1sing3un) added video frame sampling intervals.
- [@daiping8](https://github.com/daiping8) implemented GCS delete operations.
- [@BABTUNA](https://github.com/BABTUNA) optimized multi-column aggregations.
- [@plusplusjiajia](https://github.com/plusplusjiajia) added Alibaba Cloud OSS support.
- [@aaron-ang](https://github.com/aaron-ang) refactored file byte-range fields.
- [@kyuds](https://github.com/kyuds) fixed Ray integration compatibility.
- [@ARDA7787](https://github.com/ARDA7787) improved retry jitter calculations.
- [@mikedep333](https://github.com/mikedep333) cleaned up feature gates.
- [@0xdeadd](https://github.com/0xdeadd) bumped the minimum PyArrow version to 16.


## Upgrade


```text
uv   add   "  daft>=0.7.15  "
```


Or try the latest nightly:


```text
uv   pip   install   daft   --pre   --extra-index-url   https://nightly.daft.ai
```


Check the[full changelog](https://github.com/Eventual-Inc/Daft/releases/tag/v0.7.15) for the complete list of merged PRs.


## Join the Community


Questions about try_cast, Flight shuffle tuning, or UUIDv7 extraction? Connect with the Daft community:


- [Slack](https://daft.ai/slack) — Daily discussions and real-time support
- [GitHub Discussions](https://github.com/Eventual-Inc/Daft/discussions) — Technical deep dives and feature requests
- [Documentation](https://docs.daft.ai/) — Complete API reference and tutorials
