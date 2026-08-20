---
schema_version: "1.0.0"
document_id: "fc1650f9d83dcd9070be344ec1f6019f0de5937569d1d9a3ca411979a89ad441"
company_key: "silicom-ltd-ordinary-shares"
company: "Silicom Ltd"
source_id: "silicom-ltd-ordinary-shares-rss-23a299f84053"
canonical_url: "https://www.silicom-usa.com/spdk-1-1-memory-management/"
published_at: "2016-07-12T10:54:29+00:00"
first_seen_at: "2026-07-20T23:19:15.462150+00:00"
fetched_at: "2026-07-28T22:27:31.756931+00:00"
content_hash: "sha256:3b9528c17649ea0f3c388a73f0aee1acca94ee92b239dccff389b411e2a9b931"
---

# SPDK 1.1 Memory Management

# SPDK 1.1 Memory Management


July 12, 2016


# SPDK 1.1 Memory Management


**General**
Intel® DPDK kernel bypass and line rate packet processing framework has its many advantages. On the same time, however, network application designers and programmers are facing a challenge of adapting to a new paradigm and infrastructure.


In order to help developer keeping their focus on their business logic and, while maximizing performance without compromising on stability, Silicom came with Smartsilc Performance Development Kit (SPDK), essentially a set of libraries above DPDK, to support exactly that.


One of the important features of SPDK is the memory management layer. This layer streamlines packet processing, and offers better overall stability.


**Advantages**
SPDK 1.1 software package carries along the following clear advantages regarding memory management \[1\]:


Block level (Arrays of packets) work rather than packet level
Certain types of packet processing may benefit immensely by the mere fact that the ingress packets are grouped and brought for processing as block of packets. Tight loop of packet processing logic can easily be implemented at the application level, in order to process blocks of packet at a time, thus achieving better overall performance compared to a per-packet scheme. Configurable packet block size assist with performance fine tune.


**Inter thread steering**
When managing packet in block rings, as indeed done by SPDK, packet can be easily steered across processing entities (threads). Zero copy typed packet blocks makes it even faster and simpler.


\[1\] SPDK-UG-1.1.7 user’s guide
