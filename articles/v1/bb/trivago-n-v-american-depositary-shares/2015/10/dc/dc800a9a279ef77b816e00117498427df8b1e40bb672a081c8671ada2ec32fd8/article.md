---
schema_version: "1.0.0"
document_id: "dc800a9a279ef77b816e00117498427df8b1e40bb672a081c8671ada2ec32fd8"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2015-10-15-tcache/"
published_at: "2015-10-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:32.834035+00:00"
content_hash: "sha256:2ac60059410ca7b82dd1a3a09106148420d4ee1491e3365dfa699e1376f2bc8a"
---

# tCache - Scalable data-aware Java Caching

### Highly scalable, flexible caching within the JVM Heap


Caching data is an essential part in many high-load scenarios. A local 1st-level cache can augment a shared 2nd-level cache like *Redis* and *Memcached* to further boost performance. An in-process cache involves no network overhead, so the cache speed is only limited by local resources like CPU, memory transfer speed and locking. *tCache* is a production-proof local in-process cache for the JVM, which is part of trivago’s OpenSource Java library[triava](https://github.com/trivago/triava) (Apache v2 license). This article outlines cutting-edge features like data-aware evictions that are operating near lock-free.


## tCache core features


tCache takes a creative approach for near lock-free evictions and supports data-aware evictions. Its key features are:


- High throughput of both read and write operations
- Extensive options on controlling the validity of entries
- Evictions that are data-aware or use LFU or LRU
- Loaders : Loaders load data, if the data is not yet in the Cache
- Jam policy for writing: Wait for eviction or drop
- Exchangeable backing Map (ConcurrentHashMap, Highscalelib:NonBlockingHashMap)
- Verbose statistics: Put, Hit, Miss, Drop, Eviction statistics, Heap size


Configuration of features is individual per Cache instance, by using a cache Builder:


```text
TCacheFactory factory  :   TCacheFactory.  standardFactory  ();
Builder<  Integer  ,   String  > builder  :   factory.  builder  ();
builder.  setId  (  "int2string"  ).  setEvictionPolicy  (EvictionPolicy.LRU);   // configure
Cache<  Integer  ,   String  > cache  :   builder.  build  ();   // construct
```


## Cache consistency: Expiration and invalidation


Entities in a Cache should be consistent with the origin server. When a customer entity gets changed in a Database, the entry in the Cache is not valid anymore. tCache supports various ways to remove Cache entries.


- Idle time
- Expiration time or expiration interval
- Explicit removal, either immediately (delete) or within time interval (expire-until)
- Evictions by LFU, LRU, Clock or custom type-safe data-aware evictions


## The problems of mass insertions and mass deletions


tCache is very well prepared for mass operations, and can permanently process 1 Million or more put operations per second. Databases, REST services and even the network infrastructure are typically less well prepared for such peak loads. Loading the entities of all customers into a cache can massively stress the infrastructure up to a level of denial of service.


Load-spreading can help and is an intrinsic feature of tCache. Entries can automatically get individual expiration times within a given interval. This time interval is configured when constructing the Cache as “cache time spread”, so a simple *cache.put(key, value)* can assign the cache entry a lifetime between 4 and 5 hours. The same is possible for explicit deletions, a call to *cache.expireUntil(key, interval)* will remove the entry from the cache sometime between now and the end of the given interval. Thus the interval based functionalities “cache time spread” and “expire until” are useful in scenarios with mass insertions and deletions respectively. They are guaranteeing a better load distribution on the origin server for the next re-fetch, as not all elements that were mass-inserted once will expire at the same time.


## Eviction


When the cache is full, *tCache* offers three options:


- Add the element, and evict at least one other element (default)
- Do not add the element (drop)
- Ignore the fact, and allow the new element anyways (not recommended)


By default LFU eviction is being used. One can also use LRU or an own implementation:


```text
static   class   CustomerClassEvictor   extends   FreezingEvictor  <  Integer  ,   CustomerType  > {
@  Override
public   long   getFreezeValue  (Integer   userId  , TCacheHolder<  CustomerType  >   custType  ) {
return   custType.  peek  ().  getPriority  ();
}
}
```


Above is a complete eviction implementation. It is data aware, inspecting the CustomerType. The value returned by *getFreezeValue()* is used to sort the entries for eviction. Here it ensures that entries of type *Premium* are evicted last.


```text
enum   CustomerType   {
Guest  (  0  ),   Registered  (  5  ),   Premium  (  9  );
int   priority;
CustomerType  (  int   priority  ) {   this  .priority  :   priority; };
public   int   getPriority  () {   return   priority; }
}
```


## Speed and Benchmarks


For reaching a high read and write throughput, specific workload is done in background threads:


1. Expiration is done at regular intervals by a dedicated thread.
2. Eviction is not inflicting the writing thread. A slight cache overfill is allowed, so the background thread can run the eviciton in parallel to the application threads.
3. The current time is provided as[TimeSource](https://github.com/trivago/triava/blob/master/src/main/java/com/trivago/triava/time/TimeSource.java) by an[own thread](https://github.com/trivago/triava/blob/master/src/main/java/com/trivago/triava/time/EstimatorTimeSource.java) .


The speed of any cache depends on the usage pattern. Important factors are the write rate, the hit rate and whether elements get expired or evicted. The following graphics shows a benchmark of tCache in comparison with other cache implementations. The[Cache2K benchmark](http://cache2k.org/benchmarks.html) performs 3 million get requests using a[Loader](https://github.com/trivago/triava/blob/master/src/main/java/com/trivago/triava/tcache/core/CacheLoader.java) . The access pattern varies, for example Eff95 represents a 95% hit rate.


## Usage and future outlook


*tCache* is fully production ready. It is used in trivago’s backend systems since 2010, in normal settings as well as in high-throughput scenarios including lots of evictions. In the past 5 years the library has evolved in features (put-if-absent, CacheLoader, custom evictions, …) and in scalability (asynchronous evicitons, TimeSource, …). Another article will go deeper into those scalability details. The road to the future is wide and open. Possible additions are a load-spreaded background prefill via *cache.putAll(keys, interval)* , serialization and deserialization of a complete cache, heuristic evictions and ideas from future contributors.


To get started with *tCache* ,[code examples](https://github.com/trivago/triava/blob/master/src/examples/java/com/trivago/examples/CacheExample.java) are available. You can[download](https://github.com/trivago/triava/) it as part of trivago’s OpenSource Java library *triava* under Apache v2 license. If you tried, please leave your feedback and feature ideas here.


## Links


- [Project page](https://github.com/trivago/triava/)
- [Code examples](https://github.com/trivago/triava/blob/master/src/examples/java/com/trivago/examples/CacheExample.java)
- Maven central: In preparation
