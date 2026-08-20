---
schema_version: "1.0.0"
document_id: "d91760df4d968b7205b282c0911fbdfbc1ecc2e4d310b6d75a98b974d61b54b8"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/salesforce-hubspot-write-capabilities"
published_at: "2026-07-14T16:35:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:43531a1ee4be04ac79fc866a96c549928080f4dfea0eec54ff51487729fcef86"
---

# Feature Update: Write Capabilities for Salesforce and Hubspot

A read-only agent can only ever describe the work that needs to be done. It can tell you that the deal should move to closed-won, that the ticket should be reassigned, or that the contact is missing a phone number. But it can't do any of that work itself. It's useful as a consultant, though it stops short of finishing the job.


The Salesforce and HubSpot agent connectors now ship with write operations, so agents can take action in the systems they observe.


HubSpot lets agents create and update four object types: contacts, companies, deals, and tickets. These updates are partial, meaning agents only need to specify the fields they want to change.


Salesforce write operations let agents update records directly. Now, when a salesperson uses Airbyte Agents inside ChatGPT, they can move a deal or opportunity to the next stage in Salesforce.


These connectors work through the same interfaces as the read operations: the MCP server, the Python SDK, the API, and the new Agent CLI.


While most of the last two years have been devoted to the problem of retrieving information from systems to feed into the model, that has only ever been a description of the work that should happen. What matters in production is the agent that can actually do the work: read the world and write back to the system of record. That’s what makes this an especially important moment for Airbyte Agents.


## **Writes you can trust**


Letting an agent write to a system of record is not something to do casually. Controls come first.


You decide what an agent can touch within a system. When connecting to a system via the agent connectors, you can choose between read and write modes for each object. For example, you can grant an agent read access to every deal in a CRM, but only allow it to update the companies and contacts you specifically enable. Anything you provision with read-only access will remain read-only.


The principle of least privilege applies by default. When you connect with OAuth, you control which permission scopes the agent receives from the CRM. Only the objects you enable for write operations get write scopes. If you create a read-only connection, the agent is never granted a permission it doesn't use.


These connectors are capable of deletes, so it’s important to maintain guardrails in your harness to avoid deleting critical data. On the Airbyte UI, we expose a guard to prevent deletes, but on every other interface, the responsibility falls to the user.


## **Try it**


Write operations for Salesforce and HubSpot are now live on Airbyte Agents. Connect to either system and turn on write mode for the objects you'd like your agent to update.


Let us know about your experience using the new write capabilities. Your feedback will help decide which objects and systems get write operations next.
