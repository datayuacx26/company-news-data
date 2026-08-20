---
schema_version: "1.0.0"
document_id: "42bae4caca3a35adea2f0f7d681fcd2b39bd2b291af84e76935bad0defcd5601"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/bun-built-in-redis-client/"
published_at: "2026-07-25T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:28898afd733a4ccacb4d07a530bff6e8777822f3a34fbcf7404a75ea8fae56ff"
---

# Using Bun's Built-In Redis Client

Bun ships a built-in Redis client you import directly —` import { redis, RedisClient } from "bun"` — so for most caching, session, pub/sub, and rate-limiting workloads you no longer need to add` ioredis` or` node-redis` . Bun’s native Redis client has a Promise-based API with built-in connection management, fully typed responses, and TLS support, and it supports Redis server versions 7.2 and up. This guide is specifically about that *built-in* client — not` ioredis` running on Bun, which is what most “Redis with Bun” tutorials silently demonstrate.


The distinction matters. A common pattern across existing Bun-plus-Redis tutorials is to` bun add ioredis` and call` new Redis()` — that’s the third-party library, not the runtime API. Everything below uses the API that ships inside Bun itself.


## Key Takeaways


- Import the client with` import { redis, RedisClient } from "bun"` ; the default` redis` instance reads` REDIS_URL` , then` VALKEY_URL` , then falls back to` redis://localhost:6379` .
- The client opens no connection until the first command runs, automatically pipelines commands, and reconnects with exponential backoff from 50ms up to a 2000ms cap over a default of 10 retries.
- As of Bun 1.3 the client wraps 66 Redis commands as typed methods;` redis.send(command, argsArray)` runs any command that isn’t wrapped — which is also how you issue` MULTI` /` EXEC` today.
- Pub/Sub takes over a connection, so you call` .duplicate()` to get a second connection for publishing or running other commands.
- As of Bun 1.3.x the built-in client does not support Redis Cluster or Sentinel; for those, reach for` node-redis` or` ioredis` .


## Connecting with the built-in Redis client


The fastest path is the default` redis` export, which connects lazily from environment variables. By default, the client reads connection information from` REDIS_URL` , then` VALKEY_URL` , and if neither is set, defaults to` redis://localhost:6379` . No socket is opened until you issue the first command.


```text
import   {   redis  ,   RedisClient   }   from   "  bun  "  ;


// Default client — reads REDIS_URL / VALKEY_URL from the environment
await   redis  .  set  (  "  hello  "  ,   "  world  "  );
const   result   =   await   redis  .  get  (  "  hello  "  );


// Custom client with an explicit URL
const   client   =   new   RedisClient  (  "  redis://username:password@localhost:6379  "  );
await   client  .  set  (  "  counter  "  ,   "  0  "  );
await   client  .  incr  (  "  counter  "  );
```


Use the default` redis` instance for app-wide access; construct your own` RedisClient` when you need a separate connection, distinct options, or a different database. The[Bun Redis documentation](https://bun.com/docs/runtime/redis) lists the full set of supported URL schemes, including` rediss://` and` redis+tls://` for TLS and` redis+unix://` for Unix sockets.


Connection management is handled for you:


- No connection is made until a command is executed; the first command initiates the connection, and it stays open for subsequent commands until you call` client.close()` .
- When a connection drops, the client recovers on its own: it starts with a 50ms delay and doubles it on each attempt, caps the reconnection delay at 2000ms, and retries up to` maxRetries` times (default 10).
- Commands issued while disconnected are queued when` enableOfflineQueue` is true (the default) and rejected immediately when it’s false.


These are the same behaviors you’d otherwise wire up by hand in` ioredis` .


## Core operations and the` send()` fallback


The client exposes typed methods for everyday Redis commands and converts replies to native JavaScript values, so you skip most manual parsing:


- integer responses come back as JavaScript numbers;
- bulk and simple strings as strings;
- null bulk strings as` null` ; and
- arrays as JavaScript arrays.


There’s also command-specific coercion:` EXISTS` returns a boolean instead of a number, and` SISMEMBER` returns a boolean.


Tune any of this through the options object:


```text
const   client   =   new   RedisClient  (  "  redis://localhost:6379  "  , {
connectionTimeout  :   5000  ,       // default 10000 ms
autoReconnect  :   true  ,           // default true
maxRetries  :   10  ,                // default 10
enableOfflineQueue  :   true  ,      // default true
enableAutoPipelining  :   true  ,    // default true
tls  :   true  ,
});
```


```text
// Strings and expiry
await   redis  .  set  (  "  session:123  "  ,   "  active  "  );
await   redis  .  expire  (  "  session:123  "  ,   3600  );   // seconds
const   ttl   =   await   redis  .  ttl  (  "  session:123  "  );


// Counters
await   redis  .  set  (  "  counter  "  ,   "  0  "  );
await   redis  .  incr  (  "  counter  "  );
await   redis  .  decr  (  "  counter  "  );


// exists() returns a boolean
const   present   =   await   redis  .  exists  (  "  session:123  "  );   // true


// Hashes
await   redis  .  hmset  (  "  user:123  "  , [  "  name  "  ,   "  Alice  "  ,   "  email  "  ,   "  alice@example.com  "  ]);
const   [  name  ,   email  ]   =   await   redis  .  hmget  (  "  user:123  "  , [  "  name  "  ,   "  email  "  ]);
```


Not every command has a dedicated method. All standard operations are supported — including hashes, lists, and sets — totaling 66 commands as of Bun 1.3. For anything outside that set, use` send()` . The` send` method runs any Redis command, including ones without a dedicated method; the first argument is the command name and the second is an array of string arguments.


```text
// Raw command fallback — name + array of string args
await   redis  .  send  (  "  LPUSH  "  , [  "  mylist  "  ,   "  value1  "  ,   "  value2  "  ]);
const   list   =   await   redis  .  send  (  "  LRANGE  "  , [  "  mylist  "  ,   "  0  "  ,   "  -1  "  ]);
```


` send()` is also how you run transactions today, which is the client’s main API gap covered later.


## Pub/Sub with` .duplicate()`


Redis Pub/Sub is supported, but a subscribed connection can’t do anything else. Subscribing takes over the` RedisClient` connection: a client with subscriptions can only call` subscribe()` , so to send other commands you create a separate connection with` .duplicate()` . The subscribe callback receives the message first and the channel second.


```text
import   {   RedisClient   }   from   "  bun  "  ;


const   redis   =   new   RedisClient  (  "  redis://localhost:6379  "  );
await   redis  .  connect  ();


// Second connection for publishing / other commands
const   subscriber   =   await   redis  .  duplicate  ();


await   subscriber  .  subscribe  (  "  notifications  "  , (  message  ,   channel  )   =>   {
console  .  log  (  `  [  ${  channel  }  ]   ${  message  }`  );
});


await   redis  .  publish  (  "  notifications  "  ,   "  Hello from Bun!  "  );
```


Pub/Sub was added in Bun 1.2.23, and the docs still flag it as experimental — stable in practice, but worth pinning your Bun version if you depend on it. Unsubscribe with` .unsubscribe()` (no argument clears all channels; pass a channel or listener to scope it).


## Automatic pipelining


The built-in client pipelines by default — you don’t opt in. The client automatically pipelines commands, improving performance by sending multiple commands in a batch and processing responses as they arrive. Issue several awaited commands together (for example with` Promise.all` ) and they go out in one batch rather than one round-trip each:


```text
const   [  a  ,   b  ,   c  ]   =   await   Promise  .  all  ([
redis  .  get  (  "  user:1:name  "  ),
redis  .  get  (  "  user:2:name  "  ),
redis  .  get  (  "  user:3:name  "  ),
]);
```


Automatic pipelining is where the performance gap with` ioredis` widens. Bun describes its Redis client as significantly faster than` ioredis` , with the advantage increasing as batch size grows. Bun’s published, reproducible figures from the 1.2.9 release notes back that up: in their GET benchmark, Bun.redis was 44.82% faster at batches of 10, 58.29% faster at batches of 100, and 85.39% faster at batches of 1000 — roughly 1.85x at the largest batch, not a fixed multiple. (Bun’s own[1.3 release blog](https://bun.com/blog/bun-v1.3) separately headlines a benchmark chart putting the built-in client at more than 7.9x ioredis’s throughput, so that figure is Bun’s own published number, not just secondary reporting.)


A handful of commands disable auto-pipelining because they’re stateful — among them` AUTH` ,` INFO` ,` MULTI` ,` EXEC` ,` WATCH` ,` SUBSCRIBE` , and` SELECT` . To turn pipelining off globally, set` enableAutoPipelining: false` .


## Applied patterns: cache, sessions, and rate limiting


The patterns below transfer from any Redis client; here they run entirely on the built-in API. Treat this as one small service module.


**Cache-aside.** Check Redis first, fall through to the database on a miss, then populate the cache with a TTL.


```text
import   {   redis   }   from   "  bun  "  ;


async   function   getUser  (  userId  :   string  ) {
const   cacheKey   =   `  user:  ${  userId  }`  ;
const   cached   =   await   redis  .  get  (  cacheKey  );
if (  cached  )   return   JSON  .  parse  (  cached  );


const   user   =   await   db  .  getUser  (  userId  );         // your data source
await   redis  .  set  (  cacheKey  ,   JSON  .  stringify  (  user  ));
await   redis  .  expire  (  cacheKey  ,   3600  );            // 1 hour
return   user  ;
}
```


For write-through, update the cache in the same operation that writes the database. For stale-while-revalidate, serve the cached value immediately and refresh it in the background once the TTL is near.


**Session storage with TTL.** Store sessions as hashes and let Redis expire them.


```text
async   function   createSession  (  userId  :   number  ,   data  :   object  ) {
const   sessionId   =   crypto  .  randomUUID  ();
const   key   =   `  session:  ${  sessionId  }`  ;
await   redis  .  hmset  (  key  , [
"  userId  "  ,   String  (  userId  ),
"  created  "  ,   String  (  Date  .  now  ()),
"  data  "  ,   JSON  .  stringify  (  data  ),
]);
await   redis  .  expire  (  key  ,   86400  );   // 24 hours
return   sessionId  ;
}
```


**Sliding-window rate limiting with sorted sets.** Sorted-set commands aren’t wrapped as dedicated methods, so this is a natural place for` send()` . Drop entries older than the window, count what remains, and add the current request.


```text
async   function   rateLimit  (  id  :   string  ,   limit   =   100  ,   windowMs   =   60_000  ) {
const   key   =   `  ratelimit:  ${  id  }`  ;
const   now   =   Date  .  now  ();


await   redis  .  send  (  "  ZREMRANGEBYSCORE  "  , [  key  ,   "  0  "  ,   String  (  now   -   windowMs  )]);
const   count   =   Number  (  await   redis  .  send  (  "  ZCARD  "  , [  key  ]));


if (  count   >=   limit  )   return   {   allowed  :   false  ,   remaining  :   0   };


await   redis  .  send  (  "  ZADD  "  , [  key  ,   String  (  now  ),   `${  now  }  -  ${  crypto  .  randomUUID  ()  }`  ]);
await   redis  .  expire  (  key  ,   Math  .  ceil  (  windowMs   /   1000  ));
return   {   allowed  :   true  ,   remaining  :   limit   -   count   -   1   };
}
```


Because the client auto-pipelines, the` ZREMRANGEBYSCORE` /` ZCARD` /` ZADD` /` EXPIRE` sequence batches efficiently without manual pipeline objects.


## Limits, and when to still use ioredis or node-redis


The built-in client covers the common cases above, but it has documented gaps as of Bun 1.3.x. Transactions (MULTI/EXEC) must be done through raw commands, and Redis Sentinel and Redis Cluster are unsupported. If you need clustering, Sentinel-based failover, or Redis Stack modules (search, JSON, time-series, probabilistic structures), reach for a full-featured library.[Bun’s own issue tracking the migration](https://github.com/oven-sh/bun/issues/18907) recommends[node-redis](https://github.com/redis/node-redis) as the preferred client for new projects, with[ioredis](https://github.com/redis/ioredis) as the established alternative. The 1.3 blog notes that support for clusters, streams, and Lua scripting is in the works — so re-check the docs’ Limitations section before assuming current status, since this surface is still moving.


Capability Bun built-in` redis`` node-redis` /` ioredis`


Install step None — ships with Bun` bun add node-redis` /` ioredis`


Auto-pipelining On by default Configurable / manual


Pub/Sub Yes (experimental, via` .duplicate()` ) Yes


Transactions (MULTI/EXEC) Via raw` send()` only Native API


Redis Cluster Not supported Supported


Redis Sentinel Not supported Supported


Redis Stack modules Use a library` node-redis` supports them


One implementation note worth a caveat: the docs describe the client as natively compiled (currently characterized as implemented in Rust following Bun’s 2026 port away from Zig). That detail doesn’t change a single line of your application code — the API surface is identical regardless of the underlying language.


For caching, sessions, pub/sub, counters, and rate limiting, the built-in client is enough, and dropping a dependency is a real win. The moment you need Cluster, Sentinel, Redis Stack, or rich transactional flows, install` node-redis` . Start with` import { redis } from "bun"` against a local server, confirm your command set is covered, and pin your Bun version so an evolving API doesn’t surprise you in production.


## FAQs


How do I run a Redis transaction with Bun's built-in client?


Bun's built-in client has no native MULTI/EXEC API, so you run transactions through the raw send() method. Issue redis.send('MULTI', \[\]), then your queued commands, then redis.send('EXEC', \[\]). Because MULTI, EXEC, and WATCH disable auto-pipelining, they are sent individually rather than batched. If you need a richer native transaction API, node-redis or ioredis provide one.


Does Bun's built-in Redis client work with Redis Cluster or Sentinel?


No. As of Bun 1.3.x the built-in client does not support Redis Cluster or Redis Sentinel, per the docs' Limitations section. For sharded clustering, Sentinel-based failover, or Redis Stack modules like search, JSON, and time-series, use a full-featured library instead. Bun's own migration issue recommends node-redis as the preferred client for new projects, with ioredis as the established alternative.


What is the difference between using Bun's built-in redis import and running ioredis on Bun?


The built-in client is the runtime API you reach through import { redis, RedisClient } from 'bun', requiring no install and pipelining commands by default. Running ioredis means bun add ioredis and new Redis(), a third-party library with its own connection handling. Most 'Redis with Bun' tutorials silently use ioredis. The built-in client ships zero-dependency; ioredis adds native Cluster, Sentinel, and transaction APIs the built-in client lacks.


What happens to commands sent while Bun's Redis client is disconnected?


Commands issued while the client is disconnected are queued and replayed once the connection is restored, because enableOfflineQueue defaults to true. The client reconnects automatically using exponential backoff starting at 50ms, doubling each attempt, capping at 2000ms, over a default of 10 retries. Set enableOfflineQueue to false to reject commands immediately during a disconnect instead of buffering them.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
