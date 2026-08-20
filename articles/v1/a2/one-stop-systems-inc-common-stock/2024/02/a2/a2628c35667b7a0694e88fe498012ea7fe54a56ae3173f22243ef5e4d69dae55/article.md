---
schema_version: "1.0.0"
document_id: "a2628c35667b7a0694e88fe498012ea7fe54a56ae3173f22243ef5e4d69dae55"
company_key: "one-stop-systems-inc-common-stock"
company: "One Stop Systems Inc."
source_id: "one-stop-systems-inc-common-stock-atom-3c5790b2a18f"
canonical_url: "https://onestopsystems.com/blogs/one-stop-systems-blog/redundancy-and-management-of-rugged-edge-servers"
published_at: "2024-02-13T16:52:26+00:00"
first_seen_at: "2026-07-20T23:19:18.941877+00:00"
fetched_at: "2026-08-20T01:38:38.553176+00:00"
content_hash: "sha256:76ac36651ce09aa67ab09840264b4291c8eb0dad153b58dda62edfcefc5417b7"
---

# Redundancy and Management of Rugged Edge Servers

**By Jim Ison, Chief Product Officer**


Computer server redundancy, including backup power supplies, RAID storage devices and applications that automatically fail-over, keeps critical systems up and running longer than non-redundant systems. Similarly, effective system monitoring can provide early warning of failures and allow system managers to remotely manage these systems, further improving application uptime. While the concepts of computer system redundancy and system management are well-established in all levels of computing, from the personal computer to the largest hyperscale datacenters, the unique challenges of placing datacenter-class computing elements performing AI applications in mobile edge environments, like aircraft, ships, and land vehicles, brings unique challenges to system redundancy and management.


A datacenter provides reliable conditioned air, abundant stable power, and a benign environment for computer servers where redundancy is achieved by backup power supplies, extra storage drives and multiple data paths in case of network or cable failures. System monitoring in the datacenter is used to alert maintenance personnel employed onsite to replace failed parts quickly to keep systems running at peak performance. Conversely, a similar performance server in a rugged edge environment, such as an aircraft fuselage pod hanging under an unmanned aerial vehicle (UAV), is subjected to extreme temperatures, power fluctuations, shock and vibration while also being expected to accomplish the mission while maintenance personnel are hundreds of miles away. Redundancy and system management must be flexible to the edge environment and application.


Redundancy in server-class AI systems for rugged edge applications ensures objectives are met, even in the event of a failure. The system must be able to meet these objectives with fluctuating “dirty” power, a damaged power rail or a failed power supply. Both robust power sub-systems and tightly coupled management, monitor and control systems provided by a baseboard management controller (BMC) can work in concert to provide continuous operation in edge systems, even if there is not 100% power supply redundancy (referred to as 2N) due to size, weight, or power constraints. An example of an industry leading multi-GPU server used in a UAV pod for autonomous operation in GPS and communication-denied areas may look like this:


- 1000W of available compute power from a UAV engine fed to two 500W power supplies in the server
- A fully loaded, 100% utilized AI system, involving two ruggedized enterprise GPUs at 100% clock rate drawing 600W
- One power supply fails, which is detected by the BMC in real time, leaving 500W total available power
- Both GPUs lower their clock rate via out-of-band commands from the BMC to 75% of maximum now drawing 450W
- The BMC alarms and notifies the user that a power supply failure has occurred
- Both GPUs continue to run in this reduced state for the time it takes to finish the mission
- Upon return to the airfield, the failed supply is replaced, and full power is restored
- The BMC stops alarming and resets the GPU clock rate to 100%
- There was never any downtime
- 90% of AI applications do not scale linearly with GPU clock rate, so 75% reduction may only cause 5-15% reduction in AI inference performance for the duration of the mission.


This is an example of how redundancy is achieved without needing to have 2N the available resources. This simplified illustration referred to power supplies and assumes the GPUs made up 100% of the power draw. This same concept is used in SSD storage using generally accepted RAID 5 redundancy practices as a balance between redundancy and cost. For an application needing 150TB of data storage, it may necessitate the use of five 30TB SSDs. RAID 5 allows for redundancy by adding only a single additional 30TB SSD to be able to retain functionality and recover the data using parity calculations instead of requiring an entire duplicate set of 150TB SSDs.


In the example above, the edge server benefitted from a BMC that was focused on edge use cases instead of the benign datacenter. OSS takes the approach of utilizing commercially available off-the-shelf (COTS) datacenter-class components where it makes sense, while enhancing those COTS components for use in edge environments. The OSS unified baseboard management controller (U-BMC) is a prime example of enhancements required to take advantage of many OSS technologies adapted to large scale AI applications at the edge. The U-BMC is a whole system management controller that aggregates the BMC of the base motherboard, as well as additional shelf management controllers and system resources not managed by the motherboard BMC.


First, OSS provides PCI Express expansion products to handle multiple AI inference applications or large-scale AI inference and re-training applications such as large language models, sensor fusion, object recognition, threat detection and autonomous navigation in GPS or communications denied areas. To field these applications, OSS customers use products like the OSS 4U Pro and express box line of PCIe expansion systems which are not servers or workstations, but instead, they provide scale-out expansion of high-performance GPUs, FPGAs, NVMe drives and edge I/O devices to an AI server. To provide the widest compatibility for the expansion systems, the U-BMC allows these expansion systems to seamlessly integrate with existing servers or OSS short depth rugged server (SDS) products. This combination of server and expansion allows the server and one or more of our expansion products to be managed, monitored and controlled like a single integrated system with a massive amount of PCIe resources interconnected by the highest bandwidth, lowest latency PCIe switched fabric to move data in real time among AI resources.


Second, unlike the standard datacenter server, OSS servers are designed to operate in both government and commercial harsh edge environments where “dirty” power from generators, engines and batteries with large spikes are common. Mix in the environmental conditions in which OSS servers may operate autonomously, such as temperature variances in places from Death Valley to 50,000 feet altitude, with moisture ranging from salt fog to rain and extreme vibration and shock from washboard dirt roadways to propeller aircraft and one can see that a higher level of management, monitoring, and control than a server snug in a power and cooling conditioned datacenter is required.


The U-BMC adds unique value to server-level systems in these environments, especially in autonomous operations where there are no service technicians for miles, by allowing the system to adapt to changing conditions automatically or by remote control without failing. The U-BMC has features to handle these environments such as controlling sensors connected to the server, turning on heaters in extreme cold, changing the PCIe fabric to reroute data around failed components and connecting to the Controller Area Network (CAN) bus in cars and autonomous trucks to monitor the vehicle conditions to be able to take action on information provided by the vehicle, such as ignition on/off. Additional U-BMC edge features, unique to OSS and valuable for customers deploying edge AI, were developed in 2023. More features will continue to be added in 2024, such as real time dynamic clock control of enterprise AI GPUs based on power or temperature fluctuations to keep systems running in a reduced state rather than shutting down, in the example above. Since every vehicle deployment of OSS customers is unique, the mainstream datacenter COTS BMC cannot be relied on to handle the demands of the rugged edge.


Third, due to operating in diverse industry standard and regulatory markets required by military standards, commercial aerospace FAA or EASA, and highway NHTSA or ETSC agencies, the U-BMC is designed to adapt to the unique requirements imposed by servers residing on, or controlling, vehicles. A standard datacenter server BMC needs to conform to the basic agency requirements of electrical interference and personal safety regulated by agencies such as the FCC or CE and administered by testing companies such as UL and TüV. The U-BMC is designed to cover all edge requirements of the datacenter as well as adding standards organization compliance such as those required by military customers including SOSA compliant functions for sensor management, system management, and task management not found in datacenter server BMCs.


In 2022, OSS introduced the U-BMC included in the flagship Rigel edge supercomputer and PCIe Gen 5 4U Pro products. In 2023, OSS expanded the platforms that include the U-BMC to the PCIe Gen5 SDS rugged server for commercial and military edge applications with more platforms to come. U-BMC enables OSS to offer a 'single pane of glass' management for complex systems, even when the server is in a separate enclosure from GPUs, FPGAs, and NVMe storage. It also includes an open-source Redfish API for easy integration with industry-standard BMCs and advanced management tools.


OSS is focused on applying COTS technologies to rugged environments to allow our customers to expect the same performance in mobile environments that they can get in datacenters today. Through practical large-scale AI inferencing and re-training systems and a holistic management design, OSS provides powerful systems that include redundancy, management and control of edge systems that give our customers configuration control.


*Click the buttons below to share this blog post!*


[Return to the main Blog page](https://onestopsystems.com/pages/one-stop-systems-blog)
