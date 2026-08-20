---
schema_version: "1.0.0"
document_id: "8a1ddb83b61dac3a626b022b1bfa84c28c7f3df79d87339cc71d0c762898d8f9"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/the-first-principles-of-cybersecurity-still-apply/"
published_at: "2026-08-18T17:50:22+00:00"
first_seen_at: "2026-08-18T21:34:18.453897+00:00"
fetched_at: "2026-08-18T21:34:20.075846+00:00"
content_hash: "sha256:9ba4812580002c6ccc7d207a0e40076ed70c161277c6332178a8b07b67e21192"
---

# The First Principles Of Cybersecurity Still Apply

Years ago,[security researchers](https://www.cnet.com/news/privacy/study-unpatched-pcs-compromised-in-20-minutes/) demonstrated a simple reality: Connect an unprotected computer directly to the internet, and it won’t stay untouched for long. The exact timeline varied — sometimes it took minutes, sometimes it took longer — but the outcome was always predictable; attackers would find the system and begin probing it for weaknesses.


Technology has changed since then. Operating systems improved. Endpoint protection became stronger. Security teams became more mature. What hasn’t changed is the principle.


- If you expose an unprotected system to hostile networks, someone will eventually find it.


[Recent attacks](https://www.fbi.gov/investigate/cyber/alerts/2026/malicious-cyber-actors-targeting-water-and-wastewater-sector-internet--facing-programmable-logic-controllers-causing-operational-disruptions) against US water and wastewater facilities remind us that this principle applies to operational technology (OT) just as it does to traditional IT.


### **The Attack Path Wasn’t Sophisticated**


Many discussions surrounding public water-supply cybersecurity issues correctly identity budget constraints, staffing shortages, and limited access to OT security expertise as factors. Those issues are real. But they are often treated as the primary reasons for cyberattacks. In many cases, they are not.


The more fundamental issue is that organizations continue to expose vulnerable operational systems that control physical processes to networks that attackers can reach. Think about that for a moment.


Most organizations would never place a critical Windows server directly on the public internet without controls around it. Yet we continue to discover[programmable logic controllers (PLCs)](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-335a) ,[industrial control systems](https://cybersecuritynews.com/145000-unsecured-ics-devices-exposed/) ,[cameras](https://hackread.com/us-tops-list-unsecured-cameras-exposing-homes-offices/) , and other IoT and OT assets that remain directly accessible. The recent water-sector incidents, along with similar attacks against exposed PLCs[earlier this year](https://www.ic3.gov/CSA/2026/260407.pdf) , reflect the same underlying problem — different threat actors, different targets, same vulnerability:


- The devices were visible.
- The devices were reachable.
- The devices became targets.


### **Security By Obscurity Has Never Been An Effective Security Strategy**


Many operators assume their environment is too small, too remote, or too specialized to attract attention. That assumption no longer holds, if it ever did in the first place.


Services such as Shodan continuously scan the internet for exposed devices and services. Shodan is helpful for security analysts to understand what systems within their enterprise are exposed, but it also means attackers no longer need to know about your organization to find your systems. These services have effectively automated discovery, and the result is that visibility itself becomes a risk. Security leaders need to realize that not every attack is driven by ideology, geopolitical objectives, or a multimillion-dollar ransom demand. Sometimes attackers compromise a system simply because they can.


### **AI Makes The Problem Worse**


Agentic AI introduces another challenge for critical infrastructure operators. Organizations are rapidly exploring how AI can improve productivity, automate workflows, and help security teams operate more efficiently. Threat actors are pursuing similar goals and using automation to identify exposed assets, gather intelligence, and streamline exploitation. This is shifting the economics of reconnaissance in the attacker’s favor.


Finding exposed systems no longer requires significant effort. Correlating those systems with known weaknesses becomes easier and building repeatable attack workflows becomes faster. Organizations should not assume that future threats will involve highly skilled adversaries manually targeting individual assets, and many attacks will now begin with automated systems continuously searching for opportunities.


### **The Cybersecurity Industry Is Moving In The Right Direction**


Don’t consider this to be more doom-mongering, as there are positive developments. Following the recent water-sector incidents, managed detection and response providers have begun stepping forward with programs designed to help municipalities improve visibility and security. Some cybersecurity vendors in OT are offering discounted solutions for those businesses that are critical and severely lacking in resources. Policymakers and regulators are also renewing conversations around critical infrastructure protection and cybersecurity requirements. These efforts matter, but they should not distract organizations from the most important lesson: Technology alone cannot compensate for poor architecture.


### **Return To First Principles**


Business leaders running OT environments don’t need to invent entirely new security concepts, as many of the most effective protections have existed for decades. Start with the basics:


- Don’t expose operational systems directly to the internet.
- Segment operational workflows from one another.
- Restrict user access to only the systems required for their jobs.
- Closely manage connectivity between IT and OT environments.
- Monitor and secure remote-access pathways.


As IT and OT continue to converge, these fundamentals become even more important. Human resources personnel do not need direct paths into manufacturing systems. Administrative networks do not need unrestricted access to industrial control environments. Security boundaries remain relevant regardless of the technology being protected.


### **Don’t Forget Remote Connectivity**


One final point deserves attention. Many critical infrastructure operators rely on remote sites that require continuous monitoring and management. Water utilities, for example, often connect treatment facilities, pumping stations, reservoirs, and distribution systems across large geographic areas. These connections are necessary, but they also create risk.


[Recent incidents](https://thehackernews.com/2026/08/hackers-breach-polish-power-plant.html) demonstrate that attackers are increasingly targeting communications infrastructure, including private cellular networks used to connect remote operational environments. Organizations should treat these connections with the same rigor they apply to internet-facing assets: Secure them, monitor them, and verify access continuously. Remote connectivity should not become an unmonitored pathway into operational systems.


### **The One Question You Need To Ask**


The cybersecurity challenges facing municipalities and critical infrastructure operators are complex. Funding shortages, staffing gaps, and aging infrastructure all contribute to the problem, but before organizations pursue new tools, new regulations, or new technologies, they should ask a simpler question:


- **Why is this system reachable in the first place?**


Forrester clients interested in this topic should[connect with us](https://www.forrester.com/inquiry) to discuss via an inquiry or guidance session.


Share


-
-
