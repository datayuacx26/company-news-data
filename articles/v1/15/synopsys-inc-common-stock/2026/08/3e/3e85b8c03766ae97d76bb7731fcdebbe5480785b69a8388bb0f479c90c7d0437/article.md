---
schema_version: "1.0.0"
document_id: "3e85b8c03766ae97d76bb7731fcdebbe5480785b69a8388bb0f479c90c7d0437"
company_key: "synopsys-inc-common-stock"
company: "Synopsys Inc."
source_id: "synopsys-inc-common-stock-news-import-736729784437"
canonical_url: "https://www.synopsys.com/blogs/chip-design/gate-all-around-complementary-fets-whats-next-transistor-scaling.html"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-20T00:19:47.398494+00:00"
fetched_at: "2026-08-20T00:19:49.482974+00:00"
content_hash: "sha256:9b433571e7fb3e0a9d9fa8f8f80ec42f9fd503985dd672a76516ff653f0cf1d7"
---

# From Gate‑All‑Around to Complementary FETs: What’s Next in Transistor Scaling

## Introduction


Two years ago, we introduced[why the industry was moving to gate‑all‑around (GAA) transistors](https://www.synopsys.com/blogs/chip-design/what-are-gate-all-around-gaa-transistors.html) and how foundation IP co-optimized with[electronic design automation (EDA) flows](https://www.synopsys.com/glossary/what-is-electronic-design-automation.html) would be key to success. Since then, GAA has moved from early adoption into the mainstream of leading‑edge design, and the industry's attention is already turning to what comes after it. This article revisits where GAA stands today and looks ahead to its successor, the complementary FET (CFET).


For several decades, semiconductor industry growth has followed "Moore's law" \[1\], with the number of transistors doubling roughly every two years. Initially, the transistors were planar structures. Moore's law meant that transistors continued to be smaller in dimensions, so planar transistors soon proved inefficient in controlling the flow of charge in the channel and[FinFETs](https://www.synopsys.com/glossary/what-is-a-finfet.html) were introduced \[2\]. The "fins", semiconductor channels protruding out of the wafer in the out-of-plane direction, have the gate covering the channel on three sides, giving the gate better electrostatic control of the channel than planar FETs. They, however, had a challenge of their own. The current carrying capacity of the transistor is proportional to the number of fins (which act as "parallel channels" between the source and the drain). That makes the effective width of the transistor quantized (in steps of fin number and associated space in the layout). Also, as FinFETs kept scaling down towards the ~3nm node, they became less efficient as an electrical switch. Near the bottom of the fin, closer to the substrate, gate control weakens, making the area susceptible to leakage. Furthermore, FinFET transistor variability increases as scaling continues, making it less suitable for 2nm node and beyond.


## GAA Today: Where the Technology Stands


Beginning at around the 2nm node, the industry started transitioning to a new transistor architecture, gate-all-around (GAA). In GAA, there are nanosheets of semiconductors with the gate wrapping around them on all four sides (Figure 1), which lets the gate voltage control the charge in the channel more effectively than a FinFET.


##


***Figure 1:** Cross-section showing nanosheets stacked as in a GAA transistor.*


##


You can see the gate's stronger grip in a steeper sub-threshold slope (SS), which translates to a higher *Ion/Ioff* ratio (a transistor, as a switch, will carry the current *Ion* in its ***** "on"* **** state and the current *Ioff* , when in the *"off"* state). GAA offers better electrostatic control and hence the steeper SS. Another advantage of GAA is that the channel thickness is better controlled in the manufacturing process, leading to smaller device variations.


GAA can also be thought of as fins flipped sideways and then stacked vertically. That makes the width continuous in the plane of the layout \[3\], allowing the process to be optimized using the nanosheet width as a variable. In effect, nanosheet width becomes a knob designers can turn to balance performance against area (Figure 2). Wide transistors help achieve high performance in a standard cell owing to higher current carrying capability, which makes high-performance standard cells ideal for high-performance computing. Conversely, narrow transistor width can be used for high-density, low-power standard cells, which are better suited to battery-powered mobile applications \[3\].


It's worth noting that planar, FinFET, and GAA devices are each likely to keep playing a role: not every system-on-chip (SoC) function needs to be implemented in the most advanced node, and heterogeneous, multi‑die designs increasingly bring these transistor types together in a single product.


##


***Figure 2:** Standard cell representational top view (not to scale). GAA allows flexible transistor width.*


## Why GAA Is Critical in the AI Era


An important implication of transistor scaling is that each subsequent node consumes less power than its predecessors. Right now, the semiconductor industry's growth is fueled by the artificial intelligence (AI) "revolution". With the introduction of large language models to a broader audience in 2022, the user base of AI tools has grown exponentially. Training such models requires a huge amount of computation, which, in turn, needs specialized hardware such as GPUs, often sitting in large data centers. It is predicted that, at the current rate of growth, data centers will consume ~7-8% of global electricity demand sometime around 2030 (Figure 3) \[3, 4\]. That kind of power draw puts real pressure on the global electricity infrastructure, and it's a big reason the industry is so focused on designing chips that consume less power. This makes GAA (and its successor transistor geometries) particularly important in the AI era.


Today, high‑performance mobile is leading GAA adoption, with hyperscale servers and high‑performance CPUs following closely behind—a clear shift from the early‑adoption picture of just two years ago.


##


***Figure 3:** Current and future forecasts on global electricity demand due to data centers \[3\].*


## Two Enablers: Backside Power and DTCO


Two key technology innovations are helping realize the full benefits of GAA transistors at advanced nodes:


1. **Backside power delivery:** Traditionally, the transistors are built first, then many layers of metal are stacked on top to route power and signals between them. As dimensions shrank, routing became harder, with signal and power competing for space. In recent years, foundries have begun separating power and signal delivery to the transistor from two opposite sides of the transistor layer. This frees up more space for both and makes routing easier. With backside power delivery, bulk silicon no longer sitting beneath the transistors to carry heat away, so self-heating becomes a challenge to manage.
2. **Design technology co-optimization (DTCO):** Standard cells and static random-access memories (SRAMs) take up most of the area inside a[system on a chip (SoC)](https://www.synopsys.com/glossary/what-is-system-on-a-chip.html) \[5\], so they largely determine its[power, performance, and area (PPA)](https://www.synopsys.com/glossary/what-is-power-performance-area-ppa.html) . Beyond reducing the dimensions that impact cell area—such as contacted poly pitch (CPP), gate length, and metal pitch—advanced nodes require optimizing many other aspects to achieve desired PPA, including co-optimization with advanced EDA tools \[5\]. A technology can be optimized for PPA at multiple levels: gate length, poly pitch, and metal pitch at the first level; then gate cross-coupled structures and diffusion breaks; then optimizing for place-and-route access for the pins; and ultimately, at the block level \[5\].


## How Synopsys Helps Design Teams Make the Leap


Realizing these benefits in a real SoC depends on high‑quality IP and EDA flows that are co‑optimized for the target process—the same point we made two years ago, and it holds even more true today. Drawing on a 20‑year history of developing[Foundation IP](https://www.synopsys.com/designware-ip/memories-logic-libraries.html) across the planar and FinFET eras, and on work with hundreds of customers, Synopsys 2nm GAA Foundation IP is being delivered to customers today, co‑optimized with Synopsys EDA flows to enable the most area-optimized designs, and, ultimately, the lowest total cost of ownership at advanced nodes.[Synopsys' technology computer-aided design (TCAD)](https://www.synopsys.com/manufacturing/tcad.html) tools help in technology development and DTCO by enabling simulation of electrical/thermal characteristics of transistors at the advanced node geometries, including GAA and CFET. On the EDA side for physical implementation, the[Synopsys Fusion Design Platform™](https://www.synopsys.com/implementation-and-signoff/fusion-design-platform.html) (an integrated, golden‑signoff RTL‑to‑GDSII flow) has been qualified for GAA process technologies at major foundries. In addition, Synopsys has collaborated with foundries to optimize our AMS Design Reference Flow, based on Synopsys Custom Design Platform, for the GAA process. As the industry moves toward CFET and angstrom‑scale nodes, this same IP‑plus‑EDA co‑optimization is what will let design teams hit ever more demanding PPA targets.


## What’s Next: The Move to CFETs


The unprecedented demand for semiconductor chips driven by the AI "revolution" calls for continued device scaling to enable lower power consumption, smaller chip area, and faster chips. Two years ago, we noted that CFETs were on the horizon; today they have moved from concept to demonstrated silicon. Beyond GAA, the next generation of transistors is expected to be CFETs, where the NMOS and PMOS are stacked vertically (Figure 4), shrinking footprint and increasing transistor density.


##


***Figure 4:** 3D image showing NMOS and PMOS stacked vertically in a CFET-like arrangement.*


##


Recently, researchers demonstrated working ring oscillators and SRAM cells built with CFETs \[6\]. As with GAA technology, DTCO will be an important part of PPA optimization for CFETs \[6\]. Recent work demonstrated 3.5-track CFET designs in A7 (7 Å technology equivalent) reaching 46% area reduction compared to their N2 (2nm technology equivalent) reference, while keeping performance the same \[7\]. Potentially combined with backside power delivery and with the growing role of multi‑die, chiplet‑based design, CFET is poised to be the next step in extending scaling into the angstrom era.


[Learn more about our Synopsys Foundation IP solution](https://www.synopsys.com/designware-ip/memories-logic-libraries.html)


## References:


1. R.R. Schaller, "Moore's law: past, present and future," *IEEE Spectrum,* vol 34, no.6, pp. 52-59, 1997.
2. D. Hisamoto et al., "FinFET—a self-aligned double-gate MOSFET scalable to 20 nm," IEEE Transactions on Electron Devices, vol. 47, no. 12, pp. 2320–2325, Dec. 2000, doi: 10.1109/16.887014, 2000.
3. W. Kwon et al., "[Gate-All-Around Technology for Sustainable AI: A Foundation for Future Logic Architectures](https://ieeexplore.ieee.org/document/11353479) ," in *2025 IEEE International Electron Devices Meeting (IEDM)* , San Francisco, CA, USA, pp. 1-4, 2025.
4. IEA, *Electricity 2024: Analysis and Forecast to 2026* , Paris, France: International Energy Agency, 2024.
5. T. Song et al., "3nm gate-all-around (GAA) design-technology co-optimization (DTCO) for succeeding PPA by technology,” in *2022 IEEE Custom Integrated Circuits Conference (CICC)* , 2022.
6. S. Liao et al., "First demonstration of CFET ring oscillator and SRAM bit-cell functionality at gate pitch smaller than 48 nm for future logic and SRAM technology," in *2025 IEEE International Electron Devices Meeting (IEDM)* , 2025.
7. J.Y. Lin et al., "3.5T CFET Block-Level DTCO for Superior PPA in A7 Node by Split Power, hDR Cells, Optimized Pins and BEOL," in *2025 IEEE International Electron Devices Meeting (IEDM)* , 2025.


- [About Synopsys](https://www.synopsys.com/blogs/chip-design/category.about-synopsys.html)
- [Engineering Central](https://www.synopsys.com/blogs/chip-design/category.engineering-central.html)
- [Silicon IP](https://www.synopsys.com/blogs/chip-design/category.silicon-ip.html)
- [Foundation IP](https://www.synopsys.com/blogs/chip-design/category.foundation-ip.html)
