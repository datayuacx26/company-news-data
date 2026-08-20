---
schema_version: "1.0.0"
document_id: "3553c7522213d7585be394f4a08cee6567569145f3d7ce574b030a535d05259c"
company_key: "vertiv-holdings-llc-class-a-common-stock"
company: "Vertiv Holdings LLC"
source_id: "vertiv-holdings-llc-class-a-common-stock-news-import-f0997059a52b"
canonical_url: "https://www.vertiv.com/en-us/insights/articles/blog-posts/from-rack-to-data-hall-the-practical-path-to-800-vdc/"
published_at: null
first_seen_at: "2026-07-24T06:56:37.102534+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:098fa20029021e79e626d15e3f6620bede91056de5af9f3c4fee0f34645bab8f"
---

# From rack to data hall: The practical path to 800 VDC

Rack density is rising faster than traditional power delivery can keep up. AI racks are already at 140 kilowatts (kW)


, with 240 kW close behind. In DCD’s “[2026 Trends and Outlooks](https://www.vertiv.com/en-us/insights/articles/blog-posts/full--stack-infrastructure-planning-for-high----density-ai-deployments/) ,” Vertiv and industry experts discussed how operators are preparing for higher-density


AI designs, including 600 kW racks and megawatt-class scenarios.


That is why the industry is moving toward 800-volt DC (VDC).


Higher-voltage DC reduces the current required to deliver a given amount of power, creating a practical path to greater rack density. But the voltage alone is not the architecture. The real work is designing how conversion, distribution, protection, energy storage, controls, cooling, and service operate together from the grid to the chip.


Vertiv is developing that complete path, beginning with rack-adjacent sidecars and progressing toward pod-level and centralized DC architectures as the IT ecosystem, standards, and customer requirements mature.


### The physics driving 800 VDC


Somewhere between 350 and 400 kW per rack, the traditional approach begins to break down. Connector sizes, busbars, and the sheer volume of copper needed to carry that much current start to run out of room. At the same time, the power shelves inside the rack are competing for space with the compute itself. As racks scale from 72 to 576 GPUs, the silicon takes over nearly the entire enclosure, and the 8 to 16 rack units currently occupied by power conversion need to be given back to the compute.


Density is the forcing function, but it isn't acting alone. Grid interconnect timelines are stretching, and compliance requirements for large sites continue to climb. AI training clusters also behave differently than traditional workloads: when tens of megawatts of GPUs operate in unison, the resulting load swings ripple upstream toward the grid and downstream into the silicon. Each of these pressures deserves its own deeper discussion. Together, they point in the same direction. The power architecture has to change.


### Power leaves the rack


The first change arrives at the rack, and it defines the near-term horizon: deployments beginning in 2027.


As Scott Armul, Vertiv's Chief Product and Technology Officer, framed it at Vertiv's May 2026 Investor Conference: “ *This is a story of DC power in the rack moving to DC power in the pod.* ”


Two things happen at once. The power shelves move out of the rack into a standalone power center, and the voltage steps up. Vertiv, along with a broader industry coalition, has aligned around 800 VDC, as well as ±400 VDC


, as the voltage needed to move that much energy across a busbar into a rack full of 800 VDC-native architectures.


The architecture that makes this practical is the sidecar: a dedicated 800 VDC power center bundled alongside the IT rack rather than built into it. It returns the 8 to 16 rack units that power conversion previously consumed inside a 42U enclosure back to the compute. Just as important is what it doesn't change. The sidecar allows customers to retain much of their upstream AC infrastructure while introducing 800 VDC close to the compute, containing the initial 800 VDC transition close to the compute rather than requiring an immediate facility-wide redesign.


Vertiv's near-term expression of that architecture is the Vertiv™ PowerDirect 5000


, an 800 VDC sidecar, delivering from 400 kW up to 900 kW directly into the compute over a busbar tied to the GPU rack.


### The voltage is only the beginning


Raising the distribution voltage solves an important physical problem, but a deployable architecture requires much more than a converter. As 800 VDC moves from the rack toward the pod and data hall, distribution, protection, grounding, energy storage, controls, commissioning, and service must evolve with it. The transition also has to coordinate with the thermal system because every change in compute power becomes a change in heat.


The practical challenge is found at the interfaces between these elements. That is where capacity becomes stranded, protection schemes become inconsistent, operating risk increases, and component-level gains can disappear. Vertiv is developing 800 VDC as a complete power architecture within a converged physical infrastructure system.


### Where the work actually stands


800 VDC is an architectural transition rather than a single product launch, and the pieces arrive on different timelines. Separating those stages is worth doing because the distance between an announcement and a shipment is where credibility is built or lost.


Engineering validation is well advanced. Vertiv has been developing this architecture for a long time, and lab-level validation is underway right now with a customer, on their intended GPU rack. That work extends past the customer's own requirements to the full ecosystem, with power conversion, IT, and power distribution validated together as one bundled system rather than as separate parts that meet for the first time on a live site.


From there, the architecture moves into scaling. Functional customer demonstration is underway today. Portfolio commercialization begins in the second half of 2026, followed by customer pilots and an expanding deployment ramp through 2027. And this isn't happening in the abstract. As Scott confirmed when an investor asked about launch customers, the vast majority of what Vertiv presented is tied to specific customer engagements the company is designing around, building around, and preparing for launch.


### The optionality bridge


Here's the part of this transition that often gets lost, and it may be the most important point for anyone planning infrastructure today: adopting 800 VDC doesn't force you to replace what you already have.


Traditional AC architecture works with current GPUs, and through the sidecar approach, it works with 800 VDC power as well. The same holds for the next level of GPUs on the way. Operators can adopt higher-voltage DC at the rack without rebuilding the upstream power train, and there will be many flavors of this transition depending on the customer type and the business model involved.


That flexibility is the bridge between where the industry is now and where it's heading. It means the shift to 800 VDC is a phased evolution rather than a forced migration, and customers can move at the pace their programs demand.


### A phased path to centralized DC


Beyond the pod sits the longer-term horizon: the 2028 to 2029 timeframe and beyond, when 800 VDC-native rack architectures become the norm for purpose-built AI factories.


At that stage, the logical next step is for the sidecar to decouple from the IT entirely and extend across the facility. A first move is pooling power centers to serve distribution at the pod level rather than the rack level. The more meaningful transition is an MV-connected centralized DC architecture, where the conversion to 800 VDC happens at the facility scale.


Vertiv sees two parallel paths to get there, and they aren't mutually exclusive.


The first is an MV DC UPS


: a traditional transformer paired to a rectifier that converts medium voltage to DC and distributes 800 VDC throughout the site. It carries a higher technology readiness level today, and its supply chain is more mature, making it the likely first mover.


The second is the solid-state transformer,


which offers the promise of better efficiency, a smaller footprint, and fewer conversion points in the power train. Supply chain maturity for solid-state technology still has ground to cover, which is exactly why the two paths run in parallel, and Vertiv is developing both.


*Multiple AI power architectures will coexist across customers and applications.*


### Proving it works


Architecture claims are only as strong as the testing behind them, and this is where validation becomes concrete.


Vertiv is bringing online a purpose-built infrastructure AI system validation lab centered on a 1.5 megawatt (MW)


AI pod and thermal test vehicles of up to 600 kW, built to simulate how an entire AI pod performs under real-world operating conditions. The lab allows Vertiv to drop in equipment and study performance profiles, run application-level failure-mode analysis, and benchmark system behavior in a controlled environment rather than a live customer site.


That capability matters because of what it protects: time to market. GPU generations are evolving on a rapid cycle, and infrastructure development has to run inside that cycle to keep pace. As Scott put it, it's not necessarily about being alone. It's about being ahead.


### What operators should do now


Customers planning AI capacity should begin by answering four questions:


What rack and pod density must the facility support?


The architecture required for a 150 kW rack is different from one designed for 600 kW or more.


How much of the existing AC infrastructure should be preserved?


A sidecar can provide a practical bridge for brownfield sites and mixed-use facilities, while purpose-built AI factories may justify moving DC farther upstream.


Where should conversion, energy storage, and protection reside?


Rack, pod, and data-hall architectures create different tradeoffs in redundancy, efficiency, footprint, serviceability, and flexibility.


How will the complete system be validated and operated?


Protection coordination, controls, commissioning, thermal response, service practices, and future GPU generations must be designed together.


The correct decision is therefore not simply whether to adopt 800 VDC. It is where to introduce it, how far to extend it, and how to preserve options as the compute roadmap evolves.


### What comes next


The transition to 800 VDC is already moving from concept to physical systems, customer validation, and deployment planning.


The companies that lead this transition will have to do more than build an efficient converter. They will need to understand how power moves from the grid to the rack, how dynamic AI loads interact with the electrical and thermal systems, how the architecture is protected and serviced, and how it can scale across successive generations of compute.


Vertiv is developing and validating that complete architecture across the rack, pod, data hall, and grid interface.


800 VDC is the next major power transition for AI. Vertiv intends to make it deployable at scale.


### Watch the “Power Shift” video series by Scott Armul, Vertiv Chief Product and Technology Officer, for a closer look at how AI rack densities are reshaping power architecture.
