---
schema_version: "1.0.0"
document_id: "ac43ad5c1dbe9feb9005f8f65774a11129b86a3d95cc0df4c8474f49a09b92f5"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/towards-satisfactory-web3-software-engineering"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:4ad9e5d1aadc49d60cfb7ea47064d129e6a183033c0818da603dbec06ba36240"
---

# Towards SATisfactory Web3 Software Engineering

#### *Applying lightweight formal methods and SAT solvers to build better blockchain applications.*


In recent years, web3 has experienced significant growth with the emergence of innovative applications that are reshaping industries and unlocking new opportunities. However, this growth has also exposed vulnerabilities.[Numerous security breaches](https://arxiv.org/pdf/2210.16209.pdf) and[hacks](https://eprint.iacr.org/2016/1007.pdf) have not only resulted in substantial financial losses but undermined trust in the ecosystem, reinforcing the need to enhance the security of these applications.


In web3, traditional methods of bug detection and code verification fall short. What’s more, errors are made worse by the decentralized nature of the network; unlike centralized systems, errors cannot be easily reversed or undone, but require consensus among all participants to initiate a hard fork.


Immutability is a double-edged sword - bugs and errors can persist for a long time, increasing the likelihood of exploitation. Rather than patches, the entire application logic needs to be replaced. This can be a difficult and tedious task, especially if end-users also need to migrate to a new address of the code on-chain.


Understanding the risk of having faulty and immutable code handling millions of dollars worth of digital assets, the web3 space has been quick to apply formal methods when possible. The application of theorem provers, bounded model checkers, symbolic execution, static analysis, and linters became hot topics at industry conferences (e.g.[Devcon 2022](https://www.youtube.com/watch?v=ETlNhV9jYJw&pp=ygUlZm9ybWFsIG1ldGhvZHMgZXRoZXJldW0gZGV2Y29uIGJvZ290YQ%3D%3D) ,[DevCon 2019](https://www.youtube.com/watch?v=tq5XH3JedqM&pp=ygU4Zm9ybWFsIG1ldGhvZHMgZXRoZXJldW0gc2NpZW5jZSBvZiBibG9ja2NoYWluIGNvbmZlcmVuY2U%3D) ). They have been applied to both low-level consensus algorithms and high-level dApp code. However, formal methods are non-trivial, and expertise in relevant tools and techniques is hard to find. Additionally, the cost and time required to formally prove an execution matches a specification can be prohibitive. Hackathon-projects-turned-startups, or scrappy DAOs trying to develop blockchain ecosystems lack both the in-house expertise and the capital to afford these approaches.


Enter lightweight formal methods. These methods[offer a practical approach](http://www4.cs.fau.de/~barner/me/barner05alightweight.pdf) to identifying and fixing bugs in dApp code by sacrificing full verification for usability.


They frequently use tools that are powered by state-of-the-art SAT solvers to find bugs. Often available as push-button tools, they focus on the detection of well-known classes of bugs. Their success stems from the limited nature of blockchain execution, which restricts the state space for dApps. While they may not provide complete verification, they’re very effective in catching common bugs and preventing catastrophic errors.


With this in mind, it can be argued that lightweight formal methods are critical to web3 development. First, the blockchain community has already embraced some lightweight formal methods, and they fit well with the ethos of both independent and institutional web3 developers. There are SAT-based techniques that are not yet used that could have prevented errors in real blockchain systems. And, numerous web3 domains exist that align well with the next phase of lightweight formal methods.


### **What are Lightweight Formal Methods?**


Different authors may use “lightweight formal methods” to describe various approaches.[Daniel Jackson](https://mitpress.mit.edu/9780262528900/software-abstractions/) , Professor of Computer Science at MIT, suggests that lightweight formal methods are those where “\[software\] models are developed incrementally, driven by the modelers' perception of which aspects of the software matter most, and of where the greatest risks lie, and automated tools are exploited to find flaws as early as possible.”


This definition doesn’t discourage powerful tooling, but emphasizes that its use be focused on certain areas of concern. Naturally, this reduces the cost of using these methods, and by definition, won’t consider the entire system at once.


Lightweight formal methods might not be sufficient to make claims about the entire system, but they’re useful in detecting local bugs and confirming particular behaviors of a system. They may view the correctness of the system as the satisfaction of several properties, some or all of which can be modeled, automatically checked, and used to guide implementations. Being able to rapidly prototype systems at the specification level saves development time. And, trading total confidence for practicality has its benefits—they often don’t take as long to employ, are less costly than “full” formal methods, and are more accessible to average developers, reducing the need for expensive in-house experts or consultants.


Web3 developers have already embraced lightweight formal methods for bug detection in languages like Solidity, commonly used for Ethereum dApps.[Numerous tools](https://ieeexplore.ieee.org/abstract/document/8782988) such as[EthBMC](https://www.usenix.org/conference/usenixsecurity20/presentation/frank) ,[Manticore](https://github.com/trailofbits/manticore) , and[Slither](https://ieeexplore.ieee.org/abstract/document/8823898?casa_token=nJUgMHUTChgAAAAA:WAsDiuPRp4mNkO6T4tzdEZc57mYcjSg9c6gQ9YCYZ8tMLnt_ViMRwisCIbIxaqGXgS9htgIg) have also been able to identify issues such as unintended reentrancy and integer overflow problems.


Identifying these common issues can be invaluable. In 2016, careless use of a[reentrant function](https://quantstamp.com/blog/what-is-a-re-entrancy-attack) resulted in a loss of 11.5 million Ether, valued at about $50M USD at the time. Other classes of bugs include checking for dependencies on timestamps (which can be modified by network operators to some extent), integer under- and overflow issues, and mishandled exceptions.


Under the hood, these tools are employing SAT solvers like[Z3](https://github.com/Z3Prover/z3) and[CVC5](https://cvc5.github.io/) to find bugs. These approaches are powerful, but they don’t scale. Since deploying and executing code on-chain costs money, dApps are incentivized to keep their on-chain footprint small. Since blockchain applications remain relatively small they stand to benefit from these tools. However, the tools still fall short of showing program correctness; typically, the correctness of a dApp is more than just non-reentrant functions.


Despite the shortcomings covered above, the beauty of lightweight methods is their simplicity and accessibility, making them a perfect fit for web3 developers. First, their value: symbolic execution can catch more bugs than testing alone. Because they don’t require extensive investment or expertise, they are as relevant to independent developers as they are to well-funded projects. What’s more, they can be integrated and improved collaboratively, a crucial feature when it comes to the ever-evolving complexity of web3 dApps.


### **Building More Secure Systems with Lightweight Formal Methods**


Of course, lightweight formal methods are not a silver bullet. Rather, they complement testing and code audits, both still essential components in ensuring high-quality code. Unit tests and end-to-end tests help ensure that at least individual functions and “happy path” scenarios work as intended, but they may miss large classes of bugs. Economic attacks in particular—including flash loan issues—are difficult to encode in tests, as they involve interactions with other on-chain dApps and complex settings.


Audits are the last line of defense against incorrect code. However, they’re typically conducted near the end of the project’s development. At that point, it may be costly to correct bugs in complex systems, as entire modules may need to be rewritten. Clearly, this scenario is better than deploying bad code. But can web3 developers do better?


In some cases, they can. A 15-line token deployed on Ethereum probably doesn’t require additional steps in development, but when it comes to complex systems like cross-chain protocols (“bridges”), side-chains, rollups, and DeFi applications, lightweight methods can be used at the specification level to identify potential feature interactions before code is even written.


For example, feature interactions can be studied to determine if certain features are mutually compatible, or refine problematic feature interactions using a holistic view of the system prior to writing code. This can be especially effective for systems that are to be deployed on multiple blockchain platforms, each with its own quirks. In this case, developers should see their code as a software product line, where each deployment is a specific configuration of features.


In a software product line, we can use lightweight formal methods—specifically SAT solvers— to identify feature interactions. It’s possible that if feature interaction analysis had been applied to the[Meter bridge](https://blog.chainsafe.io/breaking-down-the-meter-io-hack-a46a389e7ae4) , a loss of funds could have been avoided. Specifically, the decision to support digital assets as “wrapped” tokens alongside native tokens may have been detected as a problematic feature interaction.


Instead of focusing on the code itself, this approach applies lightweight formal methods at the specification level. This is important because these systems are continuously growing. Cross-chain bridges will likely expand to support more chains as blockchains like Ethereum embrace multi-chain futures, each chain with its own functionality and user base. DeFi interactions will increase in complexity as more trading options emerge, new tokens are added, and composability becomes a defining feature of the ecosystem. Specification-level lightweight formal methods can also overcome the scaling issue of applying SAT solvers directly to code.


Specification-level formal methods can also be beneficial for testing and audits. The artifacts generated through these methods provide detailed information about various aspects of the system. They can help guide test cases and be shared with code auditors to make their work more effective. Of course, it's important to note that tests and audits are still crucial to check implementations for errors.


### **Satisfiability: Future Applications**


Looking ahead, lightweight formal methods have a lot of promise when it comes to addressing web3-specific challenges. While their current adoption among web3 developers is a positive trend, there are still areas lacking appropriate methods. One example is their application in ensuring the correctness of zero-knowledge proof systems. Without a doubt, these could be instrumental in helping securely scale systems like these and preventing the next major hack.


It will be important to apply SAT to other areas of software engineering, and recognize these tools themselves face technical challenges that need to be mitigated to further enhance their effectiveness and reliability.


Ultimately, lightweight formal methods can allow developers to build safer and more reliable applications, mitigating risk and instilling greater confidence in the web3 ecosystem.


‍ *‍*[Share this post on Twitter](https://ctt.ac/okAl7) *.*
