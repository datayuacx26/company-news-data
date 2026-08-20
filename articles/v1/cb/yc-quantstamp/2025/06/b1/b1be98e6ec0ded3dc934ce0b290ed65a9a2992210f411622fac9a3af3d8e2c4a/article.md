---
schema_version: "1.0.0"
document_id: "b1be98e6ec0ded3dc934ce0b290ed65a9a2992210f411622fac9a3af3d8e2c4a"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/proper-treatment-of-randomness-on-evm-compatible-networks"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:f753bc4b233595c25c79e15fdbde13b168052e98873cc8bf221ad4987c64267e"
---

# Proper Treatment of Randomness on EVM-Compatible Networks

Solidity does not have a function for generating random numbers. It is for a rather simple reason: all the miners maintain the same local state of the blockchain, and they have to arrive at the same results when interpreting transactions. If transactions could invoke instructions for generating random numbers, miners would have varying results and the integrity of the blockchain state could not be maintained. However, Ethereum dapp developers sometimes desire random numbers in order to develop lotteries, NFT drops, and games where randomness influences the outcomes. Therefore, over time, developers have invented several techniques to bypass this limitation. Some of them work, some do not, but none of them work if they are not used properly. The purpose of this article is to provide an overview of such techniques and make recommendations for treating randomness properly in your Ethereum dapps. Namely, we will speak about:


- Making decisions based on block characteristics and hashing
- Using verifiable random functions (VRF)
- Using the commit-and-reveal scheme


### **Decisions Based on Block Characteristics and Hashing**


In order to demonstrate the issues that developers can face while handling randomness, we’ll first look into the history and demonstrate what can go wrong. In 2018, an application called[FOMO3D](https://medium.com/quantstamp/what-we-learned-from-fomo3d-part-1-2c316db3d1e1) —and actually a Ponzi scheme—[became very popular on the Ethereum network](https://medium.com/quantstamp/what-we-learned-from-fomo3d-part-1-2c316db3d1e1) . This application provided the world with a live demonstration of a randomness exploit on-chain. We can see its vulnerability via the following simplified snippet:


`


```text
function enter() external payable {


require(msg.value == 1 ether, “Did not pay enough”);


if (blockhash(block.number) % 50 == 0) {
msg.sender.transfer(this.balance / 100);
}


// … more logic
}


```


`


The snippet features a function called enter, which uses the current blockhash to decide if the caller should receive some reward (a fraction of the smart contract’s balance). This code is vulnerable because an attacker can come up with a smart contract that can call the enter function on their behalf, but before contributing, can check the very same block characteristic used to make the decision, and know whether it will be receiving a reward or not. In this case, it checks the block hash. The exploit code would look somewhat like this:


```text


contract Exploit {


function(address fomo) public payable {
require(blockhash(block.number) % 50 == 0);


Fomo3d(fomo).enter{value: msg.value}();
}


// … more functions
}


```


The developers of FOMO3D were clearly aware of this attack vector, so they decided to prevent smart contracts from calling this method and added a requirement that the caller of the enter method does not have any smart contract code associated with its address on on chain:


`


```text
function enter() external payable {
require(extcodesize(msg.sender) == 0, “Only EOAs”);
require(msg.value == 1 ether, “Did not pay enough”);


if (blockhash(block.number) % 50 == 0) {
msg.sender.transfer(this.balance / 100);
}


// … more logic
}


```


`


This is a common technique that developers use when trying to avoid some damage that smart contracts can do to their dapp. Unfortunately, it does not work in this form because there are several times in the life cycle of a smart contract when there may be no code associated with the contract’s address. One of these times is during the construction, so placing the exploit code into the constructor would still allow the exploit to occur. What the developers should have used instead is:


`


```text
function enter() external payable {
require(msg.sender == tx.origin, “Only EOAs”);
require(msg.value == 1 ether, “Did not pay enough”);


if (blockhash(block.number) % 50 == 0) {
msg.sender.transfer(this.balance / 100);
}


// … more logic
}


```


`


This is currently the only method on Ethereum that guarantees that the caller is not a smart contract; however, it may eventually become unavailable.


If smart contracts were the only way of exploiting randomness, our survey could end here, or at least take a very different direction. Unfortunately, there are currently other ways of circumventing these measures, which include miners and[a service called flashbots](https://docs.flashbots.net/) . Flashbots is essentially a private mining pool where users can submit transactions. The pool will attempt to mine the transactions with some additional special conditions. Namely, the service allows batches of transactions to be submitted and guarantees that:


- If mined, all the transactions will be arranged consecutively one after another inside a single block
- That all of them will be mined or none of them will
- That they will be mined only if they all succeed


Flashbots are usually referred to in the context of MEV (miner extractable value) but as shown here, they can also be used for exploiting wrong implementations of randomness.


Instead of looking at simplifications of very old smart contracts (FOMO3D dates back to 2018), the excerpt below is from a[recent contract that showed up on etherscan during late 2021](https://etherscan.io/address/0xf042a49fb03cb9d98cba9def8711cee85dc72281#code) .


The snippet above is part of an NFT game where the owners could stake their NFTs in order to collect some assets. During the unstaking process, the smart contract would try randomly deciding whether or not assets should be sent to them. The attack vector for exploiting this code using flashbolt should now be clear. The attacker imagines the state of the world when unstaking is favorable; the undertaker should end up with the NFT, along with the additional assets the NFT collected. If they ended up with these assets, then they clearly use them, which forms the batch of transactions that will explode the code:


1. Unstake
2. Assume that the account now holds the assets, and transfer them (wherever)


The attacker would submit the two transactions to flashbots. If the assets were stolen during unstaking, the second transaction (which assumes their presence) would fail, and the entire batch would fail to mine. The attacker can keep trying until it works out. Since any attempts that do not mine don’t cost anything (no transaction on-chain = no gas spent), so there is no drawback to doing so.


### **Verifiable Random Function**


A Verifiable Random Function (VRF) is a cryptographic function that utilizes a private and public key pair to generate (pseudo) random numbers. A private key is used to generate the number, and a public key can be used to verify that the number was generated correctly. An implementation of a[VRF is currently provided by Chainlink within their system of on-chain oracles](https://docs.chain.link/docs/chainlink-vrf/) . Developers can consume randomness from Chainlink in an asynchronous manner. The smart contract code will generate a request passed to the Chainlink’s coordinator contract. The coordinator will wait for the oracle agents to fulfill the request, and when the randomness becomes available, it will call back a designated method and pass the random input as a parameter.


The use of VRF is often referenced as “the solution” for the problems with randomness, but we show below a snippet of code that uses the VRF for sourcing unpredictable random seed, but due to improper handling, it can be exploited for the attacker to hit the desired outcome.


In the following snippet, we have the enter() method that is to be called by the user. The smart contract holds some random seed that it uses to determine if the user should win a jackpot. Upon the entry, a decision is made, the seed is quickly swapped for a subsequent call, and a randomness request to Chainlink is made. The randomness by Chainlink will arrive at some point, and the timing of the response cannot be influenced by the user. The snippet also assumes that the onlyEOA modifier prevents smart contract interaction with the enter() method.


`


```text
// the current random seed
uint256 public seed;


// address to Chainlink’s coordinator
Chainlink public chainlink;


// the randomness fulfillment called by Chainlink
function fulfillRandomness(bytes32 requestId, uint256 randomness) internal {
seed = randomness;
}


// the user method
function enter() external payable onlyEOA {
if (uint256(keccak256(abi.encodePacked(seed))) % 2 == 0) {
// reward user if they are lucky
this.balance.transfer(msg.sender);
}
seed = keccak256(
// quick seed change
abi.encodePacked(seed, blockhash(block.number))
)
// request new randomness
chainlink.requestRandomness();
}


```


`


The code can be exploited via flashbots exactly as described in the previous section. An attacker would submit repeated requests batching two transactions. The first would call the enter() method. The second would assume that additional ether was sent to accounts, and use it. The batch would be mined only if both the transactions succeed. A successful exploit transaction would further be indistinguishable from an honest transaction.


Note that the code would provide multiple attack vectors if the onlyEOA modifier was not present. A smart contract could query the current seed and only enter under the right conditions, or it could make a series of requests to enter() in a single transaction (getting all the invested funds back in the one that introduces the right seed).


### **Commit-And-Reveal**


The commit-and-reveal is a design pattern that breaks evaluating a random event into two steps. In the first step, the user commits to performing an action. In the subsequent step, the action is evaluated. For the commit and reveal pattern to work, the code needs to satisfy three conditions:


1. All the assets related to the random event have to be collected during the commitment, and users cannot retract the commitment after they committed.
2. The outcome decision is locked during the commitment time, and the users cannot infer the outcome while they are making the commitment.
3. The users cannot commit again for the same random event.


The asynchronous fulfillment of the VRF offered by Chainlink is, in fact, also a variant of the commit and reveal pattern (with provable fairness added).


The following snippets of code show the possible flaws in the commit and reveal scheme implementation. We assume that the getSafeRandom() method produces randomness.


`


```text
// tracking the commitments
mapping (address => bool) public commits;


function commit() external payable {
require(commits[msg.sender] == 0);
require(msg.value == 1 ether);
commits[msg.sender] = true;
}


function reveal() external {
require(commits[msg.sender]);
int256 commit = getSafeRandom();
if (commit % 2 == 0) {
// reward the user if they are lucky
this.balance.transfer(msg.sender);
}
}


```


`


The snippet above violates the point (2) in both of its aspects. The commitment outcome is not locked at the time of the commitment. The user can be retrying the reveal method repeatedly. The following code appears to fix the problem, but it does not do so


`


```text
// tracking the commitments
mapping (address => uint256) public commits;


function commit() external payable {
require(commits[msg.sender] == 0);
require(msg.value == 1 ether);
commits[msg.sender] = getSafeRandom();
}


function reveal() external {
require(commits[msg.sender] != 0);
if (uint256(commits[msg.sender]) % 2 == 0) {
// reward the user if they are lucky
this.balance.transfer(msg.sender);
}
}


```


`


The code still violates the point (2) as the user can infer the outcome of the commitment while making the commitment. A smart contract can revert the commitment because it can query the value. If the smart contract interaction is prevented for the commit() method, an auxiliary smart contract can still query a commit of an EOA, which can be used for an exploit in conjunction with flashbots. The next code snippet fixes this problem by making the mapping of commits private, which ensures that it cannot be queried by smart contracts. This seemingly resolves the violation of point (2) provided that the getSafeRandom() method is safe and produces unpredictable results. Unfortunately, the code still has problems.


`


```text
// tracking the commitments
mapping (address => uint256) private commits;


function commit() external payable {
require(commits[msg.sender] == 0);
require(msg.value == 1 ether);
commits[msg.sender] = getSafeRandom();
}


function reveal() external {
require(commits[msg.sender] != 0);
if (uint256(commits[msg.sender]) % 2 == 0) {
// reward the user if they are lucky
this.balance.transfer(msg.sender);
}
}


```


`


The outcome of the commitment in the snippet above can no longer be queries. However, it can be immediately resolved, so the code still violates point (2). A smart contract exploiting this snippet would commit, and in the same transaction right after the commit, it would call reveal. It would then revert the transaction if the outcome did not yield any reward. If the code was equipped with transaction locks (i.e., the reveal transaction hash would need to differ from the commit transaction hash), or if the code prevented smart contract interaction, the code would still be vulnerable if the attacker used a batch submitted to flashbots. A lock based on blocks would be needed:


`


```text
// tracking commitments
mapping (address => uint256) private commits;


// tracking locks
mapping (address => uint256) public blocks;


function commit() external payable {
require(commits[msg.sender] == 0);
commits[msg.sender] = getSafeRandom();
blocks[msg.sender] = block.number;
}


function reveal() external {
require(blocks[msg.sender] < block.number - 32);
require(commits[msg.sender] != 0);
if (uint256(commits[msg.sender]) % 2 == 0) {
// reward user if they are lucky
this.balance.transfer(msg.sender);
}
}


```


`


The presented snippet prevents the reveal for 32 blocks after committing. It appears that even a lock for the duration of a single block is currently sufficient to prevent the attack. However, if a mining pool or service such as flashbots acquired enough hash power to mine two subsequent blocks with a high probability, the same attack would be possible (and would succeed with the same high probability). Therefore, assuming that the getSafeRandom() function is safe, a longer lock period increases the safety of this pattern with respect to the future changes of the network hashpower control.


### **Conclusion**


So what randomness technique should you use in your project? This effectively depends on the application that you are developing.


- If your application provides a limited window to execute a certain task, so that the user has problems winning the race and get a single transaction within the window (this is the case for many of the recent NFT drops), then immediate resolution of randomness based on some block characteristics may be an option. The attacker will not have a chance to try their luck again, and needs to take what they are given, or just abort.
- If your application is to run for an extended period of time, allowing for repeated entries at varying times, and repeated resolutions of random events, then you need to consider all the solutions at hand. You will be bound to the commit and reveal pattern, which may or may not include the VRF.


### **Bounty**


Quantstamp is offering a $5000 bounty at ETH Denver 2022 for implementing a dapp that uses randomness. The dapp should use randomness in a non-trivial way, and the implementation needs to guarantee that the randomness is not exploitable. The bug bounty submission has to include a discussion that explains the user flow, how the randomness is acquired, and how an exploit or manipulation are avoided. The submission will further be judged based on good software engineering practices (organization of the code, comments, and unit tests), and creativity.
