---
schema_version: "1.0.0"
document_id: "2e224d646fb1335c85a8fed9bf199246af4b777246598f0ec19704b8f6c3e9fc"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2026/05/13/who-tests-the-ai-viavis-case-for-the-realistic-digital-twin/"
published_at: "2026-05-13T17:17:12+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:50:55.365924+00:00"
content_hash: "sha256:347197229e5e710b1d539b7830ff0532c7f738d746e7836ef6f833a524aee228"
---

# Who Tests the AI? VIAVI’s Case for The Realistic Digital Twin

**


# Who Tests the AI? VIAVI’s Case for The Realistic Digital Twin


- **Sameh Yamany
- ** May 13, 2026
- 1


By 2030, almost 40% of wireless traffic will be AI traffic. The question every operator will face is: who tests the AI, and with what data?


VIAVI turned 100 in 2023, marking a century of leadership in test and measurement, from the optics that move data through fiber to the wireless instruments that have measured every generation from 1G to AI-RAN. The reason that history matters now is that 6G will not be designed in a lab. It will be designed against AI traffic that does not yet exist, for applications that are not yet built, by operators who cannot afford to find out in production whether the design works.


AI is going to make decisions in the network. Someone must test those decisions. And to test AI, you need AI; trained on real data, deployed inside a digital twin that behaves the way the live network behaves.


### The 40%


By 2030, AI traffic will account for close to 40% of all wireless and network traffic. Understanding what that traffic looks like, how it behaves, and how it interacts with everything else on the network is now a design constraint for 6G.


Most of the AI conversations in telecoms today revolve around the AI inside the network: the agents recommending optimizations, the models predicting failures, the agentic platforms running closed loops. AI through the network is also important. Inference traffic between data centers, model updates pushed to edge devices, sensing data from drones and vehicles and ships — all of it has to be tested, qualified, and designed against.


### Who Tests the AI


Who tests AI? How? Against what data?


A model trained on the wrong distribution will drift. A model trained on synthetic data can fail the moment it meets a real subscriber, a real radio environment, or a real city changing shape under construction cranes. The data is the test. The data is the design.


VIAVI’s answer is data for AI, not just emulation. The right kind of data, fed to the right kind of twin, used to train and test the models that will run the network.


### What the Digital Twin is


Most digital twins use synthetic data. They render a city block or ray-trace a beam. They model a single user walking through the model with a handset.


The same twin, faced with a real network, can fail. A 5G research environment behaves differently from the live coverage of a real metropolitan area, and most operators have encountered this gap.


VIAVI has 25 years of data collection from real networks with field instruments running in 2G, 3G, 4G, 5G and 5G-Advanced environments. KPI verification, root-cause analysis, troubleshooting data and more, gathered originally to fix things, are now repurposed as the training set for an AI engine that emulates how the network responds when something changes.


### Three Perspectives


A combination of perspectives is what enables VIAVI’s approach to generative reality:


- High volume, high granularity, time-series data from the live network.
- 3D spatial information including building geometry, beam shapes, and handover patterns.
- Field-collected sensor and instrument data correlated with operational telemetry from AIOps and Location Intelligence.


### AI-RAN Scenario Generation


Take for example a fire breaking in a city. The emergency services activated and the drones flying. The coverage is shifting in real time. The operator needs to know how the network will behave — not in the lab, but in the actual environment, with the actual gNB configuration, with the actual transport layer, with the actual subscriber load.


The twin generates the scenario and the AI predicts the network response. The operator sees the answer before any change is made to the live network, and with new scenarios, the twin becomes better.


On top of that, applications get built and RAN scenarios get tested. Different interfaces get emulated. The twin becomes a working environment for everything that needs to be tested before going live.


### VIAVI Generative Reality Digital Twin™ (GRDT)


GRDT is a high-fidelity digital replica of a live network, calibrated and continuously synchronized with real network data, where AI tests AI in real time.


It mirrors the traffic, conditions, and scale of the network in operation, and evolves as the network evolves. Every operational change, AI model, configuration, and upgrade is tested, validated, and optimized inside the twin first, in real time, before it touches the live network.


The result is faster next-generation network deployments, with the live network protected at every step.


### Use Cases for GRDT


**Construction Sites**


Coverage changes daily as cranes go up. The twin generates heat-map predictions, runs *what-if* scenarios across multiple service providers, and shows what happens when a 5G antenna is moved to a vehicle or a drone to maintain coverage.


**Port Authority**


Physical and network interactions are emulated together. Congestion is modelled against the number of ships and the height of containers. Dynamic beamforming are tested in the twin. A 5G antenna is placed on a barge, moved around the port, with the twin showing fidelity within three or four milliseconds of the antenna’s position changing. Container terminals are critical infrastructure. They are also one of the most complex coverage environments in commercial wireless.


**Developer Sandbox**


A US operator wanted to give application developers a real network to build against, without giving them access to the live network. VIAVI built the sandbox. Developers test against an environment that behaves like the operator’s network because it is trained on the operator’s network.


**Agriculture and IoT**


IoT interactions emulated at scale, with the behavior patterns specific to low-coverage rural environments.


**Railways**


VIAVI tests railway signals and the communications layer that runs on top. European operators are migrating from 2G to 5G and 5G-Advanced for railway communications. The twin runs the what-if scenarios that no live track can host.


**Non-Terrestrial Networks (NTN)**


In collaboration with Rohde & Schwarz. The satellite emulation is one half, while the other half is real terrestrial data. The twin models the interaction between the two, including Doppler effects, delay handover, and NB-IoT scenarios.


**AI-RAN, AIOps, and the Data Layer**


VIAVI runs a state-of-the-art O-RAN lab certified by the O-RAN Alliance, in Phoenix, Arizona. The lab’s job is no longer only to emulate radio access networks. The lab’s job now is to create open, sharable datasets designed for any party that needs to test or train AI-RAN models.


That is the upstream of the AIOps story. AIOps closes the day-two loop: predict, detect, resolve. The twin closes the day-zero and day-one loop: simulate, validate, optimize before deployment. Both run on the same data foundation and depend on real network telemetry, not synthetic generation.


**Quantum-Safe from Day One**


6G is being specified with a non-negotiable requirement: quantum-safe from day one. VIAVI is building digital twins for quantum-safe network design.


If 6G has to be quantum-safe at launch, the testing has to start now, in the twin, before the standard is finalized. There is no time to test quantum readiness in the live network after the fact.


**Integrated Sensing and Communication**


VIAVI has been doing RF sensing and fiber sensing for years. ISAC merges sensing into the communication layer itself. The twin must absorb that (model environments through sensing data) then test communication scenarios on top of the same model.


### What This Means for 6G Design


Real data is the most valuable element and the twins trained on years of real network telemetry will be the ones that matter for 6G.


The twin is where AI learns.


*Based on a keynote by Sameh Yamany, Chief Technology Officer, VIAVI Solutions | Global 6G Conference*


- ** Categories:[6G](https://blog.viavisolutions.com/category/6g/)
- ** Tags:[6G](https://blog.viavisolutions.com/tag/6g/) ,[AI](https://blog.viavisolutions.com/tag/ai/) ,[Digital Twin](https://blog.viavisolutions.com/tag/digital-twin/) ,[quantum-safe](https://blog.viavisolutions.com/tag/quantum-safe/)


-
-
-
-
-
-
