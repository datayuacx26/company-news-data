---
schema_version: "1.0.0"
document_id: "ba015c9bfc30343bf6c0c7e22476f2ae7beb12959204380a990d0a5f2aaddb68"
company_key: "yc-bland-ai"
company: "Bland AI"
source_id: "yc-bland-ai-news-import-c77b84ac3c61"
canonical_url: "https://www.bland.ai/blog/new-event-based-automations-trigger-ai-calls-when-a-salesforce-lead-record-changes"
published_at: "2026-01-16T15:00:00+00:00"
first_seen_at: "2026-07-21T10:41:07.173500+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:0e95d72f025d97a4b0587fd2e53cece2a0b05a537bab601c84c06ac1dbc19fce"
---

# New Event-Based Automations: Trigger AI Calls When a Salesforce Lead Record Changes

[Back to blog](https://www.bland.ai/blog)


# New Event-Based Automations: Trigger AI Calls When a Salesforce Lead Record Changes


Announcing new event-based automations


[Kyle Dias](https://www.bland.ai/blog/author/kyle-dias) January 16, 2026


Updated June 16, 2026


3 min read


Today, we’re excited to announce a new feature that makes it easy for teams to automatically trigger AI calls when a Salesforce lead record changes.


While Salesforce is a common use case, this feature isn’t limited to it. You can use the same event-based logic with **any external integration** —no manual workflows, brittle workarounds, or custom engineering required.


This feature helps:


- **Sales teams** respond instantly to high-intent changes
- **RevOps teams** enforce consistent follow-up without manual workflows
- **Support and CX teams** trigger proactive outreach based on case updates


Most importantly, it ensures that *important moments don’t get missed* and that follow-ups happen exactly when they matter most.


## **Automate calls directly from Salesforce**#


At a high level, the feature is built around three core components:


**1. Events**


Events define *when* something happens. In Salesforce, this could be:


- A lead, opportunity, or case being created
- A record being updated


**2. Conditions**


Conditions define whether an action should occur. These are based on your custom Salesforce fields. For example, if your event is “lead created,” Bland automatically pulls in all custom fields on the lead record. You can then specify criteria such as revenue, lead source, pipeline stage, or any other field on the lead record. **3. Actions**


Actions define *what happens next* . Today, this includes:


- Triggering an AI phone call with Bland
- Sending Slack notifications
- Firing webhooks


This sounds simple but the power comes from how precise and flexible it is. Some examples of when teams use event-based calling include:


- A lead’s annual revenue crosses a defined threshold
- A deal moves into a specific pipeline stage
- A customer case requires follow-up information
- A meeting is canceled and needs to be rescheduled


Only when your defined conditions are met does the call go out, ensuring relevance and avoiding unnecessary outreach.


## **Built for Second Touches and Follow-Ups**#


One of the biggest benefits of this feature is the ability to route contacts into **different call pathways** based on the event and conditions.


Previously, teams relied on a single call flow and handled follow-ups manually or through complex internal logic. Now, Salesforce events can automatically route contacts into the right pathway based on where they are in their lifecycle.


That means:


- Initial inbound calls can follow one pathway
- Follow-up calls can use a different pathway
- Reschedules, clarifications, or case updates can each trigger their own tailored experience


For example:


- A meeting is canceled → automatically trigger a rescheduling call
- A customer case changes status → trigger a follow-up to gather missing details


All of this happens automatically, driven by CRM state, not human memory.


## **See It in Action**#


We’ve recorded a short walkthrough showing how to:


- Select Salesforce events
- Define conditions using your custom fields in Salesforce
- Trigger calls and route them into different pathways


👉 Watch the Loom demo to see how it works end-to-end. Note: This walkthrough assumes familiarity with Salesforce and CRM workflows.


‍


## **What’s Next**#


This release focuses on **phone calls** as the primary action. Additional actions, such as SMS, are planned for a future release.


If you’re already using Bland, this is the fastest way to add sophisticated follow-ups and multi-touch workflows. If you’re new to Bland, this is a powerful example of how AI calling fits directly into existing sales and ops infrastructure.


We’re excited to see how you use it!
