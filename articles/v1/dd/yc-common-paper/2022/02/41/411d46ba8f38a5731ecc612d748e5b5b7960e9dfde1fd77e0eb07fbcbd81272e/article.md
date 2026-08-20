---
schema_version: "1.0.0"
document_id: "411d46ba8f38a5731ecc612d748e5b5b7960e9dfde1fd77e0eb07fbcbd81272e"
company_key: "yc-common-paper"
company: "Common Paper"
source_id: "yc-common-paper-rss-3b76f5ee637c"
canonical_url: "https://commonpaper.com/blog/contracts-should-be-apis/"
published_at: "2022-02-07T16:20:46+00:00"
first_seen_at: "2026-07-20T23:20:15.555416+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:ca72cae8e6272d5f97f9f6136827b2d246d91dcdfa82bc2e1b30719518ba5cec"
---

# Contracts should be APIs

Contracts control trillions of dollars of economic activity. On a scale this big, even small inefficiencies on each individual contract would add up to billions of dollars of waste. Unfortunately, the inefficiencies in contracting are not small.


In the last decade, electronic signatures have reduced the need to send dead trees through the mail, but companies are still operating on the same set of fundamental assumptions that they had when contracts were on paper. There is still work to be done. If we rethink the jobs that we want contracts to do, the benefits can be profound.


## **Data in and data out**


Getting data into contracts is time consuming and manual, even if the data is about something that has happened many, many times before. When a vendor and a customer start a business relationship, they often argue over whose contract template to use. Regardless of which they choose, one side has to review something entirely new from scratch. Then they negotiate specific terms, with each proposal transmitted via a tracked change in a Microsoft Word document attached to an email. The latency of each of these proposals is usually measured in days if not weeks, and too often they reflect arguments over semantics rather than substance.


The problems don’t stop when the contract is signed. Business stakeholders in each company need access to the information in the contract in order to do their jobs, but it’s not structured, easily accessible, or machine readable. The process for getting the data out is just as manual and time consuming as getting the data in. The right contract must be found and the relevant term must be located and interpreted in the context of the full agreement.


At Common Paper, we have a different vision: **Contracts should be APIs** .


Like the APIs that sit between software applications, contracts are interfaces between companies. They contain data in their clauses and prescribe actions in their obligations. A wide variety of systems and teams need to take action based on the information within them. If we build software that treats contracts as APIs, we can unlock the scalability, clarity, and efficiency that APIs have brought to the world of software.


## **Why this is hard**


Countless products and services have been deployed to try to address the pain points outlined above. But none of them address the root cause that prevents us from working with contracts as APIs: **Contract terms are unstructured and inconsistent** .


As an example, consider the following ways to describe the governing law of a contract:


Each of these are legally valid. There are an infinite number of other, also legally valid, ways to describe governing law. You could make a similar graphic for every term in most contracts.


A tremendous amount of time, energy, and expense goes into drafting, negotiating, and then parsing these different terms. There is a crucially important difference between them, but it’s buried in a wall of text.


## **Standards & structure**


Rather than each company coming up with its own bespoke phrasing for the same term, we have a better way: standard contracts. There are a number of widely used examples that show how contract standardization adds value for all parties involved.


- Early stage startup financings use the SAFE
- Open source software uses the Apache license
- Online ad sales use the IAB standard terms
- Financial derivatives use the ISDA master agreement


These standards have eliminated some of the inconsistency in how terms are written, but there is an opportunity to go even further. In practice, most standard agreements are used with additional riders, addenda, or tweaks to the standard contract. That’s better than each party starting from scratch, but we’re left with a mix of standards plus unstructured and inconsistent terms. Unstructured terms mean unstructured data, which makes it incredibly hard for contracts to interact with other systems programmatically.


We can head this off if we treat the standard terms as an atomic unit that is not modified directly. The details about the specific parties to the contract and the terms that vary from deal to deal will still need to be included, but we can create a section at the beginning of the contract to capture these variables in a structured format.


Continuing with the governing law example above, we can use a standard term like “This Agreement shall be governed by the laws of **Governing Law.** ” Then, we treat the bold term as a variable that we set in the beginning of the contract, analogous to **Governing Law** = Delaware.


Grouping the variables and their values in a table makes these contracts dramatically easier to draft, negotiate, and understand. After reviewing the standard terms once, people and companies can re-use them over and over while saving time and reducing risk.


## **Automation and enablement**


This is better for the humans involved, and it also opens up new possibilities for software. If contract terms become structured and standardized, then we can also make each of the variables programmatically accessible and reliably interpreted. This makes a whole host of manual and time consuming processes trivial to automate. As one example, imagine going through due diligence for a round of financing and pulling answers to the investor’s questions with a single click.


A future where contracts are APIs will finally trade static artifacts for automated interfaces, unlocking the level of integration we expect from working with technology in the 21st century.


At Common Paper, we’re building this future. If you’d like to bring this vision to life in your business, you can[try out our standard contracts](https://www.commonpaper.com/standards) or[sign up today](https://app.commonpaper.com/auth/auth0?screen_hint=signup) .
