---
schema_version: "1.0.0"
document_id: "4c1728c9b6c4e7f5b469d3368bdb4857fa18f2cfdf6b090dee9616a0f4754036"
company_key: "silicon-laboratories-inc-common-stock"
company: "Silicon Laboratories Inc."
source_id: "silicon-laboratories-inc-common-stock-news-import-7b63a781b7d1"
canonical_url: "https://www.silabs.com/blog/introducing-silicon-labs-simplicity-sdk-for-zephyr-rtos"
published_at: "2026-06-25T15:20:36.506+00:00"
first_seen_at: "2026-07-26T00:14:15.933714+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:9ccdeab0a1511ed4501ec96ee392e7366d749ff5d1b9dee5b63cd8402368f33d"
---

# Introducing Silicon Labs Simplicity SDK for Zephyr RTOS - Silicon Labs

IoT devices are becoming incredibly complex, driven by rising user expectations, expanding protocol support, and stricter security requirements. As a result, embedded developers need software platforms that provide the flexibility to add features and functionality while maintaining long-term reliability to support long product lifecycles.


Open-source ecosystems offer rapid innovation and broad community engagement, yet production teams still require predictable releases, validated software paths, and responsive support. To meet these needs, Silicon Labs is introducing the **Simplicity SDK for Zephyr** , a downstream, fully supported distribution of the Zephyr RTOS designed specifically for our wireless platforms.


This new SDK gives developers the advantages of Zephyr’s modern RTOS architecture and open-source ecosystem while ensuring the quality, testing, and lifecycle support expected from Silicon Labs. It marks a significant milestone in expanding customer choice without compromising performance, portability, or reliability.


##
Zephyr Adoption and Silicon Labs’ Commitment


Zephyr has become one of the most active open-source real-time operating systems, driven by contributions from more than a thousand developers and governed by the Linux Foundation. The project includes a preemptive kernel, integrated wireless stacks, a modular architecture, and a configurable build system that scales from simple sensor nodes to more advanced IoT gateways.


Silicon Labs has steadily expanded its participation in the Zephyr community since joining the project in 2021. This has included upstream support for multiple development kits, collaboration with longstanding contributors, and hiring experienced Zephyr maintainers, including leadership within the project’s Bluetooth Working Group. In 2025, Silicon Labs advanced to[Platinum membership](https://www.silabs.com/blog/silicon-labs-expands-support-for-zephyr-project) , underscoring our commitment to shaping the future of low-power wireless software in the open-source ecosystem.


This growing investment, along with rising customer interest, created a clear need for a Silicon Labs-maintained and fully qualified Zephyr-based SDK for production applications.


##
What Simplicity SDK for Zephyr Delivers


Simplicity SDK for Zephyr integrates the features, drivers, and wireless stacks required to build commercial grade applications on Silicon Labs hardware. While aligned with upstream Zephyr, several key enhancements differentiate the downstream distribution.


- **Validated wireless stacks and drivers**
Every release undergoes full Silicon Labs QA, ensuring predictable performance and long term maintainability across supported devices.
- **Consistent behavior across hardware**
The SDK provides a unified, verified implementation of peripherals, networking, and platform features across Series 2 devices.
- **Support where it matters most**
The downstream SDK gives product teams a clear, supported path for development, testing, and commercialization without introducing deviation from core Zephyr workflows.


##
Simplicity SDK for Zephyr Platform and Kit Availability


The first release of Simplicity SDK for Zephyr supports a wide range of Series 2 devices, including:


- EFR32xG22
- EFR32xG24
- EFR32xG27
- EFR32xG29


A curated set of development kits and radio boards for these devices will be enabled, ensuring an easy entry point for development and a smooth path to production.


##
Choosing the Right Software Platform


Simplicity SDK and Simplicity SDK for Zephyr each serve important roles in the developer journey. Simplicity SDK offers deep hardware control, robust tooling, and optimized drivers for customers building tightly tuned wireless applications. Simplicity SDK for Zephyr provides a modular RTOS with integrated middleware, strong abstraction layers, and broad portability across vendors. With Simplicity SDK for Zephyr, developers can now adopt Zephyr while maintaining access to validated, production-grade software support.


In terms of performance, memory footprint, and current consumption, Silicon Labs hardware is comparable between both SDKs. Ongoing optimization work continues across multiple EFR32 platforms to ensure consistency in real-world deployments.


##
Building a Stronger Open-Source Wireless Ecosystem


Simplicity SDK for Zephyr reflects Silicon Labs’ balanced approach to open-source. Our aim is to keep upstream and downstream differences minimal while ensuring customers have access to a supported, stable development environment. We will continue contributing upstream, broadening device support, and collaborating with the community to strengthen Zephyr’s role in low-power wireless innovation.


##
Getting Started with Silicon Labs’ Zephyr SDK


Simplicity SDK for Zephyr will be available through standard distribution channels with documentation, migration resources, and onboarding guidance. For teams building on Series 2, the SDK offers a modern, flexible platform for Bluetooth with future support for Wi-Fi and multi-protocol applications backed by Silicon Labs expertise and long-term support.
