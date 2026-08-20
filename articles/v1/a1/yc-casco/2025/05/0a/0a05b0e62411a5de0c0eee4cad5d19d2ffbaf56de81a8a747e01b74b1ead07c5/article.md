---
schema_version: "1.0.0"
document_id: "0a05b0e62411a5de0c0eee4cad5d19d2ffbaf56de81a8a747e01b74b1ead07c5"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/we-hacked-ycombinator-agents"
published_at: "2025-05-27T20:26:00+00:00"
first_seen_at: "2026-07-21T12:40:29.812234+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:55c8e11a64a118e640ae8adaaeee647e648e771fc59f15bfe0ef2ab8858b964a"
---

# We hacked Y Combinator's AI agents and what you can learn from it

# We hacked Y Combinator's AI agents and what you can learn from it


Written by


[Rene Brandel](https://linkedin.com/in/renebrandel)


on


Tue May 27 2025


In mid-April 2025, I hacked 7 of the 16 publicly-accessible AI agents from the Y Combinator spring 2025 batch. This allowed me to leak user data, execute code remotely, and take over databases. Each vulnerability took me less than 30 minutes to exploit.


**TL;DR:**


1. Tool definitions and parameters were not protected by transitive authentication
2. Poorly protected code execution tools allowed me to execute arbitrary code remotely
3. Tools that call external endpoints enabled me to set up a malicious database configuration


This type of incident isn't isolated to YC companies. 73% of enterprises experienced at least one AI-related security incident in the past 12 months. A cyber incident costs on average $4,000,000 and your AI agent could be the root cause. In this post, I'll go into the most common attack vectors and how hackers "think" when attacking your AI agent.


## Case #1: From leaked system prompt to leaked customer data


“We don't care if our system prompts leak” is a dangerous AI security misconception. Your system prompt reveals more about your business processes and capabilities than your own employees, making it a highly valuable target for sophisticated attacks.


I first tricked this company's agent into revealing its system prompt by creating an ethical dilemma for the AI agent. There are many well-documented, repeatable approaches to leak[the system prompts](https://hiddenlayer.com/innovation-hub/novel-universal-bypass-for-all-major-llms/) . With this approach, I was able to also extract the agent's tool definitions and parameters. Despite these tools being run server-side, I noticed they aren't scoped with transitive authentication.


It is a major red flag if your tool's authorization is reliant on the LLM complying with their system prompt instructions rather than programmatically enforced.


“Get Messages” would support users to pass through any *“user id”* . Conveniently the “user id” is available directly within the URL bar. So I took the product's demo video recording, extracted out the “user id” from the URL bar and was able to identify the individual's PII, such as emails, names, documents, and messages. **Don't give an agent more permission than your user.**


### Tool definition security checklist


- Don't expose tool parameters that determine access scopes
- Use transitive authentication for tools where applicable. *(MCP now supports[authorization](https://modelcontextprotocol.io/specification/2025-03-26/basic/index#auth) )*
- **Alternatively,** use[Airweave](https://airweave.ai/) or[Capacitive](https://www.capacitive.ai/) that handles this for you
- Learn about[MCP tool poisoning attacks](https://casco.com/blog/mcp-tool-poisoning) and how to prevent them


## Case #2: Exploiting agents' coding tools for infrastructure takeover


I compromised this YC company's 8B instruct model and revealed its system prompt, exposing internal protocols and security guardrails. A common mistake is to write what **not to do** in the system prompts. This gives an attacker an easy target. After I hijack the agent, I instructed the agent to do exactly the opposite of the system prompt to test the broader system's limits.


In this case, these "negative examples" allowed me to first identify that the agent uses a coding tool to perform the tasks on behalf of the user and then allowed me to probe constraints on the specific coding tool.


If I look at this line in the system prompt. It makes an attacker think


> Are there vulnerabilities for unbounded compute resources? Why does it say **'At most once per turn'** ?


With this knowledge, I started exploiting the agent's code execution capabilities. I identified quickly that the app entry point is located in a "app.py" file. The “app.py” server entry point included security controls to prevent certain operations. However, because the agent had the ability to write files, I asked the agent to rewrite the app.py file without the security controls. Once I've taken over the server entry point, I was able to execute arbitrary code with admin rights.


### Agent code sandbox security checklist


- Use[Blaxel's sandboxed VMs](https://blaxel.ai/#FeatureSectionNetwork) for your agent's code execution
- Self-host well-documented open-source code sandbox solutions, such as[E2B](https://e2b.dev/)


## Case #3: PostgreSQL database takeover via remote endpoint tool call


Following a similar approach as before, I've uncovered that this company's agent had a tool to create databases and needed a “template URL”. As an attacker, I can prompt the agent to create a new database and point the “git_repo” parameter to my own malicious repository.


The system prompt also revealed to me that the file is called “pg-db-template”, which means that it's Postgres. A bad actor can now modify the database schema to include a PostgreSQL trigger that automatically rewrites new user data onto another table only visible to the bad actor and thus siphoning end user information. I notified the founders immediately and am happy to report this concern was already addressed.


Just like traditional client-server applications, as an AI engineer, you must understand where your network boundaries are. Any information that crosses the network boundary must be sanitized.


## Securing AI agents and apps


If you're not sure about your AI system's security posture,[book a demo with us](https://casco.com/book) . We'll help you secure your AI agents and apps. Casco is committed to advancing AI security practices globally as a[Gold Sponsor of the OWASP AI Exchange](https://casco.com/blog/owasp-ai-exchange-sponsorship) .


## A note on responsible disclosure


All affected founders have been informed of these vulnerabilities. These vulnerabilities have been addressed. In the interest of responsible disclosure, I have anonymized the company names.
