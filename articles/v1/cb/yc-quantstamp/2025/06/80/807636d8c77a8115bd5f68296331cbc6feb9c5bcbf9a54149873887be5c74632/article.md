---
schema_version: "1.0.0"
document_id: "807636d8c77a8115bd5f68296331cbc6feb9c5bcbf9a54149873887be5c74632"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/how-to-be-an-eth-2-0-validator-on-the-topaz-testnet"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:f9bd9a2171e75b8c949c92c240f3816ca2c36312bb87482e75775332d3acf41c"
---

# How to Be an ETH 2.0 Validator on the Topaz Testnet

The Topaz Testnet is a public Ethereum 2.0 testnet created by[Prysmatic Labs](https://prylabs.net/) . It is a testnet version of Ethereum 2.0 Phase 0 which is planned to launch on mainnet later this year.


Anyone can participate in the Topaz Testnet as a validator, and this article will walk you through the process.


### Why Participate?


Ethereum 2.0, aka ETH 2.0, is a highly anticipated upgrade to Ethereum which improves scalability by shifting from[Proof-of-Work](https://en.wikipedia.org/wiki/Proof_of_work) to[Proof-of-Stake](https://en.wikipedia.org/wiki/Proof_of_stake) . By running an ETH 2.0 validator on Topaz Testnet, you can help contribute to the testing phase of this new version of Ethereum.


You also get to practice setting up a validator and staking testnet ETH without risking actual funds on mainnet.


### **Requirements**


To setup an ETH 2.0 Validator, you should have some familiarity with the command-line, as well as a basic understanding of how to use and send Ether.


This article focuses on setting up a Prysm ETH 2.0 Validator on a Linux-based computer. Mac OS X, Windows and ARM64 are also supported, though not covered in this article. You can find instructions for those operating systems in the[Prysmatic Labs Documentation.](https://docs.prylabs.network/docs/getting-started/)


You will need a moderately powerful computer which has a reliable internet connection. This can be a physical computer or a cloud instance. If it is a physical computer, it needs to be always-on, otherwise your validator may be penalized while inactive and lose some funds.


*Recommended Specifications :*


Operating System: 64-bit Linux


Processor: Intel Core i7–4770 or AMD FX-8310 or better


Memory: 8GB RAM


Storage: 100GB available space SSD


Internet: Broadband connection


*Minimum Requirements:*


Operating System: 64-bit Linux


Processor: Intel Core i5–760 or AMD FX-8100 or better


Memory: 4GB RAM


Storage: 20GB available space SSD


Internet: Broadband connection


While a physical computer is the ideal way to run an ETH 2.0 validator from a decentralization perspective, cloud instances can be easier to set up and maintain. These instances can be set up at providers such as[Vultr](https://vultr.com/) ,[Kamatera](https://www.kamatera.com/) , or[Digital Ocean](https://digitalocean.com/) .


You will also need a[Metamask](https://metamask.io/) or[Portis Wallet](https://www.portis.io/) to receive and send testnet funds. Your wallet can be on the same computer, or a separate computer from the node.


### **How to Setup a Prysm ETH 2.0 Validator**


Prysmatic Labs has a great website with instructions on how to setup the Prysm Validator:[https://prylabs.net/participate](https://prylabs.net/participate)


If you are an experienced Linux user who is knowledgeable about the mechanics of ETH 2.0, those instructions may be sufficient. Otherwise, we provide additional context and background information here.


We recommend keeping both[the Prysm page](https://prylabs.net/participate) and this page open as you go through these steps.


1. Get Prysm by executing the instructions from[the Prysm page](https://prylabs.net/participate) in a Terminal window:


```text
mkdir prysm && cd prysm
curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh


```


1. Get at least 32 GöETH


Navigate to the[Prysm page](https://prylabs.net/participate) and unlock your Metamask or Portis Wallet. If you are using Brave, you may need to turn down your shields for the page to work properly. Once you see your wallet address on the page, click the button \[Need GöETH?\] which will send you the appropriate amount of GöETH.


1. Generate a validator public / private key


` ./prysm.sh validator accounts create`


After inputting this command, you choose a directory to store your keystore files and a password. Then you copy and paste the deposit data into the Prysm page.


You will also see a public key - which you should jot down and save.


1. Start your beacon chain and validator clients.


As explained on the Prysm page, you can now start your beacon chain and validator clients.


In one terminal, run:


` ./prysm.sh beacon-chain`


In another terminal run


` ./prysm.sh validator`


‍
You will see a lot of messages in the Beacon Chain terminal as the Beacon Chain client synchronizes to the latest block. This will take several hours and will make extensive use of CPU and memory. If you are running this on your main computer, don’t plan on doing anything else resource intensive in the meantime.


If at any time your terminal session or Beacon Chain client is terminated, you can start it up again with the same command. It should continue where it left off.


Once the Beacon Chain client is synchronized, you should see messages similar to the above.


1. Make sure the beacon chain is synchronized as in step 4, then send your validator deposit


*Warning: If you do this before Step 4 is complete, you may suffer penalties.* After the deposit transaction completes on the Goerli test network, it will take some time to be included in the Beacon Chain. This may take a few hours.


1. After your deposit has made its way to the Beacon Chain, you should see messages similar to these:


This means you are now in the queue to be a validator. The “positionInActivationQueue” is your position in the Queue, which should steadily decrease. The pubKey is the first 12 characters of your public key on the ETH 2.0 Beacon Chain.


The queue to be a validator may be quite substantial, so it may take anywhere from a few hours to multiple days for you to become a validator.


Once your validator is activated, you should see messages similar to the following:


Congratulations, you’ve successfully set up an ETH 2 Validator on Topaz Testnet! You are now staking and contributing to the security of the Topaz network.


You can monitor your validator’s status and earnings in the validator terminal, or by putting your public key in a beacon chain explorer.


### Additional Tips


If you are running your validator and beacon chain through a cloud instance, the processes will close when your terminal window closes. The best way to get around this is to use a session handler such as tmux to keep those processes running. Session handling is beyond the scope of this article but you can find more information about how to do session handling with tmux[here](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) .


It is possible to run multiple validators through a single validator runtime. You can find more details on how to do this in the[Prysm documentation](https://docs.prylabs.network/docs/how-prysm-works/prysm-validator-client/) .


### Additional Resources


The[Prysmatic Labs Discord](https://discord.com/invite/YMVYzv6) is the best place to find support.


For more detailed information about the process of being a validator,[the Prysmatic documentation](https://docs.prylabs.network/docs/getting-started/) is extremely helpful.


*Quantstamp is happy to be*[auditing](https://github.com/quantstamp/prysm-1) *Prysmatic Labs' ETH 2.0 client*
