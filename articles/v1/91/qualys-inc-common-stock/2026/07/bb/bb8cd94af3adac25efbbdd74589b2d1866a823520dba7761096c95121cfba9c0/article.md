---
schema_version: "1.0.0"
document_id: "bb8cd94af3adac25efbbdd74589b2d1866a823520dba7761096c95121cfba9c0"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/vulnerabilities-threat-research/2026/07/21/cve-2026-8933-snap-confine-local-privilege-escalation"
published_at: "2026-07-21T15:02:12+00:00"
first_seen_at: "2026-07-21T15:44:45.631952+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:b086ea10c908ccd0f20f099dc1f2d3b9841f8252cc399082a5626d8404a9628b"
---

# CVE-2026-8933: Local Privilege Escalation in Set-Capabilities snap-confine

#### Table of Contents


- What is the Attack Surface for CVE-2026-8933?
- Exploitation Mechanism
- Affected Versions & Remediation
- Technical Details of the CVE-2026-8933
- Acknowledgments
- Qualys QID Coverage for Detecting CVE-2026-8933
- Discover Vulnerable CVE-2026-8933 Assets with Qualys CyberSecurity Asset Management
- Automatically Patch CVE-2026-8933 with Qualys Patch Management


The[Qualys Threat Research Unit (TRU)](https://www.qualys.com/tru) has identified a Local Privilege Escalation (LPE) vulnerability in snap-confine (CVE-2026-8933). This flaw allows an unprivileged local user to gain full root access on default installations of Ubuntu Desktop 24.04, 25.10, and 26.04. The issue stems from a security hardening change that inadvertently introduced a race condition during sandbox initialization.


## **What is the Attack Surface for CVE-2026-8933?**


snap-confine is the enforcement component that builds the execution environment for snap applications. In recent Ubuntu releases, Canonical shifted snap-confine from a set-uid-root binary to a set-capabilities model to enforce least privilege. While intended to reduce the attack surface, this change means snap-confine now executes with the effective UID of the calling user while retaining near-root capabilities. During sandbox setup, the binary creates temporary directories and files under /tmp that are initially owned by the unprivileged user. Ownership is transferred to the root shortly after, but a narrow window remains during which the caller retains full control.


## **Exploitation Mechanism**


CVE-2026-8933 is rated High severity. The exploit leverages two concurrent race conditions.


First, an attacker mounts a FUSE filesystem over the temporary scratch directory immediately after creation. This bypasses the mount namespace isolation snap-confine applies later, keeping the directory accessible outside the sandbox.


Second, the attacker creates a symlink pointing to an arbitrary target file. When snap-confine attempts to create a sandbox file, the open() call follows the symlink and writes to the target. A second race condition allows the attacker to widen file permissions to 0666 before snap-confine calls fchown() to transfer ownership to root.


To bypass AppArmor confinement, the exploit targets the */run/udev/*** path, which permits read-write access. By dropping a malicious .rules file in */run/udev/rules.d/* and triggering a FUSE mount/unmount cycle, the attacker forces systemd-udevd to execute arbitrary commands as root.


## **Affected Versions & Remediation**


The following Ubuntu releases ship the vulnerable set-capabilities variant of snap-confine by default:


- Ubuntu Desktop 26.04
- Ubuntu Desktop 25.10
- Ubuntu Desktop 24.04 (systems updated to current snapd packages)


Organizations should apply the latest snapd updates immediately. Canonical has released patches through the Ubuntu Security Team following coordinated disclosure.


## **Technical Details** of the **CVE-2026-8933**


Full technical analysis, including source code references and proof-of-concept execution, is available in the Qualys Security Advisory:


[https://cdn2.qualys.com/advisory/2026/07/21/snap-confine-set-capabilities.txt](https://cdn2.qualys.com/advisory/2026/07/21/snap-confine-set-capabilities.txt)


## **Acknowledgments**


Qualys thanks the Ubuntu Security Team, in particular Eduardo Barretto and Zygmunt Krynicki at Canonical, for their rapid coordination and thorough patch review; and the linux-distros community, for their collaboration in hardening the snap ecosystem. Coordinated disclosure remains a cornerstone of responsible vulnerability research, and we value the trust and transparency built through these partnerships.


## **Qualys QID Coverage for Detecting CVE-2026-8933**


Qualys is releasing the QIDs as they become available. Please check the Qualys Vulnerability Knowledgebase for the full list of coverage.


## **Discover Vulnerable **CVE-2026-8933**** **Assets with Qualys CyberSecurity Asset Management**


Identify all internet-facing instances and internal endpoints running vulnerable snapd configurations using[CyberSecurity Asset Management 3.0](https://www.qualys.com/apps/cybersecurity-asset-management) . Use this QQL statement to scope your environment:


```text
operatingSystem. name: ["Ubuntu"]
```


```text
software:(name:"snapd")
```


CyberSecurity Asset Management maintains a catalog of hardware and software lifecycle data built and curated by a dedicated research team, covering over 5,500 software publishers and 300,000 software releases, with automated daily updates to all CyberSecurity Asset Management customers.


#### Free Trial


### Start a free trial of Qualys CyberSecurity Asset Management (CSAM).


[Try Today](https://www.qualys.com/free-trial-new/cyber-security-asset-management?utm_source=blog&utm_medium=website&leadsource=344578577)


## **Automatically Patch CVE-2026-8933 with Qualys Patch Management**


Patches for this vulnerability are available.[Qualys Patch Management](https://www.qualys.com/apps/patch-management) can automatically deploy those patches to vulnerable assets when they become available.


You can use “Patch Now,” found to the right of the vulnerability, to add this vulnerability to a patch job. Once patches are released, Qualys will identify the relevant patches for this vulnerability and automatically add them to a patch job. This will allow you to deploy those patches to vulnerable devices, all from the Qualys platform.
