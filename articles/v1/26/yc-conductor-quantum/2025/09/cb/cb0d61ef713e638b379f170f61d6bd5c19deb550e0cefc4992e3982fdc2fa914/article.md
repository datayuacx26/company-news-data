---
schema_version: "1.0.0"
document_id: "cb0d61ef713e638b379f170f61d6bd5c19deb550e0cefc4992e3982fdc2fa914"
company_key: "yc-conductor-quantum"
company: "Conductor Quantum"
source_id: "yc-conductor-quantum-rss-fd18baa21fa7"
canonical_url: "https://blog.conductorquantum.com/p/announcing-the-conductor-models-playground"
published_at: "2025-09-28T19:01:15+00:00"
first_seen_at: "2026-07-24T23:15:28.367668+00:00"
fetched_at: "2026-07-28T20:55:46.222495+00:00"
content_hash: "sha256:e8393cc59ca0f24d371265f13bb13c8233f900bda0674a136f772d43b7f875e5"
---

# Announcing Conductor Models

# Announcing Conductor Models


### Product


Sep 28, 2025


*Originally published on December 10th, 2024.*


At Conductor Quantum, we’re building foundational software for quantum computers. Today, we’re excited to unveil


**[Models](https://docs.conductorquantum.com/models/overview)** —an SDK to use our machine learning models for quantum device calibration and characterization.


## Why Calibration Matters


Before you can harness the power of a quantum computer, you first need to turn it on—and that’s no small feat. For quantum computers based on spin qubits in semiconductors, this process begins with


**trapping and manipulating single electrons** . Imagine isolating a single electron from a sea of billions flowing through a semiconductor device every second. This is where quantum engineers start their journey.


But, before electrons can be trapped, engineers must ensure that the semiconductor device itself is functioning correctly. This involves a series of vital health checks:


1.


**Turn-On Check** - Engineers determine the voltage at which current begins to flow through the device.


2.


**Pinch-Off Check** - By sweeping the voltage on each gate electrode, they observe when the current transitions from high to low, ensuring the gates function as intended.


3.


**Coulomb Blockade Check** - Detecting Coulomb oscillations signals the formation of a quantum dot—a critical milestone for enabling quantum transport and, ultimately, qubits.


These steps are essential because they validate whether a device is operational. They also provide key insights into device performance—vital as semiconductor quantum devices move into the


**foundry era** , with fabs such as Global Foundries, Imec, and Intel producing chips at scale.


Our models enable users to


**extract reliable performance metrics** both within a single wafer and across multiple wafers, empowering them to make data-driven decisions about their devices.


## What’s in Our Models SDK?


We’re launching with three flagship models that focus on semiconductor quantum device health checks:


### Turn-On Parameter Extraction Model


-


Automatically classifies current traces as successful or unsuccessful turn-ons.


-


Accurately extracts the turn-on voltage with 99% reliability.


-


Helps you identify the voltage threshold at which current begins to flow through your device.


### Pinch-Off Parameter Extraction Model


-


Extracts critical parameters such as cutoff, transition, and saturation voltages from a pinch-off sweep.


-


Enables coarse quantum dot tuning and provides insights into gate-electrode performance and device hysteresis.


### Coulomb Peak Finder


-


Identifies the presence of Coulomb peaks in current traces and returns their corresponding voltage values.


-


This is a foundational tool for tuning algorithms, enabling adaptability across various device architectures and scalability to qubit formation.


## What’s Next?


All models shown are for single parameter datasets. The natural next step are models for two-dimensional measurements, such as charge stability diagrams. For decades, engineers have manually analyzed these datasets. But as devices grow more complex, the need for automated, scalable tools becomes unavoidable. Stay tuned as we expand Models to tackle this challenge and more.


## About Conductor Quantum


Conductor Quantum is an American company headquartered in San Francisco, California. We are assembling a leveraged pack of hungry animals and cracked engineers whose sole focus is to develop software to enable fault-tolerant quantum computation.


If you would like to expand the quantum frontier and solve one of the hardest technological challenges of our time, this is your chance.


Join us.


Conductor Quantum, Inc.


founders@conductorquantum.com
