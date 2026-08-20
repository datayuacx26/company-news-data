---
schema_version: "1.0.0"
document_id: "391ee14effd5094115cad5e8ee3fcbb072c6a8fca60a872a9eeebc349e95bce9"
company_key: "yc-vapi"
company: "Vapi"
source_id: "yc-vapi-news-import-8dd0a49247bf"
canonical_url: "https://vapi.ai/blog/introducing-vapi-cli"
published_at: "2025-07-08T00:00:00+00:00"
first_seen_at: "2026-07-24T05:51:02.715878+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:dbd5f314e08a2c931a7ce19ae31d25b27f67429065973a1e62cc51b57b701947"
---

# Introducing Vapi CLI: The Best Developer Experience for Building Voice AI Agents - Vapi AI Blog

**TL;DR:** We've built the Vapi CLI to bring world-class developer experience to your terminal and IDE. Drop Vapi into any project with one command, debug webhooks locally without ngrok, and turn your IDE into a Vapi expert with MCP integration.


---


Building voice AI agents shouldn't require constantly switching between your terminal, browser, and IDE. You shouldn't need to set up` ngrok` tunnels just to test webhooks. And your IDE definitely shouldn't hallucinate Vapi APIs that don't exist.


That's why we built the **Vapi CLI** - a command-line interface designed from the ground up to give developers the best possible experience when building voice AI applications.


## Everything You Do in the Dashboard, Now in Your Terminal


The Vapi CLI gives you complete control over your voice AI infrastructure without leaving your terminal:


```text
# List and manage your voice assistants
vapi assistant list
vapi assistant create


# Handle phone numbers and calls
vapi phone list
vapi call create


# Configure webhooks and tools
vapi webhook create https://myapp.com/webhook
vapi tool create


# Debug with real-time logs
vapi logs calls
vapi logs webhooks


```


Every feature in the Vapi dashboard is now available as a simple command. No more context switching between terminal and browser when you're in the flow.


## Drop Vapi Into Any Project


The magic happens with` vapi init` . Point it at any codebase and it automatically:


- **Detects your tech stack** - React, Vue, Next.js, Python, Go, Flutter, React Native, and dozens more
- **Installs the appropriate Vapi SDK** for your framework
- **Generates working code examples** tailored to your project structure
- **Sets up environment configuration** templates


```text
cd my-existing-app
vapi init


# Detected: Next.js application
# ✓ Installed @vapi-ai/web SDK
# ✓ Generated /components/VapiButton.tsx
# ✓ Created /pages/api/vapi/webhook.ts
# ✓ Added environment template (.env.example)


```


No more copy-pasting from docs or figuring out integration patterns. You get production-ready code that follows best practices for your specific setup.


## Turn Your IDE Into a Vapi Expert


Here's something that will change how you develop with Vapi: **MCP (Model Context Protocol) integration** .


Run` vapi mcp setup` and your IDE's AI assistant (Cursor, Windsurf, VSCode with Copilot) gains complete, accurate knowledge of Vapi's APIs, features, and best practices.


**Before MCP:**


- AI suggests non-existent Vapi methods
- You waste time debugging hallucinated code
- Documentation lookups break your flow


**After MCP:**


- AI knows exactly how Vapi works
- Code suggestions are accurate and current
- Your development velocity increases dramatically


Ask your IDE: *"How do I create a voice assistant that transfers calls to a human?"* and get real, working Vapi code instead of guesswork.


## Debug Webhooks Locally (No More ngrok)


Testing webhooks during development has always been painful. Not anymore.


```text
vapi listen --forward-to localhost:3000/webhook


```


That's it. The CLI starts a local server that receives all your Vapi webhook events and forwards them to your development server in real-time.


**What you get:**


- Instant webhook testing without external tunnels
- Real-time event logging for debugging
- Automatic request forwarding to any local endpoint
- No more ngrok setup or configuration


## Built for Production


The CLI isn't just a development tool - it's designed for your entire workflow:


```text
# Multi-account support for teams
vapi auth switch production
vapi auth switch staging


# Comprehensive logging and monitoring
vapi logs errors
vapi logs calls --assistant-id abc123


# Campaign management at scale
vapi campaign create
vapi campaign list


```


Whether you're prototyping your first voice agent or managing production campaigns with thousands of calls, the CLI scales with your needs.


## Get Started in 10 Seconds


```text
curl -sSL https://vapi.ai/install.sh | bash
vapi login
vapi init


```


That's it. You're ready to build voice AI agents with the best developer experience possible.


## Try it now


The Vapi CLI is just the beginning. We're building toward a future where voice AI development is as smooth and intuitive as any other part of your stack.


Stop context switching. Start shipping voice AI agents faster than ever before.


*The Vapi CLI is available now. Install it, try it, and let us know what you think. We're just getting started.*
