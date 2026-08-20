---
schema_version: "1.0.0"
document_id: "6d646b05ad45bfcfafcaf9ecefa65c6eb3297dbe879ffb6ec3a13ec217353589"
company_key: "yc-todesktop"
company: "ToDesktop"
source_id: "yc-todesktop-news-import-b82ee277d06d"
canonical_url: "https://www.todesktop.com/blog/posts/dynamically-signing-debian-repo-manifests-with-worker-service-bindings"
published_at: "2025-04-08T10:12:00+00:00"
first_seen_at: "2026-07-24T04:14:47.478048+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:e787fdc50f036533b7c04621d038627e177075eea4ffe32d7e8fc706a34e4afd"
---

# Dynamically Signing Debian Repo Manifests with Worker Service Bindings

We recently added[Debian APT repository support to ToDesktop](https://www.todesktop.com/electron/docs/guides/debian-apt-repository-support) . We needed to dynamically sign Debian repo manifests with PGP without exposing PGP private keys to unnecessary risk or turning our system into a tangled mess. The solution? Cloudflare Workers service bindings. They let us isolate sensitive operations, keep our codebase clean, and get isolated visibility into what's happening. Here's how we made it work.


## The Problem: Keys Are a Liability


Customers have been asking us for Debian APT repo support for a long time. We finally decided to do it. With most other solutions, the Debian manifest files are signed at build time. This didn't really jive with how we store version release info in our database and also it meant giving build containers access to the private keys — which we wanted to avoid.


We looked at letting our main CDN worker handle signing directly? But, that’s just as bad. Keys in the wrong place would expose unnecessary risk. We needed a way to sign on the fly without making things overly complexity.


## The Solution: Service Bindings


With Cloudflare Workers service bindings, we split the load. The main CDN worker stays lean, serving files like a champ. When a request hits an uncached Debian` Release` endpoint needing a signed manifest, it calls a separate PGP worker through a binding. The CDN worker doesn’t need to know how to sign—it just says, “Hey, handle this,” and gets a signed response back.


## How it works


Service bindings let one worker call another’s functions like they’re local. Cloudflare’s magic makes it seamless. Here’s the gist in the CDN worker:


```text
const   signedContent =  await   env. SIGNING_WORKER  . signMessage  (content);


```


That` env.SIGNING_WORKER` is the binding, pointing to our PGP worker. The CDN worker stays clean and clueless about signing operations and private keys.


## Why?


It's not immediately obvious but this isn’t just a neat trick. It actually has a bunch of benefits for security, maintenance, and visibility. Here’s the breakdown:


#### 1. Security


- **Key Isolation** : Private keys live in the PGP worker, fetched from Azure KeyVault only when needed. Build containers? No keys. CDN worker? No keys. If something gets compromised, the blast radius is isolated.
- **Audit Power** : Since the PGP worker is standalone, it's easy for us to pile on logging and audit controls without cluttering the CDN. Every signing request, every key access—tracked and locked down.


#### 2. Observability


- **Granular Insight** : With signing isolated, we can see exactly what’s happening: how many requests, any errors, CPU time spent on signing? It's much easier to inspect metrics when we can just use Cloudflares worker-level metrics.
- **Fixes Made Easy** : If signing hiccups, we know right where to look. No digging through CDN noise to troubleshoot.


#### 3. Modularity


- **Focused Roles** : The CDN worker sticks to serving files. The PGP worker handles the crypto. Clear jobs, no overlap.
- **Painless Updates** : Need to tweak the PGP logic? Debug a signing bug? We can do it without touching the CDN worker.


#### 4. Bonus: A Tiny Performance Nod


- **Smaller Footprint** : The CDN worker stays at 20KB (gzipped) instead of ballooning to 200KB (gzipped) with PGP code baked in. Cold starts might be a hair faster—nice, but not the main event.
- **Independent Scaling** : If something spikes, the PGP worker is scaled independently.


## Typescript


Typescript helps make sure that the interface is correct, everyone knows what’s expected. The PGP worker’s interface looks like this:


```text
import   {  WorkerEntrypoint   }  from    'cloudflare:workers'  ;


export    default    class    PgpWorker    extends    WorkerEntrypoint   {
// we use openPGP.js to sign messages
public    async    signMessage  ( body  :  string  ,  detached  :  boolean  ):  Promise  < Response  >;
}


```


And the CDN worker's` Env` :


```text
import    type    PgpWorker    from    '@todesktop/pgp'  ;
import   {  Service   }  from    '@cloudflare/workers-types'  ;


export    interface    Env   {
ENVIRONMENT  :  'dev'   |  'prod'  ;
R2_BUCKET  : R2Bucket;
SIGNING_WORKER  :  Service  < PgpWorker  >;
}


```


Now,` env.SIGNING_WORKER.signMessage` is type-safe with autocompletion.


## Takeaway


By isolating our signing operations in a dedicated worker, we achieved better security through key isolation, improved visibility through focused monitoring, and simplified maintenance through clear separation of concerns.


The result is a system that's more secure, easier to maintain, and provides the on-the-fly signing capabilities our customers need.
