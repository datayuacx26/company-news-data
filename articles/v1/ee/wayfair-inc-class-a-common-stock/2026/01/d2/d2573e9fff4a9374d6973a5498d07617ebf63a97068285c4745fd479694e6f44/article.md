---
schema_version: "1.0.0"
document_id: "d2573e9fff4a9374d6973a5498d07617ebf63a97068285c4745fd479694e6f44"
company_key: "wayfair-inc-class-a-common-stock"
company: "Wayfair Inc."
source_id: "wayfair-inc-class-a-common-stock-news-import-42cff1750757"
canonical_url: "https://www.aboutwayfair.com/careers/tech-blog/flag-early-fix-faster-real-time-intervention-for-supplier-transfers-with-wilma"
published_at: "2026-01-15T20:57:44.988+00:00"
first_seen_at: "2026-07-22T19:30:59.923738+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:86fe288b13547883ca470730d2b786e997bab980ef316ad897aaf8c9f3147f5d"
---

# Flag Early, Fix Faster: Real-Time Intervention for Supplier Transfers with Wilma

From its beginning, customer experience has been a core principle at Wayfair. We continually seek ways to make every interaction a delightful experience even when something goes slightly amiss. As part of this commitment, Wayfair developed the Supplier Transfer Program (STP).


The STP is a customer engagement program designed to give suppliers greater involvement in the post-order experience, enabling them to more effectively manage incident-related costs. Through the program, suppliers will interact with the customer, diagnose the post-order issue and process the appropriate resolution. By leveraging their product expertise, suppliers enrolled in the STP can offer the most appropriate resolution for the customer. This encourages more low-cost resolutions, better customer satisfaction and in the moment customer feedback regarding product quality, packaging and supplier customer service.


But as STP scales now spanning more suppliers, more products and more customer interactions, the volume of ongoing conversations grows rapidly. Each of these conversations represents a moment that matters to the customer and without the right safeguards, even small missteps can quickly translate into confusion, delays or frustration.


Historically, maintaining quality within STP relies heavily on manual monitoring. A team of Wayfair associates “patrol” supplier–customer conversations, skimming message threads for signs of confusion, stalled progress, policy mismatches or customer frustration. This process is time-consuming, inconsistent by nature and difficult to scale. As caseload grows, the likelihood increases that a contact transferred to a supplier is not handled appropriately or is reviewed too late, causing a poor supplier experience and a poor customer experience. These instances are critical, as they often indicate moments when the customer experience is at risk and where a Wayfair associate needs to intervene quickly to course-correct.


To address this, we introduced our Large Language Model (LLM)-based automation for intervention flagging, Wilma, an AI-powered system designed to proactively identify supplier transfers that may require Wayfair associate involvement. After each new exchange between suppliers and customers, a ticket comment is automatically generated. Wilma evaluates the comments in real time, analyzing customer, supplier and associate messages to determine whether the interaction is progressing smoothly or showing early signs of risk.


When Wayfair involvement is needed, the model applies a clear reason code and flags the ticket directly in ServiceHub, the platform that integrates various communication channels and customer information, enabling associates to focus their attention where it will have the greatest impact on the customer.


By automating what was previously a fully manual process, Wilma strengthens the operational guardrails that ensure STP continues to deliver on its promise to customers. Continuous, real-time monitoring replaces delayed human spot checks, allowing the system to quickly identify and escalate situations where customers might otherwise experience confusion, delays or inconsistent outcomes. This proactive approach helps ensure customers receive the right resolution faster, improving their overall experience.


Early results show that Wilma can automatically clear approximately 65% of STP tickets without human intervention. This allows Wayfair associates to focus on the most complex or sensitive cases, improving response times and consistency while ensuring that customers receive timely, high-quality support when it matters most.


# How Does Wilma Work?


The STP Intervention Flagging system runs on an event-driven automation pipeline using LangGraph. Its job is simple: every time a new supplier–customer message comes in, it instantly (approximately one second) decides whether a Wayfair associate needs to step in, with no manual monitoring or waiting.


When someone adds a new comment to an STP ticket in SupportHub, a webhook sends an event to a Publish/Subscribe (Pub/Sub) topic. Our Pub/Sub consumer listens for these events and as soon as it detects a new comment, it triggers the LangGraph workflow. The graph then coordinates everything needed to determine whether intervention is required: calling LLMs for classification, using specialized prompts and updating the ticket’s metadata directly in SupportHub.


Wilma’s event-driven system architecture


Once the workflow graph is triggered, it runs through a focused sequence of classification and decision steps:


1. Identify who wrote the comment


The system first deterministically identifies whether the new comment came from a customer, a supplier or a Wayfair associate. This matters because each type of author has different situations that might require intervention.


2. Evaluate the comment using role-specific criteria


The system dynamically selects a specific prompt for the author based on their role, which only includes intervention categories relevant to that role. This targeted approach minimizes noise, boosts accuracy and streamlines the process by eliminating unnecessary comparisons.


For instance, the intervention categories are tailored as follows:


- **Customer:** frustration, stalled issue, refund escalation
- **Supplier:** incorrect guidance, policy misalignment, inability to act
- **Wayfair associate:** internal notes that may require escalation


Wilma’s graph architecture


3. Decide whether intervention is needed


If the model determines that intervention is required, the system automatically posts an update on the ticket in SupportHub. This includes adding the **intervention flag** , specifying the **intervention reason** and providing a short **rationale** for why the intervention is needed.


When a ticket is flagged, it is automatically routed to a global partner support associates’ queue, providing the associate with critical information for immediate intervention, including notification of the required intervention, the specific reason code for the flag and the precise comment that triggered it, which enables efficient prioritization and addressing of the highest-risk conversations.


**Intervention Flag Example (STP Transfer)**


**Customer Comment**
*“I already gave you photos so many times! I want to speak to a manager.”*


**Wilma Assessment**
**Intervention needed:** Yes


**Intervention Reason:** Escalating Frustration or Conflict


**Rationale:** The customer's statement ("I already gave you photos so many times!") clearly signals growing frustration and a demand to escalate to a manager, indicating a breakdown in the current support interaction. This meets the criteria for **Escalating Frustration or Conflict** and requires Wayfair intervention.


# What We Measure


Wilma is evaluated on precision, recall, reason accuracy and timeliness — metrics that determine how effectively and reliably it identifies tickets requiring associate intervention. Wilma significantly outperformed its original target human baselines of 80% precision and 90% recall, achieving **95% precision** , **97% recall** , **99.8% reason accuracy** and **99.7% timely flagging** , all surpassing human baseline performance. These results confirm that the model is both highly accurate and operationally dependable in real-time decision-making.


Wilma's improvements translated into significant operational gains, directly enhancing the customer experience: a 29% reduction in time-to-resolution compared to the pre-Wilma period in which means customers get quicker answers and outcomes, cutting down on frustration and waiting time.


Wilma has also improved the supplier experience, as evidenced by an increase in the supplier satisfaction score from 3.1 to 3.8 (out of five), driven by faster response times and proactive agent interventions. The solution’s effectiveness is further validated by a 2.8% reduction in overall ticket re-open rates, indicating that issues are being resolved correctly the first time. Collectively, these outcomes illustrate Wilma’s capability to improve operational scale, boost resolution speed, ensure greater consistency and elevate the overall customer experience, all without increasing the need for manual work.


# Lessons Learned: Author-Aware Prompts


The initial design utilized a single prompt to process all incoming valid tickets, achieving an accuracy of 70%. A significant issue was model hallucination, where the model incorrectly predicted intervention reason codes that were not applicable to the comment author. This approach also incurred a higher input token cost due to the single, consolidated prompt. Furthermore, the use of a format string template for prompting proved inefficient, as it offered only a static method for handling the prompts.


Based on the learnings from the initial approach, the agentic workflow graph was optimized by splitting the prompt into three distinct parts, each based on the comment author of interest (Supplier, Customer or Wayfair associate). This modification effectively eliminated model hallucinations, as each prompt was now dedicated to only the relevant classes for the specific comment author. Instead of the format string, the Jinja2 template format was adopted, which allowed for a more dynamic, Pythonic approach to splitting the prompt using an if-elif-else block structure. This new design also resulted in a lower input token cost, as the overall prompt size for each comment author was reduced by removing irrelevant intervention reason code classes. Consequently, the prediction accuracy increased significantly to 96% with this optimized design.


Initial agentic workflow graph to final optimized agentic workflow graph


**Author-aware prompts beat one-size-fits-all.** Rather than relying on a single generalized prompt in which many intervention reasons were not applicable to a given comment author, separating the prompt into author-aware variants ensured that each prompt only included the intervention reasons relevant to that author. This constraint significantly increased the deterministic behavior of the overall solution by reducing ambiguity in the model’s decision space, which in turn improved precision, recall and overall prediction stability.


# Lessons Learned: Evaluate and Improve


Wilma incorporates continuous evaluation through both human-in-the-loop feedback and systematic model monitoring. Each flagged ticket requires associate validation and any disagreement triggers a structured “thumbs-down” workflow where the associate identifies whether the flag or its reason was incorrect and provides the corrected rationale. These real-time signals form a high-quality feedback loop that directly informs ongoing model refinement.


Beyond human feedback, Wilma’s performance is continuously monitored through offline evaluation, quality assurance team reviews and observability tooling in Arize. Arize enables trace-level visibility into flagging behavior, supports rapid debugging and provides automated alerts for anomalous shifts in key metrics, ensuring the model remains accurate, stable and reliable as workflows evolve.


Visualization of a graph invocation in Arize


# Future Plans


While the constant improvement of prompts and flagging categories is necessary to keep pace with evolving business processes, we are also exploring the development of reusable tools to expedite ticket automation across various workflows. These tools are designed to enhance information gathering, allowing AI agents to fetch necessary context from sources beyond the immediate ticket content, such as building Model Context Protocol tools for extracting essential information from unstructured sources like order trail, a tool to log and track all actions and communications related to an order. Providing AI agents with access to the right tools will significantly expand their knowledge base beyond pre-programmed prompt instructions, empowering them to make accurate, data-driven decisions and take the required actions.


*The authors would like to acknowledge and thank Dave Parry, Graham Ganssle, Erin McGuire, Claire Perreault and Janet Yue for their contributions to Wilma.*
