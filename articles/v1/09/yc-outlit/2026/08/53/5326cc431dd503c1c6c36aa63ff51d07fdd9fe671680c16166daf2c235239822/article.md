---
schema_version: "1.0.0"
document_id: "5326cc431dd503c1c6c36aa63ff51d07fdd9fe671680c16166daf2c235239822"
company_key: "yc-outlit"
company: "Outlit"
source_id: "yc-outlit-news-import-30f1306359d9"
canonical_url: "https://www.outlit.ai/blog/identity-resolution-as-infrastructure"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-15T05:14:18.474336+00:00"
fetched_at: "2026-08-15T05:14:20.724882+00:00"
content_hash: "sha256:35123c410dc48e3951ad784e15a255d2d83b63b43d26bac89b23086e32573367"
---

# Customer Identity Resolution

Customer identity resolution connects each CRM record, product event, invoice, support ticket, email, or call to the right customer before an AI agent uses it. Every system has its own version of the customer: your CRM might call the company Acme, product analytics might know it by a workspace ID, and billing might list the parent company. All three records can be accurate and still end up attached to the wrong account.


Customer identity resolution for AI agents is how you decide which records and events belong to the same customer. It matches IDs across systems and keeps disagreements visible. If two customer records need to be merged, a person can review them before an agent uses the result.


Putting all the data in one place doesn't solve that. Centralization makes records easier to search. It doesn't prove that they belong to the same customer.


Most B2B identity problems don't look obviously broken. The records themselves can be right. The connections between them aren't.


The examples below use demo data to show how those account relationships can go wrong.


## What does customer identity resolution actually do?


Customer identity resolution does two jobs:


1. **Matching decides where new information belongs.** A product event, invoice, ticket, email, or call needs to be attached to the right customer using the identifiers and account relationships available at that moment.
2. **Reconciliation decides what to preserve when records disagree.** When several records represent the same customer, reconciliation keeps their source identifiers and conflicting values visible. A reviewed merge can then bring duplicate customer records together.


Matching keeps one customer from being split into several incomplete profiles. Reconciliation repairs the split when it has already happened.


An AI agent shouldn't have to guess its way through either job. It should receive a resolved customer object: one customer profile, plus the source records and any disagreements that still need attention.


## Why does email-domain matching break in B2B?


Email domains are useful clues. They're not proof of customer ownership.


In B2B, the exceptions pile up quickly:


- **One customer uses several domains.** A product analytics account might contain users from a parent company, an acquired brand, and regional domains while all activity belongs to one customer relationship.
- **A parent company has several subsidiaries.** The parent, operating company, and legal billing entity may appear as separate records. A CS team might manage them as one customer, or each might have its own contract, workspace, and history. You need an explicit decision instead of a domain-based guess.
- **A consulting firm works across client accounts.** The firm might be a customer itself while its employees also generate activity inside several customers' accounts. Their domain identifies the person doing the work, not necessarily the customer that owns the event.
- **Your own team appears inside customer accounts.** Implementation and support teams are often invited into a customer's product account. If you use every user's domain as the owner, setup activity can get attached to your company instead of the customer.
- **A customer rebrands.** The company name and primary email domain change. Its product account, contract history, support history, and customer relationship don't. A simple domain join creates a new customer halfway through the timeline.


These aren't rare edge cases. A rule that works in a clean demo can start splitting customers or pulling unrelated activity together as soon as real account relationships show up.


A product account or workspace ID usually tells you more than the email domain of the person who generated the event. You still need to know where that ID came from, which customer it was linked to, and whether the link is still current.


Identity resolution isn't a cleanup project you finish once. A new source, rebrand, acquisition, or account migration can add another identifier that needs to be connected to the right customer.


The source system also has to send a stable identifier that can be mapped to a customer. An account or workspace ID is usually the strongest evidence, but a verified subscription, CRM account, billing customer, or contact mapping can also establish ownership. If an event arrives with no stable identifier or verified mapping, the system should preserve that uncertainty instead of guessing.


## How are identity matching and reconciliation different?


**Matching** answers one question: “Which existing customer should receive this record or event?”


Say a product analytics event arrives from` consultant@opsfirm.com` inside a customer workspace. The email domain points to the consulting firm, but the product account ID points to the client. The event should stay with the client while the record still shows who performed it.


**Reconciliation** asks a different question: “What should happen when the same customer exists as several records or those records disagree?”


Now say a rebrand left one customer record under the old company name and another under the new domain. Or maybe a parent and subsidiary were imported separately even though the team manages them as one customer.


Once someone reviews that relationship, a merge can bring their identifiers, contacts, billing events, facts, and support history into the customer record the team keeps.


You don't want to merge customers automatically because their names or domains look similar. If identifiers conflict, keep both visible until a person or a trusted rule decides what belongs together.


## What happens when customer identity is resolved incorrectly?


Missing data is obvious. A believable but wrong customer story is harder to notice.


Imagine a company that rebranded from Acme Labs to Northstar. Product activity still uses Acme's account identifier. New users arrive with Northstar email addresses. Billing is under a parent company. A consultant and one of your implementation engineers are active in the account too.


Every record can be accurate. A weak join can still produce several wrong outcomes:


- create separate Acme and Northstar customer profiles
- attach the consultant's activity to the consulting firm
- attach setup activity to your own company
- pull the parent's unrelated activity into the operating customer's timeline


An AI model can summarize whichever records it receives. It can't fix the customer identity graph by sounding confident about the result.


## Why does identity resolution happen before an AI agent works?


Finding a record isn't the same as knowing who it belongs to. An agent can retrieve an invoice, product event, support ticket, or conversation. That doesn't mean those records describe the same customer.


Identity resolution should happen first. It gives the agent one customer profile to use when it prepares a renewal brief, explains a support issue, finds a risk signal, or recommends a next step. That's the difference between[tool access](https://www.outlit.ai/blog/tool-access-is-not-customer-context) and customer context infrastructure.


Every workflow can reuse the same identity decisions. Without that shared layer, each agent prompt ends up rebuilding a slightly different answer to “Who is this customer?”


## How can you audit customer identity resolution?


Before building an agent on customer data, answer five questions:


1. **What identifies the account?** Is it a customer ID, product account, workspace, subscription, or something else?
2. **Which identifiers can you trust?** Separate stable account identifiers from clues such as names and email domains.
3. **Which real-world exceptions do you support?** Test subsidiaries, rebrands, multiple domains, consultants, and your own team inside customer accounts.
4. **What happens when two records disagree?** Keep both identifiers and the reason for each match instead of choosing the most convenient answer.
5. **How do duplicate customers get merged?** Decide who reviews the merge and which history moves to the customer record you keep.


If the answers live only in an agent prompt, every workflow will make identity decisions its own way. Put them in a reusable context layer, and every workflow can start with the same customer. That's identity resolution infrastructure. You maintain the rules once instead of rewriting them in every prompt.


## How does Outlit use identity resolution?


Outlit is an AI agent for customer success teams. It monitors every account and helps move onboarding, retention, and expansion work forward.


That agent runs on customer context infrastructure. Outlit connects product usage, billing, CRM, support, email, meeting, and identity data. Identity resolution attaches each event and interaction to the right customer, and Outlit keeps the account record current as the customer changes.


Identity resolution handles who each record belongs to. Outlit keeps those connections in a reusable[Customer Context Graph](https://www.outlit.ai/products/customer-context-graph) , while the broader[customer context infrastructure](https://www.outlit.ai/blog/what-is-customer-context-infrastructure) turns the account history into facts and signals. Outlit can then watch for changes such as stalled onboarding or renewal risk and give the agent enough context to help the customer success team respond.
