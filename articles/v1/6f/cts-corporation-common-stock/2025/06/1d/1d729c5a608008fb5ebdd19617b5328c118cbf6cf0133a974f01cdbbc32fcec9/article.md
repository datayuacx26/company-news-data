---
schema_version: "1.0.0"
document_id: "1d729c5a608008fb5ebdd19617b5328c118cbf6cf0133a974f01cdbbc32fcec9"
company_key: "cts-corporation-common-stock"
company: "CTS Corporation"
source_id: "cts-corporation-common-stock-news-import-47f21efce330"
canonical_url: "https://www.ctscorp.com/Resources/Blog/CTS-Develops-Current-Sensing-for-Hitachi-SiC-Based-EV-Power-Module"
published_at: "2025-06-24T03:27:57.330+00:00"
first_seen_at: "2026-07-21T15:29:46.953658+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:294423601610be3450d5bda1bb8188156a6601d8e692694ca37fb3ee7e0cd42a"
---

# CTS Develops Current Sensing for Hitachi SiC-Based EV Power Module

In late spring of 2022, Swiss-based electronics company Hitachi Energy announced the global launch of the *RoadPakTM* , an innovative power semiconductor module for electric vehicle inverters of all kinds.


The RoadPak was the first dedicated e-mobility module by Hitachi to leverage *silicon carbide* ( *SiC* ) technology. Utilizing SiC-based *Metal-Oxide-Semiconductor Field-Effect Transistors* ( *MOSFETs* ) to achieve greater levels of power density, the RoadPak enables for instance faster battery charging and greater operational reliability over time while minimizing power losses.


In wake of the launch, Hitachi Energy approached CTS Corporation for a custom current sensing solution that would not only fit the particular package design of the RoadPak, but also be able to handle the high current levels of the module. At the same time, the solution had to contend with the unique challenges posed by the inherent characteristics of the SiC MOSFET technology.


## Accommodating Existing Designs


The first and immediate task was to design a very compact current sensor housing that would fit the package of the already launched RoadPak module. The CTS design team came up with a solution that would accommodate three essential criteria, namely easy integration, structural support and space constraints.


Ease of integration was achieved by implementing a snap-fit mechanism, allowing the end customer to effortlessly attach the current sensor module to the RoadPak with minimal tooling. When attached, the current sensing module simultaneously supports the press-fit pins that protrude from the RoadPak, simplifying the overall assembly of the package. And finally, the CTS team was able to produce a current sensing core narrow enough to be placed around the RoadPak’s thin output busbar while still leaving enough room for it to be welded onto the main busbar terminal.


Next came the challenge of finding the right, small-size sensing element that also could be calibrated for the three different current ranges of RoadPak modules (600, 800 and 1150 APK). By adding proper sensing electronics, the RoadPak and the CTS module would make a complete, one-stop solution, ready for direct integration in electric vehicle architectures worldwide. The[MLX91217 Hall-effect sensor from Melexis](https://www.melexis.com/en/product/MLX91217/250-kHz-Conventional-Hall-Current-Sensor-with-improved-diagnostics) was chosen due to its high accuracy, fast response time and wide current measurement range.


## Solving the Challenges of Silicon Carbide Sensing


Yet, the solution still had to meet the unique challenges that arise when using SiC-based technologies. Silicon carbide devices exhibit incredibly short switching time, down to mere nanoseconds, which leads to very high voltage slew rates. Consequently, parasitic capacitively-coupled noise can inject false signals into nearby circuitry, affecting the current readings as the signals can be misinterpreted as real current variations.


To circumvent this issue, the CTS team employed a double-insulation design, fully isolating the sensor itself from the busbar. At the same time, the electronic routing was done in such a way to couple as little as possible with the busbar in order to minimize the effect. This has proven very effective as the CTS current sensing module for the RoadPak continues to show excellent accuracy over extended periods of time.


The Hitachi RoadPak™ is available for electric vehicle OEMs worldwide, and it can also be acquired with the CTS current sensing module. The module is available both with and without an integrated Hall-effect sensor.


To learn more about Hitachi Energy and RoadPak™ SiC-based power module for EVs, visit[the company's website](https://www.hitachienergy.com/products-and-solutions/semiconductors/e-mobility-modules) .


**CTS Current Sensor Module for Hitachi RoadPak™**


**Parameter** **Value** **Unit**


Current Range +/-600, +/-850, +/-1200 A


Sensitivity 3.33 mV/A


Linearity Error <1 % N_LE


Thermal Offset Drift <5 mV


Operating Temperature -40 to 125 ºC


---


## Related Resources


[Critical Sensing Systems](https://www.ctscorp.com/Products/Current-Sensors)


[CTS Current Sensors](https://www.ctscorp.com/Products/Current-Sensors)


[CTS Cores and Shields](https://www.ctscorp.com/Products/Current-Sensors/Cores-and-Shields)
