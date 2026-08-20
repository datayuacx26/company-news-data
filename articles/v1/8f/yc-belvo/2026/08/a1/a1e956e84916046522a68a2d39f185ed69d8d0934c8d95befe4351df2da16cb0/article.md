---
schema_version: "1.0.0"
document_id: "a1e956e84916046522a68a2d39f185ed69d8d0934c8d95befe4351df2da16cb0"
company_key: "yc-belvo"
company: "Belvo"
source_id: "yc-belvo-rss-3d04ef8adabc"
canonical_url: "https://belvo.com/blog/belvo-mcps-the-new-agentic-open-finance/"
published_at: "2026-08-10T12:00:00+00:00"
first_seen_at: "2026-08-10T15:51:58.994120+00:00"
fetched_at: "2026-08-10T15:52:00.117153+00:00"
content_hash: "sha256:c8416bb62be16980cd5cb908070585e47ed3cc7289adb37217eb3fdf3ef8e6ce"
---

# Belvo MCPs: the new agentic open finance

From now on, your AI agents can communicate directly with Belvo to improve your Open Finance integration, build your own client-facing agents or analyze your data and activity. Here is how our three new MCP servers give innovators in Latin America a way to build with financial data.


We started Belvo with a simple conviction: financial access in Latin America should not depend on who you know, which bank you use, or how much history a bureau holds on you. For seven years, that conviction has taken the shape of infrastructure. We built the rails that let banks, lenders, and fintechs connect to their customers' financial lives with consent, and turn that data into better products. Today, those rails sit behind more than 80 million connected accounts, making Belvo the leading open finance platform in Latin America.


That has always taken real engineering time, and not only at the start. Teams spend weeks building the connection, then more time keeping it healthy, holding each user's data current, and making sense of what comes through it. Your developers could still build every step manually. But today, we are making it easier by enabling your agents to handle all of it.


## Why we're doing this


An agent deciding whether to approve a loan, or explaining a statement to a customer, can only be as accurate as the data it's connected to. Across the financial innovators we work with, that's stopped being hypothetical: internal copilots, customer-facing assistants, and agent-based decisioning tools are all moving from pilot to production faster than most roadmaps accounted for.


Most of our customers already have the connection those agents need: verified, consented data through Belvo. What's usually missing is a fast way to put it in front of an agent. Someone still has to build the integration that lets a model call it directly. Until that's built, what the agent sees is something weaker: a number copied in by hand, or an export that's already a few days old. When an agent's answer becomes a decision about someone's money, that's the gap worth closing.


There's already a standard for this. Claude, ChatGPT, and Cursor all speak the Model Context Protocol, or MCP. A server publishes what its tools do and what they need, and the model calls the right one, in the format the server expects, at the moment it needs it – and nobody hand-codes that connection in advance.


So we built on it. Three MCP servers now sit on top of the connection your team already has with Belvo. One is a toolkit for developers, so getting the connection right stops being the hard part. Another lets an agent pull and analyze a specific user's own data, with that user's consent, to power agents that respond to someone's real financial life. The last opens anonymized data analysis at scale, together with the details of your API calls to Belvo, so your team can tune the flows and use the data retrieved in a smarter way to bring new solutions to market


## Develop: a toolkit for builders


Until now, connecting to Open Finance meant developers reading dense documentation and spending weeks writing code. The distance between signing up and seeing real value could be a major hurdle.


Today, that barrier is gone. Developers are no longer the only ones writing the code. Your internal AI tools can now ask direct questions to Belvo:


- "Build the frontend flow for a user connecting their bank account in Brazil."
- "Show me a working example of the fiscal data endpoint in Python."
- "How do I consume enriched income data after connecting an account?"


Today, the toolkit helps AI agents integrate with Belvo using precise, documentation-grounded guidance, and we plan to expand its capabilities over time to include features like usage metrics, error analysis, and deeper API diagnostics.


## Access: live data access


The second area moves consented data from storage to action. Once a user grants explicit consent, Belvo's MCP allows your AI agents to access and analyze their financial data to drive real-time experiences. Every source we connect to is now agent-callable:


- **Banking data (Brazil):** balances, spending, and investments across a user's connected accounts, through Open Finance
- **Employment data (Brazil and Mexico):** employment history, for a clearer read on income and stability
- **Fiscal data (Mexico):** hundreds of thousands of invoice details and tax returns turned into a clear and simple view your agents can access


Transactions, income, spending patterns, and payment history become questions in plain language with verified answers behind them:


- "When does this user receive their salary?"
- "Did this user pay their credit cards late at any bank?"
- "How much did this user spend on transportation over the last year?"


The mechanics matter here, because this is where trust lives. Belvo requires a consented, identity-bound credential before returning anything; data is never handed over on request alone. Once consent is granted, the agent can query only the specific, scoped data it was authorized for, and every call is logged and auditable. If asked, the agent can also update and refresh the user's information.


### Proof, not theory: Panorama


We didn't want to launch a theoretical diagram, so we built a working product first.[Panorama](https://panorama.belvo.com/) is a demonstration built on Belvo's data access: a person connects their own accounts through Belvo's consent flow, then asks an AI assistant about their finances end to end. What Panorama does for one person's accounts, your product can do for your entire user base.


Panorama also shows how Belvo keeps data segregated between users. Each person's data stays scoped to the link they consented to, so nobody else can reach it. For customers building agents on Belvo's MCP, that's the proof point: however many links you hold, each end user's data is read on its own, and never crosses into another user's.


## Analyze: getting the most out of the data


The third area is where we're headed next. Building[on what Bela currently does within the dashboard](https://belvo.com/blog/bela-ai-assistant/) , the data analysis capabilities of our MCP will take portfolio-level insights and API-activity data out of the dashboard and directly into your preferred AI tool. Next up, we will be launching this in our marketplace. Here is a glimpse of what you will be able to ask:


- "Analyze the distribution of credit card limits across my users."
- "What percentage of my portfolio had an income change in the last 90 days?"
- "Which spending categories are driving the biggest month-over-month change?"
- “What are the most common API errors I receive from Belvo and how could I fix them?


The same consent rules apply: teams see their organization's aggregated portfolio data, never an individual user's records outside the consent framework that governs everything else Belvo does.


## The part that doesn't change


Anyone can put an MCP server in front of an API. The protocol gets agents to us; what makes the answers worth trusting is everything underneath it.


AI is probabilistic by nature. Even a well-built agent carries some chance of error, and in credit decisions or payment flows, "probably right" is not a standard. That is exactly why the data an agent reasons over has to be deterministic: verified at the source, consented, and structured by people who understand it. For years our engineers have been building the logic behind categorization, income verification, and risk insights, learning alongside our customers what a salary deposit looks like across hundreds of institutions and what signals actually predict repayment. Our MCP turns that accumulated knowledge into something an agent can call. We know how to build the prompt because we spent years learning what to ask.


This launch is also not our first step in this direction. Bela answers integration questions in our dashboard today. Our custom reports deliver exactly the data a customer needs without stitching together multiple endpoints. AI Collections puts an agent on the phone to recover payments. The MCP is the next stretch of the same road: the same infrastructure, now reachable wherever our customers' AI happens to live.


## Where this goes


We see Belvo's MCP helping three groups:


- **Existing customers** will get more out of the data they already access, asking questions in natural language instead of building analysis pipelines.
- **Financial innovators** who aren't ready for a full integration, or who want to validate an idea before committing engineering time, will finally have a straightforward way to start building with verified financial data.
- **AI companies** building for this region get something they can't assemble on their own: a consented, identity-bound connection to real financial data in Brazil and Mexico, reachable through the same protocol their product already speaks.


All three point to the same future: an open finance that is open not just between institutions, but to every interface where people and businesses now get things done. We spent almost a decade making financial data accessible to developers. Now we're making it accessible to the agents working alongside them.


Are you ready to see what your agents can build on Belvo? Talk to our team.
