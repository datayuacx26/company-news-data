---
schema_version: "1.0.0"
document_id: "f345a57a1ef1b02f24639f339aca2eb448655bd2f05e346b5624dcdd26652066"
company_key: "logitech-international-s-a-ordinary-shares"
company: "Logitech International S.A."
source_id: "logitech-international-s-a-ordinary-shares-rss-9883ecf3a078"
canonical_url: "https://www.logitech.com/blog/2026/02/10/bluetooth-shorter-connection-intervals-paving-the-way-for-bluetooth-innovation/"
published_at: "2026-02-10T16:59:54+00:00"
first_seen_at: "2026-07-20T03:30:07.888502+00:00"
fetched_at: "2026-07-28T22:21:02.085623+00:00"
content_hash: "sha256:0759d6c07cc39e214632520a64d3aca50e224ed46608877f7d83a7b3eb4d2916"
---

# Bluetooth® Shorter Connection Intervals: Paving the Way for Bluetooth Innovation

Last year, the Bluetooth Special Interest Group (SIG) introduced a new feature: Bluetooth® Shorter Connection Intervals (SCI). Fundamentally, Bluetooth SCI reduces the minimum interval for Bluetooth® LE connections down to sub-millisecond levels, allowing the implementation of wireless applications with ultra-low latency and, consequently, with extraordinary responsiveness for users.


This article provides an overview of Bluetooth SCI and the benefits it supports.


### **Bluetooth® SCI**


The Bluetooth® Core Specification is the official technical rulebook for Bluetooth wireless communication and networking. It details the complete protocol stack, down from the physical layer used by the radio to transmit and receive wireless signals, all the way up to the host layers (e.g., GATT) providing the common interface and structure used by application profiles to implement specific functions with Bluetooth technology, such as keyboards, mice, and many others. The Bluetooth Core Specification and application profile specifications are managed by the Bluetooth SIG but developed and maintained by Bluetooth SIG member companies collaborating through working groups.


The Bluetooth SCI feature was conceived in the Bluetooth Core Specification Working Group with the primary goal of enabling the configuration of Bluetooth LE links with connection intervals shorter than 7.5 ms, the previous minimum, using the LE ACL transport. Adding Bluetooth SCI involved various specification changes, including introducing new control PDUs in the Link Layer, new HCI commands and events, new feature exchange mechanisms, and other updates. Importantly, Bluetooth SCI did not alter the radio, which means that it can run on the hardware found in existing Bluetooth devices.


### **How Bluetooth® SCI works**


Bluetooth SCI introduced host and controller support procedures that allow Central and Peripheral devices to negotiate and update the connection timing parameters of an existing ACL link. At a high level, this is how Bluetooth SCI works: First, the devices must connect according to standard Bluetooth LE procedures and read each other’s supported features. If Bluetooth SCI is supported by both, the Central device will choose the configuration parameters for a connection rate change. Then, the Central will initiate the connection rate update procedure, although the Peripheral device can also request that the Central initiate one. The connection rate update procedure will involve updating the following parameters:


- Connection interval: Time between successive connection events for central and peripheral devices to exchange data during an active connection
- Subrate factor: How often a device participates in connection events relative to the regular connection interval
- Peripheral latency: Maximum number of consecutive connection events a peripheral can skip if it has no data to send
- Continuation number: Number of consecutive connection events a device must attend after sending data, even if skippable due to subrating
- LE ACL flush timeout: Timeout that allows preventing outdated traffic from blocking new one; enabled locally in each device and independently from a remote peer


In summary, Bluetooth SCI establishes a new framework for negotiating and applying timing parameters for an LE ACL connection that works with all Bluetooth LE GFSK PHYs.


### **Bluetooth SCI benefits**


Bluetooth SCI represents a significant enhancement to Bluetooth technology and is poised to greatly improve the responsiveness of wireless devices and the experience of their users. It increases the rate at which connected devices can exchange packets. While previous versions of the Bluetooth Core Specification allowed data exchange every 7.5 ms, Bluetooth SCI provides up to 20 times more opportunities for communication in the same amount of time by allowing exchanges in as little as 0.375 µs. It is important to note that Bluetooth SCI does not increase the radio’s speed; it substantially increases usable airtime.


### **Key benefits of Bluetooth SCI include:**


- Reducing latency, leading to more fluid and responsive device interactions
- Maximizing Bluetooth LE performance, especially when paired with short frame space (the time separation between packets), allowing packets to be transmitted in rapid succession
- Increasing throughput, such as during firmware upgrades
- Enabling fast poll rate switching through a dedicated link layer command, allowing devices to efficiently adapt to changes in usage patterns or data loads in real time
- Increasing wireless robustness due to more frequent transmission and retransmission opportunities
- The optional flush functionality for discarding old, irrelevant data, which is particularly important for time-sensitive sensor data
- Optimizing for low latency without necessarily increasing energy consumption


Bluetooth SCI offers powerful new ways to tailor the connection rate and overall performance of a wireless system to the needs of the application.


### **What’s Next?**


Companies like Logitech are actively participating in Bluetooth® technology development, expanding its boundaries through engagement in Bluetooth SIG working groups and by leading or contributing to the definition of groundbreaking features like Bluetooth SCI. As a global player in wireless peripheral markets, Logitech believes Bluetooth SCI is paving the way for years of innovation, offering OEMs capabilities and performance that are necessary for next-generation products designed to deliver faster interactions, higher robustness, and more adaptable performance.


In future articles, we will explore how Bluetooth SCI will become the foundation for a new Bluetooth feature that will standardize advanced human interface device (HID) applications with ultra-low latency, unlocking new levels of responsiveness for mice, keyboards, gaming controllers, and other latency‑sensitive use cases.


*Note: This piece was also published on[bluetooth.com](https://www.bluetooth.com/blog/bluetooth-shorter-connection-intervals-paving-the-way-for-bluetooth-innovation/)*
