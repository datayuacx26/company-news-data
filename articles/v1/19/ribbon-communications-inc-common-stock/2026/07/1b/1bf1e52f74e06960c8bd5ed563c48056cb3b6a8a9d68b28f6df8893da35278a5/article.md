---
schema_version: "1.0.0"
document_id: "1bf1e52f74e06960c8bd5ed563c48056cb3b6a8a9d68b28f6df8893da35278a5"
company_key: "ribbon-communications-inc-common-stock"
company: "Ribbon Communications Inc."
source_id: "ribbon-communications-inc-common-stock-news-import-9103da99152d"
canonical_url: "https://ribboncommunications.com/company/media-center/blog/helping-businesses-manage-blocked-calls-how-sip-603-improves-transparency-troubleshooting-call"
published_at: null
first_seen_at: "2026-07-22T11:51:43.108752+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:5ea099f446077cd607d218f71b38c061827f5a6f93f2d029c8783006618747c4"
---

# Helping Businesses Manage Blocked Calls: How SIP 603+ improves transparency in troubleshooting Call Failures

Imagine pulling up to a gas pump, inserting your credit card, and having the display on the pump say “denied”. You call your credit card company, and they say, “Oh, we don’t know, maybe it’s the merchant’s fault, or the card reader is bad…, we can look into it and get back to you in a few weeks.” Most of us would be pretty upset with that response.


Unfortunately, until recently, if a business placed calls to its customers and those calls were blocked, the business might have had to wait weeks to find out what was happening. This was not because their service provider wasn’t interested; it was because the reason for the blocked calls was hard to uncover.


Historically, blocked calls could fail without a clear explanation. Different service providers returned different SIP error codes, such as 603 (Denied), 607 (Unwanted), or 608 (Rejected). None of these cryptic codes clearly indicated that a service provider was intentionally blocking the call. It could just as easily have been another network failure that drove the response, leaving the originating service provider and enterprises in the dark about why their calls were not reaching their customers. Was the call blocked due to suspected spam, an invalid number, or a routing issue? Troubleshooting frequently involved deep log analysis using data that lacked complete context. Too often, the business owner was completely in the dark as to the cause and, more importantly, how to resolve the issue.


As part of its March 2025 order targeting unlawful robocalls, the Federal Communications Commission (FCC) introduced an important change to how reason codes (like 603) for blocked calls are communicated across networks. The FCC required support for a new and enhanced SIP response code, “603+”. While it may sound like a small technical update, it represents a meaningful step toward providing greater clarity and consistency across the telecommunications ecosystem.


### What is the SIP 603+ code?


SIP 603+ removes ambiguity as to the reason a call was blocked.


The SIP 603+ response builds on the standard SIP 603 “Decline” message and introduces two important enhancements. First, it replaces the generic SIP 603 reason phrase “Decline” with the more explicit “Network Blocked,” making the rejection's intent immediately clear. Second, SIP 603+ includes a SIP Reason header. The reason header adds context, including:


- **Cause Parameter (Code):** Indicates the specific, machine-readable reason for the block, such as Q.850 cause 21 (call rejected)
- **Location:** Identifies *where* in the network the call was blocked, such as the originating network, transit network, network serving the called party, etc.
- **Text/Redress Information:** Provides a reason text, URL, email, or telephone number, allowing the caller to contact the entity responsible for blocking the call to resolve potential inaccuracies.


When a call is blocked using “reasonable analytics” (i.e., the service provider believes the call is not legitimate), the service provider that terminates the call must return 603+, and that response must be passed unchanged across the entire call path. Simply put, 603+ clearly signals that a call was blocked by policy, and not by accident.


This standardization replaces guesswork with a single, consistent indicator that improves visibility, speeds troubleshooting, and supports faster redress when legitimate traffic is affected.


### When does the SIP 603+ go into effect, and how can providers be ready for it?


The requirement goes into effect on March 25, 2026, a year after the rule was passed, giving providers time to update systems, test interoperability, and align signaling, analytics, and operational workflows. Supporting 603+ is not just a configuration change; it requires coordination across the network.


### Ribbon is Ready


Ribbon is ready for SIP 603+. Ribbon has updated its product portfolio, including[Session Border Controller](https://ribboncommunications.com/products/service-provider-products/session-border-controllers-service-providers) (SBC),[Centralized Policy and Routing Engine (PSX)](https://ribboncommunications.com/products/service-provider-products/policy-and-routing-service-providers) ,[Call Trust](https://ribboncommunications.com/products/service-provider-products/identity-assurance/ribbon-call-trust-services) , and[C15 Compact Softswitch](https://ribboncommunications.com/products/service-provider-products/call-controller/c15-call-controller) to support the FCC‑mandated SIP 603+ response code. This ensures clear, standards‑compliant blocked‑call signaling across IP and mixed networks. These updates help customers meet regulatory requirements while improving call transparency, reducing operational complexity, and protecting legitimate traffic.


[Contact an Expert Today](https://ribboncommunications.com/resource/contact-expert-mobile-network-solutions)
