---
schema_version: "1.0.0"
document_id: "4e87b45fb83fadc407b0d96fd4e479883c7851c066dc0620979da0aa2562b6af"
company_key: "aurora-innovation-inc-class-a-common-stock"
company: "Aurora Innovation Inc."
source_id: "aurora-innovation-inc-class-a-common-stock-rss-b929390c0bbe"
canonical_url: "https://aurora.tech/newsroom/inside-auroras-photonic-engine-engineering-lidar-for-automotive-scale"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:45.755868+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:f05b2621c7c4af8bcd72196821adff27514f29ea2254a9211c08b5f4df0f33fc"
---

# Inside Aurora’s Photonic Engine: Engineering Lidar for Automotive Scale

*To move our FMCW lidar from discrete optics to an automotive-grade photonic engine built for scale, Aurora has assembled a team with deep expertise in integrated photonics, compound semiconductor design, and precision optical manufacturing.*


*This blog was written by three members of that team:*[James Ferrara](https://www.linkedin.com/in/james-ferrara-a2650753/) ***,*** **[Phillip Sandborn](https://www.linkedin.com/in/phillip-sandborn-lidar/) ***,*** *and*[Parth Panchal](https://www.linkedin.com/in/parth-panchal/) ***.***


Imagine building a sensor that manipulates light at the nanometer scale (70,000 times narrower than a human hair) while detecting objects a kilometer away. It has to do that while surviving road vibration, temperature extremes, and the reliability standards of the automotive industry, in a package small enough to fit in the palm of your hand.


Aurora’s Firstlight LiDAR photonics engine


These are the challenges we work on every day as part of the Aurora Lidar team. In the transition from[development and testing](https://aurora.tech/newsroom/firstlight-lidar-on-a-chip) of our lidar-on-a-chip into a product qualified for commercial operations, the demands on our sensors have sharpened. Bottom-line sensor performance still matters, but the system also has to be manufactured repeatably, qualified to automotive standards, and built in much larger volumes. At Aurora, we’ve developed high-performance lidar sensors over many years, and the work ahead is just as significant. Our near-term goal is to put thousands of self-driving trucks on the road, and our photonic engine is central to that mission.


## How the Aurora Driver sees the road


The Aurora Driver perceives its environment through a combination of lidar, radar, and cameras, each playing a crucial role in building a comprehensive picture of the road ahead. For lidar, the Aurora Driver uses an FMCW (Frequency-Modulated Continuous-Wave) approach. Rather than firing short pulses and timing their return (the way a bat uses echolocation), FMCW lidar sends out a continuous beam whose frequency changes in a known pattern, then mixes the return signal with a local reference beam. That approach proves immensely valuable when driving an 80,000 pound robot at highway speeds. Because moving objects impose a doppler shift on the return light, every point in the point cloud carries its own instantaneous velocity. The Aurora Driver can distinguish a stopped vehicle from one merging across a lane without waiting to compare multiple returns to determine the object’s velocity. And because the receiver only responds to light that matches the emitted frequency and timing, sunlight and other vehicles' lidar read as noise rather than as phantom objects.


High-density point cloud from Aurora’s lidar test range in Bozeman, MT


The next generation of our FirstLight lidar reaches up to one kilometer of range, roughly double that of the closest FMCW competitor. At typical highway speeds, this can give the Aurora Driver more than 34 seconds to react to future scenarios accordingly.


## **How we got here**


The photonic engine did not arrive in one breakthrough. It is the product of years of acquired expertise and iteration. The FMCW lidar approach is core to our self-driving truck strategy. The architecture and signal-processing depth that came with the acquisition of Blackmore in 2019 is what first let us train and test Aurora Driver-powered trucks on public highways, fulfilling the company mission of deploying self-driving technology safely and quickly. However, FMCW implemented with discrete optics meant bulky assemblies of fiber, lasers, and transceivers, workable for a test fleet but expensive and likely impractical for a real production fleet. The silicon photonics and packaging expertise of the OURS team, acquired in 2021, gave us a path to condense those discrete parts onto chips. By 2023 we had demonstrated the individual optical functions working in chip form. Today, we have iterated over multiple generations of design and fabrication, refining both the photonic building blocks and the packaging that holds them together. Photonic integration is what makes it possible to now deploy a high-performance lidar sensor at commercial scale.


## **Owning the lidar stack**


The atomically thin epitaxial layers of our lasers and optical amplifiers, the low-loss silicon-nitride waveguides, the high-responsivity photodiodes, even the microoptical couplers that route light in and out of the engine: each of these gives us multiple opportunities to optimize the sensor for performance, manufacturing, testing, and validation against strict automotive qualification standards. We understand exactly how our hardware behaves across operating conditions, iterate on chip designs quickly guided by performance data across large datasets, and build toward automotive qualification with full visibility into every layer of the stack. Owning the design, the IP, the process know-how, and the manufacturing transfer is what allows us to build a high-performance lidar sensor at the volume commercial deployment requires.


## **What the photonic engine does**


Our photonic engine is a multi-chip module that consolidates the core optical functions required for lidar into a single compact assembly: light generation, amplification, modulation, transmission, and detection. Two device families do most of the work. Silicon photonic integrated circuits route and manipulate light within the system. Our latest generation delivers more than 10% lower on-chip optical loss than the prior generation, and our custom-designed photodiode achieves 25% higher sensitivity than the comparable foundry library components. After a kilometer, the returning light is extraordinarily faint, and detection comes down to whether enough photons clear the noise floor. Every fraction of a decibel of loss removed inside the module, and every bit of sensitivity added at the detector, adds up to photons that make the difference between seeing an object or not. Light is generated and amplified by compound semiconductor (III-V) devices: a semiconductor laser source and custom semiconductor optical amplifiers, or SOAs. The silicon photonics route modulates and detects light; the III-V components generate and boost the light. We developed our amplifiers in-house in under two years, and co-designed them with the silicon photonic chips from the start. This lets us move past basic datasheet comparison for vendor-produced parts and towards a perfectly tuned system. This “custom-design” philosophy is core to how we hit loss and efficiency targets that the system requires. The system is also fiber-free. Fiber connectors in vehicle environments accumulate fatigue damage from continuous vibration. Over hundreds of thousands of miles, that mechanical stress degrades optical coupling and introduces failure modes that are difficult to predict or inspect. Our design routes light through integrated photonics and precision-engineered optical interfaces instead, eliminating that failure mechanism and giving us a more compact, qualification-friendly package.


## **The hardest part: moving light between chips**


A core capability to achieve the “perfectly-tuned” system for reliable automotive conditions is our ability to perform active alignment of microlenses between chips during the assembly process. Moving light reliably between multiple chips inside a compact package requires sub-micron alignment tolerances, maintained under vibration, thermal cycling, and years of service life on a commercial truck. The process that achieves this has to be repeatable at manufacturing scale. In a lab, you can take your time with each alignment. On a production line, you don’t have that luxury.


The Aurora photonic engine in action: light is emitted and received by the module at the heart of our Firstlight system (photo captured with an infrared-sensitive filter with a long exposure)


So we align lens couplers actively. As each lens is placed, we send light through our chips and monitor the coupled optical power in real time, minutely adjusting the lens position until the signal peaks, then fixing it there. Running that with automated sensor-driven feedback on every optical path is what turns laboratory-grade coupling into a repeatable manufacturing step. This active-alignment know-how and expertise has been developed over several years, and is essential to bringing the photonics engine to production.


## **Building for production**


Performance is necessary but not sufficient for an automotive-grade system. The harder bar is proving the part can be built the same way, to the same spec, every time, at volume. Automotive supply chains run on frameworks like PPAP (Production Part Approval Process), which require documented evidence of design validation, process control, and measurement system capability before a part is approved for production. It is how an OEM gains confidence that a supplier can deliver consistent quality across hundreds of thousands of units, not just one good prototype. We have designed the photonic engine toward that bar from early in the program rather than treating qualification as a final gate. In practice that means building manufacturability into the architecture, standing up internal test platforms that screen components across large statistical datasets, and tracking performance trends across design generations. It also means taking on a harder version of the problem than most photonics programs face. Because we mount bare dies and align them optically inside the package, qualifying the assembly means qualifying the process that builds it, not just dropping in parts that arrive pre-qualified. That is more work up front, and it is the right kind of work for a part that has to survive years of service on the road.


## **What comes next**


The photonic engine we have today is one stage in a longer development story. The photonics industry is moving toward higher levels of integration: co-packaged optics, hybrid approaches that consolidate multiple optical functions into fewer components. Done right, that means smaller packages and lower cost without sacrificing the reliability standards automotive deployment requires. We are already exploring what that looks like for our architecture. Concretely, our next step is pulling more of these discrete optical functions onto fewer chips while leveraging the alignment precision and reliability margins we have spent years building. Integration is easy to chase if you are willing to relax the automotive constraints. We are not. This work spans nanophotonics, compound semiconductor design, precision micro-optical assembly, automotive qualification, and systems engineering at scale. Most teams spend careers in one of those domains. At Aurora, they all converge on a single problem – with real trucks moving freight on public highways depending on the answer. There are still plenty of hard problems in autonomous sensing to solve. That is what makes this work worth doing.
