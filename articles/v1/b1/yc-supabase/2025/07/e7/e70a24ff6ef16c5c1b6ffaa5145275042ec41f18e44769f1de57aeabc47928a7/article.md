---
schema_version: "1.0.0"
document_id: "e70a24ff6ef16c5c1b6ffaa5145275042ec41f18e44769f1de57aeabc47928a7"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/persistent-storage-for-faster-edge-functions"
published_at: "2025-07-18T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:e614eeb4135229557c0d0378d516ed142dd6c3947f1dd5b2e78edacf85fb7390"
---

# Persistent Storage and 97% Faster Cold Starts for Edge Functions

Today, we are introducing Persistent Storage and up to 97% faster cold start times for Edge Functions. Previously, Edge Functions only supported ephemeral file storage by writing to` /tmp` directory. Many common libraries for performing tasks, such as zipping/unzipping files and image transformations, are built to work with persistent file storage, so making them work with Edge Functions required extra steps.


The persistent storage option is built on top of the S3 protocol. It allows you to mount any[S3-compatible bucket](https://supabase.com/docs/guides/storage/s3/compatibility) , including[Supabase Storage Buckets](https://supabase.com/docs/guides/storage) , as a directory for your Edge Functions. You can perform operations such as reading and writing files to the mounted buckets as you would in a POSIX file system.


`
_10


// read from S3 bucket


_10


const data = await Deno.readFile('/s3/my-bucket/results.csv')


_10


_10


// make a directory


_10


await Deno.mkdir('/s3/my-bucket/sub-dir')


_10


_10


// write to S3 bucket


_10


await Deno.writeTextFile('/s3/my-bucket/demo.txt', 'hello world')


`


## How to configure#


To access an S3 bucket from Edge Functions, you must set the following as environment variables in Edge Function Secrets.


- ` S3FS_ENDPOINT_URL`
- ` S3FS_REGION`
- ` S3FS_ACCESS_KEY_ID`
- ` S3FS_SECRET_ACCESS_KEY`


If you are using Supabase Storage,[follow this guide](https://supabase.com/docs/guides/storage/s3/authentication) to enable and create an access key and id.


## Use Case: SQLite in Edge Functions#


The S3 File System simplifies workflows that involve reading and transforming data stored in an S3 bucket.


For example, imagine you are building an IoT app where a device backs up its SQLite database to S3. You can set up a scheduled Edge Function to read this data and then push the data to your primary Postgres database for aggregates and reporting.


`
_37


// Following example is simplified for readability


_37


_37


import { DB } from "https://deno.land/x/sqlite@v3.9.1/mod.ts";


_37


import { supabase } from '../shared/client.ts'


_37


_37


const today = new Date().toISOString().split('T')\[0\]


_37


const backupDBPath = \`backups/backup-${today}.db\`


_37


_37


// Use S3 FS to read the Sqlite DB


_37


const data = Deno.readFileSync(\`/s3/${backupDBPath}\`);


_37


_37


// Create an in-memory SQLite from the data downloaded from S3


_37


// This is faster than directly reading from S3


_37


const db = new DB();


_37


db.deserialize(data);


_37


_37


function calculateStats(rows: IoTData\[\], date: string): StatsSummary {


_37


// ....


_37


}


_37


_37


Deno.serve(async (req)=>{


_37


// Assuming IoT data is stored in a table called 'sensor_data'


_37


const rows = db.queryEntries<IoTData>(\`


_37


SELECT * FROM sensor_data


_37


WHERE date(timestamp) = date('now', 'localtime')


_37


\`)


_37


_37


// Calculate statistics


_37


const stats = calculateStats(rows, today)


_37


_37


// Insert stats into Supabase


_37


const { data, error } = await supabase


_37


.from('iot_daily_stats')


_37


.insert(\[stats\])


_37


_37


return new Response("OK);


_37


});


`


## 97% Faster Function Boot Times, Even Under Load#


Previously, Edge Functions with large dependencies or doing preparation work at the start (e.g., parsing/loading configs, initializing AI models) would incur a noticeable boot delay. Sometimes, these slow neighbors can impact other functions running on the same machine. All JavaScript *workers* in the Supabase Edge Functions Runtime were cooperatively scheduled on the same[Tokio thread pool](https://github.com/tokio-rs/tokio) . If one worker had heavy startup logic, such as parsing JavaScript modules or running synchronous operations, it could delay every worker scheduled after. This led to occasional long‑tail latency spikes in high-traffic projects.


To address this issue, we moved workers which are still performing initial script evaluation onto a dedicated blocking pool. This approach prevents heavy initialization tasks from blocking the Tokio thread, significantly reducing boot time spikes for other functions.


### The result#


Boot times are now more predictable and wait times for cold starts are now much faster. Here’s a result of a[benchmark](https://github.com/supabase/edge-runtime/blob/develop/k6/specs/mixed.ts) we did to compare boot times before and after these changes.


Metric Before After (Delta)


**Avg** **870 ms** **42 ms** **95 %**


**P95** 8,502 ms 86 ms **99 %**


**P99** 15,069 ms 460 ms **97 %**


**Worst** 24,300 ms 1 630 ms **93 %**


**Spikes > 1 s** 47 % 4 % **43 pp**


## Support for Synchronous APIs#


By offloading expensive compute at function boot time onto a separate pool, we were able to enable the use of synchronous File APIs during function boot time. Some libraries only support synchronous File APIs (eg, SQLite), and this would allow you to set them up on Edge Functions before it starts processing requests.


You can now safely use the following synchronous Deno APIs (and their Node counterparts) *during* initial script evaluation:


- Deno.statSync
- Deno.removeSync
- Deno.writeFileSync
- Deno.writeTextFileSync
- Deno.readFileSync
- Deno.readTextFileSync
- Deno.mkdirSync
- Deno.makeTempDirSync
- Deno.readDirSync


**Keep in mind** that the sync APIs are available only during initial script evaluation and aren’t supported in callbacks like HTTP handlers or setTimeout.


`
_10


Deno.statSync('...') // ✅


_10


_10


setTimeout(() => {


_10


Deno.statSync('...') // 💣 ERROR! Deno.statSync is blocklisted on the current context


_10


})


_10


_10


Deno.serve(() => {


_10


_10


})


`


## How to try#


These changes are already deployed and available to use on all regions.
