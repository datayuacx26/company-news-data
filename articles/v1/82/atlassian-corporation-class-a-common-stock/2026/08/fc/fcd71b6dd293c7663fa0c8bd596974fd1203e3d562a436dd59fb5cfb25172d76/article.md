---
schema_version: "1.0.0"
document_id: "fcd71b6dd293c7663fa0c8bd596974fd1203e3d562a436dd59fb5cfb25172d76"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/ai-at-work/connect-salesforce-to-the-teamwork-graph"
published_at: "2026-08-17T17:11:34+00:00"
first_seen_at: "2026-08-18T05:54:03.979956+00:00"
fetched_at: "2026-08-18T05:54:05.264475+00:00"
content_hash: "sha256:a812b35bc81b4a39dd66490d2dff319b7f78dced815f7817fca53c91b4a039b9"
---

# Connect Salesforce to the Teamwork Graph to unlock customer context for your teams and agents

Customer work rarely lives in one place. Salesforce holds the account, opportunity, contact, and case details. Jira shows the product work behind a customer commitment. Confluence holds the account plan, enablement materials, and meeting notes. Jira Service Management tracks support requests and escalations. Slack, Microsoft Teams, Google Drive, and other tools hold the conversations and assets that explain what’s really happening.


The hard part isn’t that teams lack information; it’s that the information is scattered. Before an account call, pipeline review, escalation, or campaign-planning session, people rebuild the customer picture by hand, switching tabs and stitching context together.


Once an admin has configured and enabled the Teamwork Graph connector for Salesforce, Salesforce context that respects existing permissions can be searched and synthesized alongside your Atlassian work and knowledge, so teams can ask better questions and prepare faster, with the full picture in one place.


## The opportunity: connected customer context


Customer-facing teams make better decisions when they can see the customer record and the work around it together:


- **Sales** can find the latest opportunity, see what’s changed, and spot related product work or support cases before the conversation.
- **Customer success** can catch renewal risk earlier by combining account details, case activity, open work, and internal notes with features being built in Jira or product roadmaps in Confluence.
- **Marketing** can connect campaign planning to customer segments, field feedback, and launch materials.
- **Support** can triage high-priority cases with account context already in view.


Salesforce tells you what’s happening with the customer. Atlassian shows the work, knowledge, and collaboration around that customer. Teamwork Graph lets teams find and understand that context in the flow of work.


## The power of a connected experience


**How to read this table:** The first three approaches are useful on their own, but each leaves part of the customer picture disconnected. The final row is the ideal state: a connected experience that brings Salesforce context together with Atlassian teamwork context.


Approach Status Good at Where teams still get stuck


Salesforce alone Works well on its own CRM records, opportunities, contacts, cases, and pipeline data May not show the product, support, knowledge, or collaboration context around the account


Atlassian apps alone Works well on its own Planning, delivery, service, and documentation Customer and pipeline context lives elsewhere, in the CRM


Collaboration tools alone Works well on its own Fast-moving conversation and informal knowledge Hard to find, summarize, or connect back to the customer record


**Connected experience powered by Teamwork Graph with Salesforce context** **Ideal State** **Finding and reasoning across CRM and teamwork context together** **Teams need the Salesforce Connector enabled and active Salesforce licenses for seamless access**


The goal isn’t to replace the tools teams already use. It’s to connect customer context to the work record so teams can find and understand it with less manual reconstruction.


## Three ways Teamwork Graph helps you find and understand customer context


### 1. Find the right records


Search and chat across Salesforce objects alongside your company knowledge. For example:


> **“Find enterprise accounts with open cases this month.”**
>
>
> ***“What campaigns are related to this account segment?”***


> *“Which Atlassian customer accounts have open opportunities but unresolved support cases that could put the deal at risk?”*


### 2. Prepare with the full picture


The highest-value context is the relationship between records, work, documents, cases, and people. Before a customer call, ask for a brief that pulls it together:


> *“Prepare me for my Northstar Labs call. Include open opportunities, recent account notes, open support issues, customer-facing commitments, and anything stale that might need attention.”*


That turns a scattered prep workflow into a single question, so teams spend more time on the conversation and less time hunting for context.


### 3. Answer questions across your book of business


Ask aggregate questions and get a synthesized answer across many records, instead of exporting and cross-referencing by hand:


> *“Which opportunities over $100k haven’t moved in 30 days?”*
>
>
> **“Which of Florence Garcia’s renewals have open support cases?”**
>
>
> ***“Summarize the state of our top 3 accounts this quarter.”***


Rovo reads across the connected records and returns a synthesized view, so teams can quickly see what needs attention.


## From understanding to action


Finding and understanding the customer picture is the foundation. The next step is doing something with it, and this is where Rovo agents go further: with your approval and your own Salesforce access, an agent can take the action for you instead of leaving you with the data entry.


Take the busywork after every customer call. Reps normally rewatch or summarize the conversation, then retype the outcomes into Salesforce. It is the work that keeps the CRM current, and the first thing to slip on a busy day.


With Rovo, you can build an agent that closes the loop. After a call recorded in Loom, the agent reads the recording, pulls out what changed (next steps, stage, close date, and a short recap), and updates the Salesforce opportunity on your behalf. You review the proposed changes, confirm, and move on.


> *“Review this meeting recording and update Salesforce accordingly.*[Meeting Link](http://loom.com/) *”*


The agent reads the Loom transcript, drafts the update, shows you exactly what it will change, and writes it back to Salesforce once you approve, respecting your permissions. Loom holds the conversation, Salesforce holds the deal, and Rovo connects them, so the call becomes the update and reps stay focused on the customer instead of the keyboard. That is “type less, sell more.”


## Governed by Design. Controlled by You.


Teamwork Graph is designed with enterprise trust at its core. Customer privacy and data governance are enforced throughout the integration, giving admins control over what flows into from Salesforce. Admins choose which supported objects (accounts, opportunities, contacts, cases, and campaigns) and which standard and custom fields are ingested. Access to ingested data respects existing Salesforce permissions, ensuring users only see information they are authorized to view. Teamwork Graph syncs with Salesforce to update and delete objects that are updated and deleted in Salesforce.


Beyond indexing, Rovo can augment Teamwork Graph context by calling Salesforce’s MCP directly, always operating within the user’s permissions to show only what the user is authorized to view in Salesforce.


## Get started


Connect Salesforce to Teamwork Graph from[Atlassian Administration](https://admin.atlassian.com/) , then try one high-value workflow:


1. [Connect Salesforce](https://support.atlassian.com/organization-administration/docs/connect-salesforce-to-teamwork-graph/) and confirm the objects and fields you want indexed.
2. Ask Rovo to prepare you for an upcoming account conversation.
3. Review the result, and share it with your team.


Learn how to connect Salesforce to Teamwork Graph:[https://support.atlassian.com/organization-administration/docs/connect-salesforce-to-teamwork-graph/](https://support.atlassian.com/organization-administration/docs/connect-salesforce-to-teamwork-graph/)


## Where this is headed


The Salesforce connector is one part of a broader shift: Atlassian’s Teamwork Graph is the context layer for teamwork across first-party and third-party tools. Jira tracks work. Confluence captures knowledge. Jira Service Management manages support and service. Salesforce captures customer and revenue context. Teamwork Graph connects these signals, and Rovo gives teams a way to find and reason over them together.


This post focuses on finding and understanding Salesforce data in Rovo as well as a glimpse of our agentic capabilities. In a follow-up, we’ll cover how Rovo goes further; using automations and agents to take action in Salesforce and help keep your CRM up to date and allow teams to **“Type Less and Sell More.”**
