---
schema_version: "1.0.0"
document_id: "6894fd2ad3e56d66ccd5ff266f335bd27253d515d6928c985a0037df2448a914"
company_key: "stmicroelectronics-n-v-common-stock"
company: "STMicroelectronics N.V."
source_id: "stmicroelectronics-n-v-common-stock-rss-811b8a53447b"
canonical_url: "https://blog.st.com/iis3dwb10is-vibration-sensor/"
published_at: "2026-06-03T08:27:39+00:00"
first_seen_at: "2026-07-24T02:26:40.894294+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:08a4e6fc82ef3201c8eeefa1b6215ce7494e3c6744c3bb3af1618f5d3a8381b3"
---

# IIS3DWB10IS, the 1st digital vibration sensor with 10 kHz bandwidth to enable new era for predictive maintenance

**The[IIS3DWB10IS](https://www.st.com/en/mems-and-sensors/iis3dwb10is.html?ecmp=tt49698_gl_link_jun2026) is the first digital vibration sensor with a frequency range exceeding 10 kHz, paving the way for more powerful anomaly detection and predictive maintenance, as well as new robotics and shock-sensing applications.** Traditionally, engineers relied on far costlier piezoelectric sensors to support this kind of bandwidth. Additionally, the IIS3DWB10IS embeds an enhanced ISPU (Intelligent Sensor Processing Unit), compared to its predecessor (the[IIS3DWB](https://www.st.com/en/mems-and-sensors/iis3dwb.html?ecmp=tt49698_gl_link_jun2026) ). It can handle higher data streams from the sensing element (up to an 80 kHz sampling rate) and perform on-device computations. We currently have nearly a dozen design wins and about four times as many design ins, proof of the overwhelmingly positive feedback from early customers.


## Why high-bandwidth digital vibration sensors used to be impossible?


The IIS3DWB10IS


Vibration analysis remains extremely popular, in part because it powers machine learning at the edge. While there are many ways to capture the data that neural networks will use, some of the earliest applications relied on vibration monitoring to detect anomalies. Crucially, the success of vibration analysis stems from the fact that sensors are cost-effective, straightforward, and usable at nearly any scale. However, detecting vibrations at frequencies above 10 kHz remains so difficult that engineers must either use a secondary piezoelectric sensor and a more powerful MCU or forgo detection altogether. **That’s because sensing such high frequencies requires more complex sensing elements and ASICs capable of dealing with massive sampling rates.**


## What did the IIS3DWB10IS do differently?


### A new architecture


The IIS3DWB10IS addresses the challenges inherent to vibration sensors by offering an entirely new architecture and ASIC. **The new architecture is possible thanks to a completely new lithographic process that adds two epitaxial layers on top of the buried polysilicon layer, rather than just one.** Coming up with such an architecture has its own structural challenges, which is why competing devices adopt a simpler approach. However, we were able to overcome these challenges thanks to our decades of experience in MEMS manufacturing, which enabled us to offer something no one else in the industry is currently selling.


A simple representation of vibration analysis in an industrial application


### ISPU 2.0


The other critical innovation that makes the IIS3DWB10IS possible is the presence of an ISPU capable of processing the massive amount of information captured when sensing high-frequency vibrations. In practical terms, it means having a 32-bit RISC architecture with its own RAM to reduce bottlenecks. **It also features a hardware floating-point unit to optimize FFT and RMS real-time computations, which are essential in condition monitoring applications, and it remains C-programmable for low-level access.** As a result, the system can do more before involving the host MCU, thereby improving energy efficiency.


## How does the IIS3DWB10IS speak to engineers?


### A low noise level


Behind the data sheets and technical explanations lie two realities that explain the positive feedback from early adopters of the IIS3DWB10IS. First of all, despite being the first digital vibration sensor of its kind, it can go head-to-head with far more expensive piezoelectric alternatives. **That’s in part because the IIS3DWB10IS has a noise density of 35 µg/√Hz.** While it’s not as low as a traditional piezoelectric sensor, it’s far below the 50 µg/√Hz most engineers target. On the other hand, the IIS3DWB10IS greatly simplifies the design process and helps significantly lower the bill of materials. Put simply, the tradeoff has convinced many teams to move to our device.


### A very large scale


**Second of all, it’s easy to overlook that the IIS3DWB10IS offers a full-scale range that’s user-selectable from ±50 to ±200, with no increase in noise, allowing teams to use one device for a wide range of applications.** We know that most industrial applications require only ±50 g and a maximum operating temperature of 125 ºC. However, many competing solutions limit their scale to preserve their signal-to-noise ratio. The IIS3DWB10 not only offers a uniquely high bandwidth and supports such temperatures, but also provides a range that exceeds most expectations, meaning that most teams only need to qualify one component for all their use cases.


- [Learn more about the IIS3DWB10IS](https://www.st.com/en/mems-and-sensors/iis3dwb10is.html?ecmp=tt49698_gl_link_jun2026)
