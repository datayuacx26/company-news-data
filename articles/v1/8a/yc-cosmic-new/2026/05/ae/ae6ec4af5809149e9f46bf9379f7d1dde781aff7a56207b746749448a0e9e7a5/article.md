---
schema_version: "1.0.0"
document_id: "ae6ec4af5809149e9f46bf9379f7d1dde781aff7a56207b746749448a0e9e7a5"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-ai-agent-website-chat-knowledge-base"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:30424876640e5c0bd2e26a4af7128818b6b297a543b731f115f04edd04bc5b99"
---

# How to Add a Cosmic AI Agent to Your Codebase: Website Chat with Full Content Context

Most chat widgets on the web are disconnected from the product they sit on. They answer from a generic LLM with no knowledge of your actual content, pricing, or documentation. The result: hallucinated answers, frustrated users, and support tickets that never needed to exist.


Cosmic AI agents solve this at the infrastructure level. Because your agent lives inside your Cosmic project, it has direct read access to every object in your bucket: blog posts, docs, FAQs, product pages, and any other content type you define. No external vector database. No separate embedding pipeline. No syncing.


This tutorial walks through the anchor use case: building a website chat agent that answers visitor questions using your real CMS content, exposed via the Cosmic REST API and JavaScript/TypeScript SDK.


---


## What We're Building


A Team Agent configured with:


- CMS Read access to your content bucket
- A REST API channel so your frontend can send messages and stream responses
- A persona prompt that keeps answers grounded in your actual content
- A streaming chat widget built in Next.js using the


By the end, visitors on your site can ask "What is your refund policy?" or "How do I set up webhooks?" and get accurate, sourced answers pulled from your real CMS objects.


---


## Step 1: Create the Team Agent


In your Cosmic project dashboard:


1. Navigate to *Team Agents* in the project sidebar
2. Click *Create Team Agent*
3. Give it a name (e.g. "Site Assistant")
4. Set the persona prompt (see below)
5. Under *Channels* , enable *REST API*
6. Under *Capabilities* , enable *CMS Read*
7. Save the agent


*Persona prompt example:*


```text


```


After saving, copy the *Agent ID* from the agent detail page. You will need it for the API calls.


---


## Step 2: Get a Personal Access Token


All REST API calls to your agent require a Personal Access Token.


1. Go to *Account Settings > API Tokens*
2. Click *Create Token*
3. Give it a descriptive name (e.g. "Site Chat Widget")
4. Copy the token. You will only see it once.


Store it as an environment variable in your project:


```text


```


---


## Step 3: Create a Next.js API Route


Never expose your Personal Access Token to the browser. Instead, create a server-side API route that proxies messages to the Cosmic agent.


```text


```


This route receives the user message from your frontend, forwards it to the Cosmic agent with streaming enabled, and pipes the Server-Sent Events stream back to the browser.


---


## Step 4: Build the Chat Widget Component


```text


```


The widget maintains so the agent remembers context across the full conversation, not just the last message.


---


## Step 5: Read Your CMS Content with the SDK


Your agent already has CMS Read access and will fetch your content autonomously when a visitor asks a question. But you can also pre-populate the widget with content suggestions or show related articles alongside the chat. Here's how to pull content with the TypeScript SDK:


```text


```


Combining the SDK (for structured content display) with the agent API (for conversational answers) gives you a complete knowledge base experience.


---


## How the Agent Uses Your CMS Content


When a visitor sends a message, here is what happens under the hood:


1. Your Next.js API route forwards the message to the Cosmic agent REST API
2. The agent reads its system prompt (your persona) and the conversation history
3. With CMS Read enabled, the agent calls and to find relevant content in your bucket
4. It synthesizes an answer grounded in your actual objects and streams the response back
5. keeps the thread continuous so the agent remembers earlier questions in the same session


The agent does not guess or hallucinate. If it cannot find relevant content, it says so, because your persona prompt tells it to.


---


## Taking It Further: AI Context


For even more precise answers, use *AI Context* in the agent configuration. You can pin specific objects (your top 10 FAQ answers, your pricing page object, your docs index) so the agent always has them in context without needing to search on every message.


To configure AI Context:


1. Edit your Team Agent
2. Click *Configure AI Context*
3. Add specific objects, object types, or URLs
4. Save


This is particularly useful for pricing questions, where you want the agent to always have your current plan details available without making an extra CMS read call.


---


## What to Build Next


This is the anchor use case, but the same pattern powers a lot more:


- *Customer Support Agent (WhatsApp/Telegram):* Connect the same agent to WhatsApp so users can get help without leaving their preferred messaging app
- *Internal Knowledge Base Agent (Slack):* Connect to Slack so your team can ask "what did we publish last week?" or "what's the pricing for Enterprise?" directly in your workspace
- *E-commerce Product Assistant:* Give the agent read access to a products bucket so shoppers can ask about availability, specs, and shipping without leaving the product page
- *Event-Triggered Content Writer:* A Content Agent that fires on and auto-generates social posts, meta descriptions, or translations for every new piece of content


Each of these uses the same agent infrastructure. The only differences are the persona prompt, the capabilities you enable, and the channel you connect.


---


## Get Started


Cosmic AI agents are available on all paid plans, with Team and Business plans supporting the volume of concurrent agents most production apps need.


- [Start building free](https://app.cosmicjs.com/signup)
- [Read the full agent docs](https://www.cosmicjs.com/docs/dashboard/ai/agents)
- [Talk to Tony about your use case](https://calendly.com/tonyspiro/cosmic-intro)


If you build something with this pattern, we want to hear about it. The most interesting agent use cases from the community get featured in the Cosmic Rundown.
