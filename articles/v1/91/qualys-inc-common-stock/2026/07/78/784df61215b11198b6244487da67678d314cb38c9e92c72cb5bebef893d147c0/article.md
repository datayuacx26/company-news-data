---
schema_version: "1.0.0"
document_id: "784df61215b11198b6244487da67678d314cb38c9e92c72cb5bebef893d147c0"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/vulnerabilities-threat-research/2026/07/22/refluxfs-a-linux-kernel-local-privilege-escalation-to-root-in-xfs-cve-2026-64600"
published_at: "2026-07-22T16:23:46+00:00"
first_seen_at: "2026-07-22T16:58:27.524166+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:8969a83054daadb1dd3e3f6d8f5460a8fc835d3e31e88a601a415c920d554acc"
---

# RefluXFS: A Linux Kernel Local Privilege Escalation to Root in XFS (CVE-2026-64600)

#### Table of Contents


- Executive summary
- Discovery process and research methodology
- Understanding the Potential Impact of the RefluXFS Vulnerability
- Watch a RefluXFS Proof-of-concept Demonstration
- Affected Systems & Prerequisites
- How does RefluXFS work?
- Why Existing Mitigations Dont Stop It
- Technical Details of the RefluXFS vulnerability:
- Acknowledgments
- Qualys QID Coverage for Detecting theRefluXFS vulnerability (CVE-2026-64600):
- Conclusion


## **Executive summary**


[Qualys Threat Research Unit (TRU)](https://www.qualys.com/tru) identified CVE-2026-64600, a race condition in the Linux kernel’s XFS filesystem copy-on-write path. An attacker with an ordinary local account can exploit this race condition to overwrite protected files on disk and gain host root privileges on affected systems, including deployments running SELinux in Enforcing mode.


This discovery emerged from a structured research initiative between Qualys and Anthropic, where we integrated Claude Mythos Preview into our manual audit workflow to accelerate our research while maintaining strict human oversight.


Using this vulnerability, a process running as an ordinary, unprivileged user can trigger the flaw and gain the ability to overwrite any readable file on an XFS volume at the block layer. This primitive converts directly into host root privileges. Exploitation is highly reliable and leaves no kernel log output. The vulnerability affects any Linux distribution that ships an XFS root filesystem with reflink enabled. This includes default installations of major enterprise platforms such as RHEL, Oracle Linux, Amazon Linux, and Fedora.


This vulnerability has been present since kernel version 4.11 (circa 2017) and, based on analysis with[Qualys CyberSecurity Asset Management](https://www.qualys.com/apps/cybersecurity-asset-management) , potentially impacts over 16.4 million systems worldwide, requiring immediate attention due to its high exploitability and lack of kernel logging.


Immediate kernel patching is recommended to neutralize this vulnerability. Exploitation succeeds consistently under standard hardening settings, and the on-disk modification survives a system reboot.


Vendor-fixed kernels are now available and being backported to enterprise distributions. Organizations should prioritize patching exposed and multi-tenant systems and ensure a reboot to verify the update. As of now, there are no reliable or practical mitigations or temporary configuration changes available.


We rate this as an emergency priority because exploitation could begin from ordinary local privileges. The vulnerability is present in standard enterprise kernel builds, and a successful exploitation provides host root. The exploitation works under common kernel hardening settings, and fixed kernels are available.


RefluXFS is less about what AI can do than how it’s used responsibly. Capable models have been broadly available for some time; direction, not capability, was the constraint. The lesson isn’t that AI can be aimed at software to produce exploits; it’s that AI, paired with expert judgment, helps defenders find and close serious flaws before they’re abused.


## **Discovery process and research methodology**


For this research, we tasked Claude Mythos Preview with hunting for a Dirty COW–style race condition in the Linux kernel, iteratively refining our prompts to narrow its focus toward race conditions in the core memory-management and filesystem directories. After several iterations, the model identified a race condition in the XFS filesystem and generated a functional proof-of-concept local privilege escalation. Our security researchers then took over: we reviewed the model’s reasoning, reproduced the exploit, and independently verified every technical claim before coordinating disclosure with upstream maintainers. The model also produced an initial draft of the advisory, which our team validated and corrected against our own testing. This human-validated, AI-accelerated approach let us surface a complex kernel race condition while holding to the strict accuracy and responsible-disclosure standards expected; every finding here cleared the same evidence bar we apply to any Qualys security advisory.


---


**Find out more about Qualys’ participation in Anthropic’s Project Glasswing.**


[Read More](https://blog.qualys.com/product-tech/2026/06/05/advancing-cybersecurity-in-the-age-of-frontier-ai-qualys-steps-into-project-glasswing)


---


## **Understanding the Potential Impact of the RefluXFS Vulnerability**


An unprivileged local user can overwrite the on-disk contents of any readable file on a reflink-enabled XFS volume. Our proof-of-concept reliably gains host root privileges by surgically modifying /etc/passwd or SUID-root binaries. Changes persist across reboots, leave no kernel log output, and bypass standard file metadata checks.


While CVE-2026-64600 itself is strictly local, successful exploitation on a compromised host could enable attackers to establish persistence, manipulate credentials, or facilitate lateral movement within a network.


## **Watch a RefluXFS Proof-of-concept Demonstration**


The following proof-of-concept shown in the video below shows the RefluXFS vulnerability on a default RHEL 10.2 deployment. Starting as an unprivileged local user with no administrative rights, the exploit triggers the race condition to silently overwrite a protected system file at the block layer. Within seconds, the root account’s password protection is stripped, granting immediate, passwordless root access. The modification persists across reboots and leaves no kernel log artifacts.


RefluXFS PoC execution on RHEL 10.2. Unprivileged user escalates to root by exploiting the XFS reflink race condition.


## **Affected Systems & Prerequisites**


### **Vulnerability Scope & Impact:**


The RefluXFS vulnerability affects systems meeting **all three** of the following criteria:


1. **Kernel Version:** Running Linux kernel v4.11 or later (2017) without the specific security patch.
2. **Filesystem Configuration:** Utilizing an XFS filesystem with reflink=1 in its superblock.
3. **File Structure:** The filesystem must contain both a high-value target (a root-owned configuration file or SUID-root binary) and a directory writable by an unprivileged local user.


### **Affected Distributions:**


This vulnerability has been present in every mainline and stable kernel since 2017. It requires no special capabilities or non-default configurations. The distributions listed below are those confirmed to be affected; this is not an exhaustive list, and other distributions, particularly those derived from RHEL, are likely vulnerable as well:


- RHEL 8, 9, and 10
- CentOS Stream 8, 9, and 10
- Oracle Linux 8, 9, and 10
- Rocky and AlmaLinux 8, 9, 10
- CloudLinux 8, 9, and 10
- Amazon Linux 2023 and Amazon Linux 2 AMIs from December 2022 onward
- Fedora Server 31+
- Debian, Ubuntu, and SUSE do not use XFS by default, but become exposed if an administrator manually selects XFS during installation, and reflink=1.


For those running affected distributions, we recommend that you apply the latest security updates via your distribution’s advisory and perform a system reboot.


## **How does RefluXFS work?**


RefluXFS is a race condition in the Linux kernel’s XFS filesystem copy-on-write (CoW) path. It allows an unprivileged local user to overwrite the on-disk contents of any readable file on a reflink-enabled XFS volume.


The vulnerability triggers when two concurrent \`O_DIRECT\` writes target the same reflinked file. XFS uses CoW to handle writes to shared blocks: it allocates a new private block, remaps the file, and decrements the original block’s reference count. To avoid deadlocks, the kernel drops its inode lock while waiting for transaction log space. During this window, a second writer can complete its own CoW cycle, remapping the file to a new block and dropping the original block’s reference count to one.


When the first writer re-acquires the lock, it re-checks the reference count using a stale physical block address captured before the lock was dropped. Seeing a count of one, it incorrectly assumes the block is private and proceeds to write directly to the original file’s physical block. Because \`O_DIRECT\` bypasses kernel page cache and lacks revalidation, the write persists to disk, overwriting the target file. The change survives reboots, leaves no kernel log output, and bypasses standard file metadata checks.


## **Why Existing Mitigations Don’t Stop It**


If you’re wondering whether your usual kernel hardening will catch this, the short answer is no, and it’s worth understanding why. The flaw lives at the filesystem allocation layer, which sits below or outside the reach of the defenses most systems rely on.


Memory-protection features like KASLR, SMEP, and SMAP are aimed at very different attack surfaces and simply don’t apply to block-layer writes. Kernel lockdown doesn’t help either, since it places no restrictions on O_DIRECT or FICLONE for unprivileged users. SELinux doesn’t block the affected path in testing, and seccomp profiles are no barrier as long as they permit write and ioctl, which ordinary profiles do.


The isolation mechanisms you might lean on in containerized environments fall short for the same underlying reason: user-namespace restrictions, container capability limits, and hardened allocators all operate at layers the flaw never touches. Because the vulnerability works through normal filesystem allocation, it slips past defenses designed to police memory, system calls, and privilege boundaries.


**Required Action:** This isn’t a vulnerability you can harden around, isolate, or live-patch. SELinux, container boundaries, memory protections, and even active threat monitoring all fall short because the flaw operates at the filesystem allocation layer, below every traditional defense. Immediate kernel patching and a full reboot are the only reliable mitigations.


## **Technical Details of the RefluXFS vulnerability:**


You can find the technical details of this vulnerability at:[https://cdn2.qualys.com/advisory/2026/07/22/RefluXFS.txt](https://cdn2.qualys.com/advisory/2026/07/22/RefluXFS.txt)


## Acknowledgments


We thank the XFS maintainers and the Linux kernel security team, particularly Carlos Maiolino, Greg Kroah-Hartman, Darrick J. Wong, Linus Torvalds, David Woodhouse, and Willy Tarreau, for their prompt coordination and rigorous patching efforts. We also thank the Anthropic team for their continued collaboration and thoughtful engagement throughout this work.


We extend our appreciation to the linux-distros mailing list community, particularly Sam James, Caryl Takvorian, Denis Pilipchuk, Salvatore Bonaccorso, and Solar Designer, for their constructive feedback during the coordinated disclosure process.


## **Qualys QID Coverage for Detecting the RefluXFS vulnerability (CVE-2026-64600** ) **:**


Qualys is releasing the QIDs in the table below as they become available.


Qualys customers can use QID 45097 – Linux Kernel Version Running to identify the Linux kernel version active on the system at the time of the scan.


**QID** **Title** **VulnSigs Version**


6600012 Oracle Enterprise Linux Security Update for kernel (ELSA-2026-39494) VULNSIGS-2.6.656-2


6053109 Red Hat Update for kernel (RHSA-2026:41229) VULNSIGS-2.6.656-2


6053104 Red Hat Update for kernel (RHSA-2026:41062) VULNSIGS-2.6.656-2


6052390 Red Hat Update for kernel (RHSA-2026:41063) VULNSIGS-2.6.654-2


6052332 Red Hat Update for kernel (RHSA-2026:40425) VULNSIGS-2.6.654-2


944816 AlmaLinux Security Update for kernel (ALSA-2026:39179) VULNSIGS-2.6.653-2


6052326 Red Hat Update for kernel (RHSA-2026:39984) VULNSIGS-2.6.653-2


963680 Rocky Linux Security Update for kernel (RLSA-2026:39179) VULNSIGS-2.6.653-2


6052307 Red Hat Update for kernel security (RHSA-2026:39494) VULNSIGS-2.6.652-2


289032 Fedora Security Update for kernel (FEDORA-2026-085c9e92a4) VULNSIGS-2.6.651-2


6052294 Red Hat Update for kernel (RHSA-2026:39179) VULNSIGS-2.6.652-2


6052291 Red Hat Update for kernel-rt (RHSA-2026:39180) VULNSIGS-2.6.652-2


387841 RefluXFS: Local Privilege Escalation via XFS reflink direct-I/O race TBD (Noon PST)


944834 AlmaLinux Security Update for kernel (ALSA-2026:39494) VULNSIGS-2.6.653-2


963688 Rocky Linux Security Update for kernel (RLSA-2026:39494) VULNSIGS-2.6.653-2


963682 Rocky Linux Security Update for kernel-rt (RLSA-2026:39180) VULNSIGS-2.6.653-2


944802 AlmaLinux Security Update for kernel-rt (ALSA-2026:39180) VULNSIGS-2.6.652-2


**Immediate Action Required:** Scan all Linux endpoints using QID(s). Prioritize patching for internet-facing assets.


Please check the Qualys Vulnerability Knowledgebase for the full list of coverage for this vulnerability.


## Conclusion


RefluXFS demonstrates the value of applying advanced technology within a disciplined, expert-led security research process. The significance lies not in the technology alone, but in how it can help researchers identify serious flaws earlier, validate their impact, and support coordinated disclosure before attackers can exploit them. A resilient security posture ultimately depends on continuous visibility and timely action: we encourage customers to discover and understand exposed assets via CSAM and VMDR to identify and prioritize vulnerabilities across those assets, and use Patch Management to remediate them before the exploitation window closes.


---


---
