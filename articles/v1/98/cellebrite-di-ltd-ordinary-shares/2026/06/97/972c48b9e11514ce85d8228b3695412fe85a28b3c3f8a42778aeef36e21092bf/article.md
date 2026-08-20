---
schema_version: "1.0.0"
document_id: "972c48b9e11514ce85d8228b3695412fe85a28b3c3f8a42778aeef36e21092bf"
company_key: "cellebrite-di-ltd-ordinary-shares"
company: "Cellebrite DI Ltd."
source_id: "cellebrite-di-ltd-ordinary-shares-news-import-5108dc200e80"
canonical_url: "https://cellebrite.com/en/blog/virtual-arm-hardware-firmware-development-corellium-atlas/"
published_at: "2026-06-12T08:32:25+00:00"
first_seen_at: "2026-07-21T12:57:31.558749+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:df09792e779371a2bec3541ea59cf615213335db161ebd681649eaf23ec370e5"
---

# Building Embedded Firmware Without Touching Hardware

[Blog](https://cellebrite.com/en/blog/) / Building Embedded Firmware Without Touching Hardware


# Building Embedded Firmware Without Touching Hardware


June 12, 2026


| Jason Yamada


-


-
-
- Email


*An AI-native workflow for Arm development using Corellium Atlas, MCP, and Kiro IDE*


**Key highlights:**


- **Physical hardware slows embedded development:** Boards, labs, HIL rigs, wiring, flashing, and hardware failures create delays that AI-generated code cannot overcome on its own.
- **Corellium Atlas makes Arm hardware instantly programmable:** Teams can provision, snapshot, reset, share, and test virtual Arm-based devices without waiting on physical boards, lab access, shipping delays, and configuration issues.
- **MCP brings virtual hardware into the IDE:** Corellium MCP and CoreModel MCP let AI agents create devices, manage instances, inject peripheral signals, and validate behavior directly inside the development workflow.
- **The workflow shifts embedded testing earlier:** Developers can build, deploy, stimulate, observe, and iterate on firmware against any virtual hardware before reaching the HIL.


Embedded development has carried the same limitation for decades. Boards have to be procured, racked, wired, and maintained. All this happens before the boards are even added to the testing pool and takes anywhere from weeks to months. Test labs are expensive and never scale the way teams need them to. Every other corner of software engineering has automated past these problems. Embedded has lingered on simulators and emulators that don’t provide the fidelity of physical models leading to bugs discovered while testing in the HIL.


This study looks at what changes when the hardware itself becomes a virtual resource — provisioned by API, scripted from an IDE, and accessible to AI agents and MCP-based workflows. Specifically, it walks through designing, building, deploying, and validating production-style firmware on a virtual i.MX93-EVK, end to end, without a physical board.


## The cost of staying tied to physical hardware


Hardware labs, device farms, and HIL rigs are expensive and operationally heavy. Boards have to be procured, shipped, inventoried, configured, validated, integrated into automation, and replaced when they fail. The availability of that setup is limited and quality is variable. A single failure in that system can derail an entire sprint, forcing teams to reorder boards, resolder hardware, and rebuild integrations from scratch.


Even after the hardware arrives, the integration work is only beginning. Boards still need to be wired, flashed, mounted in racks, and connected to CI. If anything fails during bring-up, the cycle starts over. For globally distributed teams, the friction only grows: physical boards can’t be shared instantly across time zones, snapshotted to a known state, or provisioned on demand.


Traditional simulators were supposed to help here. In practice, most of them validate narrow, preset scenarios that don’t survive contact with real firmware and only really test at the driver level. They’re useful for unit-level work but not the full fidelity of the physical hardware, leaving bugs to be discovered in the HIL. Debug should never happen in the HIL.


**Key Take Aways:**


- Hardware is limited
- HIL systems are complex
- Time slips
- Sprints slip
- Simulators don’t work
- Bugs discovered in the HIL


## Why AI makes the bottleneck harder to ignore


AI is reshaping embedded software development. More code, means more bugs, with less available testing surface when using the traditional HIL approach to automation testing and deployment. AI agents can reason over requirements, build logs, and test failures. Code generation is fast and getting faster. MCP servers expose tools — build systems, debuggers, peripherals, instance management — directly to the model inside the IDE.


The bottleneck is no longer writing code. It’s getting that code in front of hardware fast enough to learn anything from it. If the loop ends at “now wait for the lab,” the model’s speed advantage disappears. Hardware that can’t be created, reset, observed, and shared programmatically becomes the constraint.


**Key Take Aways:**


- AI code generation is here
- The HIL, and SIL are not fast enough or capable enough
- The speed of AI and Agentic coding is lost


## Corellium Atlas as the foundation


Corellium Atlas provides hypervisor-backed virtual hardware for Arm targets — modeled to the memory map and capable of running the same firmware images intended for the physical device. For this study I used Atlas alongside Kiro IDE, Corellium MCP, CoreModel MCP, and the Corellium VS Code extension to build an AI-assisted workflow for the i.MX93-EVK.


## The workflow


The target was a custom FreeRTOS firmware for the i.MX93’s Cortex-M33 core, including sensor handling and security logic. Sensor data flowed from the M33 across to the A55 application processor, where an HMI-style PLC application surfaced it through a login screen, alert acknowledgment, and a reset path back to the M33.


Every step happened inside the Kiro IDE:


- **Instance lifecycle:** Corellium MCP handled create, start, stop, snapshot, share, and inspect operations. The virtual EVK could be brought up, torn down, or rolled back to a snapshot without leaving the editor.
- **Peripheral stimulus:** CoreModel MCP drove SPI, GPIO, CAN, I²C, and UART signals into the running instance. No external boards, no jumper wires, no logic analyzers physically attached. Test signals were function calls.
- **Device access:** The Corellium VS Code extension provided a console straight into the virtual device, so debug and validation didn’t require a browser session.
- **Orchestration:** Kiro IDE tied the loop together: design, code, build, deploy, exercise peripherals, observe behavior, iterate.


The experience was indistinguishable from having an i.MX93-EVK wired to my desk, with three real advantages: every step was scriptable, every state could be captured in a snapshot, and nothing depended on a shared lab resource.


## Future of embedded software development today


Done traditionally, this kind of bring-up coordinates across embedded engineers, application developers, automation engineers, QA engineers, and whoever owns the hardware lab. Boards have to be available. Wiring has to be correct. Lab time has to be reserved. Failures during flashing or signal injection cost real hours.


Corellium Atlas virtual models and MCPs collapsed that coordination cost. Bring-up was a tool call. Peripheral testing was a script. A failed run reverted to a known-good snapshot in seconds. The same steps I took are reproducible with the same configured environment.


**Key Take Aways:**


- Firmware was generated for Linux and M33 on iMX 93
- End-to-end code to validation testing was done instantly with bring up and MCP orchestration


## The shift this points to


The interesting claim isn’t that virtual hardware can replicate a board. It’s that hardware infrastructure can start behaving like software infrastructure — instantly available, scriptable, shareable, resettable, scalable, and legible to AI agents over MCP. With that fact, embedded development stops being the part of the stack where automation is limited or not possible and becomes an instant and easy shift left.


The teams that move fastest from here won’t be the ones with the biggest labs. They’ll be the teams who adopt Corellium Atlas and whose hardware can be created, controlled, and validated from the same place their code lives.


**If you want to try this for yourself, I have the snapshots that can run and clone from here** : (follow the PLC_Demo_RunGuide in the Github repo)


**i.MX93 | PLC Demo | NK68X61C5B82I**


**Generic Linux | Sensor Simulator | WZFN10L339GU**


**The repo to the firmware I built and ran is here:**


https://github.com/corellium/PLC-Demo.git


# Frequently Asked Questions (FAQ)


### What is Corellium Atlas?


Corellium Atlas provides virtual Arm hardware models that allow embedded teams to build, deploy, debug, and validate firmware without relying on physical boards. In this workflow, Atlas was used to run a virtual i.MX93-EVK and validate production-style firmware end to end.


### What problem does virtual hardware solve for embedded teams?


Virtual hardware reduces dependency on physical boards, hardware labs, and HIL systems during early development and validation. Instead of waiting for boards to arrive, be wired, configured, flashed, or repaired, teams can provision and reset virtual devices through software.


### Why does this matter for AI-assisted embedded development?


AI can generate code quickly, but embedded teams still need a way to test that code against realistic hardware behavior. When the workflow stops at “wait for lab access,” AI speed is lost. Virtual hardware keeps the development loop fast because hardware can be created, controlled, and observed programmatically.


### What is MCP in this workflow?


MCP, or Model Context Protocol, gives AI agents access to tools and systems inside the development environment. In this workflow, Corellium MCP handled virtual device lifecycle operations, while CoreModel MCP drove peripheral signals such as SPI, GPIO, CAN, I²C, and UART into the running virtual device.


Share this post


-


-
-
- Email


**
