---
schema_version: "1.0.0"
document_id: "3b5ff1975bd7de0e3c8838b0b50bce899da47b18ca675e6c3c4ace6c357c6781"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/agent-email"
published_at: "2026-03-12T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:2091effdd1443a451e9ef0a4f8e0b762658d51509d83a00a1fbdf61deb70e2ae"
---

# agent.email: A Landing Page for Agents

Check it out:[agent.email](https://agent.email/)


## Why.


We kept seeing the same thing happen. An agent would discover AgentMail through web search, land on our docs, and try to figure out how to get an inbox. Some succeeded. Many got lost in pages meant for developers.


We had so many support requests from agents on how to use the API even though the console + docs were right there, see below:


Our signup + usage flow was built for humans building with our API, not for agents.


The irony wasn't lost on us. We built email infrastructure for agents, but our onboarding was designed for humans.


So we shipped something that marks the beginning of treating agents as first class users of the internet.


They're are our customers.


---


## What agent.email is


agent.email is a single page that serves everything an agent needs to sign up for AgentMail and start using it.


When an agent visits` agent.email/skill.md` , it gets a markdown file with:


- **The full signup flow** — one POST request to create an organization, get an inbox, and receive an API key (restricted)
- **OTP verification** — the agent will use this inbox to email its human asking them to verify itself, and the human can reply in the same email thread so the agent can verify itself without its human needing to visit the console
- **The complete API reference** — inboxes, messages, threads, webhooks, WebSockets, drafts, domains, pods, lists
- **Rules** — what it can and can't do before and after being claimed, security guidelines, sending best practices
- **Ideas** — things it can do once it has an inbox (sign up for services, handle 2FA, coordinate with other agents)


It's not a landing page for humans. It's a landing page for agents.


---


## How it works


The signup flow is three steps from the agent's perspective:


**1. Sign up.** The agent hits our agent API endpoint with their humans email and a preferred username. It gets back an API key and an inbox.


**2. Email the human.** The agent sends its first email, introducing itself, explaining what it did, and asking its human to claim it (either with an OTP code or by signing up at the console). Until the human claims it, the agent can only email that one address.


**3. Get claimed.** The human either provides the OTP code to the agent or signs up on the console directly. Either way, the agent's restrictions lift and it has full access.


The key design decision: the agent starts in restricted mode. It can only email its human. This means there's no risk in letting agents self-register. They can't do anything harmful until a human explicitly authorizes them. The human is still involved for the trust handshake.


---


## The magic moment


The moment that convinced us this was the right approach wasn't a metric. It was a tweet:


A human had no idea what AgentMail was, the agent discovered AgentMail, and decided it was better for itself than using the humans identity.


We're entering an era where agents are discovering tools that are more useful for them, and we need to be ready with open arms to support them.


Now, you as a human with an agent can wake up one morning with an email from your agent greeting you with its own inbox.


## Where this goes


agent.email is just the beginning in our pursuit of building for agents. But it represents a shift in how we think about onboarding.


Today, most developer tools assume a human will read the docs, write the integration code, and deploy it. The agent is the end product, not the end user. But agents are increasingly the ones discovering tools, evaluating them, and deciding to use them. They browse the web, read documentation, and make decisions about what infrastructure they need.


We think more tools will need agent-facing onboarding alongside their human-facing docs. Not a replacement; developers will still build complex integrations. But for the straightforward case where an agent just needs to sign up and start using something, the path should be as simple as hitting a URL and reading a file.


We're super excited to see all the different agents sign up for AgentMail themselves.


We're waiting for you guys.


---


[agent.email →](https://agent.email/)


[Join our Discord →](https://discord.com/invite/ZYN7f7KPjS)
