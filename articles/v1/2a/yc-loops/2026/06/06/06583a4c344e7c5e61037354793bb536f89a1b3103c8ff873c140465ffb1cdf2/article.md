---
schema_version: "1.0.0"
document_id: "06583a4c344e7c5e61037354793bb536f89a1b3103c8ff873c140465ffb1cdf2"
company_key: "yc-loops"
company: "Loops"
source_id: "yc-loops-news-import-d3d77458967f"
canonical_url: "https://loops.so/changelog/content-api-goals"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-25T13:13:35.252230+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:19733a7359a46daa676d5313140b742a56d7ae295aa9ecdd62b02695a076e4a4"
---

# Content API and Goals

### **Overview**


We are launching the Content API for Loops: new APIs for campaigns, transactional email, and workflows, plus LMX for writing email content in code.


The goal is simple: the email you create through the API should be the same email your team can open, review, edit, and send in Loops. Code, agents, and the visual editor now work on the same canvas instead of separate templates that drift apart.


Campaign API, Transactional API, LMX, and Goals are available to all users.


The public Workflow API can list workflows and read workflow nodes. Creating, editing, and publishing workflows from code is not currently available.


### **Campaign API**


The Campaign API lets you create and update Loops campaigns from code. It is built for the work that often happens before a campaign reaches the editor: gathering product changes, turning internal notes into customer-facing copy, drafting a subject line, and preparing the body for review.


You can connect it to your own tools and agents, whether that means GitHub pull requests, Linear issues, Notion docs, or an internal release process. The result is a real Loops draft, not a separate artifact that needs to be pasted into the editor later.


Once the draft is in Loops, your team can keep working visually: edit the header, adjust the layout, check links, and send for review. The API and editor stay pointed at the same campaign, so you can move between automation and manual polish without rebuilding the email.


[Read the docs](https://loops.so/docs/api-reference/examples/campaigns)


**Use cases:** Changelog emails, product launches, monthly updates, release notes, and agent-assisted campaign drafts.


### **Transactional API**


The Transactional API lets you manage product emails in Loops without treating every copy change like a deploy. Password resets, magic links, invites, billing notices, receipts, and alerts can live as real Loops emails while still being controlled from code.


Instead of maintaining a custom HTML template or rendering package, you send the event and variables Loops needs for that recipient. Loops handles the template, fills in the dynamic values, and sends the message.


That gives engineering teams a code-friendly workflow and gives non-technical teammates a visual place to review or adjust the email when needed. One email, two ways to work on it.


[Read the docs](https://loops.so/docs/api-reference/examples/transactional-emails)


**Use cases:** Password resets, magic links, receipts, invites, billing updates, account alerts, and usage notifications.


### **Workflow API**


The public Workflow API lets tools list workflows, inspect a simplified workflow, and read workflow node details. Events can already start an existing workflow.


Creating, editing, and publishing workflows from code is not currently available.


See the[current API reference](https://loops.so/agents/api) for supported endpoints.


[View the API reference](https://loops.so/agents/api)


**Use cases:** Workflow inspection, triggered product notifications, lifecycle reporting, and journey QA.


### **Goals**


Goals helps answer the question every campaign eventually gets judged by: did it work?


Instead of stopping at opens and clicks, Goals lets you define the outcome you care about. Choose the eligible audience, the conversion event, and the measurement window, then see whether the email moved that number.


That outcome data also gives agents and automated workflows a better signal. If the goal is activation, upgrade, retention, or demo booking, future email work can optimize toward the actual result instead of the first click.


Goals is available in Loops.[Read the launch update](https://loops.so/changelog/goals-are-live) .


[Read the docs](https://loops.so/docs/goals/overview)


**Use cases:** Activation, upgrades, demo bookings, purchases, retention, churn reduction, and any product event that means the campaign succeeded.


### **LMX**


LMX is the markup language behind this launch. It gives your code and agents a readable way to describe Loops email content directly.


LMX is XML-based and maps to the editor one-to-one. A button in LMX is a button in the editor. A column is a column. Content written in code can be opened visually, and content edited visually can come back as clean LMX.


There is no npm package to install and no compile step to maintain. Loops still handles the email-client work underneath, including Outlook quirks, dark mode behavior, Gmail clipping, and CSS inlining.


[Read the docs](https://loops.so/docs/creating-emails/lmx)


**Use cases:** Server-generated emails, reusable templates, API-created campaign drafts, transactional messages, and product-driven content.


### **Wrapping up**


Campaign API, Transactional API, LMX, and Goals are available now. The public Workflow API currently provides read-only access to workflows and workflow nodes.


Together, these updates make Loops a shared canvas for email work across the editor, code, and agents. Create in code, review in Loops, send through the same system, and measure the outcome that matters.
