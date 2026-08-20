---
schema_version: "1.0.0"
document_id: "455d746ee2b28fc21bcf692254b2878c1619fa7540de709d3b86514609f0c6ee"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/pt-metasploit-pro-5-1-released"
published_at: "2026-08-03T14:48:13+00:00"
first_seen_at: "2026-08-03T16:41:20.820651+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:9e4448ab3af71aeb13049122b03cfc6fba6f748076679dde8f24ce50f1d352a5"
---

# Metasploit Pro 5.1 Released

Today marks the release of Metasploit Pro 5.1 - building upon the foundation laid in 5.0, adding new evasion primitives for HTTP Meterpreter payloads, support for tracking service hierarchies, a deeper and more interactive Network Topology view, and continuing our commitment to a modern, consistent UI. This release is powered by


[Metasploit Framework 6.5](https://www.rapid7.com/blog/post/pt-metasploit-framework-6-5-released/) .


## Malleable C2 Profiles


One of the most requested capabilities in modern red-team engagements is the ability to blend Meterpreter's network traffic into legitimate-looking patterns. Metasploit Pro 5.1 brings full Malleable C2 profile support, powered by Metasploit Framework 6.5, directly into the Pro UI — no command-line knowledge required.


Malleable C2 profiles let you load a standard profile and reshape Meterpreter's HTTP(S) traffic to emulate legitimate services, browser sessions, or any other traffic pattern you need. All Meterpreter flavours — Windows, Linux, Java, Python, and PHP — are supported, including stageless and staged payloads (e.g.


meterpreter/reverse_https


and


meterpreter_reverse_https


). This functionality is compatible with


[publicly available profile libraries](https://github.com/BC-SECURITY/Malleable-C2-Profiles) .


## Profile support across the Pro UI


Malleable C2 profiles are now available in every part of the workflow where a payload is configured:


- **Single Module Run:**


The module options page now includes a Malleable C2 section.


- **Listeners (New & Edit):**


You can now choose from profiles already uploaded to the server or upload a new


.profile


file directly from your browser.


- **Payload Generator:**


The standalone payload generator also exposes the profile picker, so standalone payloads can carry the same C2 profile as the rest of your operation.


*Figure 1 Malleable Profiles*


## Improved Payload Section


Alongside the Malleable C2 integration, the payload selector has been overhauled across the Listener, Module Run, and Payload Generator pages. You can now filter payloads by platform and stage, making it much faster to find the right payload in large lists.


**


**


*Figure 2: Advanced Payload Options*


**


*Figure 3: Additional Payload Options*


## Service Hierarchy Tracking Support


The Discovered Services table has been overhauled with a cleaner, more capable interface consistent with the rest of Pro 5.1.


- **Service hierarchy visibility:**


The most significant new capability. Services can have parent-child relationships - for example, an HTTP service running over TCP, or a tunnelled protocol layered over another. The new table exposes this hierarchy directly with dedicated columns showing each service's parent and child services, so you can immediately understand how discovered services relate to one another without drilling into individual records.


- **Search and sort across all columns:**


You can now search across host name, host address, service name, protocol, port, and info in a single query. All major columns are sortable, including parent services.


- **Inline editing:**


Service fields (name, port, protocol, state, resource) can be edited directly from the table without navigating away.


*Figure 4: Service Options*


**


**


*Figure 5: Service*


*Hierarchy* *Display*


## Network Topology Enhancements


Building on Metasploit Pro 5.0's improvements to the Network Topology, we've added additional support and functionality for exploring your internal infrastructure. Previously, each node in the graph provided a high level summary of the host details when hovering over the node. This has now been moved into a dedicated side panel that surfaces everything you know about a host without leaving the topology view.


## Rich host information panels


Click any node in the topology graph and the side panel now shows a consolidated summary of everything Metasploit knows about that host:


- **Sessions:**


all sessions (open and closed) opened against the host, including session type, exploit used, payload, and timestamps.


- **Loot:**


captured loot items associated with the host, including type, name, and content type.


- **Credentials:**


cracked and captured credentials organised by service, de-duplicated and sorted with successful logins first.


- **Modules run:**


a list of every module that has been executed against the host.


- **Tags:**


any tags applied to the host or its sessions.


*Figure 6: Network Topology Display*


## New filter options


The topology graph toolbar has three new filters to help focus on the hosts that matter:


- **Filter by bruteforce**


- highlight services that can be bruteforced remotely on a host.


- **Filter by tag**


- narrow the graph to hosts carrying a specific session or host tag.


- **Filter by username**


- show only hosts where a particular user account has been compromised.


- **Filter by module**


- surface hosts that have had a specific module run against them.


**


**


*Figure 7: Network Topology Graph Filter Options*


**


## Discovered Vulnerabilities - Modern UI


The Discovered Vulnerabilities table has been fully rewritten, bringing it in line with the UI overhaul introduced across the rest of Pro in 5.0.


Key improvements:


- **High level view and granular views**


- Each registered vulnerability provides a high view such as references and affected services, as well as a more granular expandable breakdown view.


- **Inline editing**


- vulnerability details can be edited directly from the table without navigating to a separate page.


- **Nexpose integration preserved**


- all existing InsightVM/Nexpose push and pull workflows are retained in the new implementation.


**


*Figure 8: Discovered Vulnerabilities Modern UI*


## Attack technique filtering support


MITRE ATT&CK® is a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations. The ATT&CK knowledge base is used as a foundation for the development of specific threat models and methodologies in the private sector, in government, and in the cybersecurity product and service community. Metasploit Pro now supports searching for modules by these techniques:


*Figure 9: Attack Technique Filtering Search*


## Upgrading


Existing Pro installations can be upgraded through the standard update mechanism. Full upgrade instructions are available in the


[Metasploit Pro documentation](https://help.metasploit.com/Content/managing-updating-metasploit/updating-metasploit.html) .


These features are available in Metasploit Pro 5.1.0 onwards. We're proud to collaborate with our customers, who are often the source of inspiration for product evolution. Ideas for improvements or enhancements can be shared with our Support team to help refine and submit them to the Product team on your behalf.
