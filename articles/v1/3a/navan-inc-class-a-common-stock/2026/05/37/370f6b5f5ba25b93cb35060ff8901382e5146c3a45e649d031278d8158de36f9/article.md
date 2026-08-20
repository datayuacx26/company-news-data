---
schema_version: "1.0.0"
document_id: "370f6b5f5ba25b93cb35060ff8901382e5146c3a45e649d031278d8158de36f9"
company_key: "navan-inc-class-a-common-stock"
company: "Navan Inc."
source_id: "navan-inc-class-a-common-stock-news-import-0382cbfb66e0"
canonical_url: "https://navan.com/blog/ai-expense-categorization"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-22T05:23:37.972655+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:62c4bee2510a2acb88188a7c11e8ea72c3231c0c7dff0b50c5ec462814c733fe"
---

# How We Taught an AI to Code Expenses like an Accountant (So You Don’t Have To)

Nobody gets into accounting to manually code transactions. Yet, every business expense needs an expense type like meals and entertainment, software, travel, or office supplies before it can be reconciled and synced to an ERP system.


At scale, getting this right matters: Miscoded expenses cascade into incorrect general ledger (GL) entries, failed audits, and finance teams burning hours on corrections instead of analysis. For years, the industry standard was either "let the employee guess" or "apply a static rule based on merchant category codes." Neither works well. Employees don't know (or care) about accounting taxonomies. Merchant category codes (MCC) are coarse — a hotel restaurant and a client dinner both map to food and beverage, but they might belong to completely different expense types under a company's policy. At Navan, we replaced our rules-based categorization system with an LLM-powered agent that reasons across rich transaction contexts to select the right expense type automatically.


Below, we’ll cover how we got the AI to correctly code 90% of transactions and what we learned.


## Death by a Thousand Miscodes


Our rules engine handled the common cases fine. A charge at United Airlines? That's airfare. A Hilton charge? Lodging. It struggled with the more nuanced charges. A $47 charge at a hotel gift shop would be coded as lodging. A WeWork charge could be office supplies, coworking, or rent depending on the company's chart of accounts. Conference registration fees, rideshares to the airport, parking at a hotel, each of these lived in a gray zone where static rules either guessed wrong or punted to the employee. We set out to rebuild the system to learn from user behavior and apply the unique context we have on every trip and transaction.


## Engineering the Context Window


Rather than asking the model a generic question like, "What expense type is this?," we assemble as rich a context window as possible for every transaction. Our model reasons across critical travel and expense data:


**Signal**


**What it includes**


**Why it matters / How we use it**


The company’s expense types


Custom names and descriptions for expense categories


A company that calls it “Client Entertainment” vs. “Meals — Business Development” is telling us how they want transactions categorized. We filter the available policy types to only those relevant for the user’s policy template and transaction amount, so the model isn’t choosing from an overwhelming list of irrelevant options.


The transaction itself


Merchant name, MCC category, city, country, and amount with currency


This core information is always present and forms the base context for classification.


Receipt data


OCR‑extracted text from uploaded receipts


A restaurant receipt that shows “2 guests” tells us something different than a solo meal. This data is available when the user has uploaded a receipt and is often the trigger for a re‑evaluation.


Trip indicator


A boolean flag indicating whether the user is currently on a business trip


This single signal resolves a surprising amount of ambiguity: a meal in another city during an active trip is likely a travel meal, not a team lunch.


Calendar context


Meeting name, location, time, participant count, and attendee details


A $200 dinner the same evening as a “Client Dinner — Acme Corp” event strongly suggests entertainment, not a personal meal. This is available when the user’s calendar is connected and relevant events exist.


Participant count


Number of participants involved in the transaction


This helps distinguish, for example, a solo meal from a team lunch or client dinner.


User profile data


Region, department, subsidiary, and HR attributes


Expense policies often vary by role or location, so we use this to apply the right rules and interpretations.


Custom field values


Company‑specific metadata already attached to the transaction


These fields provide additional classification signals tailored to each company’s setup.


L3 card data


Line‑item detail from the card network (what was purchased, not just where)


This reveals exactly what was bought, when available, and improves classification. Availability varies by merchant and card type.


The model is designed to reason whether it has two signals or all of them. A transaction evaluated at creation might only have the merchant and amount; the same transaction re-evaluated after a receipt upload will have substantially more to work with.


We deliberately excluded historical merchant patterns, or how a merchant is usually coded. Early tests showed this introduced a conformity bias. The whole point of the AI agnet is for it to reason about the specific context of this transaction — not to rubber-stamp what happened last time. The model already has the merchant category and name, which is sufficient for merchant-level signal without anchoring it to historical defaults.


We also deliberately avoid overburdening the prompt with complex classification rules. The model already has strong inferential capability, so giving it the transaction context, providing the available expense types with descriptions, and letting it reason is more effective than trying to encode every edge case as an explicit rule.


## The Agent in Action


To see how these signals work in practice, we can look at the "reasoning" the agent generates when evaluating a transaction.


### Solving Merchant Ambiguity


Sometimes the merchant name is a distraction. In this case, McDonald's was the merchant, but the agent identified the true nature of the expense:


```text
"reasoning": "The receipt is for 'Registration' for 'McDonald's Worldwide 2026', which is a professional event/conference. Despite the merchant name being 'McDonald's', the OCR clearly identifies this as a conference registration fee, not a meal."
```


### Leveraging Calendar and Participants


Here, the calendar and participant data helped the system distinguish between a personal meal and a team event:


```text
"reasoning": "The receipt shows a large order of multiple food items (multiple quantities of Manchurian, rice, chicken, etc.) consistent with a group meal. The calendar event 'Team Vignesh – Quarterly Engagement' confirms this was an internal team event with 8 participants. As the user is not on a business trip, TEAM_EVENTS_AND_MEALS is the correct policy."
```


### Understanding Specific Items


The agent understands exactly what was bought, not just where it was purchased from, and categorizes the expense type accordingly:


```text
"reasoning": "The receipt confirms the purchase of an 'Apple Magic Keyboard', which is a piece of ergonomic computer hardware. This falls directly under the 'IT/Ergonomic Equipment' custom policy."
```


## Triggering Re-evaluation During the Transaction Lifecycle


The agent’s suggestions trigger at several points in the transaction lifecycle.


**When it runs**


**What triggers it**


**How it behaves / why it matters**


At expense creation


A user creates a new expense with a receipt.


The agent evaluates immediately and applies its suggestion.


After receipt upload to an existing transaction


A receipt is attached to a credit card charge that’s already been created.


We introduce a deliberate delay before the agent evaluates. If a user uploads a receipt and immediately changes the expense type, we don’t want the agent to override them seconds later.


After description changes


A user adds or updates the description on an existing transaction.


The description often contains contextual details (e.g., “team dinner” or “client meeting”) that can resolve ambiguity the model couldn’t handle with a merchant name alone. The same delay and safe‑to‑repeat safeguards apply.


After L3 data arrives


Line‑item detail settles from the card network or is processed from an external transaction.


Detailed data tells the model what was purchased, not just where — turning a generic office supply store charge into a clearly identifiable equipment purchase, for example. The agent reevaluates when this data becomes available.


### Respecting Human Judgment


Two guardrails keep the system from being disruptive.


#### 1. We never override a human.


Before applying a suggestion to an existing transaction, we check whether the user or admin has manually changed the expense type. If they have, the agent backs off and asks why the human changed the expense type, even if it disagrees. Human judgment is ground truth.


2. We stop at the ERP boundary.


Once a transaction has been synced to the company's accounting system, we don't touch it. Changes after sync would create reconciliation nightmares.


### Handling Hallucinations


Even with a well-scoped prompt, from time to time, the model hallucinates an invalid policy type. Rather than discarding the result, a second lightweight LLM call is made to map the invalid output to the closest valid match. By leveraging the primary model's original reasoning and applying explicit fallback chains for ambiguous categories, this step corrects misspelled enums or disabled policy types. Ultimately, it serves as a cheap recovery mechanism that salvages otherwise-wasted classifications.


### Model Selection


We tested multiple models through hundreds of Braintrust experiments, evaluating accuracy, latency, and cost. Gemini 3.1 Flash-Lite gave us the best tradeoff — fast enough for inline expense creation flows, accurate enough to beat the rules engine by a wide margin, and cost-effective at our transaction volumes. Larger models gave marginal accuracy gains that didn't justify the latency hit for a feature that needs to feel instant.


## Measuring Accuracy in the Real World


To trust an AI-powered expense classifier in production, we needed a rigorous way to measure whether it was actually better than the rules engine it replaced. That meant defining what “right” even means for an expense type, assembling high-quality labeled data, and building a scoring system that could handle multiple reasonable answers instead of treating everything as a simple right-or-wrong classification.


### Establishing a Baseline


Before we could measure improvement, we needed to understand how bad the current state was. We started by analyzing how often admins and users overrode the system-assigned policy type, a proxy for if the rules engine got it wrong.


But this number alone is misleading. An override rate tells you the system is wrong; it doesn't tell you what "right" looks like.


### Building a Golden Dataset


This turned out to be harder than expected. Our first instinct was to use admin-overridden transactions as labeled data — the admin's choice must be correct, right? Not always. We found that even overridden transactions weren't always coded to the most fitting expense type. Sometimes admins picked "close enough" under time pressure, or applied a convention that had since changed.


We needed rigorous human verification. Early in the project, we invested in manually reviewing and labeling a golden dataset with correct expense types — verified by people who understood the companies' policies. This was slow, but it gave us a reliable foundation for all subsequent evaluation.


### The Scoring Problem


With labeled data in hand, we needed a scorer, and this is where expense type classification gets interesting. Unlike binary classification, expense types often have multiple defensible answers.


Is a hardware purchase "Tools and Materials" or "Office Supplies"? Is a client dinner "Meals" or "Entertaining Clients"? Is an Uber to the airport "Taxi and Rideshare" or "Ground Transportation"?


A naive exact-match scorer would penalize the model for answers that any reasonable human would accept. So, we built a multi-dimensional scorer that tracks:


- **Exact match** : Did the model pick the precise expected type?
- **Category match:** Even if not exact, is the prediction in the same semantic category? (For example, "Taxi and Rideshare" and "Ground Transportation" are both in the Transportation category)
- **Traveling variant match:** Many expense types have a "traveling" counterpart (Meals vs. Traveling Meals). If the model picks one of these and is wrong because of lacking trip information, it has still done a reasonable job.
- **Top-3 match:** Is the expected answer in the model's top three candidates?
- **Reciprocal rank:** Where does the correct answer fall in the ranked candidate list?
- **Weighted composite:** A single score that gives full credit for exact match, gives partial credit for traveling variants and category matches, and traces the degradation curve.


We also experimented with an LLM-as-judge scorer, and had a model evaluate whether another model's prediction was reasonable given the context. This was useful for catching cases where our category mappings were too rigid and for validating edge cases where multiple answers are genuinely correct.


### Iterating Through Experiments


With a reliable dataset and scorer in place, it became a matter of running hundreds of Braintrust experiments, varying the input context (what signals to include or exclude), testing different models, and iterating on prompt structure. Each experiment gave us fine-grained visibility into where accuracy improved or regressed, broken down by category, company, and transaction type.


## Results


The accuracy improvement from the rules-based approach to the LLM agent has been substantial. Transactions that once required manual re-coding are now classified correctly on the first pass, with 90% accurately coded over the past three months. Admin override rates have dropped significantly, meaning finance teams are spending less time on corrections and more time on analysis.


Taken together, these improvements move us closer to the larger product vision: expense categorization that happens reliably in the background, while humans stay focused on the exceptions that genuinely require judgment.


We’re also creating richer feedback loops so when users override our suggestions, we gather more information from them and use the corrections to improve future suggestions for that specific company.


## What's Next


We’re continuing to iterate. The current system is a strong foundation, but expense categorization is not a problem you solve once and walk away from. Every company has its own taxonomy, every transaction can gain new context over time, and every admin correction is a signal we can use to make the system better.


We have historical spending data, approval patterns, and team-level conventions that could provide additional signals without overcomplicating the prompt.


That’s the goal: a system where finance teams can trust the coding on the vast majority of transactions and spend less time correcting expenses after the fact. Manual expense categorization shouldn’t be anyone’s job, and increasingly, it doesn’t have to be.


---


Share this article


---


-
-
-


Tags


Smarter Travel. Cleaner Books. Happier Teams.


Navan’s all-in-one platform — built to streamline T&E.


[Get started](https://navan.com/get-started)


---


---


Tags


Share this article


-
-
-


---


This content is for informational purposes only. It doesn't necessarily reflect the views of Navan and should not be construed as legal, tax, benefits, financial, accounting, or other advice. If you need specific advice for your business, please consult with an expert, as rules and regulations change regularly.
