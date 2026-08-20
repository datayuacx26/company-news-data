---
schema_version: "1.0.0"
document_id: "71ff1f1f022d80760801baf760aa44f95fcc3175f9a330519034b74399e38d7b"
company_key: "yc-stackup"
company: "Stackup"
source_id: "yc-stackup-news-import-4adfae6a34ed"
canonical_url: "https://www.stackup.fi/resources/mpc-wallets-a-complete-technical-guide"
published_at: null
first_seen_at: "2026-07-27T05:25:45.190590+00:00"
fetched_at: "2026-07-28T21:16:46.713740+00:00"
content_hash: "sha256:e2e0633a558988e96a3da70cb22e590573e80bd3ac8781005c17a1c0a3c77fb4"
---

# MPC Wallets: A Complete Technical Guide (2025)

## What Is an MPC Wallet?


An MPC (Multi-Party Computation) wallet is a cryptocurrency wallet that splits a private key into multiple encrypted shares distributed across different parties or devices. Unlike traditional wallets where one entity holds the complete private key, MPC wallets ensure no single party ever has access to the full key, even during transaction signing.


When you need to sign a transaction with an MPC wallet, the key shares work together to generate a valid signature without ever reconstructing the private key in one place. This cryptographic technique, borrowed from[secure computation research](https://eprint.iacr.org/2016/013.pdf) dating back to the 1980s, has found new life in protecting digital assets.


## How MPC Wallets Actually Work


### The Technical Foundation


MPC wallets rely on threshold signature schemes (TSS), a specific application of multi-party computation. The process begins with distributed key generation (DKG), where cryptographic shares are created without any single party ever seeing the complete key. When it's time to sign a transaction, these parties engage in a multi-round communication protocol, exchanging cryptographic proofs while keeping their individual shares secret.


Most implementations use a threshold system—for instance, requiring 2 out of 3 shares or 3 out of 5 shares to authorize a transaction. This threshold approach provides redundancy while maintaining security, ensuring that losing one share doesn't mean losing access to your funds.


### The Cryptographic Protocols used for MPC


Modern MPC wallets have evolved through several generations of protocols. The[GG18 protocol](https://eprint.iacr.org/2019/114.pdf) ,developed by Gennaro and Goldfeder in 2018, was the first practical threshold ECDSA implementation, though it required nine rounds of communication for signing. This was improved in GG20, which reduced communication complexity and fixed security vulnerabilities. Today, the state-of-the-art[CGGMP21 protocol](https://eprint.iacr.org/2021/060) requires only four signing rounds, making it fast enough for practical use while maintaining enhanced security proofs.


Each protocol generation represents a careful balance between security, performance, and implementation complexity. Understanding these differences helps explain why MPC wallet performance varies significantly between providers. Those using newer protocols can offer faster signing times and better user experiences.


### The Mathematics Behind MPC


The mathematical foundation of MPC rests on[Shamir's Secret Sharing](https://web.mit.edu/6.857/OldStuff/Fall03/ref/Shamir-HowToShareASecret.pdf) , where a secret is divided into n shares such that any k shares can reconstruct it. MPC leverages homomorphic properties, allowing operations on encrypted shares to produce encrypted results of operations on the underlying values. Combined with[zero-knowledge proofs](https://www.youtube.com/watch?v=fOGdb1CTu5c) that let parties prove they performed calculations correctly without revealing their inputs, this creates a system where even if an attacker compromises some shares below the threshold, they learn nothing about the private key.


### Real-World Implementations


In practice, the most common consumer implementation is the 2-of-2 model, where your device holds one share and the service provider holds another. Both must cooperate to sign transactions, creating a simple but effective security model, though it does create dependency on the provider's availability.


For users seeking more flexibility, the 2-of-3 model has become the industry standard. Here, shares are distributed between the user's device, the service provider, and a backup recovery service. Any two shares can sign transactions, providing redundancy if one party is unavailable while maintaining security if one share is compromised.


Enterprise deployments often use more complex arrangements like 3-of-5 models, distributing shares across multiple user devices, service providers, and hardware security modules. While this increases coordination complexity, it provides the granular control and redundancy that institutions require for managing large treasuries.


### Network Communication in MPC


The often-overlooked aspect of MPC is how network communication affects real-world performance. During signing, parties must complete multiple rounds of communication: generating and broadcasting commitments, exchanging revealed values, computing partial signatures, and finally combining them into the final signature. Each round requires all participating parties to be online and responsive.


Network latency directly impacts signing speed. A transaction might complete in 2-5 seconds on good connections but take 10-30 seconds on poor ones. This is why MPC wallets often feel slower than hardware wallets, which sign instantly once connected. It's also why geographic distribution of shares, while good for security, can significantly impact performance.


## MPC Wallets vs Other Wallet Types


### MPC vs Hardware Wallets


Hardware wallets like[Ledger](https://www.ledger.com/) and[Trezor](https://trezor.io/) store the complete private key in a secure hardware element, requiring physical device access for every transaction. This creates a single point of failure if the device is lost or damaged, though it eliminates the need for network coordination. MPC wallets, by contrast, have no complete key anywhere, can sign transactions from multiple devices, and offer built-in redundancy through distributed shares. The trade-off is the requirement for coordination between parties and dependence on network connectivity.


### MPC vs Multisig Smart Contract Wallets


The distinction between MPC and multisig wallets like[Safe](https://safe.global/) (formerly Gnosis Safe) often confuses newcomers, but they operate at fundamentally different layers. Multisig wallets use smart contracts requiring multiple signatures, with all signers and thresholds visible on-chain. This transparency can be a security risk for high-value accounts but provides auditability. They also incur higher gas fees due to on-chain verification but support complex transaction batching and conditional logic.


MPC wallets operate at the cryptographic layer, keeping the signing structure completely private. They generate standard transactions indistinguishable from single-signature transactions, resulting in lower fees. However, they're limited to single transaction signing and can't implement complex logic like spending limits or time-locked transactions that smart contract wallets handle easily.


It's worth noting that newer wallet architectures are emerging that challenge this traditional trade-off. Keystore-based wallets, for instance, use cryptographic proofs to achieve both the privacy of MPC and the flexibility of multisig. These hybrid approaches store only a root hash on-chain while enabling complex access policies verified through zero-knowledge proofs, suggesting that the rigid categorization between MPC and multisig may soon become obsolete.


### MPC vs Traditional EOA Wallets


Traditional EOA wallets remain the simplest option: one private key controls everything. This simplicity is both their strength and weakness. While universally supported and requiring no coordination, the complete loss of funds if the key is compromised makes them unsuitable for large holdings or organizational use. MPC wallets evolved specifically to address this critical vulnerability while maintaining reasonable usability.


## The Emergence of Hybrid Approaches


As the limitations of both MPC and multisig wallets have become apparent, a new generation of wallet architectures is emerging that attempts to combine the best of both worlds. These hybrid approaches recognize that MPC's privacy and efficiency are valuable, but so are multisig's flexibility and on-chain verifiability.


One such approach uses cryptographic proofs to maintain privacy while enabling complex access policies. Instead of exposing all signers on-chain like traditional multisig, or requiring real-time coordination like MPC, these systems store only a cryptographic commitment (a root hash) on-chain. When users want to make transactions, they generate proofs showing their action satisfies the access policy without revealing the policy itself.


This architecture delivers several advantages that neither MPC nor multisig can achieve alone. The access control remains completely private. Observers can't see who the signers are or what the approval structure looks like. Gas costs stay low since the on-chain footprint is minimal. Yet unlike MPC, these systems can implement complex policies like spending limits, time-based restrictions, or role-based access controls that are verified cryptographically.


For example, Stackup (which uses a keystore architecture rather than MPC) enables teams to define sophisticated access policies such as multi-user permissions, social recovery, spending limits while keeping the entire configuration private and gas-efficient. This represents a different evolutionary path from both MPC and multisig, one that maintains the sovereignty of smart contract wallets while achieving the privacy and efficiency traditionally associated with MPC.


While this article is on MPC, most teams will find that hybrid approaches are superior to MPC, and the general recommendation is often to use a wallet like Stackup from the beginning instead of either MPC or a traditional multisig.


## Security Benefits and Trade-offs of MPC Wallets


### Advantages


The elimination of single points of failure represents MPC's core security innovation. According to[Chainalysis' 2024 Crypto Crime Report](https://www.chainalysis.com/chainalysis-crypto-crime-report-2024/) , over $2 billion was lost to private key compromises, risks that MPC wallets inherently mitigate by ensuring no complete key exists anywhere. This distributed architecture also enables flexible access control without exposing the structure on-chain, crucial for institutions that don't want to broadcast their treasury management practices.


Another underappreciated advantage is key rotation without address changes. Unlike traditional wallets where changing keys means moving to a new address, MPC systems can refresh key shares periodically while maintaining the same wallet address. This preserves transaction history and eliminates the need to update deposit addresses with exchanges and partners.


### Limitations


However, MPC wallets come with their own set of challenges. Network dependency is perhaps the most visible limitation—all participating parties must be online and able to communicate during signing. Network failures or unavailable parties can delay or prevent transactions entirely. This makes MPC unsuitable for scenarios requiring guaranteed transaction timing.


Additionally, while[2024 crypo exploits](https://www.chainalysis.com/chainalysis-crypto-crime-report-2024/) , primarily had to do with private keys, access control issues have been the primary culprit of over[$3.1 billion lost in the first half of 2025 alone](https://cointelegraph.com/news/crypto-losses-hit-3-1b-in-2025-as-access-control-fails) . While MPC helps control access, it is less comprehensive and more centralized than smart contract based options.


Trust assumptions represent another consideration. Most consumer MPC wallets require trusting the wallet provider with one or more key shares. While the provider can't steal funds alone, they become a critical dependency for wallet access. If the provider disappears or is compromised, users may face significant challenges recovering their funds.


The inability to batch transactions or implement complex logic limits MPC's utility for sophisticated DeFi operations. Each transaction requires the full MPC signing ceremony, making it inefficient for users who need to interact with multiple protocols in sequence. Smart contract wallets handle these scenarios far more elegantly.


Recovery complexity also deserves attention. While MPC wallets offer recovery options, the process is more complex than simply importing a seed phrase. Users must understand the share distribution, maintain recovery shares securely, and follow specific procedures that vary by provider. This complexity can be daunting for less technical users.


## When to Use MPC Wallets


### Ideal Use Cases


Institutional treasury management represents perhaps the strongest use case for MPC wallets. Organizations need secure custody without the operational overhead of hardware wallets, and the ability to distribute signing authority across team members without on-chain visibility provides exactly what they need. The technology enables approval workflows while maintaining privacy about internal operations, though without the UX features of smart contract wallets such as gasless transactions and batched transactions.


Cross-platform access scenarios also benefit from MPC's distributed architecture. Users who need to access funds from multiple devices aren't tied to a single physical hardware wallet. The shares can be distributed across phones, laptops, and cloud services, providing flexibility without compromising security.


Social recovery scenarios showcase MPC's unique capabilities. You can distribute backup shares to trusted friends or family members who individually hold useless data but collectively can help recover your account. This creates a safety net without giving any single person control over your funds.


### When Other Solutions May Be Better


Teams that need an additional access of security, directly onchain, may benefit more from smart contract wallets. Additionally organizations that frequently need to make transactions to multiple parties benefit from smart contract wallets. MPC wallets cannot batch transactions and set up automations such as payment of gas fees, so additional management of the wallet is required.


High-frequency trading and MEV extraction require rapid transaction signing that MPC's communication overhead makes impractical. The multi-round protocol adds seconds to each signature, which can mean missed opportunities in fast-moving markets. Traditional hot wallets or specialized trading infrastructure serve these needs better.


Complex DeFi operations often require transaction batching, conditional logic, or sophisticated access controls that MPC wallets simply can't provide. If you need to set spending limits, create approval hierarchies, or execute complex transaction sequences, smart contract wallets offer far more flexibility.


For users seeking maximum decentralization and self-sovereignty, MPC may not align with their philosophy. The reliance on wallet providers and network coordination introduces dependencies that purists find unacceptable. Hardware wallets or carefully managed EOAs remain the gold standard for complete self-custody.


## Popular MPC Wallet Providers (2025): Detailed Comparison


### Enterprise Solutions


Fireblocks has established itself as the leading institutional MPC provider, serving exchanges, payment companies, and large funds. With SOC 2 Type II certification and insurance coverage up to $150 million, they offer the compliance and security guarantees institutions require. Their customizable threshold models (typically 3-of-4) and direct custody support for over 50 blockchains justify their enterprise pricing starting at $150,000 annually.


Copper (formerly Copper.co) focuses on institutional investors and crypto funds, offering unique features like real-time settlement and off-exchange trading through their ClearLoop network. Their 3-of-4 threshold model integrates with hardware security modules for additional security, and their 0.15% custody fee aligns with traditional asset management pricing models.


### Consumer Solutions


ZenGo has reimagined MPC for retail users by eliminating seed phrases entirely. Their 2-of-2 model splits shares between the user's device and ZenGo's servers, using biometric authentication and email-based recovery to create a user experience that feels more like a traditional app than a crypto wallet. The free tier covers basic usage, with premium features available for $99.99 annually.


Coinbase Wallet's MPC feature integrates seamlessly with the broader Coinbase ecosystem, offering direct fiat on/off ramps and exchange integration. Their 2-of-3 model distributes shares between the user's device, Coinbase's infrastructure, and a backup service, providing redundancy while maintaining the user experience Coinbase users expect.


### Developer Platforms


Lit Protocol takes a different approach, offering programmable MPC for DApp developers. Rather than providing a wallet directly, they enable developers to build applications with conditional access control via smart contracts. Their decentralized node network uses a variable threshold (typically 2/3 of network nodes), with a pay-per-signature pricing model that aligns costs with usage.


Web3Auth focuses on bringing Web2 users into Web3 by enabling social login for blockchain applications. Their non-custodial architecture maintains user sovereignty while providing familiar OAuth login flows. At $0.02 per monthly active user after the free tier, they've found a pricing model that scales with application success.


### Cost Analysis


When evaluating MPC providers, the total cost of ownership extends well beyond subscription fees. Setup fees range from zero for consumer solutions to $50,000 for enterprise deployments. Monthly or annual fees vary from free to over $200,000 depending on features and support levels. Transaction or signature fees add $0.001 to $0.50 per operation.


Hidden costs often surprise organizations. Integration development typically requires 40-200 engineering hours. Compliance audits can cost $10,000 to $50,000. Training and onboarding consume 20-100 hours of team time. Backup and recovery infrastructure adds variable costs depending on redundancy requirements. These hidden costs can easily exceed the visible subscription fees, making thorough planning essential.


## Setting Up Your First MPC Wallet: Step-by-Step Guide


### Consumer Setup (Using ZenGo as Example)


Setting up a consumer MPC wallet takes just 5-10 minutes and requires minimal technical skill. After downloading the app from your device's app store, you'll create an account using your email address. The app then guides you through biometric setup, enabling FaceID or fingerprint authentication for transaction approval.


Recovery setup is crucial and unique to ZenGo's approach. You'll verify your email, create a 3D face map for biometric recovery, and optionally save an encrypted recovery file. Once complete, you can fund the wallet using built-in purchase options or by transferring crypto from another wallet. We strongly recommend performing a test recovery on a second device to ensure you understand the process before storing significant funds.


### Enterprise Setup (Generic Process)


Enterprise MPC deployment is a multi-month project requiring careful planning and execution. The provider selection phase alone takes 2-4 weeks, involving security audits, legal review of contracts, and technical integration assessment.


Implementation follows, typically requiring 4-8 weeks for API integration, key ceremony planning, share distribution setup, and HSM configuration if applicable. This phase demands close coordination between security, development, and operations teams.


Testing cannot be rushed, usually requiring 2-3 weeks for testnet transactions, recovery drills, load testing, and security penetration testing. Only after thorough testing should you proceed to deployment, which itself takes 1-2 weeks for gradual rollout, team training, documentation finalization, and monitoring setup.


The total timeline of 2-4 months may seem excessive, but rushing enterprise MPC deployment risks costly mistakes. Each phase builds on the previous one, and shortcuts invariably lead to problems when managing significant assets.


## Common MPC Wallet Myths Debunked


### Myth 1: "MPC Wallets Are Unhackable"


While MPC eliminates single points of failure, claiming they're unhackable oversimplifies security. Vulnerabilities can emerge from compromised key generation ceremonies where initial randomness is subverted. Side-channel attacks during signing can leak information about shares. Social engineering targeting multiple share holders simultaneously remains a threat. Implementation bugs in MPC protocols have been discovered and patched. Security requires constant vigilance, not magical technology.


### Myth 2: "MPC Is Always Better Than Multisig"


Each technology has distinct advantages that make it superior for different use cases. MPC provides better privacy since the signing structure isn't visible on-chain, lower gas costs since transactions appear as standard single-signature operations, and works on all blockchains including Bitcoin. Multisig offers on-chain transparency that some users prefer for auditability, doesn't require network coordination between signers, and supports complex logic like spending limits and time-locks that MPC cannot implement.


### Myth 3: "All MPC Implementations Are Equal"


Massive variations exist between implementations. Protocol security varies significantly between older GG18 implementations and modern CGGMP deployments. Some providers refresh shares regularly while others never do. Network architecture ranges from fully centralized to distributed across independent nodes. Recovery mechanisms vary from non-existent to sophisticated multi-party procedures. These differences materially impact security and usability.


### Myth 4: "MPC Wallets Don't Need Backup"


This dangerous misconception leads to fund loss. Share corruption can occur from storage failures or software bugs. Providers can disappear due to bankruptcy or regulatory action. Devices containing shares are regularly lost or damaged. Recovery shares need secure storage separate from operational shares. Backup remains as critical for MPC as for any other wallet type.


## Security Incidents and Lessons Learned


### Historical MPC-Related Incidents


While no major MPC wallet has suffered direct cryptographic compromise, several incidents provide valuable lessons. The 2023 Multichain bridge incident, while not strictly an MPC wallet, used MPC for cross-chain operations. When the CEO who held critical shares went missing, $126 million became locked indefinitely. The lesson is clear: avoid single person dependencies in share distribution.


In 2024, an unnamed exchange discovered a vulnerability in their custom MPC implementation during internal review. They patched it before exploitation, but it highlights the importance of using audited, standard protocols rather than rolling your own cryptography.


### Security Best Practices


For individual users, security starts with enabling all available authentication factors and testing recovery procedures quarterly. Monitor signing requests carefully—if a transaction looks different than expected, investigate before approving. When possible, use separate devices for different shares, and ensure recovery shares are truly separated across different locations or trusted people.


Organizations face additional complexity. Regular key refresh ceremonies reduce the risk of share compromise over time. Signing policies should match operational needs without creating unnecessary friction. Anomalous signing patterns might indicate compromise, making monitoring essential. Detailed audit logs support both security and compliance requirements.


Planning for employee turnover deserves special attention. When someone with signing authority leaves, shares should be refreshed immediately. Time-locks for large transactions provide an additional safety net, allowing intervention if unauthorized transactions are detected. Geographical distribution of shares protects against regional incidents but must be balanced against performance needs.


## Performance Benchmarks and Limitations


### Signing Speed Reality


Performance varies dramatically across wallet types and implementations. Hardware wallets typically complete signing in 1-2 seconds once connected, while software EOAs sign almost instantly. MPC wallets face inherent delays from network coordination: 2-of-2 configurations typically take 2-5 seconds, while 3-of-5 deployments can require 5-15 seconds. Multisig wallets have the widest variance, from one minute for well-coordinated teams to 30 minutes or more when signers are distributed across time zones.


### Throughput Constraints


MPC wallets face inherent throughput limitations that make them unsuitable for certain applications. Consumer MPC implementations typically handle 10-20 transactions per minute, while enterprise solutions might reach 50-100 transactions per minute. Even highly optimized deployments rarely exceed 500 transactions per minute. These constraints make MPC inappropriate for high-frequency trading, MEV extraction, automated market making, or mass payment processing where thousands of transactions must be processed quickly.


### Geographic Impact on Performance


The physical distribution of shares significantly impacts performance. When all shares reside in the same region, MPC adds only 1-2 seconds of overhead. Cross-continental distribution increases this to 3-5 seconds, while truly global distribution can add 5-10 seconds per transaction. Organizations must carefully balance the security benefits of geographic distribution against their performance requirements.


## Regulatory Landscape and Compliance


### Current Regulatory Status (2025)


The regulatory treatment of MPC wallets has crystallized significantly over the past two years. In the United States, the SEC now recognizes qualified custodian status for some MPC providers, though FinCEN requires the same KYC/AML procedures as traditional custodians. State-level money transmitter licenses remain a patchwork of requirements that providers must navigate carefully.


The European Union's MiCA framework explicitly addresses MPC custody, requiring segregation of client assets and mandating either insurance coverage or capital requirements proportional to assets under custody. This regulatory clarity has accelerated institutional adoption across Europe.


Asia-Pacific presents a diverse regulatory landscape. Singapore recognizes MPC under its Payment Services Act, providing clear guidelines for providers. Japan requires local key share storage, reflecting data sovereignty concerns. Hong Kong includes MPC in its VASP licensing framework, treating it equivalently to other custody solutions.


### Compliance Considerations


Organizations using MPC must address multiple compliance requirements. Audit requirements typically start with SOC 2 Type II certification for financial services, though some jurisdictions require additional certifications. Insurance coverage between $10 million and $150 million is standard, with requirements scaling with assets under management.


Data residency requirements complicate international deployments. Some jurisdictions mandate local share storage, preventing fully distributed architectures. Recovery procedures must be documented and tested regularly to demonstrate reliable access for compliance purposes. Detailed audit trails of all signing events support both security and regulatory requirements.


## Troubleshooting Common MPC Wallet Issues


### When Transactions Won't Sign


"Timeout during signature generation" usually indicates network connectivity issues between shares. Check your internet connection and retry with better connectivity. If the problem persists, verify all share holders are online. Providers with redundant communication channels experience fewer such issues.


"Insufficient shares available" means required share holders are offline or unresponsive. Contact missing share holders or use backup shares if available. This highlights the importance of maintaining backup shares and implementing automated signing where appropriate to reduce dependence on human availability.


"Invalid signature produced" suggests share corruption or protocol mismatch. This serious issue requires initiating a share refresh ceremony immediately. Regular share refresh and version consistency checks prevent most occurrences.


### Recovery Challenges


"Cannot access wallet after device loss" typically results from insufficient recovery planning. Contact your provider's support with transaction history proof to initiate recovery procedures. This scenario emphasizes the importance of testing recovery procedures before they're needed and maintaining multiple recovery options.


"Recovery share not recognized" indicates recovery data corruption or version incompatibility. Provider support can sometimes resolve this with additional verification, but it highlights the need to store recovery data in multiple locations and verify integrity periodically.


### Performance Problems


When transactions take 30+ seconds, geographic distribution of shares is usually the culprit. Consider optimizing share placement based on usage patterns or upgrading network infrastructure. For organizations, regional share distribution aligned with operational centers often provides the best balance of security and performance.


## Migrating to or from MPC Wallets


### Strategic Migration Planning


Migration requires careful orchestration to avoid fund loss or operational disruption. The assessment phase involves cataloging all current wallet addresses, documenting signing procedures, identifying integration points, and calculating migration costs including both direct expenses and opportunity costs from operational disruption.


Parallel running allows testing without risk. Set up the MPC solution alongside your existing system, test with small amounts to verify functionality, train team members on new procedures, and document everything. This phase typically reveals unexpected challenges that would be costly to discover after full migration.


Gradual migration reduces risk significantly. Start by moving 10% of funds, monitor for issues for at least a week, then increase the migration percentage weekly. This approach allows quick rollback if problems emerge while building team confidence with the new system.


The completion phase involves final fund transfer, updating all integrations, decommissioning old wallets securely, and conducting a post-migration audit. Don't rush this phase—thorough verification prevents future problems.


### Migration Strategies by Source


When migrating from hardware wallets to MPC, generate new MPC wallets rather than attempting to import seeds, which would compromise MPC's security model. Transfer funds in batches to verify control at each step. Update deposit addresses with all exchanges and partners. Maintain hardware wallet access for emergency situations until you're completely confident in the MPC setup.


EOA or[MetaMask](https://metamask.io/) migration follows similar principles. Create the MPC wallet with the same derivation path if possible to maintain address consistency. Prioritize high-value asset transfers first when risk is lowest. Update all DApp connections methodically. Keep the EOA accessible for legacy contracts that require specific addresses.


Multisig migration requires full approval from all signers, making it the most complex migration type. Consider keeping the multisig for large cold storage while using MPC for operational wallets. Document new approval workflows carefully since they'll differ significantly from multisig procedures.


When migrating from MPC to other solutions, ensure all share holders agree to the migration plan. Generate new wallets in the target system—never attempt to export MPC private keys, which defeats the entire security model. Transfer in stages to verify control at each step. Maintain MPC access until the migration is confirmed complete and all integrations are updated.


## Expert Recommendations by Use Case


### For Different Organization Sizes


Startups with less than $10 million in crypto assets and limited team interaction may benefit from MPC wallets, complemented by a hardware wallet for treasury reserves. However, many startups will end up needing the advanced features of hybrid wallets like smart accounts.


Mid-size companies managing $10-100 million should adopt a hybrid approach. Use 3-of-5 MPC for daily operations where multiple team members need access, but keep large holdings in multisig smart wallets with time-locks. This provides operational flexibility without compromising security for reserves.


Enterprises with over $100 million require institutional-grade solutions. Deploy enterprise MPC with HSM integration and comprehensive insurance. Maintain air-gapped multisig for cold storage that's accessed rarely. This defense-in-depth approach satisfies regulatory requirements while protecting against diverse threat vectors.


### For Specific Use Cases


DeFi protocols should generally prefer smart contract wallets over MPC. The need for on-chain transparency, complex logic implementation, and transaction batching makes multisig smart wallets with time-locks more suitable. MPC might serve for cross-chain operations where smart contract wallets aren't available.


Individual power users benefit from a diversified approach. Use MPC for convenient daily transactions where the balance between security and usability matters most. Keep medium-term holdings in hardware wallets for better isolation. Store long-term holdings in multisig or geographically distributed seeds for maximum security.


## Conclusion: MPC is falling out of favor, and smart contract wallets are becoming more popular


As the crypto ecosystem matures, we're witnessing a shift from one-size-fits-all solutions toward specialized tools optimized for specific use cases. MPC wallets exemplify this evolution, offering unique benefits that make them indispensable for certain users while being unnecessary or even counterproductive for others.


The key to making informed decisions about wallet technology lies in understanding not just individual approaches but the entire ecosystem surrounding them. Provider stability matters as much as cryptographic security. Regulatory compliance requirements can dictate technology choices. Integration complexity affects total cost of ownership far more than subscription fees. Long-term viability depends on the provider's business model and the broader adoption of standards.


The emergence of hybrid architectures that combine MPC's privacy with multisig's flexibility—such as keystore-based wallets that use cryptographic proofs to enable complex policies without on-chain exposure—suggests we're moving beyond traditional categorizations. These innovations demonstrate that the future isn't about choosing between MPC or multisig, but about finding new architectures that transcend their individual limitations.


For teams evaluating wallet infrastructure, the best solution is rarely a single technology. Instead, thoughtful combinations of MPC, multisig, hardware wallets, smart contracts, and emerging hybrid approaches—each serving its purpose in a comprehensive security strategy—provide the resilience modern crypto operations require.


As we look toward the future, innovations like[account abstraction](https://www.stackup.fi/resources/what-is-account-abstraction) , social recovery, decentralized MPC networks, and proof-based access control will continue blurring the lines between different wallet types. The winners won't be those who pick the "best" technology, but those who understand how to combine different approaches to match their specific needs, risk tolerance, and operational requirements.


Stay informed about developments in wallet technology, test new solutions thoroughly before committing significant assets, and always prioritize security fundamentals regardless of the underlying technology. In the end, the best wallet is the one that keeps your assets safe while enabling you to achieve your goals—whether that's MPC, multisig, hardware, or increasingly, innovative hybrids that combine the best of multiple approaches.


‍
