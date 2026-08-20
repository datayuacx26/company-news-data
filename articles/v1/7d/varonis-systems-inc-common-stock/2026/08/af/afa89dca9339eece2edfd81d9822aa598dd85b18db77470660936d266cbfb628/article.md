---
schema_version: "1.0.0"
document_id: "afa89dca9339eece2edfd81d9822aa598dd85b18db77470660936d266cbfb628"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-rss-915499d71e96"
canonical_url: "https://www.varonis.com/blog/rovoblast"
published_at: "2026-08-07T22:15:00+00:00"
first_seen_at: "2026-08-08T01:00:31.152082+00:00"
fetched_at: "2026-08-08T01:00:32.788098+00:00"
content_hash: "sha256:539a9d14fd7352959b33fdbc89b654814c6ce43309b87c8e2d255673c632e35c"
---

# RovoBlast: How One Click Triggered Atlassian’s AI Assistant to Leak Data

Varonis Threat Labs uncovered a vulnerability in Rovo, Atlassian's enterprise AI assistant. Dubbed RovoBlast, a single click on a link triggers the attacker's embedded instructions and forces Rovo to accept externally supplied parameters as trusted inputs within a user's session. No jailbreaks, no permission bypass, and no warnings or confirmation.


The same capabilities that make Rovo a powerful tool also make RovoBlast especially dangerous. Rovo operates as an AI layer across the core products in the Atlassian platform, including Jira, Confluence, Bitbucket, as well as other connected SaaS tools like Slack, Microsoft 365, and Google. Atlassian also features autonomous-agent capabilities that can carry out multi-step actions without user involvement.


When AI can search, connect, and act across business systems, the blast radius of a mistake or attack grows significantly, a risk that CISOs and security teams are increasingly concerned about.


Rovo operates within the trust boundary that security teams rely on to enforce controls, visibility, and accountability. Actions executed under a legitimate user identity inherit existing access and blend into normal AI-assisted workflows, leaving little to distinguish abuse from routine use.


We[responsibly disclosed RovoBlast to Atlassian](https://bugcrowd.com/disclosures/bf1922fb-99d0-4d3b-b419-1728720d29ec/one-click-data-exfiltration-via-rovochatprompt-url-parameter-confluence-rovo) , which was fixed and published via Crowd Source in Bug Crowd, then debuted at DEF CON 34. Continue reading to discover how combining trusted input, broad data access, and built-in automations create a low-friction path to organizational data exposure.


## Meet Rovo


Atlassian's Rovo is an "AI teammate" that unifies search, chat, and agent actions across Jira, Confluence, and connected SaaS apps. It's powered by Atlassian's Teamwork Graph and a growing set of connectors and agent capabilities.


Rovo's value comes from context: federated search across Atlassian and third-party tools, conversational answers in Rovo Chat, and task-taking Rovo Agents. The capabilities that make Rovo helpful also create an expansive attack surface if external inputs aren't treated as untrusted throughout execution.


## Parameter to Prompt strikes again


In January 2026, Varonis Threat Labs uncovered[Reprompt in Copilot](https://www.varonis.com/blog/reprompt?hsLang=en) , showing how a single click on a crafted link could turn a benign URL parameter into a Parameter-to-Prompt (P2P) pathway that executes inside the user's trusted AI session. RovoBlast presents a similar opportunity.


Like other AI assistants, Rovo accepts externally supplied parameters that run automatically, meaning a link is infused with data and instructions. Rovo uses the "rovoChatPrompt" parameter to inject content directly into its chat entry. For example, Rovo's chat entry can be invoked with a route and a pre-filled prompt via the following pattern:


*https://home.atlassian.com/chat?rovoChatPathway=chat&rovoChatPrompt=<prompt>*


When a user clicks a link formatted this way, the content is surfaced directly inside Rovo Chat. Without proper guardrails, this P2P technique can be abused to seed attacker instructions into a trusted session — exactly the primitive needed to start a one-click exfiltration chain.


Importantly, the severity of this problem is heightened by the fact that[Rovo cannot be fully uninstalled](https://community.atlassian.com/forums/Rovo-articles/Why-Can-t-You-Disable-Rovo-And-What-to-do-Instead/ba-p/3159063) . Organizations attempting to remove the risk may not be able to eliminate Rovo's presence in their environment or the associated attack surface, making robust input validation and security controls even more critical.


## What can Rovo actually access?


Once we discovered we could reliably run arbitrary instructions as the user, the next question became unavoidable: **What exactly does Rovo have access to inside the organization?**


Rovo isn't just a chat interface; it's a federated access layer glued onto Atlassian's ecosystem and every connected SaaS source. And unlike traditional search engines that tend to stay in their own lane, Rovo happily blends data from multiple systems, interprets it, and summarizes it on demand. We made a simple request for it to list all available data sources. The results speak for themselves:


Rovo confidently enumerated every major surface it could read from, including Jira, Confluence, Bitbucket, Slack, Google Workspace, Microsoft 365, relational databases, uploaded files, web pages, and even archives. No jailbreak, filter bypass, or friction just an honest list of everywhere it can look into.


The full list is even larger due to[Rovo Connectors](https://www.atlassian.com/software/rovo/connectors) that allow more than 50 different platforms to be connected.


## Finding the perfect exfiltration path through the ResearchAgent


At this stage, we knew two things:


1. Rovo will happily run attacker-supplied instructions
2. Rovo has access to essentially everything an organization stores across its connected platforms


The natural next step was to figure out *how* to turn that access into *actionable leakage* . We went hunting for agent capabilities that could turn seeded instructions into a full data exfiltration chain. Rovo didn't disappoint.


While enumerating its available tools, ResearchAgent immediately stood out.


ResearchAgent looks like a standard "internet research" tool at first glance, but when you read what it can do — specifically what it can do *autonomously —* the implications become clear. The first two bullets in the chat below highlight a turnkey leakage engine:


Let's break these bullets down further:


- **Deep multi** ‑ **source open web research:** If an attacker can seed a prompt, Rovo can pull data from internal sources and then *push it to the public web* as part of its "research"
- **Multi** ‑ **step browsing and navigation across arbitrary websites:** Multi‑step autonomy means fetch → transform → upload is just a chain of actions away


In other words, ResearchAgent isn't just a browsing tool. It's everything an attacker needs to convert internal organizational knowledge into an externally reachable payload-with zero user interaction beyond the initial click.


We didn't need jailbreaks or prompt‑surgery exploits. Just a single Rovo link pre-filled with a crafted prompt.


This is where the threat escalates from, "Rovo can leak data if misused" to **"Rovo includes built** ‑ **in automation that accelerates exfiltration once misused."**


## Where are my safeguards?


Enterprise AI assistants such as Rovo or Microsoft Copilot operate at the intersection of highly privileged data, automated actions, and untrusted inputs, including links, documents, connectors, and comments. That is a powerful and risky mix.


The same features that make assistants helpful — pre-filled prompts, auto-context, agent tools, federated search — also create paths for instruction injection, context confusion, and silent data leakage if strong guardrails aren't in place.


In our testing, Rovo's guardrails around untrusted prompts were almost non-existent:


- A crafted link using rovoChatPrompt (e.g., https://home.atlassian.com/chat?rovoChatPathway=chat&rovoChatPrompt=<prompt>) auto-surfaces content directly into Rovo Chat
- The organization ID from "/o/< ID>/chat" can be empty in the URL and Atlassian will redirect it directly into the default organization ID of the user:


- No warning, confirmation, and taint label indicating the session was seeded by an external parameter
- Rovo searched across organizational content (Jira, Confluence, connected SaaS) when asked and summarized sensitive information with ease. In fact, we were able to successfully exploit this behavior with minimal to no guardrail bypasses in most cases. The lack of meaningful barriers allowed untrusted input to flow directly into trusted sessions and exfiltrate data.
- Since the session is already saved in the user's browser, all the attacker needs is a click to obtain the desired data.


## Double request and chain request are not needed


Our exploit path does not rely on chain request to bypass guardrails, one P2P click was usually enough to get Rovo to retrieve and summarize sensitive data. However, once seeded, chaining autonomous steps via ResearchAgent (e.g., multistep browsing and posting) reduces visible touchpoints and keeps actions within a single agent running.


Practically, that means fewer user-facing interactions, fewer opportunities for the UI to warn or interrupt, and a cleaner audit surface that looks like "normal research" rather than repeated user prompts.


As a reference to our original Reprompt research, double-request was not required here; the leakage path worked without it.


## RovoBlast in action


## Recommendations


For Atlassian customers, the most effective defense is to shrink Rovo's blast radius within your organization.


Limit which systems Rovo can access, disconnect unused integrations, and keep high sensitivity areas (legal, HR, finance, IR) out of scope entirely. The less the assistant can see, the less it can leak, regardless of prompt injection or agent abuse.


Our second recommendation is to reduce Rovo's capabilities that are unnecessary for your use. Disable browsing agents, multistep automation, or any AI features your teams don't actively rely on. Pair that with basic monitoring tasks such as reviewing assistant logs, alert on unusual agent runs, and periodically test how your environment reacts to seeded prompts. These lightweight steps go a long way toward containing the damage if (or when) a malicious prompt finds its way in.


## The AI hacking trifecta


While analyzing RovoBlast, we noticed a pattern that extended far beyond Atlassian Rovo. Similar attacks have appeared across multiple AI platforms, each using different technologies but following the same underlying path.


Simon Willison describes the "Lethal Trifecta" for AI agents as the combination of access to private data, exposure to untrusted content, and the ability to communicate externally. RovoBlast clearly fits that model, but our research showed something interesting: direct internet access is not always required. In SearchLeak, for example, the assistant itself could not reach the internet, yet data still escaped through trusted services and browser behavior.


As a result, we started thinking about these attacks through a different lens: **Enter, Evade, Escape.**


### **Enter**


Every attack starts with turning data into instructions.


In RovoBlast, that entry point was the rovoChatPrompt parameter. In other platforms, it might be an email, document, web page, knowledge base entry, repository, connector, or agent memory. The common theme is that content crosses a trust boundary and is later interpreted as a command.


A useful rule of thumb: If the model can read it, it can potentially become an instruction.


### **Evade**


Once inside, attackers must bypass whatever controls are intended to stop them.


Sometimes that means prompt smuggling, role confusion, encoding tricks, or carefully crafted wording. In other cases, it is much simpler. During our testing, Rovo often required little to no guardrail bypassing to retrieve and summarize sensitive information. The challenge is not that controls do not exist, but that security checks and execution paths do not always interpret the same data in the same way.


### **Escape**


Finally, the data needs a way out.


Direct internet access is the obvious route, but it is rarely the only one. ResearchAgent demonstrated how built-in browsing capabilities can create a natural exfiltration channel. Other systems may provide alternative paths through trusted domains, image fetches, link previews, webhooks, logs, third-party connectors, or other agent actions.


The key lesson is that blocking one exit does not eliminate the risk. Attackers look for the next trusted pathway capable of carrying data beyond the intended boundary.


RovoBlast wasn't just a vulnerability in Atlassian Rovo. It was another example of a broader AI security pattern where untrusted inputs, autonomous behavior, and trusted communication paths combine to create new opportunities for data exposure.


Attack


Enter


Evade


Exit


Reprompt


q param (P2P)


double-request


URL fetch


RovoBlast


Rovo Chat prompt


polite urgency


ResearchAgent


SearchLeak


q param (P2P)


render race


Bing SSRF


EchoLeak


email


XPIA classifier bypass


Teams/SharePoint proxy


Antigravity


poisoned doc


subagent approval bypass


webhook.site


ForcedLeak


Web-to-Lead


trusted-context abuse


expired CSP domain


Superhuman


email


image-tag exfil


Google Form GET


GitLab Duo


poisoned repo


sanitizer (DOMPurify) race


markdown image


ShadowLeak


email


server-side execution


agent browse tool


AgentFlayer


poisoned doc


url_safe bypass


Azure Blob image


CometJacking


URL param


Base64 (DLP bypass)


external URL


## The bottom line


What RovoBlast exposes is not just a prompt injection flaw but a trust gap at the heart of enterprise AI. Organizations are racing to connect AI systems to more data to drive productivity, but every new connection increases the blast radius when something goes wrong.


Rovo didn't expose this risk recklessly. It did so because it operates deeply inside the enterprise trust boundary, with access, identity, and autonomy by default. In that environment, a single misclassified input can quietly turn productivity into data exposure.


Reprompt first showed this risk in consumer AI. RovoBlast shows how much higher the stakes are when the same patterns move into the enterprise.


Learn more about assessing AI security risk in your environment with more findings from[Varonis Threat Labs](https://www.varonis.com/varonis-threat-labs?hsLang=en) .
