---
schema_version: "1.0.0"
document_id: "1431822fb8f421c874f78f5001a989235c4286e434cd73952f7a906d32f0cc1a"
company_key: "western-digital-corporation-common-stock"
company: "Western Digital Corporation"
source_id: "western-digital-corporation-common-stock-rss-a1b03adbb2f1"
canonical_url: "https://blog.westerndigital.com/post-quantum-cryptography-enterprise-hdd-security/"
published_at: "2026-05-18T12:57:32+00:00"
first_seen_at: "2026-07-20T23:21:38.045244+00:00"
fetched_at: "2026-07-28T21:13:26.363881+00:00"
content_hash: "sha256:12ce2d956e8492aa312cef869032c2a34c8f56964d5a316f9c3c20e8e2f802cc"
---

# Securing Data Storage for the Quantum Age

May 18, 2026


7 min read


[Technology](https://blog.westerndigital.com/category/technology/)


# Securing Data Storage for the Quantum Age


[Reed Martin](https://blog.westerndigital.com/?guest_author=reed-martin)


## Key Takeaways


- Quantum computing is an exciting new technology that offers tremendous new capabilities in computing, but among them is the potential to render some existing encryption standards obsolete.
- WD has integrated NIST-approved post-quantum cryptography1 (PQC) standards into its newest upcoming UltraSMR drives—the first HDDs to implement quantum-resistant algorithms. This will help protect WD HDDs’ secure boot processes, firmware updates, and the overall security of the data center.
- Adopting PQC-enabled drives now is not premature—WD is helping CIOs and IT leaders safeguard their data workflows and meet policy requirements as quantum computing matures.
- WD differentiates itself by proactively deploying PQC protections into its Ultrastar® DC HC6100 UltraSMR hard disk drives. We are providing this technology to our enterprise customers sooner rather than later and helping to enhance their security now and into the next decade.
- WD’s PQC-enabled drives integrate seamlessly with existing ecosystems and data center infrastructures. PQC capability is transparent to the host and the customer and does not require software or process changes.


Safeguarding enterprise data at global scale is as complex as it is crucial. Whether at rest or in transit, the billions of bytes residing on enterprise hard drives today are typically encrypted to mitigate security weaknesses at points along the data path or workflow. However, a challenge that often goes unnoticed or unappreciated is keeping the drives themselves secure against multiple threats ranging from mundane to sophisticated.


## WD’s commitment to enterprise data security


At WD we make protecting our customers’ storage solutions part of our company mission. This is why upgrading the algorithms that protect our storage hardware from bad actors is critical to our engineering and innovation efforts.


Toward that end, WD’s rollout of NIST-approved quantum computing safeguards in our latest enterprise-class UltraSMR drives includes an additional hardware module, since pushing out updated HDD drivers or firmware is not enough to address this challenge.


Hackers and other adversaries may not yet have access to quantum computing capabilities to defeat RSA security and other long-established safeguards, but that day may be coming soon. As a result, WD is proactively upgrading the capabilities of its product lineup to help address the threat now.


## Why quantum computing threatens current encryption standards


Traditional encryption relies on factoring large numbers within a system. But because quantum computers will be able to generate prime factor solutions exponentially faster, bad actors are expected to be able to use such systems to defeat NIST’s current cryptographic protections within a decade, according to the organization.2


Specifically, a quantum computer or even a cloud service offering quantum *capabilities* could potentially forge encrypted digital signatures on firmware updates—making malicious code look authentic and potentially granting an attacker full control of a legacy hard drive.


While this in itself would not compromise an entire network’s security, quantum-powered malware could overcome the cryptographic checks at multiple levels of protection including those that verify bootloaders and firmware, allowing hackers to use forged keys. Enterprises might then unknowingly be using compromised hard drives within their critical infrastructure.


Protecting against this scenario is top of mind for WD because drive-level security is a longer-term challenge than most IT professionals realize. Enterprise 3.5” HDDs can remain in service for five years or longer, potentially extending a window of enterprise vulnerability. If either the cryptographic signatures used in a fleet of drives’ secure boot code or firmware updates were to become vulnerable at any point during that interval, these bad actors could potentially exploit already-deployed hard drive infrastructures and extract critical information.


The unknown vulnerability might persist until those drives were physically removed from the data centers upon their retirement. Destroying the physical drives in industrial-grade hardware shredders might not matter as much at that point, since the data that the drives held could have already been vulnerable and extracted.


## How post-quantum cryptography protects enterprise HDDs


Fortunately, implementing PQC capability in HDDs or enhancing the security posture of the drives’ enterprise customers does not require a functional quantum computer. It can be done today.


PQC safeguards work by deploying longer secure keys and better algorithms. WD is taking the step to include additional hardware-based security elements into our UltraSMR drives themselves that are designed to help them remain secure even if bad actors have access to a quantum computer to supercharge their hacking efforts.


## WD’s PQC implementation adhering to NIST standards


By deploying these innovations, WD is following mandated NIST-recommended algorithms and CNSA 2.0 guidelines surrounding compliance and interoperability across enterprise ecosystems. WD’s newly deployed public key infrastructure (PKI) and hardware security modules (HSMs) which are part of this initiative will continue to support code-signing and certificate management.


WD has implemented PQC on the newest, upcoming HDDs such as the Ultrastar DC HC6100 currently in qualification at hyperscale customers, and will eventually expand this capability across additional WD enterprise hard drives. By deploying post-quantum algorithms now and making the timing of the adoption a priority, WD will ensure that its high-capacity enterprise hard drives include the latest safeguards before quantum computing becomes mainstream and invariably available from as-a-service platforms.


## Preparing your infrastructure for PQC storage


Enterprise IT professionals should look ahead to updating their drive fleets to the newest upcoming WD Ultrastar HDDs while also laying the groundwork for putting the drives into production by:


- Generating a plan for PQC adoption by working with storage vendors such as WD who are deploying PQC-enabled HDDs. Adoption of WD’s enhanced technology can help streamline HDD fleet transitions with less disruption or need for urgent deployments later.
- Prioritizing the cataloging of long-lived assets and planning to upgrade new HDD deployments with WD’s PQC-capable drives for all HDD-inclusive storage systems.
- Modernizing the enterprise’s key management infrastructure by testing to confirm that current APIs and orchestration frameworks can handle longer post-quantum keys and certificates, hybrid certificates, and centralized key management systems.3


## The time for PQC is now


This adoption of PQC safeguards should not be considered “early” or “premature” since the challenges related to quantum computing are present-day concerns for CIOs, CTOs, and IT professionals.4 Indeed, enterprises should prepare today and allocate related budgets accordingly since quantum-related security threats may arrive sooner than executives in the C-Suite expect.


The innovation and effort WD has put toward incorporating this new security posture will be critical to protecting enterprise data, data center infrastructures, and workflows today and well into the next decade. PQC safeguards and algorithm adoption are designed as a step toward providing additional security layers that differentiate WD from our competitors and build on our legacy of effectively helping our customers safeguard enterprise data.


## **Get the Technical Deep Dive: WD’s Post-Quantum Security White Paper**


Discover how WD is implementing NIST-approved PQC algorithms to protect enterprise storage devices from quantum threats. Our white paper ***Post-Quantum Security for Storage Devices: Why Now and How We’ll Help You Transition*** provides technical details on:
● **ML-DSA-87 implementation** for secure boot, firmware signing, and secure messaging
● **CNSA 2.0 compliance** and regulatory alignment (EU & U.S.)
**●** **Migration strategies** including dual-signing, PKI/HSM readiness, and rollback safeguards
**●** **Four-phase roadmap** to prepare your infrastructure for quantum-safe storage


[Download](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/collateral/white-paper/white-paper-post-quantum-cryptography-enterprise-hdd-security.pdf)


1. National Institute of Standards and Technology, “Module-Lattice-Based Digital Signature Standard,” August 13, 2024,[https://tinyurl.com/3wyt5n4e](https://tinyurl.com/3wyt5n4e)
2. National Institute of Standards and Technology, “NIST Releases First 3 Finalized Post-Quantum Encryption Standards,” August 13, 2024,[https://tinyurl.com/ydpjb7yv](https://tinyurl.com/ydpjb7yv)
3. Valverde, Diego. “Quantum Computing Forces Shift to Post-Quantum Security.” *Mexico Business News* , February 3, 2026,[https://tinyurl.com/4yyyxvbp](http://tinyurl.com/4yyyxvbp)
4. Edwards, John. “10 top priorities for CIOs in 2025,” *CIO.com,* January 13, 2025,[https://tinyurl.com/bdzwmjpe](https://tinyurl.com/bdzwmjpe)


[HDD](https://blog.westerndigital.com/tag/hdd/)[Innovation](https://blog.westerndigital.com/tag/innovation/)[Security](https://blog.westerndigital.com/tag/security/)
