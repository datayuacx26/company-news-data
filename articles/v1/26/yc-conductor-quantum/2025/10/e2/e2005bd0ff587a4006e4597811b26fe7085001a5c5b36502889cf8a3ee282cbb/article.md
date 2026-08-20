---
schema_version: "1.0.0"
document_id: "e2005bd0ff587a4006e4597811b26fe7085001a5c5b36502889cf8a3ee282cbb"
company_key: "yc-conductor-quantum"
company: "Conductor Quantum"
source_id: "yc-conductor-quantum-rss-fd18baa21fa7"
canonical_url: "https://blog.conductorquantum.com/p/announcing-stanza-a-new-framework"
published_at: "2025-10-04T18:55:00+00:00"
first_seen_at: "2026-07-24T23:15:28.367668+00:00"
fetched_at: "2026-07-28T20:55:46.222495+00:00"
content_hash: "sha256:386d41b02772013703ccf2a1caff490303f3400d41dd95c7fa782c7a7585ca67"
---

# Announcing Stanza: A New Framework for Quantum Device Tune‑Ups

# Announcing Stanza: A New Framework for Quantum Device Tune‑Ups


### Product


Oct 04, 2025


**We are excited to introduce [Stanza](https://docs.conductorquantum.com/stanza/getting-started/overview)** , an open-source Python framework for building tune-up sequences for quantum devices. Quantum hardware is like a musical instrument – each device needs careful


**tuning** before it can perform. Stanza aims to make this tuning process


**easier, faster, and more automated** for quantum engineers. By combining simple configuration files with Python routines, Stanza lets you orchestrate complex device calibrations with minimal code. No heavy frameworks required (we don’t depend on QCoDeS).


Whether you’re a seasoned quantum researcher tired of writing one-off scripts, or a software engineer stepping into the quantum lab, Stanza provides a friendly way to define your device setup, write calibration routines as Python functions, and execute them with


**push-button simplicity** . Let’s dive into what Stanza is and how it works.


Thanks for reading! Subscribe for free to receive new posts and support my work.


## What Is Stanza?


[Stanza](https://docs.conductorquantum.com/stanza/getting-started/overview) is a


**Python framework for automating quantum device tune-ups** from us at Conductor Quantum. In practical terms, it’s a tool to help you configure your quantum hardware and run calibration routines without the usual hassle.


## Why Did We Build Stanza?


The motivation for Stanza comes from our experience in the quantum lab.


**Quantum device calibration is a bottleneck** : as devices scale up, manually tuning dozens of voltages by hand or maintaining huge scripts becomes unmanageable


[\[1\]](https://conductorquantum.substack.com/p/who-is-conductor-quantum-software) . Existing tools (like QCoDeS) can help, but they often carry a lot of baggage or steep learning curves for new users. We wanted something more lightweight and tailored to the specific workflow of


**quantum dot and spin qubit tune-ups** .


Stanza was built to


**embrace automation and scalability** from the ground up. By separating the configuration, routine logic, and execution engine, we make it easier to adapt and scale your tuning procedures. For example, when your lab moves from a single-qubit device to a 16-dot array, you can update your configuration file with new gates and add routines, without rewriting everything from scratch. The execution layer will orchestrate it all in a consistent way.


Another reason: we see Stanza as a stepping stone toward fully automated tune-up


**integrated with AI** . Conductor Quantum’s broader mission is to remove the human bottleneck in quantum scaling


[\[2\]](https://www.ycombinator.com/launches/Lfe-conductor-quantum-quantum-computers-on-silicon-chips-using-ai) . We’ve developed machine learning models for tasks like identifying turn-on points, pinch-off voltages, and Coulomb blockade peaks in device data


[\[3\]](https://conductorquantum.substack.com/p/announcing-our-latest-model-for-coulomb) . Stanza is designed to play nicely with these models. In the near future, you can imagine running a Stanza routine to collect data and then seamlessly feeding that data into our AI models for analysis – all in one pipeline. This combination of


**quantum device control + AI-driven analysis** is what excites us, and Stanza is a core part of that vision.


Lastly, practicality: We deliberately made Stanza


**agnostic of specific control electronics vendor frameworks** . It currently supports instruments like Quantum Machine’s QDevil


**QDAC-II** (a 24-channel DAC for quantum control) out-of-the-box via raw PyVisa. We know every lab has a different mix of equipment, so we plan to


**add support for more control electronics** over time, from other DACs to measurement units and beyond. If it speaks Python, we want Stanza to be able to talk to it! By not being tied to one closed ecosystem, Stanza can evolve with the needs of the community.


## Key Features


Key aspects of Stanza include:


-


**YAML Device Configuration** : YAML is a human readable format to define your device’s components in a single file. You can list out your


**gates** ,


**contacts** (e.g. source/drain), and


**instruments** along with their parameters and limits. This configuration-driven approach means you can tweak hardware setups or routine parameters by editing a single file,


**no code changes needed** . For example, you might specify which gate is a “barrier” vs. a “plunger”, which channels they connect to, safe voltage ranges, etc. Stanza will load this config and understand your device’s topology.


-


**Simple Routine Definitions** : Write


**tune-up sequences as simple Python functions** . You just decorate a function with @routine (provided by Stanza) and implement your calibration logic inside. The function can take parameters (like voltages to sweep or which gate to tune), and those parameters are automatically populated from the YAML config at runtime. This means your routine code stays clean and


**focused on the experiment** , not on parsing config files or setting up instruments.


-


**Automatic Logging and Results** : Every time you run a routine, Stanza handles the data logging for you behind the scenes. It can log data to convenient formats like HDF5 or JSONL, so you


**never forget to hit “save”** on that important measurement. Routines can also


**return results** (like measured currents, extracted parameters, etc.), and Stanza makes it easy to retrieve those results later or pass them into subsequent routines. This is great for building multi-step calibrations – for instance, using the output of a coarse tune-up as the input for a fine-tune routine.


-


**Lightweight and No QCoDeS Dependency** : Stanza does


**not rely on QCoDeS** , the (often heavyweight) framework commonly used in quantum labs. Instead, Stanza is a clean-slate design tailor-made for quantum device tuning. It interfaces with hardware directly via its own drivers or lightweight libraries, so you don’t have to pull in the entire QCoDeS stack. The result is a more


**streamlined experience** with fewer moving parts – just install Stanza and you’re ready to go.


Stanza lets you focus on the problem (like tuning up that double quantum dot and adjacent charge sensor) rather than boilerplate code.


**In summary,** Stanza is our approach to simplifying the quantum tuning process with a modern, lightweight framework. We can’t wait to see how you use Stanza in your labs and projects, and we’re excited to continue improving it with your input. Together, let’s automate the boring stuff, so you can focus on the exciting science and engineering that truly matters.


*Happy tuning,*


**[Hello Stanza.](https://docs.conductorquantum.com/stanza/getting-started/overview)**


Thanks for reading! Subscribe for free to receive new posts and support my work.
