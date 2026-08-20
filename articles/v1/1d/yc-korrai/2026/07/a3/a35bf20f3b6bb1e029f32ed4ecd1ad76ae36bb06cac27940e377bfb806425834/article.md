---
schema_version: "1.0.0"
document_id: "a35bf20f3b6bb1e029f32ed4ecd1ad76ae36bb06cac27940e377bfb806425834"
company_key: "yc-korrai"
company: "KorrAI"
source_id: "yc-korrai-news-import-a686cf049d8d"
canonical_url: "https://www.korrai.com/blog/exploratory-analysis-tunnel-monitoring-dangjin-south-korea"
published_at: null
first_seen_at: "2026-07-22T01:41:37.374471+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:53b14f9eaf63fc8e1f4e5ed4e9b5e1836991adde25c3964b5d21a70c83fc9104"
---

# Exploratory Analysis: Tunnel Construction Monitoring Dangjin, South Korea

### **Overview**


KorrAI conducted an independent analysis using its proprietary[UrbanSAR pipeline](https://www.korrai.com/technology) to monitor ground deformation associated with tunnel construction in Dangjin, South Korea. The site, built on reclaimed coastal land with soft, compressible soils, poses a high risk of subsidence during excavation, Boring machine operations, and other tunneling activities.


Using Persistent Scatterer Interferometry (PSI), UrbanSAR processed a 3-year archive of Sentinel-1 satellite data, detecting millimeter-scale deformation trends across the urban landscape. The analysis produced high-resolution velocity maps and displacement time series, aligned with phases of tunnel construction.


While[previous academic work](https://www.sciencedirect.com/science/article/pii/S0303243422000472) has explored this region, this analysis highlights UrbanSAR’s capabilities for scalable, high-precision deformation monitoring in complex built environments.


**Figure 1 demonstrates the deformation velocity, highlighting that the area of interest (AOI) undergoes substantial subsidence as a result of tunneling activities.**


### **Context**


Dangjin’s coastal industrial zone has undergone significant infrastructure development, including subsurface tunnel construction. Built on variable reclaimed land, the area is particularly vulnerable to settlement from underground works.


Traditional monitoring methods like leveling surveys offer limited coverage and are cost-prohibitive for large urban zones. Satellite-based PSI provides a scalable alternative, capable of tracking deformation at millimeter-level accuracy across dense urban settings. KorrAI’s UrbanSAR platform automates this with a cloud-native PSI-based architecture, processing extensive Sentinel-1 archives to provide accurate deformation insights.


### **Challenge**


The project presented several challenges:


- **Tunnel-Induced Deformation in Reclaimed Soil:** Subsurface activity in soft, layered fill created localized subsidence risks.
- **Construction-Phase Correlation:** Aligning displacement trends with excavation events such as shaft sinking and TBM drilling was essential to interpret cause-and-effect relationships.
- **High-Density Urban Backscatter:** The complex industrial landscape introduced challenges for Persistent Scatterer point selection and coherence preservation.
- **Data Volume:** This study included over 78[Sentinel-1](https://www.esa.int/Applications/Observing_the_Earth/Copernicus/Sentinel-1) images from 2016 to 2019 with precise temporal alignment.


### **Methodology**


Using KorrAI’s UrbanSAR pipeline, a full PSI stack was processed over the Dangjin AOI with the following specifications:


> **Scope:** Focused on a 1.5 km × 1.5 km industrial corridor surrounding the tunnel alignment, including vertical shafts and road sections A–A′ and B–B′.


> **Data:** 78 Sentinel-1B images (descending track) from September 2016 to July 2019.


> **Precision:** Deformation velocity accuracy recorded more than ±1 mm/year


The pipeline automatically extracted and visualized deformation trends along critical sections, identifying Persistent Scatterer points (e.g., PS-A1 to PS-A4, PS-B1 to PS-B5) and quantifying cumulative LOS displacements up to ~20 mm.


### Key Observations


The displacement profiles along both **A–A′** and **B–B′** sections reveal clear subsidence trends during tunnel construction phases.


- **Section A–A′ (along tunnel axis)** : Subsidence began during Shaft-1 excavation and intensified during TBM operation, with cumulative LOS displacement up to −15 mm.
- **Section B–B′ (transverse to tunnel)** : Variability in subsidence was higher. Points closer to the shoreline (PS-B4, PS-B5) showed greater displacement (~−18 mm), likely due to deeper sediments and softer ground.


Together, these patterns confirm both longitudinal and lateral deformation, influenced by tunneling progress and local subsurface conditions.


**Figure 2.a: Deformation Profiles of PS Points Along Profile A-A'**


**Figure 2.b: Deformation Profiles of PS Points Along Profile B-B'**


### **Insights**


The UrbanSAR analysis demonstrated:


- **Correlation with construction** : Clear subsidence trends were observed near **Shaft-1** , particularly during active excavation and TBM boring phases.
- **Recovery Trends:** Cumulative displacement exceeded **15 mm** at specific PS locations, which shows a rebound during groundwater recharging.
- **Multi-directional impact:** Section **B–B′** , though not aligned with the tunnel axis, revealed differential deformation due to lateral variations in soil stratigraphy, validating the need for broader monitoring.


These insights enable city engineers and infrastructure stakeholders to validate structural safety, anticipate risk zones, and implement mitigation before damage escalates.


### **Conclusion**


This applied analysis demonstrates KorrAI’s UrbanSAR in monitoring tunnel-induced ground movement with millimeter-level precision. Tracking millimeter-level deformation across densely built environments, without ground instrumentation, represents a major step forward in infrastructure intelligence. This approach already supports proactive asset[monitoring for transport, utilities, and insurance sectors](https://www.korrai.com/solutions/critical-infrastructure) globally, scaled at different levels.
