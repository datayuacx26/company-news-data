---
schema_version: "1.0.0"
document_id: "5b7ca752e6c582351df9f46fd053a77da912b0ab9e98d1e7bd7707e08ce4c090"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2025/11/06/the-crucial-role-of-pnt-and-reality-capture-technologies-in-rebuilding-following-a-natural-disaster/"
published_at: "2025-11-06T20:28:22+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:fe998af516c5d40a4c62447f5e139b018ed1b335cedbea7af81c9a0bb1b0904f"
---

# The Crucial Role of PNT and Reality Capture Technologies in Rebuilding Following a Natural Disaster

**


# The Crucial Role of PNT and Reality Capture Technologies in Rebuilding Following a Natural Disaster


- **William Dillingham
- ** November 6, 2025
- 2


Be it an earthquake, tsunami, flood or wildfire, Positioning, Navigation and Timing (PNT) and reality capture technologies play a vital role in disaster recovery and rebuilding efforts. In this blog, we examine how these can be implemented with a real-world example of how VIAVI’s technology is helping to rebuild following the United States’ worst wildfire in over 100 years.


With climate change, the risk and intensity of flood and wildfire events have increased significantly. Here in the U.S., the Palisades fire in Los Angeles made news earlier this year and other major global fires from this year alone include those in[Israel/the West Bank](https://en.wikipedia.org/wiki/2025_Israel%E2%80%93West_Bank_fires) ,[South Korea](https://atmosphere.copernicus.eu/wildfires-2025-review-asean-reduces-emissions-haze-persists) ,[the Amazon](https://ctif.org/news/wildfires-fires-and-accidents-central-and-south-america-first-few-weeks-2025) and the[Arctic Circle](https://atmosphere.copernicus.eu/wildfires-2025-review-asean-reduces-emissions-haze-persists) .


The U.S.’s most deadly wildfire in over 100 years took place just two years ago, in August 2023, with the Lahaina fire on the island of Maui, Hawaii taking over 100 lives. It is also estimated that over 2,000 structures were destroyed, with overall losses initially projected to be in excess of $6 billion USD.


In this blog we’ll examine how the geospatial data and mapping consultancy firm Basemap Consulting has implemented LiDAR, camera, and PNT data from VIAVI’s Inertial Labs division and[Hexagon │ NovAtel](https://novatel.com/) to help rebuild communities after the Lahaina fire.


### **Mapping the Extent of the Damage**


Following the fire, the pace of recovery has been, to an extent, limited by logistical, regulatory, and practical challenges. Hawaii’s isolation can mean that resources are limited or need to be imported, including building materials, labor, and specific skills.


Resources that can be deployed quickly are a plus and aerial and satellite images are now foundational tools for disaster planning and response. Notably, LiDAR and InSAR (interferometric synthetic aperture radar) are rapidly being adopted for post-event assessments.


One of the key challenges in rebuilding is that there is very little left in the hardest hit areas, meaning property boundaries are hard to identify and fresh surveys are an essential first step. Due to permitting and financing processes, surveying individual properties is not very efficient and is a significant cost burden.


Before the fire, drones were being discussed as a way to boost the efficiency of surveying. After the fire, they became a vital tool in defining the property boundaries, alongside physical features, terrain, structures, utilities, and trees.


### **The Capture**


Capture began in late 2024, once the cleanup of debris in the area was nearly complete and permits for the flights were secured.


The first step was to divide the 1,500 acres (600 hectares) of interest into five subsites, which was undertaken via an imaging flight from a hill overlooking the affected area. Next, Basemap placed several standard aerial targets in each subsite. New and recently repainted street monuments were also repurposed to serve as additional aerial targets.


Tying these images back to precise locations is essential. For this, GNSS data was taken from a newly created reference station on a nearby hill. From this, Basemap augmented this with a portable GNSS base station.


These observation files were then used by Basemap in a post-processed kinematic (PPK) combined with an inertial measurement unit (IMU) step using VIAVI’s Inertial Labs’ PCMasterPro software. This software features[Hexagon │ NovAtel’s Waypoint Inertial Explorer](https://novatel.com/products/waypoint-post-processing-software/inertial-explorer) and provides the spatial fidelity needed by the images and LiDAR point clouds to meet surveying and engineering specifications.


To achieve this, Basemap used a[RESEPI™ Lite (Remote Sensing Payload Instrument)](https://inertiallabs.com/products/resepi-lidar-remote-sensing-payload-instruments/) from Inertial Labs, a VIAVI Solutions Company. This is a combined single-antenna GNSS-aided inertial navigation system with datalogger (with this provided by the OEM7 receiver from Hexagon │ NovAtel), LiDAR, camera, and communications system. For this system, a Kalman filter is also applied to the output before synchronizing with GNSS data.


This Waypoint IE software is fully integrated with third-party IMU data and couples GNSS information from up to 32 bases with inertial data to deliver precise location information even with lower-grade inertial sensors.


### **Ensuring the Tech Stack is Right**


There are three key considerations when creating the technical composition of a drone reality capture system. The first is the laser, with the choice of LiDAR capabilities dictated by the end purpose for which it’s being used. The second consideration is the camera and many drone LiDAR systems – including VIAVI’s Inertial Labs’ offerings – integrate an RGB camera to collect both LiDAR points and high-resolution imagery simultaneously. The photos produced by this can be used not only for traditional photogrammetry, but also for colorizing the LiDAR’s point cloud.


It’s important to note that each sensor’s stated performance is for very specific conditions, meaning consideration in their selection is important to meet these conditions.


The third and most fundamental consideration is the system core, with its onboard processing/algorithms and its PNT technology. Without high-quality PNT systems, it will be impossible to generate the vital laser point clouds or color images, as these require precise information on both the location and orientation of the system.


As such, RESEPI has been created to provide a high-quality core first and is bundled with a range of sensors to meet the multiple needs of different applications, with the technology available in a broad range of payloads and focusing on applications where size, weight, and power (SWaP) criteria are critical to success.


### **Rebuilding**


Lahaina has been a significant Pacific trading hub, the birthplace of kings, and the seat of the Hawaiian Kingdom during the 1800s. It has been, until the fire, seen as a vision of beauty and tranquility. Rebuilding the community is vital, but the pace of this is handicapped by the challenges of being a remote island with few surveyors, and reduced access to building materials, labor, and skills.


The approach taken by Basemap Consulting highlights the important role that using drone-based imaging and LiDAR mapped precisely using PNT networks can play in the rebuilding of a community after a natural disaster.


Using this combination of imaging and LiDAR capture flights, Basemap was able to provide local surveyors and developers with a precise and comprehensive spatial foundation alongside data on terrain, and any existing features that would prove useful for engineering design.


According to Basemap Consulting’s CEO, Daniel Windham, photogrammetric and LiDAR drone capture reduces the project’s work by between *50 and 80%* , versus traditional surveying methods, and most features can be heads-up drafted from images and LiDAR point clouds.


- ** Categories:[Aerospace and Defense](https://blog.viavisolutions.com/category/aerospace-and-defense/)
- ** Tags:[PNT](https://blog.viavisolutions.com/tag/pnt/)


-
-
-
-
-
-
