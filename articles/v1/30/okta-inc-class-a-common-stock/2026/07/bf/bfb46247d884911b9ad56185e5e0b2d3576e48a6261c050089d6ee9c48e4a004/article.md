---
schema_version: "1.0.0"
document_id: "bfb46247d884911b9ad56185e5e0b2d3576e48a6261c050089d6ee9c48e4a004"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/identity-security/replace-dd-2875-automated-icam-workflows/"
published_at: "2026-07-09T07:00:00+00:00"
first_seen_at: "2026-07-22T07:05:37.626494+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:499b2507e29607c60a584effce2e6a3e06663ad86645731859a77e890653a4ea"
---

# Accelerating system access at mission speed: Retiring legacy SAAR to improve operational efficiency

### Topics


---


Cybersecurity


,


Data Privacy


### Table of Contents


---


---


### Share


-
-
-


---


Ready to make Identity a business advantage? Sign up today.


[Get started](https://www.okta.com/free-trial/)


For decades, the System Authorization Access Request (SAAR) Form DD-2875 has been a common Department of War (DoW) rite of passage. Yet, as our operational environments shift toward cloud architectures, multi-domain operations, and aggressive timelines, the traditional DD-2875 process has become a clear liability. Its reliance on manual data entry, sequential paper or email-based approvals, and static documentation cannot scale to modern mission demands.


Fortunately, change is underway. The DoW Chief Information Officer (CIO) recently published a landmark memorandum and an accompanying[ICAM Workflow Implementation Guide](https://dowcio.war.gov/Portals/0/Documents/Library/ICAMWorkflowImplementationGuide.pdf) . This directive spells out an undeniable mandate: The department must retire the archaic, paper-based legacy DD-2875 process and replace it with modern, automated, enterprise-wide access provisioning and access governance workflows.


The DoW’s new vision moves beyond turning paper forms into digital PDFs; it fundamentally restructures access around an "automation-first, attribute-driven" paradigm. To achieve this, defense agencies must stop trying to bolt disparate identity tools together and instead leverage a converged, unified identity security fabric.


**TLDR:** If you’re excited about this modernization, Okta is here to help by providing a comprehensive identity security platform, not just another ICAM tool.


### Watch: Modernizing the SAAR workflow with Okta Identity Governance


Watch this demo to learn how[Okta Identity Governance](https://www.okta.com/products/identity-governance/) can help navigate changes to the SAAR Form DD-2875.


## The vision: Automated account provisioning and access governance


The recently issued ICAM Workflow Implementation Guide sets a high bar for what modern mission readiness looks like. The goal is to replace days-long administrative processes with automated workflows that provision access within hours—or minutes—while dramatically lowering security risk.


According to the guide, all modernized SAAR workflows must adhere to several strict technical and operational pillars:


- **Utilize authoritative attribute stores:** Systems must dynamically connect to authoritative identity sources, like the Defense Information Systems Agency (DISA) and the Defense Manpower Data Center (DMDC), to validate user records, clearances, and training certifications in real time before granting access.
- **Automate the full lifecycle:** Systems must seamlessly handle joiner, mover, and leaver (JML) events. Crucially, when an individual separates from an organization (a "leaver" event), their access must be completely disabled across all connected systems within 24 hours of notification.
- **Enforce risk-based, attribute-driven decisions:** Automate low-risk, standard operational access based on verified attributes (attribute-based access control, or ABAC). Human intervention and manual approvals should be reserved for exceptional, high-risk, or privileged access scenarios.
- **Support defensive cyber operations (DCO):** Every identity event—from the initial request and attribute validation to automated provisioning and eventual deprovisioning—must generate rich, immutable telemetry and logs to fuel continuous monitoring and threat hunting.
- **Deliver an exceptional user experience:** Workflows must be highly configurable, Section 508 compliant, and completely intuitive, requiring minimal training for the end warfighter.


Meeting this mandate requires a modern foundational approach to identity architecture.


## The evolution of identity: Okta’s converged identity security fabric


To understand how we meet this vision, it helps to look at our own journey. For years, Okta was known primarily as a world-class provider of single sign-on (SSO) and multi-factor authentication (MFA).


As enterprise and defense environments grew more complex, organizations repeatedly tried—and failed—to build their own identity architectures by bolting together disparate point solutions. They attempted to link legacy directory services, standalone identity providers (IdPs), isolated MFA, complicated governance tools, siloed access request systems, and independent security response engines.


The result was always the same: immense architectural friction, dangerous data latency, high maintenance costs, low adoption among application owners, and severe security blind spots. We realized that treating identity as a series of isolated gates simply doesn't work.


True identity security requires a converged platform approach that secures the full lifecycle—pre-, during-, and post-authentication. This evolution led to our unified[identity security fabric](https://www.okta.com/solutions/identity-security-fabric/) , which shifts the focus from individual product silos (such as SSO and MFA) to delivering fully integrated, critical capabilities as enterprise functions.


### Core capabilities of Okta’s identity security fabric


- **Comprehensive environment integration:** Natively binding together directory, IdP, provisioning, and governance functions across multi-cloud infrastructure, legacy on-premise networks, private APIs, and diverse identity types—including standard human personnel, non-human accounts, and autonomous AI agents.
- **Automated lifecycle and access governance:** Orchestrating the end-to-end user lifecycle through low-code automation, ad-hoc access request routing, and fine-grained user entitlements.
- **Adaptive contextual authentication:** Continuously verifying user identity and device health at the moment of access, dynamically adjusting security friction based on factors such as real-time risk parameters, attributes, roles, and device context.
- **Continuous post-authentication threat response:** Persistently evaluating session risk *after* login has occurred, feeding immediate telemetry to DCO, and automatically revoking access across the enterprise the moment a threat is detected.
- **Immutable, non-repudiable auditing:** Guaranteeing a continuous, unchangeable stream of identity event data for audit, compliance, and DCO.


## Streamlining the SAAR: Mapping Okta to the implementation guidance


When we look specifically at replacing the DD-2875 process,[Okta Identity Governance (OIG)](https://www.okta.com/products/identity-governance/) is the engine that turns the DoW’s vision into reality. OIG maps directly to the ICAM Workflow Implementation Guide's core requirements by executing an automation-first strategy:


### Constructing the "identity cube" via authoritative sources


Rather than relying on manual data entry,[Okta Universal Directory](https://www.okta.com/products/universal-directory/) has a profile sourcing feature to aggregate attributes from trusted military data stores, including DISA, DMDC, Active Directory, and HR systems. This constructs a dynamic, multi-dimensional "identity cube" for every user. When an authoritative store updates a warfighter’s attribute—for example, a cyber awareness training or security clearance status—Okta immediately recognizes that change.


### Automating the JML lifecycle and the 24-hour rule


[Okta Lifecycle Management](https://www.okta.com/products/lifecycle-management/) entirely automates JML events. When you register a personnel change, Okta instantly provisions or modifies application access via downstream APIs. More importantly, when a leaver event triggers, Okta instantly deprovisions access across all connected cloud and on-premises systems simultaneously—easily exceeding the DoW’s strict 24-hour account disablement requirement and eliminating the risk of orphan accounts.


### Automation-first, risk-based approvals


OIG enables administrators to build highly configurable access request paths. For everyday, low-risk tools, OIG uses attribute-driven logic to auto-approve and auto-provision access the moment a user joins a specific unit or role. If a user needs elevated or high-risk access, OIG seamlessly intercepts the request and enforces automated multi-stage routing to the resource owner or supervisor for manual attestation.


### Frictionless user experience


Warfighters don't have time to navigate clunky, legacy portals. OIG allows personnel to request system access directly from their[Okta SSO dashboard](https://www.okta.com/products/single-sign-on-workforce-identity/) or via standard communication channels like Microsoft Teams or Slack. The interface is intuitive, fully Section 508-compliant, and requires virtually no training.


## The disparate tool approach vs. the native platform advantage


Many defense components attempt to meet these new guidelines by taking a legacy approach: They string together a legacy directory, a separate third-party IdP for SSO, and a standalone access request or ticketing tool. From an architectural standpoint, this introduces severe integration friction—what we call the "integration tax."


When you use disparate tools, your access request solution is inherently desynchronized from your authentication solution. Administrators must constantly synchronize data via fragile, scheduled batch jobs or custom API connectors. If a connector breaks or sync latency delays a record update, a warfighter might wait days for access, directly impacting operational velocity. Furthermore, troubleshooting access issues across three different vendor platforms creates immense administrative overhead and introduces security blind spots.


By contrast, performing access requests natively from within the[Okta Identity Platform](https://www.okta.com/products/workforce-identity/) eliminates the integration tax entirely. Because access requests, identity attributes, and authentication policies all live natively within the same platform, there is zero data latency. The platform evaluates policies in real time and takes action instantly, letting administrators manage everything from a single, unified pane of glass.


While we advocate for a unified platform approach, we recognize that transition takes time. If you must retain certain third-party tools, the[Okta Integration Network](https://www.okta.com/solutions/okta-integration-network/) easily connects with more than 8,200 integrations. Combined with no- and low-code[Okta Workflows](https://www.okta.com/products/workflows/) , Okta provides a flexible bridge to help you modernize at your own pace.


## Completing the fabric: Continuous auditing and true Zero Trust


Replacing the DD-2875 access request workflow is an essential first step, but a true identity security fabric does not stop there. Within Okta Identity Governance, we offer two other critical capabilities that complete the governance lifecycle:


- **Access certification campaigns:** Compliance cannot be a once-a-year check. OIG allows organizations to run automated, scheduled, or event-driven access certification campaigns. Supervisors and data owners are automatically prompted to review and re-attest their personnel’s active permissions. If an employee has shifted roles or no longer requires a privilege, access is automatically revoked, providing an absolute, unchangeable audit trail.
- **Entitlement management:** Modern systems are complex, and users often accumulate fine-grained permissions over time. Okta’s Entitlement Management provides granular governance across applications and infrastructure, enforcing critical separation-of-duties (SoD) checks. This ensures that no single identity accumulates a toxic combination of entitlements that could compromise system integrity.


### The power of a single converged platform


The true magic happens when you bring authentication (SSO/MFA), access requests, access certifications, and entitlement management onto one converged platform. In a legacy, fragmented environment, your authentication tool can’t see what access a manager approved in a ticketing system, and your auditing tool can’t detect if a user is actively abusing an old privilege.


With Okta’s integrated fabric, those walls crumble. Because authentication and governance share the same context, your DCO team gets a continuous, unchangeable telemetry stream detailing exactly *who* requested access, *why* they were approved, *what* entitlements they hold, and *how* they use them at the moment of login, thereby providing a complete non-repudiable audit trail.


By unifying these functions, the DoW can finally move away from the administrative burden of paper documentation and fully realize a secure, agile, post-authentication[Zero Trust](https://www.okta.com/solutions/zero-trust/) posture. The era of the manual DD-2875 is over—replaced by modern, automated, and secure identity orchestration built for the modern warfighter.


## Modernize your SAAR workflows with Okta


See how Okta’s unified identity security fabric can help you meet the new ICAM requirements with an automation-first strategy.


[Contact sales](https://www.okta.com/contact-sales/)


About the Author


[Sabrina Lea Director, DoD/IC Programs Sabrina Lea is a computer engineer and enterprise software solutions leader, with over 15 years of experience in the technology industry. At Okta, she focuses on bringing Modern Identity solutions to mission critical government organizations inside the Department of Defense. Prior to Okta, Sabrina worked as an Account Manager and a Solutions Engineer at Splunk, where she was dedicated to helping national security customers use data analytics to support their IT, cybersecurity, and mission requirements. Sabrina has also served in a number of engineering and technical leadership positions at NSA, USCYBERCOM, and Booz Allen Hamilton, focused on data analytics as well as offensive and defensive cyberspace operations. She holds an MBA from Georgetown University, an MS from National Defense University, and a BS in Computer Engineering from University of Maryland Baltimore County. She's a native of Louisiana (Geaux Tigers!), and she currently lives in Northern Virginia with her husband and two children.](https://www.okta.com/blog/author/sabrina-lea/)


[Tony Ardura Senior Solutions Engineer Tony Ardura graduated from Virginia Tech with a degree in computer science. He has spent his entire career supporting the entire U.S. Federal Government; although he has primarily focused on supporting the missions of the U.S. Army, U.S. Air Force, and U.S. Intelligence Community. Tony has served as a software developer, solutions engineer, and solutions architect for a variety of both GOTS and COTS solutions. His background is primarily in Identity, data governance, security, and software development.](https://www.okta.com/blog/author/tony-ardura/)
