---
schema_version: "1.0.0"
document_id: "2b991d461c9c882a3ec8ee1b029165b13ab810e86104740f1da727684b100014"
company_key: "yc-shadeform"
company: "Shadeform"
source_id: "yc-shadeform-news-import-b903e9afa9f0"
canonical_url: "https://shadeform.com/resources/articles/benchmarking-gpus-for-md-simulations-speed-and-cost-insights-from-simatomic--shadeform"
published_at: "2025-08-08T12:00:00+00:00"
first_seen_at: "2026-07-24T00:27:10.014959+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:8436e10c0bb3a1b5ddf3c66bd95b55899d9c8c12cd595e3c72cffb66514dcde7"
---

# Benchmarking GPUs for MD Simulations: Speed and Cost Insights from SimAtomic & Shadeform

## Overview


Molecular dynamics (MD) simulations are notoriously computationally intensive, especially when run with explicit solvent. GPUs have become essential for accelerating MD engines like OpenMM, offering order-of-magnitude speedups over CPUs \[1\].


There are a number of cloud providers that make it easy to access the GPUs required for MD simulations, but their pricing and performance vary greatly.


In order to evaluate the most performant and cost-effective offerings, the team at[Simatomic](https://simatomic.com/) , an all-in-one molecular dynamics simulation platform, partnered with[Shadeform](https://shadeform.ai/) , a marketplace for accessing GPUs across 20+ cloud providers, to benchmark MD simulation workloads on a range of GPU offerings from clouds such as[Nebius](https://shadeform.ai/clouds/nebius) ,[Scaleway](https://shadeform.ai/clouds/scaleway) ,[Hyperstack](https://shadeform.ai/clouds/hyperstack) and[AWS](https://aws.com/) .


Our evaluation focused on two key metrics: **simulation speed** (ns/day) and **cost efficiency** (cost per 100 ns simulated).


One frequently overlooked bottleneck is disk I/O. Writing outputs (trajectories, checkpoints) too frequently can significantly throttle GPU performance, up to 4× slower, primarily due to the overhead of transferring data from GPU to CPU memory. We tested this explicitly and optimized the output interval to mitigate it.


To ensure consistent setup and reproducibility, we used UnoMD \[2\], an open source python package to simply run MD simulations built on OpenMM.


Here we present a high-level comparison of NVIDIA’s **A100** , **H100** , **H200** , **L40S** , **V100** , and **T4** GPUs for running MD, evaluating both raw performance and cost-effectiveness in a cloud context.


## Benchmark setup


We simulated the T4 Lysozyme (PDB ID: 4W52), a medium-sized biomolecular system in explicit water solvent, with (~43,861 atoms total). Simulations were run using UnoMD/OpenMM on a CUDA platform 12.2 with: 2 fs integration timestep, PME electrostatics & 100 ps simulation time in mixed precision.


To avoid I/O-related slowdowns, trajectory frames were saved every 1,000 or 10000 steps (2ps or 20ps), based on tests confirming this interval maintained high GPU utilization \[3\].


We ran UnoMD’s` quickrun` function using save intervals of 10, 100, 1,000 & 10,000:


` >> quickrun(protein_file="unomd/example/4W52.pdb",`
` md_save_interval=1000, nsteps=50000,`
` platform_name="CUDA")`


## Effects of Output Frequency on GPU utilization


As shown in Fig. 1, reducing the frequency of trajectory saving significantly improves GPU utilization and simulation throughput in OpenMM.


This is due to the overhead of transferring data from GPU to CPU during each save, which interrupts computation and leaves the GPU idle. Saving less often reduces these interruptions, allowing the GPU tocompute more efficiently.


This effect is especially pronounced in short runs (e.g., 100 ps), where saving events represent a larger fraction of the total runtime. In longer simulations, this overhead is amortized over more timesteps, making the impact less severe. Thus, minimizing save frequency is critical for maximizing performance especially in short OpenMM simulations.


Fig 1) Comparison of GPU utilization vs saving interval (ps) for 100 ps simulation of T4 Lysozyme.


## Insight


Our benchmarks show that **not all GPUs are equally cost-effective** , even if their peak performance is high.


As illustrated in Fig. 2, both the H200 and L40S GPUs reach over 500 ns/day, while T4 and V100 top out below 250 ns/day, despite maintaining high GPU utilization (≥90%).


However, Fig. 3 highlights a crucial point: raw speed alone does not equate to cost-efficiency.


When normalized to the AWS T4 baseline (~$17.52 per 100 ns), Nebius and Scaleway L40S offer the most economical performance at $7.07 and $7.21 per 100 ns, nearly a 60% reduction in cost. The H200 on Nebius also delivers excellent value at $15.26 per 100 ns, making it ~13% more cost-efficient than the AWS T4.


In contrast, the AWS V100, despite being over twice as fast as the T4, is the least cost-effective option at $30.99 per 100 ns, a 77% increase over the baseline.


These findings underscore that cost-effectiveness is shaped by both GPU architecture and cloud pricing, not just simulation speed. While the H100 & H200 excels in raw performance, it is optimized for machine learning and hybrid MD-AI workflows (e.g., using machine-learned force fields).


For traditional MD simulations, the L40S remains the most cost-efficient choice, offering top-tier performance at a fraction of the cost. Meanwhile, emerging platforms like Hyperstack or Scaleway, that we accessed through Shadeform, show promising price-performance on A100 and H100 as well.


Fig 2) Comparison of GPU utilization vs saving interval (ps) for 100 ps simulation of T4 Lysozyme.


Fig. 3) Cost per 100 ns for simulating T4 Lysozyme (~44K atoms) using OpenMM. T4 (g4dn.2xlarge, $0.752/hr, 200 GB storage), V100 (p3.2xlarge, $3.06/hr), H200 (Nebius, $3.53/hr, 200 GB storage), L40S (Nebius, $1.58/hr, 200 GB storage), H100 (Scaleway via Shadeform, $3.08/hr, 3.5 TB storage), A100 (Hyperstack via Shadeform, $1.35/hr, 800 GB Storage), and H100 (Hyperstack via Schadeform, $1.90/hr, 800 GB storage). Costs assume a 24-hour runtime and are normalized against T4. GPU utilization was measured at optimized saving intervals.


## GPU-Specific highlights


### H200 on Nebius – Best Performance


The H200 on Nebius achieved the highest performance at 555 ns/day and costs $15.26 per 100 ns, making it a top choice when time-to-solution is critical.


While not the cheapest, it’s still ~13% more cost-efficient than the AWS T4 and is uniquely suited for AI-enhanced workflows like machine-learned force fields.


### L40S on Nebius and Scaleway – Best Value Overall


The L40S reached 536 ns/day and delivered the lowest cost per 100 ns at just $7.07 (Nebius), and $7.07 (Scaleway), offering nearly H200-level speed at less than half the cost.


It provides the best balance of performance and affordability, making it ideal for most traditional MD workloads.


### T4 (AWS) – Budget Option for Long Queues


With 103 ns/day performance and a baseline cost of $17.52 per 100 ns, the T4 is the slowest GPU but the cheapest per hour. Though runtimes can be 4–5× longer than with high-end GPUs.


### V100 (AWS) – Solid Speed, Poor Value


The V100 performed well at 237 ns/day, but its cost of $30.99 per 100 ns makes it the least efficient option, ~77% more expensive than T4.


Given that both H200 and L40S outperform it in speed and price, the V100 offers limited value in today’s GPU landscape.


### A100 on Hyperstack – Budget-Friendly and Consistent


The A100 on Hyperstack, delivers 250 ns/day at $1.35/hr, translating to $12.96 per 100 ns.


While not the fastest, it’s more efficient than T4 and V100 and suitable for users seeking balanced speed and affordability.


### H100 on Scaleway – High-End Specs, Moderate Efficiency


Scaleway’s H100 runs at 450 ns/day and costs $16.43 per 100 ns. It includes 3.5 TB of storage and 80 GB VRAM.


Though not the cheapest, it offers decent value for heavy-duty workloads requiring large local storage or high memory bandwidth.


## Disclaimer


GPU compute credits were generously provided by Shadeform and Nebius to support this benchmarking study. However, all experiments were conducted independently using open-source tools, and the analysis and conclusions presented here reflect Simatomic's own findings without external influence.


## References


\[1\][https://blog.salad.com/openmm-gpu-benchmark](https://blog.salad.com/openmm-gpu-benchmark)


\[2\] UnoMD GitHub –[https://github.com/simatomic/unomd](https://github.com/simatomic/unomd)
