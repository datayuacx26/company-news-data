---
schema_version: "1.0.0"
document_id: "43c67c70e5d8e0136114e6f06b9d84d4e94784b3f9b56566160d85be77f19cc2"
company_key: "yc-korrai"
company: "KorrAI"
source_id: "yc-korrai-news-import-a686cf049d8d"
canonical_url: "https://www.korrai.com/blog/integrating-insar-into-infrastructure-strategy-a-playbook"
published_at: null
first_seen_at: "2026-07-22T01:41:37.374471+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:84f941a15809d3b90fdbbff7ddaec1d6a293db9e344e9db00858f98384ac9eec"
---

# Integrating InSAR Into Infrastructure Strategy: A playbook

> “ *What makes this piece timely and valuable is its focus on integration. I’ve seen many teams struggle to meaningfully plug InSAR data into their workflows. It’s not just about generating displacement maps, it’s about embedding temporal insights into project decisions across the lifecycle, so that actions are based on evidence, not assumptions.” ‍* -Prof Dr Vern Singhroy, Technical Advisor, KorrAI, Chief Scientist, RADARSAT Constellation Mission


## A New Lens on Ground Stability: InSAR


Infrastructure has long battled a quiet disruptor, unstable ground. Even with exhaustive planning, sudden subsidence, unexpected slope failures, or structural deformation can throw costly wrenches into otherwise sound projects.


One moment, a tunnel project is underway beneath a city street; the next, a shift in the earth has cracked pavement and disrupted utilities. Whether it’s an aqueduct sagging under invisible strain or a pipeline corridor jeopardized by creeping slopes, the consequences range from expensive to catastrophic.


For long, geotechnical teams have relied on tools like boreholes, inclinometers, and periodic surveys. While useful, these methods often miss the in-between moments. They capture isolated data points rather than a stream. And by the time traditional surveys reveal an issue, the damage has often begun. This reactive approach no longer satisfies today’s demands, where risk tolerances are razor-thin and the stakes are high.


As infrastructure spreads and as older systems age, the pressure to catch problems early grows. Engineers, risk managers, and safety specialists now push toward proactive oversight, adopting smarter, broader, and more consistent monitoring strategies.


At the center of this shift stands[InSAR (Interferometric Synthetic Aperture Radar)](https://www.korrai.com/blog/introduction-to-insar-guide-for-geotechnical-engineers-risk-analysts-and-safety-engineers) , a technology that monitors the Earth’s movement from space, quietly but constantly, offering wide coverage and millimeter-level precision.


### A Case in Point: InSAR’s impact on Seattle’s tunnel project


During[Seattle’s SR 99 tunnel project](https://wsdot.wa.gov/construction-planning/search-projects/sr-99-tunnel) , the massive boring machine “Bertha” stalled after hitting a buried steel pipe. Repairing it required a deep rescue shaft and aggressive dewatering. At surface level, all seemed under control.


But beneath the surface, things were moving.


InSAR told the story that traditional tools couldn’t. While standard monitoring showed little cause for concern, satellite radar detected up to 60mm/year of ground subsidence, far beyond the expected range. The data exposed a clear link between aquifer depletion and widespread settlement, revealing risks that would have gone unnoticed until damage appeared.


InSAR detected deformation early, enabling smarter decisions, reducing risks, and cutting remediation costs, which proved its value in urban tunneling. It replaces assumptions with added confidence where guesswork once ruled. More on the Seattle Tunnel project in the case studies section.


### What This Playbook Offers


This article isn’t theory. It’s a practical guide for geotechnical professionals, safety leads, and risk analysts. Here's what you’ll take away:


- Where InSAR fits across your project lifecycle from early planning to long-term monitoring.
- How to blend InSAR with your current strategy for a layered, proactive risk management strategy.
- Step-wise guide to integrate InSAR into your workflow without overhauling everything.


By the end, you’ll understand not just why InSAR is a game-changer, but how to put it to work, helping you build safer, smarter, and more resilient infrastructure with fewer surprises.


## What is InSAR: What Can it Do That Your Current Toolkit Can’t?


Inclinometers. Piezometers. Extensometers. Manual surveying. These are long-trusted tools in every engineer’s kit. They’re precise, proven, and deeply embedded in project workflows.


But as infrastructure grows in scale, complexity, and regulatory oversight, traditional methods are being asked to do more than they were designed for. Site visits become logistically challenging. Data collection lags behind ground reality. And coverage? Still mostly point-based. Some ground sensors can’t say what’s going on meters away.


This is where InSAR steps in, not as a replacement, but as a force multiplier.


### A Quick Refresher: What Exactly is InSAR?


If you're already knee-deep in ground motion data, feel free to skip ahead. If not, here’s the elevator pitch.


What it is:


- Interferometric Synthetic Aperture Radar (InSAR) is a remote sensing technique for monitoring millimeter-level displacement of the Earth’s surface through repeated radar imaging of an area over time.
- Data is typically obtained from satellites carrying Synthetic Aperture Radar (SAR) sensors that transmit microwave spectrum signals. These radar waves penetrate through cloud cover and also operate in darkness, making them suitable for tracking deformation irrespective of the weather conditions.


What it does: Measures displacement at the millimeter scale, even across cities, mines, or remote corridors, without a single site visit.


Still curious? Our previous issue on[Introduction to InSAR](https://www.korrai.com/blog/introduction-to-insar-guide-for-geotechnical-engineers-risk-analysts-and-safety-engineers) goes deeper into Line-of-Sight, coherence, SAR geometry, targets, and selection of the right SAR data, giving you a foundational understanding of InSAR and key principles.


### How InSAR Stacks Up: A Side-by-Side Comparison to Traditional Tools


InSAR Integration Table Feature Traditional Tools (Inclinometers, Surveying, etc.) InSAR Monitoring Integration Benefits


Precision Sub-mm to mm at discrete points mm-level over large areas Validate and cross-check localized measurements with broader spatial trends


Coverage Point-based, limited spatial scope Thousands of km² in a single pass Fills spatial gaps between instruments, enabling complete ground behavior mapping


Cost Efficiency High cost per inspection, difficult to scale Cost drops significantly per area monitored Reduces need for dense instrumentation; more insights with fewer physical installations


Data Collection Manual, requires site access Remote, continuous, digital Enhances safety, reduces site visits, and supports real-time decision-making


Update Frequency Dependent on manual intervals Every few days to weeks, depending on the satellite Enables early warnings and trend tracking between manual readings


‍


**Types of Movements Best Detected by InSAR**


InSAR performs exceptionally well in scenarios involving slow, widespread ground movement that traditional point-based tools might miss. It is particularly effective at detecting gradual subsidence, the kind often caused by **sustained groundwater extraction** or the aforementioned tunneling activity. These slow, continuous shifts can go unnoticed in manual surveys but are clearly captured through InSAR-based monitoring.


It is also a powerful tool for identifying **pre-failure creep** , the subtle deformations that indicate slope instability well before a collapse or landslide occurs. Catching these early signals allows engineers to take action before a critical failure develops.


In addition, InSAR excels at tracking **regional displacement patterns** . For large-scale infrastructure such as pipelines, aqueducts, and transport corridors, it provides a consistent and expansive view of how ground conditions are evolving across the entire area.


## Integrating InSAR into Existing Systems


### InSAR + Inclinometers, Piezometers, and Borehole Logs


Ground sensors provide high-resolution data at specific points. InSAR gives a wide-area view of surface movement. When combined, they offer a more complete understanding of ground behavior. Instead of just complementing each other, they work as a unified system that fills in the gaps left by either method alone.


Together, they provide:


- **Complete ground behavior profiles** , from deep subsurface movement to wider surface trends.
- A way to **fill the gaps** between instrumented areas
- **Cross-validation** : If both sources detect similar trends, confidence increases. If they diverge, it’s a signal to investigate further.


**Example:**


You notice slight horizontal movement in one inclinometer, but InSAR shows movement over a broader slope zone. Suddenly, it's not just a local anomaly; it's a broader failure zone in the making that potentially changes the risk profile.


### Plug-and-Play InSAR Data


One of the best things about modern InSAR platforms is how easily they integrate with tools you already use.


- **File formats** : InSAR outputs are typically available as GeoTIFFs, shapefiles, CSVs, or even Excel-ready tables, ready for import into GIS platforms, CAD software, or geotechnical modeling environments.
- **Dashboards** : Some platforms (like KorrAI) offer built-in dashboards with direct import options to integrate other data into the dashboard.


Importing files in KorrAI Dashboard


- **Data layering** : InSAR results can be overlaid with site plans, borehole logs, satellite imagery, or sensor networks in your existing GIS tools like ArcGIS or QGIS.
- **Alerts and reports** : Customizable thresholds of movement can trigger automatic alerts or PDF reports, and this is great for operations teams and regulatory compliance.


In short, you don’t need to change your systems. InSAR plugs right in. Don’t need to start from scratch. InSAR simply adds more insight, more coverage, and less guesswork to your workflows.


## Accuracy, Resolution, and Coverage: Practical Insights for Engineers


InSAR is highly versatile in usage. Satellite revisit times typically range from 6 to 46 days, and spatial resolution can vary from as fine as 0.3 meters to over 30 meters, depending on the satellite system, band type, and imaging mode.


Faster revisit time → Responsive to dynamic cases such as landslides or tunneling


Slower revisit time → Long-term subsidence


While InSAR performs well in detecting broad deformation patterns, very localized movement, such as displacement in small retaining walls or specific slope features, may require enhanced setups. This is where GNSS stations or corner reflectors come into play, helping improve both accuracy and consistency by anchoring satellite data to fixed ground reference points.


To understand how the correct satellites and SAR bands are selected for any project, read:[Choosing the Right InSAR Data for Your Project](https://www.korrai.com/blog/insar-in-practice-choosing-the-right-data-for-your-project) , and explore our proprietary[GNSS based calibration methodology](https://www.korrai.com/blog/enhancing-insar-accuracy-with-gnss-calibration) to understand how we enhance InSAR accuracy assisted by public and local GNSS network.


## When and Where to Use InSAR in Your Project Lifecycle


InSAR isn’t just a “nice-to-have”. When applied at the right stage, it can shift your entire project strategy from reactive to predictive. By embedding InSAR at key points throughout your project lifecycle, you get earlier warnings, broader context, and greater confidence in design and risk decisions.


Let’s walk through where and how InSAR delivers maximum value across the typical phases of infrastructure development.


### InSAR Across Project Phases


{{features-widget}}


## Costs and ROI


While InSAR might sound like a cutting-edge technology, and it is, its cost-to-value ratio makes it one of the **most cost-effective tools** in your geotechnical and infrastructure monitoring toolkit. And when you factor in the risks it helps you avoid, the return on investment (ROI) becomes even more compelling.


### Why the ROI on InSAR Makes Sense


InSAR is more than a monitoring upgrade. It is a strategic investment that delivers value across the entire project lifecycle. Its value starts with timing. The earlier you detect ground movement, the more control you have over how to respond. Instead of scrambling to stabilize a failing slope or redesign a foundation, teams can act while the problem is still small, often avoiding disruption altogether.


Scheduled monitoring makes maintenance smarter, less frequent, and more targeted.


You can match the cost of installing 5-6 monitoring points with the price of surveying an entire critical area via InSAR. From front-end planning to long-term operations, InSAR enables better decisions at lower risk, and over time, that’s exactly what drives a strong return on investment.


### Who Benefits and How?


{{services-widget}}


### How to Justify InSAR in Your Proposals or to Clients


If you're proposing InSAR to a client, regulator, or internal decision-maker, here are ways to make the case effectively:


1. **Focus on Risk Reduction**


Frame InSAR as a tool for **risk avoidance** , not just data collection. For critical infrastructure, the cost of failure is massive, both financially and reputationally. InSAR offers a way to quantify and manage risks early.


> "With InSAR, we can detect the early signs of ground movement before traditional instruments would trigger. That gives us a wider window to act and prevents expensive delays or remediation."


1. **Highlight the Cost Efficiency**


An InSAR dataset can eliminate dozens of physical visits, reducing both labor and equipment overhead while needing a lower ground sensor density.


> "For the cost of installing 5–6 extra monitoring points, we can instead monitor the entire corridor, detect patterns between instruments, and capture early deformation trends."


1. **Position InSAR as a Strategic Advantage**


Present an innovative approach to proactive engineering decisions.


> "By integrating InSAR, we're providing a resilient, more data-driven strategy to decision-making"


## Choosing the Right InSAR Product


### Why In-House InSAR Monitoring Is Challenging


While some teams consider managing InSAR internally, it’s typically not practical without dedicated remote sensing expertise. The process involves more than accessing satellite images. It requires specialized tools, cloud-based processing pipelines, the right algorithms and hazard grading, and importantly, people with a deep understanding of radar science and error correction.


Without dedicated remote sensing expertise, the learning curve is steep, and training efforts can quickly outweigh the returns.


Therefore, for most infrastructure teams, outsourcing to a specialized provider is far more efficient, cost-effective, and reliable.


### Key Features to Look For


When selecting an InSAR solution, focus on products that offer:


- **Consistent Monitoring** : Frequent updates (every 4–12 days) or longer as per your project, with high reliability
- **Multi-Scale Coverage** : Both regional and site-specific resolution
- **Clear Dashboards** : Intuitive visualizations, time series, risk indices, and alert systems
- **Engineering Support** : Not just data, but expert interpretation aligned with geotechnical needs
- **Data Integration** : GIS-compatible outputs (e.g., GeoTIFF, shapefiles) or API access to plug into your systems


#### Internal Stakeholders to Involve


To get the most out of InSAR, alignment across key teams is essential. Geotechnical leaders play a central role in interpreting the data, while project managers ensure those insights are embedded in planning and design decisions. Risk and safety teams help define thresholds for alerts and shape response protocols. Meanwhile, GIS and IT teams handle data integration so that InSAR fits smoothly into your existing systems. Extensive training isn’t necessary, but it’s important that core users are comfortable navigating the dashboard and interpreting insights. A short onboarding will suffice for this.


## How KorrAI Makes This Simple


Integrating InSAR into your infrastructure strategy can seem daunting, but **KorrAI** simplifies this process with its advanced technology stack, expert support, and proven real-world applications.


### Platform Overview: Ground Motion Monitor, Risk Index, and AI Alerts


**KorrAI's platform** leverages satellite-based InSAR data to detect millimeter-level ground movements, providing timely alerts to mitigate risks to critical infrastructure. Key components include:


- [Ground Motion Monitor :](https://www.korrai.com/products/ground-motion-monitor) This flagship product utilizes InSAR data to detect precise ground movements, calibrated with GNSS sensor networks. Combined with an advanced data processing pipeline and AI-powered algorithms, it helps mitigate risks for infrastructure assets globally.
- [Ground Motion Risk Index :](https://www.korrai.com/products/ground-motion-risk-index) A proprietary hazard grading system that processes InSAR data at an unprecedented scale, identifying subsidence threats at regional and property levels.
- [TRAIL](https://www.korrai.com/products/trail) **:** An industry-grade AI worker that understands the specifics of your mines and infrastructure. It combines connected data, graph-aware AI, and autonomous workflows to turn your site into a queryable system.
- [Drone-Based InSAR](https://www.korrai.com/book-meeting) : While satellite-based InSAR excels in monitoring large regions, KorrAI enhances this capability with **drone-based InSAR** for high-resolution monitoring of specific structures or areas within a site. This approach increases accuracy and responsiveness for critical areas or hotspots, providing detailed insights that inform timely interventions.


By offering a comprehensive suite of tools, KorrAI simplifies the adoption of InSAR technology, enabling proactive risk management and safeguarding critical infrastructure assets.


### Expert Support: Geotechnical Interpretation and Remote Sensing


Beyond technology, we are a team of experts in remote sensing, processing architecture, analytics, and geotechnical and executive interpretation. Our team collaborates with clients to translate complex data into practical strategies, ensuring that insights are not only accurate but also actionable within existing engineering frameworks.


## Real-World Integrations


**Case Study: InSAR's Contributions in Seattle's Tunnel Project (Continued…)**


The SR 99 Tunnel Project in Seattle showcased[InSAR as the ultimate companion](https://www.xyht.com/professional-surveyor-archives/feature-babysitting-bertha/#:~:text=InSAR%3A%20Interferometric%20Synthetic,to%20provide%20redundancy.) to the dense sensor network (5000+ data points) across the project, where the integrated approach saved the project from fatal consequences.


The beauty of the InSAR approach was its comprehensive coverage. While conventional instruments were focused on the immediate tunnel zone, InSAR revealed a subsidence pattern extending approximately[1 km²](https://www.xyht.com/professional-surveyor-archives/feature-babysitting-bertha/) around the rescue shaft. This wide-area perspective allowed engineers to see the full impact of dewatering activities that extended well beyond the expected zone of influence - something the ground-based survey networks alone might have missed.


After November 25, 2014, the settlement rate decreased significantly to about[0.06 mm/day](https://onlinepubs.trb.org/onlinepubs/webinars/161107.pdf) , showing how InSAR could track the entire deformation lifecycle with remarkable precision.


By combining opposing look directions from RADARSAT-2 Spotlight imagery collected between[2012 and 2015](https://eijournal.com/print/articles/learn-the-ground-rules-insar-enables-proactive-urban-infrastructure-monitoring) , analysts could derive 2D deformation maps (vertical + east-west) that painted the complete picture of what was happening beneath Seattle's streets.


### KorrAI Case Studies of InSAR Integration


KorrAI's solutions have been successfully integrated into various sectors:


- **Mining Operations** : Continuous monitoring of tailings dams and excavation sites to detect early signs of ground instability, preventing potential failures and ensuring safety.
- **Data Centers** : Ensuring the stability of ground conditions to protect critical data infrastructure from subsidence or other ground movements that could compromise operations.
- **Infrastructure Corridors** : Monitoring transportation networks, including railways and highways, to detect and address ground movement issues that could affect structural integrity and safety.


**Case Study 1:**


After a section collapsed on Teton Pass Highway (WY-22) in June 2024, we were roped in by[Wyoming Department of Transportation](https://www.dot.state.wy.us/news/teton-pass-reopened-damaged-area-continues-to-be-monitored) to monitor the 17.5-mile highway corridor using InSAR.
Utilizing data from Sentinel-1A, processed through our proprietary algorithms, we successfully identified multiple areas requiring investigation before visible signs of instability. This helped prevent further issues while helping validate the effectiveness of repairs and improvements.


[Read the case study here.](https://www.korrai.com/blog/case-study-monitoring-teton-pass-highway-stability-for-wydot)


**Case Study 2:**


We were engaged through the[Canadian Space Agency’s SmartEarth initiative](https://www.asc-csa.gc.ca/eng/funding-programs/programs/smartearth/contributions-grants-contracts-awarded.asp#:~:text=KorrAI%20Technologies,by%20permafrost%20thaw.) to monitor 12,000 km² (including 250 km of roadways and 3 airports) of Northern Canada for ground displacement due to permafrost thawing.
We leveraged Sentinel‑1 and RADARSAT data and applied our proprietary UrbanSAR and Hazard Grade models to detect early signs of ground instability due to thermokarst, well before visible damage surfaced. All of this made accessible via our Open Ground Motion Service (OGMS).


[Read the case study here.](https://www.korrai.com/blog/case-study-monitoring-public-infrastructure-in-permafrost-regions-of-northern-canada)


## From Optional to Essential


InSAR has evolved from a niche tool into a practical solution for modern infrastructure monitoring. It offers millimeter-level insights across vast areas, integrates seamlessly with existing workflows, and supports better decisions at every stage, from site selection to long-term maintenance.


The value lies not just in the data, but in what you do with it. With the right setup and alignment across teams, InSAR becomes more than a monitoring layer. It becomes a strategic advantage. One that helps you reduce risk, cut unnecessary costs, and act with clarity before problems surface.


So, how to adopt? Start small: pilot it on a site, or include it in your next feasibility study. Once you see the value, scaling up is simple and the results are immediate. Let the ground speak before it shifts.


A project lead once described InSAR as “having a time machine and telescope rolled into one.”


> *“Having helped deploy InSAR across some of Canada’s most remote and sensitive infrastructure corridors, I’ve seen how it reshapes what we consider ‘monitorable.’ Integration of InSAR in critical projects, no longer has to be complex. With the right tools and support, InSAR can serve a major strategic advantage.”*
>
>
> - Prof Dr Vern Singhroy


### Key Takeaways


1. InSAR shifts infrastructure monitoring from reactive to predictive, offering continuous, millimeter-level insights across your site.
2. It enhances and extends traditional monitoring, filling gaps between ground sensors, validating assumptions, and improving models.
3. Integration is straightforward, with outputs that fit easily into existing GIS platforms, dashboards, and workflows without an overhaul to your system.
4. The ROI is driven by early action, reduced field effort, smarter site selection, and fewer costly surprises throughout design, construction, and operations.
5. In-house implementation is complex, but partnering with an expert service provider ensures fast, accurate insights without requiring deep radar expertise.
6. Successful adoption depends on internal alignment, especially across geotechnical, risk, operations, and GIS teams, with minimal training needed to get started.
