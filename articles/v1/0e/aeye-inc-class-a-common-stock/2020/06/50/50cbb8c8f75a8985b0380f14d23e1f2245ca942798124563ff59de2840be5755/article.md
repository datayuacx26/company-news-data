---
schema_version: "1.0.0"
document_id: "50cbb8c8f75a8985b0380f14d23e1f2245ca942798124563ff59de2840be5755"
company_key: "aeye-inc-class-a-common-stock"
company: "AEye Inc."
source_id: "aeye-inc-class-a-common-stock-rss-1927df7d28ee"
canonical_url: "https://www.aeye.ai/blog/the-odyssey-of-fmcw/"
published_at: "2020-06-12T07:00:00+00:00"
first_seen_at: "2026-07-20T23:23:00.433930+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:2cc22cdd09c3face8519d03ef4a61a58ee99ee2a27fbf1528055956fedef8234"
---

# The Odyssey of FMCW

-
-
-
-
-


### **The Odyssey of FMCW**


**An Epic Introduction to AEye’s Latest White Paper** *Time of Flight vs. FMCW LiDAR: A Side-by-Side Comparison*


Everyone loves a good myth. From stories told around the campfire to the great classics by Homer or Virgil, myths have endured as a powerful force that has rooted itself in the very fabric of humanity’s collective identity. But myths are not just stories. They can be romantic exaggerations of the truth. Just as the Sirens in Homer’s *The Odyssey* attempted to lure Odysseus to his doom with their enchanted songs, proponents of Frequency Modulated Continuous Wave (FMCW) LiDAR systems mythologize the value and performance of the technology, leading the autonomous vehicle industry astray and — quite possibly — to shipwreck.


As the result of a recent virtual roundtable AEye hosted with[AutoSens](https://auto-sens.com/) , we developed a white paper that frames out some of the essential points of comparison between FMCW and Time of Flight (TOF) LiDARs. This provides an introduction to that[paper](https://www.aeye.ai/resources/white-papers/time-of-flight-vs-fmcw-lidar-a-side-by-side-comparison/) .


#### [Myth #1: FMCW is a (new) revolutionary technology](https://www.aeye.ai/resources/white-papers/time-of-flight-vs-fmcw-lidar-a-side-by-side-comparison/#claim1)


Contrary to the myth, FMCW LiDAR has been around since 1967, the same year The Beatles released their innovative *Sgt. Pepper’s* album and radically transformed the boundaries of pop music. But like an original vinyl copy of *Sgt. Pepper’s* , FMCW is well worn and appreciated, but no longer revolutionary. Its beginnings stem from work done at MIT Lincoln Laboratory — only seven years after the laser itself was invented. What *has* changed in recent years is the higher availability of long coherence-length lasers. While this has rejuvenated interest in the established technology, there are still several limitations that must be addressed for the successful evaluation and integration into autonomous vehicles.


#### [Myth #2: FMCW detects/tracks objects farther, faster](https://www.aeye.ai/resources/white-papers/time-of-flight-vs-fmcw-lidar-a-side-by-side-comparison/#claim2)


When you write your own story, you have the luxury of leaving out any unflattering details. That’s why, when it comes to measuring FMCW tracking and detection range, there is one critical dimension that is rarely addressed: laser shot rate (e.g. latency). Tracking and detection range both are heavily influenced by laser shot rate, because higher laser shot density (in space and/or time) provides more information that allows for faster detection times and better noise filtering. In addition, shot rate allows you to get longer range with greater confidence. While TOF LiDAR offers very fast laser shot rates (several million shots per second in the AEye system), many FMCW ones are only capable of shot rates in the 10’s to 100’s of thousands of shots per second, which is roughly 50x slower.


An agile TOF system will detect small, low reflectivity objects faster and at over 200M range. Although the myth claims that FMCW has superior range, in reality, there hasn’t been an FMCW system that can match the range of an advanced TOF system.


#### [Myth #3: FMCW measures velocity and range more accurately and efficiently](https://www.aeye.ai/resources/white-papers/time-of-flight-vs-fmcw-lidar-a-side-by-side-comparison/#claim3)


Like squandering your life away in search of Bigfoot or the Loch Ness Monster, combining the measurements for instantaneous velocity proves meaningless in the case of autonomous vehicle applications.


When compared to the myth of FMCW, the TOF system requirement of multiple laser shots to determine target velocity seems like extra overhead. There are two types of velocity, radial (objects moving toward each other directly or collinear) and lateral (objects moving in perpendicular directions). While measuring radial velocity is critical,[accurately measuring lateral velocity impacts an AVs ability to address over 90% of the most dangerous edge cases, such as a car running a red light, swerving vehicles, or a child darting onto the street.](https://www.aeye.ai/idar/#idar-in-action) FMCW does not and cannot measure lateral velocity.


Let’s also consider how much a target moves during an FMCW measurement and then compare that with the range accuracy offered by that FMCW system. To establish a velocity that is statistically significant, a target should move at least 2cm, which takes about 500us. To capture velocity, an FMCW system is captive this entire dwell time.


Compounding the captivity time, FMCW often requires a minimum two laser sweeps (up and down) to form an unambiguous detection, with the down sweep providing information needed to overcome ambiguity arising from the mixing range + Doppler shift — doubling the dwell time.


While an FMCW system is waiting 500us to estimate velocity, an agile TOF LiDAR would use this time wisely to look at other targets before returning to the original target for a timely velocity measurement. This results in better quality sensor information being captured faster and delivered to the perception system.


#### [Myth #4: FMCW has less interference](https://www.aeye.ai/resources/white-papers/time-of-flight-vs-fmcw-lidar-a-side-by-side-comparison/#claim3)


Like Odysseus on his 10-year epic journey home after the fall of Troy, LiDAR systems must contend with multiple types of interference, and FMCW is no exception. There are two types of interference: sensor to sensor interference and environmental interference. FMCW regularly suffers environmental interference challenges from sidelobes. Inspect the point cloud quality of TOF vs FMCW under various driving conditions for yourself. You’ll discover that the multitude of potential sidelobes in FMCW leads to artifacts that impact not just local range samples, but the entire returned waveform for a given pulse.


FMCW also experiences frequent interference from clutter and reflections caused by first surface detection, such as a windshield. These reflections will be continuous and very strong, relative to distant objects, ultimately creating undesirable FFT (time) sidelobes in the transformed data. Interference from both clutter and reflections from first surface detection can significantly impact the usable dynamic range of the sensor.


#### [Myth #5: FMCW is automotive grade, reliable, and readily scalable](https://www.aeye.ai/resources/white-papers/time-of-flight-vs-fmcw-lidar-a-side-by-side-comparison/#claim5)


As depicted in Virgil’s *Aeneid* , the Greeks deployed a classic act of wartime subterfuge during the Trojan War when they used a wooden horse filled with soldiers to enter and ultimately destroy the city of Troy. As the Trojan Horse wasn’t what it seemed, the readiness of FMCW components to be reliably scalable cannot be taken at face value.


While the components used in TOF LiDAR systems are derivatives of components used in cable TV, telecom, and other industries, the new developments are the[MEMS (micro-electromechanical system)](https://medium.com/for-sight/not-all-mems-are-created-equal-f9c3f7a3cfa4) which have been previously used in virtually all air bag and pressure sensors. In short, TOF systems offer a mature automotive grade supply chain whose components are all already produced in high volume.


In comparison, the most critical component for FMCW systems is the very low phase noise laser, which has many tight requirements and no other high-volume user to help drive down volume manufacturing costs. FMCW components also include complex optics and high-speed ADC and FPGA, which are all low volume, custom, and expensive.


In contrast, TOF LiDARs already have multiple vendors selling automotive qualified components across the entire hardware stack: lasers, detectors, ASICs, etc.


#### [Myth #6: Adding FMCW to Optical Phased Arrays (OPAs) will compensate for lack of solid-state performance of FMCW](https://www.aeye.ai/resources/white-papers/time-of-flight-vs-fmcw-lidar-a-side-by-side-comparison/#claim6)


The mythical combination of a Unicorn and a Pegasus produces a third mythical creature — the Alicorn — a mythical horse with both a horn and wings. The combination of FMCW and OPAs would breed a similar mythical offspring. FMCW has a low technical readiness level and OPAs have an even lower technical readiness level (roughly TRL 3). OPAs are distinctly short-range detectors. Combining them with FMCW won’t improve that. In addition, several factors limit the usefulness of OPAs in LiDAR applications, including the ability to handle transmit power in very small optical waveguides, and that some OPAs use thermal shifting of laser wavelength to steer beams in one dimension while using phased arrays to steer beams in another dimension.


The combination of a beam steering mechanism that depends on the laser being a constant CW signal, while the ranging mechanism depends on sweeping the frequency of the laser, doesn’t work well for traditional FMCW approaches. The idea of combining FMCW with a beam steering technology that is in such an early stage of development is incredibly risky and could take another 10 years to reach usable maturity.


A better way to leverage coherence would be to combine it with an agile, software-configurable LiDAR system. This combination would not be mythical, it would be legendary.


#### **Conclusion**


If you believed in the myths of FMCW, we’re sorry to inform you that it’s all just been a Trojan Horse: FMCW is not a new, revolutionary technology, nor does it detect and track objects farther and faster. It certainly does not measure velocity and range more accurately and efficiently, or have less interference than TOF LiDAR systems. It is not automotive grade or readily scalable — and adding it to an OPAs won’t fix or improve its shortcomings. Coherence isn’t bad, but its implementation in FMCW LiDAR is wasted. AEye knows that high shot-rate, agile-scanning TOF systems serve the needs of autonomous vehicle LiDAR more effectively than FMCW when range, performance, and point cloud quality are important.


Want to learn more? Ditch the myths and read the facts in our latest white paper,[Time of Flight vs. FMCW LiDAR: A Side-by-Side Comparison.](https://www.aeye.ai/resources/white-papers/time-of-flight-vs-fmcw-lidar-a-side-by-side-comparison/#claim6)


#### About AEye


[AEye](https://www.aeye.ai/) is the premier provider of high-performance, AI-driven LiDAR systems for vehicle autonomy, advanced driver-assistance systems (ADAS), and robotic vision applications. AEye’s smart, software-configurable[iDAR™ (Intelligent Detection and Ranging)](https://www.aeye.ai/idar/) platform combines solid-state, active LiDAR, an optionally fused low-light HD camera, and integrated deterministic artificial intelligence to capture more intelligent information with less data, enabling faster, more accurate, and more reliable perception. The company is backed by world-renowned investors including Kleiner Perkins Caufield & Byers, Taiwania Capital, GM Ventures, Intel Capital, Continental AG, Hella Ventures, LG Electronics, Aisin, Airbus Ventures, SK hynix, Subaru-SBI, and Tyche Partners.


Learn more at[www.aeye.ai](https://www.aeye.ai/)
