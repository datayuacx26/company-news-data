---
schema_version: "1.0.0"
document_id: "b6513919dfaa352f65e0582ff785afa36863797cd0ac2409fa4ec992cd1c3c39"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-d41d574f701a"
canonical_url: "https://www.salesforce.com/news/stories/customer-zero-pilots/"
published_at: "2026-07-22T15:00:00+00:00"
first_seen_at: "2026-07-22T15:03:44.763629+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:c392d04091649c268df8debc40e502edae6bcd086c032b6eba58910037ef1053"
---

# How Salesforce Pilots Its Own Software

Companies often let customers and beta testers find their software bugs. Salesforce decided to find them first.


Before most major products reach a customer, it runs a rigorous gauntlet: 75,000 Salesforce employees using it in their day-to-day jobs. They file support tickets, stress-test it during chaotic real-world scenarios, and query it in multiple languages. In short, they do what real users do, which is often entirely different from what product teams anticipate.


This is the Customer Zero model. As[autonomous AI agents](https://www.salesforce.com/agentforce/ai-agents/) proliferate faster than most organizations can evaluate them, the way Salesforce stress-tests its own products internally has become one of the most instructive processes in enterprise software.


## **Following a belief, not a process**


Ask Andy White, SVP of Salesforce on Salesforce Technology, to explain Customer Zero, and he describes a philosophy rather than a process. “Customer Zero is a deep belief that Salesforce has held for a long time,” White said. “By running the company on our own products, we make our products better.”


> Customer Zero is a deep belief that Salesforce has held for a long time. By running the company on our own products, we make our products better.
>
>
> Andy White, SVP of Salesforce


White’s team, part of the Digital Enterprise Technology (DET) organization, acts as a demanding internal customer for Salesforce’s product teams, not a polite testing ground. They embed new tools into genuine workflows, measure them against rigorous business outcomes, and feed those findings directly back into the development cycle.


DET’s relationship with product development is multifaceted: an advocate representing the needs and frustrations of 75,000 internal users combined with a business operator managing costs, tracking metrics, and hitting performance targets. Last but not least, they are advocating for solutions for Salesforce customers, – not bespoke solutions for Salesforce itself.


Employees understand the exchange, as early access to new tools that won’t reach the broader market for months creates genuine enthusiasm, and that privilege is part of what makes the model work. While a traditional beta program asks a subset of customers to sample a feature and report back, Customer Zero mandates that an entire enterprise run its business on unfinished software and treats that initial discomfort as both the point and the price of admission.


## **Choosing what to pilot**


Not every tool is an immediate Customer Zero candidate. A selected tool is an intentional investment in a product the company wants to go faster while figuring out how the product could or should work. Wherever possible, the tool is paired with a greenfield approach: finding an operational area where Salesforce has a clear internal business need and no existing solution and starting there. In a true greenfield space, there is no existing process to disrupt, no deep-seated user expectations to violate, and no legacy performance baseline to defend. If an early pilot stumbles, developers can course-correct without damaging trust in systems employees already depend on.


The idea behind Customer Zero isn’t new. Salesforce has run its business on its own products for years, an approach the company calls Salesforce on Salesforce. What changed with the arrival of[Agentforce](https://www.salesforce.com/agentforce/) was the pace. For the first time, building an agent required no code and very little lead time, and teams across the company responded accordingly. The agent count climbed into the hundreds almost overnight, adoption became inconsistent, and quality suffered. Liz Aloisi, Customer Zero & Agentforce Transformation Lead and manager of Salesforce’s cross-functional agent portfolio, had a name for it: agent sprawl. This disarray generated Customer Zero learnings: fewer agents, built better, and scaled deliberately.


Product-market fit matters too. Customer Zero works best when the internal target market mirrors the external one. As White put it, if the product is built for a customer that doesn’t look like Salesforce, it’s probably not a good fit for internal piloting. The alignment between who builds, who tests, and who eventually buys makes the feedback meaningful.


## **Designing the pilot**


Once a candidate is identified, every Customer Zero pilot follows a structured path: ideate the use case, configure the agent, test it, deploy to a controlled release ring, then supervise closely before expanding. Release rings typically start small, with a few hundred employees, and then scale based on signal rather than schedule. White’s team won’t expand an agent to a full population until it hits 70% accuracy, a threshold that reflects confidence in the agent’s performance against the human baseline it’s replacing.


The “supervise” stage is where the real learning happens. Using[Agentforce Observability](https://www.salesforce.com/agentforce/observability/) , teams inspect every interaction, including what employees asked, what the agent returned, and where conversations broke down. Those patterns drive the next iteration.


Jason Perez, Senior Director of TechForce, and his team ship updates to the TechForce Agent constantly — analyzing interactions, adjusting the model, and expanding access incrementally as confidence builds.


## **Customer Zero in action: The TechForce Agent**


The TechForce Agent is one of Salesforce’s clearest examples of the Customer Zero process working as designed. TechForce handles IT support for all 75,000 Salesforce employees in a high-volume, high-stakes environment where failures are visible and feedback is immediate.


When Perez’s team began piloting an AI agent to handle support requests, they started with the simplest possible use case: answering knowledge questions. Handle rates — the share of support requests fully resolved by the agent without human involvement — climbed into the low 30s almost immediately, a strong cold-start result that gave the team the early signal it needed to expand.


The more complex workflows came next and, with them, the kind of friction that only real-world deployment produces. TechForce frequently deals with lost and stolen devices, a process that requires an hour-plus per incident for a human to review and process. The group initially deployed an agent to handle the reports, but employees described missing equipment in natural language, some typing “phone,” others “iPhone,” and others “Pixel.” Because the agent wasn’t able to map those terms to the company’s precise asset database, it risked locking the wrong device or shipping a replacement to the wrong address.


The fix required building a deterministic matching layer against serial numbers and device records — a solution born from a real IT workflow. That solution also directly contributed to the development of Agentforce Graph, a capability that allows agents to make precise, verified connections between natural language inputs and structured enterprise data, and something external customers now receive out of the box. It was a vivid example of how Customer Zero generates product intelligence that controlled testing never would. Perez’s team didn’t just improve their own process. They handed the product team a problem they hadn’t anticipated, and the product team solved it for everyone.


Over 18 months of iteration, the TechForce Agent’s customer satisfaction score climbed from 76% to 90%, with a target of 95% by year’s end. The lost-and-stolen workflow that once took over an hour now resolves in roughly 15 minutes.


## **Expanding the practical feedback loop**


Testing the TechForce Agent is the internal pilot in its purest form. Other Customer Zero deployments operate differently, running on Salesforce’s own customer-facing properties, where employees and external customers use the same live product side by side, and iteration happens in real time. The Help Agent, Salesforce’s implementation of Agentforce on[help.salesforce.com](http://help.salesforce.com/) , launched in October 2024 and is the most prominent example. More than 4 million conversations later, it resolves nearly 70% of inquiries without human intervention, with a target of 80% by year’s end.


What makes the[Help Agent relevant to the Customer Zero story](https://www.salesforce.com/news/stories/agentforce-help-agent-announcement/) is the iteration discipline behind it. For example, Salesforce users wanted deeper information about where the Help Agent was getting its answers, so Alfasha Zulkifli’s team added inline citations to the agent. When the team tested citations internally, adoption lifted 27% and trust metrics improved measurably. Rolling the feature out to external customers produced the same direct validation of the core Customer Zero thesis: What works internally works externally.


“We get a little jealous,” said Cristina Mondini, Director of GTM AI Strategy, “because our customers get this wonderful solution out of the box, whereas our internal teams had to go through a lot of trial and error to get there.”


## **Defining success and moving the goalposts**


One of the harder problems in[agentic development](https://www.salesforce.com/platform/ai-agent-development/) is predicting what “good” will look like before you have enough data to see it. Before a pilot expands, the team measures agent performance directly against the baseline. “If you don’t know where you’re starting,” Aloisi said, “it’s hard to evaluate what measurable improvements you’ve made.”


Beyond the baseline, success also requires accepting that the targets will move. As an agent’s performance improves against existing test cases, the team introduces harder test cases, like realistic customer questions drawn from actual support data, specifically designed to challenge what the agent has already mastered. A score that represented excellence six months ago becomes the new floor. The mechanism is straightforward: Raise the standard, and the product gets better.


## **What other organizations can learn**


Salesforce’s Customer Zero leaders offer the same core advice to any enterprise looking to engage its own employees as its front line of testers, before a single customer ever sees the product:


- **Start in the green space** : Find an operational area where no tool or workflow already exists. A clean slate lets you establish a true baseline, contain failure risk, and course-correct without disrupting systems people already depend on.
- **Measure before and after** : Establish a human baseline before you deploy, then keep raising the bar as the agent improves.
- **Build into the flow of work** : Integrate agents where employees already spend their time. If adoption requires changing daily habits, it will fail.
- **Earn early wins to build momentum** : Start with simple, high-confidence tasks and use those wins to earn the organizational trust required for complex workflows.


The culture behind the model matters as much as the mechanics. White identifies three elements: Be explicit about where a product sits in its development cycle so employees calibrate expectations accordingly; let people opt out rather than forcing them to work with software in beta; and reframe iteration as the goal rather than a failure. One purpose of Customer Zero is to break products before a paying customer ever encounters them.


None of this requires a massive headcount. “I don’t think it’s about size as much as it’s about signal,” White said. “As long as you’re getting good data points and you’re able to measure what you’re doing, that’s what really matters.”


The true competitive advantage of Customer Zero is the resulting product itself. When Salesforce ships something, it has already survived daily use by employees who had first access and genuine stakes in making it work. Those employees become advocates. The rough edges are found and smoothed and the lessons learned are shared publicly so customers can learn from them too. Any organization can be Customer Zero.


## Share article


-
-
-
-


## Get the latest Salesforce News & Insights, straight to your inbox


1 min read


3 min read


## Share article


-
-
-
-


## Get the latest Salesforce News & Insights, straight to your inbox


1 min read


3 min read


## Explore related content by topic


- [Agentic Enterprise](https://www.salesforce.com/news/topics/agentic-enterprise/)
- [Agents](https://www.salesforce.com/news/topics/agents/)
- [AI](https://www.salesforce.com/news/topics/ai/)
