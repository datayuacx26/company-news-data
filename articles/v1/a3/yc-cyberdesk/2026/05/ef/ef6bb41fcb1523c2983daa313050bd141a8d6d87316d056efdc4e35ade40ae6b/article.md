---
schema_version: "1.0.0"
document_id: "ef6bb41fcb1523c2983daa313050bd141a8d6d87316d056efdc4e35ade40ae6b"
company_key: "yc-cyberdesk"
company: "Cyberdesk"
source_id: "yc-cyberdesk-rss-7fc226611edf"
canonical_url: "https://cyberdesk-itcptau3z-cyberdesk.vercel.app/blog/introducing-cyberdriver-1-0"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-08-19T17:52:35.312646+00:00"
fetched_at: "2026-08-19T17:52:36.136936+00:00"
content_hash: "sha256:9c290d88e4e1d97aa59461f17aff8a80902355eb87407ae6091b3f56019f485c"
---

# Introducing Cyberdriver 1.0

Cyberdriver is how Cyberdesk reaches the computer.


It is the runtime that sits on a Windows desktop or virtual machine, keeps that machine connected to Cyberdesk, and gives AI workflows a reliable way to see the screen, move the mouse, type, run commands, collect evidence, and produce structured results.


Today we are introducing Cyberdriver 1.0 in beta: a new RustDesk-based Cyberdriver that makes the desktop feel less like a fragile agent process and more like an always-available automation surface.


## From agent script to desktop runtime


The first Cyberdriver was intentionally simple. It was a Python agent that could connect a machine to Cyberdesk and expose the tools a computer-use workflow needed: screenshots, mouse input, keyboard input, terminal commands, and a few recovery actions.


That was enough to prove the shape of the product. Developers could take software that had no API, connect it to Cyberdesk, and ask an AI workflow to operate it.


But production desktop automation needs more than a script.


It needs durable connections. It needs remote access when something goes wrong. It needs clean installation on Windows fleets. It needs session reliability, display recovery, diagnostics, logs, versioning, and a security model that understands organizations instead of passwords copied between machines.


Cyberdriver 1.0 is the next step in that direction.


## A RustDesk foundation


Cyberdriver 1.0 is built on a RustDesk-based foundation. That gives Cyberdesk a real remote desktop substrate instead of treating the agent as an isolated control loop.


The new client can register with Cyberdesk, advertise a RustDesk peer identity, connect through Cyberdesk-operated rendezvous and relay infrastructure, and authorize sessions using the Cyberdesk organization model.


That last part matters. Cyberdriver 1.0 is not asking customers to manage plain-text remote desktop passwords as the primary trust boundary. Users in the same Cyberdesk organization can connect to desktops they are allowed to use. Users outside that organization are rejected.


The RustDesk pieces remain under the hood. In the Cyberdesk dashboard, machines are still machines: named by the Cyberdesk desktop name or machine ID, not by an internal remote desktop ID.


## Thank you to RustDesk and the open-source community


Cyberdriver 1.0 would not exist in this form without RustDesk.


The RustDesk team built an open-source remote desktop system with cross-platform clients, rendezvous and relay infrastructure, input handling, screen capture, and video transport. That kind of platform work is difficult, unglamorous, and deeply valuable.


RustDesk made a self-hostable remote access foundation available to the world. Cyberdriver is a Cyberdesk product, with Cyberdesk-specific authentication, orchestration, dashboard streaming, diagnostics, and workflow integration. But it builds on the open-source work RustDesk made possible. We are grateful to the RustDesk maintainers and contributors, and we intend to respect that lineage through attribution, license compliance, and clear boundaries around what comes from the community.


## Streaming from the dashboard


The most visible change is web streaming.


For Cyberdriver 1.0+ machines, the Cyberdesk dashboard can open a live remote desktop stream directly from Desktop Tools. You can inspect the machine, use the keyboard and mouse, paste into the remote desktop, copy text back, send Ctrl+Alt+Del, reconnect, and enter fullscreen without opening a separate native remote desktop client.


This is not meant to replace the workflow engine. It is meant to make development and operations dramatically easier.


When a workflow behaves unexpectedly, you can open the desktop from the same dashboard where you see the run. When a Windows VM needs setup, you can finish configuration in the browser. When you are preparing an image, testing permissions, or verifying display behavior, you can stay in one place.


## MSI-first Windows setup


Most Cyberdesk users run automation on Windows. Cyberdriver 1.0 treats that as the default path.


The new setup is MSI-first: install Cyberdriver, enter your Cyberdesk API key, and let the desktop register. For image-based deployments, the CLI can be used from AMI bootstrap scripts so every cloned VM receives its own identity.


That matters for teams that create hundreds of machines from one image. Install the client once in the AMI, then reset the fingerprint and join Cyberdesk on first boot. Each clone appears as its own desktop in the dashboard.


```text
$Cyberdriver = "C:\Program Files\Cyberdriver\Cyberdriver.exe"
& $Cyberdriver reset-fingerprint
& $Cyberdriver join --secret $env:CYBERDESK_API_KEY --name "worker-$env:COMPUTERNAME"


```


The goal is to make Cyberdriver feel like normal infrastructure. It should fit into the way teams already build Windows images, scale worker pools, and manage environments.


## Keepalive and remote keepalive


Desktop automation often fails for mundane reasons. A VM locks. A remote desktop session goes idle. A display disappears. A workflow starts, but the machine is no longer in the state the agent expected.


Cyberdriver 1.0 includes keepalive support to keep machines lightly active while they are idle. For local cases, Cyberdriver nudges the cursor and restores it. For remote-desktop-managed VMs, remote keepalive lets a second Cyberdriver run on the host machine and keep the VM session active from the outside.


That pattern is important when the lock or idle policy is enforced by RDP, Avatara, AnyDesk, or another remote desktop layer. Running keepalive inside the VM may not be enough. Running a linked host Cyberdriver gives Cyberdesk a way to keep the session ready without repeatedly passing 2FA before every workflow.


## Diagnostics, logs, and operations


Cyberdriver 1.0 also improves the boring operational pieces that make a desktop runtime production-ready.


The client exposes diagnostics that include machine ID, peer ID, API configuration, environment, rendezvous and relay configuration, log paths, display dimensions, keepalive state, and recent log output. The CLI can tail logs. The dashboard avoids offering legacy Python-agent auto-updates to 1.x clients. Remote shutdown and update checks have a clearer path.


These details are not glamorous, but they are what make a connected desktop supportable.


## Part of a larger effort


Cyberdesk exists to make it extremely easy for developers to automate computers with AI.


That means the workflow authoring experience matters. The model matters. The replay system matters. But none of those pieces work without a reliable desktop runtime underneath them.


Cyberdriver 1.0 is that runtime becoming real infrastructure.


It gives developers a way to connect the messy, stateful, human-oriented software that businesses actually use: EHRs, ERPs, claims portals, thick clients, browser sessions, remote desktops, and internal tools. It lets AI workflows operate those systems without waiting for every vendor to ship an API.


The beta is already useful: install Cyberdriver, connect a Windows desktop, stream it from Cyberdesk, run workflows, inspect failures, and build image-based fleets with the CLI.


There is still more to do. We will continue improving streaming quality, keyboard fidelity, session recovery, clipboard behavior, multi-display handling, and production management controls.


But the direction is clear: making a computer available to an AI workflow should be as easy as installing a runtime and connecting it to your organization.


Cyberdriver 1.0 is the foundation for that.
