---
schema_version: "1.0.0"
document_id: "3e403374005974762dee5f8d586084360b59f06cac2e04510612ee1c46750d1c"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/how-jazzberry-eliminated-downtime-with-blaxel"
published_at: "2025-12-03T13:52:10+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T22:25:05.780563+00:00"
content_hash: "sha256:fcb43f592a761e04918981276d7b59294545c4ee1fd82f35d8869cdff92fbe25"
---

# How Jazzberry eliminated downtime with Blaxel

Jazzberry is building AI agents that can autonomously navigate and modify codebases to resolve engineering tasks. To do this, they require a secure and reliable environment to execute agent-generated code. This is a foundational challenge for any company building in the agentic AI space.


We spoke with Jazzberry's founder, Mateo Perez, to understand how they use Blaxel's infrastructure to power their product.


## The challenge of self-managed infrastructure


Jazzberry’s product relies on executing arbitrary user code within a secure environment. To accomplish this, their team initially built and managed his own infrastructure using Firecracker micro-VMs.


This self-managed approach presented significant challenges. Without prior experience in Firecracker, the Jazzberry team faced persistent infrastructure issues. Memory pressure from spinning up too many VMs would lead to crashes, producing unclear error messages that were time-consuming to debug. "Time was spent on managing infrastructure ourselves that could have been spent improving the product," Perez notes. After a critical downtime incident, the team decided to find a more reliable solution.


## A decoupled architecture for security


Jazzberry's core technical problem involves running agentic logic that can interact with a customer's source code. This requires executing arbitrary commands and file system operations in an isolated environment to ensure security.


To solve this, Jazzberry migrated to Blaxel and adopted a decoupled architecture using Blaxel Agents and Blaxel Sandboxes.


The main reasoning loop of their agent runs in a Blaxel Agent. When the agent needs to perform an action (such as running a bash command or reading a file) it makes a tool call to a Blaxel Sandbox. The sandbox executes the command in a completely isolated environment, returns the result to the agent, and is then discarded.


This separation ensures that the agent's execution logic is distinct from the environment where untrusted code is run.


## Results: Increased reliability and faster development


The most significant outcome of migrating to Blaxel was a dramatic increase in reliability. The memory pressure issues and sandbox crashes that led to downtime were eliminated.


"Having this handled by Blaxel did improve the reliability," Perez says, "and also let us treat everything as serverless from a development perspective, which made us move a lot faster."


By offloading infrastructure management, the development team regained valuable time that was previously spent on tedious debugging and manual interventions. The move to a serverless model accelerated their development speed and allowed them to stop managing infrastructure and focus entirely on their core product.


Perez also highlighted the responsiveness of our support. When an issue does arise, he can contact our team directly instead of spending hours debugging complex systems. "One of the most amazing parts of Blaxel is just how responsive their support is," Perez states. "Any problem you have will get escalated and handled very quickly. The support is second to none."


Interested on setting up sandboxed code execution for your AI? Try out[app.blaxel.ai](http://app.blaxel.ai/) today!
