---
schema_version: "1.0.0"
document_id: "b3bd937564c6b050a3bd5b4c99568f24169e0aa229312c274c232304ec8775f8"
company_key: "semtech-corporation-common-stock"
company: "Semtech Corporation"
source_id: "semtech-corporation-common-stock-rss-bc5ca864e78b"
canonical_url: "https://blog.semtech.com/from-san-diego-to-essen-market-insights-about-iot-and-lorawan-at-e-world-2026-and-distributech-2026"
published_at: "2026-03-18T18:27:51+00:00"
first_seen_at: "2026-07-20T03:32:47.192655+00:00"
fetched_at: "2026-07-28T21:56:54.694470+00:00"
content_hash: "sha256:06ceeb1a1a41f90a0200c7e89c11aecce4426713d59a7a98fc16d51185057492"
---

# From San Diego to Essen: Market Insights about IoT and LoRaWAN® at DTECH 2026 and E-World 2026

The first weeks of February 2026 turned into an intensive transatlantic sprint for anyone following smart utility and IoT space. Within a few days, two of the world's most influential energy and grid technology events opened their doors almost simultaneously:


**DTECH ®** (formerly DISTRIBUTECH


) in San Diego, California (February 2–5) and


**E-World Energy & Water** in Essen, Germany (February 10–12). Walking both exhibition floors, the convergence of themes was striking. AI-powered grid management, advanced metering infrastructure (AMI), cybersecurity, distributed energy resources (DER), and above all, the continued rise of LPWAN connectivity and LoRaWAN® as a disrupting communication standard for utilities and smart infrastructure.


Here are my takeaways from both events.


## E-World 2026, Essen — Europe's Energy Showcase Breaks Records


E-World 2026 set a new attendance and exhibitor record, welcoming


**more than 1,100 companies from 33 countries** across six fully booked exhibition halls. This is a 15% increase over the previous year reflecting the enormous momentum in the European energy transition. The expanded Hall 6 and a revamped Innovation Zone (40% larger than last year) signaled a clear message: *the industry is scaling up, and digital technologies are at its core.*


The dominant themes on the floor were


**AI and digitalization** , grid infrastructure modernization,


[smart metering](https://www.semtech.com/lora/lora-applications/smart-electricity-metering) , hydrogen, and energy storage. But what struck me most as an electronics specialist was the density of IoT connectivity solutions and particularly the prominence of LoRaWAN technology throughout the halls.


## AI and Digital Transformation: Everywhere You Look


Artificial intelligence permeated nearly every booth, from large utilities presenting intelligent load forecasting platforms to start-ups in the Innovation Zone offering edge AI for substation monitoring. Cloud-based IT platforms, digital twins and automated meter data management (MDM) systems dominated the digital infrastructure section. The message was clear: utilities are no longer asking


*whether* to digitalize — they are deep in the


*how* .


## Smart Metering: LoRaWAN Cements Its Position


For those of us tracking LPWAN adoption, E-World 2026 was remarkable. LoRaWAN was woven into the commercial product portfolios of major metering manufacturers and IoT hardware vendors alike.


### Companies exhibiting[LoRa®](https://www.semtech.com/lora) or LoRaWAN products and solutions at E-World 2026:


-


**ZENNER International** :


Showcased the B.One Metering as a Service platform, integrating wireless meter bus (wM-Bus) and LoRaWAN connectivity for water, heat and energy metering. ZENNER highlighted its own LoRaWAN network, one of the world's largest, with coverage in 15 countries and presented LoRaWAN gateways for both self-managed and network-managed deployments.


-


**Alpha-Omega Technology GmbH & Co. KG:** Hosted a large IoT Hub stand, gathering nine partner companies showcasing hands-on LPWAN hardware and solutions (Decent lab, Kerlink, Lobaro, Wika, etc). The stand itself ran live polls detecting any transmission signals in the ISM band — a neat demonstration of the LoRaWAN robustness against interferences in a busy radio environment of exhibition hall. With its “IoT-Shop,” Alpha-Omega operates one of Europe's leading LPWAN IoT online shops.


-


**EBZ:** Exhibited smart electricity meter RD3 integrating into LoRaWAN. Metrologically and functionally identical, the WD3 has a different communication board equipped with wM-Bus and is compliant with OMS specifications aligned with the compact profile TA7 required by BSI. This German manufacturer has already shipped more than 3.5 million meters since 2016.


-


**Sontex:** Presented a portfolio of smart water, heat metering and industrial smoke detectors. With its recent acquisition of Integra Metering, Sontex is a manufacturer of LoRaWAN-based sensors expanding fast for submetering applications.


-


**Netze BW:** A subsidiary of EnBW, one of the largest power utilities in Germany, they operate a LoRaWAN network with more than 600 gateways for smart water, heat metering and for other enterprise applications.


-


m2m Germany GmbH, Actility


and


Pepperl+Fuchs:


Demonstrated LoRaWAN-based solutions for energy monitoring, smart buildings and smart metering. Actility's ThingPark platform provided the network server backbone, while Pepperl+Fuchs presented its WILSEN.node and WILSEN.valve LoRaWAN sensors designed for industrial process environments.


-


Diehl Metering


:


Presented its "Smart Metering Hub" concept, a modular and interoperable IoT communication network architecture supporting multiple radio standards including LoRaWAN. Partner companies Miromico (LPWAN hardware), Sentinum (energy-harvesting IoT sensors on LPWAN) and Stadtwerke Duisburg Metering GmbH (showcasing a large-scale standardized radio network rollout) were co-exhibiting at the same booth.


Beyond these confirmed LoRaWAN exhibitors, the broader smart metering ecosystem at E-World included several major European meter OEMs such as Apator, Axioma, Iskraemeco, and Landis+Gyr.


## Dual Connectivity: The New Baseline in Europe


One of the strongest signals from E-World 2026 is the rapid adoption of


**dual connectivity** architectures in European utility deployments, typically combining LoRaWAN for fixed-network automated meter reading (AMR) with wM-Bus for walk-by or drive-by collection. This answers both regulatory compliance requirements (wM-Bus and OMS are legacy standards in European market) and the operational resilience demands of utilities managing aging infrastructure. Multi-protocol gateways and chips capable of seamlessly switching between LoRaWAN and wM-Bus modes were clearly on the rise.


## Where is the German Market Heading with the SMGW Concept


It was remarkable to see how the German electricity metering market is struggling to adapt and support the intricate architecture of the Smart Meter Gateway (SMGW) that is defined by Bundesamt für Sicherheit in der Informationstechnik (BSI), the Federal agency for IT. In this concept defined more than a decade ago, the SMGW uses a Local Metering Network to collect metering data, verify authenticity and add timestamps based on a unique, centralized time server. This concept creates a high dependency on SMGW as the central and unique architecture equipment: it is a single point of failure (SPOF) with no fallback to collect metering data by any other means. Considering the discussion about expanding this SMGW to embrace smart meters for water, gas, heat, and smart devices for on-demand control and energy response, the resulting complexity and cost of deployment is significant. This is especially true when utilities only demand a daily index value for simple billing purposes.


By comparison, architecture like LoRaWAN that has no risk of SPOF allows redundancy through mesh with self-healing routing, or through multiple gateways capable of receiving uplinks. This is why utilities in other countries have found reliable solutions to deploy massively smart meters.


## DTECH 2026, San Diego: The Pulse of North America's Grid Modernization


Held at the San Diego Convention Center, DTECH 2026 gathered 684 exhibitors and over 18,000 attendees, cementing its position as North America's premier transmission and distribution event. The agenda was dense: grid resilience, DER management, EV infrastructure, advanced metering, cybersecurity, and increasingly IoT and AI analytics for grid operations.


The show was headlined by Itron as the presenting partner, and the presence of San Diego Gas & Electricity as the utility host reflected a strong focus on California’s grid decarbonization and wildfire resilience. But the real action on the floor was around the convergence of IT and OT technologies — the integration of utility operational technology with modern cloud and AI platforms.


### Grid Modernization: AI Meets Physical Infrastructure


The dominant narrative at DTECH 2026 was the acceleration of


**AI-driven grid automation** . Utilities are deploying sensor networks on a large scale and need to process enormous volumes of edge data. Several sessions and booths addressed how AMI 2.0, edge computing and machine learning are being combined to enable predictive maintenance, outage detection and dynamic load management. This creates fertile ground for low-power wide-area connectivity technologies.


### IoT at DTECH 2026


While DTECH's primary audience is the Transmission & Distribution power sector, where Wi-SUN, LTE and proprietary RF have historically dominated, the IoT footprint at the show has been growing steadily as utilities recognize its value for ancillary metering applications, gas and water networks, and smart city infrastructure.


### Companies Exhibiting LoRa or LoRaWAN Products and Solutions at DTECH 2026:


DTECH 2026 also revealed growing interest for


**hybrid LPWAN architectures** in North America for smart utilities. This combines multiple sources of connectivity (RF mesh, LoRaWAN, cellular, Wi-SUN, Sidewalk), reflecting multiple scenarios and use cases.


-


Verizon Wireless


: The kiosk for IoT presented its LoRaWAN-based solutions with sensors for smart building and asset management, positioning the technology alongside its cellular offering in the USA.


-


Semtech:


The creator of LoRa technology was present at the event, reinforcing its position as an ecosystem enabler across the utility sector. Semtech is actively promoting connectivity for smart utilities with its new[AirLink® RX400](https://www.sierrawireless.com/router-solutions/rx400/) router, which supports 5G RedCap,[AirVantage® Smart Connectivity](https://www.sierrawireless.com/iot-services/smart-connectivity/) for cellular and the new[Gen 4 LoRa Plus chips (LR2021)](https://www.semtech.com/products/wireless-rf/lora-plus/lr2021) for smart metering and grid applications. These products emphasize multi-protocol capabilities, combining LoRaWAN with[Fast Long-Range Communication (FLRC)](https://blog.semtech.com/fast-long-range-communication-semtech-lr2021-enables-high-speed-image-transfer-without-compromise) for high peer-to-peer transmission up to 2.6Mbps, and Wi-SUN FAN 1.0, a particularly relevant architecture for AMI networks.


-


Vision Metering:


A smart metering specialist, Vision Metering exhibited its latest meter innovations for utilities with a focus on interoperable communication platforms relevant to North American AMI rollouts.


-


Sagemcom:


A global leader in smart metering, Sagemcom exhibited a portfolio for smart metering applications, including LoRaWAN-capable Siconia, an embedded ultrasonic water meter for utilities and submetering markets.


-


Wasion/Willfar Information Technology Company Ltd.


: Another leader in smart metering, Wasion showcased its AMI solutions for electricity, water and gas, and increasingly integrating LoRaWAN connectivity for specific market segments, particularly water metering.


## Cross-Event Trends: What Both Shows Confirmed


Walking from San Diego to Essen (metaphorically, though the jet lag was real), several themes emerged that are shaping a new smart utility space in 2026:


**1. LoRaWAN has reached mainstream status.** It is no longer an emerging technology at energy and utility trade shows. With 125 million deployed devices globally and an over 25% CAGR announced by the LoRa Alliance ®


at CES 2026 just weeks earlier, the ecosystem is large enough to naturally appear in commercial product catalogs of the world's largest meter manufacturers.


**2. Multi-protocol is a feature, not a compromise.** Both shows reinforced that utilities need connectivity flexibility. Dual LoRaWAN/wM-Bus in Europe and hybrid LoRaWAN/cellular, LoRaWAN/Wi-SUN or LoRaWAN/Sidewalk in North America are becoming the design standard for next-generation AMI infrastructure.


**3. Water and gas metering are accelerating migration from AMR to AMI.** Historically dominated by walk-by/drive-by collection methods using standard wM-Bus in Europe or proprietary protocols in the USA, for AMR, both segments are seeing rapid adoption of LoRaWAN for AMI. This is driven by the maturity of the ecosystem, the cost economics of unlicensed spectrum deployment and the growing availability of certified devices from Tier 1 OEMs. In North America


and


**** Europe, many water and gas utilities are demanding migration from AMR to AMI.


**4. Connectivity infrastructure is entering the utility IoT roadmap.** Today, many utilities use LoRaWAN connectivity services for collecting metering data every day. The old price model of connectivity paid per volume of data is disrupted; the trend is now towards embedding connectivity into the solution price and requiring a global service level agreement (SLA). The reliability of these connectivity services is demonstrated by ZENNER's 10-million-device network in Europe, Netmore’s connectivity services in 15 countries across Europe, USA, Brazil, and Indonesia, and the broader presence of Non-Terrestrial Network (NTN) discussions at both shows. Together, these developments signal that utilities are beginning to plan for truly ubiquitous coverage, including remote assets and rural infrastructure.


## Final Thoughts


E-World 2026 and DTECH 2026 paint a compelling picture: the energy and utility sector is in the midst of a deep digital transformation, and IoT connectivity — LoRaWAN in particular — is playing a structural role. For professionals in electronics, semiconductors and connected devices, these shows confirm that the window for establishing leadership in this ecosystem is closing fast as the market consolidates around proven platforms and large-scale deployments.


The next milestones to watch: European regulatory evolution on AMI mandates, the North American AMI 2.0 procurement wave and the integration of satellite and terrestrial LPWAN into unified network management platforms.


*Stay tuned for more insights from the field.*


*Disclaimer: This article reflects personal observations from visits to E-World 2026 (Essen, Germany, Feb. 10–12, 2026) and DTECH 2026 (San Diego, CA, Feb. 2–5, 2026). Company and product information is based on publicly available exhibitor announcements and press releases.*


*Semtech, the Semtech logo, LoRa®, LoRaWAN®, AirLink®, and AirVantage® are registered trademarks or service marks of Semtech Corporation or its affiliates. Other product or servicers names mentioned herein may be the trademarks of their respective owners.*
