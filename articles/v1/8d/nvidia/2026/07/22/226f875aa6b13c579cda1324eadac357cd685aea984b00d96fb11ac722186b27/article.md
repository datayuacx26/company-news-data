---
schema_version: "1.0.0"
document_id: "226f875aa6b13c579cda1324eadac357cd685aea984b00d96fb11ac722186b27"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://nvidianews.nvidia.com/news/nvidia-agent-toolkit-expands-with-new-omniverse-libraries-putting-ai-agents-to-work-building-simulation-ready-worlds"
published_at: "2026-07-20T15:00:00+00:00"
first_seen_at: "2026-07-20T15:51:13.846262+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:2233a9e58d6861f1a39668198ce7e5e2dcec7b03ce1ee85679a32d23a08d97db"
---

# NVIDIA Agent Toolkit Expands With New Omniverse Libraries, Putting AI Agents to Work Building Simulation-Ready Worlds

**News Summary:**


- NVIDIA Agent Toolkit now includes NVIDIA Omniverse libraries, giving AI agents tools and skills to help software developers integrate physical AI capabilities into their existing applications.
- New Omniverse libraries for NVIDIA RTX sensor simulation, GPU-accelerated physics simulation and simulation-ready asset validation are openly available on GitHub.
- SideFX and PTC are integrating Omniverse libraries into 3D applications for physical AI, with support for cloud and local AI systems, from NVIDIA RTX Spark to NVIDIA DGX Station.
- New NVIDIA blueprint for integrating Omniverse libraries in Blender.


NVIDIA today announced that NVIDIA Agent Toolkit now includes[NVIDIA Omniverse](https://developer.nvidia.com/omniverse) ™ libraries — a collection of software components that give AI agents tools and skills to add[physical AI](https://www.nvidia.com/en-us/glossary/generative-physical-ai/) capabilities to existing applications and prepare 3D content for simulation.


Robots, factories and autonomous systems need to be designed, tested and trained in simulation before they operate in the real world. Preparing 3D content for simulation takes more than realistic visuals — assets need the right structure, materials, scale, labels, sensors and physical properties. With NVIDIA Omniverse libraries in NVIDIA Agent Toolkit, AI agents have the tools and skills to build workflows, inspect scenes, flag issues and prepare assets, helping developers move faster from 3D content to simulation-ready environments.


“The physical AI era will be built in simulation first,” said Jensen Huang, founder and CEO of NVIDIA. “NVIDIA Agent Toolkit with Omniverse libraries brings AI agents into the 3D tools developers already use, helping build the simulation-ready worlds where robots, factories and autonomous systems are trained and tested long before they reach the real world.”


Software makers including SideFX and[PTC](https://www.ptc.com/) are integrating Omniverse libraries for agent-ready sensor simulation, physics and asset validation, helping bring agentic AI into the applications and workflows developers and technical artists already use to prepare 3D content.


**Omniverse Libraries Bring Physical AI Skills to NVIDIA Agent Toolkit**
NVIDIA Agent Toolkit helps software makers build AI agents that connect tools, skills and data sources. Omniverse libraries extend those agents into 3D and physical AI workflows with callable tools for sensor simulation, GPU-accelerated physics and simulation-ready asset validation inside existing applications.


The new Omniverse libraries — including ovrtx, ovphysx and CAD-to-SimReady skills — are openly available on[GitHub](https://github.com/NVIDIA-Omniverse/) , giving AI agents tools[to build workflows](https://developer.nvidia.com/blog/integrate-nvidia-omniverse-rtx-sensor-simulation-into-existing-apps/) for inspecting scenes, testing changes and preparing 3D assets for simulation. A new blueprint for[integrating Omniverse libraries in Blender](https://www.youtube.com/watch?v=XdtQbMXHDjQ) is also now available on[GitHub](https://github.com/NVIDIA-Omniverse/omniverse-labs/tree/main/projects/ov-blender-example) .


The libraries’ key capabilities include:


- **NVIDIA RTX sensor simulation:** ovrtx helps applications generate camera, lidar, radar and other sensor outputs from 3D scenes, so developers and AI agents can test how physical AI systems may perceive virtual environments.
- **Physical behavior:** ovphysx uses GPU-accelerated physics to bring realistic behavior to 3D scenes using properties such as collisions, mass, friction and motion, so teams can first test how objects and systems interact in simulation.
- **Simulation-ready 3D objects:** CAD-to-SimReady skills help convert computer-aided design (CAD) data to[SimReady](https://www.nvidia.com/en-us/glossary/simready/) assets built on[OpenUSD](https://www.nvidia.com/en-us/glossary/openusd/) , giving 3D content the properties needed for physical AI simulation and virtual testing.


**Software Makers Build With Omniverse Libraries**
Software makers including SideFX and PTC, as well as startups ForgeCAD, Lightwheel, Moonlake AI and Palatial, are among the first to adopt and build with Omniverse libraries, now part of NVIDIA Agent Toolkit.


SideFX is using OpenUSD workflows, as well as ovrtx and ovphysx libraries, to explore how agents can help integrate Omniverse libraries into its Houdini procedural 3D content creation workflows, giving technical artists a path to generate, test physics and prepare content for simulation.


“Procedural 3D creation is essential to building the complex, controllable worlds needed for simulation, robotics and industrial AI,” said Kim Davidson, president and CEO of SideFX. “With NVIDIA Omniverse libraries and OpenUSD, SideFX is exploring how agent-ready tools can support Houdini workflows, helping technical artists review, test and prepare procedural content for simulation while staying in control of the creative process.”


The[PTC Onshape](https://www.ptc.com/en/products/onshape) CAD and product data management (PDM) platform[is using](http://www.onshape.com/en/blog/cloud-native-cad-render-studio-nvidia-ovrtx) OpenUSD and ovrtx to connect cloud-native design workflows with physical simulation, helping product design content stay connected with CAD, PDM, collaboration and simulation workflows.


“Engineering teams are seeking more connected ways to design, collaborate and simulate throughout the development process,” said Neil Barua, president and CEO of PTC. “PTC’s work with NVIDIA supports that broader vision, while NVIDIA Omniverse libraries help enable simulation-ready workflows that bring validation and testing closer to where products are designed.”


On display at SIGGRAPH, “SimReady” Blender is a sample workflow built in Blender with NVIDIA Omniverse libraries and[NVIDIA NemoClaw™](https://www.nvidia.com/en-eu/ai/nemoclaw/?_bt=804567865336&_bk=nvidia%20nemoclaw&_bm=e&_bn=g&_bg=197993095849&gad_source=1&gad_campaignid=23744621431&gbraid=0AAAAAD4XAoGQ5597EGUzd4SypdlldcK7h&gclid=CjwKCAjwpefSBhBvEiwAzyEtZzjplA11sNjJgtjk6KNAQRuHqAmYNZ2w5-iSDaTVwVLoCAPlJTBEYRoCE5UQAvD_BwE) , showing how software makers can add agent-ready simulation capabilities — including NVIDIA RTX sensor simulation, physics and validation — into existing 3D applications while keeping creators in control. This is now openly available as a blueprint for integrating Omniverse libraries in Blender.


[The demo](https://www.youtube.com/watch?v=XdtQbMXHDjQ) also previews how these workflows, built with Omniverse libraries as part of NVIDIA Agent Toolkit, can[run locally](https://blogs.nvidia.com/blog/siggraph-news-2026/#nemoclaw-dgx-station) , from compact RTX-powered systems with[NVIDIA RTX Spark™](https://www.nvidia.com/en-us/products/rtx-spark/) to NVIDIA GB300-powered systems with[NVIDIA DGX Station™](https://www.nvidia.com/en-us/products/workstations/dgx-station/) . RTX Spark systems will be available this fall from ASUS, Dell Technologies, HP, Lenovo, Microsoft Surface and MSI, with models from Acer and GIGABYTE to follow.[DGX Station](https://www.nvidia.com/en-us/products/workstations/dgx-station/) systems are available to order from ASUS, Dell, GIGABYTE, HP,[MSI](https://www.msi.com/Landing/NVIDIA-DGX-STATION) , Supermicro and Exxact.


Startups, including those part of the[NVIDIA Inception](https://www.nvidia.com/en-us/startups/) program, are also using Omniverse libraries and skills to add agent-assisted asset and scene preparation workflows. Palatial is using Omniverse CAD-to-SimReady skills to automate the creation and validation of SimReady assets at scale from CAD inputs. Lightwheel is using Omniverse Content Agents powered by OpenUSD in its SimReadyGen technology to generate physically accurate SimReady assets from text prompts.


ForgeCAD and Moonlake AI are exploring agent-driven 3D content workflows that use Omniverse capabilities to help generate, augment and prepare assets for physical AI simulation.


*Watch the*[NVIDIA keynote](https://www.nvidia.com/en-us/events/siggraph/) *at SIGGRAPH. Learn more about*[NVIDIA Omniverse libraries](https://omniverse.nvidia.com/) *and explore available samples and documentation.*
