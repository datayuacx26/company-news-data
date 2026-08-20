---
schema_version: "1.0.0"
document_id: "2ef31a18bee0828badbe5543c319a3bd92090ba9c3c25966c4cb2a06dc77b869"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/how-salesforce-built-an-agentic-engineering-enablement-strategy-for-thousands-of-software-engineers/"
published_at: "2026-07-30T20:09:53+00:00"
first_seen_at: "2026-07-31T23:08:52.771353+00:00"
fetched_at: "2026-07-31T23:08:54.294213+00:00"
content_hash: "sha256:926c8f4651b99af3b55b063e3b41c801f6f4922b028da32acc93dcad0968237f"
---

# How Salesforce Built an Agentic Engineering Enablement Strategy for Thousands of Software Engineers

Giving thousands of software engineers access to agentic tooling is straightforward. Helping them fundamentally change how they build software is another matter entirely; one every engineering organization eventually runs into. The real constraint on[enterprise agentic engineering](https://engineering.salesforce.com/building-enterprise-ai-agents-that-are-both-autonomous-and-reliable/) is not model power, rather, it is an organization’s ability to build new skills, establish shared expectations, and create a common language for human-agent collaboration.


Left on their own, engineers do exactly what you would expect, but not all in the same way. Across an engineering organization spanning thousands of engineers, different patterns emerge. Some engineers experiment immediately, building their own workflows and standards as soon as new tools appear, which is often where the biggest productivity gains surface first. However, without a shared vision and explicit expectations from leadership, that experimentation stays local and can lead to inconsistent practices, competing definitions of “good,” and no path from individual gains to organization-wide ones.


Salesforce faced this directly. The question was never whether our engineers could shift to agentic coding; most already had. The real challenge was building the shared language needed so individual bursts of productivity could compound into productivity at scale. Salesforce’s Technology, People, Innovation, and Learning (TPIL) team took on that challenge: helping an entire engineering organization, not just its early movers, learn and grow together.


##### **Agentic transformation is an organizational learning challenge**


When software engineers receive a powerful new capability, they naturally incorporate it into their own workflows. One engineer develops a prompting strategy that accelerates coding. Another focuses on testing,[debugging](https://engineering.salesforce.com/reducing-agentforce-ai-debugging-from-two-weeks-to-same-day-with-query-driven-observability/) , or documentation. Others discover completely different ways to integrate agents into planning,[code reviews](https://engineering.salesforce.com/scaling-code-reviews-adapting-to-a-surge-in-ai-generated-code/) , or daily engineering work. None of those approaches are wrong. They are exactly what experienced engineers should do when exploring new technology.


TPIL realized the organization was not struggling because engineers lacked curiosity. The challenge was that experimentation alone was not creating a shared understanding of what effective agentic coding actually looked like. Many engineers were still asking the same questions:


- What does becoming[AI-native](https://engineering.salesforce.com/the-ai-native-engineer-how-salesforces-next-generation-is-redefining-software-development/) actually mean?
- Which behaviors matter?
- How do I know whether I am making meaningful progress?


From the beginning, TPIL treated one reality as non-negotiable: engineers do not all learn the same way. Some immediately build production systems. Others prefer examples, structured guidance, or repeated practice before changing how they work. Sustainable organizational change depends on meeting engineers where they are and helping them build confidence as they progress. That principle shaped every design decision Salesforce made in approaching this next phase of agentic transformation. At its core, this was an organizational learning challenge, not a technology one.


##### **Independent signals pointed to the same journey**


Recognizing the agentic transformation as an enablement challenge raised another question. If becoming AI-native was not a single skill to master, what did the journey actually look like? TPIL was not the only team asking. Different pockets of the organization had already started answering it in their own way. One team built a five-level model that broke out orchestration from looping. Another team focused on a 2×2 model which centered on telemetry. What each model ultimately revealed was that everyone was describing a version of the same underlying journey.


Despite working on different products and technologies, engineers consistently described the same breakthroughs, frustrations, and periods of uncertainty. They advanced through similar stages and plateaued in similar places, a pattern we captured in[this recent blog](https://engineering.salesforce.com/the-agent-coding-maturity-curve-9-stages-from-code-generation-to-trusted-automation/) on the Agent Coding Maturity Curve. No single model told the whole story, but together they pointed at the same thing.


TPIL combined that convergence with its own learning strategy to build the Proficiency Level (PL) Framework, deliberately grounded in mindset and behavior, not tools or dashboards. That distinction was the whole point. A framework built around any single tool, IDE, or product would only describe how to use that tool well; the moment the tooling changed, the model would need to be rebuilt. Instead, the PL Framework gave the organization a shared language for how to think and behave differently with agentic tooling: how to delegate, verify, and collaborate with agents as a capability rather than a feature of one product. Master that mindset once, and it travels with you to whatever tool comes next.


What it gave engineers and managers, above all, was a shared definition of “good.” A common answer to what strong agentic work actually looks like at each stage, in place of dozens of competing, tool-specific ones.


##### **Why progression mattered more than proficiency**


Designing the PL Framework solved only part of the problem. The larger challenge was helping engineers progress from one level to the next.


One of the most important mindset shifts involved validation. Many software engineers quickly discover that generating code with AI is easier than judging its quality. Outputs are not always correct. Requirements are not always interpreted as intended. Small mistakes can quietly produce unreliable software. This is where many agentic adoption efforts stall. The challenge is learning how to communicate intent, evaluate agent outputs, and build reliable engineering workflows around them. Learning to write code with agents is only part of the shift; the deeper change is how engineers think about software development itself.


As engineers continue progressing, they shift from using agents to complete individual tasks toward orchestrating systems of agents. Each stage introduces a new mental model, reinforcing that becoming AI-native depends on continuous capability development rather than isolated technical skills. For engineering leaders, that progression carries an important implication: success is measured by whether engineers are consistently developing the judgment, confidence, and capabilities needed to reach the next stage, not by how quickly they reach the final one.


##### **Making progression possible at scale**


A shared definition of “good” pointed engineers in the right direction, but direction alone does not move anyone through levels like these. TPIL built hands-on programs that made progression something engineers experienced, not just understood. AI camps gave engineers dedicated time to practice and experiment rather than just hearing about agentic tooling. Weekly sessions met engineers wherever they were starting from, then evolved as their questions did. Coaching guides gave managers a common language to move a one-on-one conversation from “are you good at AI?” to “what is the next stage of growth?,” turning progression into something managers could actively support rather than simply measure.


What turned the PL Framework from a model into an enablement practice was that combination: a framework that defined the destination and programs that helped engineers actually travel it. The four levels below are what that practice looked like in action.


##### **Why four proficiency levels create a map**


The nine stages on the[Agentic Coding Maturity Curve](http://google.com/url?q=https://engineering.salesforce.com/the-agent-coding-maturity-curve-9-stages-from-code-generation-to-trusted-automation/&sa=D&source=docs&ust=1785390908764409&usg=AOvVaw3hv3fBFtS_Dy-aYfAJyDTn) were accurate, but accuracy was not the problem TPIL needed to solve. Nine stages give a detailed picture, but they are a lot to hold in mind day to day. Change management and learning research consistently point to the same practical limit: four or five steps is about the most someone can act on at once.


TPIL built the PL Framework around four steps: a clear and progressive path with a beginning, an end, and a manageable number of stops in between. Each level does not tell an engineer which feature to reach for next; instead, it tells them which skill, mindset, or behavior to shift.


The four proficiency levels were designed to make growth visible, not to evaluate engineers. This was never meant to be a scoring system, a dashboard, or a telemetry-driven measure of who was “ahead”; it was a shared language for describing how an engineer’s thinking and workflow were changing. Growth was something engineers and managers talked about together, not something a tool measured for them.


- **AI-Assisted** : Engineers shift from writing every line themselves to treating AI as a first collaborator in everyday development.
- **AI-Validating** : Engineers shift from trusting AI output to validating it, learning that productive AI use depends on verification rather than blind trust.
- **AI-Orchestrating** : Engineers shift from directing AI one task at a time to designing systems where multiple AI agents work together toward a larger outcome.
- **AI-Native** : Engineers shift from optimizing their own workflow to codifying it as a shared standard that helps the rest of the organization move faster.


What mattered at each level was not what engineers knew, but how their behavior actually changed.


##### **Measuring enterprise AI adoption through behavior change**


Rather than focusing on attendance or course completion, TPIL looked for evidence that engineering behavior across the organization was actually changing.


Countless engineers participated in AI camps globally during the first quarter and adopted AI tools faster than the organizational baseline. Volume and speed did not prove behavior changed, but they provided TPIL with a large enough population to start tracking whether it did.


The clearer signal came from how the questions themselves changed. Weekly session attendance naturally declined as participants moved beyond foundational topics. The questions shifted with them, moving from how to get better outputs from AI to where human judgment still mattered most. That shift suggested that engineers were not just using agents more; they were thinking about the work differently.


##### **Four principles for building AI-native engineering organizations**


Every engineering organization pursuing enterprise AI adoption will encounter similar challenges, regardless of the AI models or tools they choose. Salesforce’s experience offers four principles that extend well beyond any single framework.


1. **Organizations cannot scale agentic practices until they scale shared expectations.** Software engineers naturally develop different workflows when exploring new technology.
2. **Learning journeys outperform assessment frameworks** . Proficiency is a progression, not a certification. Salesforce focused on moving engineers beyond where they are toward what comes next.
3. **Behavior change is the metric that matters.** Adoption is not about attendance; it is about evolving workflows, coaching conversations, and the complexity of questions engineers ask.
4. **The learning culture matters more than any single framework.** Tools and frameworks will become obsolete. Success depends on an engineering organization’s willingness to keep learning through each disruption.


##### **Building engineering organizations that grow with AI**


Every engineering organization will have access to increasingly powerful AI models. Not every organization will become AI-native. The difference is not the technology, it is how effectively an organization helps its engineers learn, adapt, and grow together.


**Learn More**


- Stay connected by joining our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) .
- Explore our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to see how you can get involved.
