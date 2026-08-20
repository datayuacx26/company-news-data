---
schema_version: "1.0.0"
document_id: "9b6aa8289482de9a2dffe4933aae80519ac5c2e311ed973bd9bd533fbf7120ea"
company_key: "power-integrations-inc-common-stock"
company: "Power Integrations Inc."
source_id: "power-integrations-inc-common-stock-news-import-7f08bac7fb06"
canonical_url: "https://www.power.com/resources/green-room/blog/challenges-providing-dc-dc-power-supply-solar-race-car-applications"
published_at: "2025-08-25T21:12:00+00:00"
first_seen_at: "2026-07-22T10:00:02.672687+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:7ec57ee2cc621dd07aa6393554e8fb228f56796fe6ec6f8e6f2ce7b0094d5024"
---

# Challenges of Providing a DC-DC Power Supply for Solar Race Car Applications

As part of our coverage of the[2025 Bridgestone World Solar Challenge](https://worldsolarchallenge.org/) , we’re taking a closer look at the engineering behind the scenes—specifically, the power electronics that help make[Team αCentauri’s solar car Silvretta](https://www.acentauri.ch/) a reality. While the race itself is a grueling 3,000 km journey across the Australian Outback, the preparation is just as intense.


Designing a solar race car requires not only mechanical ingenuity but also highly efficient and compact electronics to manage power delivery across the vehicle. In this post, we explore the challenges of providing a DC-DC power supply for such a demanding application—and how Power Integrations rose to meet them.


#### **Engineering Against** **Harsh** **Environment**


Energy conservation during a solar car race is paramount. The power supply must operate at the highest possible efficiency while remaining lightweight and compact to avoid burdening the vehicle with excess mass. However, physical and electrical constraints are only part of the challenge—environmental factors present equally formidable obstacles.


The Outback is an unforgiving landscape. Daytime temperatures can swing from 20°C to 40°C (68°F to 104°F), and during the peak of summer, they can soar to 50°C (122°F). In contrast, winter nights can see temperatures plummet to 0°C (32°F).


The predominantly arid terrain means airborne dust is ubiquitous. This poses a significant issue, as it can affect all exposed onboard electronics. Protective enclosures must be carefully designed to avoid obstructing airflow, which could otherwise lead to component overheating. Meanwhile, dust ingress due to inadequate insulation may cause short circuits between component terminals—raising serious safety concerns during vehicle operation.


#### **First Milestone:** **The** **2023 Solution** ****


In 2023, when Power Integrations and Team αCentauri first partnered for the Solar Challenge, PI provided a[flyback DC-DC converter to power the auxiliary systems of the solar race car](https://www.power.com/community/green-room/blog/beat-harsh-environments-outback-powigan) . At the time, the racing team required the design to be at least 95% efficient while delivering from 5 to 50 W of output power. To meet these goals, PI and αCentauri employed an[InnoSwitch3-EP](https://www.power.com/products/innoswitch/innoswitch3-ep) flyback switcher IC in their design.


The InnoSwitch3-EP device is a 750 V CV/CC offline switcher IC featuring[PowiGaN](https://www.power.com/company/our-innovations/powigan-technology) technology. It features synchronous rectification to achieve higher efficiencies. FluxLink technology provides complete isolation between the primary and secondary controllers while maintaining constant communication—eliminating the need for opto-couplers.


The InnoSwitch3-EP IC also includes various safety features and advanced protections, such as auto-restart in response to overvoltage events, fast input line overvoltage/undervoltage protection, and open SR FET-gate protection.


The resulted converter design achieved 95.32% efficiency at full load with a 150 VDC input supply. It eliminated the need for metal heatsinks while maintaining a very small footprint and low component count.


#### **The Next Step** **:** **Designing with AEC-Q100-Qualified** **InnoSwitch3-AQ** ****


For this year’s race, particular attention was given to enhancing the design’s resilience while maintaining much of the previous race’s specifications. To withstand the harsh Outback environment during the first ever winter race, Power Integrations provided an[updated design](https://www.power.com/rdr-85slr) employing an automotive-rated[Innowitch3-AQ](https://www.power.com/products/innoswitch/innoswitch3-aq) switcher IC. The new board ensures proper creepage and clearance in accordance with IEC-60664 standards (Parts 1 and 4). Additionally, it delivers 175% peak power for at least 100 milliseconds while guaranteeing top-notch output voltage regulation during load transients.


The design was also engineered with increased resistance to vibration. Automotive-rated and approved connectors ensure reliable contacts, and component selection adhered to standard automotive design practices. This ensures the DC-DC power supply board, and its components can withstand significant mechanical shocks.


The InnoSwitch3-AQ includes several features to qualify it for automotive applications. These include reinforced isolation exceeding 4000 VRMS, improved noise immunity based on EN61000-4 standards, and an extended operating ambient temperature range to meet automotive requirements. Furthermore, newer versions of the off-line controller now support operation with input voltages up to 1200 VDC—all while retaining the legacy protections and efficiency benchmarks of its predecessor.


To learn more about this design,[download the reference design report](https://www.power.com/rdr-85slr) . Solar car race teams around the world can get a design kit based on this converter *for free* .[Register here](https://pages.power.com/solar-race-car.html) .


Silvretta enters qualifying ahead of Bridgestone World Solar Challenge. Team aCentauri prepares Silvretta for race start in Darwin. Automotive-qualified InnoSwitch3-AQ switcher IC achieves over 95% efficiency across input range. The PowiGaN-based DC-DC converter sits neatly in Silvretta's electronics box, allowing a clean, compact system design. Silvretta easily passes scrutineering.


***This Mr. Green blog is part of our special coverage of Bridgestone World Solar Challenge 2025 as our #PowiGaNVan chronicles Team αCentauri’s journey through the Australian Outback. Follow #PowiGaNVan on Power Integrations’ official***[YouTube Channel](https://www.youtube.com/channel/UCQU-jmHqB3gAPPTvI6RZTcw/) ***and***[LinkedIn Page](https://www.linkedin.com/company/power-integrations/) ***.***
