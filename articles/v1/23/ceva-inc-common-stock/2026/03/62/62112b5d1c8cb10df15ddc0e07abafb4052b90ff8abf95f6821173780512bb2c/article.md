---
schema_version: "1.0.0"
document_id: "62112b5d1c8cb10df15ddc0e07abafb4052b90ff8abf95f6821173780512bb2c"
company_key: "ceva-inc-common-stock"
company: "CEVA Inc."
source_id: "ceva-inc-common-stock-rss-438e9c80ad28"
canonical_url: "https://www.ceva-ip.com/blog/the-6g-clock-ticking-why-silicon-architecture-for-2030-must-start-in-2026/"
published_at: "2026-03-26T08:41:47+00:00"
first_seen_at: "2026-08-12T09:56:21.821341+00:00"
fetched_at: "2026-08-12T09:56:23.721999+00:00"
content_hash: "sha256:c8ca9719d2a57652f1fd7733ee7010f0669e4e9d025e7e655a8cb692fa5ce723"
---

# The 6G clock ticking: Why silicon architecture for 2030 must start in 2026

19 min read


The 6G transition is no longer a distant theoretical exercise; it is a commercial inevitability driven by fundamental requirements for cellular standards to keep moving forward. 5G penetration has already surpassed 75% and is on a trajectory to reach 95% within a few years. We are


witnessing


an appreciation for continued call quality and data throughput


improvements


despite an explosion in mobile traffic. However, the wireless ecosystem projects that even this capacity will soon overload due to accelerating AI content, the integration of satellite communications (SATCOM) into the cellular fold, and the rise of


Physical


AI)


. 6G is the industry’s response to keep pace with that


exponential growth in data


communication demand


.


The 2030 Countdown: Why 2026 is the Crucial Starting Line


To understand the


urgency


, one must look at the decadal cycle of cellular evolution. History shows it takes a


bout


five


years to


finalize


a standard and fold its requirements into a functional ecosystem. While 6G is


anticipated


to take off commercially by 2030, the work-back schedule reveals a tight timeline for product builders. By 2029, hardware must be ready for compliance testing, meaning


component


technologies must be


finalized


by 2028. Consequently, underlying embedded systems must be built in 2027,


necessitating


that architectural definitions start as early as 2026.


As an example


of


what is


going


on


in the industry,


Qualcomm


’s


CEO recently hinted at the Snapdragon summit that 6G-capable devices could appear as early as 2028 for trials, making the 2028 Olympics a perfect arena for tech demos.


Unlocking the “Golden Band”: FR3 and the Business of Spectrum


Beyond architectural shifts, 6G introduces the


Frequency Range 3 (FR3)


spectrum, spanning


7.125 GHz to 24.25 GHz


. Often called the “Golden Band


for 6G


,” FR3 offers the perfect balance between the wide coverage of lower bands and the massive capacity of


mmWave


. This spectrum is expected to be a


major


business driver, enabling


the


10x higher data rates


targets


(up to 200 Gbps) and supporting “Massive MIMO Evolution” to handle the projected


4x traffic growth


by 2030


(going over


5.4 zettabytes


as


indicated


by the GSMA


Intelligence


report)


.


Sustainable


Networks


Sustainability is a core pillar of 6G, with network operators seeking to reduce


OpEx


, as


25%


of


it


is driven by power demand. 6G moves from an “always-on” to a “smart-on” philosophy, aiming for


30-50% increased power efficiency


. Key techniques include:


- Enhanced


Deep Sleep Modes:


Enabling base stations to achieve near-zero power consumption when no active users are present


, and reduction in periodic signaling


(current 5G standard mandate


high


periodic signaling that


in practice


keeps a lot of the RF and power amplifier component


s


active at all


time


)


.


- AI-Driven Beamforming: Using AI to direct signals precisely to users, reducing energy waste from broad, inefficient broadcasting.


- AI-Driven


Resource Management


:


Using AI


at the higher protocol layers for effective radio resources management


.


The AI-Native Revolution: Moving Intelligence to the Air Interface


One of t


he most significant


shift


s


in 6G is the mov


e toward an


AI-native air interface


.


Unlike 5G’s rigid mathematical models, 6G uses deep learning to dynamically adapt signal processing blocks.


This enables “adaptive waveforms” that adjust modulation in real-time to environmental conditions. It also


facilitates


Integrated Sensing & Communication (ISAC)


,


where RF reflections provide precise spatial awareness, allowing the network to proactively adjust beamforming based on user movement.


The Coordination Challenge: Managing Two-Sided AI


This transition introduces a complex challenge in how the transmitter (base station) and receiver (device) coordinate their intelligence. Unlike traditional algorithms, AI components must be synchronized through


AI Lifecycle Management (LCM)


. The industry is weighing


one-sided models


(device-only optimization) against


two-sided architectures


(essential for tasks like CSI compression). In two-sided designs, the device acts as a neural encoder and the base station as a decoder; these must be


coordinated


pairs


to some extent


.


The level of coordination is still in study, as there are few optional schemes. Examples for those schemes


are fully


matched


neural networks couples, or


alternatively


,


independent


at the NN architecture level but trained on the same data set.


This raises critical questions


on the protocol level


: should the network use


Model ID-based selection


(activating pre-loaded models) or


Model Transfer


(pushing new neural weights over the air)


or


Weights


Transfer


?


Programmable Intelligence: Why DSPs are the Preferred Path


Because 3GPP specifications


remain


fluid, the need for flexibility through programmability has never been higher. Developing 6G on hard-wired logic is risky, as spec changes could


render


silicon obsolete. This is why


Digital Signal Processors (DSPs)


are the preferred architecture. Modern DSPs are uniquely suited for the AI-native


physical


layer; they


possess


the massive number of


MACs


required


for matrix operations and are highly


efficient at the vector processing necessary for neural netw


orks.


Leading technology vendors also offer dedicated AI ISA for accelerated NN activation functions.


A fully programmable modem


powered by


AI


native DSP


processor offers a “safe bet,” allowing developers to adapt as 6G settles while


maintaining


the performance needed to lead the market.


Originally published on[EDN](https://www.edn.com/the-6g-clock-ticking-why-silicon-architecture-for-2030-must-start-in-2026/)
