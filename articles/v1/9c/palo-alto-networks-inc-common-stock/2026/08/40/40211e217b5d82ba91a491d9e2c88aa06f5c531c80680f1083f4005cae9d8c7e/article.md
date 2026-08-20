---
schema_version: "1.0.0"
document_id: "40211e217b5d82ba91a491d9e2c88aa06f5c531c80680f1083f4005cae9d8c7e"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-e942a00c1738"
canonical_url: "https://www.paloaltonetworks.com/blog/2026/08/bridging-the-gap-an-unprecedented-approach-to-browser-and-endpoint-security/"
published_at: "2026-08-06T19:36:13+00:00"
first_seen_at: "2026-08-12T22:12:37.028174+00:00"
fetched_at: "2026-08-12T22:12:39.592460+00:00"
content_hash: "sha256:a888cef849f41384aac14616e324afd7d002c4da63f81dcf6e3297882d2374c8"
---

# Bridging the Gap: An Unprecedented Approach to Browser and Endpoint Security

[AI and Cybersecurity](https://www.paloaltonetworks.com/blog/security-operations/category/ai-and-cybersecurity/)


[Must-Read Articles](https://www.paloaltonetworks.com/blog/security-operations/category/must-read-articles/)


[News and Events](https://www.paloaltonetworks.com/blog/security-operations/category/news-and-events/)


[Product Features](https://www.paloaltonetworks.com/blog/security-operations/category/product-features/)


[Uncategorized](https://www.paloaltonetworks.com/blog/category/uncategorized/)


[Use-Cases](https://www.paloaltonetworks.com/blog/security-operations/category/use-cases/)


The enterprise workforce now operates almost entirely within the web browser. In fact, employees do roughly[85% of their daily work inside it](https://start.paloaltonetworks.com/Omdia-state-of-workforce-security) , turning the browser into the sole operating system of the modern organization that connects every application, data interaction, and identity. Yet, for modern Security Operations Center (SOC) teams, the browser remains a frustrating and dangerous "black box."


While traditional Extended Detection and Response (XDR) platforms excel at monitoring endpoint hosts and processes, they treat the browser as a single, opaque process. This creates a critical visibility gap. According to research from Unit 42,[over 90% of breaches are preventable](https://www.paloaltonetworks.com/resources/research/unit-42-incident-response-report) by solving for factors such as visibility gaps. Because modern AI tools are predominantly accessed directly through the browser, it is now critical thatSOCs get visibility into what’s happening within the browser. Leaving browser activity unmonitored forces your SOC to fight today's AI-driven workflows in the dark.


**The Operational Fallout of Browser Blind Spots**


SOC analysts frequently see alerts for malicious endpoint processes but lack the granular telemetry to pinpoint the exact web tab, malicious script, or user interaction that initiated the threat. This leaves incident responders blind to sophisticated tactics like rogue extensions and cross-origin attack chains, making it nearly impossible to reconstruct the full attack narrative.


[When a security team operates with a fragmented view](https://www.paloaltonetworks.com/blog/sase/your-browser-is-your-socs-biggest-blind-spot/) , a dangerous domino effect triggers the moment an attack strikes.


- The root cause is hidden as analysts frequently see alerts for malicious endpoint processes but do not have enough context. Lacking the necessary telemetry, the SOC is forced to take extreme containment measures and completely isolate the entire endpoint machine.
- In some cases, SOCs use third party investigation tools which create problems with disconnected context and long investigation times.
- What starts as a simple browser blind spot ultimately leads to an aggressive response that unnecessarily halts daily operations, disrupts user productivity, and floods the help desk with tickets.


Recent research on Palo Alto Networks customer incidents highlights the sheer scale of this problem, revealing massive monthly volume of Cortex threat detections stemming from siloed browser activity.


To eliminate this vulnerability, Palo Alto Networks is thrilled to announce **the native** **integration of**[Prisma Browser](https://www.paloaltonetworks.com/sase/prisma-browser) **and**[Cortex XDR.](https://www.paloaltonetworks.com/cortex/cortex-xdr)


**Cortex XDR and Prisma Browser Better Together**


By unifying deep browser-level telemetry with industry-leading endpoint detection, we are providing SOC teams with visibility into the user’s primary workspace, transforming the browser from an unmonitored process into an active security sensor.


Figure1: Prisma Browser and Cortex XDR Better Together


Organizations can achieve the full benefits of this integration without complex APIs or heavy deployment overhead. Prisma Browser events including DLP violations, browser tampering, and unauthorized configuration updates are automatically fed directly into your Cortex tenant, making it incredibly easy to adopt this joint offering.


Moreover, when Cortex XDR raises an issue, browser-based events are correlated with the user’s malicious activity on the same endpoint, to add browser-based context and adding visibility to the possible starting point of the attack when it is initiated from the browser context.


## **What Makes Our Approach Different?**


Many legacy vendors attempt to solve this problem using brittle, easily bypassed browser extensions that provide basic, surface-level visibility especially providing poor visibility into unmanaged devices. Palo Alto Networks takes a fundamentally different approach. Cortex XDR now integrates natively with Prisma Browser under the hood. This ensures both layers "speak the same language," turning a massive blind spot into a rich engine of security telemetry and visibility into everything from each user action to specific activities into their device posture while executing a particular activity..


True workspace security requires a unified defense system that understands exactly how web activity can impact the host device. This first-of-its-kind integration achieves this through **three fundamental pillars** :


### **1. Find the Root Source of Attacks in Seconds**


Integrating Prisma Browser with Cortex XDR connects comprehensive endpoint visibility with deep web context. By seamlessly linking endpoint process execution directly to browser events and host execution, Cortex XDR provides an unprecedented unified data foundation that allows SOC teams to analyze complete attack narratives rather than isolated, disjointed issues.


Figure 2: SOC Teams Get Insights Into Attack Scenarios with Prisma Browser Investigation Panel


*SOC Teams Get Insights Into Attack Scenarios with Prisma Browser Investigation Panel*


**Scenario: Unmasking Phishing and Malware Narratives**


When a malicious payload executes on an endpoint, traditional tools show the threat on the host but leave analysts guessing the source of the attack. By correlating Prisma Browser events directly with Cortex XDR eliminates this guesswork. Analysts can effortlessly trace a malware alert back to the exact phishing URL, original download source, or hidden iFrame metadata, uncovering the precise forensic root cause in seconds while easily dismissing false positives.


Figure 3: Connecting the Dots: Instantly correlate browser activity with endpoint execution for faster response and zero false positives.


### **2. Respond Without Disrupting Business**


Traditional XDR tools often need to disconnect a device to mitigate a threat. While effective at stopping lateral movement, it severely disrupts user productivity and halts business operations. The integration of Prisma Browser and Cortex XDR introduces granular, precision control.


Figure 4: Prisma Browser detects a malicious file download, blocks the action and sends a detailed report to the SOC


For example, when a rogue browser extension attempts to compromise a web session, traditional tools are forced to isolate the device, forcing the employee offline, and disrupting daily operations. The integration of Prisma Browser and Cortex XDR introduces surgical containment instead. The threat is instantly neutralized and terminated only at the browser layer while simultaneously alerting Cortex, allowing the employee's laptop to stay completely online and productive.


### **3. Detect Evasive Threats in Real Time**


Prisma Browser uses a pioneering approach to analyze activity in real time, detecting threats as they happen. This ensures that even the most sophisticated, evasive threats, such as rogue extension behavior or malicious script execution, are identified and flagged in real-time within your Cortex dashboard.


Figure 5: Prisma Browser detects an evasive threat in real time and shows in the Cortex dashboard


**4. Securing GenAI Use Cases:**


As employees rush to adopt GenAI tools, critical risks emerge, such as an engineer copying proprietary source code and pasting it into an unapproved, public AI model to fix a bug. To traditional XDR, this looks like safe, standard web traffic. Prisma Browser solves this by monitoring user behavior inside the workspace to automatically detect and block data loss prevention (DLP) violations in real time. Because it connects natively to the Cortex tenant without complex APIs, shadow AI risks are instantly flagged in the SOC dashboard before they turn into major compliance issues.


## **Future-Proof Your Workspace Security**


Security operations can no longer afford to leave the browser unmonitored. By bridging the gap between what happens in the browser and activities on the endpoint, the integration of Prisma Browser and Cortex XDR accelerates investigation times, exposes hidden threats, and allows your SOC to respond with unprecedented precision.


**New to Prisma Browser?**[Talk to your account team](https://www.paloaltonetworks.com/sase/prisma-browser#product_report_modal)
