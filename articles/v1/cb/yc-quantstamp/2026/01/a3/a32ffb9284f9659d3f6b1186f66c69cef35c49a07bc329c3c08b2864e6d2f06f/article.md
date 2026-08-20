---
schema_version: "1.0.0"
document_id: "a32ffb9284f9659d3f6b1186f66c69cef35c49a07bc329c3c08b2864e6d2f06f"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/engineering-smart-contract-families-for-solidity"
published_at: "2026-01-15T19:52:14+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:23:32.114893+00:00"
content_hash: "sha256:66402ae10f64e267d2824e085dac74af75a650efbf4558315a23c14973d0268a"
---

# Engineering Smart Contract Families for Solidity

# **Engineering Smart Contract Families for Solidity**


*This blog post is based on the paper*[Smart Contract Families in Solidity](https://www.computer.org/csdl/proceedings-article/cascon/2024/10838108/23zYAlePDQA) *.*


*Scripts and examples are available in the*[Github repository](https://github.com/quantstamp/research-spl-poc/tree/examples) *.*


Decentralized applications (dApps) (e.g., DEXes) increasingly span multiple Ethereum-compatible chains, such as a number of L2s. Although these chains are intended to be compatible with the Ethereum Virtual Machine (EVM), subtle differences in opcode implementations can significantly alter smart contract behavior and security. This poses an important question: *how can developers efficiently code and manage smart contracts targeting different chains?*


## **Simplifying Multi-Chain Development with Feature Flags**


This article discusses a method for smart contract development for multiple chains simultaneously. It is based on *smart contract families,* i.e., similar blockchain applications built from shared assets, such as Solidity source code. Our goal is to help developers build contract families faster and streamline code audits. The presented method allows for adding or excluding functionalities based on the desired features (requirements) by applying feature flags over Solidity files to enable conditional compilation.


### **The Challenge: One Solidity, Many Chains**


Ethereum remains the dominant smart contract platform, but numerous L2 networks, like Optimism, Base, and Zircuit, are gaining traction. Each of these chains aims to scale Ethereum, often by altering opcode semantics due to technical constraints (e.g., zero-knowledge proofs).


Consider the opcode` selfdestruct` , historically used to clean up contract’s memory and transfer their remaining Ether. Ethereum and several rollups differ in how they handle this opcode:


Network Support for` selfdestruct`


Ethereum ✓ (deprecated)


Optimism ✓


Base ✓


Zircuit ✓


Polygon zkEVM ✗ (uses` sendall` )


Scroll ✗ (disabled, transactions revert)


zkSync Era ✗ (compile-time error)


### **Traditional Approaches and Their Limitations**


#### **Clone and Own**


Developers traditionally manage multiple variants by creating separate versions of the contract for different blockchains, each with its own implementation. This approach, known as *Clone and Own* involves significant redundancy and maintenance overhead, as each version must be independently audited, tested, and updated. For example, consider these two versions. One for the Ethereum mainnet:


> function retire() external {


> require(msg.sender == receiver);


> selfdestruct(receiver);


> }


‍


and one for an L2 that doesn’t support` selfdestruct` :


> function retire() external {


> require(msg.sender == receiver);


> receiver.transfer(address(this).balance);


> }


#### **Conditional Execution**


Another option involves embedding runtime checks within contracts to adapt behavior based on the blockchain environment dynamically. While this approach reduces redundancy, it increases contract bytecode size and operational costs due to additional runtime logic and gas consumption. Furthermore, it may not always be feasible to apply this approach. For example, some compilers may not support specific opcodes, and a compilation attempt of the Solidity code below could result in an error.


> function retire() external {


> require(msg.sender == receiver);


> if (block.chainid == 1) { // Ethereum Mainnet


> selfdestruct(receiver);


> } else {


> receiver.transfer(address(this).balance);


> }


> }


### **The Solution: Smart Contract Families with Feature Flags**


This solution draws from Software Product Line (SPL) engineering. Rather than maintaining multiple similar contract versions or costly runtime checks, it relies on conditional compilation through preprocessor directives, enabling tailored and efficient smart contract families. There are multiple ways of realizing this approach, but here we show how it can be done through the C language preprocessor.


#### **How It Works**


Developers embed conditional compilation flags directly into Solidity code, as shown below:


> function retire() external {


> require(msg.sender == receiver);


> #ifdef CONFIG_MAINNET


> selfdestruct(receiver);


> #else


> receiver.transfer(address(this).balance);


> #endif


> }


‍


The C language preprocessor is a tool that manipulates the source code before the actual compilation stage. Preprocessor directives, such as` #ifdef` and` #else` , allow developers to conditionally include or exclude sections of code based on predefined flags. These flags, like` CONFIG_MAINNET` in the example above, are symbolic constants that can be defined during the compilation process. When the preprocessor encounters a` #ifdef` directive, it checks if the specified flag is defined. If the flag is defined, the code block following` #ifdef` is included; otherwise, the code block following` #else` (if present) is included. This mechanism enables the generation of different versions of the code from a single source file by changing the defined flags during compilation, without altering the source code itself.


This snippet above illustrates how contracts can be tailored for Ethereum mainnet (` selfdestruct` ) or alternative chains (using` transfer` ). Compilation for specific chains is easily managed using flags. For example, to pick the Ethereum mainnet version, we need to pass the flag` CONFIG_MAINNET` :


> gcc -xc -E -P -D CONFIG_MAINNET RetirableSpl.sol -o Retirable.sol


‍


And for chains not supporting` selfdestruct` , omit the` CONFIG_MAINNET` flag:


> gcc -xc -E -P RetirableSpl.sol -o Retirable.sol


### **Comparison**


The table below summarizes the pros and cons of each approach.


Clone and Own Conditional Execution Conditional Compilation


Bytecode Size 👍 🚫 👍


Execution Cost 👍 🚫 👍


Compilation Cost 👍 🚫 👍


Compile-time Errors 👍 🚫 👍


Maintenance Effort 🚫 👍 👍


Audit Cost 🚫 👍 👍


Ease of understanding the code 👍 🥸 🥸


Standard Blockchain tooling 👍 👍 🚫


‍


In practice, the Clone and Own approach may work well for up to a few products. Conditional execution, on the other hand, is not necessarily well-suited for the blockchain environment and may incur compilation problems.


Finally, conditional compilation scales well and allows for a streamlined smart contract development for multiple chains simultaneously. It leverages existing tooling, such as the C preprocessor flags, although this tooling hasn’t been used widely in dApps development. The scalability of this approach has been demonstrated in the development of the Linux kernel, spanning tens of thousands of features.


We encourage developers to try the conditional compilation approach in smart contract development to improve reusability, maintainability, and cost-efficiency, ensuring their dApps remain robust, secure, and optimized for any blockchain environment.


‍
