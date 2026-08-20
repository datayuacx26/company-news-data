---
schema_version: "1.0.0"
document_id: "e5050ed0e3fe26ba3b9de9387fb438a631d5713e03da2ac643a79956cf6c9b1b"
company_key: "yc-elodin"
company: "Elodin"
source_id: "yc-elodin-news-import-b1709095e6d7"
canonical_url: "https://www.elodin.systems/post/company-update-august-2024"
published_at: null
first_seen_at: "2026-07-25T02:35:46.271436+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:14527951b57fe09011f084c88c2afef2fc51646114615edf5fbb639e77e66e95"
---

# Company Update | August 2024

Update time! This month we have something new and exciting to announce!


‍[Aleph](https://www.elodin.systems/products/aleph) (aa-lef), our first hardware product. Elodin's mission is to make building flying autonomous systems simpler and more accessible than ever before. So, we've set out to build the perfect flight computer to pair with our platform.


‍


Aleph is designed from the ground up to be easily developed custom software for. We started with the Nvidia Orin NX module and paired it with an STM32H7. Aleph comes with Elodin's source-available flight controller that is designed to be easy to modify and customize. Need to fly in GPS-denied environments? Have a custom mission profile not supported by Ardupilot or PX4? Aleph lets you make your own flight controller custom-built for your exact needs. Aleph is split into two separate PCBs connected via a board-to-board connector.


**NVIDIA Main Board** The Nvidia Jetson Orin NX module, a powerful SoC featuring an 8-core CPU, 100 TOP GPU, and 16GB of RAM. The Orin NX is the perfect chip to run ML, CV, and any other computationally intensive workload you can imagine.


**Open Expansion Board**


We paired the Orin with an STM32H757, for hard real-time processing. The STM32H7 is a powerful dual-core Cortex M7/M4 microcontroller clocked at 480MHz and 240MHz respectively. **‍**


**‍** The expansion board has the H7, 3x Bosch BMI270 IMUs, a BMM350 magnetometer, a BMP581 barometer, and the standard complement of drone connectors. The expansion board is meant to be customized to each particular mission. Aleph V0 is designed for drones, but we can easily create new expansion boards for a variety of missions.


‍
Checkout the[website](https://www.elodin.systems/products/aleph) for more images/information.
If you are interested in purchasing or learning more about Aleph -contact us .


‍


### Product updates


You can find all our past and present logs on our[documentation site](https://docs.elodin.systems/reference/changelog) .


- You can now pan and zoom plots in the editor – making it easier than ever before to inspect close details.
- We enabled VSync on Windows and Linux for the smoothest possible experience.
- We finally added a desktop icon for Windows that we know everyone was begging for.
- We added built-in global components (currently includes simulation tick + time step).
- Startup times are now improved by adding a local cache for remote GLB assets.
- The CLI now respects .gitignore when uploading simulation artifacts.
