---
schema_version: "1.0.0"
document_id: "cd5d623897d619c1f07e7a4af9a03e8b0d206037b724fbbe832f511624101405"
company_key: "rambus-inc-common-stock"
company: "Rambus Inc."
source_id: "rambus-inc-common-stock-news-import-b51b587c8dec"
canonical_url: "https://www.rambus.com/blogs/rambus-enables-socamm2-memory-modules-for-ai-servers/"
published_at: "2026-04-22T21:01:36+00:00"
first_seen_at: "2026-07-25T20:32:34.030231+00:00"
fetched_at: "2026-07-28T22:15:40.956663+00:00"
content_hash: "sha256:00552f21eee8e0412dc6e45241bc87abc380284bb252cc970663493944660960"
---

# Rambus Enables SOCAMM2 Memory Modules for AI Servers

As AI workloads reshape modern data centers, memory is one of the most critical factors for performance, efficiency, and total cost of ownership. While CPUs and accelerators continue to scale rapidly, AI server architectures are increasingly power‑limited and thermally constrained.


This has driven growing interest in Low Power DDR (LPDDR) as a server memory technology. LPDDR offers compelling power efficiency advantages, but its integration into data centers is far from straightforward. LPDDR was originally designed for battery-operated mobile devices, not racks of AI servers running 24/7. Bridging that gap required a fundamentally new architectural approach, and that is exactly why **SOCAMM2** was created.


SOCAMM2, or **Small Outline Compression Attached Memory Module** 2, is a JEDEC‑standard LPDDR server module purpose‑built for AI infrastructure. And enabling SOCAMM2 requires a new memory module chipset to unlock the benefits of LPDDR memory for AI servers.


## **Why LPDDR Needs a New Architecture in AI Servers**


LPDDR delivers excellent power efficiency by operating at lower voltages and high per‑pin data rates. However, those same characteristics introduce serious challenges when LPDDR is removed from its original application context.


In mobile phones, LPDDR devices are placed extremely close to the processor, often soldered directly to the motherboard or packaged adjacent to the application processor. This minimizes trace length and preserves signal integrity. Simply placing LPDDR devices onto traditional, slotted server modules would dramatically increase routing distances, reducing signal margin and introducing unacceptable levels of noise, crosstalk, inter‑symbol interference, and reflections.


At LPDDR5X speeds, which can reach 9.6 Gb/s per pin, even small impairments in routing or impedance control can lead to unacceptable signal performance. Standard server PCB layouts, optimized for DDR signaling, are not well‑suited to LPDDR’s interface running at these speeds.


Power delivery further complicates matters. LPDDR’s sub‑1‑volt power rails leave very little margin for voltage variation. In large server platforms, delivering clean, stable power across long board traces becomes increasingly difficult. Without localized regulation and monitoring, LPDDR’s efficiency advantages are quickly negated.


The challenge, then, was how to bring LPDDR’s benefits to AI servers while respecting its physical and electrical constraints.


## **SOCAMM2: Harnessing LPDDR Benefits While Enabling Server Modularity**


SOCAMM2 introduces an ultra‑compact, low‑profile module that can be mounted proximate to the processor, preserving short signal paths and maintaining the signal margins required at LPDDR5X data rates.


The “compression attach” technology means the back side of the SOCAMM2 module is the contact area for the module, providing the signal integrity needed. It also means the module can lie flush to the motherboard facilitating liquid cooling.


Importantly, SOCAMM2 provides the modularity and serviceability that data centers demand. Unlike soldered-down LPDDR solutions, SOCAMM2 is a removable module. Systems can be upgraded, repaired, or reconfigured without replacing the entire mainboard, dramatically improving lifecycle flexibility and reducing downtime.


Power delivery is localized on the module itself. SOCAMM2 platforms incorporate on‑module power management and monitoring, allowing tight control of LPDDR’s low‑voltages while supporting enterprise‑class telemetry, identification, and configuration. This architecture brings LPDDR power integrity under direct control, rather than relying on long, lossy motherboard power paths.


Custom substrates and interconnects further ensure that routing is optimized for LPDDR signaling. The result is a memory module that aligns with LPDDR’s electrical requirements while supporting the requirements of data center server environments.


## **The Rambus SOCAMM2 Server Memory Module Chipset**


The Rambus SOCAMM2 chipset is designed to support this innovative new module form factor by providing the **critical control, telemetry, and power delivery functions** required by JEDEC-standard SOCAMM2 memory modules in demanding AI server environments.


The chipset supports reliable, power-efficient operation of LPDDR5X-based server memory modules at up to 9.6 Gb/s and includes:


- **SPD Hub** for **module identification, configuration, and telemetry** ;
- **12A and** **3** **A** **Voltage Regulators** for **localized, efficient power conversion** .


SOCAMM2 Showing Rambus Server Chipset ICs
