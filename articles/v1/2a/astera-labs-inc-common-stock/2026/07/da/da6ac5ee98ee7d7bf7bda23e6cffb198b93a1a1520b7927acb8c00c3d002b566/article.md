---
schema_version: "1.0.0"
document_id: "da6ac5ee98ee7d7bf7bda23e6cffb198b93a1a1520b7927acb8c00c3d002b566"
company_key: "astera-labs-inc-common-stock"
company: "Astera Labs Inc."
source_id: "astera-labs-inc-common-stock-rss-6ea615f4bb0d"
canonical_url: "https://www.asteralabs.com/resources/blog/one-footprint-many-architectures-why-ai-racks-need-an-open-signal-conditioning-standard/"
published_at: "2026-07-29T01:18:18+00:00"
first_seen_at: "2026-07-29T01:36:38.656122+00:00"
fetched_at: "2026-07-29T01:36:40.631302+00:00"
content_hash: "sha256:9beb89e276e625895c9011ebd15fd7036f930bd0c3db5c2667ff3139b51da112"
---

# One Footprint, Many Architectures: Why AI Racks Need an Open Signal Conditioning Standard

Not long ago, a single AI training server housed eight accelerators, all connected within a single chassis. Today, that number has grown to 72 or more accelerators per rack — and industry roadmaps are pointing toward multi-rack and row-scale compute domains where hundreds of interconnected accelerators must exchange data with low latency and high aggregate bandwidth. The rack has become the unit of compute.


Feeding that much compute demands more lanes, higher signaling rates, and denser physical interconnects. Copper backplanes have become the preferred physical medium for these scale-up fabrics, offering twice the connection density of pluggable modules and the blind-mate serviceability that hyperscalers depend on for rapid deployment.


But as bandwidth requirements rise, the physics of the channel become less forgiving. Higher data rates amplify insertion loss across PCB traces, connectors, cable assemblies, and vias. PAM4 signaling improves spectral efficiency but tightens tolerance to return loss, crosstalk, and accumulated jitter. At some point, passive copper alone cannot close these channels.


That point has already arrived for many of today’s AI rack designs. Signal conditioners —[retimers and redrivers](https://www.asteralabs.com/products/taurus-smart-signal-conditioners/) that extend link reach and close loss budgets — have been used in high-speed serial link designs for years, but not in the volumes we’re seeing them deployed today. Signal conditioners are now a routine, structural element deployed across nearly every high-speed link in the system.


But if signal conditioners are now a standard ingredient in AI rack design, why does every vendor still require its own unique integration surface?


## The Hidden Cost of Proprietary Footprints


Signal conditioners have become load-bearing elements of AI rack design. But their physical integration has remained fragmented. Every silicon vendor brings its own package body size, its own pinout, its own breakout geometry, and its own connector alignment requirements. Changing suppliers does not mean swapping a line item on a bill of materials. It means a new package, a new pinout, a new breakout, a new power map — and, in practice, an entirely new board program.


This mismatch is particularly damaging at AI scale, where the engineering overhead of parallel board designs, the yield risk of non-standard fabrication, and the logistics of qualifying multiple-supplier alternatives all compound across millions of signal conditioner placements per deployment cycle


Three specific challenges make proprietary footprints especially costly in today’s AI infrastructure environment.


### Thick Boards Break Fine-Pitch Packages


AI system boards are among the most demanding PCB designs in production. Backplane switch and compute platforms routinely use board thicknesses of 200 mils or more to accommodate the high layer counts required for routing thousands of differential pairs alongside power delivery planes. That thickness imposes hard constraints on via technology: thicker boards require larger via drills to maintain manufacturable aspect ratios, which demand larger via pads and wider via-to-via spacing.


Many existing signal conditioner packages were designed for thinner boards with tighter BGA pitches. When a fine-pitch BGA designed for a thin add-in card is placed on a 200-mil stackup, the via pads can grow large enough to block adjacent routing channels — forcing additional escape layers, larger fan-out areas, or both. System designers are left choosing between compromising board thickness (sacrificing layer count and routing capacity), adopting non-standard fabrication processes (increasing cost and yield risk), or accepting suboptimal device placement that introduces additional insertion loss.


### Non-Standard Pinouts Multiply Design Effort


High-speed differential pairs, power rails, management interfaces, reference clocks, and AC-coupling requirements are arranged differently from one IC vendor to the next. The high-speed signal breakout — the critical transition from BGA pad through via to inner-layer routing channel — must be redesigned from scratch for each supplier. Backplane connectors from major vendors use column pitches in the 2.4–2.8 mm range; when a proprietary package breaks that alignment, the penalty shows up immediately in board area, layer count, routing congestion, and insertion loss.


AC-coupling requirements vary by protocol as well. Some protocols AC-couple on the transmit side while others, such as Ethernet and UALink, AC-couple on the receive side. Without a common framework, system teams must solve these integration challenges independently for every vendor and protocol on every board revision.


### The Compounding Cost at Scale


AI infrastructure operates at a scale where every suboptimal design decision is multiplied across millions of placements. The result is a structural bottleneck: Supply chain resilience depends on multi-vendor optionality, but multi-vendor optionality without a standard footprint requires parallel board programs that no team can afford to maintain at the pace AI infrastructure demands.


## The OCP Signal Conditioner Standard Footprint


The OCP Signal Conditioner Standard Footprint Base Specification (v1.0) addresses these challenges by standardizing the physical integration surface between the signal conditioner and the system board.


This open footprint standard is:


- **Protocol-agnostic.** It supports high-speed serial applications across Ethernet, UALink™, and proprietary AI fabrics at rates up to 224 Gbps PAM4.
- **Designed for the thick boards that AI systems require.** The specification characterizes two high-speed signal breakout strategies across three stackup thicknesses representative of AI system boards.
- **Built specifically to enable multi-vendor optionality.** The standard makes switching or adding vendors into a bill-of-materials (BOM) substitution rather than a board-level redesign.


The specification defines three footprint variants, each optimized for a distinct connector geometry:


**Variant** **Body Size** **SerDes Lanes** **Connector Fit**


x8 22 × 15 mm 16 (8 Tx + 8 Rx/side) 8-column


x16 22 × 22 mm 32 (16 Tx + 16 Rx/side) 8- or 16-column


x24 33 × 22 mm 48 (24 Tx + 24 Rx/side) 12-column


The defining architectural feature of the footprint is its flow-through routing channel design. Routing channels run through the pin field using standard high-volume manufacturing (HVM) trace geometries, carrying differential pairs cleanly through the device footprint without encountering blocking vias. This enables “tiling,” or the placing multiple devices side by side so that signals from one device route underneath an adjacent device without obstruction. Two tiled x16 devices yield 64 differential pairs in a compact routing width, with the 2.7 mm high-speed column pitch aligning directly to common backplane connector geometries.


## Application Use Cases for the Signal Conditioner Standard Footprint


The standard footprint is designed as a general solution rather than a single-application standard. Its protocol-agnostic design, scalable body sizes, and HVM-compatible geometry make it applicable across the full range of copper-based interconnect architectures in AI infrastructure.


### Scale-Up Copper Backplane


In scale-up backplane deployments, signal conditioners sit at the boundary between ASIC-side routing and the backplane connector field, where board area is at a premium. The x8, x16, and x24 variants are optimized for the connector geometries that dominate this environment, and the tiling capability enables placement directly in the connector shadow with minimal fan-out area.


### Near-Package Copper


In near-package copper architectures, signal conditioners are placed close to the accelerator, switch, or bridge device, conditioning signals over a short PCB reach before they transition to a longer board-level channel. The footprint’s compact body sizes make it a viable physical candidate wherever a signal conditioner needs to live close to the source silicon, reinforcing its value as a placement-agnostic standard.


### Midplane Copper Architecture


Midplane topologies have been gaining traction as AI rack designs move toward fully modular, cable-free compute trays. The standard footprint’s connector-aligned 2.73 mm horizontal pitch is already optimized for the high-density connector families used in these designs.


Midplane connectors often sit near the electrical and physical middle of a channel, an ideal location for symmetric equalization of both trace segments. Recent rack-scale systems (like NVIDIA’s Vera Rubin NVL72) which have adopted PCB midplanes to connect accelerator modules to front-bay networking components, and this trend should intensify as architectures scale toward higher GPU counts per rack (NVIDIA’s Kyber will target of 144 GPU packages per rack, for instance).


Across all of these topologies, the specification’s flexible internal AC-coupling framework means that a board design proven in one protocol generation can carry forward to the next without a respin of the signal conditioner sites.


## A Footprint Standard for the Scale-Up AI Moment


As rack architectures evolve toward wider form factors, denser midplanes, longer backplane reaches, and higher data rates, the case for a standard physical integration surface only strengthens. The OCP Signal Conditioner Standard Footprint provides a practical foundation for multi-vendor AI infrastructure: Design once, qualify multiple suppliers, and carry forward proven layouts as protocols and architectures continue to evolve.


To go deeper on the signal integrity analysis, board manufacturing constraints, and application-specific routing examples covered here,[download the full white paper](https://www.asteralabs.com/document-requests/?document=Scaling%20the%20Rack%3A%20Why%20AI%20Needs%20an%20Open%20Standard%20Footprint%20for%20Signal%20Conditioners) .


*Astera Labs is a member of the Open Compute Project and a contributor to the OCP Signal Conditioner Standard Footprint Base Specification.*
