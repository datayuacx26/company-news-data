---
schema_version: "1.0.0"
document_id: "2aa5740fb5c934ec3532074e73f47d3022fc26a015e2d57fbe10cdc12f165685"
company_key: "opera-limited-american-depositary-shares"
company: "Opera Limited"
source_id: "opera-limited-american-depositary-shares-news-import-d1faa49832aa"
canonical_url: "https://blogs.opera.com/security/2025/10/prompt-injection-in-opera-neon-rapid-response-through-responsible-disclosure/"
published_at: "2025-10-23T15:59:00+00:00"
first_seen_at: "2026-07-22T07:38:34.207512+00:00"
fetched_at: "2026-07-23T19:47:54.315596+00:00"
content_hash: "sha256:c1857718f792d1fbfa5582b99126c22aebe5d5f4ebfb37a19ad2a0af809f8159"
---

# Prompt injection in Opera Neon: Rapid response through responsible disclosure

Hi Opera users,


This week, we were able to address a real-world security scenario on Opera Neon thanks to the work of a security researcher team. The researchers reached out to us through our public bug bounty program to share a vulnerability they discovered in Opera Neon, our AI agentic browser that[recently launched](https://blogs.opera.com/news/2025/05/opera-neon-first-ai-agentic-browser) in Early Access for public testing. The issue was quickly validated and evaluated as low-risk due to Opera Neon’s built-in security features, and the reported attack scenario was addressed on the same day.


[Opera Neon’s built-in mitigations](https://blogs.opera.com/security/2025/10/opera-neon-understanding-agentic-browser-security) notwithstanding, this particular method of attack by nature could not pose a consistent threat to large numbers of Opera Neon users. However, we still take such reports seriously. That’s why our team moved immediately to investigate the researcher’s proof-of-concept (PoC), assess the threat, and implement a fix.


The case demonstrates how applied security practices like responsible disclosure and collaboration with researchers can provide more secure experiences for users in agentic browsers, much like their more traditional counterparts.


## Agentic browsing security


In the agentic internet, security is a key consideration, with good reason: AI Agentic browsers challenge the current security model of the internet – by their very nature, they open up new avenues of attack and previously unknown risks. That’s why we are also shifting our thinking into how we improve security for users under this new paradigm, while using existing best practices as the foundation on which to build these new, secure experiences.


At Opera, we have 30 years of experience in building safe and secure browsers. Our seasoned Security team has been collaborating with our product teams as well as with external security researchers to ensure that our products are secure against the latest threats, and that our users’ trust in us is justified.


Opera Neon, our AI agentic browser, has been built using this expertise, with our Security team performing hundreds of pen-tests and putting the browser through the wringer to help our developers identify and address key vulnerabilities. This is by no means the end of it, and work is ongoing – as expected, securing AI systems can quickly become tricky and unpredictable, which calls for wider and more extended testing.


That’s why we recently launched Opera Neon in Early Access public testing, aiming to have real-world usage insights and inviting early adopters and the broader security community to work with us in identifying gaps, determining risk, and applying solutions. You can learn more about[our efforts to keep Opera Neon secure in this very blog](https://blogs.opera.com/security/tag/agentic-browser-security) .


## Case overview


### Key info


Prompt injection vulnerabilities are one of the most common threats that AI agentic browsers face. Here, we will provide a summary of the prompt injection vulnerability identified by the security researchers and an analysis of the discovery process, Opera’s response, and the broader implications for AI-driven browsing security.


### Discovery and submission


- The report was submitted via Opera’s public bug bounty program on Bugcrowd. The report was filed on 14 October 2025, 21:50:33 UTC and included a detailed PoC demonstrating the exploit. The researchers also shared the date on which they planned to announce their findings publicly (21 October 2025).
- The initial submission was rejected by the system as a placeholder due to a lack of written down detailed reproduction steps, causing it to go unnoticed by the Opera team.
- The rejected submission was discovered by the Opera team on 20 October 2025, around 11:30:00 UTC, during routine manual checks of such tickets. Opera’s security and product teams immediately began investigating and reproducing the exploit.
- On 20 October 2025, at 16:30:39 UTC, Opera contacted the researcher, requesting a temporary delay in public disclosure to ensure user safety before a fix could be deployed.


### Nature of the vulnerability


- The vulnerability centered on how Opera Neon’s AI assistant processes webpage content. By embedding hidden or malicious instructions within a webpage, an attacker could manipulate the AI to perform unintended actions.
- The PoC demonstrated that attackers could:


- Access sensitive user data (such as Opera account details).
- Exfiltrate user information (e.g., email addresses) to attacker-controlled endpoints.


- Impact evaluation:


- Our team validated the PoC. While we were able to reproduce the attack, we could only do so with a 10% success rate. This is because AI models are non-deterministic, which means the attack cannot be reliably replicated.
- The attack’s complexity was evaluated as high. The user had to be lured to a specific malicious page, and then execute the right command that would trigger the hidden instructions. This further reduced the risk for a user accidentally falling victim to the attack.


### Opera’s response and mitigation steps


#### Immediate actions


Acknowledgment and triage: Opera’s Security team promptly acknowledged the report, initiated an internal investigation, and began reproducing the exploit.


#### Technical mitigation


- Within hours, a preliminary fix was developed by the engineering team, with additional hardening measures under consideration. By 20 October 2025, 22:17:32 UTC the fix was in production.
- The team evaluated whether the exploit required user interaction and considered further restrictions to minimize risk, e.g. by better separating the original user prompt from the untrusted website content that could lead to the indirect prompt injection.
- Opera Neon already has built-in mitigations that can contain such attacks, reducing the risk to users.
- On 21 October 2025, 03:29:37 UTC, the researchers confirmed that their PoC no longer worked following the release of the update.


###


### Moving forward


#### Transparency and communication


Opera’s Security team immediately mobilized upon discovery and adhered to our standard processes for addressing vulnerabilities. While evaluating the PoC, the product team was notified and started working on a fix, and relevant internal teams were brought in as well to support as needed.


Staying true to our values of transparency and trust, we have summarized the vulnerability and the work we did to address it in this blog post. We hope this will help raise awareness of the significance of the ongoing work in creating secure and reliable experiences in agentic browsing.


#### Lessons and recommendations


When it comes to users, we recommend exercising caution when accessing untrusted websites using an AI browser – malicious websites are just as much a risk as they are when browsing on a regular browser. Extra caution is recommended when using products in an Early Access phase.


When it comes to security researchers, we believe that knowledge sharing and responsible disclosure are some of the strongest weapons available in the fight for better product security and user safety. We welcome more researchers coming to us with their findings and we look forward to collaborating with them.


When it comes to developers, we are learning every day that many assumptions can be flawed or incomplete when charting these new waters. Developers, both in our teams and those working on similar products elsewhere have to be able to balance the possibilities and potential of AI assistants capable of executing actions, with the need for control and protection of the user.


If you’d like to report a security issue, you can do so through our public and private[Bug Bounty](https://security.opera.com/bug-bounty/) programs. Out-of-scope security issues can be[submitted directly to us](https://security.opera.com/report-security-issue/) and are subject to our[Vulnerability Policy](https://security.opera.com/policy/) .To report malicious or suspicious activity related to Opera products and services, please use the[Report Fraud](https://security.opera.com/report-fraud/) page.


[Michael Tegos](https://blogs.opera.com/security/author/mtegos/)[Product Privacy & Security Advocate at Opera](https://blogs.opera.com/security/author/mtegos/)


[Agentic Browser Security](https://blogs.opera.com/security/tag/agentic-browser-security/)


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
