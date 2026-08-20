---
schema_version: "1.0.0"
document_id: "155ea553a63e2a30df39080257008416478973af6c176ba0b9ed58a228e3caa0"
company_key: "bullish-ordinary-shares"
company: "Bullish"
source_id: "bullish-ordinary-shares-rss-b348dbb1f0cd"
canonical_url: "https://medium.com/bullish-engineering/a-deepdive-into-full-stack-security-hardware-firmware-os-and-how-trust-flows-from-the-ground-69c63dfd7cf1"
published_at: "2025-01-28T05:26:46+00:00"
first_seen_at: "2026-07-20T04:36:16.773580+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:cc5314f22f98f242e64a4089cd8fb7b8caf1cde5b8ccf20b21912554e6ca118e"
---

# From hardware to hypervisor: Building secure systems for distributed engineers

[Mohammad Nauman](https://medium.com/@inauman?source=post_page---byline--69c63dfd7cf1---------------------------------------)


9 min read


·


Jan 28, 2025


--


***TL;DR:*** *Modern security isn’t just about application-layer defenses — it’s a full-stack challenge spanning hardware, firmware, OS, and applications. This blog explores the layers of security, from the Root of Trust (RoT) in hardware to the Chain of Trust (CoT) that ensures integrity across the stack. Engineers will learn how Secure Boot, TPMs, and hypervisors contribute to system security and why understanding these foundations is crucial for building secure distributed applications.*


Press enter or click to view image in full size


## From hardware to hypervisor: Building secure systems for distributed engineers


As a software engineer, my perspective on security was once narrowly focused on the application layer — the topmost layer of a system stack. I spent much of my time thinking about safeguarding against attack vectors like SQL injection or cross-site scripting. What I didn’t fully appreciate was the complexity and importance of the underlying layers — the hardware, firmware, and operating systems — that create the foundation for secure applications.


These layers are not just passive components but active participants in enforcing security. They establish trust at the lowest levels, allowing each subsequent layer to build on that foundation. Without securing the lower layers of the stack, even well-designed application security measures can be undermined.


What sparked this journey was the book *Rootkits and Bootkits: Reversing Modern Malware and Next Generation Threats* by Alex Matrosov et al. This book provided a deep dive into the hardware and OS layers, exploring how attackers exploit vulnerabilities in these lower levels and how defenders can build systems to prevent such exploits. As Matrosov notes, understanding the persistence mechanisms of bootkits highlights the importance of securing the boot process against tampering. This realization deepened my understanding of **full-stack security** , encompassing concepts like **Secure Boot** , the **Root of Trust (RoT)** , measured boot, and the broader hardware/software security stack — concepts that directly impact distributed systems, from securing sensitive application keys to ensuring the integrity of runtime environments.


Press enter or click to view image in full size


For readers seeking deeper technical insights, I highly recommend[Rootkits and Bootkits](https://www.amazon.com/Rootkits-Bootkits-Reversing-Malware-Generation/dp/1593277164/ref=sr_1_1?crid=9JOPDXDJZF8I&dib=eyJ2IjoiMSJ9.hKUxFCwiwLF9PH64gEzb2TLXf-Yjc5QPxk-lhwfTrVk_jZVmLNpZ-AMQp0EkonYXPVZOLdqUK30wbEF-b7YZjf2_mChTV6dy-w5BAG-MEbl9XP0DZK48Xs2JCFPF83PGPr6Iq0Jul0BE8OBAt7WubdrcAwCvx8Z4qSHTe2v73cDbjkDd99tAg2dIlpnNf5PV_sr1rSq6O-mU-89kts2v2BaJ3G9J3VPd2HRjDMrKAc4.2rTZtPkyplFrfFgMPe-it_upcUiBhu3ShGT1bNOMDs4&dib_tag=se&keywords=rootkits+and+bootkits&qid=1738039971&sprefix=rootkit%2Caps%2C189&sr=8-1) and the NIST Special Publication 800–193:[Platform Firmware Resiliency Guidelines](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-193.pdf) as foundational resources.


In this blog, I aim to share what I’ve learned about **full-stack security** in a way that’s approachable for distributed engineers. We’ll explore the layered abstractions — hardware, firmware, OS, and applications — and how they interact to enforce security. The goal isn’t to transform you into a hardware or security engineer, but to provide a framework for understanding the security-first principles that underpin modern secure system design.


Let’s begin with the foundational concepts of modern operating systems and why distributed engineers need to care about the security stack.


## The foundations of modern operating systems


Modern operating systems are designed with layered security and privilege in mind, serving as the foundation for application-level security. Understanding these concepts provides the context for how higher-level application security depends on secure interactions between hardware, the OS, and the application stack.


Operating systems enforce separation of privilege between user mode and kernel mode. In user mode, applications and processes run in a restricted environment, relying on the operating system to provide controlled access to hardware and privileged operations. In contrast, kernel mode operates at the highest privilege level, managing access to hardware, memory, and critical system resources. This separation ensures that user applications cannot directly interact with the kernel, reducing the attack surface and isolating sensitive system functions.


Modern processors extend this privilege separation with a ring structure. User applications operate at Ring 3 with the least privilege, while the OS kernel resides at Ring 0, controlling critical system resources. Below these layers, the hypervisor operates at Ring -1 to isolate virtual machines, and firmware operates at Ring -2 to handle platform initialization and hardware setup. This layered design ensures that trust flows downward and that higher-privileged layers can enforce policies on lower layers.


## Rootkits vs. Bootkits: The battle beneath the OS


Security is often described as an arms race, and nowhere is this more apparent than in the ongoing battle between defenders and attackers targeting the lower levels of the stack. Rootkits and bootkits exemplify this race, highlighting the importance of securing each layer of a system.


Rootkits typically exploit vulnerabilities in the operating system, embedding themselves in kernel-level functions to gain privileged access while remaining undetected. These tools allow attackers to control system processes, intercept data, and maintain persistence. In contrast, bootkits operate at an even lower level, targeting the boot process to gain control of the system before the OS even loads. By compromising firmware or the bootloader, bootkits bypass OS-level protections entirely, ensuring persistence across reboots.


This “race to the bottom” illustrates how attackers continually shift their focus to lower layers, exploiting historically less-defended areas. To counter these threats, mechanisms like Secure Boot, TPMs, and the **Chain of Trust (CoT)** have become essential. These defenses ensure that each stage of the boot process is cryptographically validated and trusted, effectively blocking malicious code from taking control. For distributed engineers, understanding these attack vectors emphasizes the need for secure design principles that account for the entire stack, from hardware to application.


## Hardware: The root of all trust


Building upon the operating system’s foundation, the security stack extends downward into the hardware. At the core of this stack lies the concept of trust, anchored in hardware mechanisms that establish a secure foundation for everything above.


The **Trusted Platform Module (TPM)** is a specialized hardware component designed to enable secure cryptographic operations. It provides non-volatile memory (NVRAM) for storing persistent data, such as cryptographic keys, that must survive reboots. Additionally, TPMs use **Platform Configuration Registers (PCRs)** to store cryptographic measurements of each stage of the boot process, enabling a “measured boot” process. The “extend” operation cryptographically appends new measurements to existing PCR values using algorithms like SHA-256, creating an immutable log of the boot sequence.


The Root of Trust (RoT) initializes the Chain of Trust, where each layer verifies the integrity of the next. For example, firmware verifies the bootloader, which in turn verifies the OS kernel. This cascading trust ensures that no untrusted components gain control. For distributed engineers, understanding how trust flows upward from hardware to applications is key to designing systems that are resilient against advanced threats.


## A brief history of BIOS to UEFI


To understand modern secure systems, it is important to explore the evolution from BIOS to UEFI and how this transition laid the groundwork for robust security mechanisms like Secure Boot.


The Basic Input/Output System (BIOS) was the standard for initializing hardware and bootstrapping operating systems for decades. It performed essential tasks like the Power-On Self-Test (POST), hardware initialization, and loading the bootloader from storage devices. However, BIOS had significant limitations. Operating in a 16-bit real mode without memory protection, it struggled to support modern hardware and was vulnerable to exploits like buffer overflows. These constraints made BIOS inadequate for the demands of evolving systems.


The Unified Extensible Firmware Interface (UEFI) replaced BIOS, addressing its limitations and introducing advanced capabilities. UEFI operates in a 64-bit environment, supports larger memory spaces, and provides faster boot times. Its modular architecture allows extensible drivers and applications, enabling more flexible hardware initialization. Most importantly, UEFI introduced Secure Boot, which verifies the integrity of bootloaders and OS components using X.509 certificates and cryptographic signatures. By maintaining a database of trusted certificates and hashes, UEFI ensures that only authorized code is executed during the boot process, bridging firmware and OS-level security.


This evolution underscores the importance of foundational technologies in enabling secure environments for modern applications. For distributed engineers, understanding this history reveals how firmware advancements contribute to the secure systems that underpin their applications.


## Firmware: bridging hardware and security


Firmware often operates behind the scenes, yet it plays a critical role in the security stack. Acting as the bridge between hardware and higher layers like the OS, firmware initializes the system, enforces early security checks, and sets the stage for Secure Boot.


Firmware is low-level software embedded in hardware components to control their behavior. Stored in non-volatile memory such as SPI flash or ROM, firmware persists across power cycles, ensuring it can perform its tasks during every boot sequence. Platform Initialization (PI) firmware, for example, sets up critical components like the CPU, memory, and peripheral controllers, creating the environment needed for the OS to load securely.


Modern systems include two primary types of firmware: native firmware embedded in the motherboard or chipset and external device firmware, such as that found in GPUs or network cards. While these modular designs increase flexibility, they also introduce new potential vulnerabilities. Secure Boot relies on firmware to enforce trust during the boot process, ensuring that all components are cryptographically validated before execution.


For distributed engineers, understanding firmware’s role is essential. A compromised firmware layer can undermine even the most robust application-level defenses. By appreciating its critical role, engineers can build systems that align with secure infrastructure principles.


## Secure Boot: the three layers of enforcement


Secure Boot is a cornerstone of modern system security, ensuring that only trusted and authorized code is executed during the boot process. By enforcing trust at different depths of the system stack, Secure Boot provides a comprehensive framework to protect against threats like bootkits and other low-level attacks. It operates across three critical layers: platform, UEFI, and OS.


**Platform Secure Boot** ensures the integrity of the hardware and platform initialization firmware by anchoring trust in components like the TPM. It validates the boot process using cryptographic signatures and measurements stored in PCRs, establishing a Root of Trust. **UEFI Secure Boot** extends this trust by validating drivers, bootloaders, and Option ROMs against a database of trusted certificates and hashes. Finally, **OS Secure Boot** ensures that runtime components like the kernel and drivers are verified, maintaining integrity throughout system operation.


Each layer of Secure Boot builds on the trust established by the previous one, creating a comprehensive defense. Distributed engineers benefit from understanding this layered approach, as it highlights how secure foundations protect sensitive workloads and enable trustworthy application environments.


## Adopting a security-first mindset: lessons for distributed engineers


Understanding the security stack — spanning hardware, firmware, operating systems, and applications — provides engineers with a foundation for designing resilient and trustworthy systems. Security is not just about preventing attacks; it’s about preserving the integrity, confidentiality, and availability of systems and data. Each layer of the stack contributes to this mission, creating a cohesive defense against evolving threats.


A **security-first** mindset involves anticipating vulnerabilities and designing systems that are resilient from the ground up. Engineers should understand how hardware-rooted mechanisms like Secure Boot and TPMs protect the layers above. Thinking offensively, considering how attackers might exploit lower levels, and collaborating across teams are key to building robust solutions. While mechanisms like Secure Boot and verified boot help protect system integrity, vulnerabilities at higher layers — such as application flaws or misconfigurations — can still provide entry points for attackers, making ongoing security hardening and threat modeling essential.


By adopting a security-first mindset, distributed engineers can contribute to building a more resilient digital infrastructure. Understanding how hardware, firmware, and operating systems interact ensures that the applications they design are built on a foundation of trust, protecting both systems and the users who depend on them.


Feeling inspired by the cutting-edge work we’re doing at Bullish? We’re always on the lookout for talented individuals to join our team.[Explore our open roles](https://bullish.com/careers/) and be Bullish on your career. Want to stay up to date with the latest news from across Bullish? Follow us on[LinkedIn](https://www.linkedin.com/company/bebullish) and[X](https://twitter.com/Bullish) .


*Disclaimer: This material, including any material accessed through embedded links (“Information”) is for general informational purposes only and is not intended to constitute investment, tax, accounting, legal or financial advice. This Information is not an offer to buy or sell or a solicitation of an offer or to buy or sell any particular asset or to provide financial services of any kind. You should consider your own personal situation carefully and consult your professional advisors before making any investment decision.*


*Bullish makes no representation as to the accuracy, completeness, timeliness, suitability, or validity of any Information. The products, services, and solutions discussed in the Information are likely to change, so the Information may become outdated, incorrect or incomplete. Bullish is under no obligation to update Information. Bullish will not be liable for any errors or omissions in the Information. Virtual assets and related products are high risk. Access to certain assets or services referred to in the Information may not be available in your jurisdiction or to all types of investors. Information may reference the Bullish Exchange, which is licensed by the Gibraltar Financial Services Commission (DLT license: FSC1038FSA). This Information may not be quoted, deleted, or modified in any way without Bullish’s prior written consent. All rights reserved. Bullish and the Bullish Logo are trademarks of Bullish Global.*
