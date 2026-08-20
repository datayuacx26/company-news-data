---
schema_version: "1.0.0"
document_id: "e684ff9f3829d6fe3fa9fcd6dddfd6ad8bb8ea976c123790a33155e3082da380"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/microsofts-project-perception-announcement-and-how-to-implement-it-right/"
published_at: "2026-07-27T20:23:03+00:00"
first_seen_at: "2026-07-27T21:48:01.398992+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:bac7109b25fc3e177a9d58f0cf18c011aca5a4c37b536425a522f32f8b603684"
---

# Microsoft’s Project Perception Announcement And How To Implement It Right

Today, Microsoft announced Project Perception: a series of red, blue, and green team agents designed to be coordinated together in an agentic architecture to evaluate infrastructure and close gaps as close to autonomously as possible. The red team agents find potential paths to compromise, the blue team agents prioritize and evaluate them, and the green team agents build and implement security controls to harden the environment.


This is a different level of coordination and evaluation than we’ve seen with previous Microsoft announcements like MDASH. While MDASH is focused on vulnerability scanning and identification, Project Perception proposes to address the entire security lifecycle, from identifying attack paths to prioritizing and implementing fixes that harden the environment and introduce new detections.


Microsoft’s initial demos focus on hardening web apps exclusively. They show that the blue team agents are able to pull in and evaluate threat intelligence information that can then be passed off to red team agents to evaluate attack paths, perform reconnaissance, and scan for vulnerabilities. Blue team agents can investigate and prioritize the potential attacks, as well as build net-new detections to identify attacker behavior in the future. Green team agents can build potential fixes for vulnerabilities and even connect to GitHub to propose the fix and open a pull request. Project Perception also includes an MCP server so that the actions can also be executed in the CLI.


For now, the agents are available exclusively for Microsoft Defender. As of next week, they’ll be available — for now — to a select group of customers in private preview. Pricing for these agents will be based on consumption.


In its announcement, Microsoft states that its graph is a differentiator in its approach, as it makes it simpler for agents to gather context. While this could be true, it also highlights an important trend: The cyber platform push is undeniably intertwined with the AI agents and agentic systems being deployed. The more comprehensive and cohesive the visibility from the cyber platform, the better outcomes the agentic systems can realize.


### **Project Perception Gives Agentic Coordination To Users — Now Comes The Hard Work Of Implementation**


Especially juxtaposed against recent developments like OpenAI’s model attack on Hugging Face, Microsoft’s announcement represents a major development in a harness and capability that will soon be available to the public, promising to provide autonomous hardening of the environment that goes beyond singular agents fulfilling specific functions.


Other vendors like Wiz have released red, blue, and green team agents with ways for users to coordinate them. The differentiating element of this announcement is the coordination enabled by the agentic architecture between these agents and the way they work together to close the loop between identification, evaluation, and remediation.


### **Microsoft’s MAI-Cyber-1-Flash Is Also Here**


Microsoft also announced its first cybersecurity model focused on vulnerability analysis: MAI-Cyber-1-Flash. Microsoft has released several custom models over the past year, starting with the image model MAI-Image-1, with other models for transcription, reasoning, coding, and speech, but this is its first custom cybersecurity model. The shift in custom model development by Microsoft shows its desire to control the entire security stack, which it highlights in the announcement, along with a need to manage costs and control tuning of the models for specific tasks for quality.


Project Perception doesn’t exclusively rely on Microsoft’s model — the harness has an orchestration layer to choose the best model to manage quality, reliability, latency, and cost. It’s the equivalent of a multiplexer but for models. This is a best practice that Forrester recommends to any organization building a harness or evaluating a vendor’s AI capabilities — they should all support multiple models and orchestrate between them depending on the use case.


Demos and blogs can only show us so much, especially when it comes to AI. Many AI features released over the past several years talk a big game but have constraints — such as lacking business context or being too costly — that can only be understood in real-world, production deployment. Agents are nondeterministic and will potentially take different execution paths and produce different responses. This compounds in an agentic architecture, where agents can suffer cascading failures. To make the most out of a real-world deployment of an agentic architecture:


- **Require observability by default.** Failures are as opaque as the data that’s provided, so require observability data by default to monitor, detect, and understand when something goes wrong.
- **Ensure least privilege access/least agency.** No one wants a repeat of the OpenAI/Hugging Face incident. Strictly limit permissions so that agents can only access exactly what they need to at the time they need and nothing more. It’s important to separate permissions at an agent level, even in an agentic architecture, to ensure agent traceability and encapsulation to specific tasks and that they don’t take irreversible actions.
- **Give the system access to the right data.** Context is everything for an AI agent, and this is doubly true for an agentic system. Give the agent data about the enterprise environment in a properly formatted, clean structure; otherwise the agent will spend time and money on finding the right data (or potentially hallucinating and falsifying it) instead of solving the right problem.


Forrester has received positive feedback from users using AI agents from Microsoft, like the Phishing Triage Agent, but this system is much more complex. Time and practitioner experience will tell.


### **Connect With Me**


If you have questions related to this announcement and are a Forrester client, connect with me via an[inquiry or guidance session](http://forrester.com/inquiry) .


Share


-
-
