---
schema_version: "1.0.0"
document_id: "869a94be78bb325a7f752da4698f9ab7aef10556fc3dd54cd2081e2d93c67909"
company_key: "synopsys-inc-common-stock"
company: "Synopsys Inc."
source_id: "synopsys-inc-common-stock-news-import-736729784437"
canonical_url: "https://www.synopsys.com/blogs/chip-design/rf-digital-twin-of-the-moon.html"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T22:11:37.531710+00:00"
fetched_at: "2026-08-04T23:10:21.652752+00:00"
content_hash: "sha256:00cc2d104d2bc2b857e26099af65ab7ccef3927a6c01b42347a8584cbf9aa258"
---

# Creating a Digital Twin of the Moon

##


Space missions have always depended on simulation. What has changed is the fidelity, scope, and practical value of those simulations.


As NASA and the broader space community prepare for future Artemis missions, those advances are becoming essential. Returning humans to the Moon and establishing a sustained presence there will require more than launch, flight, and landing capabilities. It will also require communications systems that perform reliably in an extreme and largely unfamiliar environment.


Every asset, assumption, and variable must be tested and validated well before launch. That includes the high-bandwidth cellular network planned for the lunar surface and the antenna-bearing systems that will depend on it, including spacesuits, rovers, landers, and orbital relays.


At Synopsys, we are helping create a digital twin of the Moon to test and validate those systems.


Working with Cesium, part of Bentley Systems, Keysight, and NASA’s Glenn Research Center through the Lunar 3rd Generation Partnership Project (Lunar 3GPP), we are combining high-fidelity topography, communications modeling, mission dynamics, and hardware validation in a virtual environment designed to support Artemis planning and operations.


**


## Modeling RF propagation across the lunar surface


Communications begin with physics. And physics on the Moon is very different from physics on Earth.


In the lunar vacuum, signals are not affected by atmospheric losses such as oxygen or water vapor absorption. But they still experience spreading loss, and they can be significantly influenced by the reflective and absorptive properties of the lunar regolith. Terrain becomes one of the most important variables in determining whether a signal reaches its destination.


That is why high-resolution lunar data is so important. Cesium’s 3D tiles, built from datasets captured by NASA’s Lunar Reconnaissance Orbiter, represent the Moon with a level of geospatial accuracy far beyond earlier models, which were too coarse for applications such as telecommunications planning.


High geospatial accuracy makes it possible to answer critical questions:


- Will a ridge interrupt line of sight between assets?
- Will a crater create a persistent coverage gap for astronauts or rovers?
- Will large boulders or terrain features create multi-path reflection interference that reduce signal quality even in good coverage areas?
- How will connectivity change as vehicles move across the surface or as surface relays shift position?


Without this kind of modeling, NASA would face fundamental limitations. There is no opportunity for physical site surveys, no iterative field testing, and no ability to walk the terrain and refine deployments over time.


Critical design decisions must be made without direct access to the environment itself. Treating terrain as an active engineering variable inside a broader mission simulation helps close that gap.


*A digital model of the lunar environment, created with*[Ansys RF Channel Modeler](https://www.ansys.com/products/missions/ansys-rf-channel-modeler) *, helps engineers see how terrain and movement could affect wireless coverage during a mission*


## Validating antenna performance on every mission asset


A communications system is only as reliable as the antennas connecting it.


In a lunar mission, antennas are embedded in everything: the cellular infrastructure itself, the spacesuits astronauts wear, the rovers they drive, the landers that carry crews and cargo, and the relays on the surface and orbiting overhead. Each of those antennas must perform in a different physical configuration, on a different platform, under different conditions of movement and orientation.


The digital twin allows engineers on Earth to evaluate that performance in context. High-fidelity antenna models are placed onto detailed representations of the assets on which they will be mounted, then assessed as those assets move through realistic mission scenarios. What looks robust in isolation may behave very differently on a rover crossing uneven terrain or in a spacesuit as an astronaut turns, bends, and moves on the lunar surface.


In addition to reducing signal quality, poor antenna performance can disrupt telemetry, delay commands, weaken situational awareness, and narrow safety margins for astronauts operating with little room for error.


By modeling antennas as part of the full mission environment rather than as standalone components, engineers can see how platform design, motion, terrain, and network dynamics affect communications performance.


*Antenna models can be tested on realistic mission assets to understand how vehicles, habitats, and equipment may affect signal performance*


## Bringing the lunar environment into the lab


Even the most sophisticated simulation has to demonstrate that it can predict how real systems will behave.


That is why the digital twin extends beyond software. Synthetic data generated from the simulated lunar environment drives physical communications hardware in the lab, enabling end-to-end validation of how systems are expected to perform under mission conditions.


Working with Keysight, this data is fed directly into RF test and measurement equipment, allowing engineers to connect the digital twin to real hardware. This creates a continuous path from modeled lunar terrain and mission dynamics to antenna behavior and real hardware performance.


That turns the model from an analytical tool into a practical test environment. Engineers can evaluate not only whether a propagation model looks realistic, but whether hardware behaves as expected when exposed to the terrain effects and signal conditions the model produces.


The speed of simulation is also a major advantage.


Because these scenario simulations can run in milliseconds, teams can test far more conditions than would be practical through conventional hardware evaluation alone. They can vary terrain position, asset orientation, relay geometry, and motion trajectories to see how the system responds across a much larger set of possibilities.


One of the most compelling outcomes is that communications performance becomes easier to interpret across disciplines. By using synthetic mission data to drive real 5G radios, engineers can do more than review plots and waveforms. They can hear what a voice connection would sound like in a lunar mission scenario — the dropouts as a rover passes behind a ridge, the static as a relay shifts overhead, and the clarity returning when line of sight is restored.


That direct experience helps multidisciplinary teams make better decisions faster. RF engineers, aerospace teams, software developers, mission planners, and program leaders can assess the same scenario from their own perspectives while working from a shared understanding of system behavior. It becomes easier to identify weaknesses, refine designs, and decide what needs to improve before launch.


At that point, the digital twin is doing more than representing the lunar surface. It is helping validate how a full communications architecture is likely to perform before any part of it is deployed on the Moon.


*Simulated lunar mission data can drive real communications hardware in the lab, helping teams evaluate performance before deployment on the Moon*


## Revealing problems before they become mission risks


The greatest value of a digital twin is not visualizing a design or confirming it will work. It is exposing where, when, and why the design may fail — early enough to do something about it.


For Artemis-era missions, that means uncovering communications vulnerabilities before they become operational risks on the lunar surface. It means testing how terrain, motion, relay geometry, and hardware behavior interact before astronauts depend on those systems in real time. And it means giving mission teams a way to refine architectures on Earth, where iteration is still possible.


A lunar digital twin is not simply a model of where missions will go. It is a way to evaluate whether mission-critical systems will hold up when conditions become difficult, dynamic, and unforgiving.


[Webinar: Wireless Channel Modeling](https://www.ansys.com/webinars/wireless-channel-modeling-for-dynamic-terrestrial-environments)


- [About Synopsys](https://www.synopsys.com/blogs/chip-design/category.about-synopsys.html)
- [Aerospace & Government](https://www.synopsys.com/blogs/chip-design/category.aerospace-and-government.html)
- [RF Design](https://www.synopsys.com/blogs/chip-design/category.rf-design.html)
- [Design](https://www.synopsys.com/blogs/chip-design/category.design.html)
- [Verification](https://www.synopsys.com/blogs/chip-design/category.verification.html)
- [Simulation](https://www.synopsys.com/blogs/chip-design/category.simulation.html)
