---
schema_version: "1.0.0"
document_id: "44b8182ece099d1dae0d4a850008b3fb78da041f8a07d7faf138da56a20ae0c0"
company_key: "silicom-ltd-ordinary-shares"
company: "Silicom Ltd"
source_id: "silicom-ltd-ordinary-shares-rss-23a299f84053"
canonical_url: "https://www.silicom-usa.com/fm10000-red-rock-canyon-100gbps-nic-and-redirector/"
published_at: "2016-07-20T07:42:07+00:00"
first_seen_at: "2026-07-20T23:19:15.462150+00:00"
fetched_at: "2026-07-28T22:27:31.756931+00:00"
content_hash: "sha256:d666f3dca8d1a8553cd7d8f5d9575f0f02aa0581cd64e78d2c42b1b03f57cd9a"
---

# FM10000 Red Rock Canyon 100Gbps NIC and Redirector

# FM10000 Red Rock Canyon 100Gbps NIC and Redirector


July 20, 2016


## Extended Benefits – Field Experience


July 2016


**Preface**
Intel® FM10000 based network interface card by Silicom, that is equipped with an advanced switching and forwarding engine embedded therein, was welcome with great enthusiasm by engineering teams across Silicom’s customers. Apart from being a NIC, this design addressed several other use cases and customers’ requirements in a manner which is unique to the FM10000 chipset. Following are several examples of such use cases, with extended benefits that stems out of the use of FM10000.


**High End Customer SLA**
Although two QSFP interfaces of 40G or QSFP28 of 100G links are available, it is often the case that only one of them is actually put in use. This fact has got the attention of a network architect of one of Silicom’s customers, who had his own share of field engineering and field trouble shooting; therefore it had occurred to him that, especially with high end boxes, one of the 40GbE or 100GbE links of the FM10000 based adapter, can be used to mirror out inbound traffic coming from the other link, where the latter is actually used as the uplink.


That way, network trouble shooting is made easy and quick and much more efficient. Not only that traffic can be mirrored, it can also be filtered or sampled and then mirrored. At times of crises with high end boxes out there in the field, this capability can of a lot of use.


**Uplink Load Balancing**


Another instance with different customer has shown how the filtering and forwarding engine within Intel® FM10000 can be exploited to leverage the adapter and make it a L4 load balancer, where part of the ingress.


**Figure1 – Basic load balancing with FM10000**


## Enhances Intel® FM10000 Use Cases


uplink traffic is consumed and processed by the appliance that holds the adapter, while other portion is forwarded onwards for second device to be processed.


Intel® FM10000 Red Rock Canyon chip set offer a good deal of load balancing options for incoming traffic. A cluster of ports can be grouped together to form load balancing group (LBG), over which a traffic can be engineered to be load balanced according to several simple yet powerful schemes.


**L3 hash** **L3 / L4 hash**


Source MAC address


Source IP address


Dest. MAC address Dest. IP address


Ethertype


Source port


VLAN ID Dest. port


VLAN Priority


DSCP


Symmetric MAC ISL


Symmetric IP/port


ECMP


IP protocol


**Table 1 – Exapmples for load balancing parameters**


Load balancing ingress traffic can serve well for packet processing within the system. Ingress traffic become more predictable, and processing threads can be spawned as per the expected streams of traffic. Table 1 exemplify the parameters that are hashed to become the basis for traffic distribution across ports.


**Summary**
Whatever is achieved with top of the rack gradw switching fabric, can be achieved on adapter with Intel® FM10000, side by side with its NIC functionality:


• Mirroring
• QoS
• Filtering
• Load balancing
• Packet steering
• et al.
all are available now on NIC level.


**This list extends to a list of capabilities of the native MAC and PCIe host interface, and flexibility:**
• SR-IOV
• Break out options
o 100GbE to ten 10GbE
o 400GbE to four 10GbE
o 100GbE to four 25GbE
• etc.


All these abilities are there thanks to the integration of 32K deep TCAM, coupled with FlexPipe™ engine, npth forming a powerful, yet easy to use data path with set of capabilities that are unique in the NIC landscape.
