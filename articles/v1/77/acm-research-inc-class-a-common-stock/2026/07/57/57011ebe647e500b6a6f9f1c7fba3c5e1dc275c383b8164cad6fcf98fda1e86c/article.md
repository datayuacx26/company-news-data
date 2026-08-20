---
schema_version: "1.0.0"
document_id: "57011ebe647e500b6a6f9f1c7fba3c5e1dc275c383b8164cad6fcf98fda1e86c"
company_key: "acm-research-inc-class-a-common-stock"
company: "ACM Research Inc."
source_id: "acm-research-inc-class-a-common-stock-rss-9159cf58def6"
canonical_url: "https://www.acmr.com/ai-spurring-growth-of-panel-level-packaging/"
published_at: "2026-07-22T20:43:00+00:00"
first_seen_at: "2026-07-22T22:58:48.056391+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:9dad6ba9ed9ae0ce8920e1b622a63dc12c75ee286d289163c91d3da1f7102830"
---

# AI Spurring Growth of Panel-Level Packaging

It’s not news that artificial intelligence (AI) is driving larger and larger chip packages. Nvidia’s Blackwell architecture helped define the rapid move toward multi-reticle packages for generative AI chips; its Blackwell B200 superchip is essentially a massive dual-die processor (2 x 800mm2 for total area of ~1600mm2), built to the reticle limit of 208 billion transistors.1


A round 300mm wafer can fit about 64 800mm2 chips, but since the chip is square and the wafer is round, a significant amount of silicon goes to waste in processing the chip—creating a yield issue. This is one of the reasons semiconductor manufacturers proposed moving to 450mm wafers a decade ago, an effort that has stalled indefinitely due to high R&D costs and lack of consensus regarding its viability for volume adoption.


When it comes to wafer-level packaging (WLP) or fan-out wafer-level packaging (FOWLP), you can run into the same problem. Being limited by the area of the silicon wafer can cause throughput and yield problems during the packaging process. The industry has continued scaling package dimensions and integration density to accommodate larger accelerators, more complex chiplet architectures, and stacks of high-bandwidth memory. However, these massive chip designs are increasing the amount of silicon, redistribution-layer (RDL) area and interconnect density that must be processed within a single package.


## FOPLP enables system package creation


Given these challenges, fan-out panel-level packaging (FOPLP) has attracted interest and attention from the semiconductor industry. According to a 2026 Yole Group report, panel-level packaging (PLP), including fan-in/fan-out solutions and high-end fan-out platforms, is emerging as a cost-efficient alternative to WLP while providing a pathway to replace traditional packages.2


To help solve the packaging side of the supply chain constraint, packaging manufacturers have proposed using square substrates, typically made of glass, to introduce a new method of packaging the wafers on a panel that increases the number of chips you can process at once.


Among the leading formats being developed for high-volume PLP are panels measuring approximately 310mm x 310mm, 510mm x 515mm and 600mm x 600mm. These formats provide roughly 3x and 5x, respectively, the usable processing area of a 300mm wafer (see Figure 1), although panel dimensions, substrate materials and process architectures continue to vary among manufacturers. Intel, Samsung, and some outsourced assembly and test (OSAT) companies are also moving toward panel-level packaging (PLP). This is due mainly to the growth of AI chip packages, which, as they continue to grow increasingly larger, are becoming systems, rather than individual packages placed upon a printed circuit board (PCB).


*Figure 1. A 300mm wafer compared to 515mm x 510mm and 600mm2 panels. (Source: ACM Research)*


## Tackling PLP bevel etch and clean


The transition to PLP will create new processing challenges that will require a new set of tools. Cleanliness and thickness at the panel edges are critical for yields and processing. Contamination or a raised edge bevel can create issues for lithography, deposition, and bonding and can potentially generate stress, which can cause warpage across the panel.


Effective cleaning across an entire panel is also critical for removing flux residues, particles and other contaminants without damaging fine features. The challenge becomes more difficult as panel dimensions increase, RDL lines and spaces shrink, and substrates exhibit greater warpage. Process tools must handle bowed organic and glass panels reliably, maintain chemical and deposition uniformity from edge to center, and dry the panel completely before downstream lithography, underfill, bonding or metallization steps. Through-glass vias (TGVs) and other emerging glass-based structures add further cleaning and plating requirements.


As a company that enables advanced WLP with our cleaning and deposition equipment, ACM Research offers three key tools developed to address the industry’s move to PLP. Given our expertise in both cleaning and electrochemical plating, these tools fit seamlessly into our portfolio and will help our customers move from WLP to PLP technology.


- [Ultra C bev-p panel bevel etching tool](https://www.acmr.com/industries/panel-level-packaging/ultra-c-bev-p/?utm_source=blog&utm_medium=website&utm_campaign=foplpblog1) – Designed for organic, glass and bonding panels, the double-sided bevel-etch system removes unwanted copper and residues from panel edges. Controlling edge thickness and contamination helps reduce downstream lithography, bonding and handling problems that can be amplified by panel warpage. The system enables advanced panel handling with a single robot, efficient copper removal using diluted sulfuric acid and peroxide (DSP), and high throughput of 40 panels per hour.
- [Ultra C vac-p flux cleaning tool](https://www.acmr.com/industries/panel-level-packaging/ultra-c-vac-p-flux/?utm_source=blog&utm_medium=website&utm_campaign=foplpblog1) – This system uses vacuum-assisted cleaning to remove flux and contaminants from hard-to-reach spaces around bumps and chiplets on organic and glass panels. The approach helps cleaning chemistry penetrate narrow gaps before underfill, where remaining residues can contribute to voids and reliability problems. ACM has shipped the system to a leading global semiconductor packaging manufacturer, indicating that panel-level vacuum cleaning is progressing toward production evaluation and deployment.
- [Ultra ECP ap-p tool](https://www.acmr.com/industries/panel-level-packaging/ultra-c-ecp-ap-p/?utm_source=blog&utm_medium=website&utm_campaign=foplpblog1) – Our panel-focused electrochemical plating system supports 510mm x 515mm and 600mm x 600mm panels, both glass and organic. Its horizontal plating approach is designed to provide uniform deposition across the full panel for RDL, pillar, bump, fan-out and TGV processes. Configurations can incorporate up to 16 plating chambers for copper, nickel, tin-silver, gold and other materials, while panel-handling technology can accommodate warpage of up to 7mm.


Our broader advanced packaging portfolio also addresses complementary wafer-level processes used in heterogeneous integration. The[Ultra Pmax™ PECVD system](https://www.acmr.com/tools-and-processes/pecvd/ultra-pmax/) deposits dielectric films for TSV and hybrid-bonding applications on 300mm wafers or substrates, providing the film coverage and uniformity needed before copper fill. Together with ACM’s cleaning, electroplating and stress-free polishing technologies, this extends our capabilities across multiple stages of advanced package fabrication.


These systems are designed to work together across critical panel cleaning, bevel-etch and metallization steps. As PLP moves from pilot development toward early high-volume manufacturing, the ability to manage large-area uniformity, warpage, contamination and multiple metallization schemes will be essential to achieving competitive cost and yield.


While PLP is unlikely to replace WLP across every application, it is emerging as an important complementary platform for large, highly integrated AI, high-performance compute (HPC) and chiplet-based packages.


## Panel Level Packaging FAQ


What limitations exist with wafer-level packaging (WLP) and fan-out wafer-level packaging (FOWLP)?


The scalability of WLP is constrained both by wafer size and shape, as they restrict the maximum number of dies processed per wafer. WLP mainly supports single-die solutions, limiting multi-chip integration on one package. Warpage or bending of the wafer during processing is a persistent issue that affects yield and reliability. In addition, the exposed sides of the die remain vulnerable to chipping and cracking after dicing, posing handling risks. WLP also generally requires specialized tools and processes, increasing manufacturing complexity.


FOWLP allows the WLP redistribution layer (RDL) and interposer area to exceed the die size, maintaining the thin profile while enabling more I/O connections and better electrical performance. However, while improving on WLP, FOWLP faces higher warpage due to the larger reconstituted wafer stack, which can reach significant levels and complicate handling and placement precision. Warpage can decrease yield and reliability if not properly controlled. Additionally, the FOWLP process involves complex steps that can increase production complexity and cost, making material selection and process control critical to performance.


What is fan-out panel-level packaging (FOPLP), and why is it gaining traction?


FOPLP uses larger rectangular panels instead of circular wafers to carry and package ICs, using redistribution layers to integrate multiple heterogeneous components into a single package. FOPLP offers significant cost savings through higher throughput and better utilization of materials, resulting in up to 20–30% lower costs compared to FOWLP. The process features improved electrical performance, thinner profiles, and shorter interconnects, benefiting such high-performance and miniaturized devices as smartphones, wearables, automotive electronics, and IoT applications. Additionally, FOPLP enables better scalability for volume manufacturing and supports heterogeneous integration – key to advanced semiconductor packaging.


Which companies are moving toward PLP?


A number of companies have made public their plans to integrate PLP and FOPLP into their packaging strategies targeting advanced chip development. Prime examples include TSMC, Samsung, ASE and Amkor Technology.


What are the key processing challenges with PLP?


Key processing challenges with PLP include maintaining cleanliness across large panel surfaces, controlling and minimizing warpage and raised edge bevels, and ensuring uniformity in critical process steps such as electroplating, lithography, and bonding. The larger panel sizes also introduce complexity in handling, bringing contamination risks that can impact yield. Other major concerns include achieving consistent die placement, managing mold material shrinkage during curing, and die shifting on the panel. Variations in panel size and materials also demand specialized process tool adaptations, and industry standardization is being investigated to improve design-to-manufacturing compliance.


Why is effective cleaning so important in PLP?


Effective cleaning is critically important in PLP because it ensures the removal of flux residues and other contaminants that can cause defects such as shorts, corrosion, delamination, and device failure. PLP uses larger panels compared to traditional wafers, increasing cleaning complexity as residues must be thoroughly removed even in small spaces with high surface tension, such as sub-20-micron bump pitches. Residue left after solder reflow can lead to electrochemical migration, affecting package reliability and yield. Also, cleaning must be uniform across larger panel sizes and varied materials, requiring advanced cleaning technologies, such as vacuum-based and chemical processes, to prevent defects and improve overall HVM performance.


**References**


1. Moore, S.,[“Nvidia Unveils Blackwell, Its Next GPU,”](https://spectrum.ieee.org/nvidia-blackwell) *IEEE Spectrum,* March 2024.
2. [Panel Level Packaging 2026](https://www.yolegroup.com/product/report/panel-level-packaging-2026/) , Yole Group, March 2026.
