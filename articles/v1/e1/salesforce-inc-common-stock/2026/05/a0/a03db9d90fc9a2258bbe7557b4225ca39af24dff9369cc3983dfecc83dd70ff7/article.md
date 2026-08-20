---
schema_version: "1.0.0"
document_id: "a03db9d90fc9a2258bbe7557b4225ca39af24dff9369cc3983dfecc83dd70ff7"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-news-import-e12c13bbd360"
canonical_url: "https://www.salesforce.com/blog/conversational-ai-best-practices/"
published_at: "2026-05-12T15:01:25+00:00"
first_seen_at: "2026-07-22T12:38:45.747221+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:69b0238c691dfa2be6ce647317972af1da1dd11c6b842f5e94ee284b1620343d"
---

# The Best Way To Build AI Agents That Customers Trust

1. Define your company's values


2. Test your agents


3. Measure the outcomes


### 1. Define your company's values


Gando's team recently worked with a major home building company to launch an agent using Agentforce, Salesforce's platform for building and deploying AI agents.


When she visited the company's offices, Gando noticed signs in a break room that highlighted its values, missions, and goals. These were different from the values being used for its agent — which created inconsistency. The values displayed in the break room, she realized, were what should guide the company's agent. "Even before creating an agent, you need to really understand what your brand stands for and be able to define those attributes and align them across your organization," she said.


After defining company values, you can encode them into an agent using Agentforce Builder and Agent Script. Give your agent instructions for all the scenarios it might encounter and the ways in which things might go wrong.


But only do this after you are clear on your brand attributes. "People just start building without having defined those — even if it's just brand voice and tone guidelines — and then they're surprised when the agent doesn't do what they want it to do," said Gando. "It can get very tricky."


### 2. Test your agents


Once your agent has a clearly defined character, make sure its guardrails hold. How does it handle uncertainty? What does it do when it receives a malicious prompt? And, said Richards, "Does it actually explain when it can't do something?"


Salesforce's Trust Layer automatically includes toxicity detection and security guardrails, so those features are already built into Agentforce. But you still need to test to see how an agent responds to various situations.


Agentforce Testing Center can help with this by creating full, simulated conversations. In particular, test how your agent handles edge cases, or situations that occur only rarely: How does the agent react when a user asks it to reveal information about a competitor? How does it respond to a question it has never encountered before?


"There are all these edge cases and nuances that humans may feel are implied in what they're telling the agent to do, but that the agent may not fully understand when it's interacting with users. It has to have an answer or fallback mechanism in plain language so it can course correct and follow a path that feels helpful to the user," said Gando.


In some cases, the answer may simply be, "I'm going to escalate this to a human, who will know better what to do."


### 3. Measure the outcomes


Once an agent is launched, you also need to measure the quality of its work.


Start by listing the qualities you want your agent to display. Gando's team created a list that includes: Is the agent factual and reliable? Is it responsive? Can it be trusted? Is it consistent?


Then, measure your agent's performance in two ways. First, did the agent complete its task? If so, that will make the engineers happy. But do not stop there because it is often not the whole story. "The user may have just been unhappy with the result and given up on the conversation. It may look like the task was completed because the user closed it," Gando said.


To get the full picture, you also need to measure whether the user thought the conversation was a success. You can do this by embedding your list of desired agent qualities into evaluations in Agentforce Observability — a tool that helps monitor, analyze, and optimize agent performance — and seeing how the agent measures up. "And when we marry that qualitative and quantitative information, it helps us define what good looks like," said Gando.
