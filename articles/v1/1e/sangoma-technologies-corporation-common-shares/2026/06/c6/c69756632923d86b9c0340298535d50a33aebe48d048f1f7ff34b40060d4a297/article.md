---
schema_version: "1.0.0"
document_id: "c69756632923d86b9c0340298535d50a33aebe48d048f1f7ff34b40060d4a297"
company_key: "sangoma-technologies-corporation-common-shares"
company: "Sangoma Technologies Corporation Common Shares"
source_id: "sangoma-technologies-corporation-common-shares-news-import-599c15f50aea"
canonical_url: "https://sangoma.com/blog/sangoma-microsoft-teams-integration/"
published_at: "2026-06-01T21:30:11+00:00"
first_seen_at: "2026-07-22T12:42:18.891395+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:caeb4731b378de66013891dcd3a7e034b7153f01a0015b1253dad5e5638d9726"
---

# Microsoft Teams Integration With Sangoma: How It Works and the Benefits

Microsoft Teams has become the default workspace for millions of businesses. Meetings, messaging, file sharing: it handles the collaboration layer well. But when it comes to business-grade voice calling, Teams alone often leaves gaps that matter: limited call routing, inconsistent reliability, and licensing costs that can creep up as your organization grows.[Sangoma’s Microsoft Teams integration](https://sangoma.com/microsoft-teams-integration/) is built to close those gaps, adding full PBX-grade voice capabilities to the Teams environment your staff already uses every day.


## **Why Microsoft Teams Alone Might Not Be Enough for Business Calling**


Microsoft offers its own telephony solution in the form of Teams Calling Plans, but most businesses with serious voice needs look elsewhere. According to research by Cavell Group, more than 90% of telephony-enabled Microsoft Teams users rely on third-party providers for calling services rather than Microsoft’s native option.


The gaps show up in a few places.


- Call routing in Teams is relatively basic: hunt groups, ring groups, and granular routing rules that most businesses take for granted in a PBX are either absent or limited.
- On reliability, Microsoft upgraded the Teams Phone SLA to 99.999% in April 2024 ([source](https://www.nojitter.com/ai-collaboration/i-read-the-microsoft-teams-voice-slas-so-you-don-t-have-to) ), but that guarantee covers Microsoft’s infrastructure only. It doesn’t protect against local internet outages, ISP failures, or site-level disruptions — and Teams has no native call survivability to keep calls running when any of those occur.
- If Teams goes down, calls stop, because there is no native call survivability.


Add licensing complexity on top of that, and it becomes clear why most businesses with real telephony needs are looking for a third-party solution to sit behind Teams and carry the load.


## **How Sangoma Integrates With Microsoft Teams**


Sangoma supports two distinct integration methods: the Embedded App approach and Direct Routing. The right choice depends on which Sangoma platform your organization runs and how much you want Teams involved in the day-to-day calling workflow.


### **Embedded App Integration**


Sangoma offers two embedded app options.[TeamHub](https://sangoma.com/teamhub/) for MS Teams is supported on Sangoma’s Business Voice and CommUnity platforms. Users get Sangoma calling, chat, SMS, and contact center features within Teams through the TeamHub interface, without needing a Microsoft Phone System license. The TeamHub Talk dialpad lives inside Teams, so calls are made and received from the same window staff already have open.


Sangoma Phone for MS Teams serves organizations running Switchvox as their PBX, delivering the same embedded calling experience and keeping users inside Teams while routing calls through their existing infrastructure. For further detail on features available through either embedded option, see the[Sangoma Microsoft Teams integration page](https://sangoma.com/microsoft-teams-integration/) .


### **Direct Routing**


Direct Routing connects Teams’ native dialpad to Sangoma’s infrastructure through[certified session border controllers](https://sangoma.com/products/phones-and-hardware/product/smb-sbc/) , enabling PSTN calling through the standard Teams calling interface. Users dial from within Teams the way they always have, while Sangoma handles the routing, reliability, and call control behind the scenes. Direct Routing is supported across all Sangoma platforms: Business Voice, CommUnity, and[Switchvox](https://sangoma.com/products/phones-and-hardware/products/uc-appliances/switchvox/) , making it available regardless of how your organization’s voice stack is currently configured.


A session border controller (SBC) is a dedicated device that sits at the boundary between Sangoma’s network and Microsoft’s infrastructure. It manages call signaling, enforces security policies, and ensures voice traffic is translated correctly between the two environments. Microsoft requires a certified SBC for all Direct Routing deployments, which is why this path involves more setup than the embedded app options. It also requires a Microsoft Phone System license per user, since Teams’ native dialpad needs that license to handle external PSTN calls. The embedded app paths (TeamHub for MS Teams and Sangoma Phone for MS Teams) bypass this requirement entirely because calling runs through Sangoma’s own interface rather than Teams’ native dialer.


## **What Are the Benefits of Integrating Sangoma With MS Teams?**


Before getting into individual benefits, it helps to understand how the two integration paths differ in cost. TeamHub for MS Teams and[Sangoma Phone](https://sangoma.com/products/phones-and-hardware/products/phones/) for MS Teams are included at no additional cost with[Sangoma UCaaS](https://sangoma.com/products/communications-platform/) seats, with no Microsoft Phone System license required. Direct Routing uses Teams’ native dialpad but does require a Microsoft Phone System license and involves a connector setup. For a detailed comparison of both paths, Sangoma’s post on[supercharging Microsoft Teams with Sangoma UCaaS](https://sangoma.com/blog/supercharge-microsoft-teams-with-sangoma-ucaas/) walks through the tradeoffs.


### **Advanced Call Control That MS Teams Can’t Match on Its Own**


Teams Calling Plans have become more competitively priced, with Microsoft currently listing them at around $17 per user per month, so cost is no longer where third-party providers win. The real case for Sangoma lies in the call handling capability that Teams doesn’t offer natively.


Sangoma adds ring groups, call queues, advanced call routing rules, call recording, and granular admin controls that go well beyond what Teams provides out of the box. For organizations with multi-site setups or complex inbound call workflows, the difference between Teams-native call handling and Sangoma’s PBX-grade routing is substantial. Business Voice Plus (BV+) also adds on-premises high-availability support for organizations that need a local failover layer in addition to cloud redundancy.


### **Zero Retraining for Existing MS Teams Users**


Embedded App and Direct Routing integration methods keep staff inside the Teams interface. For the embedded app path, Sangoma calling features appear in the Teams sidebar: the dialpad, call history, and voicemail are all accessible without opening any additional application. For Direct Routing, users call from the Teams dialpad they already know.


### **Reliability and Call Continuity**


Microsoft now matches the industry-standard 99.999% uptime SLA for Teams Phone, but an SLA covers the provider’s cloud infrastructure — not the full call path. If a site loses internet connectivity, if an ISP has a regional outage, or if Microsoft’s service itself goes down, Teams offers no built-in mechanism to keep calls running.


Sangoma builds call continuity into the integration itself. If Teams experiences an outage, calls continue through the Sangoma softphone or mobile client, so users keep working without waiting on Microsoft’s infrastructure to recover. For organizations with on-premises requirements, BV+ provides a local survivability layer that keeps the site operational even when both Teams and the cloud are unavailable.


### **A Complete Unified Communications Suite Beyond Voice**


Sangoma’s integration adds capabilities that Teams doesn’t natively provide: SMS with business numbers through 10DLC-registered campaigns, contact center tools through Sangoma CX, AI-powered features including call summaries and a knowledge bot, and the full[TeamHub collaboration suite](https://sangoma.com/teamhub/) . Teams handles video conferencing well, and that remains in place. Sangoma layers on the voice and[unified communications capabilities](https://sangoma.com/blog/unified-communications-solutions-guide/) that Teams doesn’t cover rather than duplicating what already works.


### **Flexible Deployment Across Any Business Environment**


Both Embedded App and Direct Routing integration paths are built to work across SMBs, enterprises, remote teams, and multi-site organizations. Sangoma scales to match the size and structure of the business without requiring a fixed deployment model. SMBs often start with TeamHub for MS Teams for its quick deployment and lower licensing overhead; larger or more IT-driven organizations typically choose Direct Routing for the additional control it provides over routing and dialplan configuration.


### **Full Preservation of Existing PBX Infrastructure**


Organizations running[Switchvox](https://sangoma.com/products/phones-and-hardware/products/uc-appliances/switchvox/) do not need to replace their PBX to integrate Teams. Direct Routing layers Teams on top of the existing Switchvox infrastructure, and Sangoma Phone for MS Teams brings the Switchvox calling experience into the Teams sidebar. Either approach preserves the investment already made in PBX hardware and configuration while extending Teams access to users who want it.


### **Simplified Operations Through a Single Vendor**


Managing voice,[UCaaS](https://sangoma.com/blog/what-is-ucaas/) , networking, and Teams integration through separate vendors creates support gaps and contract overhead. Sangoma covers all of those layers: cloud voice, UCaaS, Teams integration, and network infrastructure, from a single platform. One vendor, one support contact, and consistent service across the stack reduces the operational surface that IT teams have to manage.


## **How Sangoma Supports Your Rollout**


The deployment experience differs by integration path. TeamHub for MS Teams and Sangoma Phone for MS Teams are built for quick rollout: users install the app from the Teams App Store, log in with their Sangoma credentials, and calling features are available immediately. There is no complex infrastructure configuration required, and the IT lift is minimal.


Direct Routing involves more setup: configuring a certified session border controller, provisioning the Teams Phone System license, and connecting the routing rules between Teams and Sangoma’s platform. Sangoma’s team supports the process end-to-end, from initial configuration through testing and go-live, so organizations don’t have to manage the technical integration on their own.


## **Ready to Add Full Business Voice to Microsoft Teams?**


Sangoma’s integration adds the calling infrastructure, reliability, and UC capabilities that turn Teams into a complete business phone system. Whether the right path is the embedded TeamHub app or Direct Routing depends on your existing setup and how deeply you want Teams involved in your voice workflow.


To explore which integration model fits your organization, visit the[Sangoma Microsoft Teams integration page](https://sangoma.com/microsoft-teams-integration/) or[book a discovery call](https://sangoma.com/contact-sales/) with Sangoma’s team.


## **Sangoma Microsoft Teams Integration FAQs**


**Do I need a Microsoft Teams Phone System license to use Sangoma?**


It depends on which integration path you choose. TeamHub for MS Teams and Sangoma Phone for MS Teams do not require a Microsoft Phone System license: calling features run through the embedded Sangoma app, included at no extra cost with your Sangoma UCaaS seat. Direct Routing does require a Microsoft Phone System license, as it connects Sangoma’s infrastructure to Teams’ native dialpad.


**Can I keep my existing PBX when integrating Sangoma with Microsoft Teams?**


Yes. If you are running Switchvox, both Sangoma Phone for MS Teams and Direct Routing work with your existing PBX in place. Direct Routing layers Teams on top of your current infrastructure without replacing it, and Sangoma Phone for MS Teams surfaces Switchvox calling features inside the Teams sidebar.


**What calling features do we gain that Microsoft Teams doesn’t provide?**


Sangoma adds ring groups, call queues, advanced call routing, call recording, granular admin controls, SMS with business numbers, contact center tools through Sangoma CX, and AI productivity features including call summaries. It also provides 99.999% uptime and call continuity during Teams outages, two areas where Teams’ native calling falls short.


**Will my team have to learn a new interface?**


No. Both integration paths keep staff inside Teams. The embedded app adds a Sangoma calling tab to the Teams sidebar; Direct Routing uses the Teams dialpad your staff already know. Neither requires learning new software or changing daily workflows.


**How does Sangoma improve reliability compared to native Teams calling?**


Both Sangoma and Microsoft now offer a 99.999% uptime SLA for telephony, but an SLA only covers the provider’s cloud infrastructure.


**Will this work if we have remote, hybrid, or multi-site teams?**


Yes. Both integration paths are designed to support distributed organizations. The embedded app and Direct Routing both work over standard internet connections, and Sangoma’s mobile client ensures continuity for remote and hybrid staff. Multi-site organizations can also use Sangoma’s SIP trunking to extend reliable PSTN access across locations.
