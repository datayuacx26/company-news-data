---
schema_version: "1.0.0"
document_id: "fb9f8dcf548826018e0cd82396f13df2a45857a5eecef7200c534426992b66a6"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/how-to-evaluate-production-ai-agents-measure-system-outcomes-not-conversations/"
published_at: "2026-08-17T22:40:22+00:00"
first_seen_at: "2026-08-18T00:01:47.932862+00:00"
fetched_at: "2026-08-18T00:01:49.106308+00:00"
content_hash: "sha256:abd591f41d8dfe24b771e1969c544ee8c5ad972330f4c359ce808013fa603f6f"
---

# How to Evaluate Production AI Agents: Measure System Outcomes, Not Conversations

Imagine deploying a[production AI](https://engineering.salesforce.com/building-reliable-production-ai-with-durable-workflows/) agent that confidently tells a customer, “I have processed your refund.” The customer leaves satisfied. The conversation looks perfect. Your monitoring shows no obvious errors. Then someone checks the billing system and discovers the invoice is still open. The refund tool was never called. Nothing actually happened.


As AI agents move from chatbots into production workflows that issue refunds, schedule technicians, update customer records, and manage campaigns, AI agent evaluation becomes a high-stakes engineering problem. The challenge is no longer determining whether an agent said the right thing. It is determining whether the agent completed the right work and left the system in the correct state. Which raises the question: how can an agent look correct and still fail?


##### **When the Conversation Looks Right but the System Is Wrong**


Consider what must happen when a customer asks for a refund. The agent has to identify the correct customer and invoice, choose the appropriate tool, call it with the correct arguments, update the invoice status, and only then tell the customer the refund is complete. But most of that work is invisible in the final response.


Two agents can produce the same sentence, “Your refund has been processed,” while only one actually performs the refund. If evaluation focuses on the conversation, both interactions appear successful. The system state tells a different story. This is the central failure of many AI agent evaluation approaches. Traditional LLM evaluation measures generated responses. That works when text is the final output. It breaks down when agents use tool calling to modify production systems. Language becomes the interface. The real output is the work the agent performs. Crucially, this divergence is not a[bug](https://engineering.salesforce.com/how-ai-reduced-customer-bug-triage-from-nearly-a-year-to-less-than-a-week/) in the agent’s code but a property of how LLM-driven agents behave. The model decides, at inference time, whether and how to call a tool. Two runs of the same agent, with identical code and prompts, can land in different places: one emits the tool call, the other simply narrates that it did. This is exactly why the evaluation, not the code, has to verify the outcome.


CRMAgentBench applies this idea by running complete multi-turn workflows with stateful tools, allowing it to verify what the agent actually changed instead of simply evaluating its responses. An effective AI agent evaluation has to observe more than the conversation. It has to verify what the agent actually changed. That resolves the first mystery: an agent can sound correct while failing because its words and its actions are separate outputs. Once you see that separation, the next question becomes unavoidable: if the conversation isn’t the source of truth, what should an AI agent evaluation measure instead?


##### **The Final State Tells the Real Story**


Imagine trying to verify that the refund really happened after the conversation ended. You would not reread the chat transcript. You would inspect the billing system. Did the invoice change? Did the refund complete? Did anything else change that should not have? The system itself contains the evidence.


Production systems trust observable outcomes, not promises. That is the essence of outcome-based evaluation: measure whether the intended work occurred rather than whether the conversation sounded correct. CRMAgentBench implements this idea by giving every task a shared, stateful environment. As the conversation unfolds, the agent’s tools modify that environment. When the task finishes, the benchmark verifies that the correct state changes occurred.


Every production AI agent exists to change something outside the model. The evidence is always the same: the correct record changes, the intended workflow completes, and nothing else does. In a CRM workflow, that might mean a scheduled field-service appointment exists with the correct technician, a coupon is attached to the intended account, or a campaign lookup uses the identifier discovered during the conversation rather than one the model guessed. The conversation tells you what the agent intended to do. The system tells you what actually happened. Correct final state is necessary. It is not sufficient.


*CRMAgentBench scores correct actions and the final CRM state — not just the agent’s claims.*


##### **Why AI Agent Evaluation Must Detect Unsafe Actions**


Suppose an agent refunds the correct invoice but also modifies another customer’s record. The intended state change occurred, but so did collateral damage. Or suppose the agent eventually retrieves the correct campaign data but first calls a tool it was explicitly forbidden to use. The final answer may still be accurate, yet the agent violated the workflow’s operating constraints. A production-ready AI agent benchmark cannot treat either outcome as successful.


CRMAgentBench therefore uses strict, all-or-nothing grading. A task succeeds only if every required assertion passes, including the correct tools, arguments, execution order, final state, and the absence of forbidden actions or unintended state changes. Those checks catch two distinct failure types: an agent that performs the right action while modifying something it should not, and an agent that reaches the correct outcome after using a forbidden tool. Production systems need to detect both.


The benchmark’s hard tasks make this concrete. One marketing task withholds the campaign ID, forcing the agent to discover it before retrieving ROI. The benchmark verifies both the discovered ID and the order of tool calls, making guessing impossible. Other tasks require the agent to ask clarifying questions before acting, testing not only execution but restraint.


The lesson extends beyond any benchmark: a production AI agent must do everything required, nothing prohibited, and avoid changing anything outside the intended scope. By this point, the engineering question has shifted from “Did the AI agent generate the correct response?” to “How should you evaluate an AI agent that can modify production systems?” What if an agent can meet all of those requirements sometimes, but not consistently?


##### **Why Reliable AI Agents Need More Than One Successful Run**


Imagine an agent that succeeds once, fails the second time, and succeeds again. A one-shot score hides that instability. It tells you the model succeeded once, not that you can depend on it in production. One of the biggest challenges in production AI is distinguishing occasional success from dependable success. Users never experience an average; they experience a single interaction. At scale, even an agent with a high overall success rate can still create costly failures.


CRMAgentBench measures this with pass^k. Each task can be executed repeatedly, and pass^k estimates the probability that the agent succeeds on all k independent attempts: succeed 9 times out of 10, and pass^10 already drops to about one-in-three. Unlike pass@k, which rewards getting at least one successful result, pass^k rewards consistency. Two models with similar Task Success Rates can have very different reliability profiles. A low-variance model retains more of its performance as k grows, while a model that succeeds only intermittently falls quickly. Modern AI agent benchmarks increasingly measure reliability, not just one-off performance.


The transferable lesson is simple: evaluate whether your agent can repeat a correct workflow, not merely demonstrate it once. But even a reliable benchmark eventually encounters another limit. What happens when models improve enough to solve almost every task?


*How a Benchmark session runs: a multi-turn conversation with a persistent CRM state.*


##### **A Benchmark Stops Working When Nothing Is Difficult**


Suppose your evaluation harness suddenly reports that every frontier model is scoring near 100%. That sounds like success until you ask a harder question: what if your benchmark has simply stopped measuring anything difficult? When every strong model achieves nearly the same score, the benchmark stops helping you choose between them. It still produces numbers, but those numbers no longer reveal meaningful differences.


CRMAgentBench encountered exactly this problem. Earlier versions saw frontier models clustering near perfect scores, making it difficult to distinguish the strongest performers. Repeating the same easy tasks doesn’t solve the problem. Reliability metrics such as pass^k only expose variability when the underlying work remains challenging.


The solution is to make the benchmark harder. CRMAgentBench keeps that process inexpensive by defining agents and tasks declaratively. Engineers can introduce harder scenarios without rebuilding the framework: requiring multi-hop discovery, withholding key information so agents must ask clarifying questions, or enforcing policies that require refusing unsafe requests. Those techniques define the benchmark’s hard tier. Tasks combine discovery, strict execution order, state validation, adversarial prompts, and refusal requirements. Even the strongest model in the reported results dropped from 96% on easy tasks to 66% on hard tasks.


The broader lesson applies to any engineering team building production AI agents: your evaluation framework should be as extensible as the system it protects. If adding harder workflows is expensive, your benchmark will eventually stop exposing the failures that matter most.


##### **What to Change in Your Own AI Agent Evaluation Framework**


CRMAgentBench ultimately demonstrates a broader engineering principle. Production AI agents exist to change systems outside the model, which means AI agent evaluation should measure system outcomes rather than conversations.


When designing an AI agent evaluation framework, begin with the expected outcome. Define the state that should change, the actions required to change it, and the records that must remain untouched. Specify which tools are allowed, which arguments must be discovered rather than guessed, and which operations must occur in a specific order. Then evaluate the workflow repeatedly. A production-ready AI agent benchmark should answer five questions:


- Did the agent take the correct action?
- Did it use the correct arguments and sequence?
- Did the system reach the intended final state?
- Did it avoid forbidden actions and collateral damage?
- Can it repeat that success consistently?


Those questions apply whether your application supports customer service, commerce, infrastructure, healthcare, finance, or any other domain. The workflows will differ. The engineering standard should not. Outcome-based evaluation measures whether a production AI agent successfully changes the external system it controls, not simply whether it generates a convincing response. The most important output of a production AI agent isn’t the sentence it generates. It is the change it leaves behind.


##### **Learn More**


- Stay connected by joining our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) .
- Explore our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to see how you can get involved.
