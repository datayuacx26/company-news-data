---
schema_version: "1.0.0"
document_id: "a9099f436d179b197cbfd1bb43435bbb5419bdc28042fa78b15ba7897981cb01"
company_key: "opera-limited-american-depositary-shares"
company: "Opera Limited"
source_id: "opera-limited-american-depositary-shares-news-import-d1faa49832aa"
canonical_url: "https://blogs.opera.com/security/2025/10/gamemaker-security-update-cve-2025-12501/"
published_at: "2025-10-30T15:34:17+00:00"
first_seen_at: "2026-07-22T07:38:34.207512+00:00"
fetched_at: "2026-07-23T19:47:54.315596+00:00"
content_hash: "sha256:24f08739aafaf2cdb3166256dbd1efd66de0eaeacd17c96a8b88f3feef60c396"
---

# GameMaker security update: Patch now to prevent DoS attacks

Today we’re looking at a vulnerability discovered in GameMaker, the game development tool that streamlines and simplifies game dev for all users, regardless of skill level.


The vulnerability in question (CVE-2025-12501) can lead to application crashes through denial-of-service attacks (DoS). Our team was notified about the vulnerability through an external researcher as part of our ongoing collaboration with security experts. It was quickly addressed, and resolved in a timely fashion.


GameMaker users who use the network_create_server() function in their projects are urged to update and recompile immediately.


# The vulnerability: CVE-2025-12501 explained


The core of this vulnerability lies within the network_create_server() function, a commonly used component for implementing network functionality in GameMaker projects. Specifically, it has been found to be susceptible to an integer overflow crash when processing malformed or oversized packets. An attacker could exploit this by sending a specially crafted packet to the server, causing the GameMaker application to terminate unexpectedly.


This type of crash can lead to denial-of-service (DoS) attacks, where legitimate users are prevented from accessing or using the game or application.


# Who is affected?


Any GameMaker project compiled with runtime below 2024.14.0 that utilizes the network_create_server() function for network communication is potentially at risk.


Developers are advised to review their project codebases to identify instances of network_create_server() usage.


# Action required: Update your GameMaker IDE


GameMaker has released an update to address this vulnerability. To protect your projects and users, we strongly recommend updating your GameMaker IDE to the latest version (2024.14.0 or higher).


**How to Update:**


1. **Open GameMaker Studio 2/GameMaker:** Launch your GameMaker IDE.
2. **Check for Updates:** Navigate to “Help” -> “Check for Updates” or refer to the “IDE and Runtime Versions” section in your preferences.
3. **Install the Latest IDE:** Follow the prompts to download and install the latest stable GameMaker IDE. The patched version (2024.14.0 or higher) will include a fix for CVE-2025-12501.
4. **Recompile Your Projects:** After updating, it is crucial to recompile and export all affected projects using the 2024.14 runtime. Distribute these updated versions to your users.


**Important Note:** Simply updating the GameMaker IDE is not enough. You must ensure the runtime used by your projects is also updated and that your projects are recompiled with this updated runtime.


# Stay vigilant


Security is an ongoing process. We encourage all GameMaker developers to stay informed about the latest security advisories and best practices. Regularly checking for GameMaker updates and actively participating in the developer community can help ensure your projects remain secure.


For further information and official announcements, please refer to the official[GameMaker blog](https://gamemaker.io/en/blog) .


# Need assistance?


If you encounter any issues during the update process or require further assistance, please reach out to[GameMaker support](https://gamemaker.zendesk.com/hc/en-us) or consult the[GameMaker community forum](https://forum.gamemaker.io/index.php) .


We thank the security researcher who responsibly disclosed this vulnerability, allowing for a timely fix.


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
