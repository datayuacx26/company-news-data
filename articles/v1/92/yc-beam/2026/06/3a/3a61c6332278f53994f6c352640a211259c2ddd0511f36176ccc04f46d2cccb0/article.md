---
schema_version: "1.0.0"
document_id: "3a61c6332278f53994f6c352640a211259c2ddd0511f36176ccc04f46d2cccb0"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/modal-pricing-explained"
published_at: "2026-06-27T21:23:48.373+00:00"
first_seen_at: "2026-07-24T09:37:22.766203+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:54d1116b62b8a375f960c7153f72bcbb728b5ebcb5d026926cd9e805703941f1"
---

# Modal Pricing Explained (2026): Plans, GPU Rates, and Why Serverless Gets Expensive at Scale

[← All posts](https://www.beam.cloud/blog) Tutorials


Engineering


# Modal Pricing Explained (2026): Plans, GPU Rates, and Why Serverless Gets Expensive at Scale


Nathanael Chiang


June 27, 2026


11 min read


Modal's pricing is simple to read and punishing at sustained scale. Because Modal is managed-only with no bring-your-own-cloud path, there's no lever to pull when the bill gets too big. This guide breaks down Modal's full 2026 pricing from[modal.com/pricing](https://modal.com/pricing) and[modal.com/docs](https://modal.com/docs) (verified June 27, 2026), runs the GPU math, concedes where Modal is cheap, and shows where a runtime like Beam wins on cost: same gVisor isolation, comparable developer experience, GPUs at less than half the price, and a self-host path.


## Key Takeaways


- **Modal's effective H100 runs ~$3.95/hr** , more expensive than every major dedicated GPU provider, and you can't undercut it because there's no BYOC or self-host. The same H100 is $1.74/hr on Beam. Modal's own marketing concedes serverless only beats dedicated compute "for spiky or unpredictable workloads."
- **The trap is structural.** It isn't a markup you can negotiate away. Region multipliers (1.5–1.75x), a 3x non-preemptible multiplier, and a Sandbox CPU rate 3x the standard Function rate all stack on top of base prices, and committed-use or reserved discounts you've negotiated with AWS, GCP, or Azure can't be applied to the underlying GPUs.
- **Modal's per-second CPU rate is competitive, but it isn't the floor.** Its $0.0000131/physical-core/s (~$0.047/core-hr) is low for serverless, yet Beam's on-demand CPU starts at $0.04/core-hr all-in, lower than Modal's effective rate. If your utilization is low and spiky and you never touch a GPU, Modal works. Above roughly half-utilization sustained on GPUs, it doesn't, and Modal gives you no way to switch models.
- **Self-hosting is the biggest lever on cost, and Modal doesn't offer it.** Beam's AGPL-3.0 runtime lets you run the identical sandbox and GPU API on your own cloud credits, turning the vendor's price into a ceiling instead of a floor. Modal's retail rate is both floor and ceiling.


## Modal's 2026 plans, tier by tier


All figures verified at[modal.com/pricing](https://modal.com/pricing) on 2026-06-27.


Starter Team Enterprise


Monthly cost $0 + compute $250/mo + compute Custom


Free credits $30/month $100/month Custom


Workspace seats Up to 3 Unlimited Unlimited


Containers / GPU concurrency 100 / 10 1,000 / 50 Custom


Log retention 1 day 30 days Custom


Committed cloud spend — — AWS / GCP only


There's no subscription floor on Starter, an advantage for spiky usage: you pay $0 to hold the door open and only pay for the compute you burn. The Team plan's $250/month is a fixed fee layered on top of per-second compute. Only Enterprise surfaces the option to "use committed spend on AWS and GCP," and as the next section covers, that's a billing mechanism, not BYOC.


## Resource pricing (the base rates)


From[modal.com/pricing](https://modal.com/pricing) (2026-06-27):


- **CPU:** $0.0000131 / physical core / second (a physical core = 2 vCPU), minimum 0.125 cores per container
- **Memory:** $0.00000222 / GiB / second
- **Volumes:** $0.09 / GiB / month, with 1 TiB/month free


That CPU rate is low. Because Modal bills per *physical* core (2 vCPU), the per-vCPU rate works out to roughly $0.0236/vCPU-hr, or about $0.047/core-hr. That's competitive for serverless, per-second compute. It isn't the cheapest CPU available, though: Beam's on-demand CPU starts at $0.04/core-hr all-in, below Modal's effective rate, and on-demand is the right model for the steady, predictable CPU workloads where per-second serverless billing stops paying off.


## GPU pricing (per second / per hour)


GPU Per second ≈ Per hour


Nvidia B200 $0.001736 ~$6.25


Nvidia H200 $0.001261 ~$4.54


Nvidia H100 $0.001097 ~$3.95


Nvidia RTX PRO 6000 $0.000842 ~$3.03


Nvidia A100 80GB $0.000694 ~$2.50


Nvidia A100 40GB $0.000583 ~$2.10


Nvidia L40S $0.000542 ~$1.95


Nvidia A10 $0.000306 ~$1.10


Nvidia L4 $0.000222 ~$0.80


Nvidia T4 $0.000164 ~$0.59


These are the headline rates. What stacks on top is where the real cost lives.


## The multipliers that inflate the bill


Three multipliers, all documented on Modal's pricing page and docs (2026-06-27), can compound on the same workload:


- **Region selection: 1.5–1.75x base prices** when you pin workloads to specific regions.
- **Non-preemptible execution: 3x base prices** for guaranteed (non-preemptible) Functions.
- **Sandbox tier: ~3x the standard CPU rate.** Modal **Sandboxes** are billed on a non-preemptible tier at **$0.00003942 / core / second** for CPU and **$0.00000672 / GiB / second** for memory, versus $0.0000131 and $0.00000222 for standard Functions. GPU rates inside Sandboxes follow standard GPU pricing.


That Sandbox premium is easy to miss, and it matters for agent builders: if you're running untrusted or agent-generated code in Modal Sandboxes, your CPU is 3x what the headline Function rate implies. Third-party analyses note that CPU workloads pinned to US regions with non-preemptible execution can hit a combined multiplier in the 3.75x range, turning an advertised base cost into something multiples higher on the invoice.


## Modal is managed-only: no BYOC, no self-host, no applying your own discounts


This is the structural core of the whole pricing argument. **Modal runs entirely on Modal's own managed, multi-cloud infrastructure. There's no bring-your-own-cloud option, no self-hosting, and no on-prem deployment** , verified across[modal.com/pricing](https://modal.com/pricing) ,[modal.com/docs](https://modal.com/docs) , and Modal's blog (2026-06-27), and corroborated by multiple independent comparisons describing Modal as "managed-only, no BYOC."


One nuance to get right. Modal *does* let enterprise customers **transact through the AWS and GCP marketplaces to draw down committed spend** : existing AWS or GCP spend commitments can be applied to Modal usage. But this is a **procurement and billing mechanism** . You buy Modal as a SaaS line item on your cloud bill to burn down a committed-spend agreement. It's **not** the same as:


- Running Modal's compute inside your own AWS/GCP/Azure account (BYOC), or
- Applying your negotiated **reserved-instance or committed-use hardware discounts** to the GPUs Modal runs your code on.


You always pay Modal's retail per-second rate for the underlying compute. There's no way to bring an H100 you're already paying for under a 1-year reservation and have Modal orchestrate it. (Azure is absent from Modal's marketplace messaging; only AWS and GCP are referenced.) The retail rate is both the floor and the ceiling. You can never go below it.


## Why this gets expensive at scale


Modal's pricing page makes the serverless case explicitly: it's more cost-effective than fixed on-demand or reserved compute for spiky or unpredictable workloads. That's true, and it's also the whole story. Serverless GPU economics invert as utilization climbs.


The industry consensus across multiple 2026 analyses: serverless wins at low utilization, but the crossover comes fast. Independent pricing breakdowns put the break-even somewhere between roughly 30% utilization (below which serverless wins on total cost) and ~61% daily utilization (about 15 hours a day) for an H100 specifically. Either way, per-second active billing on serverless carries a structural premium over equivalent dedicated GPU time. At ~$3.95/hr effective for an H100 under sustained load, Modal is more expensive than every major dedicated GPU provider, and for training jobs or high-throughput inference running above roughly half utilization, the per-second model stops being an advantage and becomes a cost multiplier.


For a workload like a continuous fine-tuning pipeline or a steady-traffic inference API, you'd be paying Modal's retail H100 rate around the clock, with no lever to convert that to a cheaper reserved or owned-hardware model. On a platform like Beam, that same retail rate is just a ceiling you can beat by self-hosting on compute you already own.


## The comparison: Modal vs Beam (and the rest)


Beam is the relevant alternative because it matches Modal where it counts and removes the structural cost trap. Beam uses **runc + gVisor** for isolation (Modal uses gVisor), achieves **sub-1-second cold starts** for cached custom images, and offers a comparable Python-first developer experience. Both platforms let you put high-end GPUs, including B200 and H200, directly inside a sandbox. The difference is the deployment model: Beam's runtime, **beta9, is open-source (AGPL-3.0)** and supports **BYOC and self-host on AWS, GCP, Azure, and Hetzner via Helm** . The managed price is a ceiling, not a floor.


**On-demand GPU: per hour**


GPU Modal (effective) Beam on-demand


B200 ~$6.25 $3.93 (SXM6)


H200 ~$4.54 $1.99 (SXM5)


H100 ~$3.95 $1.74 (PCIE)


A100 80GB ~$2.50 $1.30 (SXM4)


L40S ~$1.95 $0.72 (PCIE)


RTX PRO 6000 ~$3.03 $1.04 (PCIE)


A6000 n/a $0.51


RTX 5090 n/a $0.68


RTX 4090 n/a $0.42


The H100 line is the headline: **Modal ~$3.95/hr vs Beam H100 PCIE $1.74/hr, less than half.** Over a month of even moderate GPU use, that gap dwarfs any CPU-rate difference. At 100 H100-hours/month, that's Beam $174 vs Modal $395. Daytona's H100 sandbox GPU is also $3.95/hr, and E2B offers no GPU at any tier, since its Firecracker microVMs don't do GPU passthrough.


**Serverless: both platforms have it**


Beam, like Modal, offers per-second serverless compute that scales to zero. This isn't a Modal-only capability. Beam's serverless rates:


- **CPU:** $0.0000528 / core / second (~$0.19/core-hr)
- **RAM:** $0.0000056 / GB / second (~$0.0202/GB-hr)
- **RTX 4090:** $0.000192 / second (~$0.69/hr)
- **A10G:** $0.000292 / second (~$1.05/hr)


On the serverless, per-second axis, Modal's CPU is cheaper than Beam's: Modal's standard Function CPU runs $0.0000131/physical-core/s versus Beam's serverless $0.0000528/core/s. If your workload is CPU-only and genuinely spiky, Modal's per-second rate wins that narrow comparison. But two things flip it. First, that low CPU rate triples inside Modal Sandboxes ($0.00003942/core/s), so any agent or untrusted-code workload loses the advantage immediately. Second, Beam also offers **on-demand CPU from $0.04/core-hr all-in** , below Modal's effective ~$0.047/core-hr, which is the better fit for the steady CPU workloads where serverless per-second billing is the wrong model anyway. Across both axes, Beam's serverless GPU rates stay low and its on-demand GPUs run less than half Modal's.


Where Beam wins:


- GPUs at less than half the price, on the same gVisor runtime and the same API as CPU, with no second platform to bolt on.
- No subscription floor on the free Developer plan ($30/month in credits; Team plan $89/month vs Modal's $250).
- BYOC and self-host: run the identical sandbox and GPU API on your own AWS, GCP, Azure, or Hetzner credits, including reserved-instance and committed-use discounts you've already negotiated. The retail price becomes the most you'd pay, not the least.


## Pricing comparison


Modal Beam Daytona E2B


Subscription floor (production) None (Starter) / $250 Team None (free Developer) None $150/mo


Free credits $30/mo $30/mo $200 once $100 once


CPU $0.047/core-hr † $0.04/core-hr on-demand · $0.19/core-hr serverless $0.0504/vCPU-hr $0.0504/vCPU-hr


RAM $0.0080/GiB-hr $0.0202/GB-hr $0.0162/GiB-hr $0.0162/GiB-hr


H100 /hr ~$3.95 $1.74 $3.95 n/a


GPU in sandbox (incl. B200/H200) Yes Yes Yes (H100/RTX PRO 6000) No


Serverless / scale-to-zero Yes Yes Pause/archive Pause/resume


Self-host / BYOC No Yes (beta9, Helm) Yes Heavy (Terraform)


† Modal bills physical cores (~2 vCPU); $0.047/core-hr is ~$0.0236/vCPU-hr. Beam bills per core and offers two CPU models: on-demand from $0.04/core-hr all-in (below Modal's effective rate) and serverless per-second at $0.19/core-hr. CPU units differ by vendor, so compare carefully. All figures verified 2026-06-27.


## Developer experience: a fair comparison


Modal's developer experience is strong. Sandboxes are built on **gVisor** (a Google user-space kernel that intercepts syscalls for strong isolation), with **sub-second cold starts** claimed for cached custom images, a **default Sandbox lifetime of 5 minutes** (configurable up to 24 hours via` timeout` ), an **\`idle_timeout\`** parameter that auto-terminates idle Sandboxes, and **first-class \`modal.Secret\` objects** injected as environment variables. For runs longer than 24 hours, Modal recommends Filesystem Snapshots. Secure-by-default networking blocks inbound connections.


Beam matches the core of this. It offers gVisor isolation, sub-1-second cold starts on cached images, secrets management, scale-to-zero, custom images, and the same high-end GPUs (B200 and H200 included) inside the sandbox, while adding the deployment flexibility Modal structurally lacks. Beam also doesn't charge for cold-start or image-pull time, which matters most for the high-churn, many-spin-up pattern agents create.


One caveat in the other direction: gVisor (used by both Beam and Modal) is a thinner boundary than Firecracker microVMs, so if your threat model is hostile multi-tenant code, evaluate microVM options like E2B or CodeSandbox.


## Recommendations


- **GPU utilization low and spiky (under ~30%):** Modal's serverless model and scale-to-zero are a good fit, and the no-floor Starter plan is friendly. This is the workload serverless was built for. Start here and don't over-engineer.
- **Steady, high-utilization GPU workloads (above ~50–60%), training jobs, or anything around the clock:** you're overpaying on Modal and you can't fix it on Modal. Move to a model where retail is a ceiling: Beam managed (H100 at $1.74/hr vs ~$3.95/hr), or Beam self-hosted on committed compute you already own.
- **GPUs inside the sandbox plus existing cloud commitments:** Beam's BYOC path lets you apply reserved-instance and committed-use discounts to the underlying GPUs, which is impossible on Modal.
- **Pure spiky CPU with no GPU:** Modal's per-second rate ($0.0000131/physical-core/s) is competitive for genuinely bursty work; stay if that's your whole workload, but watch the 3x Sandbox CPU premium for agent or untrusted code, and note Beam's on-demand CPU ($0.04/core-hr all-in) is cheaper for anything steady.
- **Thresholds that should change your decision:** sustained GPU utilization crossing ~50%; any reserved or committed compute you already pay for; needing to keep data and compute in your own VPC; monthly GPU spend large enough that a 2x rate gap exceeds the cost of operating your own infra.


## FAQ


**How much does Modal cost per hour?** It depends entirely on the resource. Raw CPU is cheap, roughly $0.024/vCPU-hr at the base Function rate. GPUs are mid-to-high: an H100 is ~$3.95/hr effective, a B200 ~$6.25/hr, a T4 ~$0.59/hr. But region selection (1.5–1.75x), non-preemptible execution (3x), and the Sandbox tier (3x CPU) can multiply these substantially on a real invoice.


**Is Modal cheaper than Beam?** Only in one narrow case: serverless, per-second CPU for genuinely spiky workloads, where Modal's $0.0000131/physical-core/s beats Beam's serverless CPU rate. Everywhere else Beam is cheaper. Beam's on-demand CPU starts at $0.04/core-hr all-in (below Modal's effective ~$0.047/core-hr), its H100 is $1.74/hr vs Modal's ~$3.95/hr, and because Beam supports self-host and BYOC you can go lower still by running on compute you already own. Modal gives you no such option.


**Can I use my AWS, GCP, or Azure credits on Modal?** Not as compute. Enterprise customers can apply existing AWS or GCP committed-spend agreements to their Modal bill through the cloud marketplaces, but that's a billing arrangement: you still pay Modal's retail per-second rate, and you can't run Modal inside your own cloud account or apply reserved-instance hardware discounts to the GPUs. Azure isn't referenced at all.


**Does Modal have a subscription floor?** Not on Starter ($0 + compute, $30/month credits). The Team plan is $250/month plus compute. So unlike E2B's $150/month Pro floor, you can run production-grade spiky workloads on Modal without a fixed monthly fee. The cost pressure is on the per-second GPU rate, not the subscription.


**Does Modal support GPUs in sandboxes?** Yes. Modal puts the full lineup (T4 through B200, including H200) inside Sandboxes, billed at standard GPU rates while CPU and RAM bill at the 3x non-preemptible Sandbox tier. Beam also supports high-end GPUs including B200 and H200 in-sandbox, at lower rates.


**What's the cheapest way to run GPU sandboxes at scale?** Whichever you can run on committed or owned compute. Since Modal is managed-only, its retail rate is your floor. Beam's managed H100 is already less than half Modal's, and its self-host path lets you go lower still by applying reserved-instance discounts, making it the lowest-total-cost option for sustained GPU workloads.


## Run GPU sandboxes without the serverless tax


Modal's per-second CPU rate is competitive for genuinely spiky workloads. But the missing self-host path, the stacking multipliers, and GPU rates more than double Beam's add up fast at production scale, with no lever to bring them down. Beam runs the same gVisor sandbox API with the same high-end GPUs, prices H100s at $1.74/hour, offers on-demand CPU from $0.04/core-hr and per-second serverless that scales to zero, and lets you self-host the identical API on your own cloud credits when that's cheaper.


Get started free with $30/month in credits, no subscription floor, and GPUs at less than half the serverless rate. See[Beam pricing](https://www.beam.cloud/pricing) to compare against your workload.


*All Modal figures verified on modal.com/pricing and modal.com/docs on 2026-06-27; rates change. Per-hour GPU figures are computed from per-second rates (×3600) and rounded. Beam pricing reflects current on-demand and serverless rates. Utilization break-even thresholds (~30–61%) are estimates that vary by workload shape, so model your own traffic before deciding. Competitor rates fluctuate with availability.*


Nathanael Chiang


Published June 27, 2026


Keep Reading


## More from the Beam blog


[Engineering Tinker Model Pricing: What Fine-Tuning Costs in 2026 See what fine-tuning costs on Tinker, including worked cost examples and where renting GPUs gets cheaper. Tim Huynh](https://www.beam.cloud/blog/tinker-model-pricing)[Engineering What Is a Container, Really? Five Years of GPU Infrastructure Five years of GPU infrastructure at Beam — from ECS and Knative cold starts to a custom container runtime, FUSE lazy-loading, and a trustless binary. Luke Lombardi](https://www.beam.cloud/blog/what-is-a-container-really)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
