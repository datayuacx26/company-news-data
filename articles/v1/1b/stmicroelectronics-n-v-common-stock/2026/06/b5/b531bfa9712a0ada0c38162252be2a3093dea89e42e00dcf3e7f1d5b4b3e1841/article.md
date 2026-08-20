---
schema_version: "1.0.0"
document_id: "b531bfa9712a0ada0c38162252be2a3093dea89e42e00dcf3e7f1d5b4b3e1841"
company_key: "stmicroelectronics-n-v-common-stock"
company: "STMicroelectronics N.V."
source_id: "stmicroelectronics-n-v-common-stock-rss-811b8a53447b"
canonical_url: "https://blog.st.com/800-v-hvdc-data-center/"
published_at: "2026-06-29T13:00:01+00:00"
first_seen_at: "2026-07-24T02:26:40.894294+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:f57347cf558ea3a434058fd5c670fbaa5825f619efbbf3cdd6f444f80e225feb"
---

# Update: 800 V HVDC for AI data centers thanks to 6 kW, 12kW, and 20 kW power delivery boards

### Update, June 29, 2026


**NVIDIA recently recognized ST as a contributor to the NVIDIA AI Factory MGX Ecosystem.** NVIDIA MGX is a modular reference architecture that serves as the building block for hundreds of GPUs, CPUs, storage, and more. And thanks to our ongoing partnership, ST’s 40 V, 12 V, and 6 V architectures can help create high-density power delivery boards that will go into NVIDIA MGX racks. Put simply, NVIDIA MGX helps accelerate the creation of AI datacenters by providing a foundation that engineers can rapidly build upon. As the industry moves to 800 V HVDC AI data centers, it’s this type of collaboration that will ensure a smooth and rapid transition.


---


### Update, March 17, 2026


NVIDIA and ST will present two new delivery boards for 800 VDC architectures, based on the work we have showcased in this blog post. **The first one is a 6 kW 850 kHz LLC converter that uses the same 700 V GaN power transistor as the system featured in 2025.** However, this smaller model uses a 40 V low-voltage MOSFET on the secondary side, rather than a 100 V GaN device, and converts 800 V to 12 V, achieving a peak efficiency of 97.5% and a power density of 2,500 W/in3. In a nutshell, it’s a smaller version of the 12 kW system that omits the intermediate 54 V-to-12 V converter.


**The second delivery board is a 20 kW 650 kHz eight-level stacked LLC converter that uses a 120 V GaN device on the primary side and a 25 V MOSFET on the secondary side.** NVIDIA will use it for low-power devices as it takes 800 V and outputs 6 V at a peak efficiency of 96.5%. Despite the significantly lower output than the 6 kW board, ST still maintains similar power efficiency levels. It will help power intermediate rails that require high current but prefer a low-voltage rail to improve efficiency and power GPUs, CPUs, or memory modules across various boards and subsystems.


---


### Update, October 13, 2025


**NVIDIA validated ST’s 12 kW power delivery proof-of-concept board, effectively moving into production testing.** The GPU maker is even showcasing a complete prototype designed by ST at[OCP 2025](https://www.opencompute.org/summit/global-summit) , the summit organized by the Open Compute Project. We are also publishing a white paper on high-density power delivery solutions for next-generation AI data centers to help engineers understand the principles driving our power delivery board and how we achieved such a high power density. The document covers topologies, design considerations, wide bandgap transistors, and optimizations, among many other things.


- [Link to the Whitepaper](https://content.st.com/high-density-ai-data-center-power-delivery-mcdcdc-whitepaper.html)


---


### Original publication on June 13, 2025


**ST is a key partner of NVIDIA’s new initiative to build 800 V in-rack power distribution.**


This is made possible by ST’s high-density power delivery board (PBD), which can handle 12 kW in a small form factor. Thanks to the latest advancements in ST’s SiC and GaN power technologies, STGAP silicon-embedded galvanic isolation IP, and our advanced analog and digital processing capabilities, we are ushering in a new era for data centers. This enables NVIDIA to reduce cable bulk and improve efficiency. Our collaboration means that, for the first time in data center history, we can ensure continuous 12 kW of power delivery at over 98 % efficiency, achieving a power density exceeding 2,600 W/in3 at 50 V output.


## Why would NVIDIA even want to use 800 V?


### Rethinking unsustainable models


For decades, typical 15 kW racks relying on 48 V power distribution system comfortably met the processor, memory, and storage needs of data centers. However, with the advent of AI and the massive boom in GPU-powered applications requiring large architectures for extreme parallelism, system architects now face the challenge of designing racks with power requirements ranging from 600 kW to 1 MW. Needless to say, the dramatic increase in power consumption necessitates an entirely new approach. **Delivering 600 kW at 48 V, for instance, would require an astounding current of 12,500 A!** Simply imagining the size of the cables, busbars, and heatsinks required to deal with this is enough to make some engineers faint.


### Looking at the future of power distribution


800 V is a must in AI data centers


The most obvious solution, therefore, is to increase the input voltage to reduce the amount of current required. That would automatically lead to a significant reduction in cabling and busbars. It would also reduce the number of conversion steps between the AC grid and the DC power distribution at the rack. **In fact, in a related blog post,[NVIDIA figures that moving to 800 V could help improve efficiency by up to 5 % compared to current 54 V systems](https://developer.nvidia.com/blog/nvidia-800-v-hvdc-architecture-will-power-the-next-generation-of-ai-factories/) .** Put simply, as many focus on the sustainability of hyperscale data centers, transitioning to 800 V is one of the best ways to ensure significantly better resource utilization while meeting the technological demands for greater AI innovation.


**However, the challenge was to come up with a power distribution system capable of converting 800 V to an intermediate bus, which would then generate the GPU core voltage, all while fitting in a compact form factor suitable for a server rack.** The space constraint here is critical as racks use a standard size. Unfortunately, the close proximity of components increases electromagnetic interference and heat management issues. Consequently, engineers must find a solution to address this significantly higher voltage while maintaining a compact form factor. An 800 V system also requires an entirely new isolation and grounding system. Similarly, protection mechanisms and fault handling take on a whole new dimension. NVIDIA turned to ST to find a solution.


## How can ST squeeze 12 kW in 2,600 W/in3?


### Addressing hot-swap requirements


To tackle the challenges inherent to an 800 V rack and comply with hot-swap requirements, our teams designed two parts: a hot-swap protection circuit and the power converter. The first part uses our 1,200 V silicon carbide (SiC) devices and BCD (BIPOLAR-CMOS-DMOS) controllers with galvanic Isolation. In 2021,[the IEEE recognized ST as the inventor of the BCD family of silicon processes](https://blog.st.com/ieee-award-bcd/) . BCD enables the combination of analog and digital elements to help improve the efficiency and reliability of power management solutions. Similarly, we have[nearly 30 years of experience in silicon carbide devices](https://blog.st.com/sic-timeline/) and were the first maker to popularize them in electric cars. Hence, our past experience positioned us well to tackle this new challenge.


### Optimizing the primary side


An STGAP device


The second part is the DC-DC converter, which converts the 800 V throughout the rack into the 50 V needed by each server. To provide this capability inside such a small footprint, ST uses 650 V gallium nitride (GaN) transistors in a stacked half-bridge configuration and STGAP galvanically isolated gate drivers on the primary side. Thanks to its wide bandgap of 3.4 eV and electron mobility of 1,700 cm2/Vs,[GaN has unique properties](https://blog.st.com/powergan/) that lead to a low output capacitance and lower on-state resistance, which makes it an excellent material when dealing with such a high frequency operation.


### Optimizing the secondary side


**On the secondary side, ST uses a lower-voltage GaN transistor (100 V), lower-voltage gate drivers, and our[STM32G4](https://blog.st.com/stm32g4-mixed-signal-mcu/) microcontroller.** Thanks to its timers with a resolution of less than 200 picoseconds, the MCU enables high-performance control compatible with the switching frequencies required for such conversions. Similarly, thanks to the capabilities of its STGAP devices, ST can fully isolate the primary and secondary sides of the power converter, thus protecting it from EMIs and other adverse events that would compromise such a small form factor.


### Breaking the transformer into two sets of four smaller transformers


Besides choosing the right components, ST also needed to deal with very tight space constraints, which meant shrinking the magnetics. Especially because the 10 kV isolation occupies space originally reserved for the transformer. **To reduce the magnetics used by the transformer, we split the traditional larger full bridge topology into two sets of four smaller rectification full bridges that work in parallel.** By splitting the transformer into two sets, we can reduce the magnetic flux into the core and spread the heat dissipation. Additionally, by using these four smaller full bridges, we can utilize smaller ferrite cores, thus shrinking the overall transformer size.


This list of components and topological solutions is a testament to the extensive expertise ST has acquired over the years, which makes our solutions unique. While many have to source these parts from other makers, we can offer a holistic in-house solution, which significantly helps with optimization. This new 800 V power distribution architecture also highlights the critical importance of compact packages with minimized parasitic inductance and dual side cooling. Indeed, devices with large and unoptimized packaging would make such a small form factor impossible. **However, because ST constantly strives to offer its devices in the smallest packages, we’re a key NVIDIA partner.**


## Looking at the big picture


The industry is exploring ways to create more efficient hyperscale AI infrastructure, but not everyone is adopting the same path. For instance, some are exploring ±400 V systems. **Thanks to our expertise and portfolio, we can very easily reuse our work on power converters and fit different power requirements.** In essence, ST can already serve all companies seeking to enhance the efficiency and sustainability of their AI data centers.


**NVIDIA’s announcement and collaboration are significant.** NVIDIA has already approved our proof of concept, and we will now work to manufacture the boards for testing. This milestone highlights how collaboration can transform the data center landscape and bring efficiencies that were unimaginable just a few years ago. NVIDIA’s 800 V HVDC architecture and ST’s power distribution board represent a new era for data centers.


- [Learn more about the 800 V HVDC initiative from NVIDIA](https://developer.nvidia.com/blog/nvidia-800-v-hvdc-architecture-will-power-the-next-generation-of-ai-factories/)
