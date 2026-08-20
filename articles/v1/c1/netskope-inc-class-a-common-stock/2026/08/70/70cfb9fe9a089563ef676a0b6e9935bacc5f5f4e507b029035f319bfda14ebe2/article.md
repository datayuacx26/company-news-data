---
schema_version: "1.0.0"
document_id: "70cfb9fe9a089563ef676a0b6e9935bacc5f5f4e507b029035f319bfda14ebe2"
company_key: "netskope-inc-class-a-common-stock"
company: "Netskope Inc."
source_id: "netskope-inc-class-a-common-stock-rss-c0a3e1ef9778"
canonical_url: "https://www.netskope.com/blog/blockchain-dead-drop-resolvers-explained"
published_at: "2026-08-18T11:00:21+00:00"
first_seen_at: "2026-08-18T11:01:12.453525+00:00"
fetched_at: "2026-08-18T11:01:14.509416+00:00"
content_hash: "sha256:8a8fab7945bace9305ff278a56be8ce6cce9904f00d348ecc3210deb0d33f594"
---

# Blockchain Dead Drop Resolvers Explained

dead drop resolver (DDR) is any mechanism where malware fetches its command and control (C2) address at runtime from a third-party, cyberattacker-controlled location instead of hardcoding it.


This post is a practitioner primer. We walk through eight unrelated malware families across three chains (EVM, Solana, TON) using the same core trick: including the August 2026 ChainDrop (“mini Shai-Hulud”)[npm supply-chain compromise](https://www.netskope.com/blog/npm-stealer-reads-its-c2-from-an-ethereum-contract) that uses an Ethereum` eth_call` dead drop.


Each section contains a read-only one-liner you can run to see the technique, followed by instructions about how to approach detecting the discriminator. We close with information about the way Netskope detects these threats.


## Blockchain DDRs


Dead drop resolvers previously ran through Telegram channel bios, paste sites, and GitHub gists. Now, these public web services are being replaced by public smart contracts on the blockchain.


In such architecture, the loader (malware delivered) calls a public RPC node, reads a value from a contract, and decodes it into an IP, URL, domain, JavaScript, or a bash stager.


This matters for three reasons:


- **Takedown resistance** : A public blockchain cannot be compelled to censor a read, and an operator re-points every deployed loader by updating one contract value for a few cents, with no new binary or domain to burn.
- **Traffic blending** : The loader posts JSON-RPC to the same high-reputation crypto SaaS hosts that wallets and decentralized applications use (Infura, Cloudflare, Binance, publicnode), so host-only network signatures drown in false positives.
- **Overlooked technique** : While the technique has been vastly exploited, defenders lag behind. In fact, it’s vastly exploited exactly because of that.


The next sections explore the most common variations of the technique.


## EVM smart-contract read (eth_call)


The most common variant. The loader posts a JSON-RPC` eth_call` to a public EVM node (Ethereum, Polygon, or BNB Smart Chain).` eth_call` is read-only and creates no transaction, so it is free and leaves no on-chain trace of who asked.


The ABI-decoded response holds the C2 pointer. Operators pick the encoding: a plain UTF-8 URL, Base64, Base64 plus XOR, gzip plus Base64, or a bash one-liner.


Several actors use this shape. **TroyDen** reads a plaintext IP from a Polygon contract, as we documented in a[research blog post](https://www.netskope.com/blog/developers-in-the-crosshairs-fake-ai-tools-deliver-infostealer) . **DeadLock** ransomware reads a rotating Session-messenger relay URL from a Polygon contract. **CLEARSHORT** and **JADESNOW** both read obfuscated JavaScript or bash stagers from BNB Smart Chain contracts.


The **TroyDen** one-liner hits a live Polygon RPC and prints the plaintext C2 URL


```text
curl   -s   'https://polygon.publicnode.com'   -H   'Content-Type: application/json'   -d   '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0x1823A9a0Ec8e0C25dD957D0841e3D41a4474bAdc","data":"0x3bc5de30"},"latest"],"id":1}'   |   python3   -c   "import sys,json;r=json.load(sys.stdin)['result']; n=int(r[66:130],16); print(bytes.fromhex(r[130:130+n*2]).decode())"
```


*Output* :` http://83.97.20\[.\]150` (defanged)


The fresh case is **ChainDrop** , also called “mini Shai-Hulud,” which[compromised more than 440 npm packages in August 2026](https://www.netskope.com/blog/npm-stealer-reads-its-c2-from-an-ethereum-contract) . The dropped bun binary posts a 136-byte` eth_call` body to Ethereum mainnet RPC providers, reads the contract, then exfiltrates to the resolved domain.


ChainDrop fans out across three RPC providers (` eth.llamarpc.com` ,` go.getblock.io` ,` eth-mainnet.nodereal.io` ), so a rate-limited or downed endpoint does not break resolution. This is possible because the source of truth lives on-chain and the endpoint is interchangeable. The read-only reproduction uses the keyless endpoint from that fallback chain:


```text
curl   -s   -X   POST   https://eth-mainnet.nodereal.io/v1/1659dfb40aa24bbb8153a677b98064d7   \
-H   'Content-Type: application/json'   \
-d   '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xab09c722546fd5e6775affaf989aac3363ac7919","data":"0x02d1e413"},"latest"],"id":1}'
```


The contract is still live, but its stored value is now cleared by the operators (the call returns` 0x` ).


## Solana memo dead drop


Solana has no` eth_call` equivalent for arbitrary contract reads, so this variant abuses the memo field attached to ordinary transactions. The loader issues two calls:` getSignaturesForAddress` to list recent transactions on a wallet, then` getTransaction` to fetch one and pull a payload (for example: a Base64-encoded C2 URL from the memo instruction).


This is the **GlassWorm** one-liner:


```text
SIG  =  $(  curl   -s   'https://api.mainnet-beta.solana.com'   -H   'Content-Type: application/json'   -d   '{"jsonrpc":"2.0","method":"getSignaturesForAddress","params":["28PKnu7RzizxBzFPoLp69HLXp9bJL3JFtT2s5QzHsEA2",{"limit":1}],"id":1}'   |   python3   -c   "
import sys,json; print(json.load(sys.stdin)['result'][0]['signature'])"  )
curl   -s   'https://api.mainnet-beta.solana.com'   -H   'Content-Type: application/json'   -d   "{  \"  jsonrpc  \"  :  \"  2.0  \"  ,  \"  method  \"  :  \"  getTransaction  \"  ,  \"  params  \"  :[  \"  $SIG  \"  ,{  \"  encoding  \"  :  \"  jsonParsed  \"  ,  \"  maxSupportedTransactionVersion  \"  :0}],  \"  id  \"  :1}"   |   python3   -c   "
import sys,json,base64
instrs=json.load(sys.stdin)['result']['transaction']['message']['instructions']
for ix in instrs:
if 'Memo' in ix.get('programId',''):
print(base64.b64decode(json.loads(ix['parsed'])['link']).decode())"
```


*Output* :` http://137.184.198\[.\]91/R2dIXAJpSXwxP`


The` getSignaturesForAddress` call with` limit:1` returns the most recent signature on the wallet. If the wallet has transacted since capture, adjust the limit or pin a specific signature to reproduce the original decode.


## TON blockchain DNS


The TON variant is the simplest. The loader issues a single REST GET to tonapi.io or toncenter, calling` get_domain` on a TON smart contract, and gets back a plain C2 domain.


**SalatStealer** and **TONResolver** both use TON smart contracts via` get_domain` . The one-liner below is for TONResolver, and it currently returns a refreshed value: the original[Trend Micro report](https://www.trendmicro.com/en_us/research/26/f/tonresolver.html) captured photo-*.cfd domains, consistent with a rotated dead-drop value.


```text
curl   -s   'https://tonapi.io/v2/blockchain/accounts/0:c66119f0e5635c4380441d7a79baf0c02a0ab7ea6cd78de06507fc5dc2c1a5d9/methods/get_domain'   \
|   python3   -c   "import sys,json; print(json.load(sys.stdin)['decoded']['domain'])"
```


*Output* :` njzlopghznkamkl.cfd`


Detection and hunting


## Detection and hunting


The decoded content and the calling process separate malware from benign crypto traffic. When analyzing the traffic (or an Intrusion Prevention System alert) of a blockchain DDR RPC call, pull four artifacts from the alert:


- **RPC URL**
- **Contract address** (the` to:` field)
- **Function selector** (the first four bytes of` data:` ). It identifies which contract function is called. Different selectors on the same dispatcher contract return different payloads.
- **The decoded result** . Run the curl and decode the hex result into the C2 URL, IP, domain, JavaScript, or bash string. This is the prize: the live C2 you block and pivot on. If the call returns` 0x` , the operator has rotated and the value is stale.
- **Process context** . A non-browser process such as` node` ,` bun` , or` curl` confirms malware. A browser or wallet making the same call is benign.


Bring together **contract address + function selector + RPC URL** , and use the one-liner pattern from this post to pull the current dead-drop value:


```text
curl   -s   -X   POST   '<RPC_URL>'   \
-H   'Content-Type: application/json'   \
-d   '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"<CONTRACT_ADDRESS>","data":"<FUNCTION_SELECTOR>"},"latest"],"id":1}'
```


## Conclusions


Blockchain dead drop resolvers turn a public, censorship-resistant ledger into mutable C2 infrastructure. Eight unrelated malware families across three chains share the technique, and the August 2026 ChainDrop npm compromise shows it arriving in commodity supply chain attacks. Run the one-liners against any suspicious loader you recover. If a non-browser process reads a contract and decodes a C2 pointer, you have your answer.


Netskope One Threat Protection Intrusion Prevention System (IPS) detects blockchain DDR RPC patterns.


## Indicators of compromise


Netskope Threat Labs has made available the full set of indicators of compromise (IOCs). Visit:[https://github.com/netskopeoss/NetskopeThreatLabsIOCs/](https://github.com/netskopeoss/NetskopeThreatLabsIOCs/) .


## References


- TroyDen (Netskope):[Developers in the Crosshairs: Fake AI Tools Deliver Infostealer](https://www.netskope.com/blog/developers-in-the-crosshairs-fake-ai-tools-deliver-infostealer)
- ChainDrop (Netskope):[npm Stealer Reads Its C2 From an Ethereum Contract](https://www.netskope.com/blog/npm-stealer-reads-its-c2-from-an-ethereum-contract)
- CLEARSHORT (Google Threat Intelligence):[UNC5142 EtherHiding: Distribute Malware](https://cloud.google.com/blog/topics/threat-intelligence/unc5142-etherhiding-distribute-malware)
- JADESNOW (Google Threat Intelligence):[DPRK Adopts EtherHiding](https://cloud.google.com/blog/topics/threat-intelligence/dprk-adopts-etherhiding)
- DeadLock (Group-IB):[DeadLock Ransomware Polygon Smart Contracts](https://group-ib.com/blog/deadlock-ransomware-polygon-smart-contracts/)
- GlassWorm (Koi):[GlassWorm: First Self-Propagating Worm Using Invisible Code Hits OpenVSX Marketplace](https://koi.ai/blog/glassworm-first-self-propagating-worm-using-invisible-code-hits-openvsx-marketplace)
- TONResolver (Trend Micro):[TONResolver](https://www.trendmicro.com/en_us/research/26/f/tonresolver.html)
- SalatStealer (Breakglass):[SalatStealer’s New Trick: Using TON Blockchain DNS to Make C2 Takedowns Impossible](https://intel.breakglass.tech/post/salatstealer-s-new-trick-using-ton-blockchain-dns-to-make-c2-takedowns-impossible)
