---
schema_version: "1.0.0"
document_id: "101ac20cea7771814e57dabae37a72a01669521757aeae0ff0943c4a3c9c8623"
company_key: "yc-conductor-quantum"
company: "Conductor Quantum"
source_id: "yc-conductor-quantum-rss-fd18baa21fa7"
canonical_url: "https://blog.conductorquantum.com/p/announcing-our-latest-model-for-coulomb"
published_at: "2025-09-28T23:01:28+00:00"
first_seen_at: "2026-07-24T23:15:28.367668+00:00"
fetched_at: "2026-07-28T20:55:46.222495+00:00"
content_hash: "sha256:21dd0fc601b3e69b4d79239fe0179d881432f0c47b32137204859b44380ad5cd"
---

# Announcing Our Latest Model for Coulomb Peak Detection

# Announcing Our Latest Model for Coulomb Peak Detection


### Product


Sep 28, 2025


*Originally published on* July 21st, 2025


*.*


Within three months of launching, Conductor Quantum released six tools for quantum device characterization and analysis. Today, we’re announcing our largest and most capable model yet:


` coulomb-blockade-peak-detector-v1`


Coulomb blockade is one of the most basic phenomena encountered by quantum dot engineers.


The electronic properties of quantum dots are governed by two main effects: the Coulomb repulsion between electrons (or holes) on the quantum dot, and the electron dynamics within the quantum dot being influenced by quantum effects due to confinement. This leads to quantum dots behaving as artificial atoms, exemplified by their discrete energy spectrum.


Here we focus on what arises from the Coulomb repulsion between electrons. When an additional electron is added to the quantum dot, there is an associated energy cost — referred to as the


**charging energy** . At low temperatures, unless the electrochemical potential of the dot aligns with the bias window, tunneling of electrons onto the dot is suppressed — this is the Coulomb blockade effect.


As you sweep the gate voltage, these alignment points show up as sharp peaks in the current flowing through the device:


**Coulomb peaks** .


By counting the number of Coulomb peaks, you are able to infer how many individual electrons you have loaded onto your quantum dot. This task, while a fundamental lego building block, is mission-critical for building a quantum computer out of electron spin qubits in semiconductor devices.


The task of peak detection may seem simple, but is one that has itched computer vision and signal processing engineers for years. Detecting peaks is easy but discerning peaks of interest from noise is not.


Many quantum engineers rely on standard packages such as SciPy’s \`find_peaks\` for their peak detection. This often requires manual tuning of hyperparameters and is rarely robust in noisy conditions.


Where v0 was optimized for low-resolution readout traces, v1 is 5× more capable — designed for wafer-scale cryoprobing, long sweeps, and noisy device conditions. It doesn’t just find peaks. It finds the **right** peaks — even in the most challenging traces.


**Peak detection evolution — from SciPy (left) to Conductor v0 (middle) to our latest model v1 (right). Cleaner outputs, fewer false positives, and higher confidence in signal interpretation.**


` coulomb-blockade-peak-detector-v1` is the next step in building the most reliable control and characterization software for semiconductor quantum devices. Get in touch if you’d like to try it out and join us as we continue leveraging AI to accelerate quantum technologies.
