---
schema_version: "1.0.0"
document_id: "3a7030c79656a62aaf51b783a8fab119bd41cc5f818d164bcd2a77ab9fd53ea9"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/firebun"
published_at: "2026-03-27T12:00:00+00:00"
first_seen_at: "2026-07-22T17:13:09.827889+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:b51ef16c656018ec7b65d0680c6e00f1dd7b857edaaccbad3b4f32d629a60dc2"
---

# Why we replaced Node.js with Bun for 5x throughput

**Update (March 30, 2026):** Shortly after this post went live, Bun[shipped a fix](https://github.com/oven-sh/bun/pull/28613) for the memory leak. 🔥


---


We replaced Node.js with Bun in one of our most latency-sensitive services and got a 5x throughput increase. We also found a memory leak that only exists in Bun's HTTP model.


The service is called Firestarter. It's our warm start connection broker: it holds thousands of long-poll HTTP connections from idle run controllers, each waiting for work. When a task run arrives, Firestarter matches it to a waiting controller and sends the payload through the held connection. No cold start, no container spin-up. It's in the critical path of every task execution on Trigger.dev.


The problem: Firestarter was using too much CPU. It was running on Node.js, spending 31% of its time inside a SQLite query, parsing every request with Zod, and converting headers with` Object.fromEntries()` on every GET. It worked, but it was slow.


It took four rounds of profiling to get there, and we hit a few Bun surprises we haven't seen documented elsewhere.


## Phase 1: kill the SQLite query engine


The original connection manager was designed as a generic queryable store. It accepted arbitrary nested metadata, flattened it recursively into key-value pairs, and indexed everything in an in-memory SQLite database. Node 22 shipped with` node:sqlite` built-in, so it was zero-dependency. SQL gave us flexible partial matching on any combination of fields. It made sense at the time because we didn't know the access pattern yet.


Turns out the access pattern was always the same 4 fields. Every match attempt ran this query:


`
SELECT DISTINCT c.id, c.metadata


FROM connections c


JOIN metadata_index mi ON c.id = mi.connection_id


WHERE c.id IN (


SELECT connection_id FROM metadata_index


WHERE (key = ? AND value = ?) OR (key = ? AND value = ?)


OR (key = ? AND value = ?) OR (key = ? AND value = ?)


GROUP BY connection_id


HAVING COUNT(DISTINCT key) = ?


)


LIMIT 1


`


A correlated subquery with JOIN, GROUP BY, and HAVING COUNT(DISTINCT) for what is fundamentally a hash table lookup (we really overengineered this one). The metadata is always the same 4 fields: deployment ID, version, CPU, and memory.


We ran` node --prof` under load (500 simulated controllers, 50 concurrent supervisor requests) and processed the output with` --prof-process` .` getConnection` was **31% of total CPU time** .


We replaced SQLite with a composite-key` Map<string, Set<string>>` . The key is a null-delimited string of` deployment + version + cpu + memory` . Matching became O(1) instead of a SQL query.


The results:


Metric SQLite Map


Throughput 2,099 req/s 4,534 req/s


p50 latency 22.5ms 10.1ms


p95 latency 29.1ms 14.9ms


max latency 619ms 403ms


2.2x throughput, 2.2x better median latency. And we could drop the` --experimental-sqlite` Node.js flag.


### How we benchmarked


Every number in this post comes from the same reproducible setup: 500 simulated controllers holding persistent keep-alive connections, with k6 firing 50 concurrent supervisor match requests for 30 seconds.


`
# Terminal 1: start Firestarter


bun src/bun.ts


# Terminal 2: 500 controllers with persistent sockets


node bench/controllers.js --controllers 500


# Terminal 3: k6 load test (supervisor side)


k6 run --vus 50 --duration 30s bench/load.js


`


The controller simulator (` controllers.js` ) replicates real production behavior: each controller opens a GET` /warm-start` long-poll, receives a match or times out, then immediately re-polls on the same socket. The k6 script (` load.js` ) POSTs realistic` DequeuedMessage` payloads with configurable waitpoint sizes.


We ran this exact setup at every phase. Same hardware, same parameters. The only variable was the code. (More on our k6 setup in thebonus section below .)


## Phase 2: move to Bun


With SQLite gone, re-profiling showed 50%+ of CPU time in` node:http` internals: writev, socket management, stream handling. Node.js's HTTP stack has overhead that matters when you're holding thousands of concurrent long-poll connections.


We added a Bun entry point (` bun.ts` ) using` Bun.serve()` with its native routing API. The connection manager was already transport-agnostic (we'd extracted it during the SQLite removal), so it was mostly wiring.


Benchmarks with 500 controllers and 50 concurrent supervisor requests:


Metric Node.js (Map) Bun.serve()


Throughput 4,534 req/s 9,434 req/s


p50 latency 10.1ms 4.5ms


p95 latency 14.9ms 7.4ms


max latency 403ms 22ms


Another 2x across the board (and the Bun numbers above already include the Phase 3 optimizations below).


## Phase 3: profile and strip the hot path


Bun was faster out of the box, but we weren't done profiling. Bun has a[--cpu-prof-md](https://bun.sh/docs/project/benchmarking) flag that outputs CPU profiles as markdown instead of Chrome DevTools format. The output is grep-friendly and readable without any tooling.


`
# Start with CPU profiling, markdown output


bun --cpu-prof --cpu-prof-md --cpu-prof-dir /tmp/bun-prof src/bun.ts


# Run load test in another terminal, then kill the server


k6 run --vus 50 --duration 30s bench/load.js


# Read the profile


cat /tmp/bun-prof/*.cpuprofile.md


`


The output is a markdown table you can read in any editor:


`
## Hot Functions (Self Time)


| Self% | Self | Function | Location |


|------:|-------:|-------------------|----------------------|


| 22.0% | 87.2ms | \`_parse\` | \`zod/v3/types.js\` |


| 10.5% | 41.6ms | \`fromEntries\` | \`\[native code\]\` |


| 8.6% | 34.1ms | \`#structuredLog\` | \`structuredLogger\` |


| 4.2% | 16.6ms | \`fetch\` | \`\[native code\]\` |


| 3.1% | 12.3ms | \`addConnection\` | \`connectionManager\` |


`


Three clear hotspots.


**` Zod DequeuedMessage.safeParse()` on every POST: 22% of CPU.** Full Zod schema validation on every supervisor match request. Since this is internal traffic between our own services (the controller already validates the full schema), we replaced it with minimal field presence checks.


**` Object.fromEntries(req.headers.entries())` on every GET: 10.5% of CPU.** We were converting the headers iterator to a plain object on every request, then reading individual fields. Replaced with direct` req.headers.get()` calls.


**Debug logging even when filtered: 8.6% of CPU.** The structured logger was serializing log arguments before checking the log level. Calls like` logger.debug(...)` still built the JSON string even when debug was off.


Combined, these three fixes cut CPU usage by ~40% under identical load.


## Phase 4: compile to a single binary


Next: the runtime itself. Bun has a` bun build --compile` flag that produces a single self-contained executable. No runtime, no` node_modules` , no source files needed in the container.


`
RUN bun build --compile --minify src/bun.ts --outfile firestarter


`


The compiled binary vs interpreted Bun:


Metric Interpreted Compiled


Throughput baseline +14%


p95 latency baseline -24%


Image size ~120MB (bun + node_modules) ~68MB (single binary)


The image doesn't need Bun or Node.js installed at all. Just the binary and a CA cert bundle.


We also tried` --bytecode` (JSC bytecode precompilation) and found it actually hurt steady-state performance. Bytecode helps cold starts (CLI tools, serverless functions), but for a long-running server where JSC's JIT has warmed up, the larger binary and extra memory mapping overhead makes it slower.


Bun doesn't automatically trust the in-cluster Kubernetes CA certificate. Our node checker (which queries the K8s API for cordoned nodes) failed with` unable to verify the first certificate` . Fixed with one env var:


`
NODE_EXTRA_CA_CERTS=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt


`


## The Bun memory leak


After deploying to production, the Grafana dashboard told two stories. CPU was down. But RSS was climbing fast.


Yellow on the left is Node.js, stable at 192 MiB. Green climbing to 250 MiB is Bun with the leak. Blue on the right is the final version: flat at ~85 MiB.


### The three lifecycle paths


Every GET` /warm-start` request returns a` Promise<Response>` that Bun holds until we resolve it. There are three ways it gets resolved:


1.


**Claimed** (matched to a run): the supervisor calls` getConnection()` , we send the payload through the held promise. Bun sends the response, cleans up.


2.


**Timeout** : after 65 seconds,` scheduleCleanup` fires, calls` destroy()` which resolves the promise with a 408. Bun sends it, cleans up.


3.


**Client disconnect** : the controller pod dies, restarts, or hits a network blip. Bun fires the` abort` event on` req.signal` .


Paths 1 and 2 were fine. Path 3 was the leak.


### The root cause


When the client disconnects, our abort handler called` removeConnection()` to clean up the connection manager. But it never resolved the pending` Promise<Response>` .


`
onDisconnect: (cb) => {


req.signal.addEventListener("abort", () => {


if (!resolved) {


cb(); // cleans up the connection manager


// ...but the Promise<Response> stays pending forever


}


}, { once: true });


},


`


In Node.js with Fastify or Express, this isn't a problem. The server ties response state to the socket, and when the socket dies, everything gets cleaned up regardless of whether you called` res.end()` .


In Bun, the` fetch` handler contract is different. **Every` Promise<Response>` must settle** (this is not a suggestion). Bun holds internal request state until the promise resolves or rejects. If it never does, that state stays in memory forever.


Each leaked connection was about 500-2000 bytes. At hundreds of disconnects per hour (pod rollouts, network blips, deployment updates), that adds up fast.


### The fix


One line:


`
onDisconnect: (cb) => {


req.signal.addEventListener("abort", () => {


if (!resolved) {


cb();


// Resolve so Bun can release the request context


wrappedResolve(new Response(null, { status: 499 }));


}


}, { once: true });


},


`


499 is Nginx's "Client Closed Request" status. The client never sees it (they're already gone). But resolving the promise lets Bun free the request context.


### Why the leak was "better than before"


An earlier fix had set` idleTimeout` from Bun's default of 10 seconds to 120 seconds. The 10-second default was killing long-poll connections prematurely (Bun considers a connection with no data flowing as "idle"). This caused 6x more connection churn, and every churned connection was a client disconnect that leaked.


Bumping` idleTimeout` to 120s dramatically reduced the frequency of client disconnects, which made the leak slower. But it didn't eliminate it. Controllers still disconnect on pod rollouts, network issues, and deployment updates.


### Testing the fix


We wrote a test that reproduces the exact leak:


`
it("should resolve with 499 when client disconnects", async () => {


const ac = new AbortController();


const req = new Request("http://localhost/warm-start", {


headers: defaultHeaders,


signal: ac.signal,


});


const responsePromise = handlers.handleWarmStartGet(req);


// Simulate client disconnect


ac.abort();


// Without the fix, this promise NEVER resolves


const response = await withTimeout(responsePromise, 200);


expect(response).not.toBeNull();


expect(response!.status).toBe(499);


});


`


Without the fix,` withTimeout` returns null (the promise hangs). With the fix, it resolves with 499.


We also ran 100 connect/disconnect cycles and verified zero unresolved promises. After deploying, RSS went flat (the blue line in the graph above).


## The full picture


Node + SQLite Node + Map Bun (interpreted) Bun (compiled)


Throughput 2,099 req/s 4,534 req/s 9,434 req/s ~10,700 req/s


p50 latency 22.5ms 10.1ms 4.5ms ~3.9ms


p95 latency 29.1ms 14.9ms 7.4ms ~5.6ms


max latency 619ms 403ms 22ms ~17ms


Image size ~180MB ~180MB ~120MB ~68MB


**5x throughput. 28x better max latency. Container image down from 180MB to 68MB.** And a memory leak we had to find the hard way.


### In production


The local benchmarks told one story. Production told a better one. Memory dropped from ~192 MB to ~85 MB, but the real difference is in the stability.


CPU under Node had wild swings, spiking to nearly double its baseline under load. Bun holds a tight, flat line:


Event loop lag under Node spiked to 40-80ms. Bun barely registers:


## What we learned


**Profile before optimizing.** The SQLite removal was obvious in hindsight (isn't it always?), but we only found it because we profiled.


**Bun's HTTP model is fundamentally different from Node's.** Response lifecycle is tied to the promise, not the socket. If you're migrating long-poll or streaming endpoints, think about every code path that resolves (or doesn't resolve) your` Promise<Response>` .


**Compile your binary.**` bun build --compile` gave us 14% more throughput and 24% better p95 with zero code changes. Container image went from ~120MB to 68MB.


**Fix the leak, pay the CPU.** After fixing the memory leak, CPU went up about 5%. That's the cost of properly cleaning up connections that were previously being silently leaked.


**Benchmark at each step.** We ran the same load test (500 controllers, 50 VUs, 30s) at every phase. Without that, we wouldn't have known which changes mattered and which were noise.


**Staging is the real benchmark.** Local cores are faster than production vCPUs. We caught several surprises by deploying before declaring victory.


## Bonus: Debugging Bun in production


The short version of what we learned about debugging Bun:


-


**prom-client mostly works** under Bun, but several default collectors return bogus data.` nodejs_heap_size_*` is inaccurate (v8 compat layer),` process_heap_bytes` reports misleading numbers (Linux VmData), active handles is always 0, and GC metrics don't exist (JSC). We built custom gauges using` bun:jsc`` heapStats()` as a replacement.


-


**` heapStats()` is expensive.** It walks every object on the heap, blocking for 15-22ms on production-sized heaps. Collect on a timer, not during Prometheus scrapes.


-


**AbortSignal fires twice in Bun.** Once on client disconnect, and again when the response is sent. Without a guard, every matched connection triggers a spurious cleanup call. Without resolving the promise, every disconnected connection leaks its request context.


-


**RSS growing but heap flat?** That's native memory (mimalloc), not JS. Use` MIMALLOC_SHOW_STATS=1` to debug.


We turned all of this into a reusable agent skill (full reference below).


### Load testing with k6


Every benchmark in this post was produced with[k6](https://k6.io/) , Grafana's open source load testing tool. The CLI is excellent, the JavaScript scripting API makes it easy to build realistic test scenarios, and it can push results as Prometheus metrics for dashboarding. We also use it for performance testing other internal components, with pass/fail thresholds that work in CI.


Our setup has two components that run alongside the service:


**Controller simulator** (` controllers.js` ): spawns N controllers, each holding a persistent keep-alive socket with repeated GET` /warm-start` long-polls. This replicates production behavior where controllers reconnect on the same TCP connection.


**k6 load script** (` load.js` ): fires concurrent POST` /warm-start` requests with realistic payloads, tracking custom metrics for hit/miss rate and match latency.


The k6 script builds realistic` DequeuedMessage` payloads with configurable waitpoint sizes (for testing heavy payloads):


`
// Custom k6 metrics for warm start matching


const warmStartHits = new Counter("warm_start_hits");


const warmStartMisses = new Counter("warm_start_misses");


const matchDuration = new Trend("warm_start_match_duration", true);


export default function () {


const deployment = DEPLOYMENTS\[Math.floor(Math.random() * DEPLOYMENTS.length)\];


const machine = MACHINE_PRESETS\[Math.floor(Math.random() * MACHINE_PRESETS.length)\];


const msg = makeDequeuedMessage(deployment, machine);


const res = http.post(


\`${BASE_URL}/warm-start\`,


JSON.stringify({ dequeuedMessage: msg }),


{ headers: { "Content-Type": "application/json" } }


);


const body = JSON.parse(res.body);


if (body.didWarmStart) {


warmStartHits.add(1);


} else {


warmStartMisses.add(1);


}


matchDuration.add(res.timings.duration);


}


`


### The Bun debugging skill


We turned the debugging playbook into a reusable[agent skill](https://trigger.dev/blog/skills) . Save this as` .claude/skills/bun-debug/SKILL.md` in your project and any session can load it:


`
---


name: bun-debug


description: "Use when debugging Bun runtime issues: memory leaks,


heap profiling, JSC metrics, AbortSignal lifecycle,


prom-client compatibility."


---


# Bun Runtime Debugging


## Reference


- Official guide: https://bun.sh/blog/debugging-memory-leaks.md


- Node.js compat: https://bun.sh/docs/runtime/nodejs-compat.md


- Benchmarking/profiling: https://bun.sh/docs/project/benchmarking.md


## Memory Leak Investigation


### Heap Snapshots


\`\`\`bash


# Generate heap snapshot on exit


bun --heap-prof script.ts


# Markdown format (grep-friendly)


bun --heap-prof-md script.ts


\`\`\`


Load \`.heapsnapshot\` in Chrome DevTools Memory tab.


Compare two snapshots - the "Delta" column shows what's growing.


### JSC Heap Stats (for monitoring)


\`\`\`typescript


import { heapStats } from "bun:jsc";


const stats = heapStats();


// stats.heapSize - JSC heap size in bytes


// stats.objectCount - total live objects (leak indicator)


// stats.extraMemorySize - native memory held by JS objects


// stats.protectedObjectCount - GC-pinned objects


\`\`\`


**Key leak indicator**: \`objectCount\` trending upward.


If RSS grows but objectCount is flat, it's native memory, not JS.


### CPU Profiling


\`\`\`bash


bun --cpu-prof script.ts # Chrome DevTools format


bun --cpu-prof-md script.ts # Markdown format


bun --cpu-prof --cpu-prof-md script.ts # Both


\`\`\`


### Manual GC and Native Heap


\`\`\`typescript


Bun.gc(true); // synchronous GC


\`\`\`


\`\`\`bash


MIMALLOC_SHOW_STATS=1 bun script.ts # native allocator stats on exit


\`\`\`


## Common Leak Sources


### AbortSignal Listener Retention


\`\`\`typescript


// BAD: listener keeps closure alive until abort fires


req.signal.addEventListener("abort", () => {


cleanupLargeObject(data);


});


// BETTER: self-removes after firing


req.signal.addEventListener("abort", () => {


cleanup();


}, { once: true });


// BEST: guard against double-fire


// (Bun fires abort when response is sent too)


let resolved = false;


req.signal.addEventListener("abort", () => {


if (!resolved) cleanup();


}, { once: true });


\`\`\`


## prom-client Compatibility


| Metric | Status |


|--------|--------|


| \`process_cpu_*\` | Works |


| \`process_resident_memory_bytes\` | Works |


| \`nodejs_eventloop_lag_*\` | Works |


| \`nodejs_heap_size_*\` | Inaccurate (v8 compat) |


| \`process_heap_bytes\` | Misleading (Linux VmData) |


| \`nodejs_active_handles_total\` | Always 0 |


| \`nodejs_gc_duration_seconds\` | No data (JSC) |


### Bun.serve() HTTP Metrics


No built-in instrumentation. Wrap route handlers:


\`\`\`typescript


function withMetrics(route, method, handler) {


return async (req) => {


const start = performance.now();


const response = await handler(req);


httpDuration.observe(


{ method, route },


(performance.now() - start) / 1000


);


return response;


};


}


\`\`\`


`
