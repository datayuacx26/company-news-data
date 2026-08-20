---
schema_version: "1.0.0"
document_id: "40729a1f7f91cdef5e6b4e5fe08070c4c34bf1b7c69e86b3ce2fe9b1e4768632"
company_key: "yc-finta"
company: "Finta"
source_id: "yc-finta-rss-ddc51759d5a1"
canonical_url: "https://www.trustfinta.com/blog/finta-aurora-mcp"
published_at: "2026-04-27T21:35:48+00:00"
first_seen_at: "2026-07-25T04:38:50.826355+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:329a92232cae885793499e4cd252f7b1527e814a0a18ca29ab21a474c2833ae8"
---

# Introducing Finta’s Aurora MCP

Today we are launching Finta MCP for Aurora.


Instead of just generating text, AI apps like ChatGPT, Claude, Grok, Gemini, Cursor, and others can now interact directly with real tools to take actions on your behalf.


Finta’s Aurora MCP allows you to connect any AI app directly to your Finta Deal & CRM data. Enabling powerful workflow optimization through natural language commands:


- “Deep research 50 new leds and add them to my CRM”
- “List the contacts in my pipeline.”
- “Who should I follow up with today?”
- “Add Aileen Lee to my CRM as a target investor.”
- “Update Fred Wilson’s stage to ‘Emailed.’”


And the assistant actually performs the task inside your Finta account.


# **What is Model Context Protocol (MCP)?**


To understand Aurora, it helps to first understand Model Context Protocol (MCP). MCP,[open-sourced by Anthopic](https://www.anthropic.com/news/model-context-protocol) , is a new standard for connecting AI assistants to the systems where data lives


MCP allows AI models to securely connect to tools, data sources, and services. Instead of manually wiring custom integrations for every app, MCP provides a universal way for AI assistants to interact with software systems.


Think of MCP like a USB-C port for AI tools.


Just as USB-C lets you plug many devices into one port, MCP integrations let AI assistants connect to many applications through a shared protocol.


This means your AI can:


- Access external tools
- Execute real actions
- Pull structured data
- Automate workflows


Without MCP, AI assistants are mostly limited to text generation and analysis.


With MCP, they become AI agents capable of interacting with your software stack.


That’s where Aurora MCP comes in.


# **What Is Finta’s Aurora MCP?**


[Aurora](https://www.trustfinta.com/products/aurora-ai) is Finta’s AI deal maker, designed to help you manage relationships and fundraising workflows.


By exposing Aurora through MCP integrations, Finta allows your AI apps to:


- Access your CRM data
- Understand your deal
- Reason across your pipeline
- Add more leads to your CRM
- Assist with workflows


Instead of switching between tools, you can control your CRM with natural language.


# **Getting Started With Aurora MCP**


Setting up Aurora MCP only takes a few minutes.


Aurora works with Cursor, ChatGPT, Claude, and other MCP-compatible AI apps.


## Step 1: Add the Aurora MCP Server


Each AI application has its own settings interface for MCP servers. Navigate to the MCP page in settings.


First, add the Aurora MCP server inside your AI client.


Use the MCP URL: https://app.trustfinta.com/mcp


## Step 2: Connect Your Finta Account


After adding the MCP server:


1. Open the MCP or integrations panel in your AI app.
2. Click **Connect** next to Aurora.
3. Sign in with your Finta account.
4. Confirm the connection.


Once authenticated, the AI app can securely access your Finta data.


You only need to complete this process once per AI app. Sometimes you need to restart the app for it to recognize a new MCP has been added.


## Step 3: Start Using Aurora


Once connected, simply start chatting with your AI assistant!


Try prompts like:


- “Who is in my CRM”
- “Update an investor’s status to a new stage.”
- “Who should I follow up with today?”


Aurora will automatically run the appropriate tools to give you the data you need. Helping you to:


- move faster
- automate repetitive work
- optimize your workflows


As AI agents and MCP integrations continue to grow, this kind of natural-language control over software will become the new normal. Luckily for you, Aurora is already making it possible today.


Are you ready to level up with the next wave of AI workflow automation?[Get started with Finta today.](https://app.trustfinta.com/register?blog_MCPblog)


‍


# Connecting Aurora in Cursor


1. Open **Settings**
2. Navigate to **Features → MCP**
3. Edit your MCP configuration file
4. Add the Aurora server


Example configuration:


{


"mcpServers": {


"aurora": {


"type": "streamableHttp",


"url": "https://app.trustfinta.com/mcp"


}


}


}


Click Authenticate to connect your Finta account. Save the configuration and restart Cursor if needed.


‍
