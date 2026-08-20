---
schema_version: "1.0.0"
document_id: "f798b5bb33d5394ff53858bd18ddcf914326e56b038576e54e97f8e47f0187ff"
company_key: "arista-networks-inc-common-stock"
company: "Arista Networks Inc."
source_id: "arista-networks-inc-common-stock-news-import-c50acbb14cd0"
canonical_url: "https://blogs.arista.com/blog/ai-datacenters-are-reshaping-the-optics-industry"
published_at: null
first_seen_at: "2026-07-21T07:42:03.541009+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:77b10c2c7741d60776c7a7a9f861a9da429d1ebc113bf659fd6d56012d869bf9"
---

# AI Datacenters are Reshaping the Optics Industry

In 2016, Arista Networks together with powerful industry leaders, announced the OSFP (Octal Small Form-Factor Pluggable) specification and multi-source agreement.


Ten years later, more than 100 million OSFP are projected to ship this year, making OSFP the most important optics module form factor of all time.


The remarkable success of the OSFP form factor was largely due to a combination of:


-


High front panel density:


Supporting 32 1600G OSFP modules per 1U, and


-


Robust thermal design:


Supporting 30W+ power per module with air cooling.


This enabled OSFP to support every optics standard—from DR, FR, LR, SR to ZR—as well as all interface technologies, including fully-retimed, half-retimed, and linear or LPO optics, a technology that Arista pioneered to minimize power consumption.


OSFP will continue to thrive as the highest volume optics module form factor for the foreseeable future. That said, the relentless increase in bandwidth demands of large AI data centers are exceeding the OSFP design envelope in terms of bandwidth density, cooling capacity, and reliability.


To address the requirements of AI data centers, we developed a new 12.8 Tbps liquid cooled optics module that we call XPO (eXtra-dense Pluggable Optics). It offers 4X the front-panel density of OSFP, integrated liquid cooling that supports any kind of optics, and a large reduction in failure rates due to a combination of lower component counts and lower component temperatures.


*Figure 1: The 12.8 Tbps liquid cooled XPO Module*


**Densification — Shrinking the Network Footprint**


XPO density is a game changer. A single XPO module replaces 8 OSFP modules.


### *Figure 2: One XPO Module replaces 8 OSFP Modules*


###


### *Figure 3: A 204.8T Switch with 16 XPO modules fits into one open compute rack unit*


### *Figure 4: A 204.8T Switch with 128 1600G-OSFP modules requires four rack units*


In short, XPO allows customers to build large AI data centers with one quarter the switch racks. This is hugely important for both scale-up and scale-out applications, where without XPO the number of traditional switch racks would exceed the number of GPU racks.


Imagine a 400 MW AI datacenter with 1024 GPU racks of 128 GPUs each for a total of 128,000 GPUs. Assume 12.8T scale-up and 1.6T scale-out bandwidth per GPU. With OSFP switch racks that have a density of 1.6 Pbps per rack, this would require more than 1400 switch racks for scale-up and scale-out fabrics. With XPO, this would require 75% fewer racks, saving over 1050 racks or 44 % of the floor space.


Eliminating 75% of switch racks translates to massive reductions in construction and infrastructure costs, including power distribution, plumbing and installation costs, while accelerating deployment timelines.


Native Liquid Cooling


All large AI data centers will be liquid cooled and the switches that go into these data centers also need to be liquid cooled. While one can add liquid cooled cold plates on flat-top OSFP modules, this does not substantially improve thermal performance.


XPO solves this challenge by integrating a liquid cold plate inside the module, with two 32-channel paddle cards sharing the common cold plate which can cool both low power as well as high-power optics such as 8x1600G-ZR/ZR+ with up to 400W of power.


### *Figure 5: XPO assembly with shared cold plate and two 32-channel paddle cards*


Higher Reliability


The integrated cold plate keeps component temperatures 20-25°C lower than in an air-cooled OSFP modules. Further, the liquid flow temperature only varies gradually which greatly reduces thermal stress. Both factors significantly reduce component failure rates compared to the traditional air-cooled OSFP modules.


XPO modules have also much fewer components compared to the equivalent number of OSFP modules. Each 32-channel paddle card has only one microcontroller and one set of voltage converters, a 75% reduction in common components versus 4 OSFPs. Reducing the number of components improves reliability since the most reliable components are those that don’t exist.


XPO also improves the overall system reliability of the switch system by moving the voltage conversion from the motherboard into the XPO module, greatly reducing the number of components required on the motherboard.


Universality


One key advantage of XPO is that despite its compact size, the paddle card PCB area available is almost the same as eight OSFP modules. This allows XPO modules to use existing silicon and photonics components without new silicon development.


The large paddle card area means that XPO can support any optics solution that exists today or is in development, including 1600G-DR, FR, LR, SR, ZR, ZR+, Coherent-Lite, RF-Microwave, as well as next generation 16- and 32-channel photonics designs.


Power Efficiency


Achieving the highest optics power efficiency is incredibly important for AI data centers. XPO supports the most power efficient optic designs in two ways. First, it provides a clean electrical channel to the switch chip that supports a low-power linear-interface. Second, it supports the most power efficient photonics technologies, as well as other technologies such as RF-Microwave that are even lower power.


**A Vibrant Ecosystem**


Joining Arista in forming the XPO MSA are 45 founding members, including the world’s leading optics module suppliers. We greatly appreciate the enthusiastic support we have received from all XPO module and technology partners. For more information on the XPO MSA, please visit[www.xpomsa.com](https://www.xpomsa.com/)


**Summary**


In conclusion, XPO introduces five major innovations:


1.


A four-fold increase in front panel density enables a four-fold reduction in network switch racks which enables much denser and cost-effective data center designs.


2.


Support for all existing and future optics standards and technologies, including new technologies in development such as coherent-lite, slow&wide and RF-microwave.


3.


An integrated cold plate that efficiently cools both low power linear interface optics and high power ZR+ optics up to 400W per module.


4.


Significantly improved reliability due to lower component temperatures, minimal temperature variations reducing thermal stress and lower component count.


5.


Superior power efficiency with a clean linear channel, ability to support the lowest power photonics technologies, and efficient 50VDC power delivery.


Meeting the vast bandwidth requirements for AI scale-up, scale-out and scale-across fabrics is no easy task. The new XPO form factor was designed to address the needs of the largest AI data centers in terms of density, native liquid cooling, and reliability while preserving the manufacturability, configurability, and serviceability advantages of pluggable optics modules.


**References**


[Press Release](https://www.arista.com/en/company/news/press-release/23697-pr-20260311)


[XPO White paper](https://www.arista.com/assets/data/pdf/Whitepapers/XPO-Whitepaper.pdf)


XPO MSA Website (XPO MSA) please visit


[www.xpomsa.com](https://xpomsa.com/)


[OFC Booth #1571](https://events.arista.com/ofc-2026)


[Launch Video](https://youtu.be/0t4F95TwSG0)


[Webinar](https://events.arista.com/xpo-the-next-generation-of-pluggable-optics-for-ai-networking)
