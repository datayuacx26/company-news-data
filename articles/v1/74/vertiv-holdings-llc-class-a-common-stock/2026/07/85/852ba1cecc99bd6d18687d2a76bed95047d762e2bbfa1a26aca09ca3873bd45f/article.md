---
schema_version: "1.0.0"
document_id: "852ba1cecc99bd6d18687d2a76bed95047d762e2bbfa1a26aca09ca3873bd45f"
company_key: "vertiv-holdings-llc-class-a-common-stock"
company: "Vertiv Holdings LLC"
source_id: "vertiv-holdings-llc-class-a-common-stock-news-import-f0997059a52b"
canonical_url: "https://www.vertiv.com/en-us/insights/articles/blog-posts/beyond-backup-understanding-the-complementary-roles-of-ups-and-bess/"
published_at: null
first_seen_at: "2026-07-22T18:48:52.120325+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:01927e1c9f0d2bf2c8cd2a13ec27f6968fdd997ca59c8d4a02d79325909414b7"
---

# Beyond backup: Understanding the complementary roles of UPS and BESS

Energy storage is becoming a more important part of large data center power architecture. Grid capacity constraints, emerging interconnection requirements, alternative energy variability, demand response opportunities, and increasingly dynamic AI workloads are all causing developers and operators to evaluate energy storage more seriously.


That has also created confusion.


Behind-the-meter battery energy storage systems (BTM BESS) and double-conversion online uninterruptible power supplies (UPSs) are often discussed together because both technologies use batteries and power conversion equipment. In some cases, their capabilities can overlap. Some UPS implementations can support limited grid-interactive functions, while some BESS implementations can be designed to provide UPS-like ride-through behavior.


But the central distinction is not the battery. It is the architecture.


A UPS and a conventional BTM BESS are typically connected, controlled, and sized differently, and optimized for different roles. Understanding those differences is becoming more important as data centers grow larger, AI workloads become more dynamic, and grid operators place greater emphasis on how large loads behave.


### Two architectures, two primary roles


A double-conversion online UPS is primarily a critical-load protection system. It typically sits in series with the protected load path, meaning power to the IT load passes through the UPS. That architecture allows the UPS to continuously regenerate output power, condition voltage and frequency, isolate sensitive equipment from many upstream disturbances, and provide no-break ride-through during source transfers or utility events.


In most data centers, the UPS is designed to bridge short-duration disturbances or source transitions until longer-duration backup resources are available. These include generators, fuel cells, BESS, or other onsite power sources. Its batteries are usually sized in minutes, not hours, because the primary objective is uninterrupted critical-load continuity.


A conventional BTM BESS serves a different primary role. It typically connects in parallel with the facility electrical system, often at the facility bus, service entrance, or another site-level interconnection point behind the meter. From there, it can charge or discharge in response to site-level objectives such as peak shaving, demand charge reduction, time-of-use (TOU) energy management, alternative energy utilization, demand response participation, grid services, or longer-duration reserve energy.


That parallel connection makes BESS a dispatchable site energy asset. It can improve flexibility, support broader energy management strategies, and help shape how the facility interacts with the grid. But in its conventional form, it is not the same thing as a double-conversion UPS providing continuous, no-break power conditioning at the protected load.


### Why the distinction matters now


Historically, data center power design focused on securing capacity, building redundancy, improving efficiency, and protecting critical loads. Those priorities remain essential. But large data centers are now being asked to do more.


Grid capacity is constrained in many regions. Interconnection studies are becoming more demanding. Some utilities and grid operators are asking large loads to demonstrate more flexible operating behavior. At the same time, onsite generation, alternative energy sources, energy storage, and microgrid strategies are becoming more common in data center planning.


These trends make it less useful to think about UPS and BESS simply as “battery systems.” The more useful question is: what role does each system play in the power architecture?


A UPS is optimized for continuous protected load and power conditioning. A conventional BTM BESS is optimized for site-level energy management and grid interaction. Both may be valuable, but they act at different points in the electrical system and on different time scales.


### AI power dynamics add another layer


AI workloads make this distinction more important. Large graphics processing unit (GPU) clusters can produce highly dynamic load profiles, with power rising and falling in synchronized patterns across training or inference phases. At a sufficient scale, those changes can affect the onsite electrical infrastructure and influence the facility’s interaction with the grid.


This does not mean one device can solve the entire problem. AI power smoothing is increasingly understood as a layered architecture challenge.


Some mitigation may occur close to the load, including software-driven controls, GPU-level ramp-rate management, temporary power limits, and rack-level energy storage or capacitance. At the critical power level, a double-conversion online UPS with appropriate controls can help buffer downstream load changes more quickly before they propagate upstream. At the site level, a BTM BESS can help manage broader facility ramp rates, energy dispatch, and grid-facing flexibility over longer durations.


The UPS and BESS roles are therefore complementary, not interchangeable. The UPS is better positioned for faster, protected-load-facing support because of its series connection to the critical load path. The BESS is better positioned for broader site-level energy management because of its parallel connection to the facility's electrical system.


### Designing coordinated power architectures


For data center developers, engineers, and operators, the implication is straightforward: storage decisions should begin with the role the system is expected to perform.


- Is the requirement no-break continuity for critical IT loads?
- Is it power conditioning?
- Is it a demand charge reduction?
- Is it alternative energy utilization?
- Is it grid services participation?
- Is it interconnection flexibility?
- Is it AI power smoothing?
- Is it a longer-duration site reserve energy?


Those questions determine whether the solution points toward UPS, BESS, load-level mitigation, site controls, or some coordinated combination of all of them.


The next generation of large data centers will require more than added capacity. They will require power architectures that are resilient, flexible, and grid-aware. That starts with understanding where UPS and BESS differ, where their capabilities can overlap, and how they can be designed to work together.
