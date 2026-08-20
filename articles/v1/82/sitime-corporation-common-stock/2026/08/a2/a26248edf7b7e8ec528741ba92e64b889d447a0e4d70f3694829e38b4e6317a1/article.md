---
schema_version: "1.0.0"
document_id: "a26248edf7b7e8ec528741ba92e64b889d447a0e4d70f3694829e38b4e6317a1"
company_key: "sitime-corporation-common-stock"
company: "SiTime Corporation"
source_id: "sitime-corporation-common-stock-news-import-d9f31ba801b6"
canonical_url: "https://www.sitime.com/company/newsroom/blog/new-gnss-disciplined-oscillator-24-hour-holdover-postage-stamp-size"
published_at: "2026-08-10T13:00:00+00:00"
first_seen_at: "2026-08-10T18:55:30.816366+00:00"
fetched_at: "2026-08-10T18:55:31.577419+00:00"
content_hash: "sha256:95d033f6b6eb78de8628c1f76c7e20bd470497a0973a52e8c403d45f939b8cd8"
---

# New GNSS-Disciplined Oscillator—24-Hour Holdover in a Postage-Stamp Size

# New GNSS-Disciplined Oscillator—24-Hour Holdover in a Postage-Stamp Size


August 10, 2026


|


by


[Tyler Hohman](https://www.sitime.com/company/newsroom/author/tyler-hohman)


|


7 min read


Image


During a severe sandstorm, a transport aircraft attempts to land at a remote forward operating location. Visibility is nearly zero, forcing the crew to rely on navigation, communications and landing-assistance systems. If the aircraft were to lose its GNSS signal, local timing sources would drift. Even a few microseconds of drift could cause these systems to lose synchronization. This can cause communication disruptions, impacts to radar and ranging systems, higher inertial navigation error and ultimately reduce the accuracy of information displayed to the pilot. Lives and the mission could be at stake. In this scenario, the fallout would not stem from a hardware failure or cyberattack, but from a small timing error.


The[MicroPNT GDO-1000, a GNSS disciplined oscillator (GNSS-DO) module](https://www.viavisolutions.com/en-us/products/micropnt-gdo-1000) , new from VIAVI, a global leader in test and measurement, position, navigation and timing (PNT) and optical technologies, is built for situations like this. It features SiTime’s MEMS-based Endura® Epoch or Elite X® oscillators to deliver microsecond-class, 24-hour holdover in the smallest package.


The VIAVI µPNT GDO-1000 nears atomic-clock timing holdover precision, but with a low size, weight, power and cost (SWAP-C) profile—in a footprint the size of a postage stamp. The solution provides greater flexibility to keep networks synchronized, sensors aligned and mission systems operating with confidence across air, land, sea, space and cyber domains—even in extreme environments.


##
What Is a GNSS-DO?


GNSS signals can be vulnerable to jamming or spoofing. Signals can be interrupted in urban or geographic canyons and subject to outages. A GNSS-DO combines a local precision oscillator with timing from GNSS to provide the accurate and resilient timing required by modern defense systems. Typically, GNSS-DOs are comprised of the following:


- GNSS receiver
- High-stability local oscillator (Crystal, MEMS TCXO or OCXO, or an atomic clock)
- Disciplining algorithm (servo loop)
- 1 pulse per second (PPS) input and a 1 PPS and 10 MHz output


Under normal operating conditions, the GNSS receiver serves as the primary timing reference for the GNSS-DO, providing output signals, such as 1 PPS and 10 MHz synchronized to the GNSS 1 PPS rising edge. During this phase, the GNSS-DO continuously learns and characterizes the drift behavior of the local oscillator relative to the GNSS reference.


If GNSS signals become degraded, jammed or denied, the GNSS-DO seamlessly transitions to the local oscillator as the timing source. Leveraging this learned behavior, the system actively compensates its outputs to maintain accurate time alignment. This unique combination of long-term accuracy and short-term stability makes the GNSS-DO a critical enabler for military communications, radar systems, electronic defense platforms, sensor fusion networks and autonomous systems—where precise timing is essential for mission success and operational continuity.


##
Choosing an Oscillator with the Right Stuff


Drift can be caused by temperature fluctuations, vibration, mechanical stress, power supply variations and long-term aging effects. Left unchecked, drift accumulates over time, degrading synchronization accuracy and potentially impacting system performance. The role of the GNSS-DO is to continuously correct these errors when a reference signal is available and minimize their impact when it is not. The better the oscillator's inherent stability, the longer and more accurately the system can maintain synchronization during a GNSS outage. This is how the different types of oscillators compare:


- **CSACs**
Chip-scale atomic clocks (CSACs) offer great stability and the longest holdover performance, making them the benchmark for resilience in GNSS-denied environments. However, these advantages come with tradeoffs in size, weight, cost and procurement lead times that can limit their practicality for many deployed systems.
- **Crystal TCXOs and OCXOs**
Traditional crystal-based TCXOs and OCXOs provide lower-cost alternatives and are widely used across communications, aerospace and defense applications. While they can deliver strong timing performance, quartz is very susceptible to shock, vibration and mechanical stress, causing them to crack, break or severely degrade performance. This makes them less suitable for increasingly rugged and mobile platforms.
- **MEMS TCXOs and OCXOs**
Micro-Electro-Mechanical Systems (MEMS)-based TCXOs and OCXOs are emerging as a best-of-all-worlds solution. They provide robust resistance to vibration, shock and environmental stress while significantly reducing size and power consumption. In many cases, MEMS OCXOs offer near atomic-level frequency stability over temperature and occupy 75x less volume. This combination enables system designers to achieve precise timing and extended holdover without the SWaP-C penalties.


Image


*SiTime OCXOs beat the alternatives for SWAP in critical military defense applications.*


"Until now, customers needing reliable timing in compact systems have had to choose between two suboptimal options. CSACs are expensive and supply-constrained. Full-size OCXO-based timing solutions are too large and power-hungry for many modern platforms," said Doug Russell, senior vice president and general manager, Aerospace & Defense,[VIAVI](https://www.viavisolutions.com/en-us) . "The GDO-1000 offers a new path that doesn't force customers to compromise.”


##
About VIAVI µPNT GDO-1000


The µPNT GDO-1000 comprises a tightly integrated set of capabilities that combine resilience, precision and compact design. It features dual-frequency L1/L5 GNSS reception with microsecond-class, 24-hour holdover, enabling highly accurate timing even in degraded or contested conditions. “Its holdover performance approaches what customers expect from atomic-class clocks, in a module that fits on a standard M.2 slot and draws approximately half a watt,” said Russell. It integrates directly into modern compute platforms, time appliance cards and embedded systems without custom mechanical design.


Image


*The VIAVI µPNT GDO-1000 featuring a SiTime MEMS oscillator has dual frequency L1/L5 GNSS reception, microsecond-class 24-hour holdover and draws less than half a watt of power.*


µPNT GDO-1000 performance is enhanced by patented AI and ML algorithms which model and compensate for oscillator behavior across varying environmental conditions. At its core, SiTime’s MEMS oscillators provide improved thermal stability across the full military temperature range compared with traditional quartz OCXOs, while maintaining phase noise and Allan Deviation performance under vibration and shock.


The system also accepts an external 1 PPS input, allowing discipline from M-Code GPS, alternative navigation sources or other external references without hardware modification. Despite its miniature size, it supports multiple 1 PPS and low-phase-noise 10 MHz coaxial inputs and outputs, providing flexible integration across complex systems.


“Precision Timing is the heartbeat of all electronics,” said Sundar Vanchinathan, general manager of the Aerospace and Defense business at SiTime. “The µPNT GDO-1000 module with its advanced AI/ML holdover algorithms together with our MEMS OCXOs delivers resilient timing precision and extended service continuity in a compact, low-SWaP solution for aerospace and defense applications.”


##
How SiTime MEMS Oscillators Drive VIAVI µPNT GDO-1000 Holdover Performance and Reduce Oscillator Drift


Modern precision timing solutions such as an advanced OCXO platform and high-performance Super-TCXO® architectures are designed to directly address the combined challenges of SWaP constraints, oscillator drift and loss of reference time in GNSS-degraded environments. By improving intrinsic frequency stability, reducing phase noise and minimizing long-term aging effects, SiTime devices help maintain more accurate local timekeeping when external references are unavailable:


- **Endura Epoch OCXO:** Enhances holdover performance by tightly regulating temperature, which reduces environmental sensitivity and slows the accumulation of timing error during extended outages, while still optimizing power and size compared to legacy high-stability clocks.
- **Endura Elite-X Super-TCXO:** Improves thermal stability and long-term reliability, reducing drift across wide operating conditions, helping preserve synchronization integrity in compact, power-constrained systems.


SiTime MEMS oscillators reduce dependence on continuous external timing inputs and ensure that when reference time is lost, system clocks preserve mission continuity and system-level coherence.


##
Precision Timing Is Critical Infrastructure—The Oscillator Matters


As defense systems evolve from standalone platforms into interconnected networks of sensors, communications systems and autonomous assets, timing is becoming as fundamental to system design as power and memory management. Precision timing enables everything from PNT to secure communications, sensor fusion and synchronized operations across distributed missions.


The future belongs to timing solutions that deliver precision, resilience and performance while minimizing SWaP and system complexity. The VIAVI µPNT-GDO-1000 with a SiTime MEMS oscillator on board, provides a powerful path forward, combining accurate synchronization with robust holdover performance in a compact, power-efficient design—a solid foundation for mission success.


Learn more:


- [VIAVI µPNT GDO-1000 GNSS-DO](https://www.viavisolutions.com/en-us/products/micropnt-gdo-1000)
- [Endura Epoch OCXO](https://www.sitime.com/products/ruggedized-timing/ocxos)
- [Elite X TCXO](https://www.sitime.com/elitex-super-tcxo)


## Tags


- [Technology](https://www.sitime.com/company/newsroom/blog?tags=technology) |


- [Precision Timing](https://www.sitime.com/company/newsroom/blog?tags=precision_timing) |


- [Aerospace-Defense](https://www.sitime.com/company/newsroom/blog?tags=aerospace_defense)


##


How can we help you?


[Contact Us](https://www.sitime.com/contact-us)


[Request Samples](https://www.sitime.com/request-samples)
