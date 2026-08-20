---
schema_version: "1.0.0"
document_id: "966f018cc50a827af5789ac064ddf9ef68b2d8ea95aa35ab0748f6dd8f8caa48"
company_key: "vertiv-holdings-llc-class-a-common-stock"
company: "Vertiv Holdings LLC"
source_id: "vertiv-holdings-llc-class-a-common-stock-news-import-f0997059a52b"
canonical_url: "https://www.vertiv.com/en-us/insights/articles/educational-articles/vertiv-cooling-innovation-day-2026-end-to-end-system-design-for-high-density-ai/"
published_at: null
first_seen_at: "2026-07-22T18:48:52.120325+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:15cfbc689500e41ce9837eaad327ec0983bc6ac89c8362fd8940f0da6c477c98"
---

# Vertiv Cooling Innovation Day 2026: End-to-end system design for high-density AI

AI workloads are raising rack densities, operating temperatures, and system complexity faster than legacy infrastructure was designed to handle. In the first episode of the[Vertiv Cooling Innovation Day 2026](https://www.vertiv.com/en-us/about/news-and-events/events/2026/vertiv-cooling-innovation-day-2026/) , Vertiv's[George Hannah](https://www.datacenterdynamics.com/en/profile/george-hannah/) , VP of Thermal Systems and Advanced Development,[Tyler Voigt](https://www.datacenterdynamics.com/en/profile/tyler-voigt/) , Global Director of Thermal Controls, and[Roberto Felisi](https://www.datacenterdynamics.com/en/profile/roberto-felisi/) , Global Business Development Leader for Thermal Systems and Strategic Partnerships, join[James Raddings](https://www.datacenterdynamics.com/en/profile/james-raddings/) of DatacenterDynamics to discuss densification, the[bring your own power (BYOP)](https://www.vertiv.com/en-us/solutions/dynamic-power/) shift, and what end-to-end system design means at AI scale.


#### James Raddings, Digital Portfolio Lead, DatacenterDynamics (DCD): AI is driving higher density, higher temperatures, and greater system complexity. What has fundamentally changed on the ground?


##### George Hannah, VP, Thermal Systems and Advanced Development, Vertiv:


Liquid cooling has changed fundamentally with GPU and silicon densification. Power availability in national grids has become a real constraint — BYOP is becoming a thing. Having end-to-end solutions that work with the grid and onsite power in a complementary way has never been more important.


##### Roberto Felisi, Global Business Development Leader, Thermal Systems and Strategic Partnerships, Vertiv:


Density has changed dramatically. We have also seen an increase in temperatures. If we consider safety and the approach of the[coolant distribution units (CDUs)](https://www.vertiv.com/en-us/insights/articles/educational-articles/understanding-coolant-distribution-units-cdus-for-liquid-cooling/) , we can get to a temperature that tells us a good mix between mechanical cooling and electrical cooling is still the right compromise. Higher densification and water temperatures define the new design envelope, and no single technology serves every application.


##### Tyler Voigt, Global Director, Thermal Controls, Vertiv:


I’ve seen increasing system complexity with the types of technology used in design, deployment, and operations. What’s interesting is that we support the AI industry, but we’re also actually using AI to speed up time-to-market and deployment.


#### James: We’re seeing a shift from siloed systems to integrated system design. How has that changed the relationship between power, cooling, and compute?


##### George:


Power and cooling are now more interlocked than ever. With AI, the power side is pulling through a very dynamic load profile, which then pulses through to the thermal side. What happens on power eventually happens a few seconds or minutes later on the thermal side. Having these two systems working in close coordination, with Tyler’s team on the controls, is essential.


##### Roberto:


We see a complete integration running[from the chip through to IT heat rejection and heat reuse](https://www.vertiv.com/en-us/solutions/dynamic-power/) . The only way to optimize system performance across the full chain is to coordinate the different technologies. That coordination is what delivers tighter efficiency and operational reliability.


##### Tyler:


Historically,[IT and OT systems](https://www.vertiv.com/en-us/insights/articles/educational-articles/itot-convergence-solving-data-center-complexity-through-unified-intelligence/) operated in separate layers, each with their own embedded controls. As densities grow, the timing of responses between those layers becomes more sensitive. Controls and software play a key role in keeping them synchronized.


#### James: What does the thermal chain look like from chip to heat rejection — where are the biggest risks if it isn’t properly aligned?


##### Roberto:


The thermal chain is a single continuous system flowing from the GPU through the CDU, across the cooling infrastructure, through to heat rejection and heat reuse. The controller plays a central role — not just the hardware, but the software that manages startup, commissioning, service, and maintenance. The server depends on temperature control and flow control — put those at risk and it’s not just the cooling at stake, but the entire infrastructure investment of the data center.


##### Tyler:


Our definition of the thermal chain is managing the entire system from a single ingestion point — firmware updates deployed from cloud all the way down to a connected sensor at the bottom of the stack. The closer we can get data to the actual source, the better we can respond preemptively through whichever layer of the chain needs it.


##### George:


Thermal inertia on the liquid side is very rapid — far more so than with air cooling. If the end-to-end control system isn’t precise, and the system isn’t designed with the right buffering in place, you risk thermally and physically stressing the electronics at the end of the chain. We’ve been working for years on having a stable end-to-end thermal chain and power train system


#### James: Grid power is constrained and BYOP is growing. How is that reshaping system architecture and operations?


##### George:


Grid-sourced power is running out, particularly in the US, and that’s driving movement toward BYOP. The architecture of both the power system and the thermal system is fundamentally different on BYOP compared to grid — different power and thermal inertias call for different technical solutions across chilling, pumping, and power. Managing a BYOP site operationally is at least an order of magnitude more complex than a grid-sourced one.


##### Roberto:


Power availability is becoming a primary driver of system design. With BYOP and onsite power generation, we have the opportunity to convert what was previously waste into a resource: through an absorption chiller, cooling can be driven directly from the recovery of power generation, improving the overall efficiency of the system and architecture. We are developing hybrid solutions that combine grid connection, power generation, and different technologies to achieve the highest efficiency, heat reuse, and reliability.


##### Tyler:


Controls are the glue between these systems, and software and controls are multi-pronged across the data center lifecycle — design, deployment, commissioning, operations, and support. On deployment, controls cover auto-discovery, self-describing devices, and preemptive scripts testable against[digital twins](https://www.vertiv.com/en-us/insights/articles/blog-posts/what-digital-twins-reveal-about-ai-infrastructure-design/) . In operations, the focus is energy optimization — visibility and management of the full thermal chain.


#### James: How should we think about digital twins in practice, and what role are they playing across the system lifecycle?


##### Tyler:


There are roughly four levels of digital twin maturity: visual representation, steady-state capacity verification, time-domain dynamic prediction, and full system integration where[digital twins](https://www.vertiv.com/en-us/insights/articles/blog-posts/digital-twins-for-operations-intelligent-management/) communicate in real time. We’ve virtualized our unit controllers and run algorithms against them to determine the right defaults before equipment ships. On the operational side, running predictive loading scenarios before those loads have been deployed is becoming important as systems grow denser.


##### George:


Digital twins help customers understand what a particular architecture can do, how it will function in failure scenarios, and how it can grow in the future — including the impact of expansion on uptime, tier rating, and redundancy strategy.


##### Roberto:


A real digital twin has a dynamic model that reflects how the system operates under different ambient conditions and load profiles, and can simulate scenarios before they occur. That predictive capability plays a fundamental role in controlling the ultimate system.


#### James: Bringing this all together — how would you define end-to-end system design, and what does it mean in practice?


##### George:


It’s thinking about the system as a product in itself: a coherent connection of components that work together from end to end. For years our industry treated components as individual entities. Now, we approach them as a continuum, a single product. Design teams, application engineering, laboratories, and tooling all orient toward that single outcome.


##### Tyler:


It’s about abstracting complexity to the point that deploying AI workloads becomes as straightforward as plugging a component into a larger system. Thermal, power, and IT-based control systems are integrated into a single interface, with APIs, data connectivity, and a single point of control. That’s what scales.


##### Roberto:


System design means compute, power, and cooling are fully integrated — not just at the hardware level, but across the full lifecycle. Products, services, and controls come together as a single value proposition.


### Watch the full conversation:[End-to-end system design for high-density AI](https://www.vertiv.com/en-us/about/news-and-events/events/2026/vertiv-cooling-innovation-day-2026/)
