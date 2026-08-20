---
schema_version: "1.0.0"
document_id: "fadee3d76d0a1beaefad310d31fd84894441557704863bbf2802dc3f43012f3a"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/identity-security/risk-based-identity-governance-okta-workflows/"
published_at: "2026-07-14T07:00:00+00:00"
first_seen_at: "2026-07-22T07:05:37.626494+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:85f757289881b833f677f010d8197d0a2fbfd4bf401d7c6d7bb59a670065f43d"
---

# Automating modern governance: Triggering access certifications with Okta Identity Threat Protection Workflows

**TL;DR:** By combining Okta Identity Threat Protection with Okta Workflows, organizations can transition from static, scheduled access reviews to dynamic, real-time access certifications based on user risk, ensuring immediate remediation of potential threats.


Managing identity governance often relies on scheduled campaigns—typically quarterly or annually—to ensure users have appropriate access levels. But in today's fast-paced threat landscape, waiting for the end of the quarter to review a compromised or risky user just doesn't cut it.


What if you could trigger an access review the exact moment a threat is detected?


By combining[Okta Identity Threat Protection](https://www.okta.com/products/identity-threat-protection/) with[Okta Workflows](https://www.okta.com/products/workflows/) , you can move from static, scheduled reviews to dynamic, real-time access certifications based on user risk. Instead of just reviewing access when the calendar dictates, you can review it exactly when a threat profile changes.


## The architecture: From real-time risk to policy-driven action


Before exploring specific features, here is a high-level look at how real-time risk signals seamlessly flow into policy-driven governance actions.


### The real-time risk architecture pipeline


Flowchart of real-time risk signals from Okta Identity Threat Detection triggering dynamic certification campaigns to Okta Workflows.


Fill out the form to access this content.


To understand how these systems communicate, here’s the technical sequence of the automated governance loop:


1. **Risk ingestion:** Okta Identity Threat Protection evaluates continuous risk signals via first-party behavioral analytics or third-party vendors.
2. **Event triggers:** An entity risk level change (like from low to high) fires an asynchronous event hook in Okta.
3. **Workflow orchestration:** Okta Workflows captures the event via the “Risk Level Changed” trigger card.
4. **Context enrichment and evaluation:** The workflow executes logic checks, fetches user attributes, and evaluates the risk context.
5. **Governance execution:** If conditions are met, the workflow invokes the Okta Identity Governance API to instantly spin up a targeted micro-campaign for that specific user ID.


## Key capabilities of the Okta governance engine


Here are three features from Okta's governance engine that make this modern approach a total game-changer.


### 1. Dynamic certification campaigns triggered by risk


This is where the true magic of automation happens. Instead of relying solely on a schedule, Okta Workflows lets you dynamically generate a targeted access certification campaign.


A workflow initiates instantly when a user’s risk profile elevates. This can happen due to a session hijacking attempt detected by Okta’s first-party signals or a shared signal from an external security provider.


The workflow evaluates the user's risk level. If it identifies a high risk, Okta automatically launches a "Risky User Campaign" specifically for that individual and immediately pings your security team in a Slack channel. You get an instant, targeted security review exactly when it matters most.


### 2. Deep context for reviewers (hello, software rationalization!)


Reviewers, such as managers or resource owners, need actionable data rather than a blind list of applications when alerted to review a risky user. Okta’s[Governance Analyzer](https://www.okta.com/products/identity-governance/) provides incredible context so they can make an informed, confident decision.


One standout feature is the license utilization data. For instance, the review dashboard might flag a user who has a Microsoft Office 365 license but hasn't actually logged in for the past 90 days. This not only shrinks the attack surface from a security perspective but is also a massive win for software rationalization. You can easily spot and revoke unused licenses, saving your organization significant money in the process.


### 3. Built-in separation of duties checks


Access reviews aren't just about whether a user needs access; they must evaluate whether that access creates a toxic combination. The Okta review dashboard automatically runs checks against your organization's separation of duties (SoD) policies.


For example, if a risky user holds both "user administrator" and "billing administrator" roles, the platform immediately flags this SoD conflict for the reviewer. Coupled with machine learning-driven recommendations on whether to approve or revoke access, reviewers are perfectly equipped to mitigate compliance risks on the spot.


## Ready to build it yourself?


The demo video below walks you through the workflow setup and outcome of a triggered risk event.


### Demo: See the full governance workflow in action


### Get started: Download the Okta Workflows Template


Building this automation is straightforward using the drag-and-drop Okta Workflows console. To jumpstart your deployment, we’ve packaged the entire logic string into a pre-built template.


Once downloaded, simply upload the \`.flow\` package directly into your Okta Workflows console, map your connection hooks, and activate it.


Download the Okta Workflows Template:[Dynamic Risk-Based Access Certification](https://okta.com/sites/default/files/2026-07/genrateAccessCertificationCampaignBasedOnRisk.flow_.zip)


#### Technical blueprint and workflow manifest


For solutions architects and engineers looking to review the logic structure before importing, the \`.flow\` template executes the following underlying blueprint schema:


```text
```json


```


```text
{


```


```text
"workflowName": "Generate Access Certification Campaign based on Risk",


```


```text
"triggerEvent": "oktaitp:user.risk.detect (User Risk Detected)",


```


```text
"orchestrationModules": {


```


```text
"identityThreatProtection": [


```


```text
"retrieveUserRisk_JrLod3cJI8"


```


```text
],


```


```text
"identityGovernance": [


```


```text
"createUserCampaign_58rXvVJ_r",


```


```text
"launchACampaign_dSfl7xoInP"


```


```text
],


```


```text
"notifications": [


```


```text
"slack:sendMessageToChannel2"


```


```text
],


```


```text
"systemLogic": [


```


```text
"control:if",


```


```text
"control:join",


```


```text
"string:compose",


```


```text
"date:now",


```


```text
"date:add"


```


```text
]


```


```text
}


```


```text
}


```


By leveraging Identity Threat Protection signals to drive automated governance actions, you can finally bridge the gap between reactive security and proactive identity management. It's time to let automation do the heavy lifting!


### Prerequisites


To bring this automation to life, you'll need to license these Okta solutions in your tenant:


- [Okta Identity Threat Protection](https://www.okta.com/products/identity-threat-protection/)
- [Okta Workflows](https://www.okta.com/products/workflows/)
- [Okta Identity Governance](https://www.okta.com/products/identity-governance/)


Additionally, enable the following API scopes for your integration:


- okta.accessRequests.request.manage
- okta.accessRequests.request.read
- okta.governance.accessCertifications.manage
- okta.governance.accessCertifications.read
- okta.governance.accessRequests.manage
- okta.governance.accessRequests.read
- okta.userRisk.read
- okta.userRisk.manage


### Pro-tip for workflow configuration


When configuring the “Create User Campaign” card in your workflow, you have a lot of flexibility with reviewer assignments. You can select a reviewer directly from the dropdown (such as the user’s manager, a specific group, or individual users) and hardcode a fallback reviewer ID, or you can build custom logic to dynamically assign the reviewer based on the situation.


About the Author


[Srikanth Rajan Senior Solutions Engineer Sri Rajan is a Senior Solutions Engineer with over 15 years of experience designing and delivering identity and access management solutions. Throughout his career, he has partnered with organizations across the healthcare, telecommunications, and payment gateway industries to implement secure identity, access, and governance strategies. Passionate about helping customers solve complex security challenges, Sri brings practical, real-world insights into modern identity architectures, governance, and zero trust best practices](https://www.okta.com/blog/author/srikanth-rajan/)
