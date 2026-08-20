---
schema_version: "1.0.0"
document_id: "b6546d2188b23eb40680e27097b2e306538dcb59e524ab014d8ee199d919aae2"
company_key: "yc-elodin"
company: "Elodin"
source_id: "yc-elodin-news-import-b1709095e6d7"
canonical_url: "https://www.elodin.systems/post/update-june-2024"
published_at: null
first_seen_at: "2026-07-25T02:35:46.271436+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:c8d25aece53bb8b5d77ddcd3dcc37f4c768f8e3d4af0ebb55a122752449e2eaf"
---

# Company Update | June 2024

This past month we've been hard at work bringing the magic of Elodin to more places than ever before; from drones to satellites we've greatly expanded what you can use Elodin for!
‍
In addition, our CEO Dan, has been jet-setting around exposing the good word of Elodin. He joined the UC Berkeley rocket team at the Spaceport America cup, and appeared on the Lobster Talks podcast


‍


##### **🛰️ Elodin Flight Software**


We've been hard at work developing Elodin's next major feature, a framework for developing flight software. We call it ROCI – either named after the ship in the Expanse or a backronym for Rust Object Component Interface.


ROCI allows users to develop flight software quickly, test it against simulations, and then move it onto real hardware faster than ever before. We recently integrated our attitude determination system, built with ROCI, into a customer satellite. Below is a demo of us testing that satellite running ROCI and using Elodin as a real-time visualizer.


‍


##### **🐍 Basilisk integration**


Our latest integration is with Basilisk, a powerful tool for simulating spacecraft. This integration enables you to test Basilisk's flight software algorithms inside Elodin. Now, you can utilize your existing Elodin simulations and the full power of Basilisk.


In addition, we've added Basilisk support to our flight software framework, ROCI. This allows you to quickly package existing Basilisk modules into flight software. This integration allows users to move from simulation to real flight software quicker than ever before.


‍


##### **Introducing our Drone Control System series**


Developed a quadcopter simulation with motor response modeling for advanced control strategies.


Last week we developed a drone control system using our simulation platform. We aim to build a simulation of a simple quadcopter with a common cascaded PID control strategy. This will serve as a starting point for more sophisticated control strategies and more complex vehicles, such as a VTOL quadplane.


For a detailed guide, check out the full blog post[here](https://www.elodin.systems/post/tuning-a-drone-control-system-in-simulation) .


‍


##### **🎙️ Interview on The Lobster Talks Podcast**


**‍** Our CEO Daniel Driscoll went on the Lobster Talks Podcast where he talks Elodin, aerospace, funding, and book recommendations. Watch/Listen to the podcast[here](https://www.youtube.com/watch?v=E70WuZVZ-SU) .


Dan was interviewed by Gabriel Jarrosson (General Partner of Lobster Capital)


‍


##### 🚀 Spaceport America Cup 2024


**‍** Dan also made the trek into the desert of Las Cruces, New Mexico to join 152 teams of over 3,600 students and faculty for the annual[Spaceport America Cup](https://spaceportamericacup.com/) - The worlds largest intercollegiate rocket engineering conference and competition.
‍
As part of the trip, we sponsored the[University of California Berkeley (Launch Video!)](https://www.youtube.com/live/0IUsan9UwY0?feature=shared&t=25021)


University of California Berkeley Team / Back Half of their two stage rocket


University of California Berkeley Rocket Launch


‍


##### Product change log


You can find all our past and present logs on our documentation site:[Change Logs!](https://docs.elodin.systems/updates/changelog)


**v0.3.23**


- **(breaking)** Render the Z-axis as up in the editor (instead of the Y-axis). This is a purely visual change and does not affect simulation results, but it’s recommended to update simulations to match the new visual orientation. Typically, this requires swapping the Y and Z axes when operating on spatial positions, velocities, and forces.
- **(fix)** When a simulation file was changed, the associated pycache files would also be updated, causing the simulation to be re-built multiple times in some cases. This is now fixed.
- Add Status Bar to the editor (currently shows FPS/TPS and basic version of the connection status)
- **` elodin editor <path/to/sim>`** now watches the parent directory of the simulation file for changes in addition to the file itself. This is useful for multi-file projects. This is also the case when using the **` --watch`** flag directly. E.g. **` python <path/to/sim> run --watch`** .
- Export simulation data to a directory using **` exec.write_to_dir(path).`**


**v0.3.22**


- **(fix)** Fix errors when using **` vmap`** with **` scan`** , and non-scalar values


- When the arguments to a scan operation were non-scalar values (i.e their rank was above 0), scan would error in various ways when combined with vmap. The core issue is that some of our logic accidentally assumed an empty shape array, and when that array was non-empty, dimensions would be inserted into the wrong place.
