---
schema_version: "1.0.0"
document_id: "41bb6a027423a5536dcf6e4dce2aae38b35104cea13f68163f6d9dfe5abfef5f"
company_key: "yc-furtherai"
company: "FurtherAI"
source_id: "yc-furtherai-news-import-96169723635d"
canonical_url: "https://www.furtherai.com/blog/every-forward-deployed-engineer-just-got-promoted"
published_at: null
first_seen_at: "2026-08-17T22:27:20.987642+00:00"
fetched_at: "2026-08-17T22:27:22.805378+00:00"
content_hash: "sha256:cb200f87fba4646b3c3a725597dfba13c93724c4200aa21db27da18fc7a5208b"
---

# Every Forward Deployed Engineer Just Got Promoted

The Forward Deployed Engineer is the hottest job in enterprise AI right now. It is also the job AI is automating fastest.


Both things are happening simultaneously at FurtherAI. AI now builds the workflows our FDEs used to manually, and the role got more valuable as a result.


When we started FurtherAI, an FDE had to build almost everything around the AI.


For every new underwriting or claims workflow, they would understand the customer’s process, write the prompts, select the models, build the integrations, connect the steps, create an eval set and keep testing until the workflow was reliable.


This was the right way to get AI into production, but it had an obvious scaling problem. Every new customer and workflow required more implementation work. A company could grow quickly while quietly building a large services layer underneath the product.


## **AI can now build the workflow**


At FurtherAI, we started asking: if an AI agent can execute a workflow, why can’t another agent construct it?


Give it the intended outcome, the systems it can use and a representative eval set. The architect sets up the workflow, writes the prompts, selects the models, writes the integration code, connects the steps, runs the evals and improves the result.


The AI is no longer reasoning inside the workflow. It builds the workflow around itself.


The transition looks like this:


We are not alone in this. Palantir has a product literally called[AI FDE](https://www.palantir.com/docs/foundry/ai-fde/overview/) . It turns natural-language requests into Foundry operations, performs data transformations, manages code repositories and validates its own changes.


Parts of the FDE role are becoming software.


## **The FDE role is being unbundled**


Today, one FDE does all of it: understand the customer, design the workflow, write the code, run the evaluation, manage the deployment and identify what should become the product.


Those responsibilities are separating into two:


Layer Owner Responsibility


Workflow construction AI Architect Prompts, models, integrations, workflow assembly and testing


Product frontier Human FDE Build reusable capabilities the platform does not yet have


‍


OpenAI has split the role the same way, into customer FDEs, platform engineers and technical deployment leads.


The future FDE is no longer the human compiler between a customer request and a working workflow. They work at the edge of the product, find the problems the platform cannot solve yet and move that edge forward.


Writing the prompt or the integration code is now cheap. Defining what correct means is not. Someone still has to identify the right examples, weigh the cost of different errors and decide when a workflow is safe for production.


The eval set becomes the product specification. AI builds toward the outcome, but a human still owns whether the outcome is right.


This matters more in insurance than in most industries. A missed low-priority field is not the same as a missed exclusion or a wrong underwriting rule. One is an annoyance. The other is a claim you have to pay. An FDE here does not just decide if the workflow runs. They decide what an error costs, and that answer changes by line of business, by carrier and by state.


## **While reusable blocks compound, custom implementations don’t.**


If you have visited the FurtherAI office, you have probably noticed that we have a lot of Legos.


That is also how we think about the product.


The goal is not for the architect to write a completely different codebase for every customer. That would make bespoke development faster, but it would not give us product leverage. We would end up maintaining one hundred custom implementations instead of ten.


The architect should assemble reusable Lego blocks: connectors, workflow steps, insurance logic, evaluation components and agent capabilities.


Customer specificity does not go away. Two carriers can handle commercial property submissions with completely different referral thresholds, appetite rules and downstream systems. Today an FDE builds a separate implementation for each. With reusable blocks, both carriers run the same submission intake workflow, the same document extraction and the same evaluation framework, configured differently.


When a genuinely new capability is required, the FDE builds it into the core platform and it becomes another block. That is the part of the job that compounds. Every hard problem you solve for one insurer makes the next one faster.


## **What’s next for the FDE role?**


Automating FDE work does not mean we hire fewer FDEs. We are hiring more.


When deployment gets cheaper and faster, more customers say yes. Workflows that were never worth an implementation are now worth doing. Each FDE supports more customers and spends their time on harder problems.


We also cannot lose the deep domain context that came with manual implementation. Future FDEs still need to work directly with insurers to understand their most complex workflows, evaluate system performance, and investigate platform limitations firsthand.


We are hiring FDEs at FurtherAI, but not to hand-build workflows or write one-off implementations for every customer.


We are looking for engineers who want to work directly with world's largest insurers, find the hardest problems that still need a human, and build the product capabilities that remove them. You will spend your time on underwriting rules, claims logic and the parts of the business that nobody has automated yet. You will not spend it wiring up the same integration for the tenth time.


The work begins with one customer and makes FurtherAI better for the next hundred.


Every Forward Deployed Engineer Just Got Promoted. The repetitive implementation work disappears. What remains is customer judgment, accountability and product invention, and it is worth far more.


If that sounds like the kind of engineering you want to do,[DM me](https://www.linkedin.com/in/amangour/) . We would love to speak.
