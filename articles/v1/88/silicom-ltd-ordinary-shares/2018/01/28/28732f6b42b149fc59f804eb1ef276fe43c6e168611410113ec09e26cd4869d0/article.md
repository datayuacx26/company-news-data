---
schema_version: "1.0.0"
document_id: "28732f6b42b149fc59f804eb1ef276fe43c6e168611410113ec09e26cd4869d0"
company_key: "silicom-ltd-ordinary-shares"
company: "Silicom Ltd"
source_id: "silicom-ltd-ordinary-shares-rss-23a299f84053"
canonical_url: "https://www.silicom-usa.com/pre-integrated-fpga-engines-silicom/"
published_at: "2018-01-31T13:59:58+00:00"
first_seen_at: "2026-07-20T23:19:15.462150+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:d98336e2e333c65ee61c78f07ec6d251d6471f736821e7a1d5698d180b01166a"
---

# Pre-Integrated FPGA Engines from Silicom

# Pre-Integrated FPGA Engines from Silicom


January 31, 2018


# Pre-Integrated FPGA Engines from Silicom


### Brief


In today’s smartphone and IoT reality, networks must be able to handle almost unimaginable volumes of data – and this is creating a whole new level of performance challenges. As a result, a growing number organizations now recognize the **need to add acceleration technology to the standard processor cores in their next generation designs.**


While Graphical Processors (GPU) are currently the most widely used acceleration technology, interest in FPGA technology is rising due to its potential to deliver superior performance for process-intensive tasks. Although FPGA has existed as a niche technology for many years, its uptake has been limited due to its reputation as a difficult – almost alien – technology. Over the last few years, however, FPGA technology has been evolving very rapidly, resulting not only in chips that with more capacity, speed and versatility, but also in tools that make the programming of FPGA devices more accessible. These improvements, together with the impressive performance gains that have been demonstrated through the use of FPGA by frontrunners of Network Security, Virtualization and other market segments, are strengthening the positioning of FPGA technology as a highly competitive acceleration contender.


**The implications are significant: companies who have adopted the notion of hardware abstraction as their primary acceleration strategy now must rethink.** This is indeed a daunting task! On the one hand, it is not a trivial task to transform a software engineer into an FPGA engineer. Yes, tools are now available – such as OpenCL- that promise an environment that appears software-oriented. And yes, cases in which these tools have proven to be efficient do exist – especially for designs that use FPGA specifically for blocks of number crunching tasks. But for the most part, the efficient creation of FPGA designs still requires experienced Verilog/VHDL engineers with a deep understanding of FPGA hardware structures.


Recognizing the gap between the growing market need for FPGA-based acceleration solutions and the availability of engineers that can make FPGA work, Silicom has developed a new, more user-friendly concept: Pre-Integrated FPGA IP Modules. The offering is a platform in which hardware, FPGA firmware and associated software drivers are combined to give developers a much more efficient and user-friendly way to incorporate FPGA-based acceleration technologies into their next generation networking solutions.


## Silicom’s Generic Offload Engine


The starting point of Silicom’s Generic Network Traffic Offload Engine is the transfer of the job/packet over PCIe to an offload network interface card (NIC) through a DMA transfer. The offload engine is initially set up through register writes.


The offload job is performed by the host, which creates a list of jobs in memory. The address of the list is then transferred to the offload card through a doorbell register write. Once the card has the list of jobs, it begins fetching data for all the jobs on the list. Once all the jobs on the list have been fetched, the first 8 bytes of the list are set to zero, indicating that the memory used for the list and the memory linked to on the list can be freed.


The job data must be placed in DMA-able memory for the offload card to fetch it. This can be achieved in three ways:


1. Copy the job data to a memory area specially allocated to that purpose. This is not good for performance.
2. Dynamically map the job-data into the offload cards MMU.
3. Allocate a large DMA-able memory area, map it into the offload cards MMU, and use it for network traffic input so the data is already placed correctly.


If the job has a result to return, it must have a header specifying which location the result of the job should be returned to.


## Detailed FPGA System Overview


The design of Silicom’s Generic Platform lends itself to fast integration with dedicated modules for functions such as en/decryption, compression and pattern search. Two such examples are described below.


## Pattern Search in FPGA


Our Pattern Search example is based on a Titan IC RegEx engine, which is initially set up through register writes. The search engine can have 4 different search tables, and the job can be directed to any of them. Each job must have a 32-bit (type 1 or type 2) header specifying which memory location the result of the job should be returned to. Once a search is performed, the result is written in host memory according to the format specified below. It includes how many matches occurred, the version of the rule set, where the first match occurred, and the ID of the first match.


**Result in memory:**


**Size\[bits\]** **Name** **Description**


32 JOB_ID Firmware generated job_id. 0-511


32 MATCH_RULE_ID ID of rule that matched. Only first match


16 MATCH_COUNT Number of matches in search


16 MATCH_START_PTR Byte position in job where match started. Only first match


16 LATENCY_COUNT How long it took for RXP to scan job in microseconds


16 MATCH_ROF_REV Revision of the ruleset. Only the lower bits of the revision


Since the RegEx engine returns the results in random order, two different return modes have been implemented for two different use cases:


1. **Strict ordering** – results are returned in the exact order in which the jobs were generated


2. **Relaxed ordering** – results are returned in the same order that they exit the RegEx engine


Strict ordering has the benefit that it can utilize a sequential memory write, which has better performance. The downside considerations of a strict ordering scenario is that 1) substantial FPGA resources are required to reorder the results, and 2) it may cause searches with many matches to stall new requests.


Relaxed ordering has the benefit of the least possible latency from job start to job end with less resource usage in the FPGA, but with some potential penalties in terms of application software performance.


## IPSec Acceleration


If you are looking to accelerate IPSec alone, using FPGA is probably not a price-competitive choice since dedicated ASICs exist that can probably do the job at a lower cost. If, however, you envision accelerating several functions outside of your processor cores, then you should probably consider the use of FPGA for carrying out at least IPSec’s computational-intensive components in your offload design.


Silicom has created a pre-Integration of a certain sub-part of IPSec to demonstrate the capabilities of its platform for use in the above-described RegEx machine in combination with IPSec. The usage of this combination can be a vital component in many types of Security Applications.


## **Implementation overview**


****


**Performance brief**
IP Encapsulating Security Payload – rfc4303 10K-100K Tunnels 40Gbps in both directions. Supports multiple network interfaces. 10K new keys per sec.


**Algorithm selection**
The specific AES scheme could be AES-GCM (which includes authentication) or AES-CBC mode (using HMAC-SHA1-96 as authentication). However, AES-GCM should probably be preferred as it is listed as a SHOULD+ on the list of algorithms supporting in ESP according to[https://tools.ietf.org/html/rfc7321](https://tools.ietf.org/html/rfc7321)


**Key exchange**
The key exchange must be carried out in software due to the complexity of the task. The keys are then pushed to the FPGA from host through a DMA write.


**Key handling**
For lookup of IPsec tunnels in the ingress direction, we must follow the protocol specified in[https://www.ietf.org/rfc/rfc4303.txt](https://www.ietf.org/rfc/rfc4303.txt)


, page 9, regarding IP source, IP destination and SPI (security parameter index) if we choose to support multicast tunnels.


A unicast tunnel need only store the SPI and the crypto key. A multicast tunnel may need to store both IP source and destination addresses.


To be able to have multiple different configurations in a storage-optimized way, we can use a two table solution. The first table is named the Ingress Hash Table(IHT), which contains pointers to the secondary table named Ingress Key Table(IKT). In the IHT, all entries have a small fixed size which allows them to be implemented in BRAM inside the FPGA. The entries in the IKT will have a variable size in order to save space when using unicast sessions, since they require less space than IPv6 multicast tunnels with long keys.


The IHT hash table should use cuckoo hashing for access. Cuckoo hashing has the benefit that it has a constant search time and deletion time. The insertion can, however, take unlimited time, when moving entries around in the table. The algorithm also has the benefit that Silicom Denmark has successfully used it previously in the GTP solution in fbCapture. Due to the nature of cuckoo hashing, in which insertions may cause entries to move around (cuckooing), the table must be maintained in both host memory and on the board.


Cuckoo hashing can be accomplished by hashing the SPI into a 32-bit value, using a CRC32 function. The CRC32 has the benefit that it is very fast in hardware, and has satisfactory distribution.


To support 100K tunnels with 64-bits per entry, the IHT table would need to be 100K * 64-bit = 6.4Mbit. Rounding up the table size to a number that is a power of 2 yields 8.4Mbit. This means that the IHT table will have a load factor of 6.4Mbit/8.4Mbit = 76%, which should be feasible.


**Ingress Hash Table(IHT) Entry**
Each entry in the Ingress hash table(IHT) will contain:
8-bit : Status.
4-bit : IKT entry length in number 64-bit words.
20-bit : Pointer to ingress key table(IKT). Points to a 64-bit word.
32-bit : Security Parameter Index(SPI).


**IHT status bits decoded:**
Bit 0 : Entry valid when 1. Otherwise the entry is empty
Bit 1 : IP dst valid
Bit 2 : IP src valid
Bit 7..3 : Reserved


The IP dst/src valid fields are needed in a multicast scenario where multiple tunnels can use the same SPI. In unicast the receiver selects the SPI and collisions can avoided. In multicast the transmitter selects the SPI and SPI collisions can happen. The IP dst/src must therefore be validated.


**Ingress Key Table(IKT)**
All entries in the IKT has the same first 64-bit fields as seen below.
8-bit : Type of table entry.
8-bit : Algorithm selector.
8-bit : Reserved. 8-bit : RX DMA channel.
32-bit : SPI. Mainly for integrity check between IHT and IKT.
x-bit : Crypto key.
x-bit : IP addresses.


The size of the Crypto key and IP address fields depend on the Type field. See table below.


**Type** **Key size \[bits\]** **IP address**


0x00 0 None


0x01 128 None


0x02 192 None


0x03 256 None


0x04 384 None


0x05 512 None


0x06-3f Reserved Reserved


0x40 None IPv4 dst+src 2×32-bit


0x41 128 IPv4 dst+src 2×32-bit


0x42 192 IPv4 dst+src 2×32-bit


0x43 256 IPv4 dst+src 2×32-bit


0x44 384 IPv4 dst+src 2×32-bit


0x45 512 IPv4 dst+src 2×32-bit


0x46-7f Reserved Reserved


0x80 None IPv6 dst+src 2×128-bit


0x81 128 IPv6 dst+src 2×128-bit


0x82 192 IPv6 dst+src 2×128-bit


0x83 256 IPv6 dst+src 2×128-bit


0x84 384 IPv6 dst+src 2×128-bit


0x85 512 IPv6 dst+src 2×128-bit


## Ingress block diagram


##


## [1/10/40/100G FPGA SERVER ADAPTERS](https://www.silicom-usa.com/cats/fiberblaze-a-silicom-company/fpga-cards)
