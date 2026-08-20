---
schema_version: "1.0.0"
document_id: "70324c39b14946bc9b6aac8e11d718be92c9704346a0201f52bfa073e119c895"
company_key: "opera-limited-american-depositary-shares"
company: "Opera Limited"
source_id: "opera-limited-american-depositary-shares-news-import-d1faa49832aa"
canonical_url: "https://blogs.opera.com/security/2026/04/opera-security-responsible-disclosure-osslsigncode-quill/"
published_at: "2026-04-17T09:56:54+00:00"
first_seen_at: "2026-07-22T07:38:34.207512+00:00"
fetched_at: "2026-07-23T19:47:54.315596+00:00"
content_hash: "sha256:69b4231a39d4ba9dfc4e79d04f74980b45fb50b30829a516012a9e1b5bde27b8"
---

# How Opera’s Security team helps make the web safer through responsible disclosure

Hi everyone!


At Opera, we have 30 years of experience in building safe and secure browsers. Our seasoned Security team collaborates internally as well as with external researchers to ensure our products protect you from the latest threats. Such collaborations among security experts and communities is vital to ensure that the software you use is safe from threats, and that vulnerabilities are detected and addressed quickly and efficiently. This is particularly important as the entire web relies on a massive foundation of open-source tools.


So, knowledge sharing and responsible disclosure are some of the strongest weapons available in the fight for better product security and user safety. That’s why our Security team regularly audits popular open-source projects. Recently, we analyzed two vital supply-chain tools: **osslsigncode** and **Quill** . In the process, we helped patch several vulnerabilities, contributing to making these tools safer for everyone.


The vulnerabilities we identified were also assigned CVEs, meaning that they are now part of the Common Vulnerabilities and Exposures (CVE) list, and thus available to researchers and security experts. This is important for knowledge sharing and improving the web’s defenses and resilience against bad actors.


So here is our Security Engineer, **Antoni Klajn** , walking us through what was found and fixed.


## Securing osslsigncode


**osslsigncode** is a widely used utility for signing and verifying Windows PE files, MSI installers, and cabinet archives. For anyone relying on Authenticode signing, this tool is a critical part of the trust chain.


Our team identified several memory handling issues that could compromise the tool’s stability and security:


- **Critical Buffer Overflow:** We discovered a systematic stack buffer overflow affecting all file format handlers. When processing signatures, the tool could be forced to copy untrusted digest data into a fixed-size stack buffer without proper bounds checking. While such flaws could potentially enable Remote Code Execution (RCE) by overwriting the return address on the stack , it is important to note that modern operating systems employ robust mitigations, like stack canaries and ASLR, that significantly limit the practical exploitability of these bugs today.
- **Integer Underflow & Out-of-Bounds Reads:** We also discovered vulnerabilities in the page hash calculation logic where specially crafted files could trick the tool into reading memory it shouldn’t access. These issues typically cause the application to crash, but in certain conditions could potentially allow an attacker to extract small pieces of sensitive data from the system’s memory.


The process of responsible disclosure here was smooth and collaborative, demonstrating what the open-source spirit is all about. Upon receiving our report, the project’s author responded within 24 hours and moved immediately to implement security fixes. While some vulnerabilities were patched directly by the project’s author, our Security team actively contributed code for the remaining issues. We submitted these patches to the author, who reviewed and merged our contributions, ensuring the tool is now more resilient.


## Patching Quill


We also investigated **Quill** , an open-source tool used by developers to interact with Apple’s notarization APIs. Our analysis uncovered a few ways the tool could be manipulated:


- **Server-Side Request Forgery (SSRF):** We discovered that the application was fetching submission logs from a URL provided in an API response without validating that the URL actually pointed to a trusted Apple domain. If an attacker could somehow control that API response, they could trick Quill into making requests to internal network resources.
- **Memory Exhaustion:** We found two paths that could cause Quill to crash by running out of memory. First, the HTTP client read API response bodies without any size limits, meaning a malicious server could send an endlessly large response. Second, when parsing Mach-O binary files, the code allocated memory buffers based on size values read directly from the untrusted file without validation. A tiny, malformed file could trick the tool into trying to allocate gigabytes of memory.


We responsibly disclosed these findings to Quill’s maintainers, who quickly addressed the issues to keep their users protected.


We would like to thank the developers at these projects for working with us to patch these vulnerabilities. Collaboration and communication among the security community is key for keeping the web safe!


## A safer internet for all


The work to keep the internet secure never ends. Together with our colleagues and fellow security experts, we learn and improve every day as new security challenges rear their heads. By sharing our browser security expertise with the broader open-source community, we hope to contribute to the culture of vigilance and the security-first mindset that helps ensure that you can browse the internet with peace of mind.


As always, stay safe out there!


[Opera Team](https://blogs.opera.com/security/author/operateam/)


---


### You deserve a
better browser


Faster, safer and smarter than default browsers. Fully-featured for privacy, security, and so much more.


[Download now](https://www.opera.com/download)


---


[News,](https://blogs.opera.com/security/category/news/)[Security](https://blogs.opera.com/security/category/security/)


## [Security fix: Addressing a GX mods vulnerability](https://blogs.opera.com/security/2026/07/security-fix-gx-mods-vulnerability/)


July 3rd, 2026


## [Here’s how Opera’s Paste Protect guards you natively against clipboard attacks](https://blogs.opera.com/security/2026/07/how-opera-paste-protect-guards-against-clipboard-attacks/)


July 2nd, 2026


## [Update your browser: Security fix for Chrome zero-day CVE-2026-11645](https://blogs.opera.com/security/2026/06/update-your-browser-security-fix-for-chrome-zero-day-cve-2026-11645/)


June 11th, 2026


## [Security fix: Addressing a low-impact Pinboards vulnerability](https://blogs.opera.com/security/2026/06/security-fix-low-impact-pinboards-vulnerability/)


June 9th, 2026


[Security](https://blogs.opera.com/security/category/security/)


## [Why browsing with Opera’s VPN is safer](https://blogs.opera.com/security/2026/05/opera-vpn-is-safe/)


May 29th, 2026


[Security](https://blogs.opera.com/security/category/security/)


## [How we keep Opera users and products safe: Inside the role of Head of Security](https://blogs.opera.com/security/2026/05/meet-opera-head-of-security/)


May 8th, 2026


---


Your subscription could not be saved. Please try again.


Your subscription has been successful.


# You deserve a better browser


Opera's free VPN, Ad blocker, and Flow file sharing. Just a few of the must-have features built into Opera for faster, smoother and distraction-free browsing designed to improve your online experience.


[Download now](https://www.opera.com/download)
