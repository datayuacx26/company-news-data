---
schema_version: "1.0.0"
document_id: "eb1c941a08e00b703ab344c24627575b30aff96a6b0bd871cf544db2e9011566"
company_key: "astera-labs-inc-common-stock"
company: "Astera Labs Inc."
source_id: "astera-labs-inc-common-stock-rss-6ea615f4bb0d"
canonical_url: "https://www.asteralabs.com/resources/blog/advancing-standards-based-ai-fabric-ras-through-astera-labs-cosmos-and-insyde-supervyse/"
published_at: "2026-08-10T23:27:58+00:00"
first_seen_at: "2026-08-11T01:31:48.197372+00:00"
fetched_at: "2026-08-11T01:31:49.421430+00:00"
content_hash: "sha256:a9c2013bd271a824873b60f61116ee11ede68f09760246dec8e11f9e17c79f30"
---

# Advancing Standards-Based AI Fabric RAS Through Astera Labs’ COSMOS and Insyde Supervyse®

As the AI industry responds to demanding agentic and high-context inference workloads by adopting rack-scale and beyond architectures, data center operators are no longer managing a handful of high-speed links in isolation.


They’re managing far larger populations of GPUs, DPUs, SSDs, FPGAs, NICs, switches, and signal conditioners in complex PCIe topologies. And these proliferating devices bring with them a growing number of possible error conditions, performance anomalies, and root-cause scenarios that can be difficult to isolate when components number in the thousands. Real-time fleet telemetry has never been more vital.


As industry leaders continue to make clear,[an open ecosystem is the only viable path forward](https://www.asteralabs.com/resources/blog/ai-your-way-at-amd-advancing-ai-2026-why-open-standards-power-ai-everywhere/) to deliver AI at a scale that meets its growing demand. Functional telemetry in this environment means standardized observability that plugs directly into OpenBMC-compliant fleet management environments that hyperscalers run today.


At OCP APAC 2026, Astera Labs is bringing that story to the stage. Together with Insyde and ASPEED, we’re highlighting how[Astera Labs’ COSMOS software integration](https://www.asteralabs.com/products/cosmos/) exposes link telemetry, device health, and FW logs, enabling actionable data for[Aries Smart PCIe®/CXL® Retimers](https://www.asteralabs.com/products/pcie-cxl-smart-dsp-retimers/) and[Scorpio™ PCIe Smart Fabric Switches](https://www.asteralabs.com/products/scorpio-smart-fabric-switch/) through OpenBMC and DMTF Redfish® APIs: the same standards already anchoring hyperscale fleet operations across the industry.


## Diagnostics Your Way: Bringing COSMOS Visibility to the Platforms Hyperscalers Use


Our OCP APAC demo brings together hardware and software components from three different providers in a unified out-of-band management flow built for PCIe connectivity devices:


- Astera Labs’ Aries PCIe/CXL Smart Retimers, Scorpio PCIe Smart Fabric Switches, and COSMOS platform
- Insyde’s Supervyse® OpenBMC firmware
- ASPEED’s AST2750 OCP Data Center Secure Control Module (DC-SCM)


ASIC-level COSMOS firmware collects device and link telemetry from Scorpio and Aries. The telemetry is accessed out-of-band through the BMC over I²C/SMBus, transmitted over MCTP/PLDM, and mapped by OpenBMC services into D-Bus objects and Redfish resources. Then, Supervyse OpenBMC, and COSMOS APIs expose that data through Redfish RESTful endpoints using JSON payloads. COSMOS SDK gives engineers a programmatic integration layer for consuming this telemetry and diagnostic data, enabling integration with monitoring systems, automation pipelines, and higher-level orchestration platforms. Within the JSON payload, Astera-specific extensions provide additional fields for deeper diagnostics that are not represented by standard Redfish resources. This is a significant initiative to standardize and expose PCIe telemetry to observability platforms, enabling the QoS and fabric-level visibility hyperscalers need across PCIe and Ethernet infrastructure.


A joint Supervyse x COSMOS user interface brings these capabilities together in an operator-focused workflow. It surfaces device identity, health, firmware, temperature, link status, and selected telemetry views, helping operators discover Aries and Scorpio devices, assess their condition, investigate link or signal-integrity issues, and perform supported management actions through the same BMC-based environment.


This API/UI flow supports three operational layers:


- Device discovery and identification
- Firmware and health status monitoring
- Deeper link and SerDes diagnostics


COSMOS SDK makes these capabilities available programmatically, allowing developers to build workflows for each. It also enables out-of-band management over I2C and BMC infrastructure, telemetry review for a specific retimer or switch, and integration with higher-level dashboards or automation pipelines.


Figure 1: Device Information via Supervyse OPF


In PCIe 5.0 and PCIe 6.0 systems, this level of detail matters most at the SerDes layer, where lane health and low-level link signals can reveal problems long before they turn into downtime, prolonged debugging cycles, or costly deployment delays. With COSMOS, Astera Labs delivers visibility into Aries and Scorpio devices at the device, link, and signal level, giving operators the diagnostics they need to keep high-bandwidth AI systems efficient and reliable at scale.


## Why the Rack-Scale Era Demands Open, Standards-Based RAS Telemetry…and How COSMOS Delivers It


In today’s expansive AI infrastructure ecosystem, hyperscalers want telemetry that fits into the standardized APIs to ingest into business intelligence and fleet management workflows they already have in production, not a new standalone management stack for each vendor layered on top of everything else. The COSMOS integration with Supervyse OPF meets that demand by exposing actionable Aries and Scorpio telemetry through DMTF Redfish APIs that customers can pull directly into their own automation pipelines, monitoring systems, observability, and orchestration platforms.


Figure 2: Talend API Tester for DMTF Redfish APIs (Targeting ASPEED 2750 DC-SCM)


For the AI labs whose operations utilize hyperscale architectures, that translates into rapid deployment of new infrastructure and higher quality of life for support teams responsible for keeping it running. When telemetry flows into the tools and processes operators already use, issues get escalated, correlated, and resolved faster. That’s a meaningful advantage when a single stalled link can idle an entire rack of accelerators.


As systems scale from device-level validation to rack- and fleet-level operations, the joint solution and ecosystem collaborations become more valuable precisely because they deliver persistent, standardized signals across increasingly large and complex deployments, helping AI labs scale compute more efficiently without adding operational friction.


This is the core tension defining reliability, availability, and serviceability (RAS) at rack scale: When the speed of AI operations keeps accelerating, observability only helps if it arrives in real time through channels operators already trust. Open, standards-based telemetry is what lets diagnostic depth keep pace with deployment speed.


## Collaboratively Creating Visibility for AI Your Way


As AI infrastructure scales across the rack and the full data path, unified system observability becomes essential to keeping high-value compute online, performant, and easier to support. With COSMOS, Astera Labs is extending that observability through OpenBMC and DMTF Redfish APIs, so data center operators can see more of the system, respond faster, and integrate telemetry into the tools they already use.


It’s also a partnership story rooted in open collaboration. Together with Insyde, ASPEED, and the broader OCP ecosystem, Astera Labs is helping raise the bar for PCIe observability.[Check out our spec](https://www.asteralabs.com/document-requests/?document=COSMOS%20Rack-Scale%20Software%20Design) to see how COSMOS and DMTF Redfish help enable PCIe observability, and join us in pushing that standard forward together.


**Additional sources:**


- **COSMOS Developer Kit:**[Official product overview](https://www.asteralabs.com/products/cosmos-dev-kit/) covering device control, telemetry, diagnostics, validation, and connectivity infrastructure management.
- **Supervyse OpenBMC Firmware:** Learn more about[Insyde’s Secure, reliable, and extensible systems management firmware](https://www.insyde.com/products/supervyse/) for x86- and Arm-based enterprise, AI, and HPC servers.
- **ASPEED AST2750 Remote Management Server Processor:** A[dual-node-capable baseboard management controller](https://www.aspeedtech.com/server_ast2750/) featuring quad-core Arm Cortex-A35 processing, Arm Cortex-M4 processors, PCIe Gen 4, DDR5 support, and remote management capabilities.
- **DMTF Redfish Fabrics White Paper DSP2066:** Explore[the latest Redfish fabric data model](https://www.dmtf.org/sites/default/files/standards/documents/DSP2066_1.0.0.pdf) for managing fabric topology, endpoints, switches, ports, and more.
