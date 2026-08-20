---
schema_version: "1.0.0"
document_id: "5e91aff51ec5007bf2b6f7926547ee953cd49e4d15f5a5dc7361f3d01797246e"
company_key: "one-stop-systems-inc-common-stock"
company: "One Stop Systems Inc."
source_id: "one-stop-systems-inc-common-stock-atom-3c5790b2a18f"
canonical_url: "https://onestopsystems.com/blogs/one-stop-systems-blog/when-vpx-can-t-keep-up-with-the-threat"
published_at: "2023-11-14T15:00:03+00:00"
first_seen_at: "2026-07-20T23:19:18.941877+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:4bfff020310a26e358dcd3f58c4a66806763b4e3dc4b4480bbb3405f2ebba5fe"
---

# When VPX Can’t Keep up with the Threat

**By Tom Fries, Government/Defense Sales Manager**


Generative, Inferencing and Natural Language Processing (NLP) AI applications have all seen explosive growth (NVIDIA market cap growth 3200%) and rapid proliferation in the last two years across commercial market segments such as Automotive, Aerospace, Manufacturing, Transportation, Banking, and Entertainment. The commercial consumption of AI has quite directly driven the need for High Performance Compute (HPC) solutions, and, specifically HPC solutions that operate in some of the most extreme environments on earth. By comparison, DOD compute systems for air/ground platforms, while significantly ruggedized for austere environments, lack the compute power to employ such technological advancements in AI…or worse, lack the capability to counter AI threats employed by adversaries using commercially available SW/HW.


Currently, the DOD, with each branch having its own AI initiatives, is focusing on requirements for a common hardware interface solution (e.g. VPX, CMFF, OMS) providing a mission module approach for integrating or updating C5ISR capabilities on Air/Ground platforms. The common interface framework, while technically feasible for integrating multiple single use “black boxes” into a single chassis with a common interface, is unable to support the nanoseconds of compute speed and petaflops of data transfer needed to run AI applications. Couple the lack of processing speed/power with the current operational threat’s capabilities to extend the distance of the kill chain, and you can see the dangerous technological gap that exists in most of the DOD’s Air, Ground, Surface and Subsurface platforms.


**The Power of GPUs in AI Computing**


The transition from CPU to GPU core computing in HPC and AI/ML applications is the fundamental driving force behind today’s most cutting-edge applications. “AI Transportable” systems achieve extraordinary compute performance by use of multiple GPUs and new generation switched fabrics, such as PCIe Gen5 and NVIDIA’s NVLink. Today, these elements do not map well into the OpenVPX standard. To preserve the significant government investment in OpenVPX products, a hybrid solution is needed to gain the performance benefits of GPUs (e.g. NVIDIA H100) to realize the operational capabilities needed to employ and counter AI technologies.


At the heart of an AI Transportable system is a PCIe Gen 5 switched fabric that provides the capacity to support an NVIDIA HGX H100 4 or 8-GPU backplane. Each of the GPUs feature an external 16-lane PCIe connection, as well as meshed private NVLink connection to the other GPUs. A complex and versatile PCIe switched fabric and related management software allow dynamic or fixed lane routing between GPUs, hosts, memory, and I/O according to application demands. To achieve the SOSA-compliant requirement in a VPX format, a unique PCIe Gen 5 Host Bus Adapter expansion technology is utilized.


**Looking Forward**


Through OpenVPX extensions to AI Transportables, new levels of low-latency sensor acquisition are possible. Direct access to memory in the GPUs without transit through host memory is possible, unlocking new levels of sensor bandwidth with reduced latency. The architecture shifts the role of the host processor to the GPUs which offer greater computing power and the combined enhanced performance required to support new and emerging applications.


*Click the buttons below to share this blog post!*


[Return to the main Blog page](https://onestopsystems.com/pages/one-stop-systems-blog)
