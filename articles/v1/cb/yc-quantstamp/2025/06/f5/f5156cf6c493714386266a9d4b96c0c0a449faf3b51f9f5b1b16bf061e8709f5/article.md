---
schema_version: "1.0.0"
document_id: "f5156cf6c493714386266a9d4b96c0c0a449faf3b51f9f5b1b16bf061e8709f5"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/will-eip-7702-affect-your-code"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:479e2a6a1eda704e3c1661dae2c7415eb7135fd626010ddca8e9047abf1f906f"
---

# Will EIP-7702 Affect Your Code?

## **Introduction**


The upcoming EVM hardfork, *Pectra* , amongst other changes, will implement[EIP-7702](https://eips.ethereum.org/EIPS/eip-7702) , a proposal introducing a new transaction type that allows Externally Owned Accounts (EOAs) to delegate—and later undelegate—their behavior to smart contracts. While this upgrade enhances flexibility, it also disrupts long-standing security assumptions in many deployed contracts. With the risk that malicious actors may exploit these changes once Pectra is enabled, it is crucial to assess whether your codebase might be negatively impacted.


## **Key Resources for a Deep Dive**


If you’re not already familiar with the details of EIP-7702, consider these resources:


- [EIP-7702](https://eips.ethereum.org/EIPS/eip-7702) : The official proposal outlining the technical changes.
- [Technical Deep Dive by lightclient | Devcon SEA](https://www.youtube.com/watch?v=_k5fKlKBWV4) : An in-depth analysis of the proposal.
- [Deep Dive into Ethereum 7702 Smart Accounts](https://www.youtube.com/watch?v=ZFN2bYt9gNE) : Discusses security risks, pitfalls, and testing strategies.


A detailed diagram from[The Red Guild](https://x.com/theredguild) (third resource above) helps visualize the delegation flow and its impact.


‍


## **Identifying Vulnerable Code Patterns**


To proactively assess your codebase, search for potential vulnerabilities in the following areas. For each pattern, a code example is provided to illustrate how your security assumptions might be challenged after the EIP-7702 upgrade.


### **1. Restricting Function Calls to EOAs Only**


**The Pattern:** Developers often enforce a restriction that only EOAs can call certain functions by checking if` msg.sender == tx.origin.`


**Example 1: Restricting function access to EOAs only**


> function sensitiveAction() external {


> // This check assumes that if msg.sender equals


> // tx.origin, the caller is an EOA.


> require(msg.sender == tx.origin, "Only EOAs allowed");


> // Perform sensitive action...


> }


‍


**Potential Issue:** An EOA might delegate its behavior to a contract. In such cases, both` msg.sender` and` tx.origin` could still reflect the EOA’s address, allowing the call to pass the check—even though a contract is now executing the call.


### **2. Differentiated Behavior Based on Caller Type**


**The Pattern:** Some contracts branch their logic based on whether the caller is a contract or an EOA by using functions like` isContract()` .


**Example 2: Handling caller differently based on whether it is a contract**


> function processUser(address user) external {


> if (isContract(user)) {


> // Process logic for contracts


> } else {


> // Process logic for EOAs


> }


> }


‍


> function isContract(address account) internal view returns (bool) {


> uint256 size;


> assembly {


> size := extcodesize(account)


> }


> return size > 0;


> }


‍


**Potential Issue:** If an EOA delegates to a contract,` isContract(user)` might unexpectedly return true, thereby routing the execution to logic intended for contracts even though the original entity was an EOA. For example, initial registrations of an address as an EOA or contract through such checks might lead to false and dangerous execution paths later on within a transaction with or without a delegation, respectively.


### **3. Unsafe External Call Assumptions**


**The Pattern:** External calls often assume that sending a transaction to an EOA will always succeed. With delegation, this behavior can change.


**Example 3: External call that assumes EOAs always return success**


> function safeTransfer(address recipient, uint256 amount) external {


> (bool success, ) = recipient.call{value: amount}("");


> require(success, "Transfer failed");


> }


‍


**Potential Issue:** If an EOA now delegates to a contract that does not implement a proper fallback or` receive()` function, the call might unexpectedly fail. This is problematic when the original recipient was assumed to be an EOA.


Another concern is that low level calls such as assembly static-call also return true for addresses with no code. This could really be problematic, especially if e.g. a ERC-165 interface check was initially done at some registration transaction that is then assumed to hold for low level calls later on.


### **4. Assumptions About address(this) Behavior**


**The Pattern:** Some systems assume that` address(this)` (i.e., the current contract) should never serve as a valid signer or be involved in other identity-specific logic.


**Example 4: Assumption that` address(this)` is not a valid signer**


> function verifySignature(bytes32 hash, bytes memory signature) external view returns (bool) {


> address signer = recoverSigner(hash, signature);


> // The assumption here is that the current contract should not be a valid signer.


> require(signer != address(this), "Invalid signer");


> return true;


> }


‍


**Potential Issue:** Under a delegation scenario, an EOA might delegate its signing operation to a contract. Consequently,` address(this)` could now be a valid signer.


## **The Broken Assumption: msg.sender == tx.origin**


A critical assumption that no longer holds is:


If` msg.sender == tx.origin` , then` tx.origin` is assumed to be an EOA.


**Scenario Example:** Imagine Bob, an EOA, sponsors a` setCodeTx` transaction that includes delegation authorization to contract D while calling a function in contract C. In this transaction, both` msg.sender` and` tx.origin` appear as Bob’s address, yet Bob’s behavior is influenced by contract D. This subtle change invalidates checks that rely on the assumption that such equality confirms an EOA-only context.


### **Potential Consequences**


- **Security Bypasses:** Attackers may bypass functions restricted to EOAs or contracts.
- **Flow Disruptions:** In multi-transaction sequences of operations, a change in address behavior (from EOA to contract and vice versa) might trigger errors.
- **Critical Operation Failures:** Operations like withdrawals, liquidations, NFT minting, or signature validations (e.g., EIP-1271) could fail, be negatively impacted, or be exploited via reentrancy and gas griefing attacks.


## **Recommended Approach for Issue Identification**


Internally, we used a pattern-matching approach to identify audited codebases with snippets of code matching patterns of interest, such as the following strings:


- ` tx.origin`
- ` isContract()`
- ` codesize() / extcodesize()`
- ` code.length`


**Note:** While including` address(this)` in your search might seem useful, it may also generate numerous false positives. Use contextual knowledge of your business logic to evaluate each match carefully.


Note that this set of items may not be exhaustive. Also, we ran this analysis only on` *.sol` files. This approach is a good starting point but a careful analysis should be done with knowledge of the expected business logic of the whole codebase before being able to assess with certainty that a given codebase may or not be negatively impacted by the activation of EIP7702.


‍


## **Conclusion**


The activation of EIP-7702 introduces subtle changes that can break long-standing security assumptions, leading to potential bypasses and critical operation failures. Adopting a “better safe than sorry” approach by reviewing your contracts is essential. With a comprehensive understanding of both the technical and business implications, you can safeguard your smart contracts against potential vulnerabilities in the post-Pectra era.


At Quantstamp, we stay ahead of emerging threats and best practices in smart contract security. We’ve closely tracked EIP-7702 due to its importance for account abstraction—a space we’ve actively contributed to, including co-authoring the ERC-6900 modular account standard. With a deep history of high-stakes audits and research, we’re equipped to help teams navigate the risks posed by this transition.


If you’re seeking guidance or review ahead of the Pectra upgrade, get in touch. Our team is here to support your security goals and help future-proof your web3 project. With a proven track record in blockchain security, Quantstamp is your partner in building secure, future ready systems.


‍
