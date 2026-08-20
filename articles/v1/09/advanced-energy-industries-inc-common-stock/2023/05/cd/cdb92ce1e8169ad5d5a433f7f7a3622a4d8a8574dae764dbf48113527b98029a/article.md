---
schema_version: "1.0.0"
document_id: "cdb92ce1e8169ad5d5a433f7f7a3622a4d8a8574dae764dbf48113527b98029a"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/powering-the-compute-intensive-artificial-intelligence-revolution/"
published_at: "2023-05-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:9e3c7085447a64ad1f5367c08507c818e25a4f2a54c75fc08b8da78d0f7f5b7a"
---

# Powering the Compute-Intensive Artificial Intelligence Revolution

**Key Takeaways:**


- Rise of conversational AI tools like OpenAI’s ChatGPT and Google’s Bard.
- Potential to augment or replace traditional online search tools.
- Industry experts predict significant market growth and revenue potential for AI technologies.
- Delivering low-latency AI responses requires substantial computing power, impacting data center energy demands.
- Adoption of advanced hardware (GPUs, ASICs) increases power needs, with future racks potentially consuming up to 100 kW.
- Data centers are shifting from 12 V to 48 V systems for better efficiency and reduced cooling requirements.
- Innovations in power supply design support this shift.
- Advanced Energy Artesyn CSU front-end series offers flexible power conversion for systems requiring 550 W to 2,400 W.
- Future data center advancements will focus on improving power supply performance and density.
- Adoption of wide bandgap technologies and transition from air-based to liquid cooling methods to manage power and thermal demands.


Recent months have seen a flood of news stories highlighting so-called ‘conversational’ artificial intelligence (AI) tools such as OpenAI’s ‘[ChatGPT](https://openai.com/blog/chatgpt) ’ and Google’s ‘[Bard](https://blog.google/technology/ai/bard-google-ai-search-updates/) ’. Many of these stories focused on how such tools can augment - and possibly ultimately replace - the online search tools that we have become familiar with and depend on over the last 25 years.


Industry experts predict a bright future for such generative pre-trained transformers as AI evolves from a technology running in the background to one that takes center stage and is directly accessible to the public. ChatGPT, for example, racked up over 100 million users in just two months following its launch. Some analysts predict that the global Chatbot market will be worth around $4.9 billion by 2032, and analysts at Credit Suisse estimate that Microsoft alone could leverage ChatGPT capabilities across its businesses to generate nearly $40 billion of incremental revenue over a period of 5+ years.


Delivering low-latency, AI-based responses require substantially more computing power than traditional search-based techniques, significantly impacting the energy requirements in the data centers that house the servers processing requests. When asked to write a paragraph on its own power demands, for example, ChatGPT responds with:


*“Data centers that process ChatGPT requests require a significant amount of power to support their high computational demands. Natural language processing and deep learning algorithms, which are used to power ChatGPT's responses, require massive amounts of processing power, memory, and storage. As a result, data centers need to be equipped with high-performance servers, cooling systems, and power infrastructure to ensure that they can handle the workload. Furthermore, the energy requirements of data centers have increased in recent years due to the growth of online services and the demand for real-time responses. To address this, data center operators are increasingly adopting energy-efficient technologies such as liquid cooling, renewable energy sources, and server virtualization to reduce their carbon footprint and lower their operating costs.”*


In summary: data centers face the challenge of delivering the processing power required for low-latency, conversational responses from AI applications while keeping energy use as low as possible. One important way to do this is through careful selection of server power architectures and power supplies.


### **Addressing Performance Demands**


To meet the processing requirements of AI and machine learning (ML) software, the adoption of advanced hardware including GPUs, FPGAs and ASICs is growing, which comes with a consequent increase in power demands.


Two years ago, Intel’s ‘Ice Lake’ server processor is rated at 270 W TDP (thermal design power) while the current Emerald Rapids is increased up to 375 W TDP. Next-generation devices such as ‘Granite Rapids’ will increase this further to around 400 W TDP. AMD processors are showing a similar increase in TDP while PCIe versions of NVIDIA’s GPUs are expected to move from 350 W TDP to a staggering 1000 W TDP within a year or so. Similarly, TDPs of other key components such as memory devices and network adaptors are also expected to increase.


In the past, data center power-per-rack had been in the span of 6-15 kW while currently 80% of rack power shipped is in the 20-40 kW per rack range. Extrapolating the increased power demands of processors and GPUs, future racks could consume as much as 100 kW per rack.


The significant increase in power requirements is a critical financial issue for data centers; an issue that has been exacerbated recently thanks to significant increases in energy costs. Electricity is one of the main input costs of operating a data center, which will consume megawatts. For example, Equinix, one of the leading data center companies, has disclosed that cost of power represents 22% of its 2020 operational costs.


Other than electricity, purchasing and operating real estate is another major cost for data center operators. To enable up to 100 kW racks without increasing the footprint of each rack, power density is also a significant consideration when designing / specifying power solutions. This is driving the evolution of power supplies that, thanks to technical innovations, are supporting densities as high as 100 W/in3.


### **Powering the Performance**


Perhaps the most significant enhancement that delivers savings for higher power systems, is the move from 12 V to 48 V operation – a move that has been supported by the[Open Compute Project](http://www.opencompute.org/) (OCP). At all power levels, 48 V delivers more efficient power conversion than 12 V, often exceeding 2% benefit at full load. This, in itself, will result in significant energy savings for a data center.


What’s more, the current draw for the same power level in a 48 V system is only one quarter of the draw for a 12 V system. Consequently, as distribution losses are proportional to the square of the current, these are reduced by a factor of 16. All of this contributes to improved thermal performance, increased power density and reduced cooling requirements. 48 V operation also allows a significant reduction in the size of busbars and cabling, saving cost, space and weight.


A power solution that is compliant with the OCP Open Rack v3 (ORv3) specification is[Advanced Energy’s Artesyn® 1OU shelf](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/power-shelves/ocp-compliant/orv3-shelf/) , which is typically used for compute and storage applications that require reliable power and optional battery backup. A populated shelf generates a 50 VDC output with total potential power capability of 18 kW by accommodating 6 x 3 kW hot-swappable single-phase PSU modules such as Artesyn’s 700-015234-0100. The power inputs use a universal seven-pin connector that can be configured as star, delta or single phase, while a hot-pluggable shelf controller for network-based monitoring and control is DMTF Redfish® compatible.


Another solution is Advanced Energy’s[Artesyn CSU front-end series](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/ac-dc-front-end-psus/crps-series/) that offers flexible power conversion in the common redundant power supply (CRPS) form factor (1U x 73.5 mm x 185 mm). The CSU series addresses requirements from 550 W up to 2,400 W, covering cost-sensitive lower power systems right up to space-constrained higher power systems.


These high-performance units offer class-leading power density of up to 75 W/in3 with commonality of form, fit and function that future-proofs system designs. A variety of options are available including airflow direction that allows deployment in traditional data centers, -48 VDC data centers and telecom central offices. The DC input option enables the provision of a battery backup, while active current sharing eliminates the need to use additional components for parallel operation.


All AC-input CSU models are certified for 80 PLUS® Platinum level efficiency and digital PMBus® based control facilitates remote set-up, monitoring and control using a graphical user interface, allowing users to easily implement sophisticated power management schemes.


To support high-performance processors that demand higher power, there is an increasing need for high-efficiency, high-power-density board-level power solutions such as point-of-load (PoL) DC-DC converters. Advanced Energy’s 110 A-rated LGA110D from its Artesyn product line achieves a higher power density than any digitally controlled PoL in its class, with efficiencies up to 96% and a current density of 220 A/in2. Dual, independent 55 A/175 W outputs reduce the number of PoLs required in the target application.


While the efficiency percentage gains may seem small, the huge scale of data center power consumption makes these improvements significant. As an example, Advanced Energy’s ORv3-compliant power shelf is delivering the industry-leading efficiency at 97.8%, which is at least 0.5% higher than our next closest competitor and substantially higher than many Platinum and Titanium-graded power supplies. Even at just 0.5% higher efficiency, continuous operation at 40% load would reduce the input power by 37.83 W (7.368 kW vs. 7.406 kW), which leads to energy cost reduction by around $160 per shelf over a five-year period even for the most efficient data center (based on US EIA reported national average of 8.61¢/kWh for October 2022 and industry-leading PUE of 1.09).


### **Looking to the Future**


While the many advances described will bring significant benefits in the near-term, the continued rapid growth in data center capacity and associated power needs will require further developments.


These not only include ongoing performance and power density improvements of power supply architectures, but also the increased use of wide bandgap technologies and innovations in how heat dissipated from hyperscale data center hardware is removed.


Many consider fundamentally changing cooling methods to be an essential step and, specifically, the transition from air-based to liquid cooling, including immersion cooling in which heat generated is directly and efficiently transferred to a non-conductive fluid. This evolution will force system architects and operators to consider thermal implications and material compatibility when it comes to designing and implementing ORv3-compliant power supplies and power shelves for high-density racks.
