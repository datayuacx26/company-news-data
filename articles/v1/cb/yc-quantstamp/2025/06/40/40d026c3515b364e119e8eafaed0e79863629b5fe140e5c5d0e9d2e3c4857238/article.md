---
schema_version: "1.0.0"
document_id: "40d026c3515b364e119e8eafaed0e79863629b5fe140e5c5d0e9d2e3c4857238"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/how-the-dforce-hacker-used-reentrancy-to-steal-25-million"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:afcd67d7ad12d02c620e87dd71f4235b496833f01ba3af3de767785a30a952a9"
---

# How the dForce hacker used reentrancy to steal 25 million

On April 18th 9:00 PM EST, a hacker stole nearly 25 million USD worth of digital assets from Lendf.Me, a lending market similar to Compound created by dForce. The hacker[exploited a reentrancy vulnerability](https://quantstamp.com/blog/what-is-a-re-entrancy-attack) in order to manipulate Lendf.Me’s internal record of the hacker's collateral. After the hacker created this false collateral record, the hacker withdrew nearly all stablecoins, synthetic BTC, and wETH from the platform.


source:[defipulse.com](https://defipulse.com/dforce)


### imBTC, ERC777, and Reentrancy


The hack took place after dForce allowed imBTC, a synthetic Bitcoin asset following the[ERC777 standard](https://www.google.com/url?q=https://eips.ethereum.org/EIPS/eip-777&sa=D&ust=1587582474280000) , to be used as collateral on Lendf.Me. The ERC777 token standard was created in order to extend the capabilities currently available in ERC20 tokens.


Aside from other features, ERC777 allows the token contract to notify senders and recipients when ERC777 tokens are sent or received from their accounts. This notification is in the form of a callback to the recipient. Therefore, if the recipient of the tokens is a smart contract, the smart contract can choose to react to such events. One possible reaction to such an event is reentering the ERC777 contract and calling another send (if the particular implementation of ERC777 allows). When Lendf.Me enabled the use of imBTC as collateral, the enabled ERC777 callback notification made Lendf.Me vulnerable to reentrancy attacks.


This vulnerability allowed the attacker to create a false record of imBTC collateral within the Lendf.Me system. The attacker first truthfully deposited a substantial amount of imBTC as collateral. Subsequently, they triggered another deposit of imBTC, but within the callback and before the actual transfer of imBTC, they withdrew their original imBTC deposit. The code of Lendf.Me did not account for such a transfer or execution to the callback being possible, and performed crucial state updates after the transfer completed based on data stored in local variables. Therefore, after properly decreasing the attacker’s collateral within the hooked withdrawal, the code overwrote the attacker’s collateral value when the execution returned to the deposit being performed. In consequence, both the operations together recorded net collateral increase.


By continuing to perform the same attack, from the perspective of the protocol, the attacker’s collateral balance inflated to well over 25 million USD: however, the imBTC that the attacker used while executing the attack was already in their personal account. This allowed the attacker to finalize their attack by “borrowing” all liquidity within each of the 12 lending markets with collateral that was not physically present inside of Lendf.Me.


One of the attacking transactions. source:[Etherscan](https://etherscan.io/tx/0x9743677ccf9a95f0a0521558d1bf853226f3a87c077d40cf3df73199ba211102)


### dForce Foundation response


An hour following the exploit, the dForce Foundation paused Lendf.Me contracts, reached out to law enforcement, and contacted exchanges in order to blacklist transactions from the hacker ([source](https://www.google.com/url?q=https://cointelegraph.com/news/dforce-hacker-attempts-to-negotiate-after-allegedly-leaking-his-identity&sa=D&ust=1587582474281000) ). After the hacker initiated conversation with dForce by sending a message over an[Ethereum transaction](https://www.google.com/url?q=https://etherscan.io/tx/0xfaaffe77ec5abecb730848a218abd4e22a4a0752e497ca8c859a7f05ff89797a&sa=D&ust=1587582474282000) , dForce began responding via the same method and pressured the hacker to communicate with them via email. After realizing the hacker had used 1inch.exchange to trade stolen assets for other digital currencies, Singapore authorities contacted the exchange, who subsequently turned over the[hacker’s IP address](https://www.google.com/url?q=https://twitter.com/RyanSAdams/status/1252574107159408640?s%3D20&sa=D&ust=1587582474282000) and other metadata.


dForce communicating with the hacker via Ethereum tx and applying pressure. Source:[Etherscan](https://etherscan.io/tx/0x481e8a7c1c39ad755fbae3f7cf030b4a10f94b875bff2829582244120a36ea9a)


### Current status


After receiving pressure from the authorities and dForce, the hacker returned nearly all funds. dForce is creating an asset redistribution plan in order to return assets to affected users and providing updates to users and the community on their[Medium channel](https://www.google.com/url?q=https://medium.com/dforcenet&sa=D&ust=1587582474283000) . Mindao Yang, the founder of dForce, told users the following: “If you were harmed by this attack, we will make you whole again.”


### Security Best Practice Takeaways


Since the beginning of the year, multiple security exploits have occurred that led to financial damage. Quantstamp security engineers offer the following best practices to Solidity developers in order to facilitate secure development:


Quantstamp Senior Research Engineer Sebastian Banescu:


> Don't remove external dependencies/libraries from the scope of an independent[#security](https://twitter.com/hashtag/security?src=hash&ref_src=twsrc%5Etfw) audit just because it's "the reference" implementation[#seenThisTooOften](https://twitter.com/hashtag/seenThisTooOften?src=hash&ref_src=twsrc%5Etfw)
>
>
> The recent[https://t.co/to4G3AXHnh](https://t.co/to4G3AXHnh)[#attack](https://twitter.com/hashtag/attack?src=hash&ref_src=twsrc%5Etfw) shows that issues might arise in well established implementations as well
>
>
> — Sebastian Banescu (@banescusebi)[April 20, 2020](https://twitter.com/banescusebi/status/1252309732599181313?ref_src=twsrc%5Etfw)


‍


Quantstamp Research Engineer Martinet Lee:


> Be very serious about tests. Write extensive unit tests and functional tests. Write tests about happy paths and try out all the edge cases that you might think of. Measure test coverage to help the process. Unfortunately, there are a lot of projects that don't even have any test.
>
>
> — Martinet Lee (@martinetlee)[April 20, 2020](https://twitter.com/martinetlee/status/1252039978630164480?ref_src=twsrc%5Etfw)


‍


In addition, copy and pasting code (aka the clone-and-own approach) from other projects is very dangerous. When a developer copy and pastes code belonging to a different project, it often leads to incompatibilities that lead to exploits.


‍
