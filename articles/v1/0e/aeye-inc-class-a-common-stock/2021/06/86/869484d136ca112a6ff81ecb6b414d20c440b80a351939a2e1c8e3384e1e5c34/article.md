---
schema_version: "1.0.0"
document_id: "869484d136ca112a6ff81ecb6b414d20c440b80a351939a2e1c8e3384e1e5c34"
company_key: "aeye-inc-class-a-common-stock"
company: "AEye Inc."
source_id: "aeye-inc-class-a-common-stock-rss-1927df7d28ee"
canonical_url: "https://www.aeye.ai/blog/lidar-key-enabler-for-safe-autonomy/"
published_at: "2021-06-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:23:00.433930+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:420104293fbf4eeb9c0fbdd115774fff8f322709d028952a9f42af33685f0afe"
---

# LiDAR: Key Enabler for Safe Autonomy

-
-
-
-
-


AEye’s modular architecture enables seamless, cost-effective 360° coverage around the vehicle.


### **LiDAR: Key Enabler for Safe Autonomy**


***Reuters Events’ Car of the Future 2021 Webinar, Q&A***


Recently,[AEye](https://www.aeye.ai/) Co-Founder, GM of ADAS and VP of Corporate Development,[Jordan Greene](https://www.aeye.ai/company/jordan-greene/) , Continental’s VP and Head of LiDAR, Dr. Gunnar Juergens, and former General Motors CTO and GM Ventures President, Jon Lauckner, took part in a Reuters Events’ Car of the Future webinar to discuss LiDAR as the key enabler for safe autonomy. The panel looked at economic drivers for Level 2+ to Level 5 autonomy, use cases for automotive LiDAR, as well as implementation strategies and requirements, with a focus on software-driven LiDAR.


The 45-minute panel generated so much interest and discussion, the allotted time didn’t allow for AEye to answer all of the audience questions. Therefore, we’ve taken the liberty of packaging the questions into a few categories, and answering them here.


If you have any further questions, please feel free to reach out toinfo@aeye.ai .


#### Q: Why LiDAR, now? Where are the immediate opportunities?


**A:** Automakers are ready to deploy higher levels of autonomy (i.e., ADAS features), and to do so safely requires LiDAR. Cameras have great resolution and color information, but are limited in certain lighting conditions, as well as with the approximation of range. Meanwhile, radar has good performance in poor weather conditions, but doesn’t provide sufficient resolution at range, nor deal well with the certainty of objects’ locations due to multi-path. LiDAR fills in these performance gaps, and is the only deterministic sensor that can provide absolute certainty that an object is in your way, so that the car’s path-planning system can make the safest driving decision.


#### Q: Have we reached the right blend of cost-performance?


**A:** LiDAR has seen a massive price drop in the past three years, and as we see the automotive market ramp up production volume, we’ll continue to see cost-downs and economies of scale that will parallel that of radar. So, yes, we think we’ve reached the right level of cost-performance, as we see the technology improving and costs dropping to $100-$1000 for ADAS deployments.


#### Q: At what level of autonomy is LiDAR needed?


**A:** LiDAR is always additive because it’s the only sensor able to provide accurate 3D depth measurement, and it will always result in better outcomes. Currently, almost the entire automotive industry sees LiDAR as critical to unlocking safe Level 3+ autonomy — and an essential component of many safety-oriented L2+ ADAS features.


#### Q: Is there a “game-changing” L2+/2++ application?


**A:** We believe applications like “Highway Pilot” will be very desirable for automakers and consumers. In fact, in[a recent LinkedIn Poll](https://www.linkedin.com/feed/update/urn:li:activity:6804068044889956352/) , AEye asked followers which ADAS feature they would most like to have in their next car, and the overwhelming majority — 53% — chose Highway Pilot. The AEye sensor’s ability to detect small objects at range and at high speed makes our technology a great fit for this “game-changing” application.


#### Q: Where is the opportunity outside of passenger cars?


**A:** Although automotive and trucking are front-runners, we believe LiDAR will be in everything that moves. Expect to see it play an integral role in markets such as off-highway, ITS, aerospace, and rail — and AEye will be powering many of those solutions.


#### Q: What problem can LiDAR solve for L2+ that a low cost radar or camera can not?


**A:** LiDAR is a deterministic sensor that works in all lighting conditions with resolution at range capabilities. Therefore, it can solve corner cases that radar struggles with, such as determining a small object’s height in the road to determine the vehicle’s best course of action (i.e., drive over or maneuver safely around). Cameras are also great sensors, but are unable to see in conditions such as bright, direct sunlight or dim sunlight. They also cannot determine distance or size of an object with the same reliability of LiDAR.


#### Q: AEye’s approach to controllable sensors is an excellent vision for intelligent coordination, and even sensor-queuing. When might the automotive industry first demonstrate or deploy this (beyond the mode changes of today’s radars)?


**A:** AEye has multiple co-development and industrialization partnerships with automotive Tier 1s, such as[Continental](https://www.continental.com/en-us) , who are integrating these sensors into vehicles in volume starting in 2024 for OEMs globally. Industrial markets will see deployments earlier than this.


#### Q: Is there a level of perception and/or decision making that will be done with the point cloud in the LiDAR sensor unit vs the rest of the vehicle? In other words, how much intelligence is expected to be included in the LiDAR sensor when added to the vehicle? Is there a limit?


**A:** AEye’s LiDAR unit uses software configurability to dynamically generate a high resolution scan pattern that addresses customer use cases.


In addition to custom stored scan patterns, AEye’s LiDAR can utilize deterministic edge processing as well as cueing from other sensors such as cameras and radars to increase the accuracy and saliency of data.


And finally, AEye’s LiDAR can provide pre-classification information, such as, but not limited to, radial and lateral velocity of objects.


#### Q: Could you comment on the 1550nm laser emitter solution vs the 905nm emitter solution in terms of cost, range, eye safety, manufacturing and sensor compatibility?


**A:** Cost/complexity per transmitted photon is lower for 1550nm (detector responsivity is higher), and because of transmitter amplifiability, 1550nm detectors can be made much simpler and more robustly. With 1550nm fiber lasers, you get better beam quality and less susceptibility to solar background. 1550nm also penetrates through obscurants better than 905nm due to it being a longer wavelength (1550nm has about a 60% larger amplitude than 900nm), and having a much higher eye safety threshold. The 1550nm eye safety threshold for Maximum Permissible Exposure (MPE) is 100x that of 905nm. Accordingly, agile scanning with the 905nm wavelength is virtually impossible, whereas it is fundamentally safe with 1550nm.


From a manufacturing standpoint, 1550nm leads to simpler single transmitter systems, while 905nm leads to multi-transmitter (scanners and lasers) designs, where alignment of multiple scanners and laser diodes is very difficult to achieve.


#### Q: Has AEye looked at using the 905nm wavelength for shorter range to enable lower cost, particularly for the sensors (silicon) and lasers used?


**A:** While AEye’s design concept is wavelength independent, we’ve evaluated and rejected it due to the points mentioned above. We are always evaluating new technology. The short range sensor space is inherently a crowded, low-margin space.


#### Q: What are the options for LiDAR sensor placement?


**A:** AEye’s software-configurable scan patterns allow for flexible placement options in the grill, headlight, windshield and roof mount of the vehicle.


#### Q: How do you protect sensors from moisture/water and potential corrosion while maintaining the desired performance? What role/need do you see for sensor cleaning to enable LiDAR-based vision systems and ensure safe and optimal operation?


**A:** There are many ways to protect the sensor, from building a robust sealed enclosure to building the unit behind a protective surface such as a windshield. There will always be some level of degradation of any sensor over its lifetime, whether that’s from environmental damage (moisture, scratches, fogging, etc.) or from general wear and thermal cycling of the components. Automakers, for example, like to look at performance at the end of the lifetime of any component and make sure it meets their specifications — all the more reason to have a robust sensor that far exceeds OEM requirements when new.


Cleaning will always be a necessary tool for automotive (and other) applications. Whether it’s dirt or bugs, anything that blocks the optical path will attenuate the sensor’s signal, which is, again, a good reason to place sensors behind a windshield, where the windshield wipers are already cleaning the external surface.


[AEye](https://www.aeye.ai/) *is the premier provider of high-performance, AI-driven LiDAR systems for vehicle autonomy, advanced driver-assistance systems (ADAS), and robotic vision applications. AEye’s smart, software-configurable iDAR™ (Intelligent Detection and Ranging) platform combines solid-state, active LiDAR, an optionally fused low-light HD camera, and integrated deterministic artificial intelligence to capture more intelligent information with less data, enabling faster, more accurate, and more reliable perception. AEye has partnered with leading Tier 1s — such as Continental, Hella, and Aisin — and system integrators to configure and manufacture the sensor at scale to meet the diverse performance and functional requirements of autonomous and partially automated applications.*
