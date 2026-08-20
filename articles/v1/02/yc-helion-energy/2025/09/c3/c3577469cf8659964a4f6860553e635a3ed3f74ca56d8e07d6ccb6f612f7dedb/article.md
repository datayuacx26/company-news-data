---
schema_version: "1.0.0"
document_id: "c3577469cf8659964a4f6860553e635a3ed3f74ca56d8e07d6ccb6f612f7dedb"
company_key: "yc-helion-energy"
company: "Helion Energy"
source_id: "yc-helion-energy-news-import-5ae3e56c82e0"
canonical_url: "https://www.helionenergy.com/blog/how-we-know-whats-happening-inside-a-fusion-plasma-plasma-diagnostics"
published_at: "2025-09-22T00:00:00+00:00"
first_seen_at: "2026-07-21T22:45:07.501995+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:99406fac63b8b72e9f243f09ad20dd6f96ad89131626eeb8c53a0e6cad4d30e1"
---

# How we know what’s happening inside a fusion plasma: Plasma diagnostics

Fusion occurs in plasmas that face extreme temperatures, densities, and pressures. Plasmas, which are ionized gases confined by magnetic fields, change rapidly; to measure what's happening within them is extremely difficult. However, to understand fusion output, it is essential that we gather data from these plasmas. How do we do that? By using a series of plasma diagnostics that output data after every plasma pulse in our machines.


Together, these diagnostics (in addition to neutron and electrical diagnostics, whose details are beyond the scope of this post) enable us to learn from the plasma during every experimental campaign, inform in-house simulation work, and, ultimately, help us design machines that can reliably generate electricity from fusion.


## Obtaining data from plasma is hard


In Helion’s pulsed field-reversed configuration (FRC) approach, plasmas only last for a millisecond, which means we need very high-speed data acquisition tools to learn what is happening before the plasma disappears. The extreme conditions faced within our operational regimes make measurement even more difficult.


At millions of degrees Celsius, we cannot insert diagnostic probes or sensors directly into our machines. We operate in high magnetic fields with hundreds of mega-amps of current flowing through coils, along with x-rays and energetic particles. Electronics cannot survive in these conditions. Plasmas also have a tendency to find their way into electrodes, shorting out electronics unless the systems are carefully isolated.


Because of these challenges, most of what we measure is indirect through inductive and optical diagnostics. Inductive loops measure changing magnetic fields while optical fibers allow us to bring signals back to shielded rooms where we can collect the data.


After collecting signals such as magnetic flux, interferometry data, and optical spectra, we then rely on[physics-based simulations](https://www.helionenergy.com/articles/from-code-to-compression-how-simulation-accelerates-fusion-engineering/) to reconstruct what is happening inside. This feedback loop between diagnostics and modeling is essential. We use experimental data to verify our simulations and vice versa, which means that we are constantly learning and improving our methods to measure and predict plasma behavior.


## What we measure in the plasma


Each pulse gives us a fleeting opportunity to ask a few essential questions about the pulse: How big is it? How dense? How hot? Is it stable?


For FRCs, the primary plasma diagnostic is measuring magnetic fields. By measuring excluded magnetic flux and local magnetic fields, we can infer the plasma radius, helping us answer our first question about plasma size.


The next question is about how dense it is. Interferometry gives us an average electron density by measuring how a laser beam’s phase shifts as it passes through the plasma. With the radius and electron density in hand, we can calculate the plasma temperature using pressure balance. In an FRC, the outward pressure from the plasma must balance the inward pressure from the surrounding magnetic field. Knowing the plasma size and density allows us to determine how much pressure it is exerting, and from that we can estimate its temperature.


Beyond these core measurements, additional diagnostics provide more detailed information about temperature and stability. Spectroscopy splits the light from the plasma into its component wavelengths. The relative brightness of different lines reveals the electron temperature, while the broadening of the lines from moving ions shows the ion temperature. Fast cameras give visual information about plasma shape and whether it is approaching the chamber walls.


Every measured pulse also produces basic machine health data. These are like the plasma’s vital signs, where our operators check voltages, switch timing, and plasma radius immediately to ensure that the pulse was nominal and not damaging to the chamber walls. We use models to reconstruct more detailed parameters, including trapped flux, which helps us understand stability and confinement, and temperatures profiles. In this way we balance immediate operational needs with longer-term understanding.


## Why diagnostics matter


Diagnostics are the foundation for scaling Helion’s systems. In the early stages, they allowed us to measure how plasmas behaved in small prototypes and establish empirical scaling laws. By tracking parameters such as how quickly the plasma’s magnetic flux and particles decay, we learned how performance should change as we increased machine size and ramped up magnetic field strength. That knowledge reduced risk when moving from smaller prototypes to full-scale systems.


Today, plasma diagnostics play a different role. They validate whether our models are capturing the right physics. For example, understanding whether electrons are heating faster than we theorize helps us know if our simulations are accurate. When measurements and models agree, we have more confidence in designing the next machine.


As we transition from prototype systems like Polaris to commercial plants, diagnostics will also need to evolve. Today we can download millions of data points per pulse and analyze them between pulses. A commercial power plant continuously running will not have that same luxury. Operators will need summary indicators of success, such as whether the plasma compressed correctly, how much fusion output was achieved, and whether the plasma interacted with the walls. Diagnostics will shift from experimental analysis to fast gauges that confirm machine health in real time.


Plasma diagnostics are both essential and extraordinarily difficult. They require creative engineering, careful modeling, and indirect measurement techniques that can withstand extreme environments. Without them, we could not improve performance, scale machines, or design power plants with confidence. This is a very challenging field, and there are few experts worldwide who know how to do it well.[We are always looking for people](https://job-boards.greenhouse.io/helionenergy) with expertise in experimental diagnostics, instrumentation, and engineering who can help advance the tools needed to measure and understand fusion plasmas.
