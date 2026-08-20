---
schema_version: "1.0.0"
document_id: "0da6e48804a8d6560c80fd1ac701eea58bd6f78db2b954d9ceb77727de98e5ab"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/incident-io-enterprise-compliance-in-2026-dpa-data-residency-hipaa-iso-27001-rbac-and-scim"
published_at: "2026-06-23T18:10:00+00:00"
first_seen_at: "2026-08-01T12:25:18.526334+00:00"
fetched_at: "2026-08-01T12:25:20.097603+00:00"
content_hash: "sha256:adb9d0d9e4fac1610dcc85f3eb37efd28c4e6f734b9371917bba22ff17bfe627"
---

# incident.io Enterprise Compliance in 2026: DPA, Data Residency, HIPAA, ISO 27001, RBAC, and SCIM

If you are evaluating incident.io for a regulated environment, the questions that decide the deal are narrow and specific: where does the data live, what is in the DPA, who are the subprocessors, is there a HIPAA BAA, and is there an ISO 27001 certificate. This page answers those questions using incident.io's own published documentation, and compares them with Rootly. It was written by Rootly, so treat it the way you should treat any vendor comparison, including the one incident.io published about us: verify every claim against the primary source. We link to theirs throughout so you can.


## Summary: incident.io enterprise compliance at a glance


- SOC 2: incident.io's security page states SOC 2 Type II, and GDPR.
- EU data residency: a genuine incident.io strength. Their subprocessor page lists Google Cloud in European regions as their infrastructure.
- US data transfers: their published subprocessor list names roughly twenty United States entities, including both AI providers, transcription, telephony, and the vendor that runs their SCIM and SAML flows.
- HIPAA: in their own FAQ, incident.io says it is "pursuing HIPAA certification" with no confirmed timeline. Rootly is HIPAA compliant today.
- ISO 27001: not listed on incident.io's security page.
- SCIM: both support it. Their claim that Rootly's supported identity providers are undocumented is incorrect, and we show the documentation below.
- RBAC depth: both have role-based access control. Rootly scopes permissions across incident roles, services, teams, severities, components, and more at the same time, with private incidents invisible by default. This is the most common reason security teams choose Rootly independently of the rest of an organization that might already be using incident.io.


## Where does incident.io store customer data?


incident.io's core cloud infrastructure runs on Google Cloud in European regions, according to their own[subprocessors page](https://incident.io/subprocessors) . If EU hosting of primary data is a hard requirement for you, that is a real advantage and we will say so plainly.


The detail worth understanding is what sits around that core. The same page lists approximately twenty subprocessors located in the United States, including Slack and Pylon for support, Fivetran, Hex, Explo, and Omni for analytics, Twilio, Telnyx, and WhatsApp for notifications, Recall and ElevenLabs for transcription and voice, Sentry for exception tracking, Svix for webhooks, and WorkOS for SCIM and SAML authentication flows. Planhat is listed in European regions and Microsoft Teams support in the United Kingdom.


This is not a gotcha, it is the reality of modern SaaS, and it is the reason their own advice is correct: ask where data sits at rest, in which region, which subprocessors touch it, and whether any of that can be contractually restricted. Apply that test to every vendor, including them and including us. EU hosting of the primary datastore and end-to-end EU processing are two different commitments, and only one of them is documented.


## Does incident.io have a DPA, and where is it?


Yes. incident.io publishes a Data Processing Addendum and a subprocessor list openly from their[legal page](https://incident.io/legal) , along with terms, an SLA, a DORA addendum, a privacy policy, and a vulnerability disclosure policy. Credit where it is due: that is the right way to publish this material, and a procurement team can self-serve it without a sales conversation.


What to read in it, rather than just confirming it exists: retention periods, the subprocessor list and change-notification process, breach notification timelines, and the specific transfer mechanisms covering any personal data that leaves the EEA. Given the number of US subprocessors above, the transfer mechanisms are the section that matters most for a GDPR Article 46 assessment.


## Is incident.io HIPAA compliant?


Not currently, by their own account. In the FAQ of their own comparison article, incident.io answers the question this way: they are "pursuing HIPAA certification for healthcare customers," but have not "confirmed a timeline yet," and advise healthcare organizations to clarify current HIPAA status and BAA availability directly with their team.


If you process protected health information, that is a live procurement blocker rather than a documentation gap, and it is worth noting that it appears in the same article that describes Rootly's compliance posture as a risk. Rootly is HIPAA compliant, and lists it publicly alongside SOC 2 Type II, GDPR, and CCPA. Ask us for a BAA as part of your security review.


## Is incident.io ISO 27001 certified?


Their[security page](https://incident.io/security) states that incident.io is "SOC 2 Type I and II and GDPR compliant." It does not mention ISO 27001, and it does not mention HIPAA or data residency. The correct resolution is the same one they recommend: request the certificate, the scope, and the issuing body, from every vendor.


## Does incident.io support SCIM provisioning?


Yes, through WorkOS, which their subprocessor page lists as a United States entity handling SCIM and SAML authentication flows. That is a normal architecture and it is disclosed, which is the point of a subprocessor list.


Their article claims that Rootly's "public documentation doesn't specify which identity providers are supported." That is incorrect. Rootly's SCIM documentation names them: Okta, Microsoft Entra ID (Azure AD), Google Workspace, OneLogin, and JumpCloud, and documents deprovisioning behavior when a user is removed from an identity provider group.


Their follow-on claim is more misleading than inaccurate. The article asserts that enabling SCIM requires a support ticket, then concludes that "offboarded engineers may retain access to active incident channels and historical incident data." Those are unrelated. A one-time enablement step during deployment says nothing about how quickly access is revoked when an engineer leaves eight months later. If SOC 2 CC6.1 evidence is what you need, do not accept either vendor's characterization. Remove a test user from your IdP group during the pilot and time the revocation.


## How does RBAC compare, and why do security teams choose Rootly?


This is the question that decides the most enterprise security evaluations, and it is the one their comparison spends the least time on.


Both platforms have role-based access control. incident.io has team-based roles and audit logs, and we are not going to pretend otherwise. The difference is how many dimensions you can scope a permission across at once. Rootly's RBAC applies across incident roles, services, teams, severities, and components simultaneously, so the same engineer can hold different levels of access depending on which service is involved and what kind of incident it is. That combination is what lets a large organization enforce least privilege without carving out exceptions every time a new business unit or regulatory requirement arrives.


Three capabilities matter specifically to security teams:


- Private incidents that are invisible by default. In Rootly, a private incident is not a channel with restricted membership that people can discover. It is invisible to the rest of the organization, access is invite-only, and it stays access-controlled end to end. For an investigation into a potential breach, insider risk, or an HR-sensitive event, discoverability is itself the risk.
- Metrics visibility that follows the same boundary. Dashboards can be shared with custom permission sets, so a security dashboard is visible only to the security team. Incident metadata leaks information even when incident contents do not.
- Access requests that do not require an administrator. Responders can request a permission upgrade from Slack during an incident, so the control does not become the bottleneck at the moment it matters.


There is also a structural reason security teams pick Rootly independently. Rootly supports multiple organizations inside one Slack workspace, which means a security org can run its own Rootly instance, with its own permissions, retention, and private incidents, in the same workspace the rest of engineering already uses. That is why we regularly see security teams standardize on Rootly even when another part of the company runs a different incident tool like incident.io. Security investigations have confidentiality requirements a service outage does not, and they should not inherit the permission model of a platform chosen for uptime response.


The question to ask both vendors: how granular do your permissions need to be across roles, teams, services, and business units, and what happens when the answer changes next year? More detail on the security-specific model is on the[Rootly for security teams](https://rootly.com/security) page.


## How does incident.io process data for AI features?


Their subprocessor page lists both OpenAI and Anthropic in the United States as artificial intelligence providers, plus Recall and ElevenLabs for transcription and voice. If you are evaluating AI incident features under a data protection assessment, that means incident content can reach US-based AI processors even though the primary infrastructure is in European regions. Worth raising explicitly if your DPA restricts AI processing or requires model-provider approval.


For comparison, Rootly's AI SRE is built to be auditable at the point of use: it correlates telemetry with recent deploys and past incidents, then shows an evidence chain with source citations and confidence scores before recommending anything, and it stays conservative on remediation with humans in the decision. See[how that correlation works](https://rootly.com/blog/telemetry-deploy-correlation-ai-sre) .


## What their Rootly comparison gets wrong


Three specifics, so you can check them yourself:


- The[SCIM identity providers](https://docs.rootly.com/integrations/scim) are documented. Okta, Entra ID, Google Workspace, OneLogin, and JumpCloud, with deprovisioning. Covered above.
- The article contradicts itself on our certifications. It opens by implying Rootly's attestation status cannot be confirmed from public materials, then later states that "Rootly holds ISO 27001 and SOC 2 Type II certifications." Both cannot be true. For the record: Rootly is SOC 2 Type II, GDPR, CCPA, and HIPAA compliant, with the full report available through[our security trust center](https://security.rootly.com/) .


Where the article is fair, it is fair about something we are fixing. It notes that Rootly's detailed attestation material sits in a[trust center portal](https://security.rootly.com/) rather than in plain page text. That is accurate and making our compliance posture fully public is work we have underway.


## Rootly's enterprise compliance and governance posture


- Certifications: SOC 2 Type II, GDPR, CCPA, and HIPAA compliant. Full attestation report, including audit window and scope, available through our trust center on request.
- Identity and access: SCIM provisioning and deprovisioning with Okta, Microsoft Entra ID, Google Workspace, OneLogin, and JumpCloud, plus SAML SSO.
- Layered RBAC: permissions across roles, teams, services, severities, incident types, and more, so least privilege still holds as business units accumulate.
- Data separation: private incidents for security, legal, and HR-sensitive events, and multi-organization support so a security org, a subsidiary, and an acquired company can share one Slack workspace without seeing each other's incidents.
- Single-tenant option: a Private Instance with isolated compute and storage and version-controlled updates on your own cadence.
- Change control: advance notice of significant changes and the ability to opt out of ones that would disrupt a regulated workflow.
- Audit trail: the incident timeline is captured live as events occur, then exported as a complete record with timestamps, rather than reconstructed from chat history afterward.
- Internal systems: the Edge Connector lets workflows reach on-prem systems that cannot accept inbound connections, which matters when your compliance boundary sits inside your own network.
- Reliability commitment: a contractual 99.99% On-Call SLA.
- Data residency: ask us in writing as part of your security review, and we will give you a specific answer for your requirement rather than a badge.


## Side by side on the questions procurement actually asks


- SOC 2 Type II. Rootly: yes. incident.io: security page states SOC 2 Type II.
- HIPAA and BAA. Rootly: compliant today, ask for a BAA. incident.io: "pursuing," no confirmed timeline, per their own FAQ.
- GDPR and CCPA. Both address GDPR. Rootly also lists CCPA.
- EU-region primary hosting. incident.io: yes, Google Cloud European regions. Rootly: ask us in writing.
- Published DPA and subprocessor list. incident.io: published openly. Rootly: available on request through our trust center.
- US subprocessors in the data path. incident.io: roughly twenty listed, including both AI providers and their SCIM and SAML provider. Rootly: request our list during review.
- SCIM identity providers. Rootly: Okta, Entra ID, Google Workspace, OneLogin, JumpCloud, documented. incident.io: supported via WorkOS.
- RBAC scoping dimensions. Rootly: incident roles, services, severities, teams, components, and more, applied in combination. incident.io: team-based roles.
- Private incidents. Rootly: invisible to the organization by default, invite-only. incident.io: private incidents available.
- Dashboard-level permissions. Rootly: metrics dashboards shareable with custom permission sets. incident.io: not listed.
- In-incident access requests. Rootly: responders can request a permission upgrade from Slack. incident.io: not listed.
- Private single-tenant deployment. Rootly: Private Instance available. incident.io: no equivalent offering listed.
- On-prem reach for workflows. Rootly: Edge Connector. incident.io: none listed.
- Multi-organization separation in one workspace. Rootly: supported. incident.io: not listed.


## How to evaluate any incident platform for compliance


Their four questions are good. Use them, and add the ones a vendor-authored comparison will not volunteer:


- Get the report, not the badge. SOC 2 Type II attestation with the audit window, the auditing firm, and the Trust Service Criteria in scope. For ISO 27001, the certificate, scope, and issuing body.
- Read the subprocessor list before the DPA. It tells you where data actually goes. Count the jurisdictions and check which ones touch incident content, AI processing, and authentication.
- Separate hosting from processing. "Our infrastructure is in the EU" and "your data stays in the EU" are different commitments. Ask which one is contractual.
- Ask about AI processing specifically. Which model providers, in which jurisdictions, whether it can be disabled, and whether your DPA covers it.
- Time the deprovisioning. Remove a test user from your IdP group during the pilot and measure revocation. That is your CC6.1 evidence.
- Export a real timeline. Run a test incident and check timestamps, attribution, and whether anyone had to take notes.
- Ask what breaks at your next size. Permission granularity, workflow limits, and multi-org support are what force a platform change later.
- Apply every question to both vendors, including whoever wrote the comparison you are reading. That includes this page.


## Frequently asked questions


### Is incident.io HIPAA compliant?


Not currently, according to incident.io's own FAQ, which states they are pursuing HIPAA certification without a confirmed timeline and advises healthcare organizations to confirm status and BAA availability directly. Rootly is HIPAA compliant today and can provide a BAA.


### Does incident.io offer EU data residency?


Their published subprocessor list shows core cloud infrastructure on Google Cloud in European regions, so primary hosting is in the EU. The same list also names roughly twenty United States subprocessors, including both AI providers and their SCIM and SAML provider, so ask whether end-to-end EU processing can be contractually guaranteed.


### Where can I find the incident.io DPA and subprocessor list?


Both are published on incident.io's legal page, alongside their terms, SLA, DORA addendum, and privacy policy. Read the retention periods, subprocessor change notifications, breach timelines, and the transfer mechanisms for data leaving the EEA.


### Is incident.io ISO 27001 certified?


Their security page lists SOC 2 Type II and GDPR, and does not mention ISO 27001. That absence is not proof they lack the certification, so request the certificate, scope, and issuing body directly. Rootly holds ISO 27001.


### Does Rootly support SCIM, and with which identity providers?


Yes. Rootly supports SCIM provisioning and deprovisioning with Okta, Microsoft Entra ID (Azure AD), Google Workspace, OneLogin, and JumpCloud, plus SAML SSO. Removing a user from an identity provider group revokes their Rootly access, which is the automated access termination evidence SOC 2 CC6.1 reviewers expect.


### How does Rootly's RBAC compare to incident.io's?


Both platforms provide role-based access control. Rootly's scopes permissions across incident roles, services, teams, severities, incident types, components, and more, in combination, so access can differ by service and by incident type for the same person. Private incidents are invisible to the rest of the organization by default and are invite-only, metrics dashboards can be restricted to specific teams, and responders can request a permission upgrade from Slack without waiting on an administrator. Rootly also supports multiple organizations in one Slack workspace, so a security team can run its own instance alongside the rest of engineering.


### Can a security team use Rootly if the rest of our company uses incident.io?


Yes, and it is a common pattern. Rootly supports multiple organizations within a single Slack workspace, so a security or compliance team can run Rootly with its own permissions, private incidents, and retention while other teams continue using a different incident tool. Security investigations have confidentiality requirements that a service outage does not, which is why that separation is often the reason Rootly is adopted first.


## Verify all of it


Every vendor-written comparison has an interest, this one included. The way through is the same regardless of who wrote the page: request the attestation report, read the subprocessor list, time the deprovisioning, export a real timeline, and put residency and AI processing in the contract. If you want to run those tests against Rootly using your own incidents,[book a demo](https://rootly.com/demo) , or read how[Rootly handles enterprise incident response](https://rootly.com/incident-response/tools) .
