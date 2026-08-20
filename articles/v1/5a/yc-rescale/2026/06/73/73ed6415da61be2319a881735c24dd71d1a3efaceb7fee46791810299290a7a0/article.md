---
schema_version: "1.0.0"
document_id: "73ed6415da61be2319a881735c24dd71d1a3efaceb7fee46791810299290a7a0"
company_key: "yc-rescale"
company: "Rescale"
source_id: "yc-rescale-rss-a01d3810de6f"
canonical_url: "https://rescale.com/blog/powering-next-generation-rand-amazon-ec2-hpc8a-2/"
published_at: "2026-06-08T17:42:50+00:00"
first_seen_at: "2026-07-20T23:20:48.219568+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:6535ba21202a633b51e38e38d96ba4d3ddf03431d3f292e8b605f170390ff970"
---

# Powering the Next Generation of R&D: Amazon EC2 Hpc8a Instances Are Generally Available on Rescale

[Back to Blog](https://rescale.com/blog)


[Partnerships](https://rescale.com/blog/category/partnerships/)


## Powering the Next Generation of R&D: Amazon EC2 Hpc8a Instances Are Generally Available on Rescale


Kevin Cangemi


June 8, 2026


Engineering teams are running more simulation iterations than ever, at higher fidelity, and increasingly with AI in the loop helping generate and evaluate design variants. That puts new pressure on the compute underneath. As iteration volume rises, the definition of “fast enough” is changing. Success depends less on how quickly any single solve completes and more on how many high-fidelity iterations your infrastructure can sustain before an idea goes stale.


Keeping pace with that demand takes compute built for it. At Rescale, our ecosystem strategy is built on a simple premise: enabling engineers and scientists to access the most appropriate architectures for their specific workloads. A critical component of that strategy is our deep collaboration with Amazon Web Services (AWS), ensuring that as soon as HPC silicon innovation happens, it’s available to our customers to drive their R&D forward.


Today, the[Amazon EC2 Hpc8a](https://aws.amazon.com/blogs/aws/amazon-ec2-hpc8a-instances-powered-by-5th-gen-amd-epyc-processors-are-now-available/) instance is now generally available on the Rescale platform. You can find the full node variant under the Rescale coretype name, “Hazel.” Powered by[5th Gen AMD EPYC™](https://www.amd.com/en/products/processors/server/epyc/9005-series.html) processors, Hpc8a is built for exactly this moment, delivering the throughput and scaling efficiency that AI-driven, multi-physics workflows demand, so your teams can run more iterations, explore more of the design space, and reach answers faster.


## Performance Across Real Simulation Workloads


Contents


- 1 Performance Across Real Simulation Workloads
- 2 Architectural Efficiency: Built to Scale
- 3 Accelerating Innovation Across Industries
- 4 Evaluate Hpc8a on Your Workloads
- 5 Author


For R&D leaders, infrastructure is only as good as the time-to-market advantage it provides. Our internal performance comparisons of Hpc8a against the previous generation (Hpc7a) show remarkable versatility and speedups across Rescale’s most diverse customer workloads.


Computational Fluid Dynamics (CFD): Fluid dynamics workloads, such as those running on Siemens STAR-CCM+ or Ansys Fluent, are notoriously resource-intensive. In our internal testing, Hpc8a delivered an average speedup of 52% over Hpc7a on CFD applications, with strong, near-linear scaling as core counts increased. On a large production-scale STAR-CCM+ model, we measured roughly a 57% improvement, and on a high-fidelity Ansys Fluent case, roughly 48%, meaning teams can run higher-fidelity models and more iterations without the diminishing returns common to older hardware generations.


Finite Element Analysis (FEA): Structural analysis often creates different bottlenecks than flow solvers. For memory-intensive FEA applications such as Ansys Mechanical, Hpc8a delivered approximately a 38% speedup over Hpc7a in our testing, demonstrated on a large structural analysis workload. This lets structural engineering teams process complex jobs significantly faster than before.


> “Hpc8a is a meaningful generational leap for our customers, real performance gains on the workloads they run every day, with no change to how they work. That blend of speed and seamless access is exactly what helps engineering teams take on more ambitious simulations and get to answers faster.” — Adam McKenzie, Chief Technology Officer, Rescale


## Architectural Efficiency: Built to Scale


While raw processor speed is important, the true differentiator for High Performance Computing (HPC) often lies in how the system handles data movement. Our technical observations indicate that Hpc8a benefits from microarchitectural improvements, particularly in memory bandwidth and network latency. Why does this matter to an R&D executive?


- Reduced Bottlenecks: Enhanced memory bandwidth ensures that the powerful 5th Gen AMD EPYC cores are not left waiting for data, maximizing the value of every compute hour.
- Better Scaling: Improved network latency is the key driver behind the near-linear scaling we observed in CFD applications. It enables massive parallelization, allowing you to solve problems that were previously too large or time-consuming to be practical.


## Accelerating Innovation Across Industries


The versatility of the Hpc8a instance makes it a powerhouse for a wide range of simulation-driven industries. From automotive and aerospace to semiconductor design and life sciences, this new instance type helps customers solve increasingly complex physics problems.


Most importantly, Rescale allows for a seamless transition. Because our platform is built for agility, your engineering teams can instantly move workloads from the current generation of hardware to Hpc8a. There is no need for complex reconfiguration. Your teams just get immediate access to the performance gains that maximize innovation.


## Evaluate Hpc8a on Your Workloads


Hpc8a is now available on the Rescale platform, allowing engineering teams to benchmark performance, validate scaling behavior, and compare results against existing infrastructure using their own production workloads.


You can get started directly in Rescale or request a free trial[here](https://rescale.com/aws-and-rescale-sign-up/) .


## Author


-


[Kevin Cangemi](https://rescale.com/author/kevin-cangemi/)


Kevin Cangemi is Head of Strategic Alliances and Ecosystem at Rescale, where he leads the alliances and business development organization and sets ecosystem strategy across cloud, software, and semiconductor partners. He is focused on accelerating the adoption of AI-native engineering, simulation, and high-performance computing across simulation-driven industries.


[View all posts](https://rescale.com/author/kevin-cangemi/)
