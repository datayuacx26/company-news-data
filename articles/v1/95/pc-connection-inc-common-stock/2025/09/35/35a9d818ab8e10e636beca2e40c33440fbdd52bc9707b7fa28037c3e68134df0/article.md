---
schema_version: "1.0.0"
document_id: "35a9d818ab8e10e636beca2e40c33440fbdd52bc9707b7fa28037c3e68134df0"
company_key: "pc-connection-inc-common-stock"
company: "PC Connection Inc."
source_id: "pc-connection-inc-common-stock-rss-d57dd5940756"
canonical_url: "https://community.connection.com/upcoming-changes-to-exchange-hybrid-functionality-what-you-need-to-know-and-how-connection-can-help/"
published_at: "2025-09-12T16:33:24+00:00"
first_seen_at: "2026-07-20T23:22:23.189033+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:95bc70cf29f1a3e63756d56b3df514a19621ed6bd862f3e5bf29361a712ecd2d"
---

# Upcoming Changes to Exchange Hybrid Functionality: What You Need to Know and How Connection Can Help

Microsoft is implementing critical updates to Exchange hybrid environments as part of its Secure Future Initiative (SFI), aimed at enhancing security and modernizing hybrid configurations. These changes will directly impact organizations using Exchange Server in hybrid mode with Exchange Online, especially those relying on rich coexistence features like Free/Busy lookups, MailTips, and profile picture sharing.


### **Key Changes**


**1. Transition to a Dedicated Exchange Hybrid Application**


Historically, Exchange hybrid environments have used a shared service principal (Office 365 Exchange Online) to enable hybrid features. Starting with the April 2025 Hotfix Update (HU), Microsoft is transitioning to a dedicated Exchange hybrid application in Entra ID. By October 2025, all hybrid environments requiring rich coexistence must switch to this dedicated app, as the shared service principal will no longer be supported.


Organizations can make this change by:


- Installing the April 2025 HU (or later) and running the ConfigureExchangeHybridApplication.ps1 script.
- Alternatively, using the updated Hybrid Configuration Wizard (HCW), though the script is recommended for robustness.


**2. Deprecation of Exchange Web Services (EWS)**


Microsoft is also retiring EWS calls from Exchange Server to Exchange Online. These will be replaced with REST-based Microsoft Graph API calls, offering more granular permissions and improved security. This update is expected in Q3 2025, and organizations must adopt it by October 2026 to maintain hybrid functionality.


### **Who Needs to Act and When**


If your organization uses rich coexistence features, you must:


- Switch to the dedicated hybrid app by October 2025
- Update to Graph API permissions by October 2026


Failure to do so will result in broken hybrid features, including Free/Busy sharing and MailTips.


Organizations not using rich coexistence should still consider running the Service Principal Clean-Up Mode script to remove legacy certificates and harden their hybrid configuration.


### **How Can Connection Help**


Navigating these changes can be complex, but Connection is here to support you. Our experts can:


- Assess your current Exchange hybrid setup
- Guide you through the transition to the dedicated hybrid app
- Prepare your environment for Graph API integration
- Ensure compliance and continuity of service


Don’t wait until functionality breaks—contact your Connection team today to schedule a review and stay ahead of these critical updates.


### [Connection](https://community.connection.com/author/connection/)


**Read Next**
[Three Questions Every Executive Should Ask Before Funding an AI Initiative](https://community.connection.com/three-questions-every-executive-should-ask-before-funding-an-ai-initiative/?cm_sp=community-_-readnext-_-postpage)
[What IT Leaders Should Know About AI PC Security in 2026](https://community.connection.com/what-it-leaders-should-know-about-ai-pc-security-in-2026/?cm_sp=community-_-readnext-_-postpage)
[Backup and Recovery in 2026: 5 Priorities for Resilient Data Protection](https://community.connection.com/backup-and-recovery-in-2026-5-priorities-for-resilient-data-protection/?cm_sp=community-_-readnext-_-postpage)
[How SASE and Zero Trust Work Together to Strengthen Secure Access](https://community.connection.com/how-sase-and-zero-trust-work-together-to-strengthen-secure-access/?cm_sp=community-_-readnext-_-postpage)
[Why CIOs Are Consolidating Partners: The New Partnership Value](https://community.connection.com/why-cios-are-consolidating-partners-the-new-partnership-value/?cm_sp=community-_-readnext-_-postpage)
