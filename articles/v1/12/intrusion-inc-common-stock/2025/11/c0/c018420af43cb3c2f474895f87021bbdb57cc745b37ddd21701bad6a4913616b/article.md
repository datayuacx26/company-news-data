---
schema_version: "1.0.0"
document_id: "c018420af43cb3c2f474895f87021bbdb57cc745b37ddd21701bad6a4913616b"
company_key: "intrusion-inc-common-stock"
company: "Intrusion Inc."
source_id: "intrusion-inc-common-stock-rss-206824b90ed5"
canonical_url: "https://intrusion.com/blog/indicators-of-compromise-what-they-are-and-how-to-identify-them/"
published_at: "2025-11-11T23:00:00+00:00"
first_seen_at: "2026-07-28T22:10:21.872433+00:00"
fetched_at: "2026-07-28T22:25:18.858836+00:00"
content_hash: "sha256:d4a42765220d75c3c0ff94dfc6d2f22cec5d9d94ef34514a8a46987556d0211f"
---

# Indicators of Compromise: What They Are and How to Identify Them

Cybersecurity is essential, but managing it has never been straightforward. The more tools you add, the more vulnerabilities you introduce. Organizations manage threats by controlling the processes of identifying, responding to, and recovering from security events. Various teams under the cybersecurity domain collaborate to maintain security and prevent cyberattacks.


The defensive team’s responsibility is to look for any abnormality in the applications, servers, or network. Whenever a threat actor attacks a target, they often leave traces of their activity. These traces, which indicate malicious activity, are also known as Indicators of Compromise (IOCs).


To avoid detection, highly skilled attackers try to hide or remove these traces before they leave your network for the last time. Accordingly, security teams look for these IOCs when a cybersecurity incident occurs, or better yet, before a business-impacting security event occurs.


### What are Indicators of Compromise (IOC)?


There are too many different IOCs to discuss in one blog. So, we won’t cover them all here. IOCs can include:


- physical indicators,


- behavioral indicators,


- digital indicators,


- and even IT performance indicators.


Again, IOCs are numerous. In today’s cybersecurity landscape, when people talk about IOCs, they usually refer to suspicious IP addresses, domains, URLs, and MD5 or SHA256 hashes. Why just these select few? The answer is straightforward. They are easy to find.


Modern security software can detect these IOCs. These indicators not only reveal the occurrence of an attack, but can also tell you how the threat actor is carrying out the attack.


For example, if your security system logs indicate several outbound connections to an IP address that open-source intelligence (OSINT) says is most likely suspicious, odds are you have a malicious file somewhere in your network beaconing out to a malicious command & control (C2) server. Through OSINT research, you might also find the MD5 hashes of such files and then scan your network for evidence of those hashes.


Security software often flags abnormal activity as an IOC, indicating that a potential threat might be active. Sadly, it is not always easy to detect these red flags, since these IOCs can be as inconspicuous as metadata details (tiny bits of data about other data). The challenge here is setting up security software to detect what you might consider abnormal network and system behavior.


Every application or operating system has a log file that registers events within the system. Some of these recorded events could serve as IOCs. However, crafty attackers take the time to alter these logs to disguise or hide their activity. That being said, even the mere act of changing logs leaves a trace.


You can find these forensic artifacts in system-generated event logs or time-stamped records. Log records might show changes to files in the system directory, changes to any applications and the system registry, changes to user or admin accounts, odd connections to unusual domains, network logs showing large volumes of traffic moving outbound during non-business hours, etc.


Indeed, in all likelihood, several clues are just sitting somewhere in your network. But finding those clues is easier said than done. You would typically have to comb through the logs of thousands of IT devices on your network to find the bad apple in the orchard.


## **Types of Indicators of Compromise**


Before you can hope to identify or detect an IOC in your network, you need to know what you’re looking for. Most IOCs fall under three well-defined categories.


### **1. Network-based Indicators of Compromise**


##### **Domain Names**


Threat actors typically use C2 servers to deliver malware or exfiltrate data. While the IP addresses of these servers may frequently change, the domain names used to reach them often stay active for longer periods. When such domains are detected and flagged repeatedly, security vendors and cybersecurity researchers mark and share them as IOCs via threat intelligence feeds.


##### **IP Addresses (IPv4 and IPv6)**


Although IP addresses generally change more often than domain names, some stay active long enough to be flagged as malicious. As with malicious domains, cybersecurity experts also share bad IP addresses through OSINT feeds and other threat intelligence sources. That said, threat actors may employ IP spoofing. It’s a technique wherein threat actors replace bad IP addresses with trusted IPs to avoid detection, so be aware of this possibility.


##### **Uniform Resource Locators (URLs)**


In addition to domains and IP addresses, threat researchers may also flag specific URLs as malicious. Aside from domains or IP addresses, URLs may include additional pieces of information, such as a network protocol, a filename, and a query string. Here’s an example of a typical IOC URL: http://malicious-domain\[.\]com/update/install.php?ver=2.1


### **2. E-mail-based Indicators of Compromise**


##### **Sender’s email address or domain**


Email-based attack vectors—e.g., phishing, spear phishing, business email compromise (BEC)—typically exhibit several indicators of compromise. For instance, the sender’s email address or domain fields of an email coming from an unknown source may closely resemble those of legitimate businesses—for example, micros0ft.com instead of microsoft.com. More often than not, a domain like this has one purpose—to deceive recipients.


##### **Email body, hyperlinks, and attachments**


The email’s content itself, as well as its accompanying hyperlinks and attachments, can likewise contain IOCs. In fact, frequently used malicious attachments may already have associated MD5 hashes published in OSINT channels.


##### **Proxy IP address or domain**


Generally, threat actors use proxies or compromised servers when sending phishing emails to mask their true origins and evade detection. However, if attackers have already used those proxies and servers in previous phishing campaigns, their IP addresses or domain names may already be flagged and identified as IOCs.


### **3. Host-based Indicators of Compromise**


##### **Registry Key Changes**


Malware residing in systems can modify or introduce malicious registry keys to maintain persistence. Unusual dates, times, purposes, and types of changes in registries may indicate malicious activities. Hence, they can be considered IoCs.


##### **Files and File Hashes**


After a malicious file is discovered and analyzed (for example, in the aftermath of a malware outbreak), threat analysts generate an MD5, SHA-1, or SHA-256 hash of the file in question. This hash serves as that file’s unique fingerprint. This fingerprint is then recorded in threat intelligence platforms, shared through OSINT feeds, and can then serve as an IOC if detected on a system.


##### **Process Name and ID**


Unknown processes or instances of the same processes running multiple times simultaneously on a given system can indicate malicious activity. An unfamiliar, recently-installed application may also be malicious. You can consider these artifacts IOCs, especially if your organization has strict software installation or deployment rules, and the circumstances surrounding the software in question violate those rules.


### **Other notable indicators of compromise**


##### **Suspicious activities from unrelated geographic regions**


Network traffic associated with normal business processes usually comes from familiar geographical locations. Malicious traffic, on the other hand, such as that related to state-sponsored cyber attacks, typically originates from unexpected locations. For example, if you see traffic coming from China or North Korea, but don’t transact with anyone in those regions, you may consider that traffic an IOC.


##### **Multiple requests or attempts to access critical files**


Attackers who try to take over user accounts sometimes leave traces of their attempted break-ins. Log entries showing multiple unsuccessful logon attempts may indicate a brute force or similar cyber attack. Also, if a user account repeatedly tries to access critical resources (e.g., files, folders, or applications) beyond that account’s privileges or responsibilities, it may mean the user account is compromised.


##### **Anomalous outbound traffic**


After threat actors establish a foothold in a victim’s network, they usually contact their C2 servers. They’ll need those servers when the time comes to exfiltrate stolen data or request updates or commands. These outbound communications typically happen off-hours to avoid detection. If you observe these types of outbound connections in your network logs, take a closer look and see if they’re legitimate or not.


##### **Poor network or system performance**


Certain types of cyber attacks, such as Distributed Denial of Service (DDoS) attacks or malware outbreaks, may cause your systems, network, or services to slow down or even grind to a halt. In the case of a DDoS attack, for instance, your systems may shut down due to a barrage of requests from a botnet.


## **How to Identify Indicators of Compromise**


Theoretically speaking, it’s possible to identify IOCs using manual methods. You can, for example, review system and network logs for anomalies, inspect email headers, check domain names and IP addresses against OSINT sources, and so on. However, considering the complexity and variety of threats that are out there, it’s impractical to rely solely on manual methods to identify IOCs.


To effectively and efficiently identify IOCs, you’ll need security tools that can ingest threat intelligence from OSINT sources or that maintain massive and constantly updated IOC databases. Additionally, these tools should be capable of combining recorded or historical data with contextual information to determine whether a particular event is truly malicious. Otherwise, they’ll be prone to false positives.


[Intrusion Shield](https://intrusion.com/why-intrusion/) , for example, takes historical and reputation data from its massive and growing database of 8.5 billion IP addresses (and counting). It then combines that data with contextual information (e.g., communication patterns and behaviors) to detect threats and automatically block them.


Indicators of compromise play a crucial role in threat detection and response. To leverage IOCs effectively, adopt a multi-layered cybersecurity strategy that combines vigilant monitoring, threat intelligence, and robust security tools. Interested in learning more about Intrusion Shield?


[Book a meeting now.](https://intrusion.com/book-a-meeting/)
