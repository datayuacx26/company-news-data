---
schema_version: "1.0.0"
document_id: "b755500fc0c917a0ec784e9e0a91796d2f389856df81f59d903610277d67d613"
company_key: "synopsys-inc-common-stock"
company: "Synopsys Inc."
source_id: "synopsys-inc-common-stock-news-import-736729784437"
canonical_url: "https://www.synopsys.com/blogs/chip-design/hbm-signal-integrity-after-routing.html"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T10:01:07.732639+00:00"
fetched_at: "2026-08-19T10:01:09.199354+00:00"
content_hash: "sha256:1bb2c689be200c6ef19457e742bcd478b0ba47cf30fa6c82f9cfea2418bf8329"
---

# Stop Finding HBM Signal Integrity Problems After Routing

## Introduction


A PHY-to-memory channel integrates thousands of high-speed signals into a congested bus at near-100% routing utilization. Electrical penalties such as crosstalk, insertion loss, and skew often surface only after extraction, long after the layout is committed. Fixing the shielding pattern or wire pitch means modifying finalized routing. The demo, Full-Flow HBM Channel Prototyping, Implementation, and Analysis with 3DIC Compiler, shows how 3DIC Compiler can characterize the channel, route it, extract it, and let AI tune it, all within a single design environment.


**


## Prototype the cross-section before committing to a single trace


The demo opens in the cross-section view assistant, choosing a routing with shielding template that draws the data lines and shielding across five metal layers as a prototype. The editor lets users change offset distance, route thickness, and signal count, with every change reflected instantly in the preview. Setting net length to 300 microns, the tool generates S-parameter plots through the Synopsys HFSS-IC electromagnetic extraction tool, plotting crosstalk and insertion loss against frequency, and follows with an eye diagram. If the eye is unacceptable, the HBM configuration can be modified and rerun, before any real routing exists.


A routing feasibility study conducted early define optimal wire rules and shielding patterns, so achievable layouts and signal performance can be predicted for a given technology rather than discovered after the fact.


## 45-degree auto-routing on the JEDEC bump pattern


With an acceptable prototype, the demo runs automated die-to-die routing on the standard JEDEC HBM bump pattern on a silicon interposer. Through 3D automatic channel routing, users specify layers (Metal 4 and Metal 2), the HBM routing mode, channel boundaries, a pattern file carrying the constraints, and wire width and spacing. Applying the command creates routes with 45-degree bends. Zooming out shows all channels routed with bumps connected and vias added. Critically, the shielding follows the signals through the 45-degree bends concurrently with the signal-and-shield routing, not a manually created afterward.


A DRC-aware, multi-threaded router handles the angled, multi-stage paths caused by PHY offset placement and automatically connects shielding structures to the power delivery network without manual intervention.


## Extraction that can be inspected before compute cycles are spent


Electromagnetic extraction runs through HFSS-IC, with the task assistant collecting the stack-up file, adaptive frequency, and frequency sweep range. The extraction region is drawn, nets are selected or filtered so the region is created automatically, and VSS ground is set as the reference. The skip solve option builds the project and previews it without running, allowing the layout and ports to be confirmed before compute cycles are spent. Completed runs report worst insertion loss skew and worst near-end and far-end crosstalk, plus a histogram of insertion loss ranges by net count.


## Where AI narrows the solution space


The optimization view frames the trade-off plainly. On one axis, low crosstalk comes with transmission that is too low; on the other, high transmission comes with crosstalk that is too high. The acceptable designs sit in a bounded region between them, and the built-in AI engine searches for the optimum by sweeping wire width, pitch, shield width, and pattern parameters. The runs chart plots worst near-end crosstalk against worst insertion loss with the baseline marked at center, and the table sorts by any parameter. Wider wires reduce resistance, while wider spacing reduces crosstalk, and reserving layers for shielding limits the available routing resources.


## Watch it end to end


Manual routing at thousands of signals and near-100% utilization is difficult, slow, and error-prone, and it gets worse with every added HBM and UCIe channel. This demo shows the alternative in one flow using the Synopsys[3DIC Compiler](https://www.synopsys.com/implementation-and-signoff/3dic-design.html) platform: prototype the cross-section, route with 45-degree shielded traces, extract with HFSS-IC, and let AI find the wire-and-shield configuration that meets both crosstalk and insertion-loss targets.


- [About Synopsys](https://www.synopsys.com/blogs/chip-design/category.about-synopsys.html)
- [Engineering Central](https://www.synopsys.com/blogs/chip-design/category.engineering-central.html)
- [Multi-Die](https://www.synopsys.com/blogs/chip-design/category.multi-die-system.html)
- [Design](https://www.synopsys.com/blogs/chip-design/category.design.html)
- [3DIC Design](https://www.synopsys.com/blogs/chip-design/category.3dic-design.html)
