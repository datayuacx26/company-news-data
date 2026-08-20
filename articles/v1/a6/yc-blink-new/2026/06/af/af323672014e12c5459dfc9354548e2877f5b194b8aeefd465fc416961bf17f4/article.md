---
schema_version: "1.0.0"
document_id: "af323672014e12c5459dfc9354548e2877f5b194b8aeefd465fc416961bf17f4"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/vibe-coding-glossary"
published_at: "2026-06-05T12:57:31+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:36e41c74fa698ca023c5e50dfa52cffd3c0bf3d1a350b0ef9a2dc5cc6b7dcf75"
---

# Vibe Coding Glossary: Every Term You Need to Know in 2026

## Infrastructure Terms


**Database** Where your app stores information — user accounts, bookings, messages, products. Every real app needs a database. Some AI app builders (Blink) include a database automatically. Others (Bolt.new) don't include a persistent database, which limits what you can build.


**Authentication / Auth** The system that handles user sign-up, login, and session management. Without auth, anyone can access everything in your app. Blink includes auth automatically. Other platforms require you to set up separate auth services (Clerk, Firebase Auth) that cost $25+/month each.


**Backend** The server-side logic that processes requests, runs business rules, and communicates with the database. You don't see it as a user — it runs behind the scenes. Frontend-only builders don't include a real backend.


**Frontend** The UI a user sees and interacts with — buttons, forms, pages, animations. Frontend-only builders generate this well but stop there.


**API (Application Programming Interface)** A way for two software systems to communicate. When your app pulls weather data or charges a credit card, it calls an external API. AI app builders often have built-in integrations for common APIs (Stripe, Twilio, etc.).


**Hosting** Where your app runs on the internet. Includes servers, domains, and global delivery. Blink handles hosting automatically. If you build on Lovable or Bolt, you need a separate hosting service (Vercel, Netlify, Railway).


**Zero-Config Deployment** Publishing your app without configuring servers, environment variables, or deployment pipelines manually. You click "Publish" and it goes live. Blink's model — contrast with platforms that require Vercel setup, custom domains, and environment variable configuration.


**CDN (Content Delivery Network)** A globally distributed network that serves your app's files from servers close to each user. Makes apps load faster worldwide. Most hosting platforms include CDN; Blink includes it automatically.


---


## Development Concepts


**Version Control / Git** A system that tracks changes to code over time. Like a history log for your app. Git is the dominant system. Blink automatically saves versions — you can roll back to any previous state if a prompt breaks something.


**Deployment** The process of making your app live on the internet. Ranges from "click Publish" (Blink) to 45-minute infrastructure configuration (self-managed). Zero-config deployment is the vibe coder's goal.


**Environment Variables** Configuration values that apps need but shouldn't store in code (API keys, database passwords). Usually set in a` .env` file or a platform's settings panel. One of the first pain points for beginners on platforms that require manual setup.


**Rollback** Reverting to a previous version of your app after a change breaks something. Essential when a prompt generates code that breaks a working feature. Blink versions automatically — rollback is 2 clicks.


**Tech Stack** The combination of tools, frameworks, and services an app uses. Example: "React frontend, Node.js backend, Postgres database, hosted on Railway." AI app builders abstract the tech stack — you don't need to choose one.


**TypeScript** A version of JavaScript with type safety. Reduces runtime errors and makes large codebases more maintainable. Most modern AI code editors and AI app builders generate TypeScript by default.


---


## AI and Model Terms


**LLM (Large Language Model)** The AI models that power coding tools: Claude, GPT-4o, Gemini. They generate text (including code) based on training on vast amounts of data. Different models have different strengths — Claude is particularly strong at complex coding tasks.


**Model Router** A system that routes requests to different AI models based on task type and cost. Blink Claw includes a 200+ model router — your agent picks the best model for each task without you configuring it.


**Context Engineering** Designing what information an AI agent has access to at any moment. Advanced vibe coding and agentic coding skill. Includes writing good CLAUDE.md or AGENTS.md files that give AI agents the context they need to do quality work.


**Agentic Coding** AI-assisted development where the AI takes multi-step autonomous actions — not just completing a single prompt but running tests, reading files, making changes, and verifying results. Claude Code and Cursor in Agent mode are examples.


**MCP (Model Context Protocol)** A standard for connecting AI agents to external tools (databases, APIs, file systems). Allows an AI agent to use tools from any provider without custom integration. Blink's plugin adds 62 MCP tools to Claude Code and Cursor.


---


## Vibe Coding Community Terms


**The Wall** The point in building where prompting stops fixing the problem. Common at hour 4-8 of a first project. Usually resolved by: starting a fresh conversation, narrowing the scope, or rolling back to the last working state.


**Prompt Fatigue** What happens when you've had too many back-and-forth exchanges trying to fix one problem. Solution: start fresh, narrow scope, or take a break. Trying harder in the same failing direction rarely works.


**AI Slop** AI-generated content or code that looks complete but is low quality — correct syntax, wrong logic. Common in rushed vibe coding sessions. Prevented by: testing core flows constantly, being specific in prompts, and reviewing outputs before shipping.


**Spec-First** Writing a product specification before prompting an AI tool. Builders who spec first report 60% fewer "rebuild from scratch" moments. The spec becomes the first prompt and the north star for every follow-up.


**One-Feature Rule** Building one feature at a time and testing before adding the next. Prevents the "everything is broken and I don't know why" problem.


**Full-Stack Founder** A non-technical founder who uses AI app builders to ship full-stack products without a developer. Distinct from "indie hacker" in that they focus on business outcomes, not learning to code.


---


## Business Terms


**MVP (Minimum Viable Product)** The simplest version of an app that lets you test whether the idea works with real users. Vibe coding is particularly strong for MVPs — you can test ideas in hours instead of weeks.


**MRR (Monthly Recurring Revenue)** Monthly revenue from subscriptions. The benchmark for early-stage SaaS. Many vibe coders track their first $1K MRR as the "real product" milestone — proof that strangers will pay for what you built.


**SaaS (Software as a Service)** Software sold as a monthly/annual subscription accessed over the internet. Vibe coding is well-suited for building SaaS apps because standard patterns (auth, payments, user management) are well understood by AI models.


**Churn** The rate at which customers cancel subscriptions. Ignored at the MVP stage; critical after product-market fit. High churn usually means a core feature is missing or the user experience is too painful.


**Bootstrapped** Built without outside investment. Vibe coding lowers the cost of bootstrapping — you don't need a development budget to test ideas. Many vibe-coded products are bootstrapped to profitability.


Build this with Blink — database, auth, and hosting included. No config needed →[blink.new](https://blink.new/)


Traditional no-code (Bubble, Webflow, Glide) is visual drag-and-drop — you configure logic through a UI. Vibe coding is AI-generation — you describe what you want and the AI builds it. Vibe coding is faster for new builds; traditional no-code gives more granular control for complex configurations.


The basics matter: understand what "database," "authentication," and "deployment" mean so you can describe your requirements clearly. The rest is useful context — you don't need to understand how a CDN works to use one.


No. Low-code uses visual configuration with some code for complex logic. Vibe coding generates code through AI from text descriptions. You're not configuring anything visually — you're describing behavior in English and the AI writes the code.
