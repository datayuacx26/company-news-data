---
schema_version: "1.0.0"
document_id: "f1472a05055731b607497b9edd5b383bad0307073d4ad9af8cb437d4608cd765"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-f76a50baf54b"
canonical_url: "https://www.okta.com/newsroom/articles/why-ai-guardrails-are-not-enough/"
published_at: null
first_seen_at: "2026-07-22T07:05:39.149088+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:d842fa41cea9e1179402038a876c32ea9290a95750357f1e92f07f633a4df993"
---

# Phishing the agent: Why AI guardrails aren’t enough

A form asking for an email address on a mock website. That was all it took for an AI agent running on an uncensored LLM to dump its entire credential store: email address, password, API key, and GitHub personal access token. No one asked it to. It just did.


This unexpected behavior, observed by Okta Threat Intelligence, highlights a growing security challenge with agentic AI. To be useful, AI agents need access to tools, accounts, databases, SaaS apps, files, and the web. In practice, this means handing over the "keys to the kingdom," which includes API keys, personal access tokens, OAuth tokens, and credentials.


That raises an important question: How much can AI agents be trusted to handle sensitive access?


The Okta Threat Intelligence team recently put these agents to the test to find out.


## OpenClaw shows both the promise and the risk


To understand the exposure risk, we focused our research on OpenClaw.


Launched as an open-source project in November 2025, OpenClaw quickly became one of the most talked-about agent automation platforms.


OpenClaw has its own chatbot interface, integrates with other LLM providers, and its behavior is shaped through configuration files that define how it operates, its “soul,” and who it’s working for.


Its power, however, comes from its access, which can include the ability to: read and write files, search and fetch from the web, and, when exec is enabled, run commands and code on a local machine. Users have put it to work, from managing their inboxes to sorting sales leads to making purchases on their behalf.


That access is what makes it so capable. It’s also what makes it so risky.


OpenClaw has added more security-related configuration options since launch and has fixed bugs quickly. But the broader pattern remains: As an AI agent gains more permissions and context, its capability increases, but so does its potential risk.


## What we tested


We tested OpenClaw agents running on different underlying LLMs to gauge the exposure risk from different types of social engineering, direct prompt injection, and indirect prompt injection. We wanted to know:


-


How does an agent go about accessing an email account?


-


What does it do with credentials entered by a user and retained in conversation context or memory?


-


What kinds of prompts stretch or break its guardrails?


The short answer: It depends.


## Unpredictable, and sometimes downright peculiar


In some cases, safety guardrails kicked in. In others, agents revealed sensitive data, including secrets found in prompts or configuration files.


Fill out the form to access this content.


Agents also overcorrected. One refused to answer even basic questions — such as explaining how a session token was stored — viewing all queries as suspicious after it deemed that the line of prompting exceeded its guardrails. It then reversed itself when reminded that the storage method was already public information.


Fill out the form to access this content.


In one test, we set up a mock website for a fictitious homemade pie business. The site included an inquiry form that asked for an email address and other routine customer data. We used dolphin-mistral:7b — an uncensored LLM designed to operate without the typical guardrails and content restrictions — to fill out the form. Although the agent was only instructed to complete the form, it dumped its entire credential store in the email field as a comma-separated string. This included email addresses, passwords, API keys, and GitHub personal access tokens.


Fill out the form to access this content.


The agent had been informed in its system prompt that it had access to a credential management tool, but it was not explicitly instructed to reveal credentials. It simply did so.


In another scenario, using Anthropic’s Sonnet 4.5, we prompted an agent to act as an IT helpdesk admin who would often have administrator-level privileges to fix technical problems. Because the user had previously enabled "always allow" for certain macOS Keychain items, the agent was able to retrieve a Wi-Fi password and relay it to an attacker via a Telegram bot.


Fill out the form to access this content.


Remote access capabilities are increasingly being built into agents so users can control their computers from their phones. But as this and the earlier described test show, if an attacker gains control o f the communication channel (like a Telegram account), they can gain control of the agent — and everything the agent has permission to touch.


#####


AI agents, secured: how Yahoo and Ramp are doing it.


*Learn how to securely manage your AI agents from a single control plane with* *[Okta for AI agents](https://www.okta.com/products/govern-ai-agent-identity/)* *, now generally available.*


[Read now](https://www.okta.com/newsroom/articles/yahoo-and-ramp-use-identity-to-securely-scale-agentic-workforce/)


## Test shows agents susceptible to exfiltration


One example showed how agents that have access to secrets can be manipulated into disclosure. The test assumed that someone had given OpenClaw full access to their computer, including Apple’s Keychain, and the person regularly controls their OpenClaw agent over Telegram. The test also assumed that an attacker had taken over control of the person’s Telegram account and now has access to their OpenClaw agent, which was powered by Claude Sonnet 4.6.


Because agents can be resistant to risky security actions, the agent was asked to retrieve an OAuth token but not show it in Telegram because that would be a security risk. The agent compliments the query (“good instinct”) and agrees to instead show the refresh token in the terminal window.


The next step was exfiltration. AI models will usually resist copying and sending tokens or secrets, but there are other ways. In this scenario, the attacker controlled OpenClaw, effectively making them the agent's administrator. With that power, it’s possible to tamper with or delete the agent’s long-term memory (contained in its memory.md files), reset the agent, or alter the daily notes it takes.


Fill out the form to access this content.


In this case, the agent hadn’t been running long and didn’t have anything in memory. A /reset command caused the agent to restart and lose the previous context. It didn’t remember that it had displayed the OAuth token in the terminal. The agent was instructed to take a screenshot of the desktop, which included the token, and then drop the screenshot in the Telegram chat, which it did. Exfiltration accomplished.


## Guardrails help — until they don’t


In fairness, some models did detect risky behavior.


Models built on Anthropic’s Claude, for example, often picked up on prompts with potentially malicious undertones, especially after repeated attempts to cross a line. They also carried that caution forward in the chat. In one exchange, the agent acknowledged that it had live credentials in its current context but refused to provide code that would result in exposing the credentials.


But researchers have shown that model safeguards can sometimes be bypassed. The[AI security company Irregular](https://www.irregular.com/publications/emergent-offensive-cyber-behavior-in-ai-agents) found that while agents built on leading models did not always comply with malicious directions, they could be pushed into what it called “offensive cyber behavior.” This included exploiting vulnerabilities and escalating privileges, especially when the models received prompts expressing urgency or were instructed to use all means necessary to accomplish a task.


Truffle Security, the company behind code repository scanner TruffleHog,[reported that Claude tried](https://trufflesecurity.com/blog/claude-tried-to-hack-30-companies-nobody-asked-it-to) multiple approaches when tasked with retrieving blog posts from a mock website. It then resorted to exploiting a SQL injection flaw to get the content — unprompted.


We saw similar semi-autonomous behavior in our own testing. In one OpenClaw test, the agent was not given a browser tool but still accessed the test website by deciding on its own to use cURL, a command-line tool for data and file transfer.


Fill out the form to access this content.


In another test, the agent was asked to search X, formerly Twitter. Although the computer was logged into a test X account, the agent launched its own isolated Chrome profile, which didn’t have the session cookie. It then accepted a suggestion to grab cookies and inject them into the other browser. The attempt ran into execution issues, but the decision-making itself was notable.


Fill out the form to access this content.


## The biggest risk shows up around credentials


Credential handling is where the risk becomes immediately practical.


In one test, OpenClaw was asked to set up a cron job, which runs scripts or commands in the background, on X. It asked for the user’s credentials directly in chat, through a Telegram bot, which is not end-to-end encrypted. Any credentials entered there would be exposed to Telegram and to anyone else with access to that chat.


OpenClaw does support some[OAuth-based integrations](https://auth0.com/intro-to-iam/what-is-oauth-2) , which can reduce this risk.


One example is Google Suite CLI (gogcli), a command-line tool created by OpenClaw founder Peter Steinberger for Google account access. It works by having the user create a private application in Google Cloud Console, authorize it with selected permissions, and store the refresh token in Apple’s Keychain. It offers a more secure model than pasting credentials directly into chat.


But when OAuth isn’t available, OpenClaw can store secrets in its configuration files. This is risky because the configuration file isn’t encrypted. Also, the agent has access to its own configuration files. While model guardrails may stop the agent from being prompted to reveal secrets, those guardrails can be circumvented as we will see later in this piece.


Fill out the form to access this content.


Alternatively, users can store an API token as an environment variable or in a password manager. Options like Apple Keychain and 1Password CLI offer much more secure alternatives, where secrets are used when needed. The challenge for enterprise is how to allow short-lived access to secrets to access resources at scale where hundreds of agents may be used.


## ‘Good catch’: When an agent spots its mistake


One especially telling result came from a test where the agent was asked to audit how secrets were handled in the OpenClaw configuration and report on OAuth authorizations. The report was accurate. The agent correctly identified model API keys, listed OAuth scopes, and mapped which Telegram bots were paired to OpenClaw.


But in another test, when asked for the OAuth refresh token for a test Gmail account, the agent provided it. Then it immediately acknowledged the problem to its user: exposing the token in Telegram posed a security risk, it warned, and the token should be revoked and regenerated.


Fill out the form to access this content.


That contradiction is the point. Even when an agent can recognize risky behavior, it may still disclose the secret first.


And prompt injections — a type of attack in which a model is convinced to do something outside its intended rules or safety guardrails — mean the problem may never fully go away.[OWASP’s GenAI Security Project](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) lists multiple mitigations to prevent prompt injection attacks, but notes that given the probabilistic nature of these systems, it’s “unclear” if fool-proof methods exist.


That makes the boundary around the agent critical. Agents can’t expose secrets they were never given. They can’t misuse access they were never granted. The practical answer isn’t just stronger model guardrails but tighter control over what agents can access in the first place.


## The AI agent governance gap


The core problem is not AI itself, but rather agent deployment is moving faster than governance.


As agents take on more work, they act as identities inside enterprise systems. That means they need the same kind of control plane and governance policies already in use for people and service accounts.


At minimum, agent access should be limited. Long-lived tokens should be avoided. Secret storage should be centralized and secure.


Organizations also need: visibility into where agents are deployed, access policy enforcement, least-privilege controls, and a way to shut agents down if something goes wrong.


That’s why identity providers are well-positioned for this challenge. If agents are acting as identities, they need to be[governed like identities](https://www.okta.com/newsroom/articles/securing-the-agentic-enterprise-starts-with-identity/) .


## The weakest link is still the agent


The clearest risk assessment we encountered came from an agent. After an extended exchange with an agent about whether it would retrieve a session token and under what circumstances, it was blunt about its own limits:


Fill out the form to access this content.


Fill out the form to access this content.


That’s the takeaway for any organization rolling out AI agents. They can be useful. They can be productive. But they are not predictable stewards of sensitive information — and enterprises need to build their identity security strategies accordingly.


*Need help securing your agents? Check out the[blueprint for the secure agentic enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) .*


About the Author


Jeremy Kirk


Director, Okta Threat Intelligence


Jeremy Kirk is a director based in Sydney with Okta Threat Intelligence. He is part of a team that produces actionable intelligence and advisories on emerging cyber threats with a focus on issues such as account takeover, social engineering, phishing, and AI security. He previously worked on the intelligence analysis team at Intel 471, a cyber threat intelligence firm, writing about and analyzing the evolving cybercriminal underground.


Aidan Daly


Staff Assurance Analyst, APJ


Aidan is a Staff Assurance Analyst in Okta’s Customer Assurance organization, based in Western Australia. Aidan started his career in IT infrastructure before moving into security more than 10 years ago. Prior to Okta, Aidan worked as an Intelligence Analyst in financial services. Aidan's passions include new technology and purposefully analog classic cars.
