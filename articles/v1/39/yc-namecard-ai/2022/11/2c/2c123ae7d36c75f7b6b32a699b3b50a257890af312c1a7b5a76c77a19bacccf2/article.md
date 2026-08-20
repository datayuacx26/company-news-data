---
schema_version: "1.0.0"
document_id: "2c123ae7d36c75f7b6b32a699b3b50a257890af312c1a7b5a76c77a19bacccf2"
company_key: "yc-namecard-ai"
company: "Namecard.ai"
source_id: "yc-namecard-ai-rss-f3518cbae1fb"
canonical_url: "https://chainsigtweb3.medium.com/how-to-avoid-metamask-ip-tracing-a691d7b92f5e"
published_at: "2022-11-28T04:09:35+00:00"
first_seen_at: "2026-08-10T00:54:06.811990+00:00"
fetched_at: "2026-08-20T03:26:10.503825+00:00"
content_hash: "sha256:13d0328df9f99cc7f93689c4f1602fdac4ef653faf081f1765b3fa9819926e5e"
---

# How to avoid MetaMask IP tracing?

Philipp Katzenberger/Unsplash


As I’m sure you’ve heard, MetaMask is tracing all of our IP addresses.


Here are two easy step-by-step guides to help you keep your information safe from the prying eyes of Big Brother.


### Change to another wallet service provider.


Restore your wallet with the Seed phrase generated when you sign up for MetaMask.


We used MetaMask and Exodus as an example. Please check the privacy policy before you migrate to another wallet service.


1. Prepare your seed phrase from the MetaMask wallet.


Please don’t share your seed phrase with anyone.


2. Install a new wallet extension/ app. Choose “I have a wallet” or “Import from another wallet” on the settings page.


3. Input your seed phrase.


4. Done!


You can access your address on MetaMask on Exodus now.


*P.S. Make sure your new wallet supports the same blockchain network as MetaMask. During our test, we found glitches when we used the MetaMask seed phrase on the Phantom wallet.


### Use a third-party RPC provider with MetaMask.


According to the updated MetaMask privacy policy, MetaMask can only collect your IP addresses when you use Infura (the default) as the RPC provider for your wallet.


We used a third-party RPC provider Flashbots for this example guide. Please check the privacy policy of the third-party RPC provider you decide to use.


1. Find your ideal RPC provider


This website is a good source for nodes and RPC providers:[https://ethereumnodes.com/](https://ethereumnodes.com/)


Special thanks to website developer[David Mihal](https://twitter.com/dmihal) .


2. Google the RPC provider you want to find their settings guide. Each RPC provider has unique settings, so be sure to consult the guides for the best results.


3. Add a new network on your MetaMask


4. Set up the new network. Ensure to input the correct RPC URL, Chain ID, and currency symbol.


- Input “https://rpc.flashbots.net" as the RPC URL to use Flashbots as your RPC provider.
- Input “1” as the Chain ID to use the Ethereum blockchain network.
- Input “ETH” as the currency symbol.


5. Done!
