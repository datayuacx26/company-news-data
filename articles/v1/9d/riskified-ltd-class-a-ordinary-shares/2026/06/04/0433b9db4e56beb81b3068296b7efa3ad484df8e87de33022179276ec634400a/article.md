---
schema_version: "1.0.0"
document_id: "0433b9db4e56beb81b3068296b7efa3ad484df8e87de33022179276ec634400a"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-rss-dd7d0cc56e2d"
canonical_url: "https://medium.com/riskified-technology/how-we-made-business-logic-fast-to-change-and-boring-to-maintain-0fad281dad92"
published_at: "2026-06-30T12:08:47+00:00"
first_seen_at: "2026-07-20T23:18:31.853064+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:f1504891703cd6b95725ff1ae5d14d018a720eaec29e9daa89ed9eb14657de2f"
---

# How We Made Business Logic Fast to Change and Boring to Maintain

Press enter or click to view image in full size


Softeare Engineering


Software Architecture


Business Logic Flaw


Platform Engineering


Orchestration


# **How We Made Business Logic Fast to Change and Boring to Maintain**


## How trading compile-time safety for strict runtime validation helped us eliminate deployment bottlenecks and empower business teams.


[Moshe Berdugo](https://medium.com/@mosheberdugo?source=post_page---byline--0fad281dad92---------------------------------------)


7 min read


·


Jun 30, 2026


--


Our event-driven Chargeback Management System (CBMS) gets complicated fast. Chargebacks arrive through multiple gateways, each with its own quirks, and while the core lifecycle is similar, edge cases pile up quickly.


Most clients accept a standard flow, but some require highly customized behavior. Supporting those variations exposed a painful feedback loop: even a one-line rule change required PRs, deployments, testing, and production rollouts — making simple business adjustments slow, costly, and engineer-dependent.


This is the story of how we stopped rebuilding rule engines in every team, accepted some uncomfortable trade-offs, and built a centralized Decision Context platform that made business logic fast to change, safe to run, and boring to maintain.


## Single-Purpose Service Approach


The default solution for new requirements is often to build another single-purpose microservice, which does work. The temptation to build laser-focused decision services is strong because this approach offers perceived benefits: simplicity, blazing performance due to tailored data models, complete team autonomy, and quick wins.


However, at scale, this siloed approach brings hidden costs. The **Infrastructure Tax** requires constant rebuilding of complex, dedicated infrastructure (databases, validation, versioning, auditing, UIs), leading to high-maintenance systems. **Semantic Confusion** arises when teams use similar logic but develop conflicting definitions of core concepts (e.g., “priority”), complicating cross-domain debugging. Finally, **Knowledge Walls** silo expertise, preventing teams from leveraging shared knowledge and spotting common patterns.


We will explore the pros and cons of centralizing all global decision-making and the ultimate decision to build a dedicated platform.


## The Solution: A Centralized Decision Context Platform


Think of each decision as a journey. This framework gives it a specific *context* and sets up *decision nodes* - crucial crossroads on the path. At each crossroad, the system evaluates specific information and rules to plot the next step. Architecturally, the Decision Context framework is not just a bespoke tool for one team — it’s a **platform service** . It’s a complete, stand-alone app ready to serve the entire organization.


## The Decision-Making Hierarchy


This navigator organizes its map into three vital layers, like an inverted tree structure:


1. **The Decision Context:** This is the master container, defining the unique world (domain) of the decision- say, all decisions related to “choosing a document template”. It holds the ultimate goal ( *Decision Strategy* ): does the process stop as soon as it finds a winning rule ( *FirstWins* ), or does it gather all positive recommendations before concluding ( *AllWins* )? It also holds the fallback plan- a default outcome for a colossal failover.
2. **The Decision Group:** This layer acts as a filter, grouping rules by priority. It uses a key to decide if the rules are *Global* (apply to all customers) or *Account-designated* (specific to one merchant).
3. **The Decision Node (The Rule Itself):** These are the individual leaves on the tree. Each node has a defined *criterion* (“When to match?” - e.g., “Transaction amount > $10,000”) and a corresponding *outcome* (“What to return?” - e.g., “Document Template Id: ‘template-123’”).


Press enter or click to view image in full size


Every root node branches out into 1 or many children-nodes. When incoming data meets a rule’s criteria, the corresponding outcomes are instantly collected and sent to the next step, where they are applied to the final decision.


How we evaluate context outcome:


1. Sort the group decision by GroupsPriority order. By doing this, we ensure Riskified business rules (Global Prioritized) are the top boss. If they aren’t there, the master account rules kick in, and those override individual account definitions if they exist. If we’re missing crucial info, we follow a simple safety net: first, the merchant’s fallback; then the master account’s; and finally, the Riskified global safety net.
2. Perform a Depth-First Search (DFS) on the node tree for each group. The root node, defined by the group’s key and account ID, is structured like any other node, with the child node list determining the tree’s layout.
3. Evaluate node criteria against input data via the rule Engine (rule engine can be injected as a library into the framework)
4. Return outcome from matching leaf node (or parent’s fallback outcome)
5. Apply strategy: stop at first match (FirstWins) or continue (AllWins)
6. Fall back to rootNode’s defaultOutcome if no nested nodes match.


Riskified’s UI manages a complex, prioritized rule structure, showing both account-specific and global contexts. While authorized users can easily modify rule criteria, outcomes, and execution order, this accessibility poses a risk of product damage by inexperienced users, making robust validation essential.


## The Platformic Approach


Choosing a general-purpose platform, like the Decision Context framework, means accepting some more hassle for a potential payoff down the road. When we did it, the initial cost proved immediately substantial, while the return was always uncertain.


A platform is central. Its speed, reliability, and accuracy affect every using team — a failure is felt by all (operational coupling). This critical role creates **intense performance pressure** . We must ensure the platform is “right” for all, requiring extreme measures: much tougher testing (beyond regular services), plus extensive auditing and logging.


For the platform to become a real platform rather than just a trans-team shared service, we needed to **get everyone on board.** This meant lots of talking with different teams to agree on the core idea, the API, and the compromises we were all willing to make.


Our main challenge was finding the ideal “sweet spot,” which involved navigating constant dilemmas around ambiguous tradeoffs. A key question was whether our solution was **too abstract** : adopting generic modeling sacrifices valuable compile-time checks and necessitates a heavy reliance on runtime correctness. Moving from typed classes and DTOs to an abstract data structure (plain JSON with strict validation) meant losing the comfort of type-safety. We had to compensate. With validations. A lot of them.


## **The Great Trade-off: Why we killed type-safety to save the business**


We prioritized runtime adaptability over compile-time safety, accepting that frequently changing, non-engineer-modified decision logic makes compile-time guarantees impractical. This flexibility enabled immediate, system-wide changes without redeployment.


To mitigate risk, we enforced strict guardrails: JSON was strictly schema-validated on write; rule changes were auditable and deterministic; and failures defaulted to a safe state. We maintained hard constraints -rejecting untyped expressions, dynamic code, implicit defaults, and “best effort” parsing -ensuring invalid rules are synchronously rejected (fail-fast).


This traded compile-time certainty for tightly controlled runtime behavior, guaranteeing loud, early failures. Framework protections include **Tree Cycle detection** , **Criteria scheme validation** for rule structure, and **Managed Group Key Authorization** for internal key protection. All synchronous validations ensured every stored decision context was structurally sound, semantically correct, and authorized, providing instant user feedback


## The Payoff Is Finally Here


Yeah, it costs a bit upfront, but honestly, ditching those custom, siloed solutions for a proper platform service gives us some seriously sweet benefits-benefits we should have been enjoying ages ago:


**“So long, and thanks for all the fish”.** When *our* business analysts need to tweak a decision (like case routing or document picking), they use *one* consistent dashboard and mental model. That means *we* (the engineers) are no longer getting bugged every time some tiny rule changes. It instantly frees up a significant number of valuable story points in our quarterly dev capacity. They have now become self-sufficient.


**Unified, but *our* way:** The shared DecisionContextEvaluatorService library makes this bulletproof. Whether the request is for a dispute action or compelling evidence, the core evaluation logic-priority, deep dives (DFS), and strategy application are always the same. We fix a bug or make an improvement once, and *everyone* gets it instantly. Think of the time we save! If, for some reason, they don’t like our proposed evaluation flow? Fine, the evaluation is local on each platform caller. They can just replace *their* engine, but they still benefit from our infrastructure.


**Schema Changes are a complete non-issue:** Criteria and outcome fields are flexible (using Record<string,any>). *We* can roll out new criteria schemas or operators without consumer teams having to scramble, upgrade libraries, or deploy new code. It’s simple. We own the complexity, so they don’t have to.


**Safety is a Shared Investment:** We built expensive safety nets — like cycle detection via DFS, criteria schema validation, and rigorous authorization checks — once into the DecisionContextValidator. That means every single platform service is automatically protected from their mistakes. You’re welcome.


## **Platform Service: The Right Choice for Your Next Task?**


Often, the answer is no. That feeling of déjà vu -”I’ve done this,” or “This is just copy-paste”- may signal a need for refactoring, not a whole new system. Developing a new platform is a major undertaking, demanding deep engineering, trade-off consideration, organizational research, and high cost and effort. It should not be taken lightly. A platform is only worthwhile when:


- Multiple teams require the exact same functionality.
- Changes are frequent and urgent.
- Inconsistencies or duplication are causing real incidents or measurable monetary loss.


If the change serves only one team or domain, and platform-like usage is rare (e.g., once a quarter), stick to a domain-scoped solution.


For senior developers considering similar architectural patterns, the Decision Context framework offers several transferable lessons:


1. **Invest in Hierarchy** : Flat rule systems quickly become unmanageable. A hierarchical approach (contexts → groups → nodes) provides natural organization and enables sophisticated priority handling.
2. **Platform Over Library** : For cross-cutting concerns like decision management, a platform service provides stronger consistency guarantees and easier evolution than distributed libraries.
3. **Validate Aggressively** : When configuration can affect business outcomes, comprehensive validation isn’t optional. Cycle detection, schema validation, and authorization checks prevent entire categories of production incidents.
4. **Design for Multiple Strategies** : The FirstWins vs AllWin strategies demonstrate that evaluation semantics should be configurable, not hard-coded. Different use cases require different approaches.
5. **Embrace Explicit Priority** : Rather than implicit ordering based on insertion time or database IDs, explicit priority keys make behavior predictable and debuggable.
6. **Separate Criteria from Outcome** : The clean separation between “when does this rule match” (criteria) and “what should happen” (outcome) enables reuse and makes rules self-documenting.


The evolution from rigid, code-embedded rules to a unified Decision Context framework represents more than a technical improvement-it’s a fundamental shift in how the organization manages business logic, enabling agility without sacrificing safety.
