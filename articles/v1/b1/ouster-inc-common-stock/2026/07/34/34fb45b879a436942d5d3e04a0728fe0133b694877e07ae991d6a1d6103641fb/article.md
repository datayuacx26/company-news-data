---
schema_version: "1.0.0"
document_id: "34fb45b879a436942d5d3e04a0728fe0133b694877e07ae991d6a1d6103641fb"
company_key: "ouster-inc-common-stock"
company: "Ouster Inc."
source_id: "ouster-inc-common-stock-news-import-d3df9a0809e0"
canonical_url: "https://ouster.com/insights/blog/tum-bridging-sim-to-real-with-lidar"
published_at: null
first_seen_at: "2026-07-24T08:00:35.952779+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:1dfca0a27c0355729e5fa67e638a99d3dab0548bc03736ced6a05f0445dcc10d"
---

# TUM: Bridging the Sim-to-Real Gap with Digital Twins and Digital Lidar

Most autonomous driving begins on a screen. In a perfect, simulated world, algorithms are refined, paths are planned, and edge cases are tested without a single drop of fuel or a safety driver behind the wheel.


Perception engineering comes with the challenge of transitioning from simulation to the road. All too often, code that performs flawlessly in a virtual environment begins to struggle when it meets the messiness of the real world. This is called the "Sim-to-Real" gap - a hurdle that has historically slowed down development and introduced unnecessary risk.


To solve this, the team at the Technical University of Munich (TUM) introduced[EDGAR](https://www.mos.ed.tum.de/en/ftm/main-research/intelligent-vehicle-systems/edgar/) (Excellent Driving Garching). EDGAR is a holistic research platform that connects a physical car to a 1:1 Digital Twin, enabling faster iteration cycles for autonomous vehicle research.


By standardizing their perception layer with Ouster digital lidar and collaborating with our premium DACH partner,[General Laser](https://general-laser.at/) , the TUM team has built a bridge that allows researchers to move from simulation to the asphalt with more confidence.


### The Challenge: When Simulation and Reality Don't Match


The Sim-to-Real gap usually comes down to inconsistency. If your simulated sensor perceives the world differently than your physical sensor, your software will struggle to adapt.


Legacy analog lidar sensors are notoriously difficult to model. They often have inconsistent noise profiles and vary from unit to unit. When you combine that with the leap from high-powered development servers to the ARM-based embedded computers actually inside the vehicle, the validation process becomes a bottleneck.


The EDGAR project was designed to eliminate these variables by ensuring that every part of the stack, from the network latency to the 3D data, is identical in both the virtual and physical worlds.
