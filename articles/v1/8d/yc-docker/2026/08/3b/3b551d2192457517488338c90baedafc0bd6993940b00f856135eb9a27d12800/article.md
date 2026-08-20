---
schema_version: "1.0.0"
document_id: "3b551d2192457517488338c90baedafc0bd6993940b00f856135eb9a27d12800"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/docker-vmm-public-beta/"
published_at: "2026-08-12T18:13:20+00:00"
first_seen_at: "2026-08-12T21:33:27.943349+00:00"
fetched_at: "2026-08-12T21:33:29.398384+00:00"
content_hash: "sha256:bda077ee24306bb8c2f8f39a15fae10db3d17f76be26e178625a08bf0fb6655e"
---

# Docker VMM Public Beta: A Complete Overhaul, Built for Performance

Today we’re announcing the public beta of a fully rebuilt Docker VMM: a new first-party virtualization layer underneath[Docker Desktop](https://www.docker.com/products/docker-desktop/) , optimized for containers, and now available on both Mac and Windows starting with Docker Desktop v4.86.


## What’s Changed, and Why It Matters


Part of the magic of Docker Desktop is how it provides a seamless deployment of the Linux-native Docker engine on other platforms, like macOS and Windows. To support that, Desktop automatically creates and manages a VM and all the complicated integration of your local network and filesystem, in a safe and performant way.


Creating that VM is the job of a virtual machine monitor, the layer that sits between your hardware and the containers Docker runs. Most developers never think about it. But when it’s slow, unstable, or holding onto your machine’s memory it should have released, you notice it constantly.


Docker Desktop has always relied on a third-party VMM for this. Now it runs on Docker VMM, built by us from the ground up. That means we own the full stack, and we can tune every part of the engine for container workloads specifically. That translates directly to you: an engine that improves continuously, responds to developer feedback, and ships on our own schedule.


This matters for everyone running Docker Desktop today. Performance, stability, and governance improvements at the virtualization layer enhance the experience across the board, for every workflow, on every team.


Image 1: Isometric diagram of the Docker stack: Host, DockerVMM, and Docker Engine layers.


## The Performance Improvements Are Real


Here’s what you’ll notice when you start using the beta release of Docker VMM:


**Faster startup.** Container startup is measurably faster across the board, from first launch to project switches to restart recovery.


**Better file I/O.** File sharing between container and host is significantly faster. When you’re in an edit-compile-test loop, you’ll see improvements every single build.


**Smarter memory management.** Docker VMM returns memory to the host when containers are idle, so Docker Desktop isn’t holding onto RAM you’re not using.


**Improved stability on Windows.** For the first time, Windows developers get a VMM built and maintained by Docker, with performance and stability work coming straight from us.


**Stronger isolation, better performance.** DockerVMM still runs in a fully isolated VM, optimized for performance. On Windows, that means the isolation you’d expect from Hyper-V with the speed you’d expect from WSL2.


## One Engine, Everywhere You Run Docker


The virtualization engine powering Docker VMM also powers Docker Sandboxes (SBX). That’s not a coincidence; it’s intentional. Every improvement lands in both products, so you get them wherever you choose to run Docker.


This matters beyond performance. As we build deeper capabilities into the engine, including enterprise admin controls and tighter governance for dev environments, they surface across both products. Longer term, we’re building toward a unified runtime that spans laptop, cloud, and on-prem, where containers, Compose apps, and agents are all first-class on one foundation. Docker VMM is how Docker Desktop gets there, and this is step one.


## How To Enable It


**On Mac:** If you are already using Docker VMM in Settings, you will be automatically updated to the new engine when you upgrade to v4.86.


**On Windows:** Open Settings > General and you will see a new “Docker VMM” option. Switch it to opt in.


Image 2: Feature Flag for Docker VMM in Settings


No feature flag, no waitlist. Any Docker Desktop user on v4.86 or later can switch today. *Note: Linux support will be available at GA.*


## What’s Next


Beta runs through fall, focused on real developer workflows: builds, file syncs, and the container startup patterns you hit every day.


GA is targeted for the end of October 2026, when Docker VMM becomes the default engine for new Docker Desktop installs across Mac, Windows, and Linux. GA is the baseline, and from there, the pace picks up. Everything we build next sits on this foundation.


## Try It Today


Update to[Docker Desktop v4.86](https://docs.docker.com/desktop/release-notes/#4860) to get started.


Noticing a difference? Have ideas for where you’d want us to go next? We’re collecting feedback through[in-product](https://docs.docker.com/desktop/troubleshoot-and-support/feedback/) responses, our[community Slack](https://dockr.ly/comm-slack) , and[support](https://www.docker.com/support/) channels.


This is the best Docker Desktop has ever run, and it only gets better from here.


### Learn more


- Read more on[Docker Docs](https://docs.docker.com/desktop/features/vmm/)


**Docker VMM is available today in public beta in Docker Desktop v4.86 for Mac and Windows. Follow the Docker blog to stay up to date on GA and what comes next.**
