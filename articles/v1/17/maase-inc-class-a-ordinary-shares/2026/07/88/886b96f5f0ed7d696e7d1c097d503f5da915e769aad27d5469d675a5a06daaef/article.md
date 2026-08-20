---
schema_version: "1.0.0"
document_id: "886b96f5f0ed7d696e7d1c097d503f5da915e769aad27d5469d675a5a06daaef"
company_key: "maase-inc-class-a-ordinary-shares"
company: "Maase Inc."
source_id: "maase-inc-class-a-ordinary-shares-news-import-94e4621c6a43"
canonical_url: "https://maaseai.com/en/news/mobile-charging-robot-industry-whitepaper-2026"
published_at: null
first_seen_at: "2026-07-25T13:27:39.898411+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:9aeb2213435d6dd54450ecb85703602901a7ca22dfb9dc9d3c179fd2d844e955"
---

# Mobile Charging Robot Industry Whitepaper

## Mobile Charging Robot Industry Whitepaper


Technological Innovation, Market Transformation, and Smart Energy Network Construction


(2026)


Abstract


The profound transformation of the global transportation energy structure is advancing at an unprecedented pace, with the widespread adoption of electric vehicles becoming an irreversible trend. However, the construction and operational models of charging infrastructure are facing multiple extreme challenges, including land resources, grid capacity, investment efficiency, and user experience. The traditional static layout model, centered around fixed charging piles, is increasingly revealing the inherent limitations of its rigid system when confronted with dynamic, imbalanced, and increasingly complex charging demands. The passive "vehicle-finding-charger" model not only leads to resource misallocation and fragmented user experiences but also, on a deeper level, restricts the potential for flexible interaction between electric vehicles and the new power system.


Against this backdrop, Mobile Charging Robots (herein and after as 'MCRs') have emerged. Rather than serving as a mere substitute for fixed charging, they represent a profound paradigm shift. As a product of the deep integration of autonomous driving technology, robotics, power electronics, and the Internet of Things, MCRs liberate charging services from fixed "points" into intelligently dispatchable "flows." This achieves a fundamental transformation from "people/vehicles adapting to infrastructure" to "infrastructure proactively serving people/vehicles." Their strategic value far exceeds the surface-level concept of a "mobile power bank," directly targeting the core of coordinated optimization among energy flows, information flows, and traffic flows in future smart cities.


This whitepaper aims to construct a comprehensive and forward-looking industry landscape. We deconstruct the closed-loop technological chain of MCRs, from perception and decision-making to execution, and analyze their market pathways and competitive landscape as they transition from pilot operations to large-scale commercialization. Particularly important is that this report, taking innovative practices such as "XiaoLi Charging" as key case studies, provides an in-depth analysis of how MCRs transcend their singular charging functionality. It demonstrates their evolution into distributed energy storage nodes, building blocks for regional microgrids, and dynamic energy geo-dispatchers, thereby achieving an elevated dimension in terms of both commercial value and social benefits.


We believe that the industry is currently in a critical calibration phase on the eve of its breakout. While technological feasibility has been preliminarily validated and market pain points are clearly visible, significant challenges such as business models, policy standards, and ecosystem collaboration remain to be addressed. This report calls for a concerted effort from industry players, academia, policymakers, and investors, grounded in systems thinking, to position MCRs as a vital component of the new energy infrastructure.


## Introduction: The Structural Contradictions of the Energy Replenishment System and the Paradigm Shift of MCR


The electrification transformation of the global transportation sector has entered the most difficult part, with the penetration rate of electric vehicles rapidly rising, driven by policy support and technological iteration. However, the development model of charging networks—the infrastructure supporting this transition—is facing increasingly severe structural challenges. The traditional energy replenishment system, primarily based on fixed charging piles, is essentially an extension of the "centralized" supply mindset from the industrial era: electricity flows unidirectionally from the grid to vehicles through fixed "sockets" (charging piles). This model is gradually proving inadequate in the face of massive, decentralized, and spatiotemporally imbalanced charging demands from electric vehicles \[1\].


1. Constraints and Conflicts in Physical Space


In densely populated urban core areas, especially in older residential communities in China built in the last century, insufficient power distribution capacity, complex ownership relationships, and a lack of public space make the installation of fixed charging piles extremely difficult, creating a significant "home charging gap" \[2\]. Even in newly developed areas, charging spaces are often occupied by fuel-powered vehicles, leading to underutilization of valuable charging infrastructure and severe resource misallocation.


2. Grid-Side Load Pressure and Investment Efficiency Issues


The deployment of large-scale, centralized fast-charging piles can cause significant peak load impacts on local power grids, forcing grid companies to undertake expensive capacity expansion and upgrades. However, charging loads exhibit notable "tidal" and "random" characteristics, resulting in low utilization rates of grid assets for most of the time, prolonged investment return cycles, and high overall societal costs \[3\].


3. Fragmented User Experience and Anxiety


Users are forced to adapt to the layout of infrastructure, with "difficulty finding available piles, long waiting times, and cumbersome payment processes" becoming commonplace. Particularly during holidays at expressway service areas, long queues for charging are frequent, severely impacting the long-distance travel experience for electric vehicle users and creating a psychological barrier to widespread adoption \[4\].


The root cause of these contradictions lies in the fact that "energy supply points" (charging piles) in the current system are static and isolated, while "energy demand points" (electric vehicles) are dynamic and networked. To resolve this fundamental mismatch, a new form of infrastructure capable of dynamically bridging the gap between supply and demand must be introduced. MCRs emerge as a disruptive solution under this backdrop. By transforming charging piles from fixed "anchor points" into autonomously mobile "extensions," MCRs achieve three core transformations:


First, the Revolution of Spatial Flexibility. MCRs can actively drive to any parking space, realizing "pile-to-vehicle" service. This completely solves the issues of fixed space binding and occupancy, unlocks the potential charging value of all parking spaces, and greatly enhances the utilization efficiency of spatial resources \[5\].


Second, the Revolution of Temporal Elasticity. As mobile units integrating storage and charging, MCRs themselves act as buffer pools. They can store energy during off-peak grid hours (e.g., at night) and discharge during peak hours or when users need it. This not only mitigates grid impact but also creates additional revenue by participating in demand-side response, transforming charging facilities from pure "cost centers" into potential "profit centers" \[6\].


Third, the Revolution of Information and Synergy. Each MCR is an IoT terminal, it uploads real-time data on its status, location, and energy levels. Through cloud-based intelligent dispatching platforms, multi-robot collaborative operations and global energy optimization can be achieved. Deep integration with smart city management systems and Vehicle-to-Everything (V2X) communication forms a real-time sensing, dynamically responsive, and globally optimized smart energy service network \[7\].


It is crucial to acknowledge that the concept of MCRs was not conceived in isolation. In specialized fields such as precision agriculture, mature technological prototypes have already been developed to address the charging needs of automated agricultural machinery clusters. For instance, Harik et al. designed and implemented an autonomous charging station for agricultural electric vehicles, successfully integrating an omnidirectional mobile platform, a collaborative robotic arm, and vision-guided technology to accomplish automatic plugging and unplugging of charging tasks in unstructured environments \[8\]. This provides a solid technical pathway reference and engineering confidence for the development of MCRs in civilian scenarios.


Thus, the emergence of MCRs signifies that the EV energy replenishment system is transitioning from "Electrification 1.0" (fixed access) to "Intelligence 2.0" (flexible interconnection), serving as a key piece in constructing a new power system and smart cities.


## In-Depth Analysis of the Technological System: Building a Robust, Efficient, and Safe Autonomous Charging System


### 2.1 System Overview: Hardware Integration, Software-Defined Architecture, and Cloud-Edge-Device Collaboration


A MCR system capable of commercial operation is a complex intelligent agent that integrates advanced mechanical, electronic, sensing, computing, and communication technologies. Its architectural design follows the principles of layered decoupling and cloud-edge-device collaboration to ensure system reliability, scalability, and maintainability \[9\].


On the "Device" side, the robot body is the executor in the physical world. Its core components include:


High-Mobility Mobile Chassis:


To navigate flexibly within narrow and crowded parking lots, MCRs commonly employ omnidirectional movement solutions based on Mecanum wheels or omni-wheels. This chassis enables translation in any planar direction and in-place rotation without changing the vehicle orientation, significantly simplifying path planning and parking alignment \[10\]. The chassis must possess sufficient structural strength to carry batteries and charging modules weighing several hundred kilograms and integrate high-precision encoders and IMUs to provide accurate odometry information for motion control.


High-Density Energy Storage and Bidirectional Charging/Discharging System:


This is the "heart" of the MCR. It utilizes battery packs with high energy density and safety, such as Lithium Iron Phosphate or NMC lithium batteries, typically with capacities ranging from 30 to 200 kWh. The core of the power electronics is a bidirectional AC/DC or DC/DC converter, which acts like an intelligent valve. It can efficiently store energy drawn from the grid or photovoltaic systems and, upon command, provide fast charging to electric vehicles in constant power or constant current/voltage modes \[11\]. The system must support a wide output voltage range (e.g., 200-1000V DC) to be compatible with the full spectrum of EVs, from A00-class mini-cars to high-end models with 800V platforms.


Multi-Modal Fusion Perception System:


This serves as the robot's "eyes" and "ears," it is the foundation of autonomy. A typical configuration includes:


Mechanical or Solid-State LiDAR: Used for constructing high-precision environmental maps, enabling simultaneous localization and mapping, and detecting mid-to-long-range obstacles.


High-Resolution Stereo Cameras: Responsible for identifying vehicle models, precisely detecting charging port covers, reading license plates/QR codes, and providing rich semantic information.


Ultrasonic Radar Array: Used for detecting blind spots and preventing collisions at very close distances around the vehicle body.


Inertial Measurement Unit: Fused with wheel speedometer and visual information to provide stable and reliable motion pose estimation, offering redundancy especially during brief sensor failures \[12\].


High-Precision, Compliant Robotic Arm:


This is the key to completing the final "centimeter-level" task. Typically, a 6-DOF or 7-DOF collaborative robot arm with torque-sensing capabilities in its joints is selected, enabling "force-position hybrid control". The end-effector is a composite gripper specifically designed for charging tasks, integrating a vision camera and force sensor. It can perform a series of delicate actions compliantly, such as pressing to open the charging port cover, grasping the charging gun, aligning, and inserting it into the charging interface, while accommodating minor tolerances and mechanical resistance across different vehicle models \[8\].


High-Performance Onboard Computing Platform:


Acting as the "local brain," it processes massive sensor data and runs complex real-time control algorithms. Current mainstream solutions utilize platforms like NVIDIA Jetson AGX Orin, Huawei MDC, or similar-performance domain controllers. These platforms can run multiple deep learning models (e.g., object detection, semantic segmentation) in parallel, operate on the ROS 2 middleware, execute local path planning and obstacle avoidance algorithms, and communicate with various actuators via high-speed buses \[13\].


On the "Edge" side, this refers to edge computing nodes deployed in parking lots or operational areas.


They provide ultra-low latency, high-reliability communication connectivity for MCR clusters via 5G private networks or Wi-Fi 6/7 Mesh networks. Edge servers can handle some computation-intensive tasks, such as multi-robot collaborative path planning within local areas and the updating/distribution of high-definition maps locally. This offloads computational burden from individual robots and improves system responsiveness \[14\].


On the "Cloud" side, the central intelligent operations management platform serves as the system's "command center".


Built on a microservices architecture, it provides a suite of core services:


Resource Scheduling & Task Management Service: Uses operations research optimization algorithms to assign optimal tasks based on global orders, robot status, and real-time parking lot dynamics.


Energy Management & Transaction Service: Aggregates the storage capacity of all MCRs, predicts electricity prices and loads, automatically executes optimal charging/discharging strategies, and can connect to Virtual Power Plant platforms to participate in electricity market transactions.


Digital Twin & Simulation Service: Constructs a 1:1 virtual environment mapping the physical world for testing new algorithms, simulating operational strategies, and personnel training.


Prognostics and Health Management & Maintenance Service: Monitors the status of all core components of the robots in real-time, performs fault prediction and diagnosis, and generates preventive maintenance work orders.


User Interaction & Billing Service: Handles the entire user experience process via Aplications and Mini Programs, including charging reservations, order status updates, payment settlement, etc. \[15\].


(Core Architecture of the Autonomous Charging Process)


### 2.2 Core Key Technology Modules


1) High-Robustness SLAM and Navigation in Dynamic Environments


A parking lot is a typical "challenging environment" characterized by its dynamic, semi-structured nature and frequent weak GNSS signals. The MCR must achieve centimeter-level real-time localization within this setting. The mainstream solution currently employs a tightly coupled SLAM algorithm primarily based on LiDAR, supplemented by visual data. This approach leverages stable geometric features of the environment (e.g., pillars, corners) for matching and localization, while simultaneously using visual information for loop closure detection and semantic constraints to correct accumulated errors \[16\].


When confronted with moving vehicles and pedestrians, the system must employ multi-object tracking algorithms to distinguish static from dynamic obstacles and predict their trajectories. This enables the planning of local paths that are both safe and efficient. In recent years, end-to-end models based on deep learning for environmental understanding and navigation have also begun to be explored, aiming to achieve better generalization capabilities \[17\].


Visual Recognition of Charging Ports and Precise Servo-Controlled Plugging/Unplugging Under Complex Working Conditions


This is widely recognized as a critical challenge within the MCR technology chain, directly determining the success of the user experience. The challenges stem from multiple factors:


There are hundreds of global vehicle models with varying charging port locations (front fender, rear fender, front, rear), shapes (circular, square), and opening mechanisms (manual press, electric pop-out, remote control). Lightning conditions are complicate, scenarios include strong backlighting, shadows, and insufficient illumination at night; and the charging port area may be partially obscured by dirt, snow, or vehicle decals.


(Hierarchical Progressive Strategy Process Core Architecture)


Large-Scale Multi-Agent Collaborative Scheduling and Optimization


Deployments in large transportation hubs or commercial centers may involve clusters of dozens or even hundreds of MCRs. Efficiently scheduling such a cluster presents a complex variant of the classic "Dynamic Vehicle Routing Problem" \[19\].


The cloud-based dispatching center must process a vast number of charging requests in real-time. Each request contains information such as location, required energy, and desired completion time.


The scheduling algorithm must simultaneously consider multiple factors: Robot Status (Position, remaining battery level, health status), Task Attributes (Urgency, value), Environmental Constraints (Road/pathway capacity, location of charging depots) and Grid Signals (Real-time electricity prices). The objective function is typically multi-objective, aiming to minimize the average user waiting time, minimize total robot energy consumption (or idle travel distance), maximize total system throughput, and maximize energy arbitrage revenue. Solving this often requires a combination of methods including operations research optimization (e.g., mixed-integer programming), multi-agent reinforcement learning, and market auction mechanisms \[20\].


Full Lifecycle Functional Safety and Cybersecurity Framework


As an autonomous mobile device sharing space with humans, safety is the paramount design principle for the MCR. A multi-layered defense-in-depth framework encompassing mechanical, electrical, functional safety, and cybersecurity must be established.


Functional Safety (FuSa): Following standards such as ISO 26262 (Road Vehicles - Functional Safety) or IEC 61508 (Functional Safety of Electrical/Electronic/Programmable Electronic Safety-related Systems), conduct Hazard Analysis and Risk Assessment (HARA) for the system, define safety goals and their corresponding Automotive Safety Integrity Levels (ASIL). Implement safety mechanisms at both hardware and software levels, such as redundant drive systems, a safety controller (monitoring the main controller state), and an independent safety loop emergency stop button \[21\].


Safety of the Intended Functionality (SOTIF): Address hazards arising from performance limitations (e.g., sensor failure in extreme weather) or misuse. This requires extensive scenario testing, simulation, and verification to mitigate risks. Build a rich library of "corner case" scenarios and continuously iterate and upgrade perception and decision-making algorithms \[22\].


Cybersecurity: Adhere to standards like ISO/SAE 21434 (Road Vehicles - Cybersecurity Engineering). Prevent risks such as remote hijacking of the vehicle, user data leakage, and malicious interference with the charging process. Measures include secure bootloaders, encrypted communication links (e.g., TLS), signed firmware Over-The-Air (OTA) updates, strict access control, and continuous vulnerability monitoring and response mechanisms \[23\].


### 2.3 Bridging the Reality Gap: Safety, Reliability, Standardization, and Cost Control


Despite a clear technological roadmap, bridging several key "reality gaps" is essential to move MCRs from labs and pilot projects to large-scale commercial deployment:


The Reliability Gap


Commercial operation demands an extremely high success rate (e.g., >99.5%) for the MCR's fully automated process (from receiving an order to completing charging and returning). This necessitates overcoming a vast number of "long-tail" problems, such as identifying charging ports heavily covered in mud, operating stably in heavy rain, handling non-standard vehicle modifications, and dealing with parking lot floor reflections or extreme light/shadow interference \[24\]. This requires collecting and annotating massive amounts of real-world data covering various extreme conditions and continuously iterating algorithmic models.


The Standardization Gap


The current lack of unified industry standards is a major bottleneck hindering development. Urgently needed standards include:


Physical Interface Auxiliary Standards: Should standardized visual guidance marker mounting positions or physical guiding structures near the charging port on new energy vehicles be mandated or recommended? This would significantly reduce the difficulty and cost of robot recognition and alignment.


Communication Protocol Standards: How should robots perform low-level communication with vehicles? Beyond national charging protocols (e.g., GB/T 27930) for controlling the charging process, is there a need to define a dedicated service discovery and handshake protocol for autonomous charging (e.g., vehicles broadcasting their charging port type, positional offset, and cover opening commands)?


Data Interface and Service Standards: Should parking lot map data formats, robot status reporting formats, and cloud dispatching interfaces be unified? Standardization would break vendor lock-in and foster ecosystem prosperity and interoperability \[25\].


## Comprehensive Market Overview: Scale Evolution, Scenario Deepening, and Ecosystem Competition


### 3.1 Global and Chinese Markets: Scale Projections and Quantitative Analysis of Driving Factors


The significant gap in the EV energy replenishment market presents vast growth potential for MCRs. BloombergNEF predicts that the global electric vehicle fleet will exceed 300 million units by 2030 \[28\]. This will be accompanied by explosive growth in charging demand. However, the construction speed of fixed charging piles struggles to keep pace and suffers from structural imbalances. Synthesizing research from multiple consultancies, the market potential of mobile charging as a crucial flexible supplement is being reassessed.


Market Size Projection


We have developed a disaggregated forecasting model. Let's take it first from demand side, assuming that by 2030, 5% of the daily EV charging demand globally (including scenarios such as older residential areas, emergency charging, peak demand management, and specific commercial applications) is more suitable or must be met by mobile charging services. Now look at the supply side, factors considered include the daily service capacity per MCR, unit equipment price, service fee premium, and revenue from energy storage value-added services.


Model calculations indicate that by 2030, the total annual market size for the global mobile charging robot market (including equipment sales, leasing revenue, and operational service fees) is projected to reach a range of 800 to 1000 billion RMB \[29, 30\]. Given China's rapid EV adoption, most complex application scenarios, and strong policy support, it is expected to account for 40%-50% of the global market share, becoming the global hub of innovation and the largest single market \[31\].


In-Depth Analysis of Core Driving Factors


EV Fleet Base and the Vehicle-to-Pile Ratio Contradiction


China's EV fleet is projected to exceed 100 million units by 2030 \[32\]. Despite rapid growth in public charging piles, the contradiction of "more vehicles than piles," especially "insufficient fast-charging piles," will persist long-term. The public vehicle-to-pile ratio currently remains above 7:1, and the uneven distribution of high-quality fast-charging piles creates a rigid space for MCR substitution and supplementation \[33\].


Demand Elasticity in Extreme Scenarios


During major holidays like Chinese New Year and National Day, peak charging demand at expressway service areas can surge to over 10 times the normal level, far exceeding the capacity of fixed piles. MCRs can be rapidly deployed as "mobile charging fleets," where their social and economic value is most pronounced in such "elastic peak" scenarios \[34\].


Distributed Energy and Power Market Reform


Globally, power systems are transitioning towards more distributed, market-oriented, and digitalized models. China is actively promoting the development of Virtual Power Plants and pilot spot electricity markets. As high-quality distributed flexible resources, MCR clusters are finding commercial pathways to participate in demand response, frequency regulation ancillary services, and peak-valley arbitrage, creating a potentially massive second revenue stream beyond charging service fees \[35\].


Technology Cost Reduction Curve


The costs of key components like LiDAR, power batteries, and AI chips are following a trajectory similar to Moore's Law. The costs of core MCR components are expected to decrease by 30%-50% over the next 3-5 years, significantly lowering the barrier to commercialization \[36\].


### 3.2 Application Scenario Spectrum: Value Penetration from "Emergency Relief" to "Value-Added Enhancement"


The value proposition of MCRs is not uniformly distributed but follows a tiered penetration logic across different scenarios, which can be summarized into three value levels:


Level 1: Emergency Relief (Addressing Core Pain Points, with Strong Substitutability)


Older Residential Communities & Scenarios without Fixed Parking


This represents the "livelihood project" with the highest social value for MCRs. A large number of residential areas built in China last century lack the physical space and power capacity for widespread fixed pile installation. MCRs can function like "mobile power banks," providing "doorstep charging" services for EV owners in these communities, addressing the essential needs of tens of millions of owners with a vast and stable market \[2\].


Emergency Charging on Highways During Holidays


When service area charging piles are fully saturated, operators can quickly deploy MCR clusters in safe peripheral areas to provide emergency charging. This not only alleviates congestion but also boosts public confidence in EVs for long-distance travel, offering significant social benefits and brand value \[34\].


On-Site Stranded Vehicle Rescue


Replacing traditional towing services with more efficient (charging on-site) and lower-cost (no towing required) "mobile power delivery" rescue services. Partnerships with insurers, OEMs, and mobility platforms can establish this as a standardized emergency solution.


Level 2: Value-Added Enhancement (Improving Service Experience, Creating Additional Revenue)


High-End Commercial Complexes, Five-Star Hotels, Premium Office Buildings


Offering MCRs as a value-added service, providing a "valet charging" experience for customers or corporate members. Users place orders via an app while shopping or in meetings, and the robot completes the charging without user presence, significantly elevating the service level and customer stickiness of the venue, allowing for strong service fee premiums.


Corporate Campuses & R&D Bases


To address issues of insufficient employee charging spots and chaotic management. Companies can offer MCR services as an employee benefit or use smart dispatching to optimize internal charging resource allocation.


Large Temporary Events (Conferences, Sports Events, Music Festivals)


These events generate explosive, temporary charging demand around the venue. Deploying MCRs is the most flexible and economical solution, avoiding the need for permanent electrical modifications.


Level 3: Ecosystem Enablement (Defining Future Form, Building New Infrastructure)


Autonomous Vehicle (Robo-taxi/Robo-truck) Depots


This is the "ultimate scenario" for MCRs. Fully unmanned autonomous fleets requiring 24/7 operation necessitate fully automated energy replenishment. MCRs are essential infrastructure for seamless integration in this scenario, deeply integrated with the autonomous vehicle dispatching system \[37\].


Integrated "Solar-Storage-Charge-Discharge" Smart Energy Stations


MCRs, as mobile storage units, can interact with on-site rooftop photovoltaics, stationary battery storage, and the grid. They store energy during peak solar generation and discharge during high-price periods or when the grid requires support, dynamically optimizing the operation of the local microgrid for energy self-sufficiency and maximized revenue \[35\].


Key Nodes in City-Scale Virtual Power Plants


When thousands of MCRs are distributed across a city and aggregated via a cloud platform, they form a massive virtual power plant with adjustable capacity and rapid response. This "virtual power plant" can participate in all ancillary service markets of the power system, such as peak shaving, frequency regulation, and reserve capacity, becoming a vital component of the new power system \[38\].


### 3.3 Industry Chain Deconstruction and Competitive Landscape: Strategic Games and Alliances Among Five Key Players


The MCR industry chain is long and characterized by cross-industry convergence. The upstream consists of core component suppliers, the midstream includes complete machine manufacturers and solution providers, and the downstream comprises operators and scenario owners. The current competitive and investment focus is highly concentrated in the midstream, which presents a dynamic landscape with "five key players" vying for position.


Charging Equipment / Energy Giants Camp (Representatives: Yijiadian/Guoxuan High-Tech, Zhongneng Congcong/Zhongneng Electric, Zhida Tech)


Core Strengths: Deep industry accumulation and understanding of charging modules, energy storage batteries, power electronics, and charging operations. Strong supply chain integration and cost control advantages. Close relationships with grid companies and energy enterprises, facilitating entry into the energy storage operation market.


Typical Strategy: Often start with "remote-controlled" or "semi-automated" products (e.g., Yijiadian's first-generation product) that have relatively lower technical barriers but can quickly solve practical problems, aiming for rapid scale-up and market share capture. Leveraging their energy background, they actively explore "equipment + energy storage operation" business models, positioning MCRs as part of their integrated energy services.


Challenges: May lack inherent expertise in complex robotic motion control and AI algorithms, potentially limiting the ceiling for product intelligence and automation experience.


Professional Robotics / Special Vehicle Camp (Representatives: Yijiahe, Haihong Technology, Shitu Tech)


Core Strengths: Years of experience in robot body design, manufacturing, system integration, and field engineering deployment. Solid foundation in mechanical structure, motion control, and reliability engineering. Some companies (e.g., Yijiahe) have adopted a unique "overhead rail" technical path, cleverly avoiding complex ground-based omnidirectional navigation and achieving high reliability and coverage in specific scenarios (new parking garages).


Typical Strategy: Product design emphasizes industrial-grade robustness and practicality, focusing on reliability (uptime) and task completion rates in real-world environments. They often pursue project-based sales and deployments through deep partnerships with large property management firms and real estate developers.


Challenges: The "rail-based" approach is scenario-limited; the "wheeled" approach requires strengthening software algorithms and AI capabilities. Their business model may lean heavily towards hardware sales, with less exploration into energy operations.


Autonomous Driving Algorithm / Tech Company Camp (Representatives: Cancong Robotics/Zongmu Technology, Xingshen Intelligence)


Core Strengths: Core teams originate from the autonomous driving field, possessing deep expertise in core algorithms like SLAM, multi-sensor fusion, path planning, and decision control. Strong software-defined capabilities; products are typically high-end, pursuing a fully automated, closed-loop experience. Have natural synergy potential with autonomous vehicle OEMs.


Typical Strategy: Build technologically benchmark products, emphasizing a "fully unmanned" closed loop. They not only sell hardware but are more inclined towards providing "Robot-as-a-Service" operational models or deep revenue-sharing partnerships with scenario owners. Actively collaborate with automakers and tech companies to position themselves within the future autonomous driving ecosystem.


Challenges: Hardware integration, supply chain management, mass production, and cost control are relatively weaker areas. They need to find reliable manufacturing partners or acquire capabilities through investment/M&A.


4. Venture-Backed New Forces Camp (Representative: DHForce)


Core Strengths: Agile mechanisms, no historical baggage, willing to undertake disruptive product definition and innovation. For example, DHForce's G30 model, which features a standing driver position, supports both automatic and manual modes, expanding its application scenarios. It possesses sharp market acumen and strong marketing capabilities.


Typical Strategy: Identify differentiated niche markets, rapidly launch a Minimum Viable Product, and enter the market through flexible business models (e.g., time-sharing rentals, cooperative operations), refining the product through iteration. Skilled at leveraging capital for rapid expansion.


Challenges: Face tests in technological accumulation, supply chain stability, and long-term R&D investment capabilities. In an ecosystem surrounded by giants, they need to quickly establish their own moats.


Automakers & Cross-Border Tech Giants Camp (Representatives: BYD, Huawei, Volkswagen)


Core Strengths: Possess strong brand influence, a massive user base, deep financial and technological reserves, and top-level design capabilities for future mobility ecosystems. Their goal is often not to directly manufacture robots, but to define scenarios, set standards, and control access points.


Typical Strategy: Pursue a multi-pronged approach of in-house R&D, investment, and partnerships. For instance, BYD is co-developing a mobile charging vehicle with Zhongneng Congcong, aiming to enhance its integrated "vehicle models + charging service" solution. Huawei has filed related patents, potentially adding leverage to its Intelligent Automotive Solution or digital energy business. They often lock in excellent third-party solutions through investment or strategic cooperation, integrating them into their own ecosystem.


Challenges: Internal decision-making chains can be lengthy, potentially making them less responsive to emerging markets compared to startups.


Competitive Landscape Outlook: In the short term, the market will present a diverse landscape with "multiple technology paths progressing in parallel and various business models being explored." In the medium to long term, competition will evolve into a battle for specific "ecological niches" rather than mere product competition. Collaboration will outweigh competition, and we anticipate more cross-industry alliances: algorithm companies + contract manufacturers, charging operators + robotics companies, automakers + solution providers. Ultimately, companies that can successfully build integrated capabilities spanning "hardware + software + network + operations" and establish barriers in specific scenarios or energy operations are poised to become industry leaders.


### 3.4 Business Model Analysis: Profit Leap from "Charging Service Fees" to "Energy Asset Operations"


The high initial investment cost of MCRs dictates that relying solely on a single charging service fee model makes it difficult to achieve a healthy return on investment. The core of their commercial success lies in whether the expensive "robot equipment" can be successfully operated as a "smart energy asset" capable of generating multi-dimensional cash flows. Its business model is a value pyramid with layers building upon each other:


Foundation: Basic Charging Service Revenue


This is the most direct and stable source of cash flow, but with limited gross margins. It mainly includes:


Electricity Price Spread: Purchasing electricity from the grid at commercial/industrial or residential tariffs and selling it at market-oriented charging prices (typically referencing nearby fixed fast-charging pile prices).


Charging Service Fee: An additional service fee charged to users. It can be on par with fixed piles during off-peak hours but can command a significant premium during peak hours, in emergency scenarios, or when offering premium services like valet parking/charging—this is key to improving the gross margin of this revenue stream \[39\].


Membership/Package Revenue: Offering monthly or annual membership services to lock in long-term users.


Main Body: Energy Storage-Side Operations and Value-Added Revenue


This is the core for achieving the profit model leap and creating a gap in profitability compared to fixed piles. The characteristic of MCRs as distributed energy storage fully releases value here:


Peak-Valley Price Arbitrage: Currently the most mature and direct business model. Charging during nighttime off-peak hours (e.g., ~0.3 RMB/kWh) and discharging to charge vehicles or supply nearby commercial buildings during daytime peak hours (e.g., 1.0-1.2 RMB/kWh) to earn the price difference. Theoretical daily arbitrage for a 60kWh MCR completing one full cycle can reach several tens of RMB \[35\].


Grid Demand Response (DR) Revenue: Signing agreements with grid companies or load aggregators to reduce charging power or discharge back to the grid upon command during grid stress, thereby receiving subsidies or market-based compensation. In some pilot areas, DR subsidies can reach 2-4 RMB/kW per event.


User-Side Capacity Charge Management: Helping commercial/industrial users like malls or factories reduce their maximum demand (kW) by discharging MCRs during their peak usage hours, thereby lowering their monthly capacity (basic) electricity charges, and sharing the savings with the user.


Virtual Power Plant Aggregation Revenue: As aggregated units within a VPP, participating in electricity spot markets and frequency regulation ancillary service markets to obtain higher market-based revenue. This is the most promising future income source \[38\].


Apex: Solution and Data Value Revenue


Hardware Sales and Leasing: Directly selling or leasing MCR equipment to operators, property management companies, or automakers.


Software Platform and Service Subscription Fees: Providing intelligent dispatching systems, Energy Management Systems (EMS), and operations/maintenance management platforms to operators or site owners via software licensing or annual/monthly subscription services.


Data Value-Added Services: Under strict anonymization and regulatory compliance, the accumulated data—charging behavior, vehicle status, urban energy demand heat maps—holds immense value. It can provide data insights for urban planning, grid investment, automaker R&D, insurance actuarial services, etc.


3. Policy Incentive Sector:


Actively pursue national and local industry support policies, including:


Charging Infrastructure Construction/Operation Subsidies: e.g., Beijing's monthly deployment rewards for eligible mobile charging facilities connected to the regulatory platform \[40\].


Energy Storage Facility Investment Subsidies: e.g., Hefei City's one-time subsidy of up to 400 RMB/kWh for mobile charging facilities meeting technical criteria \[41\].


Future Potential Carbon Inclusive Benefits: By displacing ICE vehicle refueling and increasing green electricity consumption, they may accumulate carbon credits for participation in carbon trading.


Key Formula for Commercial Success: Total Daily Revenue per MCR = Basic Charging Revenue + Energy Storage Operation Revenue + (Amortized Solution Revenue) + (Amortized Policy Subsidies). The core task for operators is to maximize the proportion of the latter three revenue streams, especially energy storage operation revenue, through superior dispatching algorithms and operational strategies. This can shorten the investment payback period from an unacceptably long cycle to a commercially viable range of 3-5 years.


## Innovation Paradigm and Value Elevation: The Smart Energy Network Practice Centered on the Xiaoli Charging System


While most industry participants remain focused on optimizing the mobility and plugging/unplugging performance of individual robots, a few innovators represented by "Xiaoli Charging" have begun redefining the value of MCRs from a systems perspective. Their practice reveals that the ultimate form of an MCR is not a smarter "charging robot," but a programmable, dispatchable, and networkable physical energy node—a fundamental building block for constructing future smart energy networks. Their innovation paradigm is reflected across four dimensions:


### 4.1 Distributed Energy Storage Node: Activating "Dormant" Power Resources in Commercial and Community Sectors


Core Concept and Industry Pain Point:


Traditional commercial and industrial energy storage faces the dilemma of "diseconomies of scale." A medium-sized city may have over 5,000 small and medium-sized commercial establishments (restaurants, retail, hotels, etc.) whose electricity load profiles exhibit significant "midday peaks" and "evening peaks," with peak-to-valley differential rates commonly exceeding 50% \[42\]. However, asking individual small businesses to independently invest several hundred thousand RMB in a static energy storage system results in a long payback period (often over 8 years) and complex daily operation and maintenance, making it highly unfeasible. Consequently, vast amounts of dispersed, adjustable power resources remain "dormant."


Paradigm Breakthrough of the Xiaoli Solution:


The Xiaoli charging system does not view MCRs as isolated charging terminals but constructs them as a "cloud-based shared energy storage resource pool." Its cloud-based intelligent Energy Management System (AI-EMS) acts as a "virtual energy aggregator." This system dynamically integrates multi-dimensional information, including real-time electricity price signals, load forecasts for various commercial entities, robot location and state of charge, and grid dispatch requirements.


Specific Operation Mode and Quantified Value:


Off-Peak Energy Storage: During the grid's deep off-peak hours (e.g., 00:00-08:00), the AI-EMS dispatches Xiaoli robots across the city to charge at their respective depots or charging spots at the lowest electricity price (e.g., 0.3 RMB/kWh).


Peak Dispatch and Value Realization:


Scenario A (Commercial Peak Shaving): During commercial peak hours at noon (11:00-13:00) and evening (17:00-21:00) when prices surge (e.g., 1.2 RMB/kWh), the AI-EMS dispatches fully charged robots near commercial establishments in need, either supplying power directly to them (V2B) or charging EVs in their parking lots. This helps businesses reduce their power consumption and electricity costs during peak periods. Merchants require no upfront investment; they simply sign an agreement with the operator and share the saved electricity costs proportionally.


Scenario B (Grid Demand Response): Upon receiving a peak-shaving instruction from the grid, the AI-EMS can dispatch part of the robot cluster to reduce charging or discharge back to the grid at the specified time, earning grid subsidies.


Value Creation Calculation:


Take a medium-sized restaurant with a daily electricity consumption of 300 kWh as an example. Its peak-hour electricity price is approximately 0.9 RMB/kWh higher than the off-peak price. By receiving 50 kWh of power support from a Xiaoli robot during peak hours, the restaurant can save 45 RMB per day, amounting to about 16,000 RMB annually. For the operator, through peak-valley price arbitrage, a single robot can generate 40-60 RMB daily in energy storage revenue. Models show that in a scaled operational network, energy storage-side revenue can stably contribute 40%-60% of a single robot's total income, fundamentally reconstructing the MCR's profit model from a "costly charging tool" into an "energy asset generating stable cash flow" \[43\]. This not only solves MCRs' own commercialization challenges but also, with very low marginal cost, activates the potential for massive numbers of small and medium-sized businesses to participate in the energy internet.


### 4.2 Regional Resilient Microgrids: Enabling Energy Autonomy in Rural Areas, Scenic Spots, and Remote Scenarios


Core Concept and Scenario Deepening:


In areas with relatively weak grid infrastructure, such as rural villages, islands, remote scenic spots, or new development zones, power supply reliability and quality are often bottlenecks for development. Extending traditional grid lines or building fixed energy storage power stations requires significant investment and long cycles. Furthermore, the intermittency of renewable energy (e.g., photovoltaics) exacerbates supply instability.


Architectural Innovation of the Xiaoli System:


Xiaoli proposes and practices the concept of building a "mobile virtual microgrid". By deploying a certain number of MCRs along with their accompanying lightweight depots (capable of grid-connected/islanded switching) at key nodes within a region (e.g., township centers, scenic spot parking lots, village committees, communication base stations), these nodes are connected into an organic whole via the cloud AI-EMS.


Normal State Collaboration: During sufficient solar generation, robots at each node prioritize charging from local photovoltaics; at night or without sunlight, they replenish from the main grid. Based on load forecasts for each node, the AI-EMS dynamically dispatches robots to transfer small amounts of energy between nodes, achieving local balance.


Emergency Support: When the main grid fails or local PV output drops sharply, the system enters islanded operation mode. The AI-EMS rapidly replans, dispatching robots with sufficient charge from neighboring nodes to converge on critical load areas (e.g., clinics, emergency command centers), forming a temporary power supply network to ensure basic electricity, reducing outage impact from hours to minutes.


Strategic Value Manifestation:


This model elevates MCRs from being urban consumer products to becoming resilient infrastructure supporting national rural revitalization and new urbanization strategies. It removes the "charging difficulty" concern hindering the promotion of new energy vehicles in rural areas, provides stable energy guarantees for developing characteristic industries like tourism in remote regions, and significantly enhances local energy self-sufficiency and risk resilience \[44\]. Xiaoli's practice demonstrates that MCRs can serve as flexible tools to bridge the urban-rural digital and energy divides.


### 4.3 Dynamic Energy Geo-Dispatcher: Solving the "Spatial Mismatch" Problem of Charging Resources


Core Concept and Technical Implementation:


The construction of fixed charging piles suffers from strong "path dependence" and "heat island effects," easily becoming over-concentrated in popular areas while leaving potential demand locations underserved. The mobility of MCRs is inherently a powerful tool to solve "spatial mismatch," but simple "passive reactive" dispatch (going only when called) can still create new local congestion.


Advanced Intelligent Dispatching Algorithms by Xiaoli:


Xiaoli's cloud-based "brain" is a spatio-temporal prediction and proactive dispatch platform based on multi-source big data. It not only responds to real-time orders but also strives to predict and shape demand.


Multi-Dimensional Prediction: The platform integrates historical charging order data, real-time traffic flow data, major event calendars, weather forecasts, and even social media sentiment, using advanced models like spatio-temporal graph neural networks to generate city-level charging demand heat maps for the next 1-4 hours. These heat maps can accurately predict where charging demand "troughs" and "peaks" will emerge.


Proactive Pre-Positioning and Dynamic Balancing: Based on predicted heat maps, the dispatch system proactively moves robots with sufficient charge from "cold zones" to "warming zones" about to see increased demand, achieving "energy prepositioning." When a sudden influx of orders in one area risks congestion, the system dispatches robots from surrounding areas for support and intelligently allocates orders to prevent all robots from flocking to the same spot.


Multi-Agent Path Coordination: During cluster movement, the path planning algorithm considers not only single-robot optimality but also group optimality. Through coordinated planning, total empty travel distance and mutual interference within the robot group can be minimized, enhancing overall network efficiency. Research indicates that excellent coordination algorithms can reduce total group movement distance by over 30% \[45\].


Efficacy and Network Effects:


This predictive proactive dispatch model can improve the response time and coverage of effective charging services in a city by an order of magnitude. According to its internal models, it can expand the effective operational radius of new energy vehicles in a single city by 15-20%, particularly benefiting users sensitive to operational efficiency like ride-hailing and logistics vehicles \[46\]. More importantly, smarter dispatching leads to higher daily service frequency and revenue per robot. The overall operational efficiency and economic benefits of the network exhibit clear network effects and increasing returns to scale, constituting a strong competitive barrier.


### 4.4 Modular Agile System: The Engineering Foundation for Large-Scale Deployment, Operation, Maintenance, and Iteration


Core Concept and Industrial Design Challenge:


As an emerging product, MCR hardware technology is still rapidly iterating. Using traditional integrated design, any component upgrade or failure repair could take the entire machine offline, resulting in high maintenance costs, slow product iteration, and difficulty adapting to diverse scenario needs.


Xiaoli's Modular Architecture Practice:


From the outset of product definition, Xiaoli adopted a full-stack modular, platform-based architecture deeply inspired by advanced electric vehicle and industrial robot concepts.


Core Module Explaination:


"Skateboard" Chassis-by-Wire Platform: Serves as a universal mobile base, integrating drive, steering, braking, suspension, and basic controllers, providing standardized mechanical and electrical interfaces.


"Lego-style" Plug-and-Play Energy Storage Battery Packs: Offers standard capacity battery pack modules (e.g., 30kWh, 60kWh, 100kWh) supporting hot-swapping. Each pack has a built-in BMS and connects to the chassis via high-voltage quick-connect interfaces.


Plug-and-Play Functional Pod Modules: Include "Robotic Arm Pods" (integrating arms of varying DOF and end-effectors), "Fast-Charging Power Pods" (integrating charging modules of different power ratings), and "Perception & Computing Pods" (integrating LiDAR, cameras, computing units). These pods connect to the chassis via high-speed data buses (e.g., Ethernet) and quick-release latches.


Revolutionary Advantages:


Rapid Deployment and Scenario Adaptation: On-site deployment resembles assembling a computer. Modules are quickly selected based on customer scenarios (e.g., high-end malls need fully automatic arms; old communities may only need a remote-controlled chassis + large battery), reducing on-site installation and commissioning time from days to hours.


Efficient Operation, Maintenance, and Upgrades: Any faulty module can be quickly replaced by a field engineer, restoring the entire machine to service in minutes. Battery packs can be centrally returned to the factory for maintenance and secondary use. When a new generation of LiDAR or chip is released, only the "Perception & Computing Pod" needs upgrading, protecting asset value and enabling continuous "software-defined hardware" evolution.


Cost Optimization and Rapid Iteration: Standardized modules facilitate large-scale procurement and production, significantly reducing manufacturing costs. New feature development can focus on a single module, greatly shortening product R&D and iteration cycles, allowing the company to respond quickly to market changes \[47\].


Xiaoli's modular practice is not merely a product design method but a systems engineering mindset oriented towards scalable, sustainable operations. It ensures that MCR assets, throughout their long lifecycle, can continuously enhance capabilities and create value, rather than depreciating rapidly.


5. In-Depth Comparative Analysis of Mainstream Products and Solutions


(Note: As detailed elaboration was provided earlier, this section streamlines the product comparison table and focuses on core conclusions.)


Current market products are diverse and can be categorized into three primary technical schools, each serving distinct business logics and scenarios:


Tech-focused faction Core Products Key Technical Characteristics Key Advantages Main Challenges Typical Applying Scenarios Commercial Logic


Fully-automatic detachable DHForce G60/CanCong Lightning Power Bank/HaiHong MSO Series L4 Autonomous Driving (multi-LiDAR + vision) + collaborative robotic arm for automatic plugging/unplugging. Supports V2G/V2B. Delivers an exceptional user experience, representing the future of mobility; enables truly unattended closed-loop operations; and is well-suited for integration with autonomous driving ecosystems. Manufacturing costs and selling prices are high; algorithmic complexity is extremely high, posing significant reliability challenges in long-tail scenarios; maintenance requires a specialized team. High-end commercial real estate, technology parks, autonomous driving demonstration zones, and high-profile benchmark projects with stringent image requirements. Technological Leadership and Ecosystem Positioning. By delivering premium services to command high price premiums, and preparing for future large-scale autonomous driving operations.


Manual / Remote-controlled YiJiaDian, ZhongNeng CongCong S140, DHForce G30 Remote-controlled driving or preset-route autonomous driving, requiring manual plugging/unplugging of the charging gun. Integrated storage and charging is standard. Low cost, mature technology, and high reliability; flexible and rapid deployment; strong adaptability to complex environments (with human intervention for decision-making). Relies on on-site or remote human labor, leading to operational costs that scale linearly with expansion; user experience remains incomplete, making true "unattended operation" difficult to achieve. Old residential districts, supplementary coverage at highway service areas, vehicle battery rescue services, temporary event support, and power capacity-constrained areas. Cost-effectiveness and rapid scaling. Prioritizes solving the "availability problem" and pursues extensive scenario coverage with stable cash flow.


Rail-guided (Shared) Yijiahe N100, Haoyuan Intelligent Solution Charging piles and robotic arms are installed on steel rails mounted on parking lot canopies, moving along the tracks to serve parking spaces below. Completely solves the issue of fuel vehicles occupying charging spots; extremely high reliability (due to a controlled environment); achieves 100% charging coverage for parking spaces. Only applicable to newly built or retrofitted underground garages with a ceiling height >2.8 meters; cannot operate outside the garage; requires substantial initial installation work and has a long retrofit cycle. New residential compound underground garages, corporate headquarters indoor parking lots, newly built commercial complex parking areas. Deeply integrated with property developers. Marketed as complementary infrastructure for parking facilities, with a distinct project-based business model.


Deep Analysis Conclusions:


No Technology Path is Inherently Superior; Only the Degree of Scenario Suitability Differs


"Fully Automated" systems represent the long-term direction, but in the short term, cost and reliability remain barriers to widespread adoption. "Manual/Remote-Controlled" systems are a pragmatic choice for achieving broad coverage and generating cash flow in the near term. "Rail-Guided" systems possess irreplaceable advantages in specific scenarios (e.g., new real estate developments).


Business Models are Strongly Correlated with Technology Paths


Companies pursuing the fully automated path must explore high-value-added services (e.g., valet charging, energy operations) to support their higher costs. The manual/remote-controlled path can leverage cost advantages for rapid deployment and scale, while also exploring energy storage operations. The rail-guided path is deeply tied to real estate cycles and B2B clients.


Convergence and Evolution are Trends


The future may see the emergence of "hybrid models." For instance, fully automated robots could be deployed in areas with good infrastructure, while remote-controlled robots are used in areas with limited conditions, all managed by a unified cloud platform. Modular designs (like Xiaoli's) make such flexible configurations possible.


## Core Drivers and Systemic Challenges of Industry Development


### 6.1 Core Drivers


The Irreversible Wave of Electrification and Intelligence


Major global economies have established timelines to phase out internal combustion engine (ICE) vehicles. China’s New Energy Vehicle Industry Development Plan (2021-2035) has firmly established the dominant position of EVs \[48\]. Simultaneously, the rising levels of vehicle intelligence and connectivity provide the technical foundation for Vehicle-Pile-Grid (V2G/V2X) synergy.


Acute and Unmet User Pain Points Forming Essential Demand


"Charging difficulty" has become the primary negative factor affecting EV purchase and user experience. Users have a genuine and urgent need for convenient, reliable, and "frictionless" charging, and are willing to pay a reasonable premium for such services \[4\].


Strong Traction from National Energy Strategy and Policy


Under the "Dual Carbon" goals, constructing a new power system is a national priority. As distributed energy storage and flexible loads, MCR perfectly align with policy directions such as "Source-Grid-Load-Storage" interaction, Virtual Power Plants, and smart microgrids \[35, 38\].


Continuous Release of Underlying Technical Dividends and Cost Reductions


The costs of autonomous driving sensors (LiDAR), computing chips (AI accelerators), and power batteries continue to decline while performance improves. This makes MCRs—which were in the laboratory stage just a few years ago—commercially and technically viable today \[36\].


Rigid Pre-requisite for Future Autonomous Driving Commercialization


Large-scale commercial operation of L4+ autonomous driving necessitates unmanned energy replenishment infrastructure. MCRs represent the only mature pathway to meet this demand under current technical conditions, with their development pace closely tethered to the progress of autonomous driving \[37\].


### 6.2 Systemic Challenges and Risks


Economic Challenges: The Ultimate Test of Profit Models and ROI (Primary Risk)


Despite diverse value-added models, high initial CAPEX and uncertain OPEX keep ROI models fragile. Arbitrage gains from energy storage are heavily influenced by electricity pricing policies; the VPP market is still in its infancy; and high equipment depreciation and maintenance costs erode profits. The key to industry survival is rapidly validating a business model with a 3–5 year payback period \[49\].


The Technical Long-Tail Effect: The Difficult Leap from 99% to 99.9%


Achieving high success rates in closed demonstration scenarios is relatively easy. However, elevating the full-process success rate above the commercial requirement of 99.9%—while navigating diverse parking environments, weather conditions, and vehicle statuses nationwide—requires tackling a massive volume of "Corner Cases." This necessitates significant R&D investment and long-term accumulation of real-world road test data \[24\].


Lack of Standards and Ecosystem Fragmentation: The Invisible Wall to Scaling


The absence of national or mandatory industry standards has led to a fragmented market. Robots from different manufacturers cannot be dispatched on the same platform, cannot adapt to all vehicle models (especially those without visual assistance), and use proprietary communication protocols for charging interfaces. This increases procurement and operating costs for operators and limits network effects \[25\].


Lagging Laws and Regulations: Friction Between Innovation and Oversight


Right-of-Way and Liability: Definitions remain blurred for MCRs operating in public areas. Is their legal status a "robot," "special equipment," or a "vehicle"? Existing traffic laws do not cover them.


Insurance Gaps: Dedicated insurance products for MCR operations (including property and liability) are virtually non-existent, increasing risk exposure.


Data Security and Privacy: Ownership and regulatory requirements for collected data (VINs, location tracks, charging behavior) are unclear.


Grid Access Barriers: Barriers exist in metering, settlement, and qualification for MCRs acting as distributed resources in the electricity market \[27\].


Social Acceptance and Operational Complexity: Public skepticism regarding the safety of mobile robots, coupled with property managers' concerns over increased management burdens, creates friction. Seamless integration with existing parking barriers, space locks, and payment systems remains a tedious but vital engineering challenge.


Intense Homogeneous Competition and Potential Price Wars: In the early stages where core technological differentiation is not yet fully established and business models remain unclear, the mid-to-low-end market is highly susceptible to homogeneous competition based on price, leading to thin profit margins across the industry and undermining long-term research and development capabilities.


## Future Outlook and Multi-level Strategic Recommendations


### 7.1 Key Trend Forecasts for the Next Five Years


Technological Integration: MCRs will become a crucial component of the integrated "Vehicle-Road-Cloud-Network-Map" system


MCRs in the future will deeply integrate with Vehicle-to-Everything (V2X) technology, enabling vehicles to actively broadcast their precise parking locations and charging needs. They will also connect with city-level digital twin platforms, achieving precise scheduling based on comprehensive spatiotemporal data. Furthermore, as mobile edge computing nodes, they will contribute to urban sensing networks \[50\].


Dominance of Energy Attributes: A shift in perception from "mobile charging piles" to "mobile energy storage power stations"


Industry competition will evolve from competing on "mobility and plugging/unplugging technology" to competing on "energy storage capacity, charge/discharge power, cycle life, and energy aggregation operational capabilities." "Energy capacity" and "power" will become more critical core parameters than "maximum speed." MCR clusters, as high-quality assets for Virtual Power Plants (VPPs), will deeply participate in electricity spot and ancillary service markets \[38\].


Scenario-Specific Product Segmentation: Parallel development of specialized and general-purpose models


Specialized: Highly optimized for specific scenarios. Examples include "ultra-high-speed dedicated charging robots" for autonomous vehicle fleet depots, "low-cost, high-capacity remote-controlled robots" for older residential communities, and "ultra-slim" robots designed for underground parking garages.


General-purpose/Platform-based: Built on modular architectures, enabling rapid adaptation to various scenarios by swapping functional modules, pursuing economies of scale and operational flexibility.


Software and Data as Core Competencies: Algorithms and operations defining hardware value


Hardware will gradually become more standardized and universal. The gap between companies will increasingly be defined by the efficiency of their scheduling algorithms, the profitability of their energy trading strategies, and the intelligence level of their operation and maintenance platforms. Accumulated operational data will fuel the training of smarter AI, creating a "data-algorithm-operation" flywheel effect.


Globalization of Market Boundaries: The export of Chinese solutions will become a second growth curve


China has established global advantages in EV adoption, lithium battery supply chains, and AI applications. The insufficiency of charging infrastructure is a worldwide challenge. According to Deloitte's 2024 report "From Following to Leading—Trends and Observations on Chinese Automotive Enterprise Overseas Expansion in 2024", China's mature MCR products and operational models hold significant export potential in markets such as Europe, North America, Southeast Asia, and the Middle East, driving the entire industry chain to go global \[51\].


### 7.2 Multi-Layered Strategic Recommendations


For Governments and Regulatory Agencies:


Accelerate Standard Development to Guide Orderly Industry Growth


Led by ministries like the MIIT and NEA, collaborate with leading companies and research institutions to prioritize the formulation of key standards such as General Technical Requirements for Mobile Charging Robot Systems and Vehicle-Pile (Robot) Automatic Charging Communication Interface Specifications. Define recommended specifications and locations for visual guidance markers to lay the groundwork for interoperability.


Innovate Regulatory Mechanisms by Establishing "Sandbox" Pilots


Designate demonstration zones in key cities (e.g., Beijing, Shanghai, Shenzhen) to allow enterprises to conduct commercial operations within a controlled scope. Simultaneously explore and issue provisional management measures addressing MCR right-of-way, accident liability determination, insurance pilots, and data security management, accumulating experience for nationwide legislation.


Optimize Incentive Policies, Shifting from "Subsidizing Construction" to "Subsidizing Operations and Efficiency"


Link subsidies to actual operational efficiency metrics (e.g., average daily service sessions) and contributions to grid peak shaving (e.g., actual energy participation in demand response). Clearly include MCRs within the support framework for distributed energy storage and Virtual Power Plants (VPPs), granting them access to equivalent investment subsidies, capacity compensation, and market trading eligibility.


Strengthen Top-Level Design, Integrating into New Infrastructure Planning


Mandate or encourage the reservation of MCR passageways, dedicated parking/charging spots, and communication and power interfaces in the planning and construction of new large-scale public venues, transportation hubs, and residential communities.


For Industry Chain Enterprises:


Complete Machine/Solution Providers: Define Strategic Positioning and Build Core Moats


Assess core competencies to choose a path - "Technology Leader" (deepening full automation), "Scale Operator" (focusing on remote-controlled models + energy storage operations), or "Ecosystem Enabler" (providing modular platforms). Avoid blindly pursuing a "jack-of-all-trades" approach. Openness and collaboration are key; actively establish strategic alliances with complementary enterprises (e.g., algorithm firms, contract manufacturers, charging operators, property management companies).


Key Component Suppliers: Target MCR-specific Needs for Customized Innovation


Develop low-cost, highly reliable automotive-grade solid-state LiDAR; research and develop robotic arm end-effectors suitable for frequent plugging/unplugging with high lifespan requirements; provide highly integrated domain controller solutions. Aim to secure a leading position in this rapidly growing niche.


Charging Operators and Energy Enterprises: Position MCRs as Strategic Assets for Deployment


Do not view them merely as cost centers, but as strategic tools for building hybrid "fixed-pile + mobile robot" networks, entering the user-side energy storage service market, and acquiring high-value data. Actively explore deep-binding models such as joint ventures and cooperative operations with robotics companies.


For Investment Institutions:


Focus on Value Essence, Invest in Companies with "Network Effects" and "Data Intelligence" Potential


Prioritize teams that not only manufacture hardware but excel at building and operating robot networks, and continuously optimize network efficiency and energy revenue through algorithms. Their business model should clearly demonstrate a trend of decreasing marginal costs and increasing marginal returns with scale.


Conduct Systematic Layout Across the Entire Industry Chain


Build a strategic investment portfolio spanning upstream core components (e.g., LiDAR chips, SiC power modules), midstream solution providers with unique technologies or business models, and downstream innovative operational service platforms to share in the overall growth dividends of the industry.


Uphold Long-Termism and Maintain Strategic Patience


The MCR sector requires long-term cultivation; technological maturity, market education, and business model validation all take time. Short-term financial returns may not be pronounced, but the long-term strategic value in the energy and transportation transition is immense. Investment institutions need the patience and resolve to accompany companies through cycles.


## 8. Conclusion: Towards a Smart Energy Ecosystem Integrating Human-Vehicle-Charger-Grid-Storage


The rise of the MCR industry is far from a simple product innovation; it represents a paradigm shift signaling a profound transformation in the form of infrastructure. It marks a decisive transition of our energy replenishment system from the industrial era's "rigid pipeline" model to the digital age's "flexible network" model, engineered to meet the demands of a highly dynamic, distributed electric vehicle society \[52\]. Within this network, the relationship between energy supply and consumption evolves from a unidirectional, fixed arrangement into a multi-directional, dynamically dispatchable flow.


This whitepaper has systematically demonstrated that the core value of MCRs extends far beyond being "mobile charging piles." Through an in-depth analysis of industry practices exemplified by Xiaoli Charging, we clearly see that the ultimate form of an MCR is a programmable physical energy node. It is capable of:


Serving as a distributed energy storage unit, activating vast amounts of fragmented commercial and community-side power resources and participating in energy value restructuring.


Acting as a mobile building block, enhancing regional energy resilience and empowering rural revitalization and development in remote areas.


Functioning as an intelligent dispatch object, dynamically optimizing the geographical distribution of energy to solve the fundamental challenge of "spatial mismatch."


Being a modular product that supports agile deployment and continuous evolution, meeting the engineering requirements for large-scale operations.


Currently, the industry is at a critical juncture—the "daring leap" from technological demonstration to scaled commercialization. Economic challenges, technological long-tail problems, lack of standards, and regulatory lag present significant hurdles ahead.


However, driven by the imperative of carbon neutrality, propelled by the unstoppable wave of electric vehicle adoption, and empowered by ever-advancing digital and intelligent technologies, the historical inevitability of MCR development as a key link connecting transportation electrification and energy decarbonization is beyond doubt.


We stand at the threshold of a new era. Looking ahead, a vision of a smart energy ecosystem—deeply integrated, interactive in real-time, and co-creating value through the fusion of intelligent mobile robots, ubiquitous fixed charging piles, V2G-capable electric vehicles, highly intelligent and resilient power grids, and cloud-based intelligent systems—is no longer a distant imagination. Through the efforts of global innovators, it is gradually becoming a vivid reality. Embracing and actively participating in this transformation will not only decisively end "range anxiety" and "charging anxiety" for electric vehicles but will also pioneer a more efficient, cleaner, and sustainable future for human society.


## Reference


\[1\] International Energy Agency (IEA). (2023). Global EV Outlook 2023. Paris: IEA Publications.


\[2\] China Electric Vehicle Charging Infrastructure Promotion Alliance. (2023). Annual Report on the Development of China's Electric Vehicle Charging Infrastructure (2022-2023). Beijing.


\[3\] Sweda, T., & Klabjan, D. (2011). An agent-based decision support system for electric vehicle charging infrastructure deployment. Journal of Power Sources, 196(23), 10049-10060.


\[4\] iResearch. (2023). Research Report on Charging Behavior and Satisfaction of Chinese Electric Vehicle Users.


\[5\] National Development and Reform Commission, National Energy Administration. (2022). The 14th Five-Year Plan for a Modern Energy System.


\[6\] Kempton, W., & Tomić, J. (2005). Vehicle-to-grid power fundamentals: Calculating capacity and net revenue. Journal of Power Sources, 144(1), 268-279.


\[7\] Momtazpour, M., et al. (2015). Coordinated charging of plug-in electric vehicles in smart grids. Sustainable Energy, Grids and Networks, 1, 38-50.


\[8\] Harik, E. H. C. (2021). Design and Implementation of an Autonomous Charging Station for Agricultural Electrical Vehicles. Applied Sciences, 11(13), 6168.


\[9\] Quigley, M., et al. (2009). ROS: an open-source Robot Operating System. In ICRA workshop on open source software (Vol. 3, No. 3.2, p. 5).


\[10\] Siegwart, R., Nourbakhsh, I. R., & Scaramuzza, D. (2011). Introduction to autonomous mobile robots (2nd ed.). MIT Press.


\[11\] Ouyang Minggao. (2023). Technological Progress and Prospects of Vehicle Power Battery Systems. Automotive Engineering, 45(1), 1-10.


\[12\] Thrun, S., et al. (2005). Stanley: The robot that won the DARPA Grand Challenge. Journal of Field Robotics, 23(9), 661-692.


\[13\] Bresson, G., Alsayed, Z., Yu, L., & Glaser, S. (2017). Simultaneous localization and mapping: A survey of current trends in autonomous driving. IEEE Transactions on Intelligent Vehicles, 2(3), 194-220.


\[14\] China Academy of Information and Communications Technology. (2023). White Paper on the Development of 5G+Robot Integrated Applications.


\[15\] Lee, J., Bagheri, B., & Kao, H. A. (2015). A cyber-physical systems architecture for industry 4.0-based manufacturing systems. Manufacturing Letters, 3, 18-23.


\[16\] Cadena, C., et al. (2016). Past, present, and future of simultaneous localization and mapping: Toward the robust-perception age. IEEE Transactions on Robotics, 32(6), 1309-1332.


\[17\] Chen, L., et al. (2022). Deep learning for visual SLAM: A survey. Robotics and Autonomous Systems, 155, 104193.


\[18\] Romero-Ramirez, F. J., Muñoz-Salinas, R., & Medina-Carnicer, R. (2018). Speeded up detection of squared fiducial markers. Image and Vision Computing, 76, 38-47.


\[19\] Toth, P., & Vigo, D. (Eds.). (2014). Vehicle routing: problems, methods, and applications (2nd ed.). Society for Industrial and Applied Mathematics.


\[20\] Claes, R., & Holvoet, T. (2016). Decentralized task allocation for multi-robot systems using auction-based protocols. Autonomous Agents and Multi-Agent Systems, 30(5), 938-964.


\[21\] International Organization for Standardization. (2018). ISO 26262: Road vehicles -- Functional safety.


\[22\] International Organization for Standardization. (2019). ISO/PAS 21448: Road vehicles -- Safety of the intended functionality (SOTIF).


\[23\] International Organization for Standardization / Society of Automotive Engineers. (2021). ISO/SAE 21434: Road vehicles -- Cybersecurity engineering.


\[24\] Amodei, D., et al. (2016). Concrete problems in AI safety. arXiv preprint arXiv:1606.06565.


\[25\] National Technical Committee of Auto Standardization. (2022). National Standards for Conductive Charging Connection Devices of Electric Vehicles (GB/T 20234 Series).


\[26\] McKinsey & Company. (2023). Mobility's future: An investment reality check.


\[27\] General Office of the State Council of the People's Republic of China. (2020). Development Plan for the New Energy Vehicle Industry (2021-2035).


\[28\] BloombergNEF. (2023). Electric Vehicle Outlook 2023.


\[29\] Roland Berger. (2023). Global and China Mobile Charging Market Insights Report.


\[30\] CICC Research Department. (2024). Robotics Industry Series: Mobile Charging Robots, Setting Sail in a Blue Ocean.


\[31\] China Association of Automobile Manufacturers. (2024). China Automotive Production and Sales Data 2023 and Outlook for 2024.


\[32\] China EV100. (2023). Prospects for the Supply Chain and Market Development of New Energy Vehicles in China.


\[33\] China Charging Alliance. (2024). Monthly Report on the Operation of National Electric Vehicle Charging and Battery Swap Infrastructure (April 2024).


\[34\] Highway Bureau of the Ministry of Transport. (2023). Analysis and Safeguard Report on National Highway Network Operation During Major Holidays.


\[35\] National Development and Reform Commission, National Energy Administration. (2022). Implementation Plan for New Energy Storage Development During the 14th Five-Year Plan Period.


\[36\] Boston Consulting Group (BCG). (2022). Charging Ahead: The Future of EV Charging Infrastructure.


\[37\] Fraedrich, E., et al. (2019). Autonomous driving, infrastructure and mobility services: A complex systems perspective. Transportation Research Part A: Policy and Practice, 129, 1-15.


\[38\] General Department of the National Energy Administration. (2023). Notice on Organizing Pilot Projects for Virtual Power Plant Construction (Draft for Comments).


\[39\] Teld New Energy Co., Ltd. (2023). Annual Report.


\[40\] Beijing Municipal Urban Management Commission. (2023). Detailed Implementation Rules for the 2023 Beijing Electric Vehicle Charging and Battery Swap Facility Construction and Operation Incentive Program.


\[41\] Hefei Municipal Development and Reform Commission. (2023). Detailed Implementation Rules for Further Policies to Promote the Application of New Energy and Intelligent Connected Vehicles in Hefei.


\[42\] China Electricity Council. (2022). Annual Development Report of China's Electric Power Industry.


\[43\] Xiao Li Charging. (2024). Smart Energy Network Node Solution Business Model White Paper (Internal Document).


\[44\] National Development and Reform Commission, National Energy Administration. (2022). Guiding Opinions on Implementing the Rural Power Grid Consolidation and Upgrading Project.


\[45\] Albrecht, S. V., & Stone, P. (2018). Autonomous agents modelling other agents: A comprehensive survey and open problems. Artificial Intelligence, 258, 66-95.


\[46\] AutoNavi. (2023). 2023 Annual Traffic Analysis Report of Major Chinese Cities.


\[47\] Ulrich, K. T., & Eppinger, S. D. (2015). Product design and development (6th ed.). McGraw-Hill Education.


\[48\] Ministry of Industry and Information Technology. (2020). Interpretation of the "Development Plan for the New Energy Vehicle Industry (2021-2035)".


\[49\] PwC. (2023). China New Energy Vehicle Charging Service Market: Business Models and Profitability Challenges.


\[50\] China Intelligent and Connected Vehicles Industry Innovation Alliance. (2023). White Paper on Vehicle-Road Collaborative Development (2023).


\[51\] Deloitte. (2024). From Following to Leading: Trends and Observations on Chinese Automakers Going Global in 2024.


\[52\] Rifkin, J. (2011). The third industrial revolution: How lateral power is transforming energy, the economy, and the world. Palgrave Macmillan.
