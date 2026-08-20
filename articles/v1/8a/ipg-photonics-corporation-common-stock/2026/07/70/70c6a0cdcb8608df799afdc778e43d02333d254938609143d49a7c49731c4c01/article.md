---
schema_version: "1.0.0"
document_id: "70c6a0cdcb8608df799afdc778e43d02333d254938609143d49a7c49731c4c01"
company_key: "ipg-photonics-corporation-common-stock"
company: "IPG Photonics Corporation"
source_id: "ipg-photonics-corporation-common-stock-news-import-4e17d4fcb86c"
canonical_url: "https://www.ipgphotonics.com/newsroom/stories/laser-welding-next-gen-battery-modules"
published_at: null
first_seen_at: "2026-07-25T10:03:30.644428+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:1122bdc8e9cb470abe4988b210be682e0bb4098cb62b9f778ea2c2229de3a752"
---

# Laser Welding Next Generation EV Battery Modules

EV battery module builders face continual pressure to enhance system performance


– particularly by extending range and reducing charging times – while simultaneously lowering production costs. These price pressures often lead manufacturers to favor prismatic cells, which are currently the most economical battery form factor. Unfortunately, prismatic cells typically deliver lower performance compared to cylindrical alternatives, still leaving system designers with only half their problem solved.


It is possible to improve charge and discharge rates in prismatic designs by increasing the cross-sectional area of the busbar, thereby reducing electrical resistance. But thicker busbars introduce new challenges, especially for welding. In particular, the lasers traditionally used for busbar-to-terminal joining can struggle to achieve the deeper penetration needed without applying excessive heat that increases the chances of part damage.


Now, two technologies have overcome these challenges, thus enabling cost-effective welding of thicker busbars while still maintaining the speed, reliability, and yields required for high-volume production. The first of these technologies is[dual-beam fiber laser](https://www.ipgphotonics.com/technology/dual-beam-lasers) welding. The second is real-time, in-process[weld measurement and verificatio](https://www.ipgphotonics.com/newsroom/stories/laser-weld-monitoring-techniques-how-they-work) n using[optical coherence tomography (OCT)](https://www.ipgphotonics.com/technology/real-time-weld-measurement) .


Here we’ll learn how each of these tools supports the next generation of[advanced EV battery module manufacturing](https://www.ipgphotonics.com/pages/optimizing-ev-battery-laser-welding) .


### Dual-Beam Laser Welding


Dual-beam technology is one of the most significant advances in laser processing in recent years. Its impact is especially pronounced in[e-mobility manufacturing](https://www.ipgphotonics.com/solutions/e-mobility-manufacturing-solutions) , where it enables reliable keyhole welding of highly reflective metals such as copper and aluminum, as well as challenging dissimilar material combinations. These materials often experience spatter, porosity, and inconsistent penetration depth when welded with traditional single-beam fiber lasers.


The most widely used and effective form of dual-beam technology features a central, round “core beam” surrounded by a concentric annular “ring beam”. The power in each can be adjusted independently


– ideally across the entire 0% to 100% range.


### How Dual-Beam Laser Welding Works ****


To understand the benefits of this configuration, it’s important to realize that stable laser keyhole welding requires properly balancing two opposing forces within the molten metal.


**Pressure:** The first force is pressure that opens and maintains the keyhole. This pressure is created when the laser heats the surface and vaporized metal expands.


**Surface Tension:** The second is a combination of surface tension and viscous forces in the molten metal that act to pull the keyhole closed.


When the balance of these two opposing forces is disturbed, the keyhole can oscillate, collapse, trap gas, or eject molten metal.


In dual-beam laser welding, the core beam initiates and maintains the weld keyhole while the ring beam stabilizes the weld pool. Specifically, the ring beam gently preheats and melts material around the core. This smooths out temperature gradients and enables vapor to vent steadily, reducing pressure spikes that cause spatter, collapse, or other instabilities. In this way, the balance of forces is maintained.


Keeping the area around the keyhole molten also allows material to flow back into it. It can then spread out more evenly before solidifying. Plus, heating from the ring beam slows down cooling and solidification which prevents hot cracking in aluminum.


Ring beam pre-melting also increases the absorption of infrared light in copper, boosting process efficiency and further enhancing stability.


Together, these effects virtually eliminate spatter, deliver consistent penetration, and produce smoother joints with superior mechanical strength. And dual-beam lasers accomplish this at welding speeds up to ten times faster than single-beam systems.


****


### Achieving Precision with Single-Mode Lasers


Dual-beam lasers are available with many possible combinations of total output power, ring-to-core size ratio, and overall beam size. There is no universal “best” configuration


– as always, the optimal laser parameters depend on the specific material(s) and process requirements.


For the case of welding thicker busbars (over 2 mm), it is critical to achieve deep penetration along with large, consistent weld cross sections in order to minimize electrical resistance. There are a couple different approaches to achieving this.


The first is to use a high-power, dual-beam laser with a multi-mode core beam. This configuration makes it possible to rapidly deliver a large amount of laser energy into a relatively big weld zone.


The advantage of this approach is speed. It produces a large cross-section weld very quickly.


The negative is that delivering all this energy so rapidly creates a sizable heat-affected zone (HAZ). This increases the possibility of damaging nearby heat sensitive parts or structures (like plastic components behind the terminal).


The second approach is to use a dual-beam laser with a lower power, single-mode (TEM00) core beam. Despite its lower total power, the high beam quality enables the center beam to be focused to a much smaller spot. This yields higher energy density than what is typically attainable with a multi-mode beam.


Higher energy density beams achieve deeper penetration compared to lower energy density beams of the same total power. Additionally, the beam profile of a single-mode laser is inherently more consistent over time than a multi-mode laser, allowing for better keyhole control and improved process consistency.


The result is that a dual-beam laser with a single-mode core beam can quickly initiate a weld even in high reflectivity metals – like copper and aluminum. Simultaneously, it rapidly achieves the required weld penetration depth. Because more of the laser energy goes into welding the material, rather than heating it, it minimizes the HAZ.


And there’s another, more subtle benefit of a single-mode center beam. Its better mode quality (M


²) means an increased Rayleigh range. This is the distance over which the focused beam maintains a nearly constant spot size.


Because the beam size doesn’t vary as much above and below the point of focus, the welding process becomes much less sensitive to changes in material height or thickness. This means a more tolerant process and a wider process window. This can have a huge impact on yields in actual production welding environments.


Finally, it should be noted that all these benefits scale with laser mode quality. As M² decreases (indicating an *increased* beam quality), all these benefits become more pronounced.


Of course, there is a disadvantage to welding with a single-mode core beam. Doing so produces a narrower weld seam, meaning that it takes a longer weld to build a sufficiently large total weld cross-section. Typically, this is achieved by welding a pattern (like a spiral) or making multiple, closely spaced short welds, rather than a single, long straight weld.


Thus, there’s a clear tradeoff in process speed and weld quality. Multi-mode laser welding is faster but generates a larger HAZ. Single-mode laser welding takes longer to weld a given cross-section but minimizes HAZ and maximizes weld joint quality.


### Prioritizing Weld Quality Assurance


An EV battery module may contain hundreds of individual welds. A single defective connection can increase internal resistance, reduce pack performance, or even create a safety hazard. This means a defect rate as low as 1 in 10,000 can result in frequent module-level failures. This makes inline verification imperative, even when using highly reliable laser welding systems.


Traditionally, most weld monitoring systems have used photodiode sensors that detect the light emitted from the molten pool and plasma plume above the weld. These signals are then compared statistically to stored reference data from known good welds. While this method can reveal general process changes, it doesn’t actually measure the weld itself


– only how the emitted light differs from past averages.


Plus, because the signal depends on collected light rather than true weld geometry, it is easily influenced by unrelated factors. Variations in surface reflectivity, beam alignment, or focus position can all change the amount of returned light and trigger false readings. To make matters worse, under- and over-penetration often generate nearly identical emission profiles. This ambiguity can lead to unnecessary scrap, rework, and continuing uncertainty about actual weld quality.


[Optical Coherence Tomography (OCT)](https://www.ipgphotonics.com/technology/real-time-weld-measurement) was developed to provide a direct measurement of true weld depth. OCT uses a low-power, near-infrared measurement beam projected through the same optics as the welding laser. This means it always remains perfectly aligned and coaxial with the process beam.


The light from the OCT source enters the keyhole and is reflected back. Interferometry is used to obtain the distance to the reflecting surface


– the bottom of the keyhole in this case.


This reflection is monitored continuously to provide a real-time measurement of keyhole depth with micron-level precision. Because it senses through coherent interference, rather than relying on weld plume brightness or temperature, OCT is unaffected by changes in surface condition, material reflectivity, or beam power.


OCT is particularly valuable for single-mode core beam welding. This produces deep, narrow, high-aspect-ratio keyholes that are challenging for most optical systems to access. But OCT can readily probe keyholes that are only a few tens of microns wide. As a result, it is highly suited to measuring penetration in thick busbars where depth control is critical.


The speed of OCT instrumentation enables manufacturers to validate every weld performed in real time. Conditions like under- or over-penetration can be identified and flagged immediately


.


For high-volume EV battery production, this means[increased throughput](https://www.ipgphotonics.com/newsroom/stories/on-the-fly-laser-welding) , higher yields, and far greater confidence that every busbar connection is made to spec. Plus, the stored measurement data enables a higher degree of traceability.


### Getting Started with a Laser Solution


Together, dual-beam fiber lasers and real-time, inline OCT weld depth measurement enable reliable, cost-effective welding of thick busbars. IPG Photonics is uniquely able to combine these technologies to deliver the optimal solution for your specific welding application.


This is because we offer the largest selection of dual-beam fiber lasers available and also build and integrate our own OCT-based laser weld measurement tool. This ensures consistently high data quality, stabilitiy, and operational reliability.


Talk to one of our laser welding experts to get started on selecting the right system for your battery welding needs.


[Get Started](https://www.ipgphotonics.com/pages/discover-your-laser-solution)


### Relevant Resources


#### [AMB Dual-Beam Lasers](https://www.ipgphotonics.com/technology/dual-beam-lasers)


High-Speed, Spatter-Free Welding Technology


#### [On-The-Fly Welding Technology](https://www.ipgphotonics.com/technology/on-the-fly-laser-welding)


Continuous Precision Laser Welding


#### [Real-Time Weld Measurement](https://www.ipgphotonics.com/technology/real-time-weld-measurement)


In-Process Laser Weld Depth Measurement
