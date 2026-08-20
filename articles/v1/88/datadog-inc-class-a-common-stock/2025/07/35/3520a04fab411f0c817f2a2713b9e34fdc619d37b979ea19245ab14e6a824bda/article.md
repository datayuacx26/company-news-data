---
schema_version: "1.0.0"
document_id: "3520a04fab411f0c817f2a2713b9e34fdc619d37b979ea19245ab14e6a824bda"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/go-swiss-tables/"
published_at: "2025-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:e646412a8b034e9fd7e7171b2fa064f433417133fb24c403847734770697bc73"
---

# How Go 1.24’s Swiss Tables saved us hundreds of gigabytes

Nayef Ghattas


In[Part 1: How we tracked down a Go 1.24 memory regression across hundreds of pods](https://www.datadoghq.com/blog/engineering/go-memory-regression/) , we shared how upgrading to Go 1.24 introduced a subtle runtime regression that increased physical memory usage (RSS) across Datadog services. We worked with the Go community to identify the issue, validate the fix, and plan a safe rollout. But while tracking the rollout, we noticed something surprising: in our highest-traffic environments, memory usage didn’t just recover—it dropped significantly. That raised new questions: **What changed in Go 1.24 to make some workloads more memory efficient? And why wasn’t the improvement consistent across all environments?**


In this post, we’ll explore how Go’s new Swiss Tables implementation helped reduce memory usage in a large in-memory map, show how we profiled and sized the change, and share the struct-level optimizations that led to even larger fleet-wide savings.


To better understand why the memory usage decreased in our high-traffic environments, we started looking at live heap profiles for a given service. Looking more closely at a before-and-after profile comparison, here’s what we see:


We save roughly **500 MiB** of live heap usage on a map called` shardRoutingCache` in the` ShardRouter` package. If we[factor in the Go Garbage Collector](https://www.datadoghq.com/blog/go-memory-metrics/#heap-profiling) (GOGC), which is set to 100 by default, this leads to a reduction in memory usage of **1 GiB (i.e., 500 x 2)** .


When we include the expected ~400 MiB RSS increase from the` mallocgc` issue described in Part 1, we still see a **net 600 MiB memory usage decrease** !


But why is that, you might ask? First, let’s dig a bit deeper into the` shardRoutingCache` map and how it’s populated.


Some of our Go data-processing services use the` ShardRouter` package. As its name indicates, this package determines the target shard for incoming data based on routing keys. This is done by querying a database at startup and using the response to populate the` shardRoutingCache` map.


The map is laid out as follows:


```text
1   shardRoutingCache     map  [  string  ]  Response     // The key represents each routing key derived from the data payload    2
3   type     ShardType     int     // Enum representing the shard type (hot, warm, cold)    4
5   type     Response     struct   {    6       ShardID          int32    7       ShardType        ShardType    8       RoutingKey       string          // (We'll tackle later why we're storing the routing key both as a key and value!)    9       LastModified   *  time  .  Time      // Representing the timestamp at which the entry was last modified    10   }
```


### Estimating memory usage per entry


To better estimate the reduction in memory, let’s compute the size of each key–value pair. On 64-bit architectures, the key’s size is **16 bytes** (string header size). The size of each value would correspond to:


-


**4 bytes** for shardID` int32`


-


**8 bytes** for shardType` int`


-


**16 bytes** for the routingKey` string` headers


-


**8 bytes** for the` lastModifiedTimestamp` pointer


We’ll discuss the size of the routingKey strings themselves and the size of the` time.Time` structs` lastModifiedTimestamp` points to later (no spoilers!). For now, let’s just assume **the strings are empty, and the pointer is a nil pointer** .


This means that for each value we allocate: (4+8+16+8) = **36 bytes** , so **40 bytes** with[padding](https://go101.org/article/memory-layout.html#size-and-padding) . Overall, this leads to **56 bytes** for each key–value pair.


Most of the allocations happen on startup of the service when the database is queried. Meaning, this map is rarely inserted during the lifetime of the service. Later, we’ll see how that impacts the map size.


Now, let’s take a step back and look at how maps work in Go 1.23 versus Go 1.24 and how this impacts the size of the` shardRoutingCache` map.


## How Go 1.23’s bucket-based maps worked


### Bucket structure and layout


The runtime implementation for maps in Go 1.23 uses hash tables, arranged in an array of buckets. In the Go implementation, **the number of buckets is always a power of 2** (2n) and each bucket contains **8 slots** for storing the map’s key–value pairs. Let’s visualize this with an example **map that contains 2 buckets** :


When inserting a new key–value pair, the bucket in which the element is placed is determined by a hash function:` hash(key)` . Then, we have to scan all existing elements in the bucket to determine whether the key matches an existing key in the map:


-


**If there’s a match** , we need to update that key–value pair.


-


**Otherwise** , we just insert the element in the first empty slot.


Similarly, when reading an element from the map, we also have to scan all existing elements in the bucket. The scanning operation of all elements in a bucket for reads and writes is generally responsible for an important portion of the CPU overhead of map operations.


When a bucket is full—but other buckets still have plenty of space—new key–value pairs are added to overflow buckets that are linked to the original bucket. On each read/write operation, overflow buckets are also scanned to determine whether we’re inserting a new element or updating an existing element:


When an overflow bucket is full—but other buckets still have plenty of space—a new overflow bucket can be added and chained with the previous overflow bucket, and so on and so forth.


### Map growth and load factor


Finally, let’s discuss map growth and how it impacts the number of buckets. The initial number of buckets is determined by how the map is initialized:


```text
1   myMap   :=   make  (  map  [  string  ]  string  )    2   // This map will be initialized with one bucket    3
4   myMapWithPreallocation   :=   make  (  map  [  string  ]  string  ,   100  )    5   // For 100 elements, we need 100/8 = 12.5 buckets, the nearest power of 2    6   // is 2^4 = 16: the map will be initialized with 16 buckets
```


For each bucket, the **load factor** is the number of elements in the bucket over the size of the bucket. In the example above, bucket 0 has a load factor of **8/8** and bucket 1 has a load factor of **1/8** .


As the map grows, Go keeps track of the **average load factor** across all non-overflow buckets. When the average load factor is strictly higher than **13/16 (or 6.5/8)** , we’ll need to reallocate the map to a new one that has twice the number of buckets. Typically, the next insertion will create a new array of buckets—double the size of the previous one—and, theoretically, should also copy the contents of the old array of buckets to the new one.


Since Go is frequently used for latency-sensitive servers, we don’t want built-in operations to have arbitrarily large impact on latency. As a consequence, instead of copying the whole underlying map to the new one on a single insertion, Go pre-1.23 would keep around both arrays:


Incrementally, on each new write to the map, Go would move items from old buckets to new buckets:


**Note on load factors** : A high load factor means that, on average, each bucket will contain more key–value pairs. As a result, when inserting an element, we’ll need to scan more keys in each bucket to know whether we need to update an existing element or insert the element in an empty slot. This also applies for reading a value. On the other hand, a low load factor means that most of the buckets are sitting there empty, and leads to wasting memory.


### Estimating memory usage of the map


We should now have almost all the information we need to estimate the size of the` shardRoutingCache` map on Go 1.23. Luckily, we emit a custom metric that shows the number of elements stored in the map:


For approximately 3,500,000 elements with a maximum average load factor of **13/16** , we need at least:


-


**Required buckets** = 3,500,000 / (8 x 13/16) ≈ **538,462 buckets**


-


The smallest power of 2 greater than 538,462 is 220 = **1,048,576 buckets**


However, since 538,462 is close to 524,288 (219), and the` shardRoutingCache` map is rarely written to, **we can expect the old buckets to still be allocated** —we’re still transitioning from the old map to the new bigger map.


This means that the` shardRoutingCache` has 220 (new buckets) + 219 (old buckets) allocated, meaning **1,572,864 buckets** .


Each bucket structure contains:


-


An **overflow bucket pointer** (8 bytes on 64-bit architecture)


-


An **8-byte array** (used internally)


-


**8 key–value pairs** , of 56 bytes each: 56 × 8 = **448**


This means that **each bucket uses 464 bytes** in memory.


As a consequence, for Go 1.23, the bucket arrays total used memory is:


-


**1,572,864 buckets** × **464 bytes / bucket** = 729,808,896 bytes ≈ **696 MiB** .


This is only for the main bucket array, however. We also need to account for **overflow buckets** . For a well-distributed hash function, the number of overflow buckets should be relatively low.


The Go map implementation pre-allocates overflow buckets based on the bucket count (2n-4 overflow buckets). For 220 buckets, this means approximately **2** **16** **= 65,536 overflow buckets** might be pre-allocated, adding **65,536 overflow buckets** × **464 bytes per bucket** —about **30.4 MiB** in total.


Overall, the total estimated memory usage for the map is:


-


**Main bucket array (including old buckets) ≈ 696 MiB**


-


**Pre-allocated overflow buckets ≈ 30.4 MiB**


-


**Total ≈ 726.4 MiB**


This checks out with the observation in the live heap profile that showed around **930 MiB** of live heap usage for the` shardRoutingCache` map in Go 1.23: **730 MiB** for the map, and **200 MiB** for allocating the underlying strings for routing keys.


## How Swiss Tables and extendible hashing changed everything


Go 1.24 introduces a completely new map implementation based on[Swiss Tables](https://go.dev/blog/swisstable) and[extendible hashing](https://en.wikipedia.org/wiki/Extendible_hashing) . In Swiss Tables, data is stored into groups: Each group contains **8 slots** for key–value pairs, and a **64-bit control word** (8 byte). The number of groups in a Swiss Table is always a power of 2.


Let’s look at an example with a table that contains two groups:


Each byte in the control word is linked to a slot. The first bit indicates whether that slot is **empty** , **deleted** , or **in use** . If the slot is in use, the remaining 7 bits store the last bits of the key’s hash.


### Inserting entries in Swiss Tables


Let’s look at how a new key–value pair is inserted in this 2-group Swiss Table. First, we compute a 64-bit hash of the key,` hash(k1)` , and split it into two pieces: the first 57 bits are called` h1` , and the last 7 bits are` h2` .


We determine which group to use by computing` h1 mod 2` (since there are 2 groups). If` h1 mod 2 == 0` , we’ll try to store the key–value pair in group 0:


Before storing that pair, we check whether a key–value pair with the same key already exists in group 0. If it does, we need to update the existing pair; otherwise, we insert the new element into the first empty slot.


This is where Swiss Tables shine: previously, to do this, we had to linearly probe all the key–value pairs in a bucket.


In Swiss Tables, **the control word lets us do this more efficiently** . Since each byte contains the lower 7 bits of the hash (` h2` ) for that slot, we can first compare` h2` for the key–value pair we want to insert, with the 7 lower bits of each byte of the control word.


This operation is supported by[single instruction, multiple data (SIMD)](https://en.wikipedia.org/wiki/Single_instruction,_multiple_data) hardware, where this comparison is done with a **single CPU instruction** , in parallel across all 8 slots in the group. When specialized SIMD hardware is not available, this is implemented using standard **arithmetic and bitwise operations** .


**Note** : As of Go 1.24.2, SIMD-based map operations are not yet supported on` arm64` . You can follow (and upvote) the[GitHub issue tracking implementation for non- amd64 architectures](https://github.com/golang/go/issues/71255) .


### Handling full groups


What happens when all the slots in the group are full? Previously, in Go 1.23, we’ve had to create **overflow buckets** . With Swiss Tables, we store the key–value pair in the next group that has an available slot. Since probing is fast, the runtime can afford to check additional groups to determine whether the element already exists. The probing sequence stops when it reaches the first group with an **empty (non-deleted)** slot:


As a consequence, this fast probing technique allows us to eliminate the notion of **overflow buckets** .


### Group load factors and map growth


Now let’s look at how map growth works in Swiss Tables. As before, the **load factor** for each group is defined as the number of elements in the group divided by its capacity. In the example above, group 0 has a load factor of **8/8** and group 1 has a load factor of **2/8** .


Since probing is much faster with the control word, Swiss Tables use **a higher default maximum load factor (7/8)** , which will reduce memory usage.


When the average load factor is strictly higher than 7/8, we’ll need to re-allocate the map to a new one that has twice the number of buckets. Typically, the next insertion will split the table.


But what about limiting tail latency in latency-sensitive servers, as mentioned previously with the Go 1.23 implementation? If the map had thousands of groups, a single insert would pay the cost of moving and rehashing all the existing elements, which could take a lot of time:


Go 1.24 addresses this by setting a limit on how many groups can be stored in a single Swiss Table. A single table can store a maximum of **128 groups (1024 slots)** .


What if we want to store more than 1024 elements? This is where **extendible hashing** comes into play.


Rather than having a **single Swiss Table** implementing the entire map, each map is a **directory of one or more independent Swiss Tables** . Using **extendible hashing** , a variable number of the upper bits in the key hash is used to determine which table a key belongs to:


Extendible hashing allows for two things:


-


**Bounded group copying** : By limiting the size of a single Swiss Table, the number of elements that need to be copied when adding new groups is capped.


-


**Independent table splitting** : When a table reaches 128 groups, it is split into two 128-group Swiss Tables. This is the most expensive operation, but it’s bounded and occurs independently for each table.


As a result, for very large tables, **Go 1.24’s table-splitting approach** is **more memory efficient than Go 1.23’s** , which keeps old buckets in memory during incremental migration.


To recap the memory savings compared to Go 1.23:


-


**Higher load factor** : Swiss Tables support a higher load factor of **87.5%** (versus 81.25% in Go 1.23), requiring fewer total slots.


-


**Elimination of overflow buckets** : Swiss Table removes the need for overflow storage. They also eliminate the overflow bucket pointer, offsetting the additional footprint for the control word.


-


**More efficient growth** : Unlike Go 1.23, which keeps old buckets in memory during incremental migration, Go 1.24’s table-splitting approach is more memory efficient.


### Estimating memory usage of the map


Now let’s apply what we learned to estimate the size of the` shardRoutingCache` map on Go 1.24.


For **3,500,000 elements** with a maximum average load factor of **7/8** , we need at least:


-


**Required groups** = 3,500,000 / (8 x 7/8) ≈ **500,000 groups**


-


**Required tables** = 500,000 (groups) / 128 (groups per table) ≈ **3900 tables**


Since tables grow independently, a directory can have any number of tables—not just powers of 2.


Each table stores **128 groups** . Each group has:


-


A **control word: 8 bytes**


-


**8 key–value pairs, 56 bytes each** : 56 × 8 = **448 bytes**


This means that each table uses **(448 + 8) bytes per group x 128 groups ≈ 58,368 bytes** .


As a result, for **Go 1.24** , the total memory used by Swiss Tables is: **3,900 tables × 58,368 bytes per table = 227,635,200 bytes ≈ 217 MiB** . (In **Go 1.23** , the size of the map was approximately **726.4 MiB** .)


This aligns with what we previously observed in live heap profiles: Switching to **Go 1.24** saves roughly **500 MiB of live heap usage** —or about **1GiB of RSS** when[factoring in GOGC](https://www.datadoghq.com/blog/go-memory-metrics/#heap-profiling) —for the` shardRoutingCache` map.


## Why lower-traffic environments didn’t see the same gains


Let’s start by looking at the number of elements in the map in a lower-traffic environment:


Taking that into account, let’s apply the same formulas.


### In Go 1.23 with bucket-based hash tables


For **550,000 elements** with a maximum average load factor of **13/16** , we need at least:


-


**Required buckets** = 550,000 / (8 x 13/16) ≈ **84,615 buckets**


-


The smallest power of 2 greater than 84,615 is 217 = **131,072 buckets**


Since 84,615 is well above 216 ( **65,536** ), we can expect the old buckets from the most recent resizing to be deallocated.


This also corresponds to 213 **pre-allocated overflow buckets** , so the total number of buckets is:


-


**2** **18** **+ 2** **14** **= 139,264 buckets**


As a result, for Go 1.23, the total memory used by the bucket arrays is:


-


**139,264 buckets × 464 bytes per bucket = 64,618,496 bytes ≈ 62 MiB**


### In Go 1.24 with Swiss Tables


For the same **550,000 elements** and a maximum average load factor of **7/8** , we need:


-


**Required groups** = 550,000 / (8 × 7/8) ≈ **78,571 groups**


-


**Required tables** = 78,571 (groups) / 128 (groups per table) ≈ **614 tables**


Each table uses:


-


**(448 + 8) bytes per group × 128 groups ≈ 58,368 bytes**


So the total memory used by Swiss Tables is:


-


**614 tables × 58,368 bytes per bucket = 35,838,144 bytes ≈ 34 MiB**


There’s still a **~28 MiB reduction** in live heap usage. However, this saving is **an order of magnitude smaller** than the **200-300 MiB RSS increase** we observed due to the` mallocgc` regression. So in lower-traffic environments, overall memory usage still goes up, which aligns with our observations.


But there was still an opportunity to optimize memory usage.


## How we further reduced map memory usage


Back when we were looking at the` shardRoutingCache` map:


```text
5   type     Response     struct   {    6       ShardID          int32    7       ShardType        ShardType    8       RoutingKey       string    9       LastModified   *  time  .  Time    10   }
```


Initially, we assumed the` RoutingKey` strings were empty, and the` LastModified` pointer was` nil` . However, all our math made sense and gave us a good approximation of the memory. How is that?


After reviewing the code, we discovered that the` RoutingKey` and` LastModified` attributes are never populated in the` shardRoutingCache` map. While the` Response` struct is used elsewhere with those fields set, they remain unset in this specific use case.


While we’re at it, we also noticed that the` ShardType` field is an` int64` enum—but it only has three possible values. That means we could just use a` uint8` instead and still have room for up to 255 enum values.


Given that this map can get very large, we figured it was worth optimizing. We drafted a PR that does two things:


1.


Switches the` ShardType` field from an` int` (8 bytes) to a` uint8` (1 byte), allowing us to store up to 255 values


2.


Introduces a new type,` cachedResponse` , that only contains` ShardID` and` ShardType` —so we’re no longer storing an empty string and a nil pointer.


This brings down the the size of a single key–value pair from 56 bytes to:


-


**16 bytes** for the key fingerprint (no changes)


-


**4 bytes** for` ShardID`


-


**1 byte** for` ShardType` ( **+ 3 bytes for padding** )


This leads to **24 bytes (with padding)** for each key value pair.


In our high-traffic environments, this roughly reduces the size of the map from **217 MiB** to **93 MiB** .


If we[factor in GOGC](https://www.datadoghq.com/blog/go-memory-metrics/#heap-profiling) , that’s roughly a **250 MiB RSS decrease per pod across** all data-processing services using that component.


We confirmed the memory savings through live heap profiling—now it was time to consider the operational impact.


## How does this translate to cost reductions?


This map optimization was deployed to all data-processing services, and we can now consider two paths to achieve cost reductions.


**Option 1: Lowering the Kubernetes memory limit** of the containers. This would allow other applications on the cluster to use the memory we freed per pod.


**Option 2: Trade memory for CPU** using **` GOMEMLIMIT`** . If the workload is CPU-bound,[setting a GOMEMLIMIT](https://tip.golang.org/doc/gc-guide#Memory_limit) allows to trade the saved memory for CPU. The CPU savings could let us downscale the number of pods.


For some workloads that already set a` GOMEMLIMIT` , we observed a small reduction in average CPU usage:


## Go 1.24 in production: Takeaways and lessons


This investigation revealed several important insights about memory optimization in Go applications:


-


**By collaborating with the broader Go community, we identified, diagnosed, and helped fix a subtle—but impactful—memory regression introduced in Go 1.24.** This issue affected physical memory usage (RSS) across many workloads—despite being invisible to Go’s internal heap metrics.


-


**Each new language version brings optimizations, but also the risk of regressions that can significantly impact production systems.** Staying current with Go releases lets us take advantage of performance improvements like Swiss Tables—it also positions us well to catch and resolve issues early, before they impact production at scale.


-


**Runtime metrics and live heap profiling were critical to our investigation.** They allowed us to form accurate hypotheses, trace the discrepancy between RSS and Go-mangaged memory, and effectively communicate with the Go team. Without detailed metrics and the ability to analyze them effectively, subtle issues like this could go undetected or be misdiagnosed.


-


**Swiss Tables introduced in Go 1.24 delivered significant memory savings compared to the bucket-based hash tables in Go 1.23—particularly for large maps.** In our high-traffic environments, this led to a **~70% reduction** in map memory usage.


-


**Beyond runtime-level improvements, our own data structure optimizations had a real impact.** By refining the` Response` struct to eliminate unused fields and use appropriately sized types, we reduced memory consumption even further.


Together, these changes made Go 1.24 a net win for our services. And they reaffirmed a core engineering principle at Datadog: small details—whether in runtime internals or struct definitions—can add up to major performance gains at scale.


If collaborating upstream and solving tough performance issues sounds exciting,[check out our open roles](https://careers.datadoghq.com/all-jobs/?s=Go&parent_department_Engineering%5B0%5D=Engineering) .


-
-
-
