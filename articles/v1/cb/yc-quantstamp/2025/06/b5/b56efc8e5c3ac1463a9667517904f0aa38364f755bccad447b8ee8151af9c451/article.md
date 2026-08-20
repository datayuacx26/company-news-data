---
schema_version: "1.0.0"
document_id: "b56efc8e5c3ac1463a9667517904f0aa38364f755bccad447b8ee8151af9c451"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/what-is-a-re-entrancy-attack"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:f5764dcff8a5e57b1004f0df4d388c6c4293636ab5958e21b80c3154b855d306"
---

# What is a Re-Entrancy Attack?

‍ *This post is based on an excerpt from Fundamentals of Smart Contract Security. To read more about vulnerabilities and smart contract security issues,*[buy the book on Amazon](https://www.amazon.com/Fundamentals-Smart-Contract-Security-Richard-ebook/dp/B07S8YF27G) *or*[Momentum Press](https://www.momentumpress.net/books/fundamentals-smart-contract-security) *.*


Computer scientists say that a procedure is *re-entrant* if its execution can be interrupted in the middle, initiated over (re-entered), and both runs can complete without any errors in execution. In the context of Ethereum smart contracts, re-entrancy can lead to serious vulnerabilities.


The most famous example of this was the DAO Hack, where $70million worth of Ether was siphoned off. More recently,[Ethereum’s Constaninople hard fork was delayed](https://blog.ethereum.org/2019/01/15/security-alert-ethereum-constantinople-postponement/) because a re-entrancy vulnerability was found at the last minute.


So what exactly is a re-entracy vulnerability? How does it work, and how can it be prevented?


### **Mechanism**


An example of a re-entrant process can be sending an e-mail. A user can start typing an e-mail using their favorite client, save a draft, send another e-mail, and finish the message later. This is a harmless example. However, imagine a poorly constructed online banking system for issuing wire transfers where the account balance is checked only at the initialization step. A user could initiate several transfers without actually submitting any of them. The banking system would confirm that the user’s account holds a sufficient balance for each individual transfer. If there was no additional check at the time of the actual submission, the user could then submit all transactions and potentially exceed the balance of their account. This is the main mechanism of the re-entrancy exploit which was used in the well-known DAO hack.


### **Real-world Example - The DAO Hack**


The DAO was a popular decentralized investment fund based on smart contracts. In 2016, the DAO smart contract accumulated over $150,000,000 (at the time) of ether. If a project that requested funding received sufficient support from the DAO community, that project’s Ethereum address could withdraw ether from DAO. Unfortunately for the DAO, the transfer mechanism would transfer the ether to the external address before updating its internal state and noting that the balance was already transferred. This gave the attackers a recipe for withdrawing more ether than they were eligible for from the contract via re-entrancy.


The DAO hack took advantage of Ethereum’s fallback function to perform re-entrancy. Every Ethereum smart contract byte code contains the so-called default fallback function which has the following default implementation shown in Figure 1


This default fallback function can contain arbitrary code if the developer overrides the default implementation. If it is overridden as payable, the smart contract can accept ether. The function is executed whenever ether is transferred to the contract


*Fig. 1 The default implementation of the default fallback.*


(see the description of methods send(), transfer() and call() below) or whenever a transaction attempts to call a method that the smart contract does not implement.


Aside from calling payable methods, Solidity supports three ways of transferring ether between wallets and smart contracts. These supported methods of transferring ether are send(), transfer() and call.value(). The methods differ by how much gas they pass to the transfer for executing other methods (in case the recipient is a smart contract), and by how they handle exceptions. send() and call().value() will merely return false upon failure but transfer() will throw an exception which will also revert state to what it was before the function call. These methods are summarized below


*Table 1. Methods for transferring ether. By default, all the remaining gas is available when using call.value(), but the developer may choose to reduce the amount.*


In the case of the DAO smart contract (a basic version of which is given in figure 2) the ether was transferred using the call.value() method. That allowed the transfer to use the maximum possible gas limit and also prevented reverting the state upon possible exceptions. Thus, the attackers were able to create a sequence of recursive calls to siphon off funds from the DAO using a smart contract similar to the one presented in Figure 3


*Fig.2 A simplified DAO contract with reentrancy vulnerability in its withdrawBalance() function.*


‍


*Fig. 3 A smart contract for exploiting re-entrancy in BasicDAO from Figure 2*


The result was the following sequence of actions (also depicted below in Figure 4):


1. The proxy smart contract would ask for a legitimate withdrawal.
2. The transfer from BasicDAO to the proxy smart contract triggered a fallback function.
3. The proxy smart contract fallback function would ask BasicDAO for another withdrawal.
4. The transfer from BasicDAO to the proxy smart contract triggered a fallback function.
5. The proxy smart contract fallback function would ask BasicDAO for another withdrawal.
6. . . .


Note that the balance of the proxy smart contract was never updated (this happens after the transfer). Furthermore, notice that unless the transfer to the proxy contract fails, an exception is never thrown and the state never gets reverted.


*Fig. 4 An illustration of the re-entrancy attack.*


### Prevention


The re-entrancy attack in the DAO contract could have been avoided in several ways. Using the functions send() or transfer() instead of call.value() would not allow for recursive withdrawal calls due to the low gas stipend. Manually limiting the amount of gas passed to call.value() would achieve the same result.


Yet, there is a much simpler best practice that makes any re-entrancy attack impossible. Note that the DAO contract updates the user balance after the ether transfer. If this was done prior to the transfer, any recursive withdraw call would attempt to transfer a balance of 0 ether. This principle applies generally—if no internal state updates happen after an ether transfer or an external function call inside a method, the method is safe from the re-entrancy vulnerability.


**About the Author**


This post is written by Quantstamp Senior Research Engineer Martin Derka, PhD, and is based on an excerpt from[Fundamentals of Smart Contract Security.](https://www.amazon.com/Fundamentals-Smart-Contract-Security-Richard/dp/194944936X)


Martin holds a Ph.D in Computer Science from the University of Waterloo. He also studied at Brock University, McMaster University, Masaryk University in the Czech Republic, and has additional degrees from some of these. He is former Vanier Scholar and a brief NSERC post-doctoral fellow at Carleton University.
