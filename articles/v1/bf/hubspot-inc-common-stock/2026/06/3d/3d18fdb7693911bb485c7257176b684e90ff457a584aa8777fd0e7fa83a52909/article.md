---
schema_version: "1.0.0"
document_id: "3d18fdb7693911bb485c7257176b684e90ff457a584aa8777fd0e7fa83a52909"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-rss-7ea402701b4d"
canonical_url: "https://product.hubspot.com/blog/building-observable-access-control-at-scale-a-rule-engine-approach-to-jita"
published_at: "2026-06-30T18:30:42+00:00"
first_seen_at: "2026-07-20T03:32:21.515438+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:d680cc1f3e25e1f70b430c1f394ec371daa3e79e277f5d9d7fca697872caaac1"
---

# Building Observable Access Control at Scale: A Rule Engine Approach to JITA

## **The Problem**


At HubSpot, we have a fundamental tension built into our culture: we want to help customers succeed, which often means giving employees access to customer portals: Engineers to fix bugs, sales engineers assisting with account setup, or customer support troubleshooting issues during live support calls. But we also need to completely lock down customer data — because a breach or misuse doesn't just violate customer trust, it can expose sensitive business data, create legal liability, and cause reputational damage that's hard to recover from. Access must be granted only for legitimate, verifiable, and auditable reasons.


This isn't a small-scale challenge. We have roughly 10,000 employees who might need access, and we process over 5,500 Just-In-Time Access (JITA) requests every business day. Far too many for meaningful manual review of each one. Approximately 5,100 of those are approved, with the remaining requests denied because they don't meet our access criteria, such as lacking an associated ticket or falling outside a user's role scope. Each approval represents a moment where we're threading the needle between security and productivity, and the observability built into our system gives us confidence that the right calls are being made.


For years, we managed this through what had started as a simple access control system. It began with basic checks: Does this user have the right role? Are they assigned to the right team? But as our needs evolved, the system grew organically. We added another if-statement for the support team's workflow. Then another for engineers debugging production issues. Then special cases for managers. Then exceptions to the exceptions.


What we ended up with was several hundred lines of nested conditionals scattered throughout our codebase. It worked—technically—but it had become completely unsustainable. When someone asked "Why did Alice get access to this customer portal last Tuesday?" we couldn't answer without reading through the entire codebase and mentally tracing execution paths. There was no logging of which condition granted access, no metrics on execution time, no way to understand the system's behavior except by reading the code itself.


Adding a new rule meant finding the right place in the nested if-statements to insert your logic, which was both error-prone and time-consuming. We had no visibility into whether existing rules were still relevant or if they'd become security holes as our organization changed. The system had become a large black box, and black boxes are the enemy of security.


We needed a rewrite. But more importantly, we needed a fundamental shift in how we thought about access control, not just as a technical problem, but as an ongoing governance challenge. The question wasn't just 'does this work?' It was 'can we explain every decision this system makes, to anyone, at any time?' Because when you're handling 5,500 access requests a day touching real customer data, functionality without observability isn't enough.


****


**Figure 1: Architectural evolution from monolithic conditional logic to parallel rule evaluation**


---


## **Design Principles**


Before writing a single line of code, we established four principles that would guide every architectural decision:


**Observability First**


Traditional access control systems are built around a single question: "Should this user get access?" We realized we needed to answer several questions simultaneously: Should they get access? Why or why not? How long did it take to make that decision? Which specific rule granted access? What data did that rule consider?


Every rule would need to log its execution time, pass/fail status, and relevant metadata. Without this, diagnosing a slow access request means guessing which of 40+ rules is the bottleneck, and answering 'why did this person get access?' means manually tracing through code rather than querying a log. This wasn't a nice-to-have feature we'd add later, it was the foundation everything else would build on.


**Fail Gracefully**


We learned this lesson the hard way. In our old system, a single failing check—like an internal API going down—would break the entire access control flow. Users would be unable to get JITA even when other rules should have granted them access.


In the new system, one broken rule couldn't take down the entire system. If a rule threw an exception, we'd log the error and move on to evaluate the next rule. Since access is only granted when a rule explicitly passes, this doesn't introduce security risk—a failing rule simply doesn't grant access.


**Data-Driven Governance**


Most JITA systems accumulate rules like technical debt. Everyone's afraid to delete anything because "what if someone needs it?" We wanted the opposite: a system designed from day one to support regular review and cleanup.


This meant building not just for execution, but for analysis. We needed to track how often each rule granted access, to whom, and under what circumstances. We needed to make recertification not just possible, but straightforward.


**Developer Experience**


Adding a new rule should be safe, fast, and require minimal context about the existing system. In practice, our security team owns and implements all rule changes, but the requirements come from across HubSpot. Support leadership might say 'we need access when we're in an active live chat with a customer,' and it's our job to translate that into a rule that's both implementable and secure. The system should guide us toward correct implementations and catch common mistakes early, so we can move quickly without introducing risk."


These principles weren't just philosophical—they directly shaped our technical decisions. Observability required structured logging and metrics. Graceful failure required sandboxed evaluation. Governance required treating rules as first-class, inspectable entities. Developer experience required a clean interface and shared infrastructure.


---


## **The Rule Engine Architecture**


The core of our new system is deceptively simple: a rule engine that evaluates independent checks in parallel, with each rule reporting structured results. But the devil—and the value—is in the details.


**The Rule Interface**


Every access check implements a common interface that enforces our design principles:


```text


```


This interface does several things at once. The


setConfig()


method allows us to inject runtime context—user data, team memberships, ticket information—that the rule will operate on. The


shouldEvaluate()


method lets rules short-circuit if they're not relevant to the current request. And crucially,


setReason()


and


getReason()


enforce that every rule must explain why it passed or failed, creating the audit trail we need.


**Config-Based Evaluation**


One of our key optimizations was realizing that many rules needed the same data. In the old system, we might fetch a user's team membership five times in a single access check. Now, we fetch all necessary data once, populate an immutable request context object with everything rules might need, and pass that snapshot to every rule.


This has multiple benefits. It's faster—we're not making redundant API calls. It's more consistent—all rules operate on the same snapshot of data, eliminating race conditions. And it's easier to test—we can construct a config object with known values and verify rule behavior without hitting real APIs.


The config typically includes user properties, team associations, ticket data, and any other context rules might need. Rules can also make additional API calls if they need specialized data, but the shared config handles the common cases.


**DAG Structure and Branching Logic**


Rules are organized in a Directed Acyclic Graph (DAG) that explicitly models the flow of evaluation. This isn't just an implementation detail—we visualize this DAG and publish it internally so that anyone can see how access decisions are made without reading code.


****


**Figure 2: Simplified DAG showing role-based branching and parallel rule evaluation. Rules are evaluated with OR logic—any passing rule grants access (as long as its not preceded by a Deny). Actual production DAG contains 40+ rules with realistic rule names redacted for security.**


The DAG supports two types of rules:


**SINGLE_EDGE_RULE** : These are standard rules that, after evaluation, proceed to whatever rule comes next in the graph. Most rules are this type.


**MULTI_EDGE_RULE** : These are branching points in the DAG. For example, we have a rule early in the graph that checks: "Is this user an engineer or a support rep?" If they're an engineer, evaluation proceeds down one branch of engineer-specific rules. If they're a support rep, evaluation follows a different branch of support-specific rules.


This branching is critical for maintainability. Rather than having every rule start with "if user is an engineer AND ...", we encode role-based logic in the DAG structure itself. When you look at the visualization, you can immediately see that certain rules only apply to certain roles.


The DAG structure also enforces that we can't create circular dependencies. If Rule A depends on Rule B, and Rule B depends on Rule C, we can't accidentally make Rule C depend on Rule A. The graph itself prevents these mistakes.


**DENY Rules and Emergency Overrides**


Most rules are PASS rules: if they succeed, they grant access. But we also have DENY rules that, if they match, prevent access regardless of what other rules say. These typically encode our hard security boundaries—checks that should never be bypassed under normal circumstances.


DENY rules generally appear first in the DAG, though we have a small number of emergency PASS rules for critical teams that can override even a DENY. This escape hatch has extremely limited membership and is audited carefully, but it's necessary for situations where a production incident requires immediate access and the standard approval flow would be too slow.


**Sandboxed Execution**


Each rule evaluation is wrapped in a try-catch block. If a rule throws an exception—maybe an API it depends on is down, or there's a bug in the rule logic—we log the error with full context but continue evaluating other rules.


This is safe because of how we handle rule results. A rule only grants access if it explicitly returns a PASS result. An exception means the rule didn't pass, so we continue evaluation. This ensures that one broken rule can't prevent other, healthy rules from granting legitimate access.


We log every exception with the rule name, the exception details, and the context that triggered it. This gives us immediate visibility when rules start failing, often before users even notice an issue.


**Parallel Evaluation**


Within each branch of the DAG, rules that don't depend on each other can be evaluated in parallel. In practice, this means evaluation flows through the DAG branch by branch: we evaluate all rules up to a branching point, then follow the appropriate branch based on the branching logic, and continue parallel evaluation within that branch.


This means the evaluation time for each section of the DAG is MAX(individual rule times in that section), not SUM. The total time is the sum of the max times across each sequential stage of the DAG, but within each stage, parallelization provides significant performance benefits.


More importantly, parallel evaluation combined with per-rule timing metrics immediately reveals bottlenecks. We don't need to guess which rule is slow—we can see it directly in our logs and dashboards.


## **What Observability Unlocked**


Building an observable system wasn't just about satisfying our design principles—it fundamentally changed what problems we could solve and how quickly we could solve them.


**Finding the Bottleneck**


With 40+ rules evaluating in parallel, we started logging the execution time of each rule independently. Even though the total JITA evaluation time was MAX(rule times) rather than SUM, we could immediately see the distribution of rule performance.


One rule stood out dramatically. While most rules completed in under 100 milliseconds, this particular rule was consistently taking multiple seconds, literally orders of magnitude slower than everything else. For a support rep trying to pull up a customer portal while live on a call, that delay is the difference between a smooth interaction and an awkward silence while everyone waits. The rule was making an API call that returned far more data than necessary and then filtering the results in memory. Without per-rule observability, we might never have known — users would have been suffering through slow access requests and we'd have had no idea where to even start looking.


We optimized that specific rule by pushing the filtering logic to the API layer, and immediately saw the impact in our metrics. What had been a multi-second check became a sub-second check. Total JITA evaluation time dropped proportionally.


This pattern has repeated multiple times. When performance degrades, we don't spend hours instrumenting and guessing. We look at the per-rule metrics, identify the slow rule, and fix it.


****


**Figure 3: Per-rule execution times revealing a multi-second bottleneck (orders of magnitude slower than other rules)**


**Audit Trails with Context**


The old system could tell you that Alice got access to a customer portal at 2:47 PM. The new system can tell you


*why* : Alice got access because she's assigned to an active support ticket for that customer, filed within the last 24 hours. Or that Bob got access because he's on the Customer Success team responsible for that account's region and has an active engagement. Or that Carlos got access because he's an engineer with an approved change request affecting that customer's portal.


This context is built into the rule results. When a rule grants access, it doesn't just return "PASS"—it returns "PASS" with metadata explaining the specific reason and relevant identifiers like ticket numbers or team associations.


**Audit Log Entry Example**


Below is an actual audit log entry (with identifying details redacted) showing what gets recorded when access is granted:


Every field serves a purpose. The


approval_attribution


tells us which rule granted access. The


approval_attribution_reason


provides the human-readable explanation with concrete identifiers—in this case, the specific helpdesk ticket number and portal ID that justified the access. The


suspect_behavior


field enables anomaly detection. The


department


and


role_id


provide organizational context for access pattern analysis.


This has been transformative for security reviews and compliance audits. When someone asks 'Why did this person have access?', we can provide a specific, programmatically-generated answer with an auditable paper trail in seconds rather than manually tracing through code and logs. In the rare event of a security review or incident, we can reconstruct not just who accessed what, but the complete chain of reasoning that granted that access.


The metadata also enables aggregate analysis that was previously impossible: "How many people got access via the emergency override rule this month?" or "What percentage of JITA requests are granted because of open tickets versus team membership versus manager approval?"


**Visualization for Non-Technical Stakeholders**


Perhaps most surprisingly, the DAG visualization has become critical for communication with non-engineering teams. Product managers, support leadership, and security stakeholders can look at the visual representation of the access control flow and understand how decisions are made.


We made a deliberate choice to use verbose, descriptive rule names like


PassIfHubspotterIsCustomerSuccessAgentOfPortalRule


rather than cryptic abbreviations. In the DAG visualization, these names make the logic intuitive—you can read the graph and immediately understand "Oh, if I'm a Customer Success agent assigned to this portal, this rule will grant me access."


We don't need to explain the code. We don't need to translate technical concepts. The DAG shows: "If you're in this role, these checks apply. If this check passes, you get access for this reason." It's made access control policy something that can be discussed and refined by the entire organization, not just engineers.


---


## **Migration Strategy**


Rewriting a system that processes 5,000+ access requests per day is terrifying, like swapping out the engine of a plane mid-flight. A bug in access control could mean either locking out legitimate users (breaking their ability to help customers) or granting unauthorized access (a security incident). We needed absolute confidence before cutting over.


**Parallel Validation**


Our approach was to run both the old system and the new system simultaneously for every JITA request, compare their results, and investigate any discrepancies. For an entire month, every access request triggered evaluation in both systems, but only the old system's result was used to make the actual access decision.


The first two weeks were humbling. We found discrepancies constantly. Sometimes the new system was more permissive than it should have been—we'd missed an edge case in translating the old logic. Sometimes it was more restrictive—we'd been overly conservative in our interpretation of a rule. Each discrepancy got logged with full context: the user, the portal they were trying to access, what the old system returned, what the new system returned, and why.


We'd fix the issue, deploy the update, and continue monitoring. Gradually, the discrepancy rate dropped. After two weeks of intensive fixes, we entered the validation phase: two more weeks where we expected—and achieved—100% parity between the systems.


****


**Figure 4: Discrepancy rate during parallel validation, reaching 100% parity before cutover**


**The Cutover**


With confidence from the parallel validation, we implemented the cutover as a simple configuration flag. One value controlled which system made the actual access decision. If anything went wrong, we could flip it back within seconds.


We deployed on a Tuesday morning—deliberately choosing a high-traffic time so we'd know immediately if there were issues. We watched the metrics carefully: access request volume, approval rates, error rates, latency. Everything looked normal.


By the end of the day, we were confident. By the end of the week, we were certain. After a month of the new system running in production with zero incidents and zero rollbacks, we removed and archived the legacy code entirely.


**Zero Downtime**


The parallel validation approach gave us something invaluable: we maintained 100% uptime throughout the migration. Users never experienced any disruption. From their perspective, JITA worked exactly the same way it always had—which was exactly the point.


The investment in parallel validation was significant—it extended our timeline by a month and required additional infrastructure to run both systems simultaneously. But it eliminated the risk that typically makes these rewrites so dangerous. We weren't gambling on a big-bang deployment. We were methodically validating that the new system was equivalent to the old before trusting it with production traffic.


---


## **Annual Recertification**


Most access control systems accumulate rules like barnacles on a ship. New rules get added when someone needs access, but old rules never get removed because no one's quite sure if they're still needed. Over time, the system becomes increasingly permissive as rules pile up, each one a potential security hole.


We designed our system to resist this entropy through annual recertification—a formal process where we review every rule, examine how it's being used, and make deliberate decisions about whether to keep, modify, or eliminate it.


**The Process**


On a regular cadence, we convene a panel of about 10 people representing different disciplines: security engineers, product managers, support leadership, and our Chief Security Officer. We review each of the 40+ rules individually in an in-person meeting that typically takes several hours.


For each rule, we present data that was impossible to gather in the old system:


- How many times this rule granted access over the past year


- How many unique users benefited from this rule


- The distribution of those users across teams and roles


- Execution time statistics (is this rule a performance bottleneck?)


- Any security incidents or near-misses involving this rule


****


**Figure 5: Recertification dashboard showing usage metrics and execution statistics per rule**


The panel discusses each rule: Is this access pattern still appropriate? Has the organization changed in ways that make this rule too permissive? Is there a way to make this rule more specific or secure?


**First Recertification Results**


Our first recertification, completed recently, yielded significant changes:


**Three rules were too permissive.** They granted access more broadly than they should have, often because they were written when our organization was smaller and less structured. We replaced these with more restrictive rules that required additional verification or limited the scope to smaller, more specific teams.


**Two rules were eliminated entirely.** The usage data showed they were rarely used—and when they were used, there were other rules that could have granted the same access through more secure paths. These rules were legacy artifacts that no longer served a purpose.


**Proactive Impact Analysis**


Before implementing these changes, we ran "hypothetical metrics"—we analyzed what would happen if we removed or modified each rule. For the rules we planned to eliminate, we identified exactly who would be affected: "If we remove this rule, 23 people will lose automatic access and will need manager approval instead."


We broke this down by team and by manager, showing the daily impact: "Your team currently gets automatic JITA approval 12 times per day via this rule. After the change, those will require your manual approval, or we can work with you to establish a more specific automated check."


This proactive communication was critical. Managers could see the concrete impact on their teams and had time to prepare or work with us to find alternatives. For some teams, we discovered legitimate use cases we'd overlooked and worked with them to establish new, more secure associations that could be programmatically verified.


**The Reaction**


There was pushback, particularly from support teams who use JITA most heavily. Support representatives don't always have technical insights into why they need access—they just know they need it to do their job. These conversations required patience and collaboration.


But because we came with data—"here's how often you're using this access, here's why it's a security concern, and here's how we can accommodate your workflow more securely"—we could have productive discussions rather than simply imposing restrictions.


For some teams, we did remove access entirely. But because we'd done the analysis and communication properly, it was understood and accepted.


**Compounding Security Hygiene**


The real value of recertification isn't just the immediate changes, it's the cultural shift. Rules are no longer permanent fixtures. They're implementations of access policies that need to be justified annually with real usage data. This creates an incentive to keep rules tight and specific, because overly broad rules will face scrutiny in recertification.


To clarify the relationship: a


**policy** is the organizational decision about who should have access under what circumstances (e.g., "Customer Support agents should have access to portals for customers they're actively helping"). A


**rule** is the code implementation that enforces that policy (e.g., \`PassIfPortalIsAssociatedWithOpenHelpdeskTicketAssignedToHubspotterRule\`). The rule engine evaluates rules; recertification reviews whether those rules still correctly represent our intended policies—and whether those policies themselves are still appropriate.


Each year, this process compounds. The system doesn't just stay secure—it gets more secure over time as we identify and eliminate unnecessary access paths.


---


## **Results & Lessons Learned**


Two years after deployment, we maintain 40+ rules and confidently add 1-2 per month. When issues arise, we debug them in minutes instead of hours. And critically, our recertification process means the system is getting more secure over time, not more permissive.


**Key Lessons**


**Observability enables everything else.** We could have built a rule engine without extensive logging and metrics, but it wouldn't have unlocked the debugging, recertification, or performance optimization we've achieved. Building observability in from the start—not as an afterthought—was the most important decision we made.


**Parallel validation eliminates migration risk.** Running both systems simultaneously for a month felt expensive. In retrospect, it was the fastest path to a successful migration. The time invested in validation meant zero access-related incidents since cutover.


**Recertification turns technology into practice.** The rule engine is impressive from an engineering perspective, but annual recertification is what makes it genuinely better security. Technology alone doesn't prevent drift—data-driven governance does.


**Visualization drives organizational buy-in.** Verbose rule names like


PassIfHubspotterIsCustomerSuccessAgentOfPortalRule


in the DAG made access control policy discussable across the organization, not just an engineering problem.


**Developer experience compounds.** Making it easy to add rules means we actually add them when needed. Making rules observable means we maintain them properly. Good DX creates systems people want to improve.


**Graceful failure is a security feature.** Sandboxed evaluation means internal API outages don't cascade into JITA outages. Building resilience from the start has made the system dramatically more reliable.


---


## **What's Next**


The rule engine has opened up possibilities that weren't feasible in the old system.


**Anomaly Detection**


With per-rule execution logs and metadata, we now have the data foundation to implement anomaly detection. We could flag unusual JITA patterns—a user suddenly requesting access far more frequently than their historical baseline, or accessing portals outside their normal scope. We could identify cases where users self-assign tickets immediately before requesting JITA, a potential indicator of policy circumvention.


These capabilities are hypothetical today, but the infrastructure to support them already exists. The barrier isn't technical—it's deciding which signals matter and how to act on them without creating alert fatigue.


**Expanding the Pattern**


The rule engine pattern has already expanded beyond JITA at HubSpot. Other teams have adopted similar architectures for adjacent access control problems—validating approved requests and determining which requests should pass through internal gateways.


The pattern—observable, governable, maintainable—turns out to be broadly applicable. Any system that makes automated decisions based on complex criteria benefits from treating those decisions as discrete, inspectable rules rather than tangled conditional logic.


These implementations have validated our core insight: the hard part of access control isn't writing the initial logic, it's maintaining it over time as your organization evolves. Building for observability and governance from the start makes that evolution manageable.


**Continuous Improvement**


The recertification process will evolve as we learn what works. We're considering more frequent lightweight reviews for high-risk rules, and exploring ways to automate some of the analysis we currently do manually.


The goal isn't perfection, it's continuous improvement. Every year, the system should be a little more secure, a little more maintainable, and a little better at serving both our users and our security requirements.


If there's one thing this journey taught us, it's that security isn't a problem you solve once. It's a practice you maintain. The complexity of access control at scale doesn't go away, but with the right observability, it becomes manageable, auditable, and something you can actually reason about. A system you can see clearly is a system you can improve confidently.


We're still learning. The anomaly detection we want to build, the recertification process we're still refining, the rules we'll add and remove next year — none of it is finished. But that's the point. If this kind of challenge interests you, building systems that balance security with scale, and that get better over time rather than just bigger, we'd love to hear from you.[We're hiring.](https://www.hubspot.com/careers/jobs?hubs_signup-cta=careers-nav-cta&page=1)
