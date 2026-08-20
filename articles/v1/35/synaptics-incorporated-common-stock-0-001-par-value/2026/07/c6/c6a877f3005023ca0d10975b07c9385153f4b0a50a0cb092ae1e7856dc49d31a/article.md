---
schema_version: "1.0.0"
document_id: "c6a877f3005023ca0d10975b07c9385153f4b0a50a0cb092ae1e7856dc49d31a"
company_key: "synaptics-incorporated-common-stock-0-001-par-value"
company: "Synaptics Incorporated Common Stock $0.001 Par Value"
source_id: "synaptics-incorporated-common-stock-0-001-par-value-rss-37ccc1913767"
canonical_url: "https://www.synaptics.com/security/psirt"
published_at: "2026-07-10T22:46:19+00:00"
first_seen_at: "2026-07-24T03:02:35.512628+00:00"
fetched_at: "2026-07-28T21:41:43.051336+00:00"
content_hash: "sha256:7e77cfc7de30929ce49e217e67ecbd4ffa142404a9a0b601b139383f1c2ea4b7"
---

# PSIRT

PSIRT


# Product Security Incident Response


### Overview


A coordinated path for product security issues


Synaptics PSIRT coordinates reports of potential product security vulnerabilities with internal teams, customers, partners, and researchers, and helps manage mitigations, fixes, workarounds, and related communications. Disclosure timing may depend on customer integration, field update paths, third-party dependencies, and ecosystem coordination.


---


### Scope


What PSIRT handles


PSIRT is intended for security vulnerability reports involving Synaptics products and related security-sensitive product information. General technical support, commercial requests, and non-product security issues should use the appropriate Synaptics support or contact channel.


**In scope**


- Potential security vulnerabilities in Synaptics products reported by researchers, customers, partners, suppliers, or internal teams.
- Security-impacting behavior in Synaptics product features, update mechanisms, configuration, or default settings.


**Out of scope for this mailbox**


- Commercial solicitation, sales inquiries, or partnership requests.
- Routine product support or integration assistance.
- Website, enterprise IT, phishing, or corporate information security issues unless they are directly tied to Synaptics product security.
- Reports involving personal data submitted without consent or appropriate safeguards.


---


### Report a Vulnerability


How to contact Synaptics PSIRT


If you believe you have identified a potential security vulnerability in a Synaptics product, email the Synaptics Product Security Incident Response Team atPSIRT@synaptics.com . Please submit reports in English where possible.


Vulnerability information can be sensitive. Do not include personal data, customer confidential information, or third-party confidential information unless you have authorization and appropriate safeguards are in place.


To help Synaptics validate and coordinate the report efficiently, please include the relevant information described below under "Vulnerability Details".


**Vulnerability Details**


- Product name, part number, chipset, module, software, firmware, driver, or SDK.
- Affected version, build number and release date.
- Technical description and suspected vulnerability class.
- Steps to reproduce, proof-of-concept details, logs, traces, or test setup.
- Your contact information and preferred communication method.
- Whether the issue has been shared with others.
- Placeholder for publication decision: add a Synaptics PSIRT PGP/GPG public key here if approved. If no public key is available, replace this block with approved instructions for secure exchange of sensitive materials after PSIRT acknowledgement.


**PGP**


- Fingerprint: \[PLACEHOLDER FOR KEY\]
- \[LINK TO KEY FILE\]


---


### Vulnerability Handling Process


Synaptics PSIRT workflow


Synaptics PSIRT receives, validates, and assesses reported product security vulnerabilities, coordinates remediation and mitigation activities with engineering teams, and communicates security guidance, advisories, and updates with customers, partners, reporters, regulators, and the public as appropriate in accordance with established product security processes.


1


**Reporting & Acknowledgement**


PSIRT receives the vulnerability report, acknowledges receipt where contact information is available, and opens an internal tracking record.


2


**Validation & Risk Assessment**


Synaptics determines whether the report is in scope, assigns internal owners, evaluates reproducibility, affected products and versions, exploitability, severity, and customer impact.


3


**Remediation**


Engineering teams define, implement, test, and validate corrective actions, workarounds, firmware or software updates, or other mitigations.


4


**Disclosure & Communication**


PSIRT coordinates customer/partner communication, reporter updates, regulatory escalation where required, public advisories or bulletins where appropriate, and internal closure records.


---


### Coordinated Disclosure


Balancing transparency, customer readiness, and end-user protection


Synaptics works to coordinate vulnerability disclosure in a way that gives affected customers and ecosystem partners time to evaluate and deploy appropriate mitigations before sensitive technical details are made public.


Synaptics generally strives to address vulnerabilities and other issues within the industry-standard time of 90 days after such vulnerabilities or issues are reported. Additional time may be required where many OEMs, partners, third-party components, or fielded systems are affected and a coordinated response is necessary.


---


### Policies


Product security policies


Current policy


[Vulnerability Disclosure Policy](https://www.synaptics.com/vulnerability-disclosure-policy)


---


### Security Bulletins


Where customers can find security guidance


When Synaptics issues a security advisory, the advisory typically identifies affected Synaptics products, known affected versions where available , severity information, and the appropriate path for obtaining a fix or workaround.


Product Title Publish Date CVE ID


Biometrics[Synaptics Fingerprint Driver: Co-Installer Privilege Escalation](https://www.synaptics.com/sites/default/files/2025-12/fingerprint-driver-co-installer-security-brief-2025-12-01.pdf) Dec 1, 2025 CVE-2025-11772


Audio[Synaptics Conexant Audio Driver Security Brief - March 10, 2025](https://www.synaptics.com/sites/default/files/2025-03/audio-driver-security-brief-2025-03-11.pdf) Mar 11, 2025 CVE-2024-9157


Biometrics[Synaptics Fingerprint Driver: Encryption key](https://www.synaptics.com/sites/default/files/2024-01/fingerprint-driver-encryption-key-security-brief-2024-01-26.pdf) Jan 26, 2024 CVE-2023-6482


Biometrics[Synaptics Fingerprint Drivers HSAService Security Brief](https://www.synaptics.com/sites/default/files/2023-10/fingerprint-driver-HSAService-security-brief-2023-10-13.pdf) Oct 20, 2023 CVE-2023-5447


DisplayLink Windows[DisplayLink Driver for Windows Security Brief - Oct 11, 2023](https://www.synaptics.com/sites/default/files/nr-154525-tc-synaptics_displaylink_windows_driver_security_brief_-_oct2023.pdf) Oct 11, 2023 CVE-2023-4936


Biometrics[Synaptics Fingerprint Drivers SGX Security Brief](https://www.synaptics.com/sites/default/files/2020-12/fingerprint-driver-SGX-security-brief-2020-07-14.pdf) Jul 14, 2020 CVE-2019-18619


Biometrics[Synaptics VFS75xx Fingerprint Sensors Security Brief](https://www.synaptics.com/sites/default/files/2020-12/fingerprint-sensor-VFS7500-security-brief-2020-07-14.pdf) Jul 14, 2020 CVE-2019-18618


Audio[Audio Driver Security Brief - June 9, 2020](https://www.synaptics.com/sites/default/files/2020-12/audio-driver-security-brief-2020-06-09.pdf) Jun 9, 2020 CVE-2020-8337


Far-field Voice DSPs[Sound Device Security Brief - Apr 15, 2019](https://www.synaptics.com/sites/default/files/2020-11/sound-device-security-brief-20190415.pdf) Apr 15, 2019 CVE-2019-9730


TouchPad Family[TouchPad Security Brief - Jan 24, 2019](https://www.synaptics.com/sites/default/files/2020-11/touchpad-driver-security-brief-20190124.pdf) Jan 24, 2019 CVE-2018-15532


TouchPad Family[TouchPad Security Brief - Dec 14, 2017](https://www.synaptics.com/sites/default/files/2020-11/touchpad-driver-security-brief-20171214.pdf) Dec 14, 2017 CVE-2017-17556


---


### EU CRA Readiness


Product security response and regulatory reporting readiness


Synaptics is working toward EU CRA compliance, including meeting the September 11, 2026 deadline for vulnerability and incident reporting and achieving full compliance by December 11, 2027.


Sheryl.Tamashiro


Fri, 07/10/2026 - 15:46
