---
schema_version: "1.0.0"
document_id: "24d1e986f6eddf768cbbff57bdc0f92ce8523babb925279492e8dee40c67cb7b"
company_key: "yc-shepherd"
company: "Shepherd"
source_id: "yc-shepherd-news-import-bb872756570a"
canonical_url: "https://shepherdinsurance.com/blog/the-evolution-of-shepherds-rules-engine"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-24T01:19:36.558879+00:00"
fetched_at: "2026-07-28T22:16:03.530620+00:00"
content_hash: "sha256:322f538feb1a32d41ebf515d4511c37e4b2ef705d6b14856ae1b9fb7bef87491"
---

# The Evolution of Shepherd’s Rules Engine

Last year, we[wrote](https://www.shepherdinsurance.com/blog/how-shepherd-accelerated-underwriting-with-a-smarter-form-management-system) about how Shepherd built a rules engine to automate form selection, one of the most complex and compliance-critical parts of underwriting. Selecting the right forms on a policy means evaluating jurisdictional requirements, regulatory conditions, exposure characteristics, and more. With thousands of forms across multiple insurance products, that process quickly became a bottleneck. Shepherd addressed it by building a rules engine that let our compliance team codify form logic directly in the underwriting platform, reaching roughly **90% accuracy** and significantly accelerating underwriting velocity.


But form selection was only the beginning. Today, Shepherd’s rules engine supports a growing set of underwriting and servicing workflows across the policy lifecycle, built on **thousands of rules** and over **200 condition fields** that help determine how policies are reviewed, structured, and documented. This blog post looks at how the system expanded from a **single workflow into a broader framework** for automating underwriting and servicing decisions.


#### Rules Engine Recap


At its core, the rules engine translates insurance business logic into deterministic rules that the platform can evaluate automatically, while allowing insurance experts to define and manage that logic themselves.


At runtime, rules are executed using[json-rules-engine](https://github.com/CacheControl/json-rules-engine) , an open-source engine that evaluates conditions against the underlying data (facts). A custom caching layer ensures facts are only fetched once per evaluation, reducing database load and improving performance. The result is a system where underwriting expertise can be encoded directly in rules and applied consistently across the platform at scale.


#### The Next Evolution: AI-Generated Rules with Nova


Even with a purpose-built GUI, creating rules still required users to understand the structure of the underlying rule system. To reduce that complexity, Shepherd recently introduced **natural language rule creation powered by**[Nova](https://www.shepherdinsurance.com/blog/nova-shepherds-ai-assistant) , Shepherd’s AI agent.


Users can now describe a rule in plain language, and Nova converts that description into a structured rule object that the engine can execute. Nova generates:


- the rule’s JSON structure
- a rule name
- a human-readable summary


To ensure accuracy and safety, Nova is constrained by the same rules framework that powers the engine. It only uses approved condition fields, valid operators, and allowed values when constructing rules. This allows insurance teams to describe intent in natural language while still producing deterministic, executable logic.


##### Making Rules Searchable


Nova is also beginning to leverage the rules themselves as a source of knowledge.


Previously, forms in Shepherd’s vector database were indexed primarily using form metadata and form content. This made it possible to search form text but did not provide insight into when forms applied. Now, rule summaries describing when **forms are required or rejected** are also indexed and stored. This enables Nova to answer questions like:


‍
*“Which forms apply if the project is in California”?* Or, simply


*“Why was this form added/not added to this policy?”*


By combining form content with the rules that govern them, Nova can provide clearer explanations of underwriting, compliance and servicing decisions.


#### Expanding Beyond Forms: Five Rule-Driven Workflows


The rules engine now powers a growing set of underwriting and servicing workflows across Shepherd’s platform. Today, it supports five primary surface areas:


1. **Forms**


Determining which policy forms and endorsements apply to a given policy. These rules are primarily defined by the compliance team, encoding regulatory and jurisdictional requirements that ensure policies are issued correctly.


1. **Subjectivities**


Requirements that must be satisfied before a policy can be bound. CX and underwriting teams can configure these rules so that required conditions automatically appear on quotes when specific exposures or circumstances are present.


1. **Referral Triggers**


Rules that detect when a deal falls outside underwriting guidelines and route it for additional review or approval.


1. **Auto Symbols**


Automatically determining applicable auto coverage symbols based on the characteristics of a risk.


1. **Document Templates**


Controlling which sections or language appear in underwriting or policy documents. Within our internal document builder, users can configure rules that dynamically determine which document components should be included.


Across these workflows, the rules engine allows the teams closest to each process to encode and maintain decision logic directly in the platform as requirements evolve.


#### Making Rules More Expressive


As Shepherd expands into new products and workflows, the rules engine must handle increasingly complex logic. That has required the system to support more expressive ways of defining rules.


##### Condition Groups


Originally, rules were evaluated using a single logical operation applied to a set of conditions. This worked well for straightforward scenarios but made it difficult to represent more complex logic. Condition groups introduced the ability to structure rules with nested logical groupings. For example:


This allows users to express more realistic underwriting logic without creating multiple separate rules.


##### Field to Field Comparisons


Another improvement allowed rules to compare dynamic fields against one another rather than only against fixed values. For example, now we can construct the following to compare two different deductible values on a quote:


This enables rules to reason about relationships between fields rather than relying solely on static thresholds.


##### Scoped Rules


As Shepherd has expanded into supporting Workers Compensation with more complex regulatory requirements, another limitation became apparent.


Many underwriting datasets are structured as tabular records – for example payroll data where each row represents a state and classification combination. Earlier versions of the rules engine evaluated conditions against flattened values extracted from those records. This allowed rules to confirm that certain values existed somewhere in the dataset, but not that they appeared together within the same record. Consider the question: *“Is there a payroll record where state = CA and NCCI class = 5183?”*


Before scoped rules, the engine could confirm that CA existed within payroll states and that 5183 existed within NCCI class codes, but it couldn’t verify that those values occurred together. Scoped rules allow the engine to evaluate conditions within the same item in an array, ensuring *all conditions apply to the same record* .


##### How we implemented them


We extended the json-rules-engine library with a` SOME` operator that instructs the engine to iterate through array items, and a` ScopedAlmanac` class that evaluates nested conditions within each item rather than across flattened data.


The` ScopedAlmanac` **intercepts fact lookups during iteration** , checking the current item's properties first before falling back to global facts. Nested fields use **relative** paths (e.g.,` $.state` ,` $.ncciCode` ) that resolve within each item's scope, while the parent field uses an absolute path to extract the array itself. The image below shows how we model these rules in our database using self-referential nested condition fields.


#### Conclusion and What Comes Next


What began as a system to automate form selection has grown into Shepherd’s rules engine for **** automating underwriting and servicing decisions – enabling the team to scale complex insurance operations without scaling manual review alongside them. We’re continuing to explore additional ways to make the engine more powerful and easier to operate:


##### 1. Smarter Rule Creation with Nova


Today, Nova converts natural language descriptions into structured rule objects. Over time, Nova will become more context-aware when generating rules. For example, Nova could incorporate **form content from Shepherd’s vector database** while generating rules. Instead of requiring users to craft precise prompts, Nova could reference the language of the forms themselves and infer relevant conditions automatically, improving rule accuracy.


##### 2. Bulk Rule Generation


As Shepherd introduces new products and regulatory requirements, large sets of rules often need to be created at once. Future capabilities could allow teams to generate rules in bulk – either from structured underwriting guidelines such as spreadsheets, or from a single natural language prompt that produces multiple rules at once. This would significantly accelerate onboarding new products and updating regulatory logic.


##### 3. Event-Driven Execution


Finally, the rules engine may expand toward **event-driven execution** . Rather than evaluating rules only when workflows explicitly trigger them, rules could respond to platform events – automatically generating tasks, triggering notifications, or initiating additional underwriting or servicing actions.


‍
