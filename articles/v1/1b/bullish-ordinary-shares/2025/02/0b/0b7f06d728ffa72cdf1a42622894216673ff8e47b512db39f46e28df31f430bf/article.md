---
schema_version: "1.0.0"
document_id: "0b7f06d728ffa72cdf1a42622894216673ff8e47b512db39f46e28df31f430bf"
company_key: "bullish-ordinary-shares"
company: "Bullish"
source_id: "bullish-ordinary-shares-rss-b348dbb1f0cd"
canonical_url: "https://medium.com/bullish-engineering/bootstrapping-trust-in-distributed-systems-4e063f418b0d"
published_at: "2025-02-11T18:16:17+00:00"
first_seen_at: "2026-07-20T04:36:16.773580+00:00"
fetched_at: "2026-08-20T01:07:04.225966+00:00"
content_hash: "sha256:702ba6870347eb3f556debc079fb84645d38381f4a6457beeda24b587f9ce3fd"
---

# Bootstrapping Trust in Distributed Systems

***TL;DR:*** *Secure Boot ensures that only trusted, untampered code runs during a system’s startup process, creating a chain of trust from hardware to virtual machines. This blog explores how hardware-based trust, like Google’s Titan chip, forms the foundation of system integrity, while Dynamic Root of Trust technologies like Intel SGX add flexibility later in the boot process. It also covers how virtual environments maintain trust through vTPMs, even without direct hardware ties. Learn how Google’s Secure Boot implementation combines these layers to protect distributed systems from the ground up.*


### Bootstrapping Trust: The role of Secure Boot from hardware chips to virtual machines


In my previous blog, *From hardware to hypervisor: building secure systems for distributed engineers* , we explored how secure systems rely on more than just application-level safeguards. Beneath every robust application lies a secure foundation — rooted in the hardware, firmware, and operating system. This layered approach is essential in distributed systems, where machines constantly communicate, exchange data, and process sensitive information.


In this blog, we’ll take a deeper dive into one of the most critical mechanisms that establishes this foundation: **Secure Boot** . We’ll explore how Secure Boot ensures that each component of a system’s startup sequence is trustworthy — whether that’s the physical hardware in a data center or a virtual machine running in the cloud.


### Why Secure Boot matters in distributed systems


Imagine a scenario where a compromised machine joins your distributed network. It might appear legitimate, but beneath the surface, malicious code could be quietly exfiltrating data, tampering with workloads, or introducing vulnerabilities into your system. In distributed environments, where thousands of machines interact, **how do you ensure each machine is trustworthy?**


Secure Boot addresses this by enforcing integrity checks at every stage of a system’s startup process. From the moment a machine powers on, Secure Boot ensures that only **authorized, untampered code** runs, providing a cryptographic chain of trust that extends from the hardware to the operating system — and even into virtual machines.


### Understanding the layers of Secure Boot: from hardware to VMs


Before diving into Secure Boot layers, it’s important to clarify two key concepts: **Root of Trust (RoT)** and **Chain of Trust (CoT)** .


**Root of Trust (RoT):** The Root of Trust is the **ultimate source of security** in a system. It’s typically a **hardware component** (like Google’s Titan chip or a TPM) that is **inherently trusted** and responsible for verifying the integrity of the first code that runs when a system boots. Think of it as the **“ground floor”** of security — if this layer is compromised, everything built on top of it can be affected.


**Chain of Trust (CoT):** The Chain of Trust is the sequence of components that are **verified one after another** during the boot process. Each stage in the chain **validates the next** before handing over control, ensuring that only trusted software executes. It’s like a **relay race** , where each runner checks the baton before passing it along — if any runner drops or tampers with the baton, the entire process is compromised.


Understanding these two concepts will help frame how Secure Boot establishes and maintains trust from the hardware level to virtual machines. To see how Secure Boot functions in modern infrastructure, it helps to think of it as a **three-layered system of trust** :


#### 1. Hardware Root of Trust


At the foundation is the **Hardware Root of Trust (Hardware RoT)** . This is the ultimate source of trust for a physical system. Hardware modules like Google’s **Titan chip** or **Trusted Platform Modules (TPMs)** are embedded directly onto the motherboard, and they’re the first to initialize when a machine powers on.


These components **measure** the integrity of the firmware (like BIOS or UEFI) and verify it hasn’t been tampered with. If anything seems out of place — say, unauthorized firmware modifications — the boot process halts, preventing the system from starting in an untrusted state. Google’s Titan chip, for example, keeps the CPU in reset mode until it verifies the firmware, ensuring a **secure foundation** for everything that follows.


#### 2. Dynamic Root of Trust


While Hardware RoT starts the trust chain at power-on, the **Dynamic Root of Trust (DRTM)** comes into play **later in the boot process** . Technologies like **Intel SGX** or **AMD SEV** allow systems to dynamically establish trust after certain components have already loaded.


However, this introduces a potential **vulnerability window** . Since DRTM initializes later, there’s a gap where malicious code could load early in the boot process and modify the environment before the DRTM steps in. Google’s **Titan chip addresses this** by ensuring firmware is verified before the CPU even starts, effectively closing this gap.


DRTM solutions are typically integrated at the CPU level but might not be as deeply embedded in the motherboard’s trust chain as Titan or TPMs. This layered approach adds flexibility but requires careful coordination to prevent gaps in the trust model.


#### 3. Virtual Root of Trust


In today’s cloud environments, physical servers host **multiple virtual machines (VMs)** to maximize hardware utilization. But here’s the challenge: how do you extend the same level of trust from the physical hardware to the virtual machines running on top of it?


The **Virtual Root of Trust (vRoT)** , implemented through **virtual Trusted Platform Modules (vTPMs)** . In GCP, vTPMs are implemented **at the hypervisor level** , providing a layer of trust for virtual machines operating within a secure, hardware-validated environment. Since VMs can’t rely directly on the hardware-based RoT, **vTPMs** provide a similar mechanism in the virtualized environment.


In **Google Cloud Platform (GCP)** , for instance, **vTPMs use Platform Configuration Registers (PCRs)** to measure and verify the VM’s boot sequence. **PCR0** captures information like the **Compute Engine firmware version** and **memory encryption technology** . While this isn’t directly tied to the physical Titan chip, it creates a logical anchor of trust from the VM’s world perspective.


However, unlike hardware RoT, VM Secure Boot doesn’t follow a fully linear chain of trust from power-on to OS. Instead, it **starts with a virtual Root of Trust** that inherits trust indirectly from the secure host environment. This design allows cloud providers like Google to **scale trust** across vast virtualized environments while maintaining security integrity. Other cloud providers, like AWS and Azure, implement variations of Secure Boot that integrate hardware-based trust more closely with virtual machines. For example, AWS Nitro provides hardware-backed security features accessible at the VM level. However, the core principle remains consistent across platforms: virtualization abstracts hardware roots of trust, requiring careful coordination to maintain the chain of trust.


### The role of PCRs in Secure Boot: measuring trust at every step


At the core of Secure Boot is the concept of **measured boot** , where each component of the startup sequence is **measured** and recorded in **Platform Configuration Registers (PCRs)** . Think of PCRs as a cryptographic ledger, capturing the fingerprint of each boot stage.


In GCP Secure Boot, key PCRs include:


**PCR0** : Measures firmware components, including the Compute Engine firmware version and memory encryption details. This is relatively **static** and reflects the host’s secure state, anchored by the Titan chip.


**PCR4** : Records measurements related to the **OS shim** , second-stage bootloader, and kernel. This register is **dynamic** , updating as more components load.


**PCR7** : Captures the **Secure Boot policy** details, such as trusted signature databases. This is typically static unless the policy itself changes.


The **order of measurements** matters. Each PCR value is calculated by hashing the previous value with the new measurement, creating an immutable sequence. If even a single component changes (like during an OS upgrade), the corresponding PCR value will differ, signaling a change in the boot environment.


### Sealing secrets: protecting keys with measured boot


Beyond verifying the boot process, Secure Boot also protects sensitive information through **key sealing** . In this process, cryptographic keys are **sealed to specific PCR values** , meaning they can only be accessed if the system’s measurements match expected values.


For example, if a VM’s key is sealed to **PCR4** , and the OS shim or kernel changes (like during an upgrade from Ubuntu 18.x to 22.x), the PCR4 value will change. This prevents the key from being unsealed until it’s **resealed** to match the new boot measurements.


To manage system updates without constant resealing, **flexible sealing policies** can allow a range of acceptable PCR values. However, this introduces potential **rollback risks** if an attacker reverts the system to an older, vulnerable state that’s still within the accepted range. Another approach to mitigate brittleness is using **non-brittle PCRs** , where keys are sealed to the values (like signatures of trusted components) rather than specific PCR states. This method enhances flexibility while ensuring that the system’s integrity remains intact. Balancing flexibility with security is key to effective key management.


### How hardware and virtual Secure Boot work together


Google’s implementation of Secure Boot showcases how hardware and virtual trust models can work in tandem:


**Titan Chip (Hardware Root of Trust)** secures the physical host, ensuring the firmware and boot process start in a trusted state.


**vTPM (Virtual Root of Trust)** provides attestation for virtual machines, using PCR measurements to verify the integrity of the VM’s boot process.


While the **vTPM doesn’t directly inherit** trust from the Titan chip, it operates within a secure host environment that Titan protects. This layered approach ensures that both physical machines and virtual instances maintain a strong chain of trust.


### Rethinking trust: hardware vs. virtual roots of trust


The shift from **hardware-based trust** to **virtual trust models** reflects the evolution of cloud computing. In traditional systems, trust flowed linearly from hardware to software. But in virtualized environments, the trust model becomes more complex.


Secure Boot extends trust into these virtual systems, but it also introduces new challenges. Virtual environments require careful coordination between the physical and virtual roots of trust to ensure that the **chain of trust (CoT)** remains unbroken.


For engineers working in distributed systems, understanding these layers of trust is essential. Whether you’re securing sensitive application keys or ensuring the integrity of runtime environments, Secure Boot provides a foundation for building resilient, trustworthy systems.


### Conclusion: bootstrapping trust in modern systems


While this blog focused on Secure Boot mechanisms in Google Cloud Platform (GCP) using technologies like Titan and vTPM, other major cloud providers like AWS and Azure implement similar secure boot mechanisms. Each platform has its own hardware roots of trust and virtualization strategies to maintain boot integrity. In future blogs, we’ll explore how these platforms approach Secure Boot and how their models compare to GCP.


In distributed environments, trust isn’t just about the applications you deploy — it’s about the entire stack, from the hardware that powers your servers to the virtual machines running your workloads.


**Secure Boot** ensures that every layer of this stack starts from a place of trust. By understanding how Secure Boot functions — from **hardware roots** like Google’s Titan chip to **virtual roots** in the cloud — engineers can build systems that are not only functional but secure by design.


As we continue to push the boundaries of distributed systems, let’s remember that trust begins at the very first boot — and Secure Boot is our way of ensuring that trust carries through every layer.


Feeling inspired by the cutting-edge work we’re doing at Bullish? We’re always on the lookout for talented individuals to join our team.[Explore our open roles](https://bullish.com/careers/) and be Bullish on your career. Want to stay up to date with the latest news from across Bullish? Follow us on[LinkedIn](https://www.linkedin.com/company/bebullish) and[X](https://twitter.com/Bullish) .


*Disclaimer: This material, including any material accessed through embedded links (“Information”) is for general informational purposes only and is not intended to constitute investment, tax, accounting, legal or financial advice. This Information is not an offer to buy or sell or a solicitation of an offer or to buy or sell any particular asset or to provide financial services of any kind. You should consider your own personal situation carefully and consult your professional advisors before making any investment decision.*


*Bullish makes no representation as to the accuracy, completeness, timeliness, suitability, or validity of any Information. The products, services, and solutions discussed in the Information are likely to change, so the Information may become outdated, incorrect or incomplete. Bullish is under no obligation to update Information. Bullish will not be liable for any errors or omissions in the Information. Virtual assets and related products are high risk. Access to certain assets or services referred to in the Information may not be available in your jurisdiction or to all types of investors. Information may reference the Bullish Exchange, which is licensed by the Gibraltar Financial Services Commission (DLT license: FSC1038FSA). This Information may not be quoted, deleted, or modified in any way without Bullish’s prior written consent. All rights reserved. Bullish and the Bullish Logo are trademarks of Bullish Global.*


---


[Bootstrapping Trust in Distributed Systems](https://medium.com/bullish-engineering/bootstrapping-trust-in-distributed-systems-4e063f418b0d) was originally published in[bullish-engineering](https://medium.com/bullish-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
