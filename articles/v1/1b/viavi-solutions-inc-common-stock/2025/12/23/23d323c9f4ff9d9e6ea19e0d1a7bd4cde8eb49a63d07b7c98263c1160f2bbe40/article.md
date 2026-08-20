---
schema_version: "1.0.0"
document_id: "23d323c9f4ff9d9e6ea19e0d1a7bd4cde8eb49a63d07b7c98263c1160f2bbe40"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2025/12/03/micro-transcoder-advances-enable-lower-cost-highly-accurate-gps-simulators/"
published_at: "2025-12-03T18:34:05+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:55:05.658267+00:00"
content_hash: "sha256:ef6c78c9eb5879340d7aca28965fda7d2b51456a969bdda6d822f7f175ba950d"
---

# Micro-Transcoder Advances Enable Lower Cost, Highly Accurate GPS Simulators

**


# Micro-Transcoder Advances Enable Lower Cost, Highly Accurate GPS Simulators


- **Ed Latimer
- ** December 3, 2025
- Like


A persistent challenge looms for avionics technicians and maintainers: how do you test systems that rely on GPS when an aircraft is undergoing servicing and in the hangar?


In this blog, we examine GPS repeaters, GPS simulators, and how advances in micro-transcoding technologies are enabling GPS signals to be delivered at a fraction of the cost of what’s previously been possible.


### **GPS Repeaters**


For aviation safety, GPS-dependent aircraft systems need to be incredibly accurate. Getting a GPS signal indoors has traditionally relied on GPS repeaters. These GPS re-radiators capture live satellite signals from an external antenna and rebroadcast them within the hangar. Deploying repeaters often introduces significant logistical and technical challenges, such as the need for professional installation including site surveys and precise calibration, all while navigating stringent regulatory requirements, which often include recurring license fees.


Governing bodies like the FAA and the FCC in the US maintain tight control over the radio spectrum and rebroadcasting of GPS signals (see Ofcom, BNetzA, ANFR, and MIIT for the UK, Germany, France, and China respectively). Doing so prevents false location signals (spoofing) from being broadcast and affecting other aircraft, air traffic control, and other essential services reliant on accurate position information.


As using GPS repeaters carries a significant risk of interfering with vital systems, any license granted by such bodies requires containment of the re-radiated signal, typically within an RF Faraday cage. The license applications can be complex, time-consuming, and often difficult to obtain.


This isn’t the only challenge with GPS repeaters, as real GPS signals (no matter how weak) can also enter the hangar, potentially causing unreliable position locks and inconsistent test results.


### **GPS Simulators**


GPS simulation offers a controlled, reliable, and compliant alternative. Instead of taking external signals and rebroadcasting them, simulators can create a completely synthetic constellation of satellites. Rather than being broadcast, this simulated signal can be directed into the aircraft’s GPS antenna via an antenna coupler or plugged straight into the aircraft GPS receiver’s antenna input.


GPS simulators, therefore, do not face the same regulatory or interference problems as GPS repeaters. Additionally, simulator capabilities can extend beyond simply providing a static position fix. Advanced simulation allows technicians to set any desired location in latitude, longitude, and altitude to validate avionics systems in dynamic, real-world scenarios while still in the hangar.


For example, testing an ADS-B (automatic dependent surveillance-broadcast) system can begin with a simple static simulation to confirm the transponder is correctly broadcasting its basic position. However, a complete and comprehensive validation requires the aircraft to report changing position data. To achieve this, the simulator can generate a precise flight path with dynamic routes that simulate movement, allowing technicians to verify that the ADS-B transponder is transmitting the correct information as the plane “flies” the route.


Furthermore, GPS simulation capability could be used to train and familiarize pilots with new cockpit instrumentation, run through approach procedures or flight plans while parked securely in the hangar, or test XM weather warning systems by virtually repositioning the aircraft in real-world weather conditions.


### **GPS Simulator Requirements**


The market for GPS simulators is growing rapidly, with new products exhibiting ever-smaller form factors, enhanced accuracy, and increased functionality. These high-end systems are essential for design engineers and deep diagnostic work, enabling the simultaneous simulation of multiple GNSS constellations (GPS, GLONASS, Galileo, BeiDou) across multiple frequencies (L1, L2, L5).


High-end simulators can also replicate precise atmospheric conditions and historical satellite errors that have caused known flight anomalies. But while this level of fidelity is critical for research and development or certifying new avionics from scratch, it represents a significant capital investment, and the complexity of such systems mean they can often cost several tens of thousands of dollars per unit.


For the avionics technician, most tests requiring GPS signals only need static positional data supplied to the avionics for functional checks and routine maintenance verification. As such, a simpler solution is preferred.


### **Advances in Micro-Transcoders for Next-Gen GPS Simulators**


The VIAVI[OSPREY™](https://www.viavisolutions.com/en-us/products/osprey-gps-simulator) ( **O** perational **S** imulated **P** osition for the **R** eal-Time **E** valuation of S **y** stems) is a lower-cost GPS simulator based around Jackson Labs’ transcoder technologies. Jackson Labs, acquired by VIAVI in 2022, is renowned for its expertise in timing and frequency technology, which is also behind world-leading remote secure receiver (RSR) transcoders used in[assured PNT systems](https://www.viavisolutions.com/en-us/products/rf-transcoder) . This synergy enabled the creation of a streamlined product that can provide a simulated GPS signal at a fraction of the cost of other systems.


The OSPREY™ started as a proof-of-concept Arduino-based prototype based on the[CLAW GPS Simulator](https://www.viavisolutions.com/en-us/products/claw-gps-simulator) , while the production version of the OSPREY™ implements[VIAVI’s latest micro-transcoder](https://www.viavisolutions.com/en-us/products/micro-transcoder-10-channel-gps-simulator) . This small but powerful device is just 1-inch square and capable of simulating GPS L1 signals across 10 channels for a fast lock. The micro-transcoder is integrated into the OSPREY™, which hosts a wireless personal area network (WPAN) and runs off internal batteries for untethered operation.


The OSPREY™ is completely self-contained, requiring no external power or data cables, which makes setup quick and effortless. An avionics technician simply positions the OSPREY™ over the antenna, and the integrated coupler provides >20 dB of isolation from external signals. A dual coupler solution is available for aircraft with dual GPS antennas. For tall aircraft, the coupler housing includes a hook that allows placement with an extension pole. Although the enclosure is optimized for single or dual GPS antenna configurations, it can also connect directly to an aircraft’s GPS receiver, enabling both direct and bench testing of any GPS-enabled device.


System control is via a remote-control interface in a mobile application available for iOS and Android devices, allowing a static location or a dynamic flight path to be drawn on a map in seconds. The mobile app allows storage, recall, and simulation of up to 15 static waypoints and an additional 10 dynamic routes, each with 10 waypoints. It also automates synchronization of the GPS simulator with the latest almanac data with the press of a button.


### **Summary**


The advances in micro-transcoder technology have made GPS simulators far more affordable and practical for everyday avionics maintenance, minimizing regulatory hurdles and interference risks, and allowing technicians to perform both static and dynamic testing inside hangars. The VIAVI Solutions OSPREY™ GPS Simulator exemplifies this technological transformation, offering a compact, self-contained, lower-cost solution with an easy-to-use mobile app control for streamlined testing and training.


If you’d like to find out more about the VIAVI Solutions OSPREY™ or our laboratory-grade GPSG-1000, more information is available via our[GPS Signal Simulators page](https://www.viavisolutions.com/en-us/products/gps-signal-simulators) .


- ** Categories:[Aerospace and Defense](https://blog.viavisolutions.com/category/aerospace-and-defense/) ,[Aviation](https://blog.viavisolutions.com/category/aviation/)
- ** Tags:[avionics](https://blog.viavisolutions.com/tag/avionics/) ,[GPS simulators](https://blog.viavisolutions.com/tag/gps-simulators/)


-
-
-
-
-
-
