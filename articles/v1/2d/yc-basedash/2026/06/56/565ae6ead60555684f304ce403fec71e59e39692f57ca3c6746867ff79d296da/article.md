---
schema_version: "1.0.0"
document_id: "565ae6ead60555684f304ce403fec71e59e39692f57ca3c6746867ff79d296da"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-we-cut-a-json-round-trip-out-of-our-query-cache/"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:119c23c87351380ee4a621dddc3be1961fc6f3edfece69474372078f220ef41c"
---

# How we cut a JSON round-trip out of our query cache

Every chart in Basedash is backed by a SQL query that runs against your database. When you load a dashboard, the client hits` /api/chart/run-query` , the server runs the query, and the rows come back as JSON. We cache those results in Postgres so a popular dashboard doesn’t re-run the same expensive query for every viewer.


The caching worked. What bugged us is what happened on a cache hit: we’d parse the cached JSON into JavaScript objects, then immediately serialize those exact same objects back into JSON for the HTTP response. For a chart returning tens of thousands of rows, that’s a lot of work to turn JSON into JSON.


Here’s how we got rid of the round-trip, and the part that surprised us about where the win actually landed.


## The round-trip


The cache is a Postgres table called` QueryCache` , and the values are encrypted at rest (the rows can contain customer data, so we don’t store them in the clear). The cache layer sits behind[@epic-web/cachified](https://github.com/epicweb-dev/cachified) , which expects a cache that stores and returns objects.


So a cache hit looked like this:


1. Read the encrypted row from Postgres.
2. Decrypt it into a JSON string.
3. ` JSON.parse` that string into an array of row objects.
4. Hand the objects back up the stack.
5. The route returns` { rows, count, metadata }` , which the framework serializes with` JSON.stringify` .


Steps 3 and 5 are the problem. The bytes we pulled out of the cache were already valid JSON. The bytes we send back are the same JSON, wrapped in a small envelope. We were parsing a big string into objects and then turning those objects right back into a nearly identical string, and both operations are synchronous and run on the main event loop.


That last detail matters more than it looks, and we’ll come back to it.


## The idea: cache the bytes, not the objects


If the cached value is already the JSON we want to send, we should keep it as bytes and splice it into the response. No parse, no re-serialize.


We added two columns to` QueryCache` :


- ` rowsJson` : the encrypted JSON array of rows.
- ` metaJson` : a tiny plaintext blob with the cache metadata and the row count.


The existing` value` column still exists. On a raw entry we set it to a sentinel string,` __raw_rows_cache_entry__` , so the old object-returning code path can tell the two entry shapes apart and treat a raw entry as a miss if it ever reads one by mistake.


Building the response is then a string concatenation:


```text
export   function   createRunQueryJsonResponse  ({
count  ,
metadata  ,
rowsJson  ,
}  :   {
count  :   number  ;
metadata  :   unknown  ;
rowsJson  :   string  ;
}) {
return   new   Response  (
`{"rows":${  rowsJson  },"count":${  JSON  .  stringify  (  count  )  },"metadata":${  JSON  .  stringify  (  metadata  )  }}`  ,
{ headers: {   'Content-Type'  :   'application/json'   } },
);
}
```


` rowsJson` is dropped in verbatim.` count` and` metadata` are small, so serializing those is cheap. We wrote a test that asserts this produces byte-identical output to` JSON.stringify({ rows, count, metadata })` , because the whole point falls apart if the envelope drifts from what clients already expect.


## Keeping the two paths from colliding


The cache key is a hash of the SQL, the query params, and the row-level-security group. We prefix every raw-cache key with` v2:` so the new entries can’t collide with old-shape entries that are still warm. An entry written by one path is invisible to the other.


There’s also a multi-tenant trap here. Two different database connections can run byte-identical SQL (` SELECT * FROM users` is not exactly rare), and they must never read each other’s cached rows. The key already includes a per-connection prefix, so` connection-a:v2:...` and` connection-b:v2:...` stay disjoint even when the SQL after the prefix is the same. We added a regression test that runs identical SQL through two connection prefixes against one shared cache and asserts each side only ever sees its own rows. This is the kind of bug you want a test to fail on loudly, not a customer to find.


## The encryption problem we created


Switching to raw bytes exposed something the old path had quietly been getting away with. Our field encryption was synchronous, built on[@47ng/cloak](https://github.com/47ng/cloak) ’s sync AES-GCM helpers. Encrypting or decrypting a small secret synchronously is fine. Doing it to a multi-megabyte result set is not, because it blocks the event loop for the entire duration of the crypto.


So we added async variants,` encryptAsync` and` decryptAsync` , built on Node’s` webcrypto.subtle` :


```text
const   decrypted   =   await   webcrypto.subtle.  decrypt  (
{ name:   'AES-GCM'  , iv:   decodeBase64Url  (match.groups.iv) },
key,
decodeBase64Url  (match.groups.ciphertext),
);
return   textDecoder.  decode  (decrypted);
```


One nice property:` subtle.decrypt` checks the cloak fingerprint embedded in the value before it does any crypto work, so a value encrypted under a key we don’t have throws immediately instead of wasting cycles. The` QueryCache` model now encrypts its big fields through the async path, while the small secrets elsewhere (SSH keys, JWT secrets) stay on the sync path where the overhead doesn’t matter.


## The benchmark, and the surprise


We ran a synthetic cache-hit harness at three result sizes, 7 runs each, measuring both wall time and event-loop delay. Here’s the median and p95 for each:


rows old wall med / p95 (ms) old loop delay med / p95 (ms) new wall med / p95 (ms) new loop delay med / p95 (ms)


1k 0.82 / 2.28 1.09 / 1.15 0.88 / 1.47 1.11 / 1.40


10k 7.56 / 7.99 1.07 / 2.98 4.93 / 6.71 1.18 / 1.57


100k 72.09 / 83.42 9.64 / 15.52 88.23 / 102.04 7.92 / 9.13


At 1k rows, nothing really moves. The parse was already sub-millisecond, so removing it is noise.


At 10k rows, wall time drops from 7.56ms to 4.93ms at the median, a clean win. That’s the sweet spot where skipping the parse and re-serialize actually shows up on the clock.


At 100k rows is where it got interesting, and honestly not in the direction we first expected. Wall time got *worse* , from 72ms to 88ms at the median. But event-loop delay went the other way: p95 dropped from 15.52ms to 9.13ms, about 41% lower.


## Why we shipped the version that’s sometimes slower


That 100k row result looks like a regression if you only read the wall-time column. We almost talked ourselves out of the change because of it.


The thing to remember is that our server is a single Node process handling many requests on one event loop. Wall time for one isolated request isn’t the metric that decides whether the dashboard feels fast under load. Event-loop delay is, because while one request is synchronously parsing 100k rows, every other request on that process is frozen waiting its turn.


The async crypto path adds a little scheduling overhead, which is why a lone request’s wall time can creep up. In exchange, the heavy work yields instead of hogging the thread, so concurrent requests aren’t held hostage. We’ve been bitten by event-loop stalls before (they’re what was[killing our healthy Kubernetes pods](https://www.basedash.com/blog/what-was-killing-our-healthy-kubernetes-pods) , where multi-second freezes tripped the liveness probe), so trading a few milliseconds of single-request latency for lower tail latency across the process was an easy call once we framed it that way.


## Rollout


The whole thing sits behind a feature flag,` RUN_QUERY_RAW_BYTES_CACHE` , so we could turn it on per organization and watch the metrics before committing. The raw path also keeps the stale-while-revalidate behavior the old cache had: a stale-but-usable entry is served immediately while a fresh value gets computed in the background, so a hit never blocks on a re-query.


We verified it the boring way. Flag off with an empty cache populated only old-shape keys. Flag on populated` v2:` keys with raw rows. Flag off again with` v2:` entries already present left them untouched and used the old keys, which is exactly the disjoint behavior we wanted. Then a browser pass through a real dashboard confirmed the miss path and the hit path both returned 200s with no errors and no broken charts.


## What we took away


A few things worth keeping for the next time we touch a hot path.


1. **If the cached value is already the wire format, don’t deserialize it.** Parsing JSON into objects just to stringify them back is pure overhead, and it’s easy to miss because each step looks reasonable on its own.
2. **Watch event-loop delay, not just wall time, on a single-threaded server.** The version that’s marginally slower for one request can be meaningfully faster for everyone else hitting the box at the same time.
3. **Big synchronous crypto is an event-loop hazard.** Sync AES-GCM is fine for a 32-byte secret and a problem for a multi-megabyte payload. Use the async WebCrypto path when the values get large.
4. **Version your cache keys when you change the entry shape.** A` v2:` prefix made the migration a non-event, with old and new entries coexisting and no risk of one path misreading the other’s bytes.
5. **Test the multi-tenant isolation explicitly.** Identical SQL across two connections sharing one cache is precisely the scenario where a key-prefix bug leaks data, so we made a test fail on it rather than trusting the prefix by inspection.


If you like this kind of measure-then-cut work, we’ve written up a few more:[how we sped up tables by 4 to 5x with virtualization](https://www.basedash.com/blog/how-virtualization-increased-our-table-performance-by-500) and[how we cut our client bundle from 26 MB to 18 MB](https://www.basedash.com/blog/how-we-cut-our-client-bundle-from-26-mib-to-18-mib) . Same shape every time: profile first, change one thing, and check the metric that actually matters.


And if you want to see what runs on top of this cache,[Basedash](https://www.basedash.com/) is an AI-native BI platform that turns your SQL into dashboards your whole team can use.
