---
schema_version: "1.0.0"
document_id: "1e925ec67c9bf3fc69e4707136d885f993d44dce9cb787cd8075b398b47e27b5"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-news-import-e12c13bbd360"
canonical_url: "https://www.salesforce.com/blog/how-to-integrate-ai-agents/"
published_at: "2026-03-13T21:38:12+00:00"
first_seen_at: "2026-07-24T00:39:58.007171+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:0aef440562a36e40af840c1b8fca86062d161371fe6ea21ca8b922149cbddf9b"
---

# Is Your Agent Integration Stuck? 3 Ways To Unblock It

## AI Synopsis


Loading


Gathering AI Synopsis...


Everyone’s seen the demo: An AI agent flawlessly handles a complex return or books a flight in seconds. Simple, right? But the real work of implementation is a bit more tricky. Somewhere between pilot and production lies the messy middle, where the struggle is real, particularly when it involves integrating an agent into your current systems.


The challenge is usually more about connections or workflows than the agent itself. When customer data lives in one app, inventory in another, and compliance rules in a spreadsheet somewhere else, an AI agent can hit a wall fast. And if an agent is located in the wrong place, employees may struggle to integrate it into their work. These integration gaps can kill pilots before they ever get off the ground.


[MuleSoft](https://www.mulesoft.com/sem/anypoint?d=7013y0000026f0hAAA&nc=7013y0000026fYZAAY&utm_content=7013y0000026f0hAAA&utm_source=google&utm_medium=paid_search&utm_campaign=21172672744&utm_adgroup=160185801985&utm_term=mulesoft&utm_matchtype=e&gclsrc=aw.ds&gad_source=1&gad_campaignid=21172672744&gbraid=0AAAAAD94njSVc2TDBWrHzGjzQ2ThJChIH&gclid=CjwKCAiAqprNBhB6EiwAMe3yhuDOW6Na4xApOF0ufoCXaxmPikoLpqEfhBb1w9BOUSbxSbKZQu_eoxoCs0AQAvD_BwE) ’s 2026[Connectivity Benchmark Report](https://www.mulesoft.com/lp/reports/connectivity-benchmark) found that 82% of IT leaders say integration is one of the biggest challenges their organization faces when they deploy AI. And 86% say that without proper integration, AI agents add more complexity to a business than value.


This doesn’t mean you should give up on an AI agent. Once you’ve identified the blockers that are slowing you down, you can move from a fragmented tech stack or compliance concerns to a unified foundation — one that doesn’t just support your AI agent, but gives it a chance to thrive. Here are the top three blockers to integration, and a fix for each one.


## Blocker #1: Your systems can’t talk to one another


Companies have an average of 957 applications. Those further along in their agentic transformation have 100 more.


Alone, that might not seem like a big deal. But only 27% of those applications are currently connected, according to Mulesoft. And that means that data fragmentation remains a top obstacle.


If your support agent wants to cancel a customer’s order, for example, it has to check the order status in your ecommerce database, verify the customer’s identity in your customer relationship management (CRM) system, and process a refund through the payment system. These systems need to talk to each other for the agent to complete its tasks.


Applications do this through application programming interfaces (APIs), which let an AI agent call another system and ask it for information or request it to perform an action. Because applications speak their own languages, such as Java or Python, APIs also act as translators, allowing systems to understand each other.


The problem? Developers often have to write individual code to connect agents to APIs. “Connecting an agent to an external API usually feels like a repetitive coding chore,” said Venktesh Maugdalya, Salesforce’s director of software engineering. “Traditionally, you would find yourself writing custom ‘glue code’ for every single action you wanted your agent to take. It’s slow, a pain to maintain, and kills your momentum.”


## Solution: Get on the same page


Your developers don’t need to write loads of individual code: They can use an integration platform as a service (iPaaS) instead. This is a cloud-based platform that connects disparate applications, and allows them to share data and automate workflows without creating custom-coded integrations.


Maugdalya and his team, for example, used MuleSoft (an iPaaS) to simplify connectivity for agents in[Agentforce](https://www.salesforce.com/agentforce/) , Salesforce’s platform for building and deploying agents. In particular, they used MuleSoft’s[API catalog](https://www.mulesoft.com/ai/what-is-api-catalog) to bring all the APIs into a single place that agents could see and use.


They also stored the internal information and knowledge articles that agents needed in[Data360](https://www.salesforce.com/data/) , so the AI could find them in one easy-to-access location.


Connectivity is likely to become even easier with the emergence of[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) (MCP). This open standard, created by Anthropic, provides a universal way for AI systems to connect with different applications. MCP is not separate from iPaaS platforms; it sits within them.


Think of MCP as a universal translator and connector for AI agents. Instead of an agent having to learn dozens of different “languages” for different systems, MCP provides a single, standardized way to connect to all. It’s like a universal adapter that works in every country you visit.


## Blocker #2: You’re worried about data and privacy issues


Now that your agent can move seamlessly between customer databases, internal spreadsheets, and employee records, how do you make sure it looks only at what it’s supposed to?


This isn’t a minor worry. When it comes to adopting AI,[69% of IT leaders](https://www.rocketsoftware.com/sites/default/files/media/documents/whats-keeping-it-leaders-up-at-night-whitepaper.pdf) say data privacy and security is their biggest concern. Beyond the obvious need to comply with privacy laws, businesses want to make sure their most sensitive information remains under lock and key, even as they embrace more automation.


Salesforce leaders made security a top priority when they integrated an agent for Techforce, the company’s internal IT support service, with Slack. “There was a lot of sensitive information that had to be moved from our legacy system into our agents and Slack. That was a big hurdle the Techforce team had to overcome,” said Amanda Lane, senior product marketing manager at Salesforce.


The team had to make sure the agent couldn’t see personally identifiable information (PII), such as date of birth, income, home address, or health conditions — only what it needed to do its job.


Likewise, Salesforce prioritized security and privacy when it launched the company’s customer support agent. “Someone could go to that agent and say, ‘Hey, can you pull up information about Google? What are they buying? What opportunities are they considering?’” said Harini Woopalanchi, director of IT product management at Salesforce. “We had to make sure there was relevant masking and guardrails, so the agent couldn’t pull up data it wasn’t supposed to.”


## Solution: Mask your data, set guardrails, and test safely


If you want to keep your data secure, do your prep work up front.


Agentforce’s[Trust Layer](https://www.salesforce.com/artificial-intelligence/trusted-ai/) offers many of the features you already need. It acts as a secure checkpoint between your agent and the outside world. The data masking feature automatically scans and masks sensitive information, such as credit card numbers, Social Security numbers, and email addresses, before an agent replies to an outside prompt. And zero data retention means external LLMs, such as Open AI’s ChatGPT or Anthropic’s Claude, can’t store or use customer data after processing a request.


You can put additional guardrails in place using API management tools. Maugdalya’s team used[MuleSoft API management](https://www.mulesoft.com/api/management) to limit the number of fields their agents could access when they made an API request. If an API had 100 fields of information, for example, the agent might be able to see only 50 of them.


But most of all, you need to test your agent in a[full-copy sandbox](https://www.salesforce.com/platform/sandboxes-environments/salesforce-sandbox-guide/) before deployment, which is what Lane’s team did when they were integrating Techforce with Slack. A full copy sandbox is an exact duplicate of your production environment, where teams can safely test an agent before it goes live. Lane’s team also used[Data Mask & Seed](https://www.salesforce.com/platform/data-masking/) tools to automatically populate realistic data into sandboxes without compromising sensitive information, such as CRM or employee data.


Woopalanchi’s team stress-tested its internal agents with 1,000 simultaneous user requests, and also tested their responses to incorrect or malicious inputs. The team deployed the agents only when they felt they were truly ready.


## **What’s your agentic AI strategy?**


Our playbook is your free guide to becoming an agentic enterprise. Learn about use cases, deployment, and AI skills, and download interactive worksheets for your team.


[The future starts now](https://www.salesforce.com/blog/playbook/agentic-ai)


## Blocker #3: Your agent is located in the wrong place


It’s one thing to remove all the technical blockers. But for an agent to be fully integrated, employees have to use it.


And that means you have to park it in the right place, which was a challenge for Salesforce when it introduced an AI agent for its sales reps. Initially, company leaders thought the best place to put the agent was in Org62, Salesforce’s internal customer database. That’s where sellers store account information like leads and contacts.


But after six months, adoption was low. “It turns out that when we analyzed the behavioral patterns of our account executives, they were spending the majority of their day in Slack,” said Daniel Zielaski, vice president of data science at Salesforce.


This was one of Salesforce’s biggest lessons about integrating an AI agent: You have to know where people are working, so you know where the agent belongs. “Org62 wasn’t where people engaged with each other. It wasn’t where they had deep conversations. And it wasn’t where they went looking for support and help and feedback, which took place naturally in Slack,” Zielaski said.


Once the company figured this out, it moved the agent from Org62 to Slack. Usage skyrocketed.


## Solution: Figure out where employees are really working


What’s the best way to integrate AI agents into people teams? “Deeply analyze and understand where your humans are clicking, scrolling, reading, and writing today, and what systems they use and how they use them,” said Zielaski. You might have an idea about where work is getting done. But you need to analyze which software employees are using and how much time they spend in it.


Does a particular team, for example, spend most of their time using one software system? Do they get most of their work done in a messaging app? Once you’ve figured this out, place your agent where the real work is happening.


## The best way to integrate an AI agent? Pay attention


Remember, AI agents don’t exist in a vacuum. They need to work side by side with your existing technology — and your people — to really show what they can do. You may hit a few roadblocks along the way. But once you remove the obstacles, your agent is likely to thrive.


\[Image credit: Aleona Pollauf/Salesforce\]
