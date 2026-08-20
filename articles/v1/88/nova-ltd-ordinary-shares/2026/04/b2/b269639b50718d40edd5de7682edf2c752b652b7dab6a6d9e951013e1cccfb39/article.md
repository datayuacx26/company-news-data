---
schema_version: "1.0.0"
document_id: "b269639b50718d40edd5de7682edf2c752b652b7dab6a6d9e951013e1cccfb39"
company_key: "nova-ltd-ordinary-shares"
company: "Nova Ltd."
source_id: "nova-ltd-ordinary-shares-rss-77a6f8548034"
canonical_url: "https://www.novami.com/blog/advancing-3d-integration-and-beol-scaling-novas-metrology-innovations-at-spie-2026/"
published_at: "2026-04-22T02:04:13+00:00"
first_seen_at: "2026-07-20T23:21:33.213138+00:00"
fetched_at: "2026-07-28T21:45:30.754431+00:00"
content_hash: "sha256:8118a82620b284d7b08fb6a8e58688e437f424c3a11c93aa664325a5d3084ef7"
---

# Advancing 3D Integration and BEOL Scaling: Nova’s Metrology Innovations at SPIE 2026

At this year’s SPIE Advanced Lithography + Patterning Conference, Nova and its industry partners showcased new metrology breakthroughs that address some of the most pressing challenges facing semiconductor scaling. From wafer‑level hybrid bonding to BEOL interconnect innovation, process control at the nanoscale has become the key enabler for next‑generation device architectures.


This post highlights three SPIE papers—developed with Micron and IBM—that reflect Nova’s expanding role in shaping the metrology technologies required for 3D integration, Chiplet architecture, and advanced BEOL scaling.


**High‑Resolution Edge Metrology: Solving CMP’s Hardest Problem**


The wafer edge is one of the most challenging regions to control during CMP and one of the most critical for hybrid bonding. Variations in edge topography, thickness roll‑off, and localized defects can lead directly to bonding voids and yield loss. Traditional optical metrology provides sparse sampling in this region, leaving CMP engineers without the visibility needed to control these edge‑driven risks.


**Nova’s Imaging‑Based Innovation**


To overcome these challenges, the team introduced a


**new edge‑imaging‑driven metrology technique** that converts high‑resolution wafer‑edge images into quantitative thickness maps. When combined with Optical CD, this hybrid approach delivers:


- Comprehensive, full‑edge sampling coverage


- Fine spatial granularity along both the radial and azimuth directions


- Detection of layer transitions, macro‑chipping, and peeling


- High‑fidelity thickness extraction aligned with reference measurements


This integrated view allows CMP engineers to access edge‑specific information previously unavailable, enabling stronger control of bonding readiness and more robust process margins.


The paper and related work were co-developed by Nova Ltd. and Micron Technology. Read the full manuscript here:


**Inline Cu Recess Monitoring for Hybrid Bonding**


Hybrid Cu‑Cu bonding demands extremely tight control of copper recess across both top and bottom wafers. Even sub‑angstrom deviations can impact dielectric surface bonding or inhibit the formation of a clean Cu‑Cu interface during annealing. Traditional scatterometry is hindered by stacked underlayers, which make the optical response difficult to interpret, and conventional machine learning models often struggle when the two wafers to be bonded have different layer stacks.


**Vertical Traveling Scatterometry (VTS) to the Rescue**


Vertical Traveling Scatterometry (VTS) isolates the optical contribution from the uppermost stack layers, filtering out signals from buried structures that are typically challenging for traditional models. When combined with AFM‑trained machine learning, VTS delivers a robust inline measurement solution:


- Single ML model supports both top and bottom wafers


- Stability across varying underlayer stacks


- Faster time‑to‑solution for recess characterization during R&D


This unified model approach dramatically simplifies hybrid bonding process control, providing a consistent, scalable method for inline Cu-recess metrology.


The paper and related work were co-developed by Nova Ltd. and IBM.


Read the full manuscript here: https://www.novami.com/publications/inline-monitoring-of-hybrid-bonding-cu-recess-with-vertical-traveling-scatterometry-machine-learning/


**Inline XPS for Area Selective Deposition (ASD) on Patterned Structures**


As interconnects continue to scale, BEOL architects face persistent challenges with electromigration, parasitic capacitance, and edge-placement errors. Area‑Selective ALD (AS‑ALD) has become increasingly important because it provides a bottom‑up, self‑aligned approach to patterning -eliminating redundant lithography and enabling barrier‑less vias, reduced resistance, and atomic‑level thickness control. However, monitoring these selective processes in-line is extremely difficult, especially on patterned structures where raw XPS spectra are heavily influenced by metal line width, pitch, and geometry.


**Advanced XPS Algorithms for Patterned Targets**


Nova and IBM introduced a novel XPS methodology that incorporates feed‑forward structural information and multi‑layer intensity deconvolution, allowing clear separation of SAM inhibitors, TaN barriers, and underlying materials. The approach enables:


- Accurate TaN thickness extraction across varied pitch/linewidth patterns


- Quantification of SAM‑driven selectivity, including degradation after air exposure


- Detection of pattern‑dependent growth variations


- Reliable identification of failure modes across a broad DOE


With these capabilities, XPS becomes a powerful inline tool for controlling selective deposition in advanced BEOL integration.


**Evolving Toward Extended Metrology Models**


The scaling trends reflected in these papers highlight a growing need for extended metrology models - approaches that incorporate multiple measurement types or data sources to reliably address increasingly complex metrology challenges. As device geometries tighten and integration complexity rises, these models help address these challenges by combining complementary signals to extract parameters that are otherwise difficult to measure. The use of machine learning further strengthens this approach by improving how diverse data are correlated and interpreted, enabling solutions that would be hard to achieve with traditional methods alone. This direction will only continue to expand as advanced manufacturing requires deeper insight and higher confidence in every critical measurement.
