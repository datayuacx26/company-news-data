---
schema_version: "1.0.0"
document_id: "760cdd0927eafd572770308dba8cf08c13d3d15fee31e01f5c3a64ad3e7a54c4"
company_key: "yc-truewind"
company: "Truewind"
source_id: "yc-truewind-rss-014af42b96d1"
canonical_url: "https://www.truewind.ai/blog/truewind-visits-stanford-gsb-agentic-ai-in-accounting-field-notes"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.745562+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:9c5fbadf6926ddf52dfa72898951eba76a53e9448add4cd0063b52d5d968ce4f"
---

# Truewind Visits Stanford GSB: Agentic AI in Accounting Field Notes

On May 12, 2026, Tennison Chan and I joined Stanford Graduate School of Business's ACCT 535 course, Artificial Intelligence and Accounting Information, to talk about what we have learned building agentic AI for accounting teams. The class prompt was financial reasoning and AI behavior. Our message was simple: if you want AI in accounting to matter, stop treating it like chat.


Six years ago in San Francisco, I saw a driverless car navigate a busy intersection and had a thought I could not shake: we have autonomous vehicles, but we still cannot reliably automate bookkeeping. After three years building Truewind, the answer is clearer. Accounting is not just math. It is messy source data, tribal knowledge, judgment calls, recurring workflows, and auditability compressed into a monthly close.


That is why the most useful framing for AI in accounting is not a smarter chatbot. It is a system that can execute a workflow, explain its work, and survive review.


## The monthly close is a workflow, not a conversation


A finance team does not ask an accountant to chat about the books. It asks them to receive statements, categorize transactions, reconcile accounts, recompute balances, post journal entries, resolve exceptions, review workpapers, and sign off. Every month. The output is not prose. It is numbers that have to tie out.


Take a simple payment processor example. A customer pays $100. Stripe takes a $5 fee. The bank receives $95. One economic event now spans customer data, Stripe data, bank data, fee logic, revenue logic, and the company's chart of accounts. Multiply that by thousands of transactions and dozens of sources, and the problem stops looking like a question-answering task. It becomes a stateful, auditable workflow.


This is where agents become interesting. The right agentic system can extract source data from a PDF, classify the line items against a customer's chart of accounts, match counterparties, draft journal entries, validate that debits equal credits, tie totals back to the source statement, route ambiguous lines to a human, and preserve an audit trail. The agent is not valuable because it can produce a fluent answer. It is valuable because it can do the work in a way a finance team can inspect.


## The reasoning is the deliverable


One of our earliest lessons was humbling. We built systems that could compute a final number and hand the accountant a finished spreadsheet. We expected relief. Instead, accountants redid the work line by line, then compared our total to theirs.


At first that felt like mistrust of the model. It was actually a product insight. Accountants were not checking whether the arithmetic was hard. They were checking whether the answer could be defended. A right number that cannot be explained to a controller, CFO, or auditor is functionally the same as a wrong number.


So we changed the product philosophy from black box to glass box. Every cell should show its derivation. Every journal entry should link back to the source line that created it. Every assumption should be named. Every exception should be challengeable. In accounting, the number is the receipt. The reasoning is the deliverable.


That is also why SOPs matter so much. A standard operating procedure is a contract between the human and the agent. The human says: here is how we handle this workflow, including the edge cases. The agent says: I will follow that procedure, show my work, and flag the places where the procedure does not resolve the ambiguity. Over time, corrections become institutional memory instead of tribal knowledge trapped in someone's head.


## Reliability is a systems problem


The most dangerous AI failure modes in accounting do not look like failure. They look polished. They are well formatted, confident, and almost right.


In one broker reconciliation, we asked an agent to resolve balances that did not tie. It came back with a clean workbook. The totals matched. The schema looked valid. Then an independent review caught the issue: the agent had plugged a transaction to hit the target. When we updated the SOP to say no plugs, the next run found the nearest escape route. It used the one legitimate plug account in that workflow, unrealized gain/loss, to make the workbook tie.


That is the lesson. Agents can satisfy the letter of a rule while violating its spirit. You cannot patch your way to reliability one failure at a time. You need a stack.


At Truewind, that reliability stack looks like this:


- Frontier models for the fuzzy work: extraction, classification, counterparty matching, anomaly detection, and natural-language SOP authoring.
- Deterministic checks for the exact work: arithmetic, tie-outs, schemas, posting logic, permissions, and audit logs.
- Programmatic SOPs that constrain the order of operations and make decisions inspectable.
- Narrow, typed tools instead of open-ended instructions like "close the books."
- An eval harness that replays historical traces and catches regressions before they hit production.
- Humans on exceptions, where judgment and accountability belong.


Each layer catches what the layer below it cannot. The frontier model is important, but it is not the reliability strategy. The system around the model is the strategy.


## Width is the enemy of reliability


The biggest practical lever is narrowing the task. Do not ask an agent to close the books. Ask it to classify one Stripe payout line against one customer's chart of accounts, with a known input, a known output, a narrow tool surface, and a validation path.


Narrow tasks are easier to evaluate. They are easier to replay. They are easier to correct. They also force a clean separation between probabilistic work and deterministic work. Use the model where judgment, language, and fuzziness matter. Use code where there is one right answer. If a debit-credit check can be done in code, it should not be delegated to an LLM.


## Buy commodity. Build domain.


The same logic applies to product strategy. The wrong question is, "Should we build our own model?" The better question is, "Where does our lasting differentiation actually live?"


For most companies, it does not live in foundation models, inference infrastructure, embeddings, or generic agent plumbing. Those layers will keep getting better, cheaper, and easier to swap. Differentiation lives where the customer context lives: domain taxonomy, SOPs, workflow design, evals, audit trail, and trust.


That is especially true in accounting. Accounts payable became a large software category because many AP workflows have a similar shape. Accounts receivable, revenue recognition, close, FP&A, and tax are different. They are full of long-tail variation. Every company has its own contracts, policies, chart of accounts, thresholds, and edge cases. Traditional SaaS struggled with that variation because customization cost too much. Agents change the economics by making a custom workflow per customer more feasible, as long as the workflow remains auditable.


So our view is straightforward: buy the commodity layer and build the domain layer. Let the foundation labs compete on models. Let infrastructure vendors compete on plumbing. Build the system of record, the SOP library, the reliability harness, and the customer workflow that finance teams can trust.


## The future is not autonomous accountants


The future we see is not a world where accountants disappear. It is a world where accountants work alongside agents that handle repetitive, high-volume, well-specified work, while humans handle judgment, exceptions, relationships, and accountability.


That is a better job for the accountant and a better product for the customer. It is also a larger market than "AI that does taxes" or "AI that answers finance questions." The real opportunity is AI for the books that have to tie out.


For Truewind, that is the work: building agents that do not just generate accounting outputs, but produce work a CFO can sign and an auditor can trace. Workflows, not chat. Reliability, not vibes. Domain, not commodity.
