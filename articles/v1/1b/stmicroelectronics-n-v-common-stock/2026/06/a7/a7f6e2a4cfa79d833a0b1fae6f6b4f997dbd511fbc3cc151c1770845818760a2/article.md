---
schema_version: "1.0.0"
document_id: "a7f6e2a4cfa79d833a0b1fae6f6b4f997dbd511fbc3cc151c1770845818760a2"
company_key: "stmicroelectronics-n-v-common-stock"
company: "STMicroelectronics N.V."
source_id: "stmicroelectronics-n-v-common-stock-rss-811b8a53447b"
canonical_url: "https://blog.st.com/physical-ai/"
published_at: "2026-06-22T15:00:44+00:00"
first_seen_at: "2026-07-24T02:26:40.894294+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:3a26ff954fb65b35cdbde95f2f401e9bcf49d30606bb49c79e11f203bbee2d55"
---

# Update: Physical AI now with functional safety, ST sensors, a robotics platform, and innovations through collaboration

## Update, June 17, 2026


**ST is now taking part in[NVIDIA Halos for robotics](https://blogs.nvidia.com/blog/halos-safety-system-autonomous-vehicles/) , the first end-to-end functional safety system for robotics and physical AI.** It is built upon three components: NVIDIA IGX Thor and NVIDIA Holoscan Sensor Bridge, NVIDIA Halos OS, and NVIDIA Halos AI Systems Inspection Lab. As ST provides sensors and microcontrollers for the NVIDIA Holoscan Sensor Bridge, we wanted to ensure that developers had access to the same sensing, processing, and actuation building blocks that streamline development. It was also a natural collaboration as ST has been[championing functional safety for years](https://blog.st.com/x-cube-stl/) .


- [Learn more about NVIDIA Halos](https://blogs.nvidia.com/blog/halos-safety-system-autonomous-vehicles/)
- [Read about how ST helps accelerate physical AI development with NVIDIA](https://www.st.com/content/st_com/en/campaigns/accelerating-physical-ai-development-with-nvidia.html)


---


## Original publication, March 17, 2026


**As technology aims to enable Physical AI, ST is sharing today how collaboration brought our sensors into a Holoscan Sensor Bridge module from Leopard Imaging, enabling developers to feed multi-modal sensing data to the NVIDIA Jetson Thor or NVIDIA Jetson Orin platform in real-time and with low latency, to create the robots of tomorrow.** The goal is simple: bridging the sim-to-real gap so companies can release machines that will transform our lives and societies in a positive and meaningful way. However, to do that, companies must come together to solve the engineering challenges currently plaguing this field. This is the fruit of this initiative.


## Why is Physical AI hard?


### Reality is stranger than fiction


If science fiction were any guide, the world would already be swarming with humanoid robots. **We have sensors more powerful and precise than many could have dreamt of a few years ago.** We have even had sensors capable of[running machine-learning inferences for the last six years](https://blog.st.com/lsm6dsox-fsm-mlp-sensor-machine-learning/?ecmp=tt48648_gl_link_mar2026) . Cameras are so accurate that smartphones can scan a room and provide a 3D model of an environment. Microcontrollers now run applications that would have required supercomputers a decade ago, enabling the creation of complex machines on low-power embedded systems. In fact, advances have been so staggering that companies are looking to put AI in pins and other wearables, making our reality even more fantastic than science fiction.


**Similarly, AI has grown by leaps and bounds to the point that large language models can now help developers vibe code entire applications in a matter of minutes, and the most brilliant medical researchers are using AI to run simulations and process volumes of data points that would be overwhelming for a single person.** This has been possible thanks to advances in NVIDIA’s innovation in GPU technology, accelerated computing, interfaces, interconnects, and other optimizations.


### Physical AI is harder than regular AI


Yet humanoid robots taking over household chores are not here yet, and while autonomous vehicles have made tremendous strides, they are far from being ubiquitous. **To better tackle these engineering challenges, the industry developed the theory of “Physical AI”.** In a paper published in 2025 in the *Journal of Intelligent System of Systems Lifecycle Management*1 , Professor Vahid Salehi defines it as “artificial intelligence systems that not only process information and make decisions in the digital space but also interact directly with the physical world and use sensors to perceive their environment, model dynamic states of the real world, make autonomous decisions on how to act, and implement these decisions using actuators (e.g. robotic arms, driving movements, gripping devices).”


Put simply, the engineering challenge lies in bridging the capabilities of sensors and actuators with the possibilities afforded by AI to create real-time motor control applications that can adapt to the noisy realities humans have evolved to navigate. **It’s about closing the sim-to-real gap so that autonomous digital processes can systematically make correct decisions and perform precise, reliable actions even in the face of life’s unpredictability.** Physical AI addresses the fact that the next evolution of robots must leverage AI at the edge to ensure real-time performance while guaranteeing the safety and security of everyone around them. This is why today’s announcement is special: it directly tackles the challenges behind Physical AI.


## How can the industry solve Physical AI?


### ST sensors


The Leopard Imaging Module with ST sensors is designed for Physical AI


To start tackling Physical AI, ST chose some of its most advanced sensors. For instance, we help design a head camera unit that uses two **[VB1940](https://www.st.com/en/imaging-and-photonics-solutions/vb1940.html?ecmp=tt48648_gl_link_mar2026)** automotive-grade 5.1-megapixel image sensors. They are unique in that[they can operate as either global shutter or rolling shutter](https://blog.st.com/cmos-image-sensors/?ecmp=tt48648_gl_link_mar2026) , offering artefact-free image capture and better image quality. The camera unit also features our **[LSM6DSV16X](https://www.st.com/en/mems-and-sensors/lsm6dsv16x.html?ecmp=tt48648_gl_link_mar2026)** six-axis inertial measurement unit[with machine learning capabilities](https://blog.st.com/lsm6dsv16x/?ecmp=tt48648_gl_link_mar2026) and our new **[VL53L9CX](https://www.st.com/en/imaging-and-photonics-solutions/vl53l9cx.html?ecmp=tt48648_gl_link_mar2026)** direct Time-of-Flight LiDAR module for spatial recognition. We also helped design a motor control application that relies on two **[STSPIN32G4](https://www.st.com/en/motor-drivers/stspin32g4.html?ecmp=tt48648_gl_link_mar2026)** , our[first motor controller with an integrated MCU](https://blog.st.com/stspin32g4-2/?ecmp=tt48648_gl_link_mar2026) , and the **[ASM330LHH](https://www.st.com/en/mems-and-sensors/asm330lhh.html?ecmp=tt48648_gl_link_mar2026)** accelerometer and gyroscope to detect motion and provide stabilization.


### Leopard Imaging module


Providing the best sensors is one piece of the Physical AI puzzle, but bundling them altogether is another, which is why we partnered with Leopard Imaging. **The company created an all-in-one module for advanced robotic systems, thereby tackling integration challenges so others didn’t have to.** As explained above, one of the critical aspects of Physical AI theory is perception. Thanks to the Leopard Imaging module, designers don’t have to fine-tune the DSP, worry about EMIs, or create a custom PCB. Leopard Imaging has a habit of[making perception modules ubiquitous](https://blog.st.com/leopard-imaging/?ecmp=tt48648_gl_link_mar2026) , and with this announcement, the[ST Authorized Partner](https://www.st.com/content/st_com/en/partner/partner-program/partnerpage/Leopard_Imaging.html?ecmp=tt48648_gl_link_mar2026) helps make Physical AI and humanoid robotics much more accessible.


## Physical AI: From module to humanoid robot, NVIDIA


### NVIDIA Holoscan Sensor Bridge


Another link in the Physical AI chain is the communication between modules, such as the head sensor unit from Leopard Imaging, and the computing platform. This is why ST and Leopard Imaging collaborated with NVIDIA to ensure the module worked with its Holoscan Sensor Bridge (HSB). **HSB is a sensor-over-Ethernet streaming platform that enables modules to send data in real time to systems responsible for decision-making or actuator control.** HSB leverages hardware-accelerated networking on supported hosts like NVIDIA Thor, IGX, and ConnectX-based platforms like DGX Spark, providing a low-latency, low-jitter, CPU-offloaded, highly-scalable solution to sensor and actuator connectivity challenges.


**Moreover, ST collaborated with NVIDIA to implement a proof of concept of the Holoscan Sensor Bridge embedded software IP running directly on the STM32H7 series of advanced Ethernet-enabled microcontrollers.** It enables a hardware-accelerated link between NVIDIA platforms and MCU-based sensor and motor control systems, thus supporting advanced artificial intelligence (AI) workloads while reducing jitter, latency, and CPU load.


### NVIDIA Jetson and Isaac Sim


Holoscan Sensor Bridge integration into a module with ST sensors means developers gain more efficient access to the NVIDIA Jetson ecosystem, thus moving closer to Physical AI. **Indeed, after bundling the right sensors into a module that supports a high-speed communication protocol, engineers can use them on a platform designed to bridge the sim-to-real gap.** This isn’t our first collaboration with NVIDIA. Avid readers of the ST Blog will remember a previous initiative that enabled developers to use[STM32Cube.AI and the TAO Toolkit to create a system that woke up a downstream Jetson Orin when detecting people](https://blog.st.com/tao-toolkit/?ecmp=tt48648_gl_link_mar2026) .


In an article published in 2025, NVIDIA outlined its “[three-computer solution for robotics](https://blogs.nvidia.com/blog/three-computers-robotics/?ecmp=tt48648_gl_link_mar2026) ”. The first computer is an NVIDIA DGX AI supercomputer for AI training; the second computer is an NVIDIA RTX PRO server to run simulations using NVIDIA Omniverse and Cosmos; and the third computer is an NVIDIA Jetson system for AI at the edge. Aligned with NVIDIA’s simulation-first philosophy, we are committed to accelerating physical AI development. **ST is the first IMU vendor to offer a device model in Isaac Sim™, NVIDIA’s Omniverse-based reference application for simulating and testing AI-driven robots.**


This enables developers to validate our sensor’s performance in a virtual environment prior to physical prototyping. Once developers specify how they expect the robot to behave, they can deploy their policies to an NVIDIA Jetson Thor™ or NVIDIA Jetson Orin™ and rapidly and efficiently benefit from Holoscan Sensor Bridge by streaming information to the computing unit. For example, the HSB module can link the Leopard Imaging module to the NVIDIA Jetson platform. **It’s even possible to use the HSB’s Ethernet connection to connect it to the NVIDIA server running Isaac Sim, allowing a hardware module in the simulation, should designers need to tweak aspects of the robotics application.**


Today’s announcement is thus unique because it addresses every step in the Physical AI chain, from sensor capture to actions performed in real-world environments. **More crucially, it shows how collaboration can make robotics applications accessible by lowering the barrier to entry.** When the industry brings sensors to modules and modules to development platforms, then developers can begin to conquer Physical AI rather than deal with component sourcing, communication bottlenecks, and other challenges that would slow the pace of innovation in humanoid robots and related fields.


- [Find out more about the ASM330LHH](https://github.com/STMicroelectronics/asm330lhh-pid?ecmp=tt48648_gl_link_mar2026)
- [Try the STSPIN32G4](https://www.st.com/en/motor-drivers/stspin32g4.html?ecmp=tt48648_gl_link_mar2026)
- [Envision using the VL53L9CX](https://www.st.com/en/imaging-and-photonics-solutions/vl53l9cx.html?ecmp=tt48648_gl_link_mar2026)
- [Explore the LSM6DSV16X](https://www.st.com/en/mems-and-sensors/lsm6dsv16x.html?ecmp=tt48648_gl_link_mar2026)
- [Check out the VB1940 image sensor](https://www.st.com/en/imaging-and-photonics-solutions/vb1940.html?ecmp=tt48648_gl_link_mar2026)
- [See the Eagle Series Holoscan Cameras from Leopard Imaging](https://leopardimaging.com/product-category/depth-sensing/stereoscopic-cameras/li-vb1940-stxxx-10gige/?ecmp=tt48648_gl_link_mar2026)
- [Become familiar with NVIDIA Holoscan Sensor Bridge](https://www.nvidia.com/en-us/technologies/holoscan-sensor-bridge/?deeplink=partners-tab--1?ecmp=tt48648_gl_link_mar2026)
- [Learn more about NVIDIA Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/?ecmp=tt48648_gl_link_mar2026)
- [Discover NVIDIA Jetson Thor](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-thor/?ecmp=tt48648_gl_link_mar2026)
- [Read the Isaac Sim documentation](https://docs.isaacsim.omniverse.nvidia.com/5.1.0/index.html?ecmp=tt48648_gl_link_mar2026)
- [NVIDIA Isaac Sim Extension for STMicroelectronics Sensors](https://github.com/STMicroelectronics/st-mems-isaac-sim2real?ecmp=tt48648_gl_link_mar2026)


---


1.


Salehi, V. (2025). Fundamentals of Physical AI. Journal of Intelligent System of Systems Lifecycle Management, 2.[https://doi.org/10.71015/z6mc6967](https://doi.org/10.71015/z6mc6967)↩︎
