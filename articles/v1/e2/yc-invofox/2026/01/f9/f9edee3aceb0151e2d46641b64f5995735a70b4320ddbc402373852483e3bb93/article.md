---
schema_version: "1.0.0"
document_id: "f9edee3aceb0151e2d46641b64f5995735a70b4320ddbc402373852483e3bb93"
company_key: "yc-invofox"
company: "Invofox"
source_id: "yc-invofox-news-import-19c8879c7fc9"
canonical_url: "https://www.invofox.com/en/blog/azure-document-intelligence-at-scale-issues/"
published_at: "2026-01-30T00:00:00+00:00"
first_seen_at: "2026-07-22T00:28:19.833120+00:00"
fetched_at: "2026-07-28T22:22:54.577865+00:00"
content_hash: "sha256:359f406cc01b30a94e8864a04f6063c44f012e16531dc60dabc95886dac8237c"
---

# Problems You'll Run Into Using Azure Document Intelligence

Table of contents


- Disclaimer
- Issue #1: No webhooks. Polling is mandatory.
- Issue #2: Rate limits kill horizontal scaling
- Issue #3: Silent regional degradation
- Issue #4: The 2,000-page hard limit
- Issue #5: Unpredictable file failures
- Smaller (but painful) issues
- Who Azure Document Intelligence is good for
- Final thoughts


## **Disclaimer**


I’m the CTO of a company that builds document parsing software, so yes — I’m biased and I definitely have a horse in this race.


That said, building in this space forced me to evaluate and stress-test basically every OCR, VLM, LLM, and “document AI” product I could get my hands on. This post isn’t a hit piece — it’s a technical rant after dealing with the same issues one too many time in production.


Azure Document Intelligence (formerly Form Recognizer) looks great on paper: managed OCR, prebuilt models, tight Azure integration. For small volumes and simple workflows, it mostly does what it says.


Things start breaking down once you push it into **high-volume, latency-sensitive, production workloads** .


This post focuses specifically on Azure’s **Read model** — the core OCR engine that everything else builds on. I’m not covering custom or invoice-specific models, just the foundational OCR layer.


If you’re evaluating OCR vendors for real production usage, here are the issues you’ll almost certainly run into.


## Issue #1: No webhooks. **Polling is mandatory.**


Azure Document Intelligence has no webhook or callback support. Every async request must be polled.


Which, apparently, is still a thing in 2026.


You submit a document, get an operation ID back, and then repeatedly ask Azure whether it’s done yet. There’s no alternative.


Azure recommends polling every 1–2 seconds. Poll faster and you’ll hit rate limits. Poll slower and latency goes up.


### What this looks like in practice


I measured polling overhead across:


- Small (25 KB, 1-page) documents
- Large (1.6 MB, 100-page) PDFs
- Conservative (1s) vs aggressive (100ms) polling


The results are unintuitive but consistent:


- **75–90% of total processing time is spent polling, not OCR**
- Cutting polling intervals from 1s → 100ms:


- Saves ~20–30% total time
- Increases request volume **6–7×**


- Rate limits make aggressive polling unusable at scale


Polling burns your GET quota while doing no useful work.


### Why this matters architecturally


This isn’t something you can “optimize away”:


- You waste compute waiting
- You waste requests polling
- You hit rate limits before CPU or throughput limits
- Concurrency collapses under load


A webhook would eliminate all of this overhead. Polling isn’t a tuning problem — it’s an architectural limitation.


## Issue #2: Rate limits kill **horizontal scaling**


Azure’s standard tier rate limits are:


- **POST (analyze): 15 TPS**
- **GET (polling): 50 TPS**


Because polling is mandatory, **GET becomes the real bottleneck** .


With recommended polling:


- Each document consumes ~1 GET/sec while processing
- That caps you at ~50 concurrent documents per region


Now consider a very normal batch workload:


> Process 5,000 documents in 10 minutes


That requires:


- ~8.3 documents/sec submission
- ~9 seconds average processing time
- **~75 concurrent documents**


That’s already **50% over what a single region can handle** .


### The workaround (spoiler: it’s ugly)


To scale, you must:


- Deploy Document Intelligence in **multiple regions**
- Often across **multiple Azure subscriptions**
- Build a custom orchestrator that:


- Load-balances POSTs
- Tracks GET usage separately
- Handles regional failures


Azure provides none of this out of the box.


## Issue #3: Silent **regional degradation**


In any multi-region deployment, there are usually **one or two regions performing significantly worse than the others** .


Not always the same regions — but always some.


What I’ve repeatedly seen in production:


- Normal latency: ~5 seconds
- Degraded region: **60+ seconds**
- **Spikes in HTTP 500 errors** , often in bursts
- **No corresponding signal in Azure’s error or health dashboards** — error charts remain flat
- **A large number of HTTP 400 (InvalidRequest) responses** that succeed immediately on retry *with no modification to the request*


- These should clearly have been reported as server-side failures (500s), not client errors


- **Azure status page** : “All services operational”
- **No alerts, no notifications**


These slowdowns and error spikes can last **hours** .


The misleading error semantics make this particularly painful to debug:


- HTTP 500s surge without visibility in Azure’s monitoring
- HTTP 400s imply client-side issues, but retries succeed instantly, masking what are clearly transient backend failures


Because Issue #2 forces you into multi-region deployments, your system will happily continue routing traffic into degraded regions unless you actively detect and avoid them — compounding both latency and error rates.


### What you’re forced to build


If you care about latency or SLAs, you must implement:


- Per-region latency tracking
- Per-region error-rate tracking (independent of Azure’s dashboards)
- Baselines and anomaly detection
- Automatic regional failover
- Alerting when a region degrades


This is monitoring and error-classification infrastructure Azure should provide — but doesn’t.


## Issue #4: The **2,000-page hard limit**


Azure has a hard cap:


- **2,000 pages per document** (paid tier)
- Anything larger must be manually chunked


This is not an edge case.


Real-world documents that routinely exceed this:


- Mortgage packages
- Legal discovery
- Financial disclosures


Chunking sounds simple until you do it:


- Split PDFs
- Track page offsets
- Reassemble results
- Deal with tables and paragraphs split across chunks


Want to process chunks in parallel? Congratulations — you just made the rate-limit problem worse.


Large documents amplify every other issue: polling overhead, rate limits, and orchestration complexity.


## Issue #5: Unpredictable **file failures**


Some files just… fail.


No clear pattern. No useful error messages.


Examples I’ve hit in production:


- PDFs that open fine everywhere → “corrupted”
- Images that render correctly → “invalid format”
- Barcode extraction failing entire documents
- Encrypted PDFs failing even after unlocking


Error messages are typically:


> “An unexpected error occurred”


Which is about as actionable as it sounds.


### The cost of workarounds


To survive in production you end up building:


- Feature-flag retries (wasting requests)
- PDF repair pipelines (Ghostscript, Poppler, ImageMagick)
- Image re-encoding and metadata stripping
- Fallback processing paths


All of this adds latency, cost, and complexity — and none of it is predictable.


To be fair: document processing is hard. These issues aren’t unique to Azure. But Azure gives you very little visibility into *why* something failed.


## Smaller (but **painful** ) issues


A few things that don’t break systems, but slowly drain your sanity:


- **Pricing opacity** : Pricing is per-page and feature-based, but responses don’t include cost information. If you want accurate accounting, you have to track it yourself.
- **Studio vs API mismatches** : “It works in the Studio” often does *not* mean it works via API. Different defaults, versions, and parameters lead to misleading POCs.
- **Breaking SDK changes** : API and SDK upgrades regularly introduce breaking changes, forcing codebase-wide migrations and accuracy re-validation.


## Who Azure Document Intelligence is **good for**


Despite all this, it can be the right tool if:


- Your volumes are low
- Latency isn’t critical
- Documents are small and simple
- You’re deeply embedded in Azure
- You have strong internal platform teams


Just go in knowing what you’ll need to build around it.


## **Final thoughts**


This isn’t meant to bash Azure. It’s a powerful platform with serious engineering behind it.


But once you operate at scale, many of the hardest problems aren’t accuracy — they’re **architecture, limits, and operational complexity** .


If you’re evaluating OCR vendors, these trade-offs matter *before* you’re locked in.


I wish I’d seen a post like this earlier.
