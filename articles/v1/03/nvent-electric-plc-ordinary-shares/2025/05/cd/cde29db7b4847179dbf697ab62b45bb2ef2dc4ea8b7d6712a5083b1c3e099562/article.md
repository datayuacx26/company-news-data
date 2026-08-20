---
schema_version: "1.0.0"
document_id: "cde29db7b4847179dbf697ab62b45bb2ef2dc4ea8b7d6712a5083b1c3e099562"
company_key: "nvent-electric-plc-ordinary-shares"
company: "nVent Electric plc"
source_id: "nvent-electric-plc-ordinary-shares-news-import-83e1433db768"
canonical_url: "https://www.nvent.com/en-us/schroff/resources/news/nvent-releases-next-generation-nvent-schroff-microtca4-crate"
published_at: "2025-05-01T12:00:00+00:00"
first_seen_at: "2026-07-23T18:41:20.344573+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:9846ac5e411f88b984e2e72c56d5742ea489229c9540ab2a5c9e04bd6614236b"
---

# nVent Releases Next Generation nVent SCHROFF MicroTCA.4 Crate

● NEW MTCA.4 Crate achieves 128 Gbits bandwidth per slot with PCIe Gen 5
● Enhanced Cooling: Improved rear card cage cooling
● Future Proof: New Cooling Unit Manager design based on controller with long life cycle
● Backward Compatible: Mix and match legacy and new chassis and cooling units


nVent Electric plc (NYSE:NVT) (“nVent”), a global leader in electrical connection and protection solutions, announced the launch of its first MTCA.4 Crate in accordance with Revision 3.0 of the MTCA.0 base standard.


In September 2023 PICMG released Revision 3.0 of the MTCA.0 specification. This revision supports PCI Express Gen 4 and 5 as well as 100GBASE-KR4 to improve system performance by 4x. To accomplish these higher data rates, SMD and compression mount connectors have been evaluated by the NextGen MTCA working group, and the simulation results have been taken as a base to define the whole data transmission channel for MicroTCA. The transmission channel has been divided into logical parts to define the limits for boards and backplane to ensure all MTCA.0 Rev 3.0 MCH’s, AMC’s and Crates work together in a PCI Express Gen4/5 or 100 GbE environment.


As these high-end switches consume more power compared to their predecessors, the power budget for the MicroTCA Carrier Hub, where the switch resides, has been upgraded from 80 W to 110 W. To prevent overheating, the cooling units’ power budget was also upgraded to 110 W. A software mechanism helps ensure the backward compatibility of old and new MCH’s, Power Modules and Cooling Units.


In the Rear Transition Module (RTM), cooling the improved 110W power limit is achieved with powerful fans and superior cooling design.


The Cooling Unit firmware performs in accordance with revision 3.0 of the MicroTCA specification. When powering up it remains in the 80W power rating. If the Power Module supports 110W per slot the cooling unit will adjust accordingly. For Power Modules only supporting 80W or if the IPMI communication gets lost it will stay in the 80W power envelope.


The Renesas H8S controller previously used on the nVent SCHROFF MTCA.4 Crates have now been set to end of life. For this reason, nVent has introduced a new Cooling unit Manager design based on the STM32 controller from STMicroelectronics which includes the additional features described above.


**Providing the highest data bandwidth**


To achieve the higher data bandwidth, the maximum trace length defined in MTCA.0 Rev. 3.0 needs to be matched. Therefore, the MCH slots moved from the outside to the middle of the card cage.


**Upgraded Materials**


The pcb material has been upgraded to a lower loss material with SMD connectors for superior performance. For optimized routing, the physical order of the AMC modules has been changed. In the past, the nVent SCHROFF MTCA.4 Crates used the physical order starting from 1 from left to right, now it’s changed into: 7-8-9-10-11-12-MCH1-MCH2-6-5-4-3-2-1.


**About nVent**


Vent is a leading global provider of electrical connection and protection solutions. We believe our inventive electrical solutions enable safer systems and ensure a more secure world. We design, manufacture, market, install and service high performance products and solutions that connect and protect some of the world's most sensitive equipment, buildings and critical processes. We offer a comprehensive range of systems protection and electrical connections solutions across industry-leading brands that are recognized globally for quality, reliability, and innovation. Our principal office is in London, and our management office in the United States is in Minneapolis. Our robust portfolio of leading electrical product brands dates back more than 100 years and includes nVent CADDY, ERICO, HOFFMAN, ILSCO, SCHROFF and TRACHTE. Learn more at[www.nvent.com.](https://www.nvent.com/en-us/home)


nVent, CADDY, ERICO, HOFFMAN, ILSCO, SCHROFF and TRACHTE are trademarks owned or licensed by nVent Services GmbH or its affiliates.


[PRODUCT PAGE](https://www.nvent.com/en-us/schroff/products/enc11850-040)
