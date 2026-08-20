---
schema_version: "1.0.0"
document_id: "3ea14cee467db884ab7fafe7d6161fcabdb58e1faecc0c558f02678becb2c91c"
company_key: "yc-helion-energy"
company: "Helion Energy"
source_id: "yc-helion-energy-news-import-5ae3e56c82e0"
canonical_url: "https://www.helionenergy.com/blog/from-code-to-compression-how-simulation-accelerates-fusion-engineering"
published_at: "2025-06-25T00:00:00+00:00"
first_seen_at: "2026-07-21T22:45:07.501995+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:e8e4fa112be5017ef274f1e3bc015a636f7fa9974153ff12e69ed23f37216826"
---

# From code to compression: How simulation accelerates fusion engineering

Before any hardware exists, Helion uses advanced simulation tools to predict how plasmas will behave inside our fusion systems. These predictions shape design decisions and give confidence that the plasma will perform as needed once the machine is built and operated.


Helion’s fusion systems depend on forming and controlling high-energy field reversed configuration (FRC) plasmas, which are inherently complex to model with internal currents, rather than external currents, generating the magnetic fields that confine the plasma. To model these systems accurately, we can use physics-based simulation tools that help predict performance, validate results, and accelerate fusion engineering and design.


### Predicting how plasmas behave


Plasma physics is complex. Unlike gases or liquids that interact only through collisions, plasmas are also influenced by electromagnetic fields. This makes them behave in ways that are difficult to understand directly with diagnostics alone. Helion’s simulation tools allow us to "see" what diagnostics cannot by filling in the gaps in experimental measurements with physics-based predictions.


Our simulation suite captures several important plasma phenomena, including magnetic field evolution, plasma confinement, and the interaction of the plasma with external circuits. These models help us understand[stability limits](https://www.helionenergy.com/articles/a-note-on-frc-instabilities/) such as interchange, tilt, and shearing instabilities that affect plasma lifetime and performance.


Because of the complexity of plasma physics, we need multiple tools in our simulation suite to capture and predict both the global dynamics and fine-scale stability properties needed to design and operate our machines for ideal plasma performance. FRC plasmas can be approximated using fluid and kinetic (particle) models. Our simulation tools combine traditional magnetohydrodynamic (MHD) models and more novel hybrid-kinetic particle-in-cell (PIC) models.


Traditional MHD simulations, using tools like CYGNUS, are a critical part of our simulation workflow. MHD models treat plasma as a continuous fluid, therefore capturing global dynamics with bulk plasma motion, pressure, and magnetic field evolution. These models are computationally efficient and are used to model FRC formation, translation, and merging, as well as the coupling of the plasma to machine circuits. They serve as valuable post-pulse analysis tools and help refine engineering designs.


Three-dimensional hybrid-kinetic PIC models, such as WarpX, an[open-source tool](https://blast-warpx.github.io/) developed by the DOE Exascale Computing Project, provide a way to simulate the detailed kinetic behavior of plasma ions and treat electrons as a fluid, capturing physics that MHD models alone cannot. WarpX includes a hybrid kinetic ion/inertialess fluid electron generalized Ohm’s law particle-in-cell algorithm that improves scalability over traditional electromagnetic PIC methods for modeling ion dominated plasma dynamics.[1,](https://doi.org/10.1016/j.cpc.2022.108457)[2](https://doi.org/10.1063/5.0178288)


*Cross-section of a 3D Hybrid-PIC simulation: Two merging FRCs exhibiting the tilt instability after merge*


### Building trust in simulation


Simulation is only useful if it can be trusted to make reliable predictions. Our simulation models are not just theoretical, they’re calibrated and validated using results from our six previous prototypes and the ongoing Polaris testing campaign, giving us a rare and growing dataset to compare against and refine our predictions. Our computational team is also collaborating with external partners to validate our models against independent experiments and contribute back to the open-source WarpX project. This process of validation and iteration with our own and external experimental data builds confidence that our models capture real plasma behavior.


While our MHD models have been validated through repeated comparison with data from past prototypes, we have expanded our simulation capability by building on WarpX to develop three-dimensional hybrid-kinetic modeling and augmented the existing solver with the addition of a particle-based electron energy equation. This work has been benchmarked against historical data from the Large-S Experiment (LSX) experiment[3](https://apps.dtic.mil/sti/tr/pdf/ADA639301.pdf) and early formation studies that demonstrated the first 3D simulations of theta-pinch FRC formation.[4](http://dx.doi.org/10.1103/PhysRevE.92.023105)


*Simulation of LSX formation*


Building on this foundation, we have recently validated our models against experimental data on FRC merging and compression. Published hybrid simulations have confirmed that magnetic compression improves merging quality and captures both fluid and kinetic effects critical to performance.[5](https://doi.org/10.48550/arXiv.2501.03425) These results provide important validation of Helion’s pulsed compression approach.


Our models predict that FRCs can achieve thermonuclear-relevant conditions, reaching densities of 10²² particles per cubic meter and temperatures of 8 to 10 keV.[6](https://youtube.com/watch?v=3FwOeN-zcPY) Continued simulation efforts will be instrumental to optimize FRC formation to increase peak compression temperatures above 20 keV. These results align with experimental scaling relationships and support our ability to design machines that achieve these conditions.


### Simulation is a design accelerator


Simulation is a key part of Helion’s design process. It allows us to explore design trade-offs, optimize machine parameters, and make engineering decisions before any hardware is built. This accelerates the development of new machines and reduces the risk of costly design changes later in the process.


Helion’s simulation-design-build feedback loop starts with physics-based modeling to predict plasma behavior. These predictions are then used to guide engineering decisions, such as magnetic coil placement and operational timing. Once a machine is built and operating, data from experiments is used to calibrate and improve the models.


One recent example that showed the value of this work came out of our plasma formation tests in our Formation testbed. With 3D modeling, we were able to see micro-instabilities that had a big impact on flux loss during formation that were not found by diagnostics or 2D models. Based on these insights, the team proposed adjustments to coil operational timing to mitigate instability growth. Experimental results confirmed improved FRC lifetimes and overall machine performance, demonstrating the role of simulation in guiding operational improvements.


Our computational team is now using simulation to inform the design of future machines, building on the validated models developed from past prototypes and insights from the Polaris testing campaign. The goal is not just to match past results, but to enable larger performance leaps between machines by reducing reliance on empirical scaling and incorporating additional first-principles physics directly into the models. With continued advances in fidelity and capability, especially in 3D kinetic modeling, we can make increasingly predictive engineering decisions that accelerate fusion power plant design.
