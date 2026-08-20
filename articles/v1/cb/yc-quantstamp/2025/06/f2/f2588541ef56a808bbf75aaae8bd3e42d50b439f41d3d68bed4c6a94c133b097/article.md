---
schema_version: "1.0.0"
document_id: "f2588541ef56a808bbf75aaae8bd3e42d50b439f41d3d68bed4c6a94c133b097"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/meet-the-authors-kacper-bak"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:884aa9e301e36728a1839ad62dc49fb2ff2298d22bd64b526805b79c78cccfd9"
---

# Meet the Authors: Kacper Bąk

#### Fundamentals of Smart Contract Security covers how blockchains function, design choices for smart contract development, common vulnerabilities, and best practices for writing smart contracts. This interview is one of a five-part series where we go behind the scenes and learn a bit more about the authors.


‍


*Kacper holds a Ph.D in Computer Science from the University of Waterloo for his work on modeling and analysis of software product lines. Before coming to Quantstamp, he worked at MathWorks (maker of MATLAB), Opera, and Samsung.*


‍


#### **Tell us a bit about your background. You have a lot of experience in software verification and software modeling - but how did you end up getting into the blockchain space?**


Over 10 years ago, I was very interested in cryptography, computer security, and software engineering. Around that time I learned about blockchain and bitcoin. I viewed bitcoin as a useful innovation that could facilitate quick and cheap cross-border payments. Over the next few years, I followed developments but was mostly occupied with software engineering research in grad school, and then working in the industry. Fast forward to 2017 and I joined Quantstamp. Along with some of my colleagues from grad school, we set out on a mission to improve the state of blockchain security amid the high-profile hacks.


#### **In addition to your work on the decentralized protocol, you’ve done a lot of in-depth smart contract audits - has your perspective on anything changed over time?**


Yes, on a few things.


First, we started doing white-glove audits to learn about real-world contracts. Our goal was to gather knowledge that would help us build a better decentralized protocol for automated security scans of smart contracts. We learned that although the underlying automated tools are useful, they are not silver bullets. Human expertise is still required to analyze the produced reports and to filter out false positives. To address some of these usability concerns, we developed human-readable reports that are now part of the protocol.


Furthermore, throughout the years, we have seen both high and low-quality smart contract code. Before the white-glove audits, I never would have expected to see projects that rush to deploy smart contracts without performing any testing upfront. These smart contracts are supposed to handle digital assets. It made me much more skeptical of the code we receive to audit. As a consequence, we released smart contract security guidelines for developers. We also try to help projects prepare for the audit long before the planned smart contract deployment.


Finally, we realized that smart contract security is a continuum. White-glove and automated audits play important roles, but they need to be accompanied by guidelines, bounties, live monitoring, assurance, etc.


#### **What’s your favourite part of the book? Who do you think will benefit the most from reading it?**


I don’t have a single favorite part of the book in the sense of a chapter - I like the overall theme. We were trying to balance two aspects: 1) the fundamentals that are likely to stay up-to-date for years, and 2) immediately useful information about security issues and possible fixes. The second aspect may help developers right now, but is likely to become less relevant over time due to the ongoing advancements. I think the book will be the most useful to developers who are new to the blockchain space. It may also help project managers to shape their thinking about security. Throughout the book, we emphasize that security should be taken into account from the very beginning of the project.


#### **What do you like most about your work, and what’s the most challenging?**


First of all, distributed ledger technologies is a really cool field to be in right now. There is a lot of ongoing innovation and people trying all sorts of crazy ideas. We are constantly learning what works and what does not.


Apart from the technical aspects of my work, I enjoy the whole decentralized way of thinking about projects. From the ground up, most teams are spread out all over the world, yet they are able to communicate effectively. I also enjoy the rather informal and fast-paced environment that manages to attract highly talented and skilled individuals.


As for challenges, I find designing decentralized systems fairly difficult. Apart from regular technical challenges (such as immature tools), you need to think about cryptoeconomic incentives, the ways that different actors may want to exploit your system, and user experience. The latter is especially problematic since decentralized systems tend to be less efficient than the centralized ones (albeit they provide other very important benefits).


#### **This industry changes incredibly fast. What advice would you give to upcoming smart contract developers? How can they stay at the top of their game and keep up with the latest developments in the space?**


To be honest, it is not very different from keeping up with the developments in any other field of knowledge. It is worth checking out academic publications, conference presentations, news outlets, startup websites, Reddit/Telegram groups, etc. It is important, however, to maintain focus while browsing the web. Blockchain is still a hot topic and discussions often revolve around digital asset pricing instead of the technology itself.


If I could give one piece of advice to upcoming smart contract developers, I’d say start thinking about security early in the development cycle and view it as a continuous concern that needs to be taken care of throughout the whole application lifecycle. Go and BUIDL! ****
