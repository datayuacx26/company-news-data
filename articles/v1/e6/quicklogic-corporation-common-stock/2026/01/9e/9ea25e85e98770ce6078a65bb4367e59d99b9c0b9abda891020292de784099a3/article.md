---
schema_version: "1.0.0"
document_id: "9ea25e85e98770ce6078a65bb4367e59d99b9c0b9abda891020292de784099a3"
company_key: "quicklogic-corporation-common-stock"
company: "QuickLogic Corporation"
source_id: "quicklogic-corporation-common-stock-rss-a25ca7be134b"
canonical_url: "https://www.quicklogic.com/2026/01/13/epson-achieves-50-energy-efficiency-with-quicklogic-efpga/"
published_at: "2026-01-13T16:00:00+00:00"
first_seen_at: "2026-07-20T23:19:30.136956+00:00"
fetched_at: "2026-07-28T22:23:40.402133+00:00"
content_hash: "sha256:e6df822d844e8791fce823be874a635b4b0d9410ec84ae78d3cabbcba86001f8"
---

# Epson Achieves 50% Energy Efficiency with QuickLogic eFPGA

Posted on[January 13, 2026](https://www.quicklogic.com/2026/01/13/epson-achieves-50-energy-efficiency-with-quicklogic-efpga/)


by[Andy Jaros](https://www.quicklogic.com/author/andyjaros/)


**First-Pass Success with QuickLogic eFPGA Hard IP on TSMC e12n**


At QuickLogic, we pride ourselves on helping customers solve their toughest design challenges with flexible, efficient, and silicon-proven eFPGA Hard IP. Recently, one of our customers announced its achievement of a major milestone that illustrates some of the benefits of implementing QuickLogic eFPGA Hard IP in advanced SoC designs.


Epson’s design goals were to significantly reduce power consumption while improving performance and adhering to a tight schedule. By moving from a pure software implementation to QuickLogic’s eFPGA Hard IP, Epson achieved a 50% improvement in overall power efficiency while maintaining the programmability needed to adapt to evolving algorithms and market requirements. Leveraging over 30 years of experience and its proprietary Australis Hard IP Generation Tool, QuickLogic delivered customer-specific Hard IP for TSMC’s e12n fabrication process that met Epson’s specifications in the first silicon pass.


According to Maho Terashima, Director of Device Technology Development at Epson, this achievement was made possible through the combination of Epson’s low-power circuit design expertise and QuickLogic’s eFPGA technology.


Epson operates in a market where energy efficiency is paramount and increasing battery capacity is not an option. Their system was originally architected to run workloads entirely in software, but as demand for more features and real-time responsiveness grew, power consumption became a limiting factor. The engineering team recognized that moving compute-intensive functions into dedicated hardware could deliver significant efficiency gains — but only if the hardware could continue to adapt to evolving algorithms and changing market requirements.


With these challenging requirements, the engineers determined that by embedding an FPGA (eFPGA) fabric directly into their SoC they could accelerate key algorithms in hardware while retaining the adaptability of a software-driven system and, in doing so, significantly lowering power consumption and improving performance.


**Optimized for Low Power on TSMC e12n**


Working closely with QuickLogic’s team, Epson selected an **Australis-generated eFPGA Hard IP core** optimized specifically for TSMC’s e12n low-power process. This combination was ideal:


1. **TSMC e12n technology** provided a baseline of excellent transistor performance with industry-leading low leakage characteristics optimized for low power consumption.
2. **QuickLogic’s proprietary eFPGA IP generator (Australis)** allowed the architecture — LUT count, Block RAM and interconnect to be tailored to the workload.
3. **Low-power optimizations** were applied throughout, including clock gating and fine-tuned routing resources to minimize dynamic switching.


The result was a right-sized fabric that avoided unnecessary overhead while delivering ample performance headroom in the first pass.


**First-Pass Silicon Success**


From design handoff to silicon validation, the IP integrated cleanly into Epson’s SoC without the need for re-spins or late-stage design changes. Epson was able to boot, configure, and validate the eFPGA subsystem immediately, accelerating its schedule and reducing risk.


**Measurable Efficiency Gains**


When the system was benchmarked, the results were clear. By migrating selected software routines into the eFPGA fabric, Epson achieved a **50% improvement in overall energy efficiency** . This not only extended battery life for their application but also enabled higher performance modes without exceeding thermal or energy budgets.


**Broader Implications**


This project illustrates why more and more SoC developers are turning to embedded FPGA IP:


1. **Flexibility** — eFPGA enables hardware to provide the flexibility of software
2. **Efficiency** — significant power and performance improvements versus running everything in software
3. **Confidence** — silicon-proven Hard IP with demonstrated first-pass success


For QuickLogic, it reinforces our commitment to delivering optimized, low-power, and customizable eFPGA IP solutions for a wide range of markets, from consumer to industrial to defense.


**Conclusion**


Epson’s achievement is a strong validation of QuickLogic’s eFPGA technology on advanced low-power processes like TSMC e12n. By combining software-defined adaptability with hardware-level efficiency, Epson was able to deliver its system on schedule, while also making it more capable and more energy-efficient.


We look forward to enabling more customers to achieve similar success as they bring innovative products to market.
