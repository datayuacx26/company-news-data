---
schema_version: "1.0.0"
document_id: "e394a802177a94b3ca0a927e4446ffb5285e6da6e9997b2cbab93bf2593dcbf4"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2022-09-15-how-to-substantially-slow-down-your-nodejs-server/"
published_at: "2022-09-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:7476fcd8ef5f08c75a8d53e16b7a59dbb103bf6bae3ef41cd1b2795be5559ff4"
---

# How to substantially slow down your Node.js server

Back in March 2022, after spending a considerable amount of effort migrating our monolithic Node.js[GraphQL](https://graphql.org/) server from[Express](https://github.com/expressjs/express) to[Fastify](https://github.com/fastify/fastify) , we noticed absolutely no performance improvements in production. That hit us like a bombshell, especially because Fastify performed exceptionally well in our[k6](https://github.com/grafana/k6) load tests in staging, where it responded to HTTP requests 107% (more than two times) faster on average than Express!


What on earth did we miss? We tried to be quite diligent with load testing in order to gather accurate and actionable metrics:


- We made the Express and Fastify staging deployments as identical as possible, where the only point of divergence would be which web framework was running under the hood.
- We ran the load tests for both staging deployments concurrently in order to simulate identical running conditions, because the data sources that GraphQL fetches data from automatically scale.
- We already have a staging environment that simulates the production environment to a high degree of parity, which simplifies the deployment aspect of the tests.


With no obvious explanation in sight, we embarked on an investigative journey. We visually combed through the Express-to-Fastify-migration PR diff multiple times hoping for a sudden “Eureka!” moment, but unfortunately to no avail.


So we had to bring out the big guns: live profiling.


We rolled out a single[pod](https://kubernetes.io/docs/concepts/workloads/pods) to production, equipped with the[heapdump](https://github.com/bnoordhuis/node-heapdump) package. This package allowed us to take a snapshot of the V8 heap in a live deployment\[1\] . Afterwards, we directed a tiny portion of our incoming traffic to this special pod.


While inspecting the heap dump, we found out that our log redaction logic was consuming obnoxious amounts of memory.


## Log redaction


Before we dig into that, let’s answer the question: What exactly is log redaction?


Log redaction is the process of overwriting sensitive information (e.g. passwords, access tokens, names, emails) before it is written to the application logs.


Not only does log redaction simplify our compliance with[GDPR](https://gdpr-info.eu/) , it also makes our logs a lot less confidential. This makes them viewable by more developers for monitoring and debugging purposes.


Since our Express days, we have been using a super fast JSON logger called[pino](https://github.com/pinojs/pino) . We used to redact sensitive information from incoming HTTP requests manually; where we would intercept JavaScript objects before being logged by` pino` , traverse them deeply (using the[deepdash](https://github.com/YuriGor/deepdash) package), and then overwrite any sensitive information if found.


That worked like a charm until the migration to Fastify, when we read on the[Fastify docs](https://www.fastify.io/docs/v4.2.x/Reference/Logging/#usage) that` pino` (which Fastify uses internally and recommends) actually has built-in low-overhead redaction support. A` yarn remove deepdash` later, we started exploring` pino` ’s redaction.


The redaction feature can be configured via` pino` ’s` redact` option:


```text
const   pino   =   require  (  'pino'  );


const   logger   =   pino  ({
redact: {
paths: [  'req.headers.authorization'  ],
censor:   '[REDACTED]'  ,
},
});


logger.  info  ({
req: {
headers: {
origin:   'https://www.trivago.com'  ,
authorization:   'Bearer abcde12345'  ,
},
},
});
```


The code above will log the following to the console:


```text
{
"level"  :   30  ,
"time"  :   1656421104743  ,
"pid"  :   17724  ,
"hostname"  :   "iHqQ3WVLaUGgh3meVbHYU"  ,
"req"  : {
"headers"  : {
"origin"  :   "https://www.trivago.com"  ,
"authorization"  :   "[REDACTED]"
}
}
}
```


Quite elegant, right? No object traversal of any kind - you just specify the paths you’d like to redact, and voilà,` pino` redacts them for you.


## Limitation


Unfortunately, real-life use cases are rarely this neat: We needed to stringify the GraphQL variables object after redaction and before logging, to prevent log post-processing tools (e.g. Elasticsearch/Kibana) from indexing individual GraphQL variable properties.


This limitation lead us to abandon` pino` ’s baked-in redaction. However, we still wanted low-overhead redaction, so we explored using the underlying redaction library that` pino` uses called[fast-redact](https://github.com/davidmarkclements/fast-redact) . It promises “very fast object redaction” by being only ≈ 1% slower than` JSON.stringify` . We used it as such:


```text
const   fastRedact   =   require  (  'fast-redact'  );
const   _   =   require  (  'lodash/fp'  );
const   pino   =   require  (  'pino'  );


const   logger   =   pino  ({
// See: https://getpino.io/#/docs/api?id=serializers-object
serializers: {
req  : (  req  )   =>   {
// Redact sensitive information from request object.
const   redactedReq   =   fastRedact  ({
paths: [  'headers.authorization'  ,   'body.variables.email'  ],
censor:   '[REDACTED]'  ,
serialize:   false  ,
})(req);


// JSON-stringify GraphQL variables, to prevent log post-processing tools
// (e.g. Elasticsearch/Kibana) from indexing individual GraphQL variable
// properties.
return   _.  set  (
'body.variables'  ,
JSON  .  stringify  (redactedReq.body.variables)
)(redactedReq);
},
},
});
```


A little bit more code, but it works fine, right? Nope!


## Fatal mistake


There’s a grave error in the code above that was impeding our Node.js server’s performance by consuming insane amounts of memory per request, and therefore heavily tanking the rate at which the server can handle requests - can you spot it?


Spoiler: it’s this innocent-looking bit ⤵️


```text
// Redact sensitive information from request object.
const   redactedReq   =   fastRedact  ({
paths: [  'headers.authorization'  ,   'body.variables.email'  ],
censor:   '[REDACTED]'  ,
serialize:   false  ,
})(req);
```


But how? Doesn’t` fast-redact` offer super fast redaction? The answer to that is: Yes, when used correctly. Unluckily, we used it incorrectly in the worst way possible.


On every request, we made` fast-redact` regenerate its redaction function!` fast-redact` ’s partial application nature was not simply adherence to functional programming aesthetics - it actually had a reason! Here’s how` fast-redact` should be used (as demonstrated in its[README.md](https://github.com/davidmarkclements/fast-redact/blob/92b27f280aa28ecc01b3a844bdf0c5c67d25a7c3/readme.md#default-usage) ):


```text
const   fastRedact   =   require  (  'fast-redact'  );
const   _   =   require  (  'lodash/fp'  );
const   pino   =   require  (  'pino'  );


// Generate redaction function ONCE ✅
const   redact   =   fastRedact  ({
paths: [  'headers.authorization'  ,   'body.variables.email'  ],
censor:   '[REDACTED]'  ,
serialize:   false  ,
});


const   logger   =   pino  ({
// See: https://getpino.io/#/docs/api?id=serializers-object
serializers: {
req  : (  req  )   =>   {
// Redact sensitive information from request object.
const   redactedReq   =   redact  (req);


// JSON-stringify GraphQL variables, to prevent log post-processing tools
// (e.g. Elasticsearch/Kibana) from indexing individual GraphQL variable
// properties.
return   _.  set  (
'body.variables'  ,
JSON  .  stringify  (redactedReq.body.variables)
)(redactedReq);
},
},
});
```


To give you a sense of the performance penalty incurred by using` fast-redact` the wrong way, here’s an isolated benchmark:


```text
fast-redact (incorrect usage) x 676 ops/sec ±5.28% (71 runs sampled)
fast-redact (correct usage) x 161,399,154 ops/sec ±1.34% (85 runs sampled)
Fastest is fast-redact (correct usage)
```


Using` fast-redact` incorrectly is 238,756 times slower! To put this into perspective: If the distance between Earth and the Sun is 1[AU](https://en.wikipedia.org/wiki/Astronomical_unit) (≈ 8.3 light minutes), then 238,756 AU (≈ 3.8 light years) is very roughly the distance between Earth and[Proxima Centauri](https://en.wikipedia.org/wiki/Proxima_Centauri) , the second nearest star to Earth after our Sun!


## Performance heaven


After rolling out the hotfix to production, the Node.js server’s memory usage dropped from an average of 4 GBs to 2 GBs, which in turn made our Kubernetes cluster in the EU region automatically scale down from 310 pods to 160 pods!


In other words, half the pods are now able to handle the same amount of incoming traffic in our busiest region.


## Final mystery


This leads us to the final mystery: How did we not catch this when we ran load tests of Express vs Fastify in staging?


The (rather apparent in hindsight) answer to that is: We manually disabled logging in the Express & Fastify deployments in staging during the load tests, because we thought that disk I/O was “irrelevant” in the context of load tests.


What we didn’t think about was that disabling logging also disabled the logic that surrounds logging, such as redaction 🤦‍♂️


## Takeaways


- When it comes to performance, Fastify was (and is) indeed superior to Express. Had we not introduced the log redaction bug while migrating, we would have reaped Fastify’s rather substantial performance benefits sooner.
- We should have resisted the urge to reimplement our log redaction logic while migrating from Express to Fastify. This could have been introduced in a follow-up PR.
- When running load tests against staging deployments, one should not disable flags that are enabled in production, no matter how out-of-scope they seem.


## Footnotes


1. Node.js actually has built-in support for generating V8 heap snapshots:[v8.getHeapSnapshot()](https://nodejs.org/docs/latest-v16.x/api/v8.html#v8getheapsnapshot)
