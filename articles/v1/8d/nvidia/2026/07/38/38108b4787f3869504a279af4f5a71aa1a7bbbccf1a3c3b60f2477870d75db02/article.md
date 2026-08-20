---
schema_version: "1.0.0"
document_id: "38108b4787f3869504a279af4f5a71aa1a7bbbccf1a3c3b60f2477870d75db02"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/medical-physics-simulation-open-source/"
published_at: "2026-07-22T13:00:11+00:00"
first_seen_at: "2026-07-22T14:00:02.828351+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:43a352abbde13303ddc7a373aff2556b6275fc4bd79f0d5c5e05d2f136cada0c"
---

# NVIDIA Open Sources First GPU-Accelerated Medical Physics Simulation Framework

Before a healthcare robot can be useful in the real world, it has to learn how the physical world pushes back. Anatomy varies. Instruments bend, press, slip and interact with tissue. Imaging can be noisy or incomplete. And the rare, edge scenarios developers most need to understand don’t appear on schedule.


That creates one of the biggest bottlenecks in healthcare robotics: obtaining the enormous amount of varied data developers need to train, test and improve robot behavior.


NVIDIA Medical Physics Simulation framework — a new open source, GPU-accelerated capability within NVIDIA Isaac for Healthcare — announced today, helps medical robotics developers model anatomy-device interaction, generate hard-to-capture scenarios, test in silico, and train or evaluate robot policies before hardware-heavy testing.


The framework brings together anatomy and medical device behavior with sensor simulation and robot learning so teams can create reusable simulation environments instead of rebuilding custom scenes for every workflow, saving developers time and bringing innovations to market faster.


Because Medical Physics Simulation is open source, healthcare robotics developers can inspect the framework, adapt it to their own devices and workflows, and build on a GPU-accelerated foundation that works seamlessly with the broader NVIDIA stack.


Open source is especially important in healthcare because teams need transparency into the data, models and weights that shape system behavior. Access to open models and model weights can help developers reproduce results, evaluate performance across different anatomies and scenarios, identify limitations and build evidence for regulatory review.


## **A Virtual Training Ground for Medical Robots**


For


[physical AI](https://www.nvidia.com/en-us/glossary/generative-physical-ai/) , experience is data in motion. Developers need to train robots to operate properly even when anatomy changes, devices behave differently, conditions shift or a policy fails unexpectedly.


Medical Physics Simulation helps developers simulate anatomy, device contact, friction and sensor inputs, then test in interactions and environments to evaluate how robots perform across those changes. Powered by NVIDIA CUDA and part of Isaac for Healthcare — built on the NVIDIA Warp, Newton and Cosmos simulation and generative AI technologies — the framework can run hundreds of parallel simulation environments, helping teams explore more scenarios and identify failure modes earlier in development.


For robot builders, this turns simulation from a bespoke engineering project into reusable infrastructure. The difference now is scale:


[benchmarks](https://arxiv.org/abs/2503.18616) show 8,192 robot-training environments running in parallel with GPU-native simulation cut training from over five hours to under two minutes.


With this framework, developers can connect vascular anatomy, flexible instruments such as catheters and guidewires, simulated X-ray imaging and reinforcement learning. The framework is designed to extend beyond that example to additional devices, anatomies, sensors and healthcare robotics domains.


Medical Physics Simulation brings together classical physics simulation and generative AI physics simulation. Classical simulation helps model known physical rules, such as device contact, friction and motion.[NVIDIA Cosmos-H Dreams](https://github.com/isaac-for-healthcare/Cosmos-H-Dreams) , the real-time generative AI physics simulation capability within Medical Physics Simulation, helps model visual scene dynamics learned from procedural data.


Together, these approaches give developers a richer way to build and test healthcare robotics systems in virtual environments before moving to physical prototypes and lab testing.


## **An Ecosystem Building the Future of Medical Robotics**


Medical robotics leaders are already


applying simulation-driven development to solve specific surgical challenges


.


CMR Surgical and


Cambridge Consultants, part of Capgemini, are using


Cosmos-H-Dreams


to implicitly learn interaction physics for soft-tissue surgical procedures and generate patient-specific simulations. CMR contributed nearly 500 hours of anonymized clinical data from its Versius Surgical Robotic System to the Open-H Embodiment open dataset, benefiting procedures including cholecystectomy, prostatectomy, hernia repair and hysterectomy.


“Open source models allow us to build on shared knowledge, accelerating responsible innovation and, ultimately, gives us the potential to deliver more consistent care and better outcomes for patients worldwide,” said Chris Fryer, chief technology officer at CMR Surgical.


Johnson & Johnson MedTech i


s using Isaac for Healthcare’s Medical Physics Simulation and a Cosmos-based foundation model


to build digital twins of its endoluminal MONARCH platform for urology, modeling complex anatomy and kidney-stone scenarios.


XCath is using the Medical Physics Simulation for endovascular autonomy policy training.


Inner Logic i


s accelerating the evolution of medical technology with synthetic data, validating device mechanics and producing in silico evidence to support regulatory pathways with NVIDIA Medical Physical Simulation.


Medtronic Structural Heart is exploring applying Medical Physics Simulation with simulated X-ray sensing to generate data for catheter navigation research.


## **A New Layer in the Isaac for Healthcare Stack**


As a modular capability within NVIDIA Isaac for Healthcare, Medical Physics Simulation can be used on its own or alongside digital twin pipelines, medical sensor simulation, the NVIDIA Isaac Lab open robot-learning framework and NVIDIA open models and policies.


*Developers can explore the open source*[Medical Physics Simulation framework](https://isaac-for-healthcare.github.io/medical-physics-simulation/) *, review available reference workflows and start building simulation environments for their own devices, anatomies and healthcare robotics applications.*
