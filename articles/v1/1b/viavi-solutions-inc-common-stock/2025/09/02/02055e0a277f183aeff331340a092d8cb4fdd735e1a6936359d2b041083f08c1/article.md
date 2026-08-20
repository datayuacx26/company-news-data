---
schema_version: "1.0.0"
document_id: "02055e0a277f183aeff331340a092d8cb4fdd735e1a6936359d2b041083f08c1"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2025/09/10/end-to-end-test-is-critical-for-fast-and-feasible-ntn-deployment/"
published_at: "2025-09-10T15:31:08+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:56:00.447075+00:00"
content_hash: "sha256:1ce71c37d09cbc11ef4e5ba16cfaa37c90c968a876aae84032cf4878bafd9c8b"
---

# End-to-End Test is Critical for Fast and Feasible NTN Deployment

**


# End-to-End Test is Critical for Fast and Feasible NTN Deployment


- **Obilor Nwamadi
- ** September 10, 2025
- Like


Starting from summer 2025, if you have one of the latest top-brand LTE smartphones, you can sign up for[direct-to-cell mobile connections to Starlink satellites](https://www.starlink.com/us/business/direct-to-cell?srsltid=AfmBOoofCI70VG8uFcPvrNJLq5jIpqubWweD2QyLMyLh7bFjLwFiCoAg) for messaging using the existing LTE spectrum in areas without terrestrial coverage. While only limited services will initially be available, these base stations in the sky can potentially deliver mobile broadband and other high value 5G services to almost any point on the planet.


This adds to services such as Apple’s satellite-to-phone communication service that was launched in 2022 and used the L- and S-bands to enable emergency texts to be sent from anywhere on the planet.


Non-terrestrial networks (NTNs) delivering broadband from satellites and drones are also set to be a crucial part of 6G and will be incredibly empowering for underserved areas, including remote and rural communities. They can cost-effectively plug gaps in ordinary terrestrial network coverage, boost service delivery to users at cell edges, provide short-term extra capacity when demand is exceptionally high, and enable global continuity of service for IoT applications. In addition, it could be of tremendous help to responders coordinating disaster relief when local infrastructure is unusable.


Successive 3GPP Releases have prepared the ground for NTN within the overall global wireless connectivity strategy, including defining use cases and introducing enhancements and optimizations. To help bring it to reality, there are now ubiquitous and affordable UAVs, established low-cost micro- and pico-satellite platforms, private space-launch services, and powerful technologies like software-defined networking and millimeter-wave (mmWave) knowhow including high-performance antennas.


### **Technical Challenges**


But let’s not underestimate the technical challenges involved in making the infrastructure literally revolve around the subscriber. Non-terrestrial networks are highly interconnected and multi-level, comprising constellations of satellites in low-earth (LEO), mid- (MEO) and geostationary (GEO) orbits. There can also be high-altitude platforms (HAPs) such as airships at stratospheric altitudes of about 20km, and drones (UAVs) at heights of a few hundred meters. These could be deployed individually, in small numbers, or even in swarms, depending on the specific use-case requirements. These vehicles carry the base stations that constitute the access network, connecting to subscriber handsets, to each other, and to the terrestrial core network via a gateway.


Orchestrating these diverse and dynamically moving airborne and space stations is complicated, calling for advanced constellation management with the facility to manage handovers, synchronization and load balancing. Their high speed (relative to user terminals) causes large Doppler shifts and there can be rapid link changes. Additionally, ionospheric interference is more pronounced than in terrestrial networks, and environmental effects like shadowing from terrain or obstructions, and rain fade can also be greater. Furthermore, latency and propagation delays are also increased, and signals in NTNs experience much longer round-trip times (RTT) that can exceed the limits defined in standard protocols such as TCP. Special acceleration and adaptation techniques are therefore needed to maintain high data throughput.


At the mmWave frequencies needed to support high value 5G and 6G broadband services, challenges such as range limitations, atmospheric attenuation, and interference are more severe and demand precise beam alignment using directional antennas. However, maintaining alignment is more difficult and requires sophisticated tracking and error correction methods because the satellites move at high speeds. NTNs must also coexist with pre-existing satellite systems, such as weather satellites, operating in similar mmWave frequency bands.


### **Test as Enablement**


The dynamic and complex nature of NTNs demands new platforms for testing and validation to assess the overall network reliability, stability, and performance before launching satellites or HAPs.


In addition to assessing individual component performance, testing in controlled conditions can check the integration of network components and verify application performance. Further important checks include testing the service and feeder links under simulated high-load conditions and with high UE mobility, as well as testing across GEO, MEO, LEO, and HAPS orbits. Also, by generating realistic RAN scenarios, developers can explore interactions between the NTN, TN and the 5G core and ensure compliance with 3GPP standards.


A suitable test platform should be capable of evaluating end-to-end network performance, including emulating satellite and UE mobility under realistic conditions including terrain, weather, doppler and delay effects. Testing satellite payloads and link budgets, ground stations, and 5G core components is also needed, to support full-stack validation of NTN systems.


The[VIAVI NTN suite](https://www.viavisolutions.com/en-us/literature/non-terrestrial-networks-brochures-en.pdf) , part of the[NITRO Wireless](https://www.google.com/search?q=nitro+wireless&sca_esv=7bf8a2f3baef1f90&ei=1_JwaJLrHZW9hbIPlobXwQw&ved=0ahUKEwjSsuKr17SOAxWVXkEAHRbDNcgQ4dUDCBA&uact=5&oq=nitro+wireless&gs_lp=Egxnd3Mtd2l6LXNlcnAiDm5pdHJvIHdpcmVsZXNzMgUQABiABDIKEAAYgAQYQxiKBTIGEAAYFhgeMgYQABgWGB4yBhAAGBYYHjIGEAAYFhgeMgYQABgWGB4yBhAAGBYYHjIGEAAYFhgeMgYQABgWGB5I7gVQ3gRY3gRwAngBkAEAmAFzoAFzqgEDMC4xuAEDyAEA-AEBmAIDoAJ_wgIKEAAYsAMY1gQYR5gDAIgGAZAGCJIHAzIuMaAHtQayBwMwLjG4B3vCBwUwLjIuMcgHCg&sclient=gws-wiz-serp) product range, fulfills these requirements. The platform enables end-to-end validation of 4G, 5G, and NTN networks using proven equipment like the[TM500 network tester](https://www.viavisolutions.com/en-us/products/tm500-network-tester) . In addition, the VIAVI TeraVM Core Emulator and Core Tester solutions guarantee that the changes to the Core (EPC & 5GC) are effectively tested before deployment. Also of note for enabling testing and modelling is the[TeraVM AI RSG](https://www.viavisolutions.com/en-us/products/teravm-ai-rsg) (RAN Scenario Generator) that emulates traffic scenarios for NOC (Network Operations Centre) optimization and AI model training. In addition, the[VIAVI Automation Management and Orchestration System (VAMOS)](https://www.viavisolutions.com/en-us/products/viavi-automation-management-and-orchestration-system-vamos) makes test cases easily accessible in a single cloud-based platform. VIAVI can also support live deployments by ensuring that the NTN and TN interference is detected and minimised.


### **Ecosystems are Vital**


VIAVI has[worked with Rohde & Schwarz](https://www.viavisolutions.com/en-us/news-releases/rohde-schwarz-and-viavi-solutions-unveil-open-ran-and-non-terrestrial-network-test-solutions) to bring the R&S™ CMX500 signaling tester into the solution, combining it with the TM500 to create PoC testbeds with end-to-end analysis for satellite and network operators. Using these systems, we are also leveraging digital twin technologies, which are especially powerful for NTN testing and modeling. By establishing accurate, constantly updated replicas of the network components and multi-orbit operating environments, it’s possible to simulate orbital dynamics, signal propagation, and handovers between satellites. After deployment, the digital twin can support further experimentation as well as predictive maintenance by using real-time telemetry data to update the virtual model. These capabilities have already been demonstrated in several successful collaborations, including a[live full-HD video transmission](https://www.rohde-schwarz.com/tw/about/news-press/all-news/kt-demonstrates-full-hd-video-transmission-between-geo-satellite-and-ue-with-rohde-schwarz-and-viavi-solutions-press-release-detailpage_229356-1547469.html) with KT SAT, a[5G NTN trial](https://www.satelliteevolution.com/post/kratos-and-intelsat-successfully-demonstrate-5g-ntn-over-geo) with Kratos and Intelsat, and[advanced satellite communications research](https://www.sutd.edu.sg/?post_type=news&p=89507) with SUTD and JSAT.


It’s possible to test the entire system using this suite, from component performance and integration to testing application performance in controlled conditions and verifying compliance with the latest 3GPP standards.


There is no doubt that NTNs have a powerful role to play in the future of 5G and 6G services. They can provide universal coverage, resilience and emergency response, equitable access, and global IoT support that are not economically feasible using terrestrial networks alone. Testing and simulation based on realistic scenarios, provided in the VIAVI NTN test suite, holds the key to removing risks and accelerating deployment of reliable and standards-compliant solutions.


- ** Categories:[Wireless](https://blog.viavisolutions.com/category/wireless/)
- ** Tags:[Non Terrestrial Networks](https://blog.viavisolutions.com/tag/non-terrestrial-networks/) ,[NTN](https://blog.viavisolutions.com/tag/ntn/)


-
-
-
-
-
-
