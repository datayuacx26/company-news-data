---
schema_version: "1.0.0"
document_id: "507065ac21a552121a0d056b52876ce803314d47125c15bc2a964061626bbe5c"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/meet-the-authors-ed-zulkoski"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:eb1e3f82d080a915cefdab0b18aed1f97cecd3581a3723e31a01db9141a176bb"
---

# Meet the Authors: Ed Zulkoski

#### Fundamentals of Smart Contract Security covers how blockchains function, design choices for smart contract development, common vulnerabilities, and best practices for writing smart contracts. This interview is one of a five-part series where we go behind the scenes and learn a bit more about the authors.


‍


*Ed holds a Ph.D in Computer Science from the University of Waterloo. His research there was primarily in SAT/SMT solvers and formal verification technologies, with a focus on understanding what makes SAT formulas hard or easy for solvers. Before joining Quantstamp, he worked at Microsoft Research.*


‍


#### **Can you share a bit about your background? What drew you to Quantstamp and how did you get involved?**


I met some of the other team members who were also Ph.D. students at the University of Waterloo. From there, I met Quantstamp’s Co-founder, Steven Stewart, and we ended up doing some work together. I was focused on SAT/SMT solvers, so the work at Quantstamp was a great fit given my expertise in this area. Smart contract analysis seemed like a great application of our work, as any vulnerabilities can have significant financial impact, while at the same time, the programs are \[usually\] small enough to be automatically analyzed in meaningful ways.


*(SAT and SMT solvers form the backbone for many automated bug-finding tools such as those used by Quantstamp)*


#### **What advice would you give to aspiring software engineers that want to build a career in this industry?**


My biggest piece of advice would be to research and understand the *fundamental* problems blockchain aims to solve, and investigate what challenges companies on the cutting-edge need to solve in order to achieve this vision. Make sure it's a field you’re actually interested in, and further try to understand *why* you're interested. It's easy to get sucked up in the hype of blockchain, but hype is an unsustainable platform for success. Blockchain is not a swiss-army knife to solve all the world's "centralization problems," but it does have the potential to revolutionize how we think about commerce and other decentralized applications.


‍


****


#### **What’s your favourite part of the book? Why was this something you wanted to be part of, and how do you think it will contribute to the space?**


Probably Chapter 2, because I feel like it gives a really good overview of the fundamentals. It covers all the cryptoeconomics that revolve around the chain, and what can go wrong if you design your blockchain or dApp in an insufficient way.


The book is ideal for a novice person, as it would let them learn more about blockchain and hopefully develop some interesting applications on top of it. It’s also got something for people who have an existing understanding and just want to deepen their knowledge.


#### **In your opinion, what are some of the biggest challenges right now in smart contract security?**


I’d say developing automated tools that are practical and useful for real world contracts. Existing tools try to look for very standard issues that can go wrong - stuff like re-entrancy, overflow and those kind of things. Tools that are both easy to use and contract-specific are probably the most useful right now. On top of that, since manual "white glove" audits are currently one of the best ways to have confidence in a smart contract's correctness, any tools that aid an auditor's ability to understand and reason through the code are highly valuable.


#### **Are there certain projects you're working on right now that you're really excited about?**


Yes, I’m excited about the monitoring service that we’re working on. Basically, a smart contract analyzer tries to figure out statically whether there’s something correct or incorrect about a contract. It provides sophisticated analysis while the contract is being deployed, meaning an added layer of security. This would be really difficult to have with typical analyzers.
