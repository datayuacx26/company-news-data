---
schema_version: "1.0.0"
document_id: "79a79a0a8da1b48299b84cde8ebb8a9c6185752f41e58a8a1512f999463879aa"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/introducing-POSSUM"
published_at: "2023-09-27T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:a5e175b392c3c1ece5eb9aecdab8f8ed3baba76be92831816fd122096d202934"
---

# Introducing POSSUM, An Open Source Retail Peripheral Platform for All

One of the greatest challenges for large retailers like Target is managing the configurations and lifecycles of retail peripherals such as scanners, scales, receipt printers, displays, self-checkout cash and coin, and more. At the scale of thousands of stores, tens of thousands of point of sale terminals, and hundreds of thousands of peripherals, managing it all in an operationally efficient and stable way, all while allowing flexibility, is not a challenge that can be taken lightly. This is where POSSUM comes in – our newest open source product, available at


[target/POSSUM](https://github.com/target/POSSUM) , which


provides


hardware abstraction for industry standard retail peripherals.


A Quick Look Back


To understand our present and future direction, one must take a quick look back. Five years ago, Target was reliant on a monolithic application stack, running on third-party hardware, and managing that stack with closed-source solutions selected from the menu of traditional enterprise solutions and patterns.


Our leaders made a commitment to moving our enterprise from our legacy application architecture to modernized enterprise checkout microservices based on open source solutions. But we did not stop there! We furthered the adoption of open source solutions in every facet of our physical point-of-sale endpoints. We moved towards a more commoditized, open architecture hardware platform. This catalyzed a large number of new offerings from our hardware vendors, enabling us to make strategic decisions and move quickly into a modernized point of sale ecosystem.


As part of this modernization journey, many engineers within Target tech contributed to an innersource effort to develop a collection of standardized APIs to allow our new Electron & Node.js point of sale applications to interact with hardware in an abstracted way, without needing to understand low level hardware capabilities across various manufacturers’ models that are ever evolving due to hardware lifecycles and progressions. The idea that would become POSSUM was born.


What is POSSUM?


POSSUM is a Spring framework Java Application, which consumes industry standard JavaPOS service objects, and exposes the hardware capabilities of retail peripherals via a standard Web API. These JavaPOS service objects are ideally provided by retail peripheral vendors as discreet stand-alone packages available for consumption by their customers. For those unfamiliar with retail peripherals, we can think of JavaPOS service objects as a device driver/library equivalent. For those familiar with retail peripherals today, you will find an OPOS service object analog in many of your existing peripheral providers today with many vendors providing JavaPOS service objects alongside their existing device drivers.


There are many hardware vendors who are already conforming to this standard and publishing JavaPOS service objects, and many others are on the journey to get there. We have included simulator capabilities within the project to provide a springboard into testing and learning more about POSSUM and its core capabilities.


Our


vision


is to mature the retail peripheral ecosystem and continue to drive the adoption of devices that conform to JavaPOS standards and best practices. POSSUM is an open architecture solution that achieves this objective. POSSUM will foster flexibility and competition in the retail peripheral space by allowing retailers to introduce peripherals of similar function from different vendors without being disruptive to the UI/UX of any given point of sale ecosystem.


Our


philosophy


for POSSUM is to partner with other retailers to maintain a portable, consistent ecosystem where JavaPOS service objects can be consumed to rapidly deploy the functionality of similar capabilities from disparate manufacturers in an abstract way that is easily consumable by software developers. To that end, you will not find hardware manufacturer-specific code embedded within POSSUM. As we evaluate new hardware offerings, conformity to the JavaPOS spec is a key consideration moving forward.


Our


commitment


to the


open source


community is to give back. Target has benefited from the open source community in many different aspects of our point-of-sale ecosystem. On our journey, we wanted to create something that we believe can be utilized by other retailers in their own open source journeys.


Our


invitation


is to other retailers and vendor partners who are interested in furthering the adoption of JavaPOS as a standard to drive innovation in their ecosystems and product offerings. We invite you to explore


our repo in GitHub at


[https://github.com/target/POSSUM](https://github.com/target/POSSUM) contribute to it, consume it, adopt it, and become part of our POSSUM community as we continue to look for opportunities to make it easier to manage retail peripherals at scale.


## RELATED POSTS


### Target's EasySweep – Simplifying Skimmer Detection for All


By Terry Woodman, July 18, 2023


Target's patented EasySweep device protects guests' data from skimmers.


### Meet Merry Maker: How Target Protects Against Digital Skimming


By Chris Carlson, February 1, 2022


Credit card skimmers are custom-made additions to payment devices (like ATMs or gas pumps) that criminals build and use to steal a consumer’s payment information.
