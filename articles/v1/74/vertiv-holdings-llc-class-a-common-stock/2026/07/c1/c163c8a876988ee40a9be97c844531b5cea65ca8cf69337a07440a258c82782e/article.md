---
schema_version: "1.0.0"
document_id: "c163c8a876988ee40a9be97c844531b5cea65ca8cf69337a07440a258c82782e"
company_key: "vertiv-holdings-llc-class-a-common-stock"
company: "Vertiv Holdings LLC"
source_id: "vertiv-holdings-llc-class-a-common-stock-news-import-f0997059a52b"
canonical_url: "https://www.vertiv.com/en-us/insights/articles/blog-posts/the-vertiv-view-high-voltage-dc-will-power-ai-expansion/"
published_at: null
first_seen_at: "2026-07-22T18:48:52.120325+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:94fd32b5a003c920fa026c7bcb98f705ac4c152f0d5f09accbeac66227c876c5"
---

# The Vertiv view: High-voltage DC will power AI expansion

#### The first in a new series, summarizing Vertiv's position on the latest market developments, looks at why AI is driving


the transition to 800 VDC infrastructure.


In electrical engineering, there's a concept called "conductor loss"—the energy that dissipates as heat when electricity flows through resistance. For decades, data centers have simply accepted it as the cost of doing business. But at AI scale, accepting that loss is like trying to fill a swimming pool through a garden hose. The infrastructure itself becomes the bottleneck.


Traditional AC power distribution is reaching its physical and practical limits when it comes to the extreme density of modern AI compute. As rack densities push toward and beyond the megawatt threshold, the physical limitations of massive copper conductors, their size, bend radius, and connector requirements, become untenable. The practical constraints of deploying this heavy, unwieldy copper, in terms of manpower and equipment, have become fundamental constraints on performance and scale.


NVIDIA is advancing a coordinated industry effort to redefine power distribution at the gigawatt-scale for the next generation of AI factories. Vertiv recently announced an expanded collaboration with NVIDIA to advance 800 VDC platform designs for these new systems. This collaboration reflects a broader industry inflection point: higher voltage DC architectures offer a fundamentally more efficient path forward for powering AI at scale.


### Opening the floodgates


The 800 VDC architecture represents a coordinated effort to redefine power distribution at megawatt-scale.


Traditionally, power flows from the grid as Medium-Voltage AC (1kV to 35kV) and is transformed to Low-Voltage AC (480V or 415V) for facility distribution. It's then converted again, often to 54 VDC, at the rack level. This series of conversion steps introduces resistance, but it is the heat loss from that resistance, in both the conversions and the conductors, that causes significant efficiency loss. Furthermore, these lower voltage levels demand higher current, which is what drives the massive copper requirements.


The 800 VDC architecture simplifies this path, skipping at least one conversion step by converting Medium-Voltage AC at the utility entrance directly to 800 VDC for distribution. One efficient conversion. One streamlined path. This results in significantly less loss, as the higher voltage and corresponding lower current substantially reduce heat losses from both conductors and conversions.


The performance improvements are substantial and quantifiable*:


- Up to 45% reduction in copper requirements
- Up to 5% improvement in overall efficiency
- As much as 30% lower total cost of ownership
- Reduction in resistance losses at densities of 100 kW to 1 MW per rack and beyond


** Source :[NVIDIA 800 VDC Architecture Will Power the Next Generation of AI Factories](https://developer.nvidia.com/blog/nvidia-800-v-hvdc-architecture-will-power-the-next-generation-of-ai-factories/)*


At these megawatt-scale rack densities, the physical constraints of large copper conductors, their sheer size, number, bend radius, and associated connectors, become the primary bottleneck. The principle of convection to dissipate heat simply runs out of steam. By increasing the voltage to 800 VDC, the current is drastically reduced. This allows for smaller copper conductors, which alleviates these physical and thermal constraints and reclaims valuable space for compute. This shift to 800 VDC architectures simultaneously addresses efficiency, capacity, and cost, the three dimensions that determine viability at AI scale.


### Engineering the complete electrical path


The transition to 800 VDC architectures requires rethinking established standards, safety protocols, and operational practices. For instance, 800 VDC systems require that only "qualified persons" work on or near the equipment, a significant change from practices around traditional compute racks. This necessitates developing new equipment, standards, and practices.


Vertiv is developing a complete, end-to-end 800 VDC ecosystem spanning converters, protection, distribution, monitoring, and lifecycle services. Every component must be designed to handle this power level safely and efficiently, from the point where power enters the facility to where it reaches the compute load.


Vertiv engineering teams have made robust progress and remain on track for readiness by late 2026, as detailed in the[AI infrastructure evolution announcement](https://www.vertiv.com/en-us/about/news-and-events/corporate-news/vertiv-accelerates-ai-infrastructure-evolution-in-alignment-with-nvidia-800-vdc-power-architecture-announcement/) . Collaboration with NVIDIA is advancing system validation on reference designs that integrate this 800 VDC distribution into[NVIDIA Vera Rubin Ultra Kyber platforms](https://www.vertiv.com/en-us/about/news-and-events/corporate-news/vertiv-develops-energy-efficient-cooling-and-power-reference-architecture-for-the-nvidia-gb300-nvl72/) . This work establishes system-level performance benchmarks and interoperability standards for next-generation AI infrastructure.


### Building the infrastructure to carry the flow


Preparation for 800 VDC extends beyond future systems. Vertiv is actively strengthening the technologies that bridge today's high-density AI workloads with tomorrow's DC-based architectures.


The move to higher voltage DC architectures represents a defining evolution in data center architecture. It requires deliberate, coordinated effort across the industry. The trajectory, however, is clear and accelerating.


800 VDC is more than a technical specification. It is the architectural shift that will define the next decade of digital capacity growth. The opportunity extends beyond powering AI to enabling an entirely new class of intelligent facilities designed from the ground up for the density, efficiency, and performance that AI workloads demand.


### From constraint to capacity


Every generation of computing has demanded a fundamental rethinking of the infrastructure that supports it. The garden hose has given way to a fire main.


The technologies, partnerships, and validation work underway today are shaping how the next generation of AI factories will be powered—efficiently, safely, and at the scale the industry requires.
