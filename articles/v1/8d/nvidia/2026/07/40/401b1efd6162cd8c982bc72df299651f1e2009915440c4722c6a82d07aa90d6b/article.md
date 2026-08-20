---
schema_version: "1.0.0"
document_id: "401b1efd6162cd8c982bc72df299651f1e2009915440c4722c6a82d07aa90d6b"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/siggraph-news-2026/"
published_at: "2026-07-20T15:00:53+00:00"
first_seen_at: "2026-07-20T15:51:13.846262+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:6795a391c6aa2b0c02bdf0f3f9254f9e798f8c1a57990ae5d7d1218c90f68e69"
---

# At SIGGRAPH, NVIDIA Advances Graphics and Simulation With Agentic and Physical AI

At this year’s SIGGRAPH conference, running through Thursday, July 23, in Los Angeles, attendees can discover how leading graphics research, neural rendering, simulation and AI are transforming how worlds are created and understood by people and machines.


The[NVIDIA keynote](https://www.nvidia.com/en-us/events/siggraph/) , taking place today, July 20, at 3:45 p.m. PT, will feature NVIDIA AI research and engineering leaders Neil Ashton, Edward Liu and Ming-Yu Liu discussing neural rendering techniques, world models and simulation methods for AI, built by AI.


Read on for the latest from the SIGGRAPH conference, with NVIDIA and partners showcasing:


- [NVIDIA keynote recap](https://blogs.nvidia.com/blog/siggraph-news-2026/#keynote)
- [Model Context Protocol connections bring agentic AI to content creation](https://blogs.nvidia.com/blog/siggraph-news-2026/#mcp)
- [New Synthetic Video Detector NIM microservice](https://blogs.nvidia.com/blog/siggraph-news-2026/#synthetic-video)
- [Cosmos 3 Edge open world model for local physical AI](https://blogs.nvidia.com/blog/siggraph-news-2026/#cosmos-3)
- [NVIDIA NemoClaw on DGX Station with NVIDIA Agent Toolkit](https://blogs.nvidia.com/blog/siggraph-news-2026/#nemoclaw-dgx-station)
- [Research breakthroughs for virtual worlds and physical AI](https://blogs.nvidia.com/blog/siggraph-news-2026/#research)


---


## In SIGGRAPH Keynote, NVIDIA Leaders Outline Next Era of Graphics and Physical AI[🔗](https://blogs.nvidia.com/blog/siggraph-news-2026/#keynote)


Ming-Yu Liu, vice president of Cosmos Lab at NVIDIA, presents at SIGGRAPH 2026.


NVIDIA AI research and engineering leaders took the stage at SIGGRAPH today to share the latest advances in neural rendering, world models and simulation. These technologies — which are changing the way digital worlds are created and used across every domain — can be applied to creative tools, industrial design, robotics and autonomous systems.


“Creators and designers need tools powerful enough to expand their imagination, malleable enough to give them the freedom to shape ideas and precise enough to realize their vision exactly as intended,” said NVIDIA founder and CEO Jensen Huang in an introductory video. “Whether for games, cinema, robotics or factory digital twins, the goal is the same: to create virtual worlds that behave with the fidelity and realism of the physical world.”


Edward Liu, director of applied deep learning research at NVIDIA, kicked off the presentation by showcasing 3D-guided neural rendering.


He shared how NVIDIA’s latest advancements in neural rendering address three key research challenges: preserving artistic intent, ensuring output is temporally stable across frames and rendering 4K content in real time.


“Control in artistic direction is a really exciting research direction for us,” said Edward Liu. “Simulation defines the world, generation enriches its appearance and artists direct the outcome…AI extending graphics the same way programmable shaders and ray tracing have extended graphics before.”


Neil Ashton, distinguished engineer at NVIDIA, covered innovation in AI physics with applications in weather forecasting, automotive aerodynamics, thermal design and more.


In climate and weather research, the


[NVIDIA Earth-2](https://www.nvidia.com/en-us/high-performance-computing/earth-2/) family of open models


is enabling scientists to boost their simulation resolutions to new levels with AI models trained on simulation data — while achieving accuracy equal or greater to traditional approaches.


Neil Ashton, distinguished engineer at NVIDIA, presents about NVIDIA Earth-2 at SIGGRAPH 2026.


“The challenge that we have is to translate the success of using these AI models in weather and climate into the world that we live in, to be able to design the planes and cars and data centers of the future,” Ashton said. “One of the things that NVIDIA is really focused on is helping to achieve that vision.”


Ashton shared how the latest model architectures developed by


[NVIDIA Research](https://www.nvidia.com/en-us/research/) and its partners compress model checkpoints down to a couple hundred megabytes, a million-times compression that enables physically accurate visualizations within a second.


Ming-Yu Liu, vice president of Cosmos Lab at NVIDIA, shared the latest advancements in world models available through


[NVIDIA Cosmos](https://www.nvidia.com/en-us/ai/cosmos/) ,


a world foundation model platform for accelerating the development of physical AI systems.


“What makes a foundation model a foundation model is its capability to consume enormous amounts of diverse data,” he said. “We trained Cosmos with data for diverse physical tasks, including world understanding, prediction, simulation and action. We use one backbone to solve all the different tasks.”


Ming-Yu Liu walked through the mixture-of-transformers model architecture used to give NVIDIA Cosmos 3 global understanding. This capability enables physically accurate results that work across different embodiments — humanoid robots, grippers or self-driving cars.


“Every embodiment speaks a different language,” he said. “Our solution is to build a common vocabulary.”


NVIDIA Cosmos includes a family of open models in various sizes, supporting developers across a range of deployment use cases. Its newest addition, released today, is


[Cosmos 3 Edge](https://blogs.nvidia.com/blog/siggraph-news-2026/#cosmos-3) , a 4-billion-parameter model built to run in real time, on device.


Ming-Yu Liu also announced Cosmos-Dreams, a collection of closed-loop simulators, with a colleague demonstrating a simulator built for autonomous vehicles that generates an entire world from a single frame, running on a single


[NVIDIA RTX PRO 6000 GPU](https://www.nvidia.com/en-us/products/workstations/professional-desktop-gpus/rtx-pro-6000/) .


Developers can use Cosmos-Dreams to verify the accuracy of their models before deploying them in a real fleet — or use it to train models on AI-generated scenarios that are difficult to create in the real world, accelerating development while saving on operational costs.


*Watch the*[full keynote](https://www.youtube.com/live/vXDetI2TUWw) *and read more about*[Cosmos 3 Edge](https://blogs.nvidia.com/blog/siggraph-news-2026/#cosmos-3) *.*


---


## AI Agents Expand Creative Tools to Millions[🔗](https://blogs.nvidia.com/blog/siggraph-news-2026/#mcp)


Image courtesy of Epic Games.


Leading creative applications are opening Model Context Protocol (MCP) connections that let AI agents work inside the tools where scenes, shots, timelines, assets and edits come to life — while creators stay in control.


For more than two decades, NVIDIA technologies — from GPU-accelerated viewports and CUDA-powered effects to NVIDIA RTX PRO ray tracing, AI denoising, neural rendering and real-time simulation — have helped accelerate the DCC tools that artists, studios and developers use to build the world’s games, films, television shows and advertising content.


MCP is opening the next chapter of accelerated creativity: applications aren’t just getting faster. They’re becoming agent-ready.


### **From Acceleration to Action**


With MCP-connected tools, an artist or technical director can ask an agent to inspect a scene for missing textures, flag inconsistent color management, prepare export variants, generate playblasts for dailies or validate a shot against pipeline rules, all while keeping creative decisions in human hands.


The same NVIDIA platform that accelerated viewports, rendering, simulation and AI effects can now power local agents, model inference and multi-application workflows on systems designed for professional creators.


NVIDIA RTX PRO workstations, DGX Spark and DGX Station systems are designed to bring accelerated AI performance closer to artists, developers and studio pipelines. Running models and agents locally can help improve responsiveness, reduce reliance on external services and keep sensitive creative data in controlled environments.


NVIDIA Agent Toolkit also supports MCP integration, including an MCP client for connecting to remote MCP servers and an MCP server for publishing tools to any MCP client.


### **The Creative Ecosystem Goes Agent-Ready**


Across the creative ecosystem, creative applications and platforms are exposing MCP connections or MCP-ready workflows, giving AI agents more grounded access to real production context.


**Adobe** is expanding its creative agent across Firefly, Express and Creative Cloud, powering AI Assistant experiences that enable creators to describe the outcome they want while the assistant orchestrates multistep workflows. Adobe is also bringing its pro-grade creative tools to third-party AI platforms through the Adobe connector, extending its creative capabilities wherever people create and work. For developers, Adobe provides the Adobe Express Developer MCP Server, enabling AI coding assistants to build Adobe Express add-ons using official documentation and application programming interfaces (APIs).


[https://blogs.nvidia.com/wp-content/uploads/2026/07/Firefly-Creative-Agentic-June-News-2.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/07/Firefly-Creative-Agentic-June-News-2.mp4)


**Affinity by** **Canva** has introduced an AI Connector for Claude that uses MCP to bring natural-language automation directly into Affinity. Designers can ask Claude to handle repetitive production tasks such as renaming layers and artboards, resizing and reformatting assets for multiple channels, applying bulk edits, optimizing vector paths and preparing files for delivery. Beyond individual tasks, Claude can also help users build reusable scripts and custom features tailored to their workflows, reducing production overhead and


giving creative professionals more time to focus on design.


[https://blogs.nvidia.com/wp-content/uploads/2026/07/Affinity-by-Canva-MCP.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/07/Affinity-by-Canva-MCP.mp4)


**Blender** offers a lightweight MCP server through


Blender


Lab, providing a natural-language interface to


Blender


’s Python API, documentation and complex setups. For independent artists and studios,


Blender


offers a strong example of how open creative tools can become agent-accessible without changing the creative center of gravity.


**Boris FX Silhouette** now includes an MCP server that lets AI assistants work directly inside your projects. Using Silhouette’s FX Scripting API as first-class MCP tools, assistants can inspect projects, build node trees, edit shapes and keyframes, and render frames. A new preferences panel simplifies setup by installing the MCP package, generating a ready-to-paste client configuration, and testing the connection. Interactive online mode connects to your active session, while offline mode runs headless instances for automation, batch processing, and large-scale workflows.


**Foundry Griptape** natively supports MCP, providing AI orchestration specifically designed for professional VFX pipelines. This integration enables studios to securely manage multiple AI models and agents while maintaining the necessary traceability and creative control. By integrating with tools like


Blender


and


Foundry


Nuke, Griptape automates repetitive production tasks — such as cleanup, matte painting and quality control — all while ensuring artists remain in final command of the creative process.


**SideFX** is bringing MCP support to Houdini 22 through its new APEX Script workflow. AI assistants can access a curated collection of APEX Script syntax, functions, documentation and examples, helping artists generate and refine code for procedural character rigs. SideFX’s initial implementation focuses on APEX Script and character rigging, while community-developed MCP servers offer broader ways for agents to interact with Houdini.


[https://blogs.nvidia.com/wp-content/uploads/2026/07/Houdini-22-MCP.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/07/Houdini-22-MCP.mp4)


**Unreal Engine** recently announced the ability to connect AI clients to Unreal Editor through MCP, enabling AI workflows that can interact with editor capabilities through a standardized protocol. For game developers, virtual production teams and real-time artists, this opens the door to assistants that can reason over scenes, assets and project state.


[https://blogs.nvidia.com/wp-content/uploads/2026/07/Unreal-MCP.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/07/Unreal-MCP.mp4)


*See how NVIDIA RTX PRO and DGX systems bring local AI agents closer to creative work at*[SIGGRAPH](https://www.nvidia.com/en-us/events/siggraph/) *.*


---


## NVIDIA AI for Media Helps Newsrooms Detect Synthetic Video[🔗](https://blogs.nvidia.com/blog/siggraph-news-2026/#synthetic-video)


Every day, video brings the world’s biggest stories into view — from breaking news across continents, to events reshaping communities, to moments that unite people across the globe. In a news cycle that moves around the clock, trustworthy video is the medium through which people see what’s happening, understand why it matters and stay connected and up to date.


For that reason, public trust in video is more essential than ever. At SIGGRAPH, NVIDIA announced the


[Synthetic Video Detector NVIDIA NIM microservice](https://build.nvidia.com/nvidia/synthetic-video-detector) , part of the NVIDIA AI for Media platform, to bring an AI-assisted detection signal into editorial and media workflows.


The NIM microservice analyzes video frame by frame to produce a classifier score of whether it contains synthetic content. Editorial teams can use that score to prioritize clips for review, flag or quarantine questionable footage, or escalate it for deeper analysis.


Rather than replacing established verification practices, the microservice provides another signal for time-sensitive decisions — helping teams move quickly while protecting editorial standards and ensuring public trust.


Synthetic Video Detector remains effective after the compression, resizing, cropping and re-encoding steps common in newsroom and social-video workflows. In NVIDIA testing, the model’s accuracy reached up to 92% on uncompressed video, 87% at 15% compression and 82% at 50% compression.


The NIM microservice can process 1080p video in as little as 22 milliseconds on NVIDIA RTX systems and approximately 30 milliseconds on NVIDIA L40 GPUs.


### **Deploy Detection Where Video Lives**


Organizations can deploy the NIM microservice closer to where sensitive video is captured, stored or distributed, including in on-premises, edge, hybrid and approved air-gapped environments. This flexibility helps teams maintain control over video data, access and operations.


Partner adoption is already helping move Synthetic Video Detector from model capability to deployable media infrastructure.


Wowza


is embedding the microservice through the


[Wowza Video Intelligence Framework](https://www.wowza.com/video-intelligence-framework) , bringing real-time synthetic video detection into livestreaming workflows used across more than 35,000 deployments in over 170 countries.


[https://blogs.nvidia.com/wp-content/uploads/2026/07/synthetic-video-nim-demo.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/07/synthetic-video-nim-demo.mp4)


That scale matters because many of the organizations most exposed to synthetic media risk, including broadcasters, government agencies, financial institutions and critical infrastructure operators, also face strict requirements around data residency, security and operational control.


By pairing Synthetic Video Detector with a video infrastructure layer customers already use,


Wowza


can help make AI-assisted verification available closer to ingest and streaming operations, allowing teams to flag questionable video in real time while keeping sensitive footage inside their own environments.


*Try the*[NVIDIA Synthetic Video Detector NIM microservice](https://build.nvidia.com/nvidia/synthetic-video-detector) *.*


*See*[notice](https://www.nvidia.com/en-eu/about-nvidia/terms-of-service/) *regarding software product information.*


---


## Now Openly Available, NVIDIA Cosmos 3 Edge Brings Frontier World Models to Edge GPUs for Local Physical AI[🔗](https://blogs.nvidia.com/blog/siggraph-news-2026/#cosmos-3)


[https://blogs.nvidia.com/wp-content/uploads/2026/07/cosmos-3-video.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/07/cosmos-3-video.mp4)


[Physical AI](https://www.nvidia.com/en-us/glossary/generative-physical-ai/) systems rely on world models to perceive, reason over and predict the physical environment. But the real world is vast, unpredictable and always changing.


Whether a robot navigating a warehouse or a camera network monitoring a factory floor, physical AI systems need to understand what’s happening now, reason about what may happen next and act quickly enough to have an impact. Until now, delivering such frontier AI at the edge has often meant trading model capability for deployment efficiency.


Now available,[NVIDIA Cosmos 3 Edge](https://blogs.nvidia.com/blog/jetson-thor-robotics-edge-ai-agent/) helps eliminate that tradeoff. The 4-billion-parameter omnimodel is optimized for memory-efficient deployment and high throughput on


[NVIDIA Jetson](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/) , NVIDIA RTX PRO and NVIDIA DGX systems, as well as GeForce RTX GPUs.


Extending NVIDIA Cosmos 3, the compact world foundation model can understand and generate text, image, video, ambient sound and action. Its


[mixture-of-transformers](https://www.nvidia.com/en-us/glossary/mixture-of-transformers/) architecture enables physically grounded, real-time vision analytics and robot action on device.


Cosmos 3 Edge delivers frontier physical AI at the edge — ranking No. 1 on


[VANTAGE-Bench](https://huggingface.co/spaces/clemson-computing/VANTAGE-Bench-Leaderboard) for vision analytics success in its parameter class and enabling state-of-the-art robot learning through post-training.


### **On-Device Physical AI Across Robotics, Autonomous Vehicles and Smart Infrastructure**


Developers can post-train Cosmos 3 Edge on proprietary robot and sensor data using the


[NVIDIA DGX Station](https://www.nvidia.com/en-us/products/workstations/dgx-station/) deskside AI supercomputer to build specialized world action models, then deploy them on


[NVIDIA Jetson Thor](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-thor/) for real-time robot control policies including for manipulation or locomotion.


Agile Robots


,


Doosan Robotics


,


Siemens


and


Skild AI


are among the partners that are evaluating Cosmos 3 Edge for robotics workflows.


For autonomous vehicles, Cosmos 3 Edge supports road-scene understanding, traffic reasoning, object-intent prediction and policy-model distillation on resource-constrained hardware. The model could be used as a student backbone for automotive policy model distillation, including with


[NVIDIA Alpamayo](https://www.nvidia.com/en-us/solutions/autonomous-vehicles/alpamayo/) vision language action models.


For smart infrastructure, Cosmos 3 Edge enables


best-in-class throughput and accuracy with real-time inference on Jetson Thor for


vision agents that reason across live video streams for traffic monitoring, public safety, logistics and industrial inspection. Developers can also run the 2-billion-parameter


[NVIDIA Nemotron](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Ultra-Technical-Report.pdf) -powered reasoning module independently on NVIDIA Jetson Orin 8GB.


[Centific](https://www.centific.com/blog/centific-brings-real-time-physical-ai-to-the-edge-with-nvidia-cosmos-3-edge) ,


[Vaidio](https://www.vaidio.ai/blog/bringing-vlm-reasoning-to-the-edge-with-nvidia-cosmos-reason-3-edge-nvidia-jetson-thor-and-vaidio-vision-ai) and


[YUAN](https://www.yuan.com.tw/newscontent/352) are evaluating Cosmos 3 Edge to accelerate vision agents running at the edge.


### **Cosmos Platform Now Openly Available**


Cosmos 3 Edge is part of the broader NVIDIA Cosmos platform for developing physical AI


[world models](https://www.nvidia.com/en-us/glossary/world-models/) . With Cosmos 3 available in Edge (4B), Nano (16B) and Super (64B) sizes, developers can choose the right model for each stage of development, from edge deployment to high-fidelity generation.


*Cosmos 3 Edge, Cosmos 3 Nano and Cosmos 3 Super are available now on*[Hugging Face](https://huggingface.co/collections/nvidia/cosmos3) *, with inference and post-training frameworks and recipes on*[GitHub](https://github.com/NVIDIA/cosmos) *.*


---


## AI Agents Made Easy: **Build and Run Personal AI Agents Locally on DGX Station With NVIDIA Agent Toolkit**[🔗](https://blogs.nvidia.com/blog/siggraph-news-2026/#nemoclaw-dgx-station)


Super agents have arrived on the desktop.


[NVIDIA DGX Station](https://www.nvidia.com/en-us/products/workstations/dgx-station/) is the ultimate deskside supercomputer for the AI era, and with NVIDIA Agent Toolkit, setup takes just three steps, and the system can be running in roughly 30 minutes.


On DGX Station,


NVIDIA Agent Toolkit brings together


[NVIDIA NemoClaw](https://www.nvidia.com/en-us/ai/nemoclaw/) ,


the


[NVIDIA Nemotron](https://www.nvidia.com/en-us/ai-data-science/foundation-models/nemotron/) 3 Ultra open model,


[NVIDIA Omniverse](https://www.nvidia.com/en-us/omniverse/) libraries as agent-accessible tools and skills, and a secure runtime in a single local system — no internet required. As workloads scale, developers can connect multiple systems together to serve concurrent users, more agents and bigger models.


This gives creatives and engineers the ability to own their own intelligence, with a system that comes ready to run locally. The full stack stack — model, agent, tools — provides a platform for creating and running domain-specific “super agents” that are customized with users’ own data and knowledge.


The open NVIDIA Agent Toolkit stack on DGX Station includes:


- **NVIDIA NemoClaw** , open blueprints for building custom autonomous agents, packaging the model, harness and runtime together as a starting point for teams building specialized, domain-specific agents.


- **NVIDIA Nemotron 3 Ultra** , a frontier 550-billion-parameter


[open model](https://www.nvidia.com/en-us/glossary/open-models/) , is optimized to run on DGX Station GB300 systems and serves as the model layer that teams can customize for their own domains.


- [NVIDIA Omniverse libraries](https://nvidianews.nvidia.com/news/nvidia-launches-omniverse-libraries-putting-ai-agents-to-work-building-simulation-ready-worlds) extend agent skills into physics simulation and 3D asset workflows, giving creative and engineering professionals tools that go well beyond general-purpose agent capabilities.


- **NVIDIA OpenShell** , the open source secure runtime, keeps agents sandboxed and governed according to defined policies for how agents interact with tools, systems and data.


- **NVIDIA GB300 Grace Blackwell Ultra Desktop Superchip** delivers data-center-level performance from the desk on DGX Station, with up to 20 petaflops of FP4 AI compute and 748GB of coherent memory to run large models such as Nemotron Ultra.


- **NVIDIA ConnectX-8 SuperNIC** delivers up to 800GB/s of bandwidth in DGX Station, delivering extremely fast, efficient network connectivity, and supports linking up to two DGX Stations to further scale model capacity and performance.


### **Harness Efficiency at Scale**


For teams running agents at scale the economics shift fundamentally on DGX Station. Nemotron 3 Ultra, tuned for an open harness, delivers leading-edge performance without the per-token cost after the hardware purchase, so users build once and can run as much as they need.


NVIDIA has announced a blueprint for integrating


[NVIDIA Omniverse libraries in Blender](https://nvidianews.nvidia.com/news/nvidia-launches-omniverse-libraries-putting-ai-agents-to-work-building-simulation-ready-worlds) — giving NemoClaw agents callable RTX sensor simulation and physics tools to prepare 3D scenes for physical AI workflows.


On DGX Station, designers and engineers can run the core pieces of that workflow — frontier model, open harness, secure runtime, 3D tools — in one box, all connected and deployable through an open blueprint.


Frontier models can orchestrate NemoClaw as a specialized sub-agent, delegating domain-specific work to an agent running locally on DGX Station, with direct access to Omniverse tools and Blender.


[LangChain](https://www.langchain.com/deep-agents) tuned its Deep Agents harness for Nemotron 3 Ultra, giving designers and engineers a production-ready path to benchmark-leading agentic performance at a fraction of the cost.


Nous Research f


ine-tuned Nemotron 3 Ultra for its Hermes Agent harness and adopted it for production workloads — a direct demonstration of the value of owning intelligence. Tuning the model for a developer’s stack enables agents that are both faster and more capable for specific domains. Hermes Agent has also added Blender to its Model Context Protocol catalog, letting teams activate Blender directly from their agent — a live example of a tool-using NemoClaw agent that can run on DGX Station.


For teams running


OpenClaw


, this stack extends what’s possible — bringing Nemotron 3 Ultra, Omniverse tools and local inference on DGX Station into an environment where OpenClaw’s persistent, long-running agents can act on them continuously.


### **Develop and Deploy Quickly With New Playbooks**


Two new playbooks are available now to help developers build and run agents out of the box with NemoClaw and dual-node deployments.


- **“**[Connect Two DGX Stations for Distributed Workloads](http://build.nvidia.com/station/connect-two-stations) **”** guides users on how to run powerful models with scalable performance connecting up to two DGX Station systems.


- **“**[Run NemoClaw With a Local LLM / On Dual DGX Station](http://build.nvidia.com/station/nemoclaw/dual-nodes) **”** provides a walkthrough on how to set up NemoClaw on DGX Station, enabling out-of-the-box agents with frontier intelligence with Nemotron Ultra.


NVIDIA DGX Station is built and available to order from


ASUS


,


Dell Technologies


,


Exxact


,


GIGABYTE


,


[HP](https://reinvent.hp.com/ZGX-FURY) ,


[MSI](https://www.msi.com/Landing/NVIDIA-DGX-STATION) and


Supermicro


.


*Get started with*[NVIDIA NemoClaw and Nemotron Ultra on DGX Station](https://build.nvidia.com/station) *.*


---


## NVIDIA Brings Graphics Research Breakthroughs to Simulation and Physical AI[🔗](https://blogs.nvidia.com/blog/siggraph-news-2026/#research)


[https://blogs.nvidia.com/wp-content/uploads/2026/07/MotionBricks.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/07/MotionBricks.mp4)


At SIGGRAPH this year, NVIDIA’s research isn’t just focused on creating worlds that look real — but that behave realistically and respond in real time.


That shift is the throughline across


[NVIDIA’s 21 accepted technical papers](https://www.nvidia.com/en-us/events/siggraph/?ncid=pa-srch-goog-402247&_bt=814816029562&_bk=siggraph%202026&_bm=p&_bn=g&_bg=198041251157&gad_source=1&gad_campaignid=23983113048&gbraid=0AAAAAD4XAoGu-CLG1KhR-QMqHhfK3cxDB&gclid=Cj0KCQjwjb3SBhDgARIsAMKiWzisIsVlPycDsStcOB9DqCiA7wYybSID14TY0ntYlBnz8m09HRa4DnkaAqMxEALw_wcB) — becoming the foundation of real-time systems that generate virtual worlds and drive machine training in the real world.


Whether the output is a game, film, robot or factory digital twin, the goal is the same: expand the canvas of creativity with AI-generated worlds that are grounded in 3D, governed by physics and directed by creators.


[https://blogs.nvidia.com/wp-content/uploads/2026/07/MotionBricks-Robot.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/07/MotionBricks-Robot.mp4)


The clearest proof is in


[MotionBricks :](https://nvlabs.github.io/motionbricks/) a real-time motion model — trained on more than 350,000 motion clips, running at game-engine speeds — that lets creators direct and connect character movements. The same model that drives the animated character on screen drives a Unitree G1 humanoid robot in the room, using computer graphics and simulation to accelerate physical AI development.


[GPC](https://yi-shi94.github.io/gpc-page/) , a


framework for training generative controllers on large-scale motion datasets,


extends that idea. NVIDIA pretrains a single controller on large-scale human motion, giving it transferable motor skills that carry over to new tasks. Think of it as the start of a foundation model for motor control.


[https://blogs.nvidia.com/wp-content/uploads/2026/07/GPC-Demo.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/07/GPC-Demo.mp4)


To build virtual worlds in which to test these movements,


[ArtiFixer](https://research.nvidia.com/labs/sil/projects/artifixer/) turns messy real-world 3D captures into clean, complete virtual scenes. It also includes a new method for predicting photoreal global illumination straight from a scene’s geometry — without tracing a single ray.


To make those virtual worlds behave as they would in the real one, a new solver brings hard-to-simulate materials — such as snow, sand and elastic solids — to life inside the NVIDIA Newton physics engine.


And to keep creators in control, the


[VideoNeuMat](https://research.nvidia.com/labs/rtr/publication/xue2026videoneumat/) pipeline gives them reusable, relightable materials to pull out of generative video models, while the


[ARDY](https://research.nvidia.com/labs/sil/projects/ardy/) autoregressive diffusion model lets them steer 3D character motion in real time from a text prompt.


*The papers linked above are openly available, with the code and models free to download. Learn more by joining*[NVIDIA at SIGGRAPH](https://www.nvidia.com/en-us/events/siggraph/) *.*
