---
schema_version: "1.0.0"
document_id: "5036445a2098b39ca7767d95426807e77f830c25fc7b58989ebb5e2b0c4bd1cf"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/how-insurers-can-launch-agentic-quote-and-buy-journeys-with-guidewire-cloud-apis"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:14:43.061733+00:00"
content_hash: "sha256:8f18ae2ded96e78f54c250feb7e17494cdec2469d033a62b1f480ad8a2ead0d3"
---

# How Insurers Can Launch Agentic Quote and Buy Journeys with Guidewire Cloud APIs

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- How Insurers Can Launch Agentic Quote and Buy Journeys with Guidewire Cloud APIs


The quote and bind process is a crucial, high-intent stage within P&C insurance distribution. In 2026, customers expect a seamless experience, demanding a bindable quote directly within their existing digital channel. Whether using a web form, mobile application, partner platform, or AI assistant, they want to avoid switching channels or waiting for a follow-up call. This blog explains how an agentic AI Quote & Buy architecture, built on Guidewire Cloud APIs, can meet that demand by turning natural language intent into safe, auditable insurance transactions:


- Agentic AI Presents a New Multi-Channel Opportunity
- What Makes Quote & Buy Truly Intelligent
- How to Use Guidewire Cloud APIs to Enable Agentic Quoting
- How to Implement the Agentic Quote & Buy Blueprint
- Increase Your Competitive Advantage with GPTs as Your Newest Distribution Channel


*For detailed, step-by-step instructions on configuring Guidewire architecture with ChatGPT, jump straight to thetechnical integration guide at the end of this blogpost.*


---


## I. Agentic AI Presents a New Multi-Channel Opportunity


The Quote & Buy journey today is no longer about directing users to a single destination, it’s about being present wherever the customer chooses to start their journey. As we move through 2026, we see a fundamental shift in users increasingly bypassing traditional search engines like Google in favor of conversational interfaces like ChatGPT to find solutions. Early movers like[Insurify](https://www.prnewswire.com/news-releases/insurify-launches-industry-first-chatgpt-insurance-comparison-app-302682073.html) and[Steadily](https://fintech.global/2026/03/17/steadily-launches-chatgpt-app-for-instant-landlord-quotes/) are already experimenting with GPT-native quote and buy journeys *(see image below)* , earning prominent placement in these AI channels while many carriers are still in the early stages of exploring what this could look like for them.


*ChatGPT Apps page showing insurance quoting apps*


---


## II. What Makes Quote & Buy Truly Intelligent


Agentic AI Quote & Buy represents a fundamental shift in how insurance is sold. In an agentic model, the difference between a basic chatbot and an AI agent is the difference between being told how to get a quote, versus having the quote and binder created in the background while the customer continues the chat conversation in plain language.


Early GPT attempts that used Generative AI to offer chat-supported Quote & Buy experiences were inconsistent and error-prone, because they were subject to a wide variety of publicly available sources of information: an outdated webpage, or a third-party insurance market with old rates. That’s where agentic AI offers a distinct advantage of drawing from your source of truth, ensuring the Quote & Buy experience is always based on the latest information in your system of record. There are three defining characteristics that make the paradigm shift necessary:


- **High-Fidelity State Management** : Unlike traditional bots, agentic AI maintains true state. If the customer needs to check a detail and pauses the chat, the AI agent holds the context of the conversation and picks up exactly where they left off.
- **Contextual Intent Mapping** : Agentic AI does not rely on rigid, pre-set menus. When a customer says, *“I need coverage for my new vehicle or home”* the AI agent interprets the intent, identifies the product, and gathers the necessary details through natural language conversation.
- **Native API Action** : The AI agent acts as a scoped service proxy, executing necessary back-office calls itself rather than instructing the customer on the quoting process. This involves handling authentication and making structured data calls through the integration layer directly into the core systems.


The agentic AI approach enables the GPT to become a first-class channel using the same trusted underwriting engine you already run today. The rest of this blog post is about how to do that in a way that is both agile and durable. By moving quickly with agentic AI, insurers can release a working Quote & Buy GPT before the distribution channel becomes saturated.


---


## III. How to Use Guidewire Cloud APIs to Enable Agentic Quoting


Building an agentic AI Quote & Buy experience on Guidewire works best with layered architecture where each tier has a clear responsibility, and no layer makes assumptions about the one above or below it.


Our agentic Quote & Buy blueprint separates concerns into three distinct layers:


- Layer 1 (LLM Integration): Connects platforms like ChatGPT, Claude, or MCP clients
- Layer 2 (Orchestration Engine): Turns unstructured messages and runs the actual quote flow via PolicyCenter REST Adapter
- Layer 3 (Guidewire PolicyCenter): Single source of truth for Product, Rules, and Rating


#### Layer 1: LLM Integration (Meet the Customer Where They Are)


The topmost layer exposes the same quoting capability through multiple AI platforms:


- [MCP Server (Model Context Protocol)](https://modelcontextprotocol.io/docs/getting-started/intro) : An open standard that allows any MCP-compatible AI client to discover and invoke insurance tools. The server registers tools like request_quote


and get_available_fields


, each with a schema that the AI reads to understand what it can do. Applications like Claude Desktop, Cursor, or any MCP client can instantly gain the ability to quote insurance with no custom extensions.
- [ChatGPT Plugin](https://openai.com/index/chatgpt-plugins/) : Exposes the same capability through OpenAI's plugin protocol, with an OpenAPI spec and a plugin manifest. ChatGPT calls the backend's REST API, and the orchestration layer handles the rest.
- [Claude Tool Interface](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview) : A direct tool-calling integration for Anthropic's Claude, mapping tool definitions to the same orchestration pipeline.


The critical design decision is that all three interfaces converge into a single StandardizedRequest


so that downstream layers can act on normalized requests.


#### Layer 2: Orchestration Engine (The Brain of the System)


This is where conversational intelligence lives. The orchestration engine turns an unstructured user message into a structured insurance transaction through several coordinated subsystems:


- **Intent Recognition** : This system identifies whether the user is providing information, requesting a quote, asking for help, or confirming a decision.
- **LLM-Based Field Extraction** : Instead of asking the customer to fill fields on a form, the system sends the customer’s natural-language message to an LLM along with the full field schema. The LLM produces structured JSON with every field it can identify.
- **Dynamic Schema Service** : The system leverages a combined schema service, which is automatically generated by integrating live field definitions from PolicyCenter Cloud APIs directly (including types, valid options, and display titles) with business rules specified in a configuration file (covering defaults, exclusions, and coverage logic). This service drives the LLM prompts, validation and responses.
- **Conversational Response Generation** : A parser for raw system output producing a natural reply using an LLM. When PolicyCenter reports, *“The field YearBuilt is required for quote,”* the customer sees *“Thanks for the info! What year was your home built?”* The system never exposes internal field names or error codes.
- **PolicyCenter REST Adapter** : The transaction engine embedded within the orchestration layer. When the state machine transitions to GENERATING_QUOTE


, the adapter executes a precise sequence of PolicyCenter Cloud API calls. This 12-step flow mirrors the exact operations a human underwriter would perform in PolicyCenter:


Step Entity Type REST URI Pattern Details


1 Account, AccountContact, AccountLocation /account/v1/accounts/${accountRefId}


+ nested contacts/locations Creates all three in a single save using BFF temp URI refs


2 Job /job/v1/jobs/${jobRefId}


product: HomeownersProduct


triggers auto-creation of HomeownersLine


3 HLLDwelling {jobUri}/lines/HomeownersLine/dwellings/${dwellingRefId}


Must be explicitly POSTed (not auto-created)


4 HLLDwelling {dwellingUri}


Retrieve for checksum, then PATCH with user data + defaults


5 HLLCovADwelling_Coverage {dwellingUri}/coverages/HLLCovADwelling


Create empty, retrieve checksum, set terms ( HLLCovADwellingLimit


, cause of loss, coinsurance)


6 HLLCovBOtherStructures_Coverage {dwellingUri}/coverages/HLLCovBOtherStructures


Create only; terms set after sync in step 8


7 HLLDwelling {dwellingUri}


Action: sync-coverages


, generates CovC, CovD, deductibles, and 40+ sub-coverages


8 HLLCovBOtherStructures_Coverage,
HLLCovCPersonalProp_Coverage,
HLLCovDLossOfUse_Coverage,
HLLSectionIDeductibles_Coverage {dwellingUri}


Set limit terms using ratios from config (CovB=10%, CovC=50%, CovD=20% of CovA)


9 HomeownersLine {jobUri}/lines/HomeownersLine


Action: sync-coverages, creates CovE (Liability), CovF (Medical), Special Limitations, etc.


10 HomeownersLine {jobUri}/lines/HomeownersLine


Sets continuousInsurance


, insuranceRefused


, and other line-level boolean defaults


11 Job {jobUri}


Action: sync-modifiers


12 Job {jobUri}


Action: quote


(triggers rating), then retrieve totalPremium


, quoteNumber


, status


- **Zero Hardcoded Field Names** : The adapter reads the dynamic schema to determine which fields exist in the current PolicyCenter environment.
- **Graceful Error Capture** : If any step were to fail, the adapter will capture the detailed error from PolicyCenter and pass it upstream. The orchestration layer then uses the LLM to ask the customer for the specific missing information in natural language.


#### Layer 3: Guidewire PolicyCenter (The Source of Truth)


PolicyCenter remains the authoritative system and your single source of truth. It owns the product model, the underwriting rules, the rating tables, and the validation logic. The adapter calls PolicyCenter Cloud APIs directly, handling authentication and request composition. The framework calls generic endpoints ( /entities/save


, /entities/retrieve


, /actions


) exposed by the PolicyCenter Cloud API layer. Schema discovery allows the agentic layer to adapt automatically when product models change, without redeploying the agent.


---


## IV. How to Implement the Agentic Quote & Buy Blueprint


In the technical walkthrough that follows, we’ll use a Homeowners product ([USA Homeowners Product Model](https://marketplace.guidewire.com/product/usa-homeowners-product-model/01t3n00000GfcsIAAR?tab=Media) ) as a reference example, but the same architecture applies to Personal Auto and other lines of business:


**Step 1: Select a Focused Product and Journey**
Pick one Line of Business from the[Guidewire Marketplace](https://marketplace.guidewire.com/) and install it on[Advanced Product Designer](https://www.guidewire.com/developers/developer-tools-and-guides/advanced-product-designer) (APD). A single LOB gives you a bounded product model and a finite set of fields. Ensure you have a PolicyCenter Cloud environment with the product deployed and Cloud API access enabled, and a Guidewire Functions instance configured to proxy requests to that environment. Start with Quote to exercise the full entity lifecycle *(account → submission → dwelling → coverages → rating)* without the compliance complexity of bind and issue.


**Step 2: Expose Quote and Bind as APIs from PolicyCenter**
Your agentic system talks directly to PolicyCenter Cloud APIs. Verify that you can manually execute a full quote flow using tools like Postman or curl: create an account, create a submission, add a dwelling, configure coverages, and call the quote action. The sequence of calls you document here becomes the blueprint for your adapter.


**Step 3: Implement the Adapter for the Transaction Flow**
Implement an adapter that translates InsuranceQuoteRequest


into a precise sequence of PC Cloud API calls required by PolicyCenter's entity model. Follow the guidelines mentioned in Section III, Layer 2 to avoid hard-coding fields and to surface PolicyCenter validation cleanly.


**Step 4: Configure the Orchestration Engine**
Set up the orchestration engine described in Section III above: connect it to your LLM provider, plug in schema discovery from PolicyCenter Cloud APIs, and define a product-specific business-rules file that drives defaults, exclusions, and conversational validation.


**Step 5: Connect Your First Front-End Channel**
Implement a thin controller (often ~100 lines of glue code) for your first AI channel. It could be an MCP client or a ChatGPT plugin that maps the platform’s request/response format onto the engine’s standardized contract.


**Step 6: Instrument Observability and Evaluation**
Log every quote attempt, validation error, and LLM interaction with structured metadata (session, step, fields, errors, timings), and build a simple dashboard so product and underwriting teams can see how real sessions behave.


**Step 7: Pilot with Controlled Traffic**
Deploy the system in a secure hosted environment accessible to AI platforms, connect it to a non-production PolicyCenter tenant, and start with internal testers before expanding to limited real customers. Use their feedback to tune prompts, defaults, and error-handling.


**Step 8: Scale to Additional Channels and Products**
Once the first journey is stable, you can add new insurance products (e.g., Personal Auto, Commercial Property) by implementing additional adapters and business-rules files, while reusing the same orchestration foundation. The InsuranceAdapter


interface defines the quote contract, with each product implementing its specific entity flow and step sequence. The blueprint easily extends beyond quotes to bind and issue using the same /actions


endpoint.


*ChatGPT integration with PolicyCenter to facilitate automated insurance quoting*


---


## V. Increase Your Competitive Advantage with GPTs as Your Newest Distribution Channel


The paradigm shift toward Agentic AI is well underway. Customers are learning to ask an AI assistant for help first, and only pick up the phone or open a web form when the digital experience lets them down. In that moment of need, they expect a real quote, not just a link.


By leveraging our agentic Quote & Buy blueprint, you will open a new AI-driven quote-and-bind channel that extends your existing distribution footprint. You can put an agentic experience wherever your customers already are and let it route cleanly into the systems you trust today. You’ll meet customers on their terms, in their moment of highest intent. You deliver the coverage they need at the exact moment of decision, and earn their loyalty before they ever leave the digital experience.


***The question is no longer whether customers will buy insurance through AI. They already are. The real question is whether, in that critical moment of choice, is it your quote that they see, or your competitors.***


---


*For detailed, step-by-step instructions on configuring our architecture with ChatGPT, follow the technical integration guide below.*


### A. How to Integrate with ChatGPT Actions


[GPT Actions](https://developers.openai.com/api/docs/actions/introduction) allow you to extend[Custom GPT](https://openai.com/index/introducing-gpts/) with capabilities by pointing it at an OpenAPI specification. The agentic insurance backend exposes a single, purpose-built action endpoint that handles the entire quote conversation: state management, field extraction, PolicyCenter interaction, and response generation all happen server-side.


**Step 1: Open the Actions Configuration**
In ChatGPT, navigate to *Explore GPTs → Create → Configure → Add Action* . This opens the action editor, where you will import the backend's OpenAPI specification.


**Step 2: Import the OpenAPI Specification**
In the action editor, click Import from URL and enter the specification endpoint:
https://\[your-host\]/openapi.yaml


ChatGPT will parse the spec and register the getInsuranceQuote


operation. The spec describes a single POST /api/chatgpt/quote


endpoint that accepts a natural language message


and an optional sessionId


.


**Step 3: Configure Authentication**
Set authentication to ‘None.’ The backend handles all downstream authentication with PolicyCenter Cloud internally, the ChatGPT caller does not need credentials.


**Step 4: Set the Privacy Policy URL**
ChatGPT requires a privacy policy URL for any action that communicates with an external service. Set it to: https://\[your-host\]/privacy


**Step 5: Save and Test**
Save the GPT configuration. ChatGPT will now call getInsuranceQuote


whenever a user asks about a home insurance quote. The first call sends only the user's initial message and the backend returns a sessionId


in the response. On every subsequent call, ChatGPT must include this sessionId


so the conversation resumes from the correct state.


##### How the Conversation Flows


Once the action is configured, the pattern is:


1. User message → ChatGPT calls POST /api/chatgpt/quote


with { message, sessionId }


2. Backend extracts fields, submits to PolicyCenter, and returns { sessionId, message, state, needsMoreInfo, collectedData, quote }


3. ChatGPT displays the message


to the user and stores the sessionId


for subsequent calls
4. If needsMoreInfo


is true


, ChatGPT asks the user for the missing fields naturally
5. When state


reaches QUOTE_COMPLETED


, the response includes a structured quote object with premium, coverage details, and a breakdown


The backend implements the PC-driven validation strategy described in Section III: it submits to PolicyCenter immediately and surfaces exactly which fields are required, rather than front-loading the user with a long questionnaire. ChatGPT never sees a raw PolicyCenter error, the backend's orchestration layer transforms every validation response into a natural follow-up question.


##### What the GPT Receives When a Quote Completes


When PolicyCenter successfully rates the submission, the response includes a quote object with:


- quoteId


: the PolicyCenter submission identifier
- premium


: annual premium amount
- coverage


: Coverage A dwelling limit
- deductible


: selected deductible
- breakdown


: per-coverage premium contributions
- coverageDetails


: full list of active coverages and their terms


### B. How to Integrate the MCP Server with ChatGPT Apps


ChatGPT now supports native MCP connections through its Apps panel, allowing users to connect any remote MCP server directly to a ChatGPT conversation without building a plugin or writing an OpenAPI spec. In contrast to the ChatGPT Action, which uses a single conversational endpoint for backend exposure, connecting via ChatGPT Apps grants ChatGPT access to discrete backend tools. These tools: r equest_quote


, get_quote_status


, and get_available_fields


, are automatically discovered and independently invoked by ChatGPT as needed, based on the flow of the conversation.


**Step 1: Connect the MCP Server in ChatGPT Apps**
In ChatGPT, open *Settings → Connected Apps → Add MCP Server* and enter the backend's Streamable HTTP endpoint: https://\[your-host\]/mcp


ChatGPT will connect to the server ( insurance-quote-mcp


, version 1.0.0) and automatically discover the three registered tools.


**Step 2: Understand the Available Tools**
The MCP server registers three tools:


- request_quote


: Accepts a message


containing all property details the user has provided, plus an optional sessionId


to continue an existing conversation. This tool should be called as soon as the user provides basic information (name, address, coverage amount). The system submits to PolicyCenter immediately and iterates based on what comes back.
- get_quote_status


: A read-only tool that returns the current session state and all fields collected so far. Use this only when the user explicitly asks what the system has recorded.
- get_available_fields


: Returns the full field schema for the homeowners product: field names, types, required status, and valid options for enumerated fields (construction types, roof types, foundation types, and others). Useful when the user provides a value you need to map to a specific code.


**Step 3: Manage Sessions Across Tool Calls**
Every response from request_quote


includes a sessionId


. ChatGPT must pass this sessionId


on every subsequent call. Since every backend submission is a new call, the message for each call must encompass all property details gathered up to that point, rather than just the latest information entered.


A typical conversation looks like:


1. User provides name, address, and coverage amount → ChatGPT calls request_quote


2. Backend returns sessionId


, a natural response, and any fields PolicyCenter identified as missing
3. User answers the follow-up → ChatGPT calls request_quote


again with all previous details plus the new answer, passing the sessionId


4. This submit-learn-ask-resubmit loop continues until PolicyCenter rates the submission
5. On success, state


is QUOTE_COMPLETED


, and the quote


object contains the full results


**Step 4: Verify the Connection**
Once configured, ask your MCP client: " *I need a homeowners insurance quote.* " The client should invoke request_quote


automatically. If you want to confirm which fields are available before starting, call get_available_fields


to return the fields discovered in the connected PolicyCenter environment.


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
