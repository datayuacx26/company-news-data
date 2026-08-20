---
schema_version: "1.0.0"
document_id: "f25fb5fe37067f1493968ef764e367f0b3e4f5d35aa674929cf978a90a3a1f8f"
company_key: "yc-korrai"
company: "KorrAI"
source_id: "yc-korrai-news-import-a686cf049d8d"
canonical_url: "https://www.korrai.com/blog/nisar-the-dual-band-revolution-in-ground-motion-monitoring"
published_at: null
first_seen_at: "2026-07-22T01:41:37.374471+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:291be0f9f8585beadc740d2c42f01e32c7775862505e67c3259645e02e2b4f2e"
---

# NISAR: The Dual-Band Revolution in Ground Motion Monitoring

## A Landmark NASA–ISRO handshake in orbit


In 2012, NASA and the Indian Space Research Organisation (ISRO) initiated a conversation around a shared goal: a satellite mission that could measure Earth's surface changes with unprecedented accuracy. It would combine NASA’s L-band radar with ISRO’s S-band system, creating the world’s first dual-frequency Synthetic Aperture Radar (SAR) satellite, capable of scanning forests, glaciers, coasts, cities, and agricultural plains with a deeper, clearer understanding of surface/sub-surface geology and subsidence.


Formal agreements followed quickly. A Technical Assistance Agreement in 2013 laid the groundwork, and by 30 September 2014, the mission was officially greenlit through an Implementing Arrangement signed by NASA Administrator Charles Bolden and ISRO Chairman K. Radhakrishnan. From there, NISAR became a binational engineering challenge: NASA’s Jet Propulsion Laboratory (JPL) would develop the L-band radar and deployable antenna system; ISRO would deliver the S-band radar and launch vehicle.


After over a decade of collaboration, NISAR launched successfully on 30 July 2025 aboard a GSLV-F16 rocket from Satish Dhawan Space Centre, entering a 747 km sun-synchronous orbit. It is now in its 90-day commissioning phase, preparing to deliver open-access radar imagery every 12 days across nearly all of Earth’s land surface.


*GSLV-F16 launches NISAR from Sriharikota, India Image Credits:*[ISRO](https://www.isro.gov.in/media_isro/image/index/Latest/gslvf16/f16img1_250725.jpg)


The significance of NISAR’s dual-band approach isn’t just technological, it’s foundational. It reflects a shift in how Earth observation missions are conceived: no longer just national programs, but international platforms for science, safety, and resilience.


## NISAR’s Engineering Breakthrough


NISAR introduces a set of technical breakthroughs that position it not just as another Earth observation mission, but as a transformational platform for ground deformation monitoring and InSAR science. The integration of dual-band SAR, a novel SweepSAR imaging mode, and enhanced data and orbit calibration systems makes it the most ambitious SAR mission to date.


### Dual-Band SAR Configuration


NISAR is the world’s first Earth observation satellite to carry two radar systems on the same platform:


- NASA’s L-band SAR (1.26 GHz, 24 cm wavelength): Ideal for deep vegetation and soil penetration, suitable for measuring subtle ground deformations across large, rural or forested regions.
- ISRO’s S-band SAR (3.2 GHz, 12 cm wavelength): Delivers high-resolution data for urban and infrastructure-heavy environments.


Both SAR systems are mounted on a modified ISRO I-3K spacecraft bus, which supports a 12-meter deployable mesh reflector, one of the largest ever used for an Earth science mission, extending from a 9-meter boom developed by NASA’s JPL.


This unique configuration allows NISAR to generate rich, complementary SAR datasets optimized for a wide range of terrain and motion types.


### SweepSAR: Imaging Wide and Fine, Without Trade-offs


NISAR also pioneers the use of SweepSAR, a next-generation SAR imaging mode. Unlike conventional SAR systems that use fixed beam positions, SweepSAR employs a one-dimensional phased-array feed that remains stationary during transmission but sweeps across the reflector during reception.


This enables:


- Wide-area imaging up to 242 km swath
- Spatial resolution of 3–10 meters
- Elimination of coverage gaps through interpolated pulse rates


So, NISAR possesses the ability to image wide areas while preserving fine detail, overcoming one of the main trade-offs of traditional SAR systems.


This animation demonstrates one view of how SweepSAR works. The red flash is a transmit event, which uses the full radar feed to create a narrow strip of signal across the 12 m reflector antenna, which is then bounced to illuminate the 242 km ground swath.
[Image Credits: NASA](https://assets.science.nasa.gov/content/dam/science/missions/nisar/nisar-jpl/images/SweepSAR_1.gif?w=960&h=640&fit=clip&crop=faces%2Cfocalpoint)


### Laser Retroreflector Array (LRA) and Orbit Calibration


NISAR also carries a Laser Retroreflector Array (LRA): an arrangement of corner-cube mirrors designed to reflect laser beams directly back to their source, regardless of the incoming angle. Ground-based stations from the International Laser Ranging Service (ILRS) can target NISAR with short-pulse lasers to retrieve ultra-precise orbit position data.


Why it matters:
The LRA serves as an independent ground-truth system for validating and correcting onboard GPS-derived orbit solutions. This is critical for ensuring the millimeter-level accuracy required for time-series interferometry and deformation mapping.


### Mission Parameters and Data Generation


NISAR operates in a 747 km sun-synchronous orbit with a 12-day repeat cycle and dawn-dusk equator crossing times, ideal for consistent shadow conditions and optimal imaging repeatability.


It is equipped with a high-capacity solid-state recorder (SSR) and Ka-band communication system, enabling it to:


- Collect and downlink up to 85 terabytes of data per day
- Detect deformation rates as subtle as 4 mm/year
- Provide free and open data access via: NASA's Alaska Satellite Facility DAAC and ISRO’s Bhoonidhi portal


Most standard data will be made available within 1–2 days of acquisition, with faster turnaround possible for emergency response.


## KorrAI’s Strategy to Utilize NISAR Data


At KorrAI, the arrival of NISAR marks a significant step toward unlocking consistent, high-quality deformation insights in regions that have historically posed challenges for InSAR alone. We have been tackling the challenges with our calibration methodology, local GNSS stations, advanced atmospheric models, and strategic corner reflector placement. But with NISAR's open-access policy and the strength of its L-band penetration capabilities, we can now push the boundaries of ground motion monitoring in areas that have dense vegetation, rugged topography, or low temporal coherence.


### Advanced Fusion and Model Calibration


KorrAI’s platform is designed to ingest and fuse multi-source Earth observation data. With NISAR, we will enhance our processing using:


- GNSS-calibrated atmospheric corrections
- AI-driven filtering and feature detection
- Cross-band validation between L-band and S-band


This enables more resilient deformation detection, improved temporal coherence in harsh conditions, and higher accuracy when validating trends over time.


As NISAR enters full operational service, our systems will be ready to leverage its data streams for **** our **compliance-grade ground motion monitoring solutions** across critical infrastructure and high-risk assets.


## How NISAR Stands Apart: A New Benchmark in Interferometric SAR


While many existing SAR satellites serve InSAR applications, NISAR’s combination of dual-band imaging, wide swath, and fine resolution gives it an edge that no single-frequency mission can match. From scientific precision to real-world utility, NISAR redefines what’s possible in satellite-based ground motion monitoring.


### Comparative Advantages Over Existing SAR Missions


Most current SAR missions operate on a single frequency band, each optimized for specific environments but limited in others:


Satellite Comparison Table Satellite Radar Band Wavelength Resolution Swath Width Revisit Cycle Data Access


Sentinel-1 C-band 5.5 cm 5–20 m 250–400 km 6–12 days (Europe), 12+ globally Open access (Copernicus Hub)


TerraSAR-X X-band 3.1 cm 1–3 m 5–30 km 11 days Commercial (Airbus)


ALOS-2 L-band 24 cm 3–10 m 50–70 km 14–46 days Limited access (JAXA)


NISAR L-band + S-band 24 cm + 12 cm 3–10 m 242 km 12 days (global) Open access (NASA & ISRO)


By contrast, NISAR combines L-band and S-band, providing complementary data that enhances temporal coherence, signal penetration, and detail across different surface types: forested, urban, glacial, agricultural, or coastal.


### Performance in Challenging Environments


**Forests and biomass-heavy terrain:**
Traditional SAR missions (like Sentinel-1 or TerraSAR-X) often experience temporal decorrelation in vegetated regions. This limits their ability to track subtle motion over time. NISAR’s L-band signal penetrates dense foliage and interacts with tree trunks and the ground, maintaining coherence across seasons and climates.


**Snow and ice-covered regions:**
L-band SAR also performs well over dry snow and ice, offering improved signal stability compared to higher-frequency bands that are more sensitive to surface changes and moisture.


**Atmospheric noise and signal decorrelation:**
The combination of S-band’s high surface resolution and L-band’s atmospheric resilience allows users to cross-reference results and reduce atmospheric noise through post-processing algorithms.


### Why It Matters for InSAR Professionals


The implications are direct for InSAR analysis:


- More consistent time-series deformation maps in forested or icy terrain
- Greater confidence in raw InSAR data, with fewer gaps or false signals
- With the right atmospheric models and processing architecture, NISAR will turn out to be a superior support for decisions in infrastructure maintenance, resource monitoring, and hazard response across regions with difficult terrain geometry, vegetation, and snow cover.


## New Possibilities for Ground Motion Monitoring with NISAR


### Monitoring Motion Below the Surface: Biomass and Soil Shifts


Because the L-band SAR interacts with shallow subsurface layers, NISAR can support advanced studies in:


- Soil moisture change and swelling/shrinkage
- Root-zone-level subsidence in agricultural land
- Permafrost and glacial movement monitoring


> A case in point: Our previous work in[Monitoring Public Infrastructure in Permafrost Regions of Northern Canada](https://www.korrai.com/blog/case-study-monitoring-public-infrastructure-in-permafrost-regions-of-northern-canada) could benefit from NISAR’s dual-band data.


Combined with auxiliary ground-truth data, this opens new pathways for **3D environmental modeling and predictive analytics** .


### Quad-Pol Imaging for Richer Surface Characterization


NISAR’s L-band system is **fully polarimetric (quad-pol)** , capable of transmitting and receiving both horizontal and vertical polarizations. This produces four independent signal channels:


Polarization Modes HH (horizontal transmit, horizontal receive) HV VH VV (vertical transmit, vertical receive)


**What it Enables:** While single or dual-polarization SAR provide limited information, quad-pol data provides a much richer, more detailed characterization of the Earth's surface. By analyzing how the signal's polarization changes upon reflection, scientists can infer detailed information about the structure, orientation, and physical properties of the objects on the ground.


### Massive Onboard Storage Means Minimal Gaps


With its high-capacity solid-state recorder (SSR) and Ka-band transmission system, NISAR can collect and downlink 85 TB of data per day, more than any previous Earth observation satellite.


**What it enables:** A promise of a consistent, gap-free, and frequently updated view of Earth’s changes with higher resolution and a wider view. This also empowers NISAR to track:


- **Slow-onset hazards** like subsidence, reservoir compaction, and slope instability
- **Post-event dynamics** after earthquakes, volcanic activity, or infrastructure failures


### NISAR at a Glance: Key Specifications


NISAR Specifications Parameter Specification


Radar Frequencies L-band (1.26 GHz), S-band (3.2 GHz)


Wavelengths L-band: 24 cm, S-band: 12 cm


Orbit Sun-synchronous, 747 km


Repeat Cycle 12 days


Swath Width Up to 242 km


Resolution 3–10 meters


Data Volume 85 TB/day


Data Access Open, via ASF DAAC (NASA) and Bhoonidhi (ISRO)


Antenna Size 12-meter deployable mesh reflector


Pointing Accuracy Enables detection of deformation ≥4 mm/year


Imaging Mode SweepSAR


Calibration GPS + Laser Retroreflector Array (ILRS)


## Concluding Remarks


NISAR represents a pivotal advancement in satellite-based Earth observation, offering a rare combination of dual-frequency SAR, wide-area coverage, and open-access data.


Its technical design addresses some of the most persistent limitations in InSAR-based ground motion monitoring, from coherence loss in forested regions to the trade-off between resolution and coverage. For practitioners working across geotechnical risk, infrastructure resilience, and environmental monitoring, NISAR introduces a new level of reliability and accessibility.


As this mission transitions into full operational service, it sets a new standard for how ground deformation can be observed, understood, and acted upon at both local and continental scales.
