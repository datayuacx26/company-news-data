---
schema_version: "1.0.0"
document_id: "85a3afeb9391121ca6cf91ed0c8f2d9040c4e77848be8eaa19af128714b77399"
company_key: "opera-limited-american-depositary-shares"
company: "Opera Limited"
source_id: "opera-limited-american-depositary-shares-news-import-d1faa49832aa"
canonical_url: "https://blogs.opera.com/security/2026/07/security-fix-gx-mods-vulnerability/"
published_at: "2026-07-03T15:27:46+00:00"
first_seen_at: "2026-07-22T07:38:34.207512+00:00"
fetched_at: "2026-07-23T19:47:54.315596+00:00"
content_hash: "sha256:02fcb621e9e75a574cdf4bd5e378ae42aec67b585865c00c4e25eaf443ecafae"
---

# Security fix: Addressing a GX mods vulnerability

Hi Opera users,


Recently, an independent security researcher responsibly disclosed a vulnerability in Opera GX. The vulnerability, which has since been patched, took advantage of our mods functionality. After investigating the researcher’s findings, we took immediate action, and a fix is already in place. If you are running Opera GX version 130.0.5847.89 and later, you have received the fix as well.


Browser security is something we take very seriously at Opera. And part of being serious about security is being transparent. We have[written before](https://security.opera.com/en/policy/#security-reports) about how Opera works with independent security researchers who work hard to identify vulnerabilities in our products and alert us to them so we can fix them before they can be exploited by bad actors.


After patching, some researchers choose to reveal the vulnerabilities they found and their methods, so that the broader security community can benefit from the shared knowledge. This is called responsible disclosure, and it is common practice across the software industry.


It’s also the reason why things like bug bounties exist – so that these researchers can be rewarded for their efforts in helping to keep the software we use every day safe and secure. Opera’s own bug bounty program can be found[here](https://bugcrowd.com/opera) .


## Vulnerability analysis


Researchers zhero_ and inzo_ discovered that, under specific conditions, a third-party website could be set up to force the installation of a mod on Opera GX. As mods on GX, especially on the GX Store, can be applied without additional confirmation steps, the researchers found they could take advantage of this to force a malicious mod to be installed upon arrival to the malicious page.


Once installed, such a mod could use advanced Cascading Style Sheets (CSS) injection rules to read specific data attributes on pages you visit – such as a username or email address – and send that data back to an attacker’s server by loading unique, tracker-like URLs.


As a proof of concept, the researchers demonstrated how a malicious site could force-install a mod, automatically redirect a logged-in user to a Google account page, and use CSS selectors to exfiltrate the user’s Gmail address.


## Triage and fix


The vulnerability was initially reported through our bug bounty program on Bugcrowd. Once our team had an opportunity to triage the bug submission, we moved to address the vulnerability. We updated our mod installation pipeline to ensure that no such mod can be downloaded and enabled without explicit user interaction and clear confirmation.


The issue was fixed as of Opera GX version 130.0.5847.89. If your Opera GX browser is up-to-date, you have already received the patched version.


After thoroughly investigating our systems and traffic, we are quite confident that the vulnerability was never exploited in the wild. The attack was not only complicated to set up, but for it to work, a very specific set of circumstances was required, which made it harder for a user to be affected.


Specifically, the user would have to be goaded into visiting the specific malicious website that would have to have been set up for this purpose, find themselves with a fresh mod installed, and ignore the corresponding message (which also includes a button to remove the mod), giving time to the redirect to go forward.


## What happens next


We want to extend our sincere thanks to the security researchers zhero_ and inzo_ who discovered and reported the issue to us. Their innovative thinking and collaborative approach show how important independent researchers are to the effort of keeping the web safe for users.


If you notice any vulnerabilities in Opera, please reach out to us – our[bug bounty program](https://bugcrowd.com/opera) is the most effective way to do that. You can also[get in touch](https://security.opera.com/en/) with our Security team directly. And of course, make sure to have the latest updates in your software installed; this is the best way to ensure you are protected!


As always, stay safe out there!


[Michael Tegos](https://blogs.opera.com/security/author/mtegos/)[Product Privacy & Security Advocate at Opera](https://blogs.opera.com/security/author/mtegos/)


---


### You deserve a
better browser


Faster, safer and smarter than default browsers. Fully-featured for privacy, security, and so much more.


[Download now](https://www.opera.com/download)


---


[News,](https://blogs.opera.com/security/category/news/)[Security](https://blogs.opera.com/security/category/security/)


## [Here’s how Opera’s Paste Protect guards you natively against clipboard attacks](https://blogs.opera.com/security/2026/07/how-opera-paste-protect-guards-against-clipboard-attacks/)


July 2nd, 2026


[Privacy](https://blogs.opera.com/security/category/privacy/)


## [How does Opera make money? An explainer on monetization](https://blogs.opera.com/security/2026/06/how-does-opera-make-money-monetization-explainer/)


June 22nd, 2026


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
