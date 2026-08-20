---
schema_version: "1.0.0"
document_id: "70593fa59a9cadd11af83bc347bdfb3a142f6c22c656dbc70d8f673c0d2d1382"
company_key: "zscaler-inc-common-stock"
company: "Zscaler Inc."
source_id: "zscaler-inc-common-stock-news-import-d539219e7401"
canonical_url: "https://www.zscaler.com/blogs/customer-stories/how-eaton-secured-100-manufacturing-facilities-zscaler"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T05:16:32.757157+00:00"
fetched_at: "2026-08-13T05:16:34.912030+00:00"
content_hash: "sha256:4a2ebb8b1acce657585e58a39465d30c334898ed9727b7fb17321c6744fd9fcd"
---

# How Eaton Secured 100+ Manufacturing Facilities with Zscaler

## **Our approach: watch first, then enforce**


My team didn’t start by writing rules. We started by looking. The mental model was sequential: see what exists, understand what it talks to, then decide what to allow. This drove our entire rollout. What made it workable across 216 sites was that the work compounds. The same device types show up everywhere, so a policy written at the first plant carries to the next. You build a body of work that lets you move faster as you go. We started last year and are at about 85 sites now, in various stages from monitoring to full enforcement.


Underneath the technology was a human strategy. Segmenting a live production floor changes how it works, and anytime you move somebody’s cheese, you have to have a conversation. Our security group had built a reputation for protecting the business first, and that goodwill carried us through the harder conversations.


### **Step 1: See everything**


Because the profiling is agentless, a device gets identified the moment it joins the network: what it is, who makes it, and more. We wired in our existing tools to go deeper.


CrowdStrike was the big one: I integrated the environment against our CrowdStrike database, so a device shows up with its security score attached. If it falls below a certain health threshold, our policy can say this device isn’t healthy anymore and shouldn’t be allowed to talk. Dragos is next on our roadmap. It’s built specifically to understand OT devices and the threats against them, and once that integration lands, I expect to write a policy against a device’s Dragos risk score—trusting a machine not just for what it is, but what’s enabled on the device.


The visibility caught real issues right away. Someone had plugged an Xbox into the OT network. I saw it in the asset list, blocked the MAC address in seconds, and never had to send anyone to the plant to hunt for a cable.


### **Step 2: Watch the traffic**


Once you know what exists, you look at what it does. The visualizer shows the flows of what a given HMI is talking to, what’s reaching out to the internet, what was allowed, and what was denied. Here is a real example: a new handheld scanner came onto a network and started communicating differently than the model it replaced. One flow showed up blocked. I could see it immediately, check the vendor documentation, confirm the new device legitimately needed that path, and open it instead of chasing a mystery outage.


### **Step 3: Write policy on the device, not the IP**


Rules are written against the profile of a device, not its IP address. When I want voice equipment to talk to certain systems, I write a policy that says a Polycom voice device gets that permission. If something else gets plugged into that jack that doesn’t match the profile, it can’t talk. It’s blocked. Sometimes that generates a support ticket, which I treat as a feature: it forces a direct conversation with site staff about putting only authorized devices on the network.


Our rollout method was deliberately gradual. We started with an open-ish "gray list" to allow what clearly isn’t risky, blocked the ports that keep you up at night (SMB, Telnet, SSH, FTP), and let the rest run so we could learn. We watched what fell through to the catch-all, decided what deserved a formal policy, then flipped to enforcement and dropped the rest.


### **Step 4: An incident-response plan you set in advance**


We built our ransomware response directly into the policies as a four-state control switch: red, orange, yellow, green. The point is to make the hard calls while writing policy and not in a room at 2 a.m. while an indicator of compromise (IOC) is spreading.


- Red cuts lateral movement and blocks traffic while continuing to collect telemetry on what’s trying to communicate.


- Orange opens just enough to remediate, letting endpoints reach CrowdStrike or a patching server.


- Yellow brings critical business processes back online.


- Green is a normal run state.


Every policy carries a checkbox for which mode it lives in, so the whole plant can change posture with a single control.


### **Step 5: Get vendors in without flying them in**


We branded our remote access service internally as Eaton Rapid Remote Access, or “ERAS.” It’s clientless, time-boxed, and running at over 100 sites now. A vendor enters through a ticket, gets approved, and lands on a bastion host adjacent to the OT network with nothing to install. Access can be scheduled ahead and capped at 72 hours.


Early on, a vendor in Germany was facing a $9,000 flight and days of downtime to fix a plant; they did it remotely in a couple of hours instead. Across the year, ERAS has been used about 450 times, saving us north of $500,000 in travel alone.


ERAS has also changed how we absorb acquisitions: drop the box in, and there’s a secure Eaton beachhead at the new facility on day one so the business can move faster.
