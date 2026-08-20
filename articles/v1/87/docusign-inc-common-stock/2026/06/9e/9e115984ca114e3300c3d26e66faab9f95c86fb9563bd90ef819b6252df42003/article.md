---
schema_version: "1.0.0"
document_id: "9e115984ca114e3300c3d26e66faab9f95c86fb9563bd90ef819b6252df42003"
company_key: "docusign-inc-common-stock"
company: "DocuSign Inc."
source_id: "docusign-inc-common-stock-news-import-aebf23be7403"
canonical_url: "https://www.docusign.com/blog/developers/agentic-raffle-workflow-claude-docusign-mcp-server"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-25T01:48:46.315991+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:8d4720b341fae052b7e458eccf7fe4ceb73ce4e9181f8d530c19d5f0a8a7751d"
---

# How we turned a Momentum NYC raffle into an agentic workflow with Claude and Docusign MCP

At[Docusign Momentum NYC](https://www.docusign.com/blog/developers/momentum-26-agentic-agreement-workflows) , we wanted to show attendees what an agentic agreement workflow could look like. So we built one around something simple: a raffle for a Nintendo Switch 2.


Attendees walked up, chatted with Claude, and entered for a chance to win the prize.


Behind the scenes, Claude triggered a[Workflow Builder](https://support.docusign.com/s/document-item?bundleId=yff1696971835267&topicId=dnx1696972415150.html) workflow, sent each attendee an eSignature entry form, retrieved completed entries from[Agreement Manager](https://support.docusign.com/s/document-item?bundleId=pqz1702943441912&topicId=adf1702945446135.html) , selected the winners, and kicked off the prize notification workflow.


Most multi-step business processes follow a similar pattern: collect information, store it, make a decision, and notify the right people. Traditionally, building that process means creating a custom application, wiring up[OAuth flows](https://developers.docusign.com/platform/auth/) , integrating multiple APIs, and writing server-side logic.


With the[Docusign MCP Server](https://developers.docusign.com/platform/mcp-server/) , you can orchestrate that same kind of flow through a conversation with an AI agent.


In this post, we’ll walk through how the raffle worked, how Claude used the Docusign MCP Server to call Docusign APIs, and how the same pattern can apply to other agreement workflows.


## Key takeaways


-


The Docusign MCP Server lets AI agents like Claude call Docusign APIs through natural language.


-


The core pattern: trigger a workflow, retrieve agreement data, process that data, and act on the result.


-


Workflow Builder handles repeatable workflow steps, while Agreement Manager stores and returns structured agreement data.


## What this example demonstrates


In this walkthrough, you’ll learn:


-


How the Docusign MCP Server enables AI agents to call Docusign APIs


-


How an AI agent can trigger Docusign workflows


-


How an AI agent can retrieve and process agreement data from Docusign Agreement Manager


-


Design considerations for building reliable agentic flows at scale


## The pattern: Agent-orchestrated agreement workflows


The raffle experience was intentionally simple for attendees. They only had to start a conversation and provide their information.


The architecture behind it did more work. Claude interpreted the request, the Docusign MCP Server gave Claude a secure way to call Docusign APIs, Workflow Builder handled the repeatable workflow steps, eSignature collected the official entry, and Agreement Manager made the completed entry data available for winner selection.


Here’s how the pieces fit together:


Component


Role


Docusign MCP Server


Provides the interface for an AI agent to securely call Docusign APIs, enabling users to trigger workflows and query agreement data using natural language


eSignature


Populates agreements with recipient data, sends agreements, and enables recipients to securely access and sign agreements


Workflow Builder


Orchestrates repeatable workflow steps, such as sending eSignature agreements or notification emails


Agreement Manager


Stores completed agreements and returns structured data, such as recipient details and agreement attributes


AI agent


Interprets user requests, calls Docusign APIs through the MCP Server, and executes business logic


This pattern works for any scenario in which you want an AI agent to:


1.


Trigger a Docusign workflow in response to a user request


2.


Retrieve data from completed agreements


3.


Process that data and make a decision


4.


Act on the decision by triggering a follow-up workflow


The AI agent handles the dynamic orchestration, and Docusign handles the agreement workflow, data, permissions, and execution.


## The use case: An agentic raffle


At Momentum NYC, attendees entered the raffle by chatting with Claude. The agent handled the process through the Docusign platform: sending entry forms, validating entries, selecting winners, and notifying them.


Here's the workflow and the components used in each step:


### Step 1. Raffle entry with MCP Server, Workflow Builder, and eSignature


A Momentum attendee opens a Claude chat and asks to enter the raffle.


Claude collects their name and email address, then passes that information through the MCP Server in a[Workflow Builder API request](https://developers.docusign.com/docs/workflow-builder-api/reference/workflowbuilder/workflows/triggerworkflow/) that triggers the raffle entry workflow.


Workflow Builder then sends the participant a Docusign eSignature agreement that includes the raffle terms and conditions and the entry form. After the participant signs it, the completed agreement becomes their official entry.


### Step 2. Data extraction and storage with Agreement Manager


Once signed, the completed agreement is automatically processed in Agreement Manager.


Agreement Manager extracts the relevant fields from the entry, including the entrant name, email address, and entry date.


This structured agreement data makes it possible for the agent to retrieve and process entries later without manually reviewing each completed agreement.


### Step 3. Data retrieval and processing with MCP Server and Agreement Manager


When it's time for the drawing, a Docusign employee prompts Claude to select three winners.


Claude sends an[Agreement Manager API request](https://developers.docusign.com/docs/agreement-manager-api/reference/agreementmanager/agreements/getagreementslist/) through the MCP Server to retrieve all completed raffle entries. The agent receives the structured entry data, validates the eligible entries, and runs a random selection algorithm to choose the winners.


### Step 4. Winner notification with MCP Server, Workflow Builder, and eSignature


For each winner, Claude sends a Workflow Builder API request through the MCP Server to trigger a prize notification workflow.


The workflow sends a congratulatory email to each winner that includes an eSignature agreement in which they provide their shipping address for the prize.


## How the Docusign MCP Server ties it together


The[Docusign MCP Server](https://developers.docusign.com/platform/mcp-server/) is what makes this pattern possible without writing application code. It exposes Docusign API methods as tools that Claude can call in response to natural language prompts.


When a user types` Enter me in the raffle` , Claude uses the MCP Server to:


-


Call the Workflow Builder API to trigger the entry workflow


-


Pass the participant's name and email as input to the workflow


When an administrator types` Select the raffle winners` , Claude uses the MCP Server to:


-


Call the Agreement Manager API to retrieve all completed raffle entries. Claude then processes the entry data and selects winners.


-


Call the Workflow Builder API to trigger the notification workflow


-


Pass the winner's name, email address, and the prize as inputs to the workflow


The agent doesn't need pre-built routes or a backend. The MCP Server provides authenticated, scoped access to Docusign APIs, and Claude handles the logic of interpreting the request and deciding which tool to call next.


## Design considerations for production-ready agentic flows


The raffle was a fun event experience, but building it surfaced practical lessons for designing reliable agentic workflows.


### Manage context window size


Retrieve agreement data as efficiently as possible. In the initial implementation, Claude pulled detailed recipient data from each raffle entry individually, and every call's JSON piled up in Claude's conversation context. At scale, this caused:


-


**Context window overflow:** The context filled up, and some runs failed to complete.


-


**Unreliable behavior:** As the context grew larger, the agent sometimes deviated from the intended selection logic.


The fix was to design the agreement and extraction process so that Agreement Manager could extract the data and return it to the agent with minimal API calls.


When building agentic agreement workflows, structure your agreements so the AI agent can retrieve the data it needs efficiently. Where appropriate,[implement custom extraction logic](https://support.docusign.com/s/document-item?bundleId=pqz1702943441912&topicId=yrx1747083411531.html) in Agreement Manager so the required data is available in Agreement Manager API responses.


### Use structured prompts for reliable multi-step workflows


When an agentic workflow involves multiple sequential steps (for example, retrieve data → filter → select → trigger notification), use structured prompt formatting to guide the agent through each phase. XML-style sections or clear markdown headings can help reduce ambiguity and keep the agent aligned with the intended process.


Structured prompts are especially important when the agent is handling consequential actions.


### Separate capabilities with clear security boundaries


Keep consequential actions behind access controls. The raffle split entry and winner selection into distinct skills with different access levels:


-


The entry skill triggered the entry workflow but could not access the winner selection logic.


-


The winner selection skill had strict guardrails to exclude invalid raffle entries and prevent tampering with the winner selection process.


For production workflows, consider separating capabilities into distinct skills or tool scopes so that a single conversation can't escalate beyond its intended permissions.


### Choose the right level of automation


Not everything needs to be agentic. In the raffle workflow, Workflow Builder handled the repeatable, structured steps, like prepopulating user information and triggering notifications. Claude handled steps that benefited from flexibility, such as interpreting user requests and running selection logic.


A good rule of thumb: use Workflow Builder for repeatable, templated steps. Use an AI agent for steps that require interpretation, data processing, or dynamic decision-making.


## What you need to build a similar workflow


To implement a similar agentic flow, you'll need:


-


[Docusign MCP Server](https://developers.docusign.com/platform/mcp-server/) : Connects an AI agent to your Docusign account and exposes Docusign API capabilities and tools


-


An AI agent: Interprets requests and calls Docusign APIs through the MCP Server. See the[MCP Server documentation](https://developers.docusign.com/platform/mcp-server/#prerequisites) for examples of AI agents that you can use.


-


[Agreement Manager](https://support.docusign.com/s/document-item?bundleId=pqz1702943441912&topicId=adf1702945446135.html) : Extracts and stores data from completed agreements


-


[Workflow Builder](https://support.docusign.com/s/document-item?bundleId=yff1696971835267&topicId=dnx1696972415150.html) : Orchestrates repeatable steps


-


[eSignature templates](https://support.docusign.com/s/document-item?bundleId=xry1643227563338&topicId=dqj1578456412286.html) : Collect the required information using forms and agreement fields designed for downstream data retrieval


The Momentum raffle processed entries, selected winners, and sent notifications entirely through conversational interactions with Claude. The same pattern can apply to any multi-step agreement workflow where you want AI-driven orchestration without building a custom application.


The reusable pattern is simple: Trigger → Retrieve → Process → Act


For a raffle, that meant triggering an entry workflow, retrieving completed entries, selecting winners, and sending prize notifications.


In another workflow, it could mean collecting onboarding documents, retrieving agreement data, checking for missing fields, and triggering the next approval step. Or it could mean finding completed agreements, extracting key terms, and routing follow-up actions to the right team.


While the use case can change, the pattern stays the same: give the agent a secure way to call Docusign APIs, use Docusign to manage the agreement workflow and data, and let the agent handle the dynamic orchestration.


## Resources


-


[MCP Server setup with Claude](https://developers.docusign.com/platform/mcp-server/anthropic-claude/)


-


[Agreement Manager UI documentation](https://support.docusign.com/s/document-item?bundleId=pqz1702943441912&topicId=adf1702945446135.html)


-


[Agreement Manager API documentation](https://developers.docusign.com/docs/agreement-manager-api/)


-


[Workflow Builder UI documentation](https://support.docusign.com/s/document-item?bundleId=yff1696971835267&topicId=dnx1696972415150.html)


-


[Workflow Builder API documentation](https://developers.docusign.com/docs/workflow-builder-api/)
