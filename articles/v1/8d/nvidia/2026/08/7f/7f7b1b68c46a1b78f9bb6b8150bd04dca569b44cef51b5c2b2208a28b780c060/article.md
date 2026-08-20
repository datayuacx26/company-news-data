---
schema_version: "1.0.0"
document_id: "7f7b1b68c46a1b78f9bb6b8150bd04dca569b44cef51b5c2b2208a28b780c060"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/800-vdc-power-architecture-ai-factory/"
published_at: "2026-08-11T15:00:06+00:00"
first_seen_at: "2026-08-11T16:11:35.071324+00:00"
fetched_at: "2026-08-11T16:11:36.987527+00:00"
content_hash: "sha256:6a8635ff3e4786f102e5a40075e624323befd3ad80c13fcc5e7821e62bd0463b"
---

# Why Scaling AI Compute Performance Requires a New Power Architecture

Every new generation of accelerated computing demands more from the infrastructure underneath it — more compute performance, higher rack density and more efficient, scalable power distribution. The bottleneck isn’t just wattage. It’s how power gets from the grid to the GPU.


In traditional power delivery, electricity travels from the grid as an alternating current (AC) and gets converted multiple times, each time adding overhead and complexity as racks become denser. At the power levels that next-generation AI compute demands, even small inefficiencies compound quickly.


800 VDC simplifies that path. By distributing power at higher voltage through a direct current (DC), fewer conversion stages stand between the grid and the accelerator — which means more of the available power reaches the compute.


NVIDIA DSX reference designs are built to guide AI factories through the transition from today’s AC infrastructure through hybrid architectures and into fully native 800 VDC facilities.


NVIDIA, Google and Microsoft have been developing the 800 VDC architecture together through the Open Compute Project (OCP), and published a joint


[white paper](https://www.opencompute.org/documents/dcf-power-distribution-lvdc-white-paper-version-1-0-final-pdf-1) March 2026 and the


[LVDC Solid-State Transformer Specification v0.3](https://www.opencompute.org/documents/ocp-sst-design-specification-v0-3-final-pdf) July 2026. More than 80 equipment manufacturers and infrastructure companies are already building products to this specification.


## **Existing Facilities Don’t Have to Wait**


Most of today’s AI factories were designed around AC distribution. The NVIDIA MGX-compatible 800 VDC power rack, arriving in the second half of 2026, creates a hybrid architecture that brings next-generation rack-scale compute performance to facilities that are already built and operational. It’s designed to slot into existing AC infrastructure and deliver 800 VDC to compute racks within the row — no changes to the building’s electrical system required.


“800 VDC unlocks the compute performance and power density required for AI at scale,” said Vladimir Troy, vice president of data center infrastructure at NVIDIA. “Through OCP, NVIDIA is working with more than 80 ecosystem companies to give AI factories a practical path forward — not just a future vision.”


For site owners, this matters because the investments already made don’t have to be stranded. Land, power rights, building infrastructure — the hybrid approach preserves all of it while opening the door to higher compute density.


## **A Roadmap for Every Stage of Growth**


800 VDC provides AI factories a roadmap to scale, with on-ramps at every stage of growth.


The power rack is where most operators will start — a near-term, hybrid-compatible path into higher-density compute. For operators building out dedicated AI factory environments, the row power center — a centralized power station for a full rack row — uses an overhead 800 VDC busway to scale power distribution across multiple rack rows, supporting up to 2 megawatts per row, with availability expected in 2027. And for new facilities being designed, the DC power block — a facility-scale unit that converts grid power directly to 800 VDC in a single step — will enable direct medium-voltage conversion at massive scale: the architecture for AI infrastructure being planned for the decade ahead.


## **An Open Standard Means a Real Supply Chain**


The 800 VDC architecture specifications define common interfaces so that power hardware from different vendors can work together inside the same 800 VDC facility. With 80+ companies building to the specifications, the supply chain is forming around an open standard.


These building blocks are captured in NVIDIA DSX reference designs, giving operators a system-level blueprint to connect power architecture, rack-scale computing and facility infrastructure as they scale AI factories.


Wood Mackenzie projects $9 trillion in global AI and data infrastructure investment through 2040. The facilities that can absorb that investment will be the ones that resolved their power architecture before compute demand outran what their infrastructure could deliver. NVIDIA, Google and Microsoft are working with the broader ecosystem to make sure 800 VDC is ready when operators need it — and that existing facilities have a path to get there now.


*Read the 800 VDC*[OCP blog](https://www.opencompute.org/blog/powering-the-next-era-of-ai-how-google-microsoft-and-nvidia-are-standardizing-and-accelerating-the-industry-transition-to-lvdc) *and the*[NVIDIA 800 VDC white paper](https://nvdam.widen.net/s/nlpfg6lzfw/nvidia-800-vdc-industry-alignment-white-paper) *. Learn more about the*[NVIDIA DSX](https://docs.nvidia.com/dsx) *reference architecture guide.*
