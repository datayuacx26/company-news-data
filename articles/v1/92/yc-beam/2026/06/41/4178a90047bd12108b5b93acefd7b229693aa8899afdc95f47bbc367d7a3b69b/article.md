---
schema_version: "1.0.0"
document_id: "4178a90047bd12108b5b93acefd7b229693aa8899afdc95f47bbc367d7a3b69b"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/e2b-pricing-explained"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-24T09:37:22.766203+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:26ee64d54b8e035ee490ad3052e24fe645058cdbeb3f3a9caa9df666a5209e1b"
---

# E2B Pricing Explained (2026): Tiers, Limits, and Cheaper Alternatives

[← All posts](https://www.beam.cloud/blog) Engineering


# E2B Pricing Explained (2026): Tiers, Limits, and Cheaper Alternatives


Tim Huynh


June 16, 2026


9 min read


E2B's pricing looks simple — a free tier, a $150/month Pro plan, and per-second compute — but the number that ends up on your invoice depends on how many sandboxes your agent runs, how long they live, and whether you ever need a GPU (you can't get one). For anything past a prototype, the per-second rate matters less than the structural costs around it: the subscription floor, the concurrency caps, and the compute you *can't* bring.


This guide breaks down E2B's 2026 pricing in full, runs the real monthly math, and compares the alternatives when cost is the deciding factor — starting with Beam, whose per-second model has no subscription floor, GPUs at roughly half the going rate, and a self-host path so you can run on compute you already pay for.


## Key Takeaways


- **E2B's per-second CPU/RAM rate is competitive — and identical to Daytona's** ($0.0504/vCPU/hr, $0.0162/GiB/hr). On raw CPU sandboxes, E2B is *not* expensive; Modal is even cheaper.
- **The real cost is structural, not per-second.** Production use means the **$150/month Pro plan** (for 24-hour sessions and 100 concurrent sandboxes), and there is **no GPU option at any tier** .
- **Paused sandboxes are free to keep running but not free to keep.** Pause/resume preserves filesystem + memory indefinitely — which means snapshot storage accrues until you delete them.
- **If you need a GPU in the sandbox, E2B can't help — and the alternatives that can vary 2x in price.** Beam's H100 is **$1.74/hr** versus **$3.95/hr** on Daytona and Modal.
- **Self-hosting is the biggest lever on cost.** Beam's AGPL-3.0 runtime (and E2B's own` e2b-dev/infra` ) let you run on your own cloud credits, turning the vendor's price into a ceiling instead of a floor.


## E2B pricing, tier by tier


All figures verified at e2b.dev/pricing on 2026-06-16.


Hobby (Free) Pro Enterprise


Monthly cost $0 $150/mo Custom


Included credits $100 one-time (subscription) Custom


Max session length 1 hour 24 hours Custom


Max concurrent sandboxes 20 100 (up to 1,100 on request) Custom


Free storage 10 GiB 20 GiB Custom


On top of any tier, you pay **usage-based compute per second** :


- **vCPU:** $0.000014/s per vCPU — i.e. **$0.0504/vCPU/hour**
- **RAM:** $0.0000045/GiB/s — i.e. **$0.0162/GiB/hour**
- **Storage:** included up to the tier limit; more on request


There is **no GPU offering** on any E2B tier. \[e2b.dev/pricing, 2026-06-16\]


### How sessions, timeouts, and persistence affect cost


E2B sandboxes have a default timeout of 5 minutes, extendable up to the tier's max continuous runtime (1 hour Hobby, 24 hours Pro). Crucially, E2B now supports **pause/resume** : pausing saves both filesystem and memory state, resuming restores it, and the continuous-runtime clock resets on resume. Pausing takes roughly **4 seconds per 1 GiB of RAM** ; resuming takes about 1 second. \[e2b.dev/docs — persistence, 2026-06-16\]


The cost implication that catches teams out: **paused sandboxes are retained indefinitely** with no automatic TTL. You stop paying for compute while paused, but the saved filesystem + memory snapshot consumes storage until you explicitly delete it. An agent that pauses thousands of sandboxes and never reaps them accumulates storage cost silently.


## What E2B actually costs in production


Per-second rates are abstract; invoices are not. Take a representative agent workload: a **2 vCPU / 4 GiB** sandbox.


- Per second: (2 × $0.000014) + (4 × $0.0000045) = **$0.000046/s**
- Per hour: **$0.1656/hr** (~$0.17)


Now run it like a product — **10 concurrent sandboxes, 8 hours/day, 22 working days** = 1,760 sandbox-hours/month:


- Compute: 1,760 × $0.1656 = **$291.46**
- Plus Pro plan (required for 24-hour sessions and sustained usage beyond the one-time $100 credit): **$150.00**
- **≈ $441/month**


That is a reasonable number for what you get. The questions that decide whether it stays reasonable:


- **Does your concurrency exceed 100?** You move to on-request pricing (up to 1,100) or Enterprise.
- **Do you need a GPU?** You can't — you now run a *second* platform alongside E2B, with its own bill.
- **Is your usage spiky?** The $150/month floor is fixed whether you run 1,760 hours or 50.


## Where E2B pricing gets expensive


E2B is priced well for its core case — CPU-bound, short-to-medium sandboxes behind a managed API. The cost pressure shows up at the edges:


- **The $150/month floor punishes low or spiky usage.** A prototype or a low-traffic agent pays $150 before the first second of compute, just to unlock 24-hour sessions and real concurrency.
- **No GPU, at any price.** Firecracker microVMs don't do GPU passthrough. If your sandbox runs a model — vision, local LLM, embeddings — E2B is structurally out, and you're integrating a separate GPU provider.
- **You can't bring your own compute (easily).** The managed product can't run on your reserved instances or committed-use discounts. (E2B's` e2b-dev/infra` is self-hostable via Terraform/Nomad, but that's an infrastructure project, not a billing toggle.)
- **Snapshot sprawl.** Indefinitely-retained paused sandboxes turn into a storage line item if you don't reap them.


None of these make E2B a bad choice — they make it a *specific* choice. When they bite, here are the alternatives, ranked by how they address cost.


## Cheaper alternatives, ranked


### 1. Beam


**No subscription floor, GPUs at ~half the rate, and a self-host path.** Beam prices sandbox compute per second like E2B, but removes the structural costs that inflate E2B invoices.


- **No required subscription for production.** The Developer plan is **free + usage with $30/month in credits** ; Team is $89/month if you need more seats and concurrency. You are not paying $150 to unlock basic session length. \[beam.cloud/pricing, 2026-06-16\]
- **GPUs in the sandbox, priced aggressively:** H100 PCIE **$1.74/hr** , A100 80GB **$1.30/hr** , RTX 4090 **$0.42/hr** , plus per-second serverless GPU (RTX 4090 $0.000192/s, A10G $0.000292/s). \[beam.cloud/pricing, 2026-06-16\]
- **Self-host / BYOC:** the AGPL-3.0` beta9` runtime runs on your own AWS/GCP/Azure/Hetzner credits or hardware, so the managed price is a ceiling. \[github.com/beam-cloud/beta9, 2026-06-16\]


**The honest trade-off:** Beam's *serverless CPU* rate ($0.0000528/core/s ≈ $0.19/core-hr) is higher than E2B's $0.0504/vCPU-hr. For pure-CPU sandboxes at steady high volume, E2B can be cheaper per hour. Beam wins on cost when you (a) have low or spiky usage and want to skip the $150 floor, (b) need a GPU, or (c) can self-host on compute you already own. For the GPU case the gap is decisive:


**100 H100-hours/month:** Beam **$174** · Daytona **$395** · Modal **$395** · E2B **not available**


```text


```


**Best for:** teams that need GPUs in the sandbox, have spiky usage that the $150 floor would tax, or want to self-host on their own cloud credits.


### 2. Daytona


**Same CPU/RAM price as E2B, with persistence built in.** Daytona's compute rates are *identical* to E2B's — $0.0504/vCPU/hr and $0.0162/GiB/hr — so it's not a way to pay less per second. \[daytona.io/pricing, 2026-06-16\]


- **Free credits:** $200 for new users (vs E2B's $100); startups up to $50k.
- **GPU:** H100 $3.95/hr, RTX PRO 6000 $3.03/hr — available, but more than 2x Beam's H100.
- **Persistence:** pause/archive lifecycle is the product's core strength.


**Best for:** teams that want E2B-equivalent pricing but need first-class persistent workspaces, and don't mind paying $3.95/hr if they occasionally need a GPU.


### 3. Modal


**The cheapest raw CPU — if you don't need to self-host.** Modal has the lowest CPU rate in this comparison: $0.0000131/physical-core/s (a physical core ≈ 2 vCPU) and $0.00000222/GiB/s. \[modal.com/pricing, 2026-06-16\]


- **Free credits:** $30/month on the Starter plan; no subscription floor.
- **GPU:** H100 $0.001097/s ( **$3.95/hr** ), A100 80GB $2.50/hr — same GPU tier as Daytona, more than Beam.
- **Caveat:** sandboxes are one feature of a broader serverless platform, and Modal is **managed-only — no self-host** .


**Best for:** cost-sensitive, CPU-bound sandbox workloads where you want the lowest per-second rate and don't need to run on your own infrastructure.


## Pricing comparison


E2B Beam Daytona Modal


Subscription floor (production) $150/mo None (free Developer) None None


Free credits $100 once $30/mo $200 once $30/mo


CPU $0.0504/vCPU-hr $0.19/core-hr $0.0504/vCPU-hr $0.047/core-hr †


RAM $0.0162/GiB-hr $0.0202/GiB-hr $0.0162/GiB-hr $0.0080/GiB-hr


H100 /hr n/a $1.74 $3.95 $3.95


GPU in sandbox No Yes Yes Yes


Self-host Heavy (infra, Terraform) Yes (beta9, Helm) Yes No


Persistence Pause/resume keep_warm_seconds Pause/archive Per-run


*CPU units differ by vendor — compare carefully. † Modal bills physical cores (~2 vCPU), so per-vCPU its CPU rate is ~$0.024/hr, the lowest here; Beam bills per core. All figures verified 2026-06-16.*


## Why Beam is the most cost-effective for production sandboxes


### You don't pay to exist


E2B's $150/month Pro plan is the price of admission for production-grade sessions and concurrency. Beam's Developer plan is free with $30/month in credits — you pay for compute you use, not for the right to use it. For agents with spiky or seasonal traffic, eliminating the floor is often a bigger saving than any per-second difference.


### GPUs at a price no managed sandbox matches


When the sandbox itself runs a model, E2B is out and the remaining options cluster at ~$3.95/hr for an H100 — except Beam at **$1.74/hr** . Over a month of even moderate GPU use, that gap dwarfs CPU-rate differences. And because GPU is the same API as CPU, you don't bolt on a second platform to get it.


### The price ceiling you can always undercut


Every managed rate above is a *retail* rate. Because` beta9` is AGPL-3.0 and supports BYOC, you can run the identical sandbox API on AWS/GCP/Azure/Hetzner credits — including reserved-instance and committed-use discounts you've already negotiated. The managed price becomes the most you'd pay, not the least. No other option here pairs a self-host path with GPU support and per-second managed pricing.


## FAQ


**Is E2B free?** There's a free Hobby tier with a one-time $100 in credits, 1-hour max sessions, 20 concurrent sandboxes, and 10 GiB storage. It's enough to prototype, not to run production — that requires the $150/month Pro plan. \[e2b.dev/pricing, 2026-06-16\]


**How much does E2B cost per hour?** A 2 vCPU / 4 GiB sandbox costs about **$0.17/hour** in compute ($0.0504/vCPU-hr + $0.0162/GiB-hr), on top of the $150/month Pro subscription for production-grade limits.


**Does E2B charge for paused sandboxes?** You stop paying compute while a sandbox is paused, but the saved filesystem + memory snapshot consumes storage, and paused sandboxes are retained indefinitely with no automatic deletion. Reap them or the storage cost accrues. \[e2b.dev/docs — persistence, 2026-06-16\]


**Does E2B support GPUs?** No. E2B's Firecracker microVMs don't offer GPU passthrough on any tier. For GPU-in-sandbox you need Beam (H100 $1.74/hr), Daytona ($3.95/hr), or Modal ($3.95/hr). \[e2b.dev/pricing; beam.cloud/pricing, 2026-06-16\]


**E2B vs Beam pricing — which is cheaper?** For pure-CPU sandboxes at steady high volume, E2B's per-second CPU rate ($0.0504/vCPU-hr) can beat Beam's serverless CPU. Beam is cheaper when you have low/spiky usage (no $150 floor), need a GPU ($1.74 vs n/a), or self-host on your own cloud credits.


**What's the cheapest code execution sandbox?** It depends on the workload. Cheapest raw CPU: Modal. Cheapest GPU: Beam. Lowest fixed cost for spiky usage: Beam or Modal (no subscription floor). Lowest *total* cost at scale: usually whichever you can self-host on committed compute — Beam or self-hosted E2B` infra` .


**Can I lower E2B costs by self-hosting?** Yes, in principle —` e2b-dev/infra` is Apache-2.0 and self-hostable via Terraform/Nomad on GCP (AWS beta). It shifts cost to your own compute but is a real infrastructure project to operate. Beam's` beta9` offers a lighter Helm-based self-host with the same effect on cost. \[github.com/e2b-dev/infra; github.com/beam-cloud/beta9, 2026-06-16\]


## Run sandboxes without the floor


E2B's pricing is fair for managed, CPU-bound sandboxes — but the $150/month floor, the missing GPU option, and the inability to bring your own compute add up fast at production scale. Beam prices per second with no subscription to run, puts H100s in the sandbox at $1.74/hour, and lets you self-host the same API on your own cloud credits when that's cheaper.


[Get started free](https://www.beam.cloud/) — $30/month in credits, no subscription floor, GPUs included.


## Sources


- E2B pricing + tiers: e2b.dev/pricing — verified 2026-06-16
- E2B persistence/timeouts: e2b.dev/docs (sandbox persistence) — verified 2026-06-16
- E2B self-host infrastructure: github.com/e2b-dev/infra — verified 2026-06-16
- Beam pricing: beam.cloud/pricing — verified 2026-06-16
- Beam runtime (` beta9` ), AGPL-3.0 + self-host: github.com/beam-cloud/beta9 — verified 2026-06-16
- Daytona pricing: daytona.io/pricing — verified 2026-06-16
- Modal pricing: modal.com/pricing — verified 2026-06-16


Tim Huynh


Published June 16, 2026


Keep Reading


## More from the Beam blog


[Engineering Tinker Model Pricing: What Fine-Tuning Costs in 2026 See what fine-tuning costs on Tinker, including worked cost examples and where renting GPUs gets cheaper. Tim Huynh](https://www.beam.cloud/blog/tinker-model-pricing)[Engineering What Is a Container, Really? Five Years of GPU Infrastructure Five years of GPU infrastructure at Beam — from ECS and Knative cold starts to a custom container runtime, FUSE lazy-loading, and a trustless binary. Luke Lombardi](https://www.beam.cloud/blog/what-is-a-container-really)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
