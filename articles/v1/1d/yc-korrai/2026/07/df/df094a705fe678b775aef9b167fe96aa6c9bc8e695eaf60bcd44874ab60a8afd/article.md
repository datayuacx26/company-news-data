---
schema_version: "1.0.0"
document_id: "df094a705fe678b775aef9b167fe96aa6c9bc8e695eaf60bcd44874ab60a8afd"
company_key: "yc-korrai"
company: "KorrAI"
source_id: "yc-korrai-news-import-a686cf049d8d"
canonical_url: "https://www.korrai.com/blog/insar-in-practice-choosing-the-right-data-for-your-project"
published_at: null
first_seen_at: "2026-07-22T01:41:37.374471+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:391a8144ec49422e7fa4ad5c54fcf44b4fc6eec5bf032cfb5fc6e5b877eaab12"
---

# InSAR in Practice: Choosing the Right Data for Your Project

> "As the Chief Scientist of RADARSAT Constellation *Mission in 2019, I led efforts to bring SAR imaging into operational use for landslide, subsidence monitoring in transportation and energy corridors. In practice, the prime pre-requisite is selection of the right acquisition parameters and matching them to site conditions. This guide reflects strategic trade-offs I’ve seen time and again” -Prof Dr Vern Singhroy, Technical Advisor, KorrAI, Chief Scientist, RADARSAT Constellation Mission*


Before satellite-InSAR can tell us whether a bridge is settling, a slope is shifting, or a dam is creeping, there’s one important step: choosing the right InSAR setup for the job.


If you're already familiar with the basics of InSAR: how it tracks ground movement with millimetre precision from space, you’re in a good place to dive into data selection and ideal cases. If not, don’t worry. You don’t need to be a radar expert to follow along. We'll quickly recap what matters, then move on to how to actually put InSAR data to work.


## A Recap of Basics


At its core, InSAR (Interferometric Synthetic Aperture Radar) uses repeated satellite radar images to measure how the Earth’s surface moves over time. It’s highly precise, weather-independent, and can cover large areas remotely. But like any tool, its performance depends on choosing the right combination of satellite, wavelength, imaging strategy, and data interpretation method.


In Part 1: Introduction to InSAR, we explored how InSAR detects ground motion, how viewing geometry affects what it sees, and how we ensure strong, reliable measurements over time, even in complex terrains.


Now, let’s shift gears from “how it works” to “how to use it.”


What kind of satellite data works best in forests versus cities?
How often can the imagery of the inspected area be captured?
And most importantly: How do you know if a solution is right for your asset and risk profile?


Let’s walk through the key decisions that shape a successful InSAR project, starting with one of the most foundational elements: the radar signal itself.


## **Choosing the Right InSAR Data**


Selecting suitable satellite data depends on specific project goals and target types. Some criteria are radar signal frequency (or wavelength), spatial and temporal resolution, and image geometry.


### **Radar Signal Wavelength**


The wavelength of the radar signals being transmitted by the SAR satellite determines how well signals can penetrate vegetation cover and the sensitivity to ground displacement.


SAR Band Table SAR Band Wavelength Best Use Cases Limitations


X-band 3.1 cm (9.6 GHz) Urban infrastructure, mining, landslide detection, precise displacement monitoring High sensitivity to vegetation, poor penetration, decorrelation over time


C-band 5.6 cm (5.4 GHz) Infrastructure monitoring, regional ground deformation, subsidence mapping Moderate penetration through vegetation, susceptible to atmospheric noise


L-band 23.6 cm (1.275 GHz) Dense vegetation, geological faults, permafrost monitoring Lower spatial resolution, raw data may lack high precision for minute-scale deformation


- **X-band** (short wavelength) is ideal for detailed monitoring of infrastructure, but struggles in vegetated areas.
- **C-band** (medium wavelength) balances sensitivity and coverage, suitable for large-scale subsidence studies.
- **L-band (Longer wavelength)** penetrates vegetation, making it useful for remote and forested regions.


**Examples**


- Targeted monitoring of subsidence in mines, dams, other critical assets → X-band (e.g., TerraSAR-X, COSMO-SkyMed)
- Tracking ground motion in remote forests → L-band (e.g., ALOS-2, NISAR)
- Infrastructure risk assessment over large scale → C-band (e.g., Sentinel-1, RADARSAT-2)


### **Temporal and Spatial Resolution Considerations**


An InSAR satellite's temporal resolution refers to how frequently satellite images are captured over the same location. A shorter revisit time improves monitoring of rapid changes.


Satellite Revisit Table Satellite Revisit Time Best for


Sentinel-1 (C-band) 6–12 days Large-scale ground deformation (subsidence, tectonics)


RADARSAT-2 (C-band) 24 days Infrastructure and mining


TerraSAR-X (X-band) 11 days Urban monitoring, tunnels, and dams


COSMO-SkyMed (X-band) 1–16 days Landslide monitoring and infrastructure risk analysis


ALOS-2 (L-band) 14–46 days Forested and rural ground motion tracking


SAOCOM (L-band) 16 days Soil moisture dynamics


When setting up monitoring schedules, one has to consider the following natural influences:


- **Rapidly changing environments (active landslides, tunneling)** → **Higher revisit rates (1–12 days, X-band or Sentinel-1)**
- **Slow-moving subsidence (groundwater extraction)** → **Longer revisit rates (12–46 days, C-band or L-band)**


Similarly, an InSAR sensor's spatial resolution or ground sampling distance affects its ability to detect millimeter-scale displacements. Think of it like pixel size on a digital photo: the finer the pixels, the sharper and more detailed the image.


Resolution Table Resolution Typical Satellite Application


0.5 – 3 m (High) TerraSAR-X, COSMO-SkyMed Small infrastructure deformations (bridges, buildings, tunnels)


10 – 30 m (Moderate) Sentinel-1, RADARSAT-2, SAOCOM Regional-scale monitoring (subsidence, earthquakes, mining)


> 30 m (Low) ALOS-2, NISAR Large-scale terrain shifts (forests, fault lines)


### **Trade-offs Between Data Resolution and Coverage Area**


In simple terms, the higher the InSAR “image” resolution, the lower the coverage area. For instance, the high-resolution X-band frequency can capture detailed structural monitoring, but within a limited field of view. The moderate resolution C-band can balance detail and area coverage, providing medium capabilities for both metrics. While L‑band is best for extensive regional or vegetated settings, it may lose sensitivity to millimeter-scale shifts.


From a technical perspective, this is a function of sensor design and physics. But in practice, coverage limitations are often shaped more by cost than by capability. Higher-resolution data like that from X-band satellites is available, but it's commercially licensed. That means users often have to weigh the level of detail needed against the price of acquiring it.


C-band satellites, such as ESA’s Sentinel-1, provide a more balanced approach: fulfilling resolution, large swath widths, and open data access. This makes them particularly attractive for regional monitoring and broad infrastructure assessments.


Ultimately, while resolution and coverage are physically linked, most end users experience this trade-off through cost. The higher the resolution, the more selective teams must be about which structures need sharper attention within the budget.


## Applications and Use Cases of InSAR


InSAR is the geotechnical engineer’s answer to the adages, “Prevention is better than a cure” and “A stitch in time saves nine”.


It helps mitigate risks by enabling a proactive approach rather than a reactive one. These are some of the industries that could benefit from InSAR:


- [Mining](https://www.korrai.com/solutions/mining-operations) : Tracks slope stability, tailings dam integrity, and groundwater depletion to prevent hazards and environmental damage.
- [Urban Infrastructure](https://www.korrai.com/solutions/critical-infrastructure) : Tracks displacement in bridges, highways, railways, and tunnels, pipelines and impact to nearby residential buildings to prevent structural damage. **‍**
- [Data Centers](https://www.korrai.com/solutions/property-insurance) : Helps conduct candidate location risk profile surveys and continuous monitoring for subsidence. **‍**
- [Property Insurance](https://www.korrai.com/solutions/property-insurance) : Helps insurers assess property risks, improve underwri﻿ting, verify claims **** and evaluate post-disaster damage.


## **Types of InSAR Analyses**


InSAR is often a catch-all for several approaches to analyzing the data. Again, while out of scope for this piece, we plan on covering it in a subsequent article. For now, here are the three most popular methods:


### 1. D-InSAR (Differential InSAR)


This is the most basic form of InSAR. It uses two radar images to measure displacement between acquisition dates.


- Works well in stable, high-coherence areas


Think of it as a “before-and-after snapshot.” Useful for sudden changes (like post-earthquake displacement), but not ideal for long-term monitoring.


### 2. PS-InSAR (Persistent Scatterer InSAR)


This technique identifies stable, long-lived targets, often buildings, rocks, or infrastructure, that consistently reflect radar signals across many acquisitions.


- High precision and minimal noise
- Ideal for urban monitoring and infrastructure


At KorrAI, we use a proprietary variation with advanced data processing techniques, called **UrbanSAR** , specifically optimized for dense urban environments where stable radar targets are abundant. UrbanSAR is trusted by major global clients including AWS, Zurich Insurance, Stantec, and more.


### 3. SBAS (Small Baseline Subset)


SBAS works best in natural terrain or semi-coherent areas, like slopes or rural ground. It compares image pairs that are close together in time and space to reduce noise.


- Better at handling vegetation and topographic effects


Commonly used for landslides, tailings, and soft-soil deformation.


### And Many More…


There are also hybrid and emerging methods, including polarimetric InSAR, interferometric stacking, and machine learning–aided approaches. These are outside the scope of this primer but represent the cutting edge of geospatial monitoring.


## Is InSAR better than Traditional Survey?


Unlike conventional surveying techniques such as leveling, GPS, or LiDAR, InSAR offers several advantages:


- **Large-Scale Coverage:** InSAR can monitor extensive geographic areas from orbit, making it ideal for regional or multi-site assessments.
- **High Sensitivity:** It detects millimeter-scale deformations, often invisible to traditional methods.
- **Cost Efficiency:** With data frequently captured by satellites, InSAR reduces the need for extensive field surveys.
- **Weather Independence:** Radar signals can penetrate clouds and operate day or night, providing reliable data in various environmental conditions.


It’s clear that InSAR is quite an advantage in many aspects compared to traditional surveys.


## **Future of InSAR in Ground Motion Monitoring**


The current advancements in satellite technology are driving InSAR to become the primary method in ground stability monitoring and pushing it toward higher-resolution imaging, AI-driven anomaly detection, and near real-time monitoring, enhancing accuracy, efficiency, and accessibility.


### **Higher-Resolution Satellites**


Next-generation satellites like NISAR (launching soon) and growing private radar constellations (from companies such as ICEYE and Capella Space) offer high-resolution images. Future X-band satellites promise sub-meter spatial resolution, improving infrastructure and vegetation monitoring. Multi-satellite constellations will reduce revisit times to 1–3 days, enabling faster detection of sudden ground shifts. L-band expansion enhances monitoring in vegetated regions, while Ka-band research aims to refine urban analysis.


### **AI-Driven Anomaly Detection & Predictive Modeling**


Machine learning automates anomaly detection, prioritizing high-risk zones. Deep learning models predict subsidence risks, while cloud-based platforms like ours process InSAR data within hours instead of days.


### **Integration with IoT & Ground Sensors**


Combining InSAR with GNSS, inclinometers, and seismic networks improves real-time monitoring of infrastructure and assets. These integrations enhance early warning systems for critical structures. At KorrAI, we’re leading the way by leveraging data from public GNSS stations as well as our on-site GNSS sensors and corner reflectors for calibration. We have written a comprehensive explainer about[our GNSS calibration methodology](https://www.korrai.com/blog/enhancing-insar-accuracy-with-gnss-calibration) for satellite InSAR data.


On-site Corner Reflector with GNSS sensor


## When to Start Incorporating InSAR


Borrowing from finance gurus, “The best time was yesterday, the next best time is now.”


The best time to incorporate InSAR is during the project planning and design stages. Earlier, the better. There is some good news, though; historical InSAR data remains valuable in establishing baselines and historical trends. These can come in very handy.


For example, as we’ve seen here at KorrAI when working with a leading data center operator, it can speed up the site selection process for data center construction by analyzing tens or hundreds of sites in parallel.


Even if InSAR wasn’t incorporated during project planning, it can still be integrated into existing risk management strategies. A retrospective analysis using archived satellite data allows professionals to detect trends, assess ongoing risks, and enhance monitoring without disrupting current operations.


## Final Thoughts


InSAR isn’t a one-size-fits-all tool. But with the right configuration, whether it’s high-resolution X-band for urban monitoring or broad L-band coverage for vegetated slopes, it becomes one of the most powerful technologies available for understanding ground stability.


We’ve walked through how different satellite types, imaging frequencies, revisit intervals, and analysis methods play into tailoring InSAR to specific use cases. From mining and infrastructure to insurance and disaster risk, the key is not just using InSAR, but using the right InSAR.


At KorrAI, we specialize in helping organizations do just that. Whether you’re screening hundreds of assets at once or need precision monitoring for a single critical structure, our end-to-end platform delivers insights you can trust, without needing to be a radar expert.


InSAR may be technical under the hood, but the goal is simple: see the unseen, act early, and prevent the preventable.


> "InSAR *can significantly improve risk monitoring, but only when dataset, processing method, and terrain conditions are aligned. This article equips professionals with the judgement needed to make those parameter choices reliably.” -Prof Dr Vern Singhroy*


If you're ready to bring satellite + AI into your risk strategy, let’s talk.
