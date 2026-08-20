---
schema_version: "1.0.0"
document_id: "cd10de39b7597791e0f1273e9b00bbd3c03e05a5a686b16bfe4c16b59d435dc8"
company_key: "teradyne-inc-common-stock"
company: "Teradyne Inc."
source_id: "teradyne-inc-common-stock-rss-deb5615369ea"
canonical_url: "https://www.teradyne.com/2026/02/10/how-ai-is-changing-computing/"
published_at: "2026-02-10T17:18:53+00:00"
first_seen_at: "2026-07-20T03:33:09.653205+00:00"
fetched_at: "2026-07-28T22:20:59.045830+00:00"
content_hash: "sha256:d2167aa4a70a58a2170f4ed65ef8b6898acb03e100f0a6481eb90abf751c752d"
---

# How AI is Changing Computing—And Why Testing Is Critical

[← Back to Blog Index](https://www.teradyne.com/company/blog)


Artificial intelligence (AI) is transforming industries, enhancing our daily lives, and improving efficiency and decision-


making


but its need for compute processing power is growing at an astonishing rate, doubling every three months


(Figure 1)


.


To


maintain


this pace, the semiconductor industry is moving beyond traditional chip development


– it has entered the era of heterogeneou


s


chiplets


in advanced


integrated packages.


(Figure 1:


The Growth


of Compute Requirements


.


Source:


[https://openai.com/index/ai-and-compute/)](https://openai.com/index/ai-and-compute/)


**The Rise of Chiplets**


Chip companies like[NVIDIA](https://www.nvidia.com/en-us/) and[AMD](https://www.amd.com/en.html) are rewriting the rules, designing architectures that combine multiple CPUs and GPUs in a single advanced package along with high bandwidth memory


(HBM)


. AI workloads require rapid access to vast amounts of data, made possible by integrating HBMs. This approach, combining two, four, or more processing cores with HBM stacks, requires a complex, advanced packaging technique developed by[TSMC](https://www.tsmc.com/english) called CoWos® – Chip-on-Wafer-on-Substrate,


typically referred to as 2.5/3D packaging


(Figure 2)


.


These packages can exceed 100 mm x


100 mm


in size and will require wafer interposer probers that can handle large


CoW


modules/


stacks and


also


meet significantly larger thermal dissipation requirements, as discussed below.


(Figure 2: 2.5D/3D packaging architecture, Source: Teradyne)


To maintain peak performance, these heterogeneously integrated advanced packaging devices need proprietary high-speed interfaces to communicate efficiently. All these requirements contribute to an increasingly complex semiconductor landscape.


**Testing Becomes More Complex in Step with Chip Advancements**


As package complexity increases, so does the need for more deliberate test strategies. In the transition from monolithic dies to chiplets, long-established test methods are not always directly transferable because test IP is now distributed across multiple dies and, in some cases, across different design teams or companies. This fragmentation requires a clearer definition of what must be tested at each stage—die, bridge, interposer, substrate, and stack—and which standards or techniques apply to each scope.


Packing multiple dies into a single chiplet-based system is a major advancement, but it raises a key challenge: verifying that every component functions correctly before final assembly. Multi-die packages require rigorous screening to avoid yield loss, and it is not enough to qualify only the dies. Interposers, substrates, bridges, and stacks also need to be validated, using test techniques appropriate to each layer.


The industry is thus moving into “known-good-everything”, from known-good-die (KGD) to known-good-interposer (KGI), to known-good-CoW (KG-CoW), and so on. (Figure 3)


(Figure 3: Possible test insertions to ensure KGD and KG-CoW. Source: Teradyne)


High-speed communication between chiplets introduces an additional layer of complexity. Dies must exchange data at extreme speeds – such as during GPU-to-HBM transfers – yet their physical and electrical interfaces vary by manufacturer. Open standards like Universal Chiplet Interconnect Express (UCIe™) continue to evolve, but chiplet interfaces still differ widely. To support this diversity, test solutions increasingly need interface IP that behaves like the device’s native protocol to avoid electrical overstress or probe-related damage. Some suppliers now offer UCIe-compliant PHY and controller IP that device makers can integrate, enabling automated test equipment (ATE) platforms to test high-speed links safely and consistently.


(Figure 4: Chip level bare cooling, Source: Teradyne)


Manufacturers and test operators must also pay close attention to thermal management. More processing power means more heat dissipation issues, requiring advanced cooling methods – perhaps even liquid cooling inside the package itself


(Figure 4)


.


More die in the package means more connections and thus more resources are needed in the tester. More transistors mean higher power supply current requirements, more power supply instruments, and an increased set of thermal challenges that demand innovative cooling solutions and advanced adaptive thermal control (ATC) strategies.


Lastly, manufacturing test operations must consider the interposer, a physical interface layer that electrically connects a chip to a substrate or other active component. For example, a multilayer or 2.5D package includes multiple die on an interposer assembled on top of a substrate. That interposer functions as a mini silicon board, routing signals from the upper floor die to the bottom floor die. It’s critical that the interposer is also a known good die or known good interposer (KGI) to ensure adequate yields for advanced packages.


**The Teradyne Solution**


With each device generation, the semiconductor content increases, leading to an increase in test complexity. This increase in test complexity is driving the need for more and more scan pattern memory. To address this, Teradyne has added additional scan memory to ensure even the most complex semiconductor designs can be tested effectively.


Teradyne[UltraFLEXplus](https://www.teradyne.com/products/ultraflexplus/) is an ATE platform that provides faster throughput for compute device testing. By incorporating the Broadside architecture, which features Teradyne’s unique approach to device interface board (DIB) design, the UltraFLEXplus enables better signal routing resulting in the best signal performance at reduced numbers of DIB PCB layers. Broadside is a combination of two things: first, the company’s PACE architecture, which features embedded controllers throughout the tester for faster throughput, and second, the division of tasks for processing test results.


Recognizing the trend of integrating optical interfaces into these advanced devices, Teradyne is also incorporating advanced optics test capabilities into its UltraFLEXplus platform. This first-to-market[double-sided wafer probe solution](https://investors.teradyne.com/news-events/press-releases/detail/9/teradyne-announces-production-system-for-double-sided-wafer-probe-test-for-silicon-photonics) delivers a high-volume test solution for emerging silicon photonics and co-packaged optics where both electrical and optical tests are required.


In addition, Teradyne continues to work with leading ecosystem suppliers to enable


the


testing of large


heterogeneously integrated


advanc


ed packages


with the most stringent advanced adaptive thermal control test requirements


toward the goal of “known-good-die” to “known-good-


CoW


” and beyond.


**The Future of AI and Semiconductor Testing**


There has been an uptick in industry recognition that semiconductor test is an integral part of today’s chiplet and advanced packaging trend.


As this unfolds, AI computing will continue its pace of unprecedented evolution, relying on semiconductor test to fill a crucial role in ensuring quality devices get to market in the shortened timelines today’s market demands. Semiconductor test will remain the unsung hero of AI-driven computing, steadily enabling the next wave of technological breakthroughs.


*[Dr. Jeorge Hurtarte](https://www.linkedin.com/in/jeorge-hurtarte-phd-mba-83107919a) is currently Senior Director and Principal Marketing Strategist in the Compute Test Division at Teradyne. Jeorge has held various technical, management and executive positions at Teradyne, Lam Research, LitePoint, TranSwitch , and Rockwell Semiconductors. Jeorge is on the Advisory Board of SEMI North America and serves as co-chair of the IEEE Heterogeneous Integration Roadmap (HIR) Test Chapter. Jeorge holds a PhD in Electrical Engineering, and three master’s degrees (MBA, Computer Science, and Telecommunications). He is also a visiting professor at the University of California, Santa Cruz and at the University of Phoenix. He is the co-author of the book Understanding Fabless IC Technology .*


### Subscribe to the Teradyne Blog
