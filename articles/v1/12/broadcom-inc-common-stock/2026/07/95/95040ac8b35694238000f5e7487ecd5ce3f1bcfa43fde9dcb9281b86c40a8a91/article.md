---
schema_version: "1.0.0"
document_id: "95040ac8b35694238000f5e7487ecd5ce3f1bcfa43fde9dcb9281b86c40a8a91"
company_key: "broadcom-inc-common-stock"
company: "Broadcom Inc."
source_id: "broadcom-inc-common-stock-rss-6823269edf1e"
canonical_url: "https://news.broadcom.com/broadcom-knights/broadcom-knights-enterprise-infrastructure"
published_at: "2026-07-21T16:00:00+00:00"
first_seen_at: "2026-07-21T17:35:38.600257+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:66285259f189abc6468220f6ef3ae04e434bcc3d1ee851b439530d138069c328"
---

# Broadcom Knights: What This Distinction Really Means for Your Infrastructure

*An ongoing blog series by our Broadcom Knights, an elite group of Partner Technical Professionals with deep technical expertise in Broadcom’s portfolio and recognized as experts in their field.*


-
-
-


When someone says they want to upgrade to VMware Cloud Foundation 9.1, the first image that comes to mind is usually a straight line: current environment at the top, VCF 9.1 at the bottom, an arrow in between. Reality almost never works that way. And the difference between knowing that in advance and discovering it halfway through a project is, in many cases, the difference between a plan that works and one that stalls.


As a Broadcom Knight, I spend a lot of time with customers, talking through projects like upgrading to VCF 9.1, and my training and hands-on work that I do as a Knight, gives me that advantage of knowing the difference in advance. The Broadcom Knights program is invitation-only. There is no exam, no course. Engineers who hold this recognition carry it because someone at Broadcom has seen their work in the field and decided it operates at a different level. That has very concrete practical consequences for the projects they are part of.


Broadcom Knights at a Knights-only special session during Broadcom's Activate event in Amsterdam


Not long ago I sat down with a client in the insurance sector who wanted to do exactly that: reach VCF 9.1. The environment on paper did not look especially complex: 27 hosts, 3 vCenters, 435 virtual machines. What appeared when we actually started looking was a different story. Five distinct processor generations coexisting in the same environment, from Haswell from 2015 to Sapphire Rapids from 2023. Eight hosts with CPU architectures that VCF 9.1 simply does not support. Thirty-four third-party appliances in Cisco IP telephony, video conferencing, network management, whose installed versions were not compatible with ESXi 8, and blocked the upgrade of the hosts running them. The environment also had one SAP host running completely standalone, without HA, without DRS, with two business-critical virtual machines and no resilience whatsoever.


That is not a technology problem. It is a visibility problem. And without that level of detail, any upgrade plan is a promise built on air.


What allows a project like this to succeed is not just knowing the target platform. It is knowing how to design a route that does not put the client in an impossible position because everything cannot move forward at the same time. In this case, the solution was to design a two-speed architecture: 70% of the environment enters VCF 9.1 from day one, distributed across a Management Domain and two Workload Domains. The remaining 30% stays on a legacy vCenter, fully functional, while the software dependencies that block it today are progressively resolved. Nothing is thrown away, nothing breaks, and the client advances with what can move now rather than waiting for everything to be ready at once.


There are details that only surface when you know the platform from the inside. For instance: for the Management Domain, we leveraged the convergence of the existing vSAN cluster (three Dell R660 nodes with Sapphire Rapids processors, already certified) rather than deploying a new one from scratch, which would have required a fourth host that was not in the budget. Or that three clusters, one of them with a completely standalone SAP host, operating without high availability gain HA for the first time through the redesign, with no additional hardware cost. Those kinds of decisions do not come from reading documentation. They come from having been in similar places before, from having early access to how the target platform works, and from having validated the architectures in the lab before proposing them to the client.


And then there is the sequencing work, which has nothing glamorous about it but is where projects are won or lost. Thirteen phases over eighteen weeks, host by host, accounting for which appliance lives on each host, which hosts have no HA and require VMs to be powered off before they can be touched, which clusters have vSAN dependencies that require a specific order, which versions of VMware Tools are old enough to cause problems during the upgrade. All of it is documented, validated, and handed off to the client's team so they can execute it with confidence, even in the steps where we are not present.


That is what it means to work with a team that includes a Broadcom Knight. It is not that the technology works differently. It is that someone has looked at all of this before the project begins, has designed the route accounting for what is actually there, and has left a plan the client understands and can execute. Infrastructure does not lie. What almost always fails is the time it takes to understand it properly.


Learn more about[the Broadcom Knights program](https://www.broadcom.com/how-to-buy/software-partners/work-with-partners/knights) .


About the Author


About the Author


[Raul Arias Lazaro](https://news.broadcom.com/author/raul-arias-lazaro)
