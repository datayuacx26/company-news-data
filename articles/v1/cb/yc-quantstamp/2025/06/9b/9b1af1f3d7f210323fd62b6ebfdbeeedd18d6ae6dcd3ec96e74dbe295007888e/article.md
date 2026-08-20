---
schema_version: "1.0.0"
document_id: "9b1af1f3d7f210323fd62b6ebfdbeeedd18d6ae6dcd3ec96e74dbe295007888e"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/what-is-a-smart-contract-audit"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:8e67bc0792cbab996bf6984e697cf3518ec33b36baa42718486144776a62b715"
---

# What is a Smart Contract Audit?

Blockchain technology is promising to revolutionize entire industries, but hacks and exploits of popular blockchain applications threaten to undermine it before it even takes off. Isn’t blockchain supposed to be secure? It’s not that simple.


#### **Blockchain is Secure, Blockchain Applications Are Not**


An enormous amount of hashpower exceeding the combined power of the world’s top supercomputers secures the Bitcoin blockchain. Similarly, incredible computing power secures Ethereum. However, while the blockchains themselves are secure, the applications running on the blockchain may not be.


These applications interact with the blockchain through smart contracts, but just like any other software, bugs in the code can lead to security vulnerabilities. Unlike most other types of software, blockchain applications often directly control financial assets. Bugs can lead to serious amounts of money being lost, such as in the infamous DAO hack.


In 2016, a year after Ethereum was created, the DAO was born. The DAO, a Decentralized Autonomous Organization, was an investment fund entirely controlled through smart contracts. Unfortunately, there was a bug in the code. Hackers exploited this vulnerability to siphon off $50 million worth of ether from the DAO. Since the DAO was autonomously governed through its code, no single actor could be called to stop the hack once it started.


Whereas bug-free code is nice to have in other types of software, in blockchain applications, it is essential. To make sure blockchain applications are safe, a security audit of the smart contracts is needed to check for bugs and vulnerabilities.


#### **How a Smart Contract Audit Works**


Smart contract security auditing is a thorough analysis of a blockchain applications’ smart contracts in order to correct design issues, errors in the code, or security vulnerabilities. A professional audit by a leading security auditing company like Quantstamp will typically involve the following steps:


1. Agreeing on a specification
2. Running tests
3. Running automated symbolic execution tools
4. Manual analysis of the code
5. Creating a report


### **Specification**


The specification and other associated documentation explain the project's architecture, design choices, and build process. By convention, this documentation is included in the project’s README file. Whitepapers and docstrings, though helpful for describing particular sections of code, are no replacement for a well-written specification. Without a specification, auditing teams have no way to know what the code should be doing and can’t tell if it works as intended. Hence, the first step of a good audit is ensuring the project contains a full specification, which will serve as the backbone for the audit process.


#### Code Freeze


Auditors will often ask when a “code freeze” is going to happen, meaning that the code has been finalized. At this step, the code should be in the final draft stage: the developers have looked over everything, ensuring that the best effort has been made at fixing any abnormal or undesirable code. A final commit hash is included in the specification provided to the audit team in order to ensure that both the project team and the audit team agree on the code being audited, and that any changes made to the project are not in scope for the audit.


### **Testing**


Tests are the simplest, easiest way to detect bugs. These range from unit tests targeting individual functions to integration tests addressing larger chunks of code. High test coverage diminishes the number of easily detectable bugs making their way into an audit, making everyone’s lives easier. In addition, tests help to ensure that all developers on a team have agreed upon the project's intended performance and functionalities, preventing confusion during the audit. They also serve as informal documentation for the auditors, demonstrating another way to give the auditors insight into the expected functionality of the project.


The easiest step of an audit is to run the test suite. If all tests pass, then it's less likely there are obvious issues. If tests fail, it's time to see what went wrong and ask the developers whether they knew of failing tests prior to the audit. If a high number of tests fail, it may be necessary to pause the audit before continuing on in case the project team needs to remake massive or critical portions of the codebase.


#### Line Coverage


Checking the test line coverage — how much of the code is evaluated by tests — is another essential step. Greater test coverage generally relates to more tested features, and more tested features correlate to fewer unknown issues and vulnerabilities. While all quality assurance engineers aspire for 100% line coverage, 85-90% line coverage per contract is reasonable for most projects. If the line coverage falls below 75% for most contracts, the project team should be informed quickly, giving them time to include more tests before deployment.


### **Automated Analysis**


As the demand for safer code grows, so does the development of automated bug detection software. Symbolic execution tools have been developed based on[research on common vulnerabilities detected within Solidity smart contracts](https://consensys.github.io/smart-contract-best-practices/bibliography) . These tools analyze a program to determine which inputs cause each part of a program to execute. This software streamlines the auditing process by making it much easier to identify common pitfalls in code, reducing audit turnaround time and freeing up human auditors to focus on complex and novel vulnerabilities.


#### False Positives


Automated analysis tools for Solidity are in a relatively early stage of development and thus far from perfect. In addition, these tools are not aware of the context in which each piece of code is written. Hence, it is common for these tools to report false positives and incorrectly claim that an issue exists. To ensure that false positives are removed from the report results, manual inspection is required for each reported vulnerability.


### **Manual Analysis**


Automated tools can help to easily pinpoint common vulnerabilities but may not understand a developer's intention. Oftentimes, software may not seem to contain vulnerabilities but differs from the intended functionality. As a result, manual inspection is necessary to enhance detection of potential vulnerabilities.


An experienced auditing team digests the specification, then either confirms that the project performs as expected or identifies aberrations, offering recommendations to the project team.


At Quantstamp we generally have multiple engineers independently look at the code, and then compare their results afterwards, minimizing the chance of missed errors.


### **Audit Report**


After inspection through tests, automated analysis, and manual analysis, the auditing team must compile a report for the project team, ideally accompanied with time for the two teams to discuss and act on the report's findings. This last step is the most essential to seeing through the audit's work into the final project. The project team should fully understand the issues and vulnerabilities detected in the current project, along with the audit team's recommended patches, then integrate those recommendations into the project. If time permits, a follow-up conversation or audit is best practice to ensure no more possible vulnerabilities remain in the project.


A final note is that there is no perfect step-by-step guide to a smart contract audit. Standards are still under development, and different teams follow different design paradigms. In the end, many significant decisions are left to the judgment of the auditing team, and the project team may disagree with the recommendations for reasons that are subjective, cultural, or otherwise. While neither party is necessarily more correct than the other, it takes time to ensure everyone is on the same page about the state of the project. As long as all the information is put forward for open discussion, the likelihood of failure decreases immensely. With all this in mind, communication and scrutiny are clearly critical to the success of a smart contract audit.


### About the Author


Nadir Akhtar is a Research Engineer with Quantstamp. A former president of Blockchain at Berkeley, he instructs edX courses in his spare time. He is currently studying Computer Science at UC Berkeley and helps Blockchain at Berkeley's educational initiatives.


*Interested in receiving a Quantstamp audit or other service? The best crypto native companies trust Quantstamp to audit their products including Chainlink, OmiseGO and Binance.*
