---
schema_version: "1.0.0"
document_id: "3415c6fd03aebfc70169ae72c9c44e922d1479611b06185c0087f17b1636954f"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/open-sourcing-our-bounty-protocol"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:8d806e6f42448c8317d604aa19bde9fbb050052624e5662a76e41f2c046e5e02"
---

# Open Sourcing Our Bounty Protocol

We are[open sourcing the code for the Quantstamp Bounty Protocol](https://github.com/quantstamp/qsp-bounty-protocol) , a decentralized marketplace for developers to identify bugs in smart contracts that automation cannot detect. It uses the power of smart contracts and cryptocurrency to leverage software engineering talent from around the world to add an essential layer of infrastructure for blockchain security.


### Why we need it


Over 250 million USD has been lost or stolen due to bugs in smart contract code. In order to scale the security of smart contract blockchains using automation, decentralized tools like the Quantstamp Security Network can be used. Although the Quantstamp Security Network detects vulnerabilities such as the re-entrancy bug that led to the DAO hack in 2016, there are certain bugs that currently only human auditors can detect.


Centralized security teams have more nuanced analysis capabilities than current generation automated analyzers, but they have limited manpower. They cannot scale to meet the growth of smart contracts being deployed onto the Ethereum network. The decentralized Bounty Protocol supplements automation by allowing human developers from around the world to report more nuanced vulnerabilities and check for bugs against specifications. Through cryptocurrency and smart contracts, these human auditors can be rewarded to meet the needs of the smart contract market as it grows.


The Quantstamp Bounty Protocol has 3 roles: Bounty Providers, Bug Hunters, and Judges.


### How it works


A Bounty Provider is someone who submits their smart contract for review to the Bounty Protocol.


Any developer, which we will refer to as bug hunters, can then review the smart contract code and report vulnerabilities if they find them. In order for the bug hunter to receive their bounty, judges must vote to decide if the bug hunter did in fact report a valid vulnerability. The judges are selected using a token curated registry (TCR).


If enough judges vote in favor of the bug hunter, the bug hunter receives their bounty.


‍


### Commit-Reveal Schemes


The Bounty Protocol uses two commit-reveal schemes in order to prevent judges and bug hunters from gaming the system.


Bug hunters submit reported vulnerabilities using a commit-reveal scheme in order to prevent front running. Without a commit-reveal process, a malicious actor can wait until an honest bug hunter submits a report on the blockchain. While that transaction is pending, the malicious actor can submit that exact answer but with a higher gas fee. If the transaction is processed first, the malicious actor succeeds.


Judges only receive a reward if they vote with the majority. In order to prevent early votes from biasing the votes of judges who vote later in the process, judges first submit a hash of their vote. After this period is over, judges submit a second transaction with their revealed vote.


### Why open source?


By open sourcing our code, we anticipate that Bounty Protocol users will benefit in two ways:


1. Enhanced security and code quality: We anticipate the quality and security of the network to increase because engineers from the open source community have the option to suggest edits to the code.
2. Transparency: Users will be able to independently verify that our network operates as we claim it does.
3. Decentralization: Users will be able to modify or fork the bounty protocol as they see fit.


A couple of users have already submitted issues, contributing to the open source Bounty Protocol.


Further Information


[Check out our repository](https://github.com/quantstamp/qsp-bounty-protocol) or[learn more about how the Bounty Protocol works](https://github.com/quantstamp/qsp-bounty-protocol/blob/develop/instructions.md) .
