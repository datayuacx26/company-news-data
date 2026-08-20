---
schema_version: "1.0.0"
document_id: "a42740ea8e0adacaf6c72a3595105f5ee2fce6a2f3a302941975ccfcdad5bf9f"
company_key: "yc-conductor-quantum"
company: "Conductor Quantum"
source_id: "yc-conductor-quantum-rss-fd18baa21fa7"
canonical_url: "https://blog.conductorquantum.com/p/health-check-is-my-quantum-chip-alive"
published_at: "2025-10-17T02:32:18+00:00"
first_seen_at: "2026-07-24T23:15:28.367668+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:ddde239b09261ff160c96423b8173e05d81b0b5fd86a73fd0396986677aa764a"
---

# Health Check: Is my quantum chip alive or dead?

# Health Check: Is my quantum chip alive or dead?


### Product


Oct 17, 2025


Powering up a new quantum chip is like getting a car roadworthy — you check for leaks, make sure the engine starts, and verify that every control responds before hitting the road. In quantum hardware, those checks are tedious — each gate voltage must be swept, every contact measured, and baseline noise characterised by hand. And as we scale to quantum chips with millions of qubits, these manual checks quickly become impossible to manage.


### Introducing Health Check


Health Check automates this whole procedure. It’s a family of quick, automated tests that confirm your chip is ready to go. Instead of writing new scripts for every sample, you run the Health Check routine and get a clear report on whether your device is healthy — and where its critical voltage thresholds lie. Designed for scale, Health Check is engineered from the ground up to run just as smoothly on a single test device as it does across multiplexed or wafer-prober systems.


Thanks for reading! Subscribe for free to receive new posts and support my work.


**Read the full documentation [here](https://docs.conductorquantum.com/stanza/latest/core-concepts/routines/health-check) .**


### Why Health Check?


Running quantum dot experiments without a health check is risky. A leaky gate or a mis-set voltage can waste hours of data collection or even damage your device. Health Check reduces that risk by running a consistent set of baseline tests every time. It helps you:


-


**Spot leakage early** by measuring any unwanted current flowing through each gate — a key indicator of material defects or device degradation.


-


**Find where conduction begins** by sweeping all gates together and identifying the global turn-on point.


-


**Calibrate individual gates** (reservoirs, plungers, barriers) so you know where each gate electrode starts to pinch-off the flow of current through the channel.


When the routine finishes, you get a short list of voltages that feed directly into more advanced experiments — saving you time and protecting your hardware.


### How It Works


Health Check runs four main tests in sequence:


1.


**Noise floor check** — measures background current so later tests can tell real signals from noise.


2.


**Leakage test** — sweeps each gate and looks for any unwanted gate current. Excessive leakage can signal fabrication issues or device wear.


3.


**Global accumulation sweep** — ramps all control gates together to find the voltage where the channel first starts conducting.


4.


**Individual gate characterisation** — sweeps each reservoir, plunger, or barrier on its own while others are held at the conduction point to find its cutoff voltage.


That’s it — no complicated maths or fitting functions in sight, just straightforward measurements that give you confidence in your hardware.


### Built-In Stanza Advantages


Health Check isn’t a standalone script; it’s part of


**Stanza** , our open-source control framework. That means it inherits useful benefits out of the box:


-


**Configuration-first design** — Define all gates and parameters in YAML; no code edits required.


-


**Hardware-agnostic and scalable** — Works across different instruments and adapts to a wide range of quantum devices through Stanza’s driver abstraction layer. Whether you’re tuning a single dot or scaling to multi-dot and multiplexed arrays, the same Health Check framework scales with you.


-


**Automatic data logging** — Every measurement and result is saved in standard


**HDF5** and


**JSONL** formats with full metadata. No manual file handling — your data’s ready for any analysis tool.


-


**Result chaining** — Downstream routines can automatically access Health Check outputs, so you don’t have to pass parameters around manually.


Together, these features make Health Check not only easier to run but also easier to reproduce, share, and build upon.


### Getting Started


Using Health Check is simple:


1.


Annotate your device YAML with gate types (for example, plunger, barrier, reservoir) and voltage limits.


2.


Add a health_check routine to your configuration — most parameters have sensible defaults. Just hit run, and you’re off.


3.


Examine the results — noise floor, leakage report, and gate cutoff voltages — to decide your next steps.


Because all data is stored automatically in HDF5/JSONL, you can revisit results later or feed them directly into higher-level tuning routines.


### Final Thoughts


Quantum devices are delicate. Making sure they’re leak-free and ready for action should be as easy as pressing a button. Health Check gives you that button — wrapping up the messy work of baseline testing into a single command. Built on top of Stanza, it benefits from configuration-first design, modularity, scalability, and standardized data formats as we build for the million-qubit era.


Health Check makes setup effortless and scalable — so you can focus less on wiring and more on discovery.


Thanks for reading! Subscribe for free to receive new posts and support my work.
