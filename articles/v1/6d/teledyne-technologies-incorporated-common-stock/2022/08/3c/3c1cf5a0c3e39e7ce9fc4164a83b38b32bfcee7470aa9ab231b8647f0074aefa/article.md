---
schema_version: "1.0.0"
document_id: "3c1cf5a0c3e39e7ce9fc4164a83b38b32bfcee7470aa9ab231b8647f0074aefa"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2022/08/physical-layer-collision-avoidance-in.html"
published_at: "2022-08-22T12:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:fa48aeb1496ba654fd29e1ec9b2b8901d30dcb35bd3eae1b63bcdcfe34879f1a"
---

# Physical-Layer Collision Avoidance in 10Base-T1S Automotive Ethernet

Fig.1, 10Base-T1S PLCA cycle. If there is no data
traffic (top), only BEACONs are seen on the bus.
Data from a node (bottom) will expand the time
between two BEACONs.


10Base-T1S (IEEE 802.cg) is a variant of[Automotive Ethernet](https://blog.teledynelecroy.com/2021/06/automotive-ethernet-in-vehicle.html) that supports half-duplex and full-duplex communication, allowing either a point-to-point direct connection between two nodes, or use of a multidrop topology with up-to-eight nodes connected on a single 25 m bus segment.


Multidrop cabling of one bus line provides options to extend and scale with fewer physical wires and less weight than point-to-point topologies . With minimum connector space at the ECU, the bus line can be expanded simply by adding sensor units. A bus line with additional sensor units for ultrasonic and short-range radar is an example of how multidrop cabling can be scaled.


Among the main objectives


of the 10Base-T1S PHY layer are reconciliation of transmissions from a variety of mediums, ensuring cooperative behavior by the nodes on a multidrop bus. One way it does this is through the use of Physical-Layer Collision Avoidance (PLCA) technology to minimize dead time and avoid collisions. In this post, we'll describe the workings of PLCA and in a future post, how you can debug PLCA timing issues using an[oscilloscope](https://teledynelecroy.com/oscilloscope/) with the[10Base-T1S TDME](https://teledynelecroy.com/options/productseries.aspx?mseries=681&groupid=88) software.


Essentially, PLCA establishes a transmission cycle used to choreograph Transmit Opportunities (TOs) on the bus. As with a group of individuals participating in a team-building exercise, if all nodes were chaotically speaking their minds at once, nothing would be heard properly and nothing would get accomplished in the time allotted. A PLCA transmit cycle establishes the opportunities to speak and the order in which nodes can be heard, while leaving enough flexibility that time is not wasted waiting for those who have nothing to say.


In PLCA, each node (aka PHY) is assigned with a unique PHY ID, and only the PHY device that owns the transmit opportunity is allowed to send data. The transmit opportunities are allocated in a round-robin algorithm starting from PHY ID = 0, which is allocated to the Master. Nodes can initiate a transmission only during the transmit opportunity that matches their own node ID. A new cycle is started when the Master node sends a synchronization pattern called the BEACON to signal the start of the PLCA cycle.


The PLCA cycle itself consists of the BEACON followed by N+1 time slots, allowing N+1 variable size DATA packets to be sent. During their transmit opportunity, a PHY may immediately transmit a packet or must transmit a COMMIT pattern of SYNC symbols to compensate for any MAC latency and to buy additional time before transmitting a packet. Nodes can enlarge the time slot to accommodate larger transmissions and can burst high priority messages. The other nodes will wait for a node to complete transmission before the cycle moves to the node with the next transmit opportunity. A new time slot starts if nothing is transmitted within a defined time (TO_TIMER) or at the end of any packet transmission.


At the beginning of each transmission cycle, Node 1 on the bus is first assigned the transmit opportunity. If there is no DATA for this node to transmit and it cannot COMMIT, it cedes its transmit opportunity to the next node on the bus.


In attempting to understand the PLCA cycle, it may help to visualize the use of a variable delay line to relate transmit opportunities to each node on the bus. The driving scheme of PLCA is to sync TO_TIMERs so that the max latency consistently remains less than one PLCA cycle. TO_TIMER is very short (typically 20 bits), so there is a negligible loss of throughput when waiting for PHYs that have nothing to transmit.


Fig. 2, Minimum latency PLCA cycles. No one has anything to transmit in cycle 1, so total latency is equal only to the number of nodes times the TO_TIMER. Only Nodes 1 and 3 have transmissions in cycle 2, so all the other nodes cede their transmit opportunity.


Fig. 3, Maximum latency PLCA cycle. Every node has the maximum size packet and sends a COMMIT
while waiting for the MAC.


The benefit of this system is that the individual nodes track TO_TIMER independently following the BEACON. Because nodes with no data to transmit will yield their transmit opportunity, the short window afforded by the TO_TIMER ensures a minimal loss of throughput or increase of latency. This variable delay is similar to TDMA, but PLCA is not a fixed or absolute reference for timed packets; instead, it adjusts according to the transmit needs of each node on the bus.


#### Also see:


[The Difference Between 10Base-T1S and 10Base-T1L](https://blog.teledynelecroy.com/2022/08/the-difference-between-10base-t1s.html)


[Automotive Ethernet in the Vehicle](https://blog.teledynelecroy.com/2021/06/automotive-ethernet-in-vehicle.html)
