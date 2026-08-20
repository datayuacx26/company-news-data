---
schema_version: "1.0.0"
document_id: "a191c4bc7b737eccf893766a47bb9ae982917c86ea3b5e86e6feea652d27c631"
company_key: "vertiv-holdings-llc-class-a-common-stock"
company: "Vertiv Holdings LLC"
source_id: "vertiv-holdings-llc-class-a-common-stock-news-import-f0997059a52b"
canonical_url: "https://www.vertiv.com/en-us/insights/articles/promotional-articles/from-legacy-to-global-standard-the-415-v-power-shift-data-centers-need/"
published_at: null
first_seen_at: "2026-08-13T04:02:26.871985+00:00"
fetched_at: "2026-08-13T04:02:28.763826+00:00"
content_hash: "sha256:d53193781cdf6080c63460906e26fdbdd44b352560113002c2140c877709d41a"
---

# From legacy to global standard: The 415 V power shift data centers need

It helps to look at how power distribution has traditionally worked in North American data centers. The long-standing standard has been a 480 V three-wire distribution system. In this setup, power is stepped down from 480 V to 208/120 V using a transformer. The resulting 208 V supply is then delivered to IT racks. While this approach has served the industry for years, it comes with a significant drawback: it requires multiple conversion stages and additional hardware — including power distribution units (PDUs) and step-down transformers — to make the power compatible with modern IT equipment.


The 415 V four-wire system takes a fundamentally different approach. The latest white paper,[“Four-wire distribution: Modernizing data center power architecture to global standards,”](https://www.vertiv.com/en-us/insights/articles/white-papers/415-v-four-wire-distribution-the-smarter-power-standard-for-modern-data-centers/) breaks down the technical details and advantages. In this star-connected configuration, a neutral conductor is carried from the medium-voltage/low-voltage (MV/LV) transformer through the switchgear and uninterruptible power supply (UPS), enabling the direct delivery of 240 V phase-to-neutral power to the rack, the voltage that modern IT equipment is optimized for.


This is a power architecture that has been the proven standard across Europe and Asia for decades because the system already provides the required neutral, downstream step-down transformers are no longer needed. This simplifies the entire distribution path and delivers a range of meaningful advantages:


- Higher efficiency:


Fewer conversion stages can reduce distribution losses and contribute to improvements in facility power usage effectiveness (PUE).
- Higher rack power density:


The higher distribution voltage allows more power to be delivered per rack, supporting the demands of HPC and AI workloads.
- More usable floor space:


Eliminating downstream transformers frees up electrical room space and increases the white space available for IT equipment.
- Global consistency:


Aligning North American facilities with the 415/240 V architecture already used across Europe and Asia simplifies design, procurement, and operations for global operators.
- Integrated protection and grounding:


Protection and grounding are now handled at the UPS and switchgear level, resulting in a leaner and safer overall design.


### Engineering the 415 V four-wire UPS


In a 415 V four-wire system, the neutral conductor must be managed directly inside the UPS. This has significant implications for both the power stage and the control architecture. The inverter and output filters must be capable of handling unbalanced phase loading and neutral current without relying on galvanic isolation.


The control firmware must actively monitor and regulate each phase independently to maintain voltage symmetry under single-phase load conditions. Protection coordination must also be redesigned so that the UPS responds correctly to phase-to-neutral and ground faults. And critically, the entire design must comply with UL standards — a non-negotiable requirement for North American deployments.


While some may view 415 V four-wire distribution in North America as an emerging trend, the reality is that it is already being deployed at scale. Vertiv has been among the first to support this transition with UL-certified systems, including the[Vertiv™ PowerUPS 9000](https://www.vertiv.com/en-us/products-catalog/critical-power/uninterruptible-power-supplies-ups/vertiv-powerups-9000/) and the[Vertiv™ Trinergy™ UPS](https://www.vertiv.com/en-us/products-catalog/critical-power/uninterruptible-power-supplies-ups/vertiv-trinergy/) , both purpose-built to operate at this voltage rating.


These systems are already running in large-scale facilities, demonstrating that 415/240 V operation is a proven and reliable approach. By securing UL certification early and deploying at scale, Vertiv gives data center operators the confidence to adopt this globally consistent architecture without delay.


### Looking ahead


The move to 415 V four-wire distribution is part of a broader evolution in data center power architecture. Other approaches — including 480 V four-wire systems delivering 277 V phase-to-neutral, and medium-voltage DC architectures — are also being explored as the industry continues to push the boundaries of efficiency and density.


But for North American operators looking to simplify their infrastructure, increase rack power density, and align with global standards today, the case for 415 V four-wire distribution is already compelling — and the technology to support it is already here. For AI data center decision-makers looking to find the full technical details and insights for this transition,[download the white paper](https://www.vertiv.com/en-us/insights/articles/white-papers/415-v-four-wire-distribution-the-smarter-power-standard-for-modern-data-centers/) .
