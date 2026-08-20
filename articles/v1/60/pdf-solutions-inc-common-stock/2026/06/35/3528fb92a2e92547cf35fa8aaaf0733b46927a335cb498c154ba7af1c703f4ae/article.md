---
schema_version: "1.0.0"
document_id: "3528fb92a2e92547cf35fa8aaaf0733b46927a335cb498c154ba7af1c703f4ae"
company_key: "pdf-solutions-inc-common-stock"
company: "PDF Solutions Inc."
source_id: "pdf-solutions-inc-common-stock-rss-7ebebcf01a1e"
canonical_url: "https://www.pdf.com/cimetrix-equipmentconnect-3-0-is-now-available/"
published_at: "2026-06-09T20:23:43+00:00"
first_seen_at: "2026-07-20T23:23:02.669886+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:ba31bb3d9ac0bffd1d97f1202e492361417156b357fb62e328375afc8b7d8780"
---

# Cimetrix® EquipmentConnect 3.0 Is Now Available

**Categories:**


##### [Equipment Solutions](https://www.pdf.com/category/equipment-solutions/)


**Tags:**


##### [Equipment Connectivity](https://www.pdf.com/tag/equipment-connectivity/)


,


##### [Smart Manufacturing/Industry 4.0](https://www.pdf.com/tag/smart-manufacturing-industry-4-0/)


,


##### [SEMI Standards](https://www.pdf.com/tag/semi-standards/)


,


##### [SECS/GEM](https://www.pdf.com/tag/secs-gem/)


Posted on June 9, 2026


by[PDF Solutions](https://www.pdf.com/author/kdaich/)


## A Simpler Way to Integrate Manufacturing Equipment with SEMI Connectivity Standards


**Cimetrix EquipmentConnect 3.0**


is now available


,


enabling equipment manufacturers to integrate equipment with one unique product that runs on multiple operating systems and supports GEM, GEM300, and Equipment Data Acquisition (EDA/Interface A) factory connectivity.


For teams building or maintaining factory connectivity software, this release is designed to reduce duplicated engineering work, simplify deployment, and lower integration risk during factory acceptance.


## The Challenge: Multiple Connectivity Integrations


Factory connectivity rarely requires a one-protocol solution. Equipment suppliers often need to support different combinations of SECS/GEM, GEM300, and EDA to meet factory acceptance requirements. The challenges include:


- Integrating multiple connectivity standards


- Creating parallel data models


- Requiring protocol-specific expertise


- Supporting multiple operating systems and hardware architectures


### The Impact


More code to maintain, more qualification work, and long development cycles increasing time to market.


### The Solution: EquipmentConnect


Cimetrix EquipmentConnect 3.0 addresses these challenges by allowing equipment suppliers to define the data model once, integrate once, and enable the required protocol combinations through licensing.


Instead of maintaining multiple connectivity implementations, suppliers can ship a single connectivity solution and configure it for each factory deployment, without rebuilding or re-integrating software for every protocol combination.


****


## What’s New in Cimetrix EquipmentConnect 3.0


EquipmentConnect 3.0 expands production support across the major SEMI connectivity standards used in modern equipment integration. Including EDA in the product, brings modern data collection capabilities into the same platform used for GEM and GEM300 connectivity. This creates a single point of integration in your equipment control application.


### Supported Protocols


**SECS/GEM**


- E5, E30, E37, E172, E173


**GEM300**


- E39, E40, E87, E90, E94, E116, E148, E157


**EDA / Interface A (Freeze 2)**


- E120, E125, E128, E132, E134, E138, E164


## Architecture: Built for Reliability and Flexibility


### Out-of-Process Design


EquipmentConnect runs as a


**separate process** rather than inside the equipment control application. Communication is handled through a gRPC API, allowing EquipmentConnect to operate in its own memory space.


- Provides fault isolation between connectivity and equipment control logic.


- Allows multiple applications that implement different features to run simultaneously.


### Data Hub: Define Once, Publish Everywhere


At the center of EquipmentConnect is the


**Data Hub.** Parameters, events, and alarms are defined once in the Data Hub and then published to enable protocols. This eliminates duplication of data definitions, event handling logic, and alarm mappings,


reducing cost, complexity, and deployment effort.


### What Developers Work With:


- **gRPC API:** Use with included protocol buffer definitions, enabling integration from environments such as C++, C#, Python, and any language that supports gRPC.


- **Control Panel GUI:** Interact with the product, test, diagnose, and generate GEM documentation, etc.


- **Built-in validation tools:** Validate SEMI standards compliance using included licenses for EquipmentTest and ECCE Plus.


- **Cross-platform support:** Develop and deploy on


**** Windows and Linux x64 and ARM64.


- **Multi-version support:** Run multiple versions of EquipmentConnect on the same machine for testing, staged rollouts, and controlled upgrades.


- **Sample applications:** Reference pre-built examples to help teams understand implementation patterns, including complex GEM300 scenarios.


## Licensing: Flexibility Without Deployment Complexity


EquipmentConnect 3.0 installs all supported protocols. Individual protocols are enabled through licensing.


Protocols that have been integrated into your equipment control application can be activated as needed in the factory by obtaining a runtime license, so you only pay for what you use.


- One deployment package for multiple customer environments


- No protocol-specific rebuilds


- Ability to enable only the protocols required by each factory


## Static and Dynamic Data Models


EquipmentConnect supports both static and dynamic data model creation, giving equipment suppliers flexibility based on equipment design and runtime behavior.


- Static models are well suited for stable parameter sets and fixed equipment configurations.


- Dynamic models are useful for recipe-driven data, runtime configuration changes, and equipment behavior that varies by process or customer use case.


## Real-World Impact


For teams preparing for customer qualification and factory acceptance, EquipmentConnect 3.0 can help reduce the amount of custom integration work required for each deployment.


- Faster qualification cycles


- Reduced engineering overhead


- Lower integration and maintenance risk


- A more consistent connectivity architecture across customers and platforms


## Getting Started


Existing customers should contact their account representative for access to EquipmentConnect 3.0, sample applications, documentation, and integration support.


For additional information, visit:


[www.pdf.com/products/cimetrix-connectivity-control/equipmentconnect](http://www.pdf.com/products/cimetrix-connectivity-control/equipmentconnect)
