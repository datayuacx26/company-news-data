---
schema_version: "1.0.0"
document_id: "a00fd040fe3a749a38fbfd3436a4248badba95be66d8da24a35c40a5635f1b70"
company_key: "rambus-inc-common-stock"
company: "Rambus Inc."
source_id: "rambus-inc-common-stock-rss-ae9cc712c4a3"
canonical_url: "https://www.rambus.com/blogs/rambus-introduces-rt-648-bringing-arm-based-root-of-trust-into-the-automotive-css-ecosystem/"
published_at: "2026-06-17T21:00:40+00:00"
first_seen_at: "2026-07-20T03:32:34.663136+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:b0ade6cc52efc8de4ca5894e744c3972c21fb7ca6cd7a6c49c39c7002be6574c"
---

# Rambus Introduces RT-648: Bringing Arm-Based Root of Trust into the Automotive CSS Ecosystem

As the automotive industry transitions to software-defined vehicles (SDVs), security architectures must evolve from discrete features into integrated, platform-level capabilities. The next generation of automotive SoCs requires not only stronger security, but a more standardized and extensible foundation that aligns with broader silicon ecosystems.


Rambus is addressing this need with the introduction of the automotive-grade CryptoManager RT-648, which is the company’s first embedded Hardware Security Module (eHSM) built around an Arm® processor and designed for pre-integration into the Arm Compute Subsystem (CSS) ecosystem.


This milestone represents a significant step forward: combining Rambus’ proven hardware security expertise with the scalability, interoperability, and ecosystem alignment of Arm-based architectures.


## **A New Foundation: Root of Trust Integrated into the Arm Ecosystem**


The RT-648 is the first Rambus Root of Trust solution to integrate a 32-bit Arm Cortex-M33 processor, aligning it more naturally with Arm-based SoC design environments, like CSS. That matters because Arm CSS gives automotive SoC developers a more standardized and pre-integrated foundation for building the complex compute platforms required by software-defined vehicles through its automotive variant, Zena CSS.


By combining key building blocks for compute, safety, security, interconnect, and software into a validated subsystem architecture, Arm Zena CSS reduces integration effort, shortens development timelines, and enables earlier software readiness. This allows OEMs and silicon providers to scale platforms across ADAS, IVI, and central compute workloads while still leaving room for differentiation.


Through the incorporation of the Arm Cortex-M33 processor, RT-648 fits naturally into Arm-based SoC design environments, helping designers add a hardware Root of Trust alongside Arm CSS-based platforms. This reduces design complexity, allows teams to work within familiar Arm toolchains and workflows, and makes it easier to embed hardware-based trust into modern automotive architectures. The result is a Root of Trust that is not only secure in its own right, but also closely aligned with the ecosystem shaping next-generation automotive SoC development.


### **Hardware-Anchored Security from First Instruction**


At its core, the RT-648 establishes a hardware root of trust from the earliest stages of device operation. Operating within an isolated, tamper-resistant environment, it provides essential security services that underpin system integrity across the full lifecycle:


- Secure boot and authentication
- Secure key storage and management
- Cryptographic acceleration
- Lifecycle state control


By anchoring these capabilities in hardware, the RT-648 ensures that trust is established before any higher-level software is executed.


### **Proven in Automotive Silicon: Deployment with Telechips**


Importantly, the RT-648 is not just a forward-looking concept, it is already deployed today **.**


As highlighted in[this white paper](https://go.rambus.com/securing-the-software-defined-vehicle) , Telechips has integrated the RT-648 Root of Trust into its automotive SoCs as part of a unified, hardware-based security architecture.


Telechips’ platforms, designed for IVI, digital cockpit, ADAS, and domain controller applications, rely on the RT-648 for a variety of tasks, including:


- Establishing trusted system initialization from power-on
- Enabling secure key management and protection
- Providing strong isolation across mixed-criticality workloads
- Supporting scalable security across product families


This integration allows Telechips to consolidate diverse workloads onto centralized SoCs while maintaining strict security and safety boundaries, an essential requirement for software-defined vehicle architectures.


“As automotive platforms evolve toward software-defined vehicles, security must be embedded into the architecture from the very beginning” said Leanne Lee, Vice President, Head of Product Strategy Planning Group at Telechips. “By integrating the Rambus RT-648 Root of Trust into our automotive SoCs, Telechips has implemented a unified hardware-based security framework that supports secure boot, domain isolation, and long-term security scalability across a wide range of automotive platforms, including IVI, digital cockpit, ADAS, and domain controllers.”


The result is clear: RT-648 is already proven in real automotive deployments, providing customers with a validated, production-ready foundation for secure SoC design.


### **Architected for Isolation, Resilience, and Safety with Post-Quantum in Mind**


The integrated Arm Cortex-M33 processor operates in a Dual Core Lock Step (DCLS) configuration, enabling independent execution of security firmware with built-in fault detection. This architecture ensures security isolation from the main application processor, protection even if higher-level software is compromised, and alignment with automotive functional safety requirements. The RT-648 is developed in accordance with ISO 26262 functional safety and ISO/SAE 21434 cybersecurity engineering, supporting ASIL-B, helping to reduce the burden on SoC developers by providing a certifiable, safety-aware Root of Trust.


The RT-648 is a forward-looking upgrade to the RT-64x family, integrating both classical and quantum-resistant cryptography, including FIPS 203 ML-KEM, FIPS 205 SLH-DSA, and FIPS 204 ML-DSA. This ensures automotive platforms are prepared for emerging threats without requiring architectural redesign.


#### **A New Chapter in Rambus Automotive Security**


The shift to software-defined vehicles demands security architectures that are not only robust, but also standardized, reusable, and able to evolve over long product lifecycles. The RT-648 addresses that need with a hardware-based security foundation designed for integration into modern Arm-based automotive platforms, helping reduce integration risk while supporting platform reuse and long-term adaptability through software updates.


By aligning with the Arm ecosystem and proving its value in production silicon with partners such as Telechips, the RT-648 extends Rambus security leadership into a new phase of automotive design. It combines deep hardware security expertise with ecosystem alignment and real-world deployment, giving SoC developers a practical foundation for building secure, scalable automotive platforms.


Learn more about the CryptoManager RT-648[here](https://www.rambus.com/security/root-of-trust/rt-648) .
