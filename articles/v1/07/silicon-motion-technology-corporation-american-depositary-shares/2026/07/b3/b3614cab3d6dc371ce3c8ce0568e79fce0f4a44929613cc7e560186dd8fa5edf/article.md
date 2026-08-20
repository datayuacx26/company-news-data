---
schema_version: "1.0.0"
document_id: "b3614cab3d6dc371ce3c8ce0568e79fce0f4a44929613cc7e560186dd8fa5edf"
company_key: "silicon-motion-technology-corporation-american-depositary-shares"
company: "Silicon Motion Technology Corporation"
source_id: "silicon-motion-technology-corporation-american-depositary-shares-news-import-e2d0ecbf2398"
canonical_url: "https://www.siliconmotion.com/company/blog/Functional-Safety/detail"
published_at: null
first_seen_at: "2026-07-22T13:30:35.445655+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:9d41ba02becf489476f251dc96be88c5bfcb1173d8e785a9db02625f296835c3"
---

# Blog-Automotive Functional-Safety Standards Up Ante for In-Vehicle Memory and Storage-Silicon Motion

This article was originally published on[Electronic Design](https://www.electronicdesign.com/markets/automotive/article/55285893/silicon-motion-automotive-functional-safety-standards-up-ante-for-in-vehicle-memory-and-storage) by Steve Shih, Automotive and Embedded Product Manager at Silicon Motion


[Contact Us](https://www.siliconmotion.com/support/contact/sales)


Increasing reliance on digital electronics and software in new vehicles requires a new generation of reliable memory that complies with strict functional-safety and security standards.


What you'll learn:


- Software-defined vehicles create a greater need for reliable and secure memory and storage solutions.
- In modern automotive design, it's essential to address functional safety and cybersecurity simultaneously.
- Adhering to industry standards such as ISO 26262, AEC-Q100, and ASPICE is crucial for meeting the stringent requirements of automotive-grade memories.


Automotive functional safety has long predated autonomous and software-defined vehicles (SDVs). As vehicles of all types become more digital, increasingly advanced features drive the demand for more memory and storage, which must meet stringent reliability and safety requirements.


These requirements are essential considerations for automotive systems designers as semiconductor content has become more pervasive. They must adapt to architectural shifts within automotive systems, delivering more functionality while also thinking about functional safety, reliability, and security. The memory and storage used in mission-critical applications can't afford to fail.


### SDVs Drive Up Semiconductor Content


The evolution of automotive electronics is increasingly driven by SDVs, which now include more advanced infotainment systems, advanced driver-assistance systems (ADAS), electric-vehicle (EV) power systems, and over-the-air (OTA) capabilities. Vehicle-to-everything (V2X) capabilities enable the vehicle's software to be upgraded overnight, offering the ability to download more detailed, up-to-date navigation information. Data can also be collected from the vehicle to diagnose a specific problem or gather system performance data over time.


Architecturally, SDVs are taking a more "zonal" approach, which groups functions together and centralizes compute, even as more data is collected from sensors and nearby networks (Fig. 1). Data growth combined with the addition of new features and services has led to the proliferation of electronic control units (ECUs) linked together with complex networking.


1. SDV architecture has transitioned from function-based grouping by domain, such as chassis and infotainment systems, to a zonal approach that organizes functions by physical area and centralizes computing.


Moving to a zonal architecture makes the vehicle more programmable, but this programmability through software requires more hardware. More computing means more semiconductor content, even as the number of ECUs and other computing devices is consolidated.


Aside from leading to more centralized data storage and increasing demands on internal connectivity, SDVs highlight the need for functional safety, reliability, and security.


### Functional Safety Intersects with Security


Functional safety and reliability have long been irrevocably intertwined. However, with the emergence of V2X capabilities, so is cybersecurity.


Bad actors can attack a connected car, as every vehicle has potential access points (Fig. 2). Each networking, memory, and storage device represents an attack surface, and V2X capabilities bring unique security challenges. A vehicular network comprises heterogeneous nodes, various speeds, and intermittent connections, and traditional security methods aren't always sufficient or effective.


2. SDVs underscore the importance of functional safety and cybersecurity, as connected cars feature vulnerable access points, including V2X communications.


Because cybersecurity rides alongside functional safety, an SDV could be vulnerable in many mundane ways. For example, using open-source software in the automotive industry can enable hackers to exploit shared system code that targets multiple vehicle models.


But, as with all computing, software isn't the only vector for threat actors to wreak havoc within a modern vehicle. Increasingly, hardware is a target for tampering and must be inherently secure to effectively contribute to overall functional safety. Thus, designers must consider cybersecurity standards along with those that govern functional safety and reliability.


### Digitization Raises Safety Stakes


As vehicles became more digitized and reliant on electronics, key standards governing automotive functional safety have been introduced. The emergence of autonomous vehicles and the growth of the EV market requires that these standards be refined and expanded.


Functional safety begins at the development phase, covering product specifications, production implementation, integration, verification, validation, and final release. Evaluating risk, including potential hazards and hazard scenarios, is critical to any automotive functional-safety program.


Typically, an automotive OEM conducts a Hazard Analysis and Risk Assessment (HARA) on any vehicle-level feature to determine the risk-reduction level needed for each potential hazard identified. This includes the likelihood and duration of a hazard during specific driving scenarios, and what the consequences might be in the event of a malfunction or failure.


HARAs don't go as deep as the component level. However, because semiconductors have become the core building blocks of modern vehicles, memory and storage devices must be automotive grade—they must handle harsh environments ranging from hot to cold and vibrations.


The semiconductor industry has its own safety standards that have to be followed even before applying others specifically related to automotive. This is through a concept known as Safety Element out of Context (SEooC), which is a bottom-up approach for developing software, hardware, or system elements. It can span different items and vehicles.


In addition, a systematic analysis technique known as failure modes, effects, and diagnostic analysis (FMEDA) is used to determine subsystem/device-level failure rates, failure modes, and diagnostic capability. FMEDA looks at all design components, including their functionality, failure modes of each component, the effect of any component failure mode on device functionality, and the ability of any automatic diagnostics to detect the failure. The FMEDA technique can predict failure rates per defined failure modes, which can be used to establish compliance with automotive functional-safety standards.


### Automotive Safety Standards Expand to Cover Memory and Compute


Several standards govern automotive functional safety that relate both to software and hardware.


Key among them is ISO 26262, which outlines guidelines to minimize the risk of accidents and ensure that automotive components perform their intended functions correctly and at the right time. ISO 26262 also lays out Automotive Safety Integrity Level (ASIL) ratings ranging from "A" for low risk to "D" for high risk—failure of a steering control system during driving is considered high risk.


ASIL ratings are becoming increasingly important as vehicles become more autonomous. ASIL D compliance is essential for supporting Level 5 autonomy.


Another relevant functional-safety standard is the AEC-Q100 standard. It ensures the safety of electronic parts by focusing on reliability, including stress testing for integrated circuits in automotive applications.


Because ISO 26262 doesn't account for systematic errors such as software flaws, Automotive SPICE (ASPICE) has emerged as the current standard for automotive software best practices. However, it has yet to be globally adopted. Software Process Improvement and Capability Determination (also known as ISO/IEC 15504, or SPICE) is a framework for software process assessment. It's designed to evaluate development factors that enable assessors to determine an organization's capacity for effectively and reliably delivering software products.


ASPICE applies this framework to the automotive industry and defines best practices for embedded software in automotive development. It differs from functional-safety standards such as ISO 26262 in that it covers how design is conducted if safety isn't a concern. Automotive designers should incorporate ASPICE and ISO 26262 guidelines to ensure effective safety practices.


The introduction of ISO/SAE 21434, meanwhile, reflects the emerging need to design vehicles that are protected against cybersecurity threats. It can cover hardware such as an automotive system-on-chip (SoC), software, or the design tool used to develop a modern vehicle.


As NAND flash has become the workhorse for automotive applications and appears in many forms, it must be highly reliable for mission-critical applications. This means it must also conform to standards such as AEC-Q100 so that it can withstand extreme environmental conditions and retain data in the event of a sudden failure caused by, say, a collision.


Given the connected nature of SDVs, automotive NAND devices must also be inherently secure, as any tampering can impact safety. That's why AES-256-bit full disk encryption is implemented for secure storage and over-the-air updates.


### NAND Flash Advances Performance Alongside Reliability


The functionality, safety, reliability, and security of NAND flash devices in automotive applications depend on many system elements, including controllers.


Silicon Motion's automotive-grade controllers comply with international functional-safety and reliability standards such as AEC-Q100, IATF 16949, ISO 26262, and ASPICE (Fig. 3). Adherence to these standards reflects that designers are developing infotainment systems for fully connected, data-driven, and intuitive in-car experience. They include immersive entertainment, immersive infotainment, and ADAS supported by V2X capabilities, which depend on robust, high-performance data storage.


3. Silicon Motion is the first company to achieve ASPICE CL3 compliance for automotive SSD controllers.


The automotive-grade eMMC, UFS, and SSD controllers developed by Silicon Motion share many of the same capabilities as those for other applications. They also support extended temperature, exhibit low defected parts per million (DPPM), and are designed to comply with the ASPICE standard.


The controllers undergo rigorous testing, including compliance with AEC-Q100 Grades 2/3, ISO 26262, ISO 21434, and IATF 16949 certifications.


The company's latest SSD controller, the SM2264XT-AT, was specifically designed for automotive applications and supports single root I/O virtualization (SR-IOV). Thanks to this feature, it's well-suited for future vehicles requiring centralized architecture implementation.


The SM2264XT-AT also supports up to eight virtual functions, reducing the CPU's burden by efficiently managing multiple virtual machines that access the SSD simultaneously. This feature is critical for software-defined vehicles, where lowering latency and ensuring fast response times for various applications is paramount.


The controller undergoes rigorous testing and adheres to various automotive processes and certifications, including AEC-Q100, ISO 26262 ASIL-B ready certification, IATF 16949 certification for supplier chain compliance, and ASPICE CL3.


As connected cars become more complex with V2X capabilities that support onboard intelligence and full autonomy, demands on memory and storage will only increase. This means designers need solutions, including storage and controllers, that are designed with functional safety in mind.


[Contact Us](https://www.siliconmotion.com/support/contact/sales)
