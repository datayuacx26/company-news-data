---
schema_version: "1.0.0"
document_id: "0e7c329d6683be82f0a8f5f9c115d37ffa0a07cccda67c8ba51ff1a0e49189ed"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/the-dark-side-of-defi-cross-chain-bridge-hacks"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:f7941809422e9cb5bff734be55221762cda43b7b6648ebe0f4d815db0cf0e43e"
---

# The Dark Side of DeFi: Cross-Chain Bridge Hacks

Every day, DeFi users rely on bridges to move millions in assets from one chain to another. Currently, there are more than[70 cross-chain bridges deployed](https://dezentralizedfinance.com/cross-chain-bridges/) , with a total of over $25B locked and daily transaction volumes in the millions. Synapse, one of the most popular cross-chain bridges, recently shared that its transaction volume alone has surpassed $5 billion. With the number of L1 and L2 chains being launched and the yield opportunities offered, the number of cross-chain bridges will only continue to grow.


However, behind the scenes, assets can't simply move between chains: the main chain retains custody, and the user receives a debt token. Once the user burns this debt, a communicator tells the custodian on the main chain that funds can be released. Ultimately, bridges combine multiple structures: a custodian, debt issuer, and an oracle. Given this, there are several attack vectors that make bridges especially vulnerable.


‍


### **Custodians: Hijacking Privileged Addresses**


In some situations, there are privileged addresses that have access to assets. With this in mind, there are two approaches an attacker could take: gaining control over these addresses, or changing which addresses are privileged. For example, when a cross-chain communication contract also has the power to change custody addresses, without careful access control, it may open up this possibility to anyone.


In August 2021,[Poly Network was attacked](https://www.coindesk.com/markets/2021/08/10/cross-chain-defi-site-poly-network-hacked-hundreds-of-millions-potentially-lost/) for $610M worth of digital assets within an hour. Prior to the incident, Poly Network had transferred $10B in assets and had a total of nearly $1B TVL. In this situation, the perpetrator was able to use the cross-chain communication contract to replace the address that controls the funds, allowing them to effectively gain control. Although all assets were returned over the next 15 days, it certainly showcased the importance of addressing these types of security vulnerabilities.


‍


### **Custodians: Crafting Proofs**


For other custodians, proofs must be presented during the withdrawal process. In these situations, an attacker would need to craft valid proofs to pass the verification process. An attacker might use one valid proof to create a different proof that would pass verification.


‍


In October 2021, a security researcher and whitehat hacker Gerhard Wagner[disclosed a vulnerability](https://www.securityweek.com/researcher-earns-2-million-critical-vulnerability-polygon#:~:text=Security%20researcher%20Gerhard%20Wagner%20earned,times%2C%20with%20different%20exit%20IDs.) involving the Polygon Plasma Bridge. As one of two bridges Polygon offers, the Plasma Bridge is generally considered the more secure bridge. The vulnerability would allow an attacker to deposit a specific amount, then use this valid proof to craft more, allowing them to withdraw from the custodian an additional 223 times. This effectively put the entire amount of funds the bridge’s DepositManager was holding—approximately $850M—at risk. Ultimately, Polygon paid out $2M as part of the bug bounty program launched on Immunefi.


‍


### **Custodians: Processing and Verification Skipped**


In another scenario, the attacker would aim to have the custodian emit a deposit event when it didn't truly deposit. In some situations, the bridge assumes that "locking" and "transferring" can be skipped when it comes to a specific token.


As illustrated in the image below, consider there are two paths: depositWrapped and deposit. In the case of depositWrapped, processing and verification of the token occurs; in contrast, with the deposit pathway, core logic assumes the special tokens are already processed. The attacker is then able to call **deposit()** with the special token—and processing is completely skipped.


‍


### **Attacking the Debt Issuer**


The debt issuer can also be attacked, for example, by arbitrarily minting a debt token.


Remember: when the user burns their debt tokens on the other chain, the communicator tells the custodian that funds can be released. What if you could trick the communicator into forwarding invalid messages: for example, to mint debt tokens?


In February this year, the popular cross-chain bridge Wormhole[experienced an attack](https://cointelegraph.com/news/wormhole-token-bridge-loses-321m-in-largest-hack-so-far-in-2022) that resulted in a loss of $320M. Ultimately, the attackers were able to forge the verification signature and use it to sign their own cross-chain request and obtain a huge amount of funds.


‍


One approach would be to bypass signature verification by deploying a fake verification contract. The attacker initiates debt issuance using an invalid signature and a fake verification contract—then, once verification is passed, debt tokens are minted and can be withdrawn into the native chain.


‍


### **Attacking the Communicator**


In this case, the attacker’s goal is to trick the communicator into forwarding invalid messages—specifically in regards to minting or redeeming debt tokens. Imagine the communicator has a bug where it validates only the first contract address of events. Deposit events can then be faked via a malicious contract that emits an identical deposit event, ultimately allowing the creation of excessive debt tokens.


‍


Another way the communicator could be attacked is by changing what the data source is saying, or could be thought of as polluting the data source. Typically, this isn't feasible because most oracles reflect real-world data—but what if the source of truth is another blockchain? In this case, history could be changed via a 51% attack. While a 51% attack is expensive, the growing amount of assets locked in bridges creates a strong incentive for the attacker.


‍


### **Attacking the Interface**


Finally, there can be attacks on the interface. While this isn't specific to the bridge, it pertains to whether there's an interface for users to deposit. For example, what if a contract allowed funds to be deposited from other addresses with the permit/allowance to do so?


Contracts have the ability to deposit funds from other addresses with allowance. They’re different from attacks on bridges because the bridge projects themselves don’t lose funds, but the users who have approved their funds. Consequently, the issue can be fixed simply by the user revoking approval.


In the situation below, a user approved a contract that allowed a third party to deposit funds into another address via **depositWithPermit()** . The contract simply calls the token contract permit function, and if the token contract has a fallback function that doesn’t revert, the attacker successfully gains access to the user’s tokens, deposits funds under their name, and is then free to withdraw.


Another possible attack vector is if the interface doesn't sanitize input and executes function calls to any contract with any data. The attacker would then craft a **transferFrom()** . In this situation, users can prevent being exploited by “revoking approval”. However, while this protects the user, revoking approval doesn’t fix the vulnerability.


‍


### **Bridges Have Broad Attack Surfaces**


In summary, bridges have a large attack surface that is difficult to secure. As demonstrated through these examples, there are often many more vulnerabilities when compared to a smart contract platform on a specific chain, and ultimately a lot that can go wrong.


‍


So what can be done? Following some best practices can help mitigate the chance of funds being lost while using bridges.


‍


**For dApp developers:**


- Whenever possible, understand the risk associated with bridging and avoid bridging for users without their consent.
- Check whether the assets in the bridge are insured, and if not, whether insurance could be set up for them.
- Avoid infinite approvals when interacting with external protocols—only approve the amount that you absolutely need during the interaction.


‍


**For users:**


- Only use bridges that have been audited by several highly reputable security firms.
- Check the audit reports to see if issues that were found have actually been fixed, not simply acknowledged. If you aren’t familiar with how to interpret audit results, check out one of our recent posts explaining[how to read an audit report](https://quantstamp.com/blog/how-to-read-an-audit-report) .
- Check whether the bridge has a bug bounty program in place. Bounty programs incentivize white hat hackers to review the code and disclose vulnerabilities before they are exploited by bad actors.


‍


While bridges present numerous risks and vulnerabilities, the demand to transfer assets from one blockchain to another is not going away. Bridges give end users access to a diverse range of DeFi protocols, and allow developers to easily launch dapps across multiple blockchains. Following a surge of growth at the end of 2021, it is likely this trend will continue. However, as more cross-chain bridges launch and TVL continues to rise, exploits will also continue to occur. By being aware of the risks involved and employing the best practices outlined above, both bridge users and blockchain developers can hopefully experience more of the benefits of bridging assets while avoiding some of the pitfalls.


‍
