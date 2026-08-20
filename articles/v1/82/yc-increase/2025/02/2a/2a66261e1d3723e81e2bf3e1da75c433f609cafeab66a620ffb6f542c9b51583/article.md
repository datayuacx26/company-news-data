---
schema_version: "1.0.0"
document_id: "2a66261e1d3723e81e2bf3e1da75c433f609cafeab66a620ffb6f542c9b51583"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/compliance-as-code"
published_at: "2025-02-24T00:00:00+00:00"
first_seen_at: "2026-07-24T11:51:21.619731+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:da1110718607e13f02af3339933d6b02a768053d7795b26963ccb91433bcf295"
---

# Compliance as code — Increase

As an engineer, how do you ensure that a code change doesn’t inadvertently break a compliance or legal obligation? More fundamentally, how do you even know what those obligations are?


[Conway’s Law](https://en.wikipedia.org/wiki/Conway%27s_law) tells us that compliance and engineering teams tend to operate in separate systems that rarely talk to each other. As a result, policies, procedures, and legal frameworks live in one world, while production systems live in another. This disconnect creates risk—what’s written in a compliance document may not reflect what actually happens in production.


We think there’s a better way to approach this.


What if your application’s code also helped manage its regulatory, compliance, and legal requirements? You’d have a source of truth for all your obligations and encoded tests to ensure they are continuously fulfilled. This would eliminate discrepancies with outdated documents and prevent you from making a change if it would bring you out of compliance. Not only would this remove uncertainty when heading into an audit or exam, but it would essentially provide an internal automated audit function—ensuring compliance is proactive, not reactive.


This system is one of the many ways we’re scaling compliance at Increase.


## How it works


Our system has three concepts: Obligation Documents, Obligations, and Fulfillments.


**Obligation Documents** define a whole regulatory or compliance framework, such as the System and Organization Controls (SOC) or Truth in Savings Act (Regulation DD). Obligation Documents live in code, not data, and define the source of truth for its obligations and audit lifecycle.


**Obligations** represent the individual, verifiable requirements for our business. Every Obligation is associated with a parent Obligation Document—which contains many individual Obligations.


**Fulfillments** are executable code that verify if an Obligation is met. There can be one or more Fulfillments associated with a single Obligation. Additionally, Fulfillments can take one of five implementation shapes.


## Types of Fulfillment


**Code Enforcement:** These fulfillments can sit in the test suite. For example, checking that transactions are processed in a certain order.


**State Checks:** These fulfillments involve checking the state of databases or external systems. For example, ensuring that all entities have verified tax IDs, financial ratios fall within acceptable ranges, or that external systems (such as firewall deny lists) are properly configured, monitored, and alerted.


**Policy Enforcement:** These fulfillments require written policies or procedures. For example, ensuring there is a policy in place to assess third-party contractors and vendors for risk to Increase.


**Manual Assurance:** These fulfillments verify that a named role within the company makes a regular assurance about the current state. For example, that security incidents have been examined by the Chief Information Security Officer.


**No Action:** Some obligations cannot be actioned by us because they require regulatory intervention. For example, conducting specific interviews. These obligations are tracked for completeness.


## Testing


All of our Obligation Fulfillments run on our[Checker framework](https://increase.com/articles/automated-invariant-monitoring) , which manages the execution scheduling and handles the results in an easy to use interface.


The neat thing about bringing obligations into your codebase is that the source of truth for requirements lives as close to the implementation as possible.


Let’s look at an Obligation based on a 2010 Federal Deposit Insurance Corporation (FDIC)[order](https://www.fdic.gov/news/financial-institution-letters/2010/fil10081.html) to address compliance issues with overdraft fees and transaction processing:


```text
code_enforced_obligation  (
<<~DESCRIPTION  ,
Transactions should be processed in a neutral order that avoids manipulating or structuring the processing order to maximize customer overdraft and related fees.
DESCRIPTION
'  processes ACH credits before debits  '
end
```


This is a deterministically provable statement that we can test against. In order to ensure we continuously remain in compliance, we have fulfillment code that looks like:


```text
it   '  processes ACH credits before debits  '  ,   :  regulations   do
account   =   make_account  (  balance  :   10  )
debit_entry   =   make_entry  (  account  ,   amount  :   -  15  )
credit_entry   =   make_entry  (  account  ,   amount  :   20  )
FedACHEntryAllocating  .  run!
expect  (  historical_balance_below_zero?  (  account  )).  to   be_falsey
end
```


We therefore have a test in our codebase against our transaction processing system that enforces a certain processing order and links back to this Obligation.


## Bidirectional linking


Often, the sub-organization building products is segmented from those interfacing with regulators and auditors. This can lead to many issues. In particular, audit requests ultimately branch out far beyond those interfacing with the auditor, either because it’s unclear how an obligation is fulfilled, it’s inaccessible to the working group, or the state of the system (a procedure, network diagram, access list, etc.) has shifted since the last audit.


A tremendous benefit of bringing regulations into code is the ability to link the source of truth (the implementation in code or policy document, for example) back to the Obligation which it fulfills.


This uniquely ensures that someone altering the implementation is aware of its otherwise opaque downstream impact. For example, a unit test may break that points back to the neutral transaction processing order Obligation, which gives a developer more context on why that code exists. Someone editing a policy document will see a footnote that links back to the Obligation, so they can proceed with cautious confidence.


The bidirectional link is also enforced by the system. A breakage in either direction notifies the team.


## Why compliance as code works for us


For many businesses, this approach may feel too heavy-handed. Compliance automation platforms like Vanta provide powerful, accessible solutions that help companies quickly achieve and maintain compliance with standard frameworks. However, these platforms are optimized for common compliance requirements.


As banking infrastructure providers we, of course, adhere to common standards such as the System and Organization Controls (SOC) and Payment Card Industry Data Security Standards (PCI DSS). However, we also have bank-specific obligations, such as Regulation E (Electronic Fund Transfers), Federal Financial Institutions Examination Council (FFIEC), and Bank Secrecy Act (BSA) and additionally must comply with evolving regulatory guidance that introduces one-off obligations. These industry-specific requirements make off-the-shelf compliance solutions impractical for us.


Compliance as code offers a more flexible and precise way to manage our obligations. It enables our engineering and compliance teams to work from the same source of truth, fostering collaboration and mutual understanding. Engineers engage directly with regulatory guidelines, while compliance specialists can inspect and interact with the code that enforces them.


By embedding compliance into our development process, we ensure that regulatory requirements are continuously enforced, easily auditable, and integrated directly into our workflows — rather than maintained as a separate, manual function. This approach keeps our infrastructure both resilient and adaptable as regulations evolve.
