---
schema_version: "1.0.0"
document_id: "6c748eba72242fbab1d9a4132771d9eed9eb57cf8612040168cb6e6314d1b0cf"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/may-2026-security-beat"
published_at: "2026-07-07T14:31:00+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:a0e6333a66d6b3b499eda7749393e4f464f190d8b39cb8ecbeaabda9b931b3f8"
---

# May 2026 Security Beat

$59.52M was lost across 29 crypto incidents, down sharply from April's ~$635M. No single hack carried the month. Instead, the same weak points kept getting hit: cross-chain message validation, private keys, and access control. The bigger story happened off-chain, where a self-propagating npm worm called Mini Shai-Hulud kept resurfacing in new waves through the month, ultimately spanning more than 1,000 malicious package versions across the npm ecosystem.


Here's the month in security 👇


‍


### **Crypto: $59.52M lost across 29 incidents**


Category Loss Incidents


Bridge / Cross-Chain ~$22.1M 3


Access Control ~$20.6M 9


Other Protocol Logic ~$5.8M 6


Key / Credential Compromise ~$10.8M 7


Price / Oracle / Flashloan ~$0.3M 4


**Total** **$59.52M** 29


Source |[DeFiLlama‍](https://defillama.com/hacks)


‍


### **The incidents that did most of the damage**


**Verus-Ethereum Bridge, May 17: ~$11.5M initially.** Missing validation on a cross-chain import payload let an attacker forge a transfer and drain ETH, tBTC, and USDC. After bounty negotiation, the attacker reportedly returned roughly 75% (~$8.5M), which cut the net loss substantially.


**THORChain, May 15: $11M+.** An attacker compromised functions across at least nine of THORChain's supported chains (Bitcoin, Ethereum, BSC, Base, Avalanche, Dogecoin, Litecoin, Bitcoin Cash, and XRP) and drained them simultaneously, then consolidated the proceeds into a two-address cluster. TRM Labs traced the flows within hours but has not attributed the exploit to a named actor, and the technical root cause has not been detailed publicly. It fits a long pattern: THORChain's losses since 2021 now approach $25M, and its native cross-chain swaps make it both a recurring target and a preferred laundering rail for major heists.


**DxSale, May 28: ~$7.3M.** An ownership override attack on BSC let the attacker seize control of contracts that should have been locked down, then drain them. The purest "trusted access" failure of the month: nothing was forged or guessed, the attacker simply took over the owner role the contracts were built to trust.


Source |[DeFiLlama](https://defillama.com/hacks) ,[TRM Labs](https://www.trmlabs.com/resources/blog/thorchain-exploit-drains-usd-11m-across-at-least-nine-chains-what-trm-knows-now)


‍


### **Mini Shai-Hulud: the month's biggest supply chain story**


Mini Shai-Hulud is a self-propagating npm worm that runs at install time, steals developer and CI/CD credentials (GitHub and npm tokens, OIDC tokens, AWS and Kubernetes material, Vault tokens, SSH keys), then uses that access to republish itself into more packages. It also abuses trusted publishing, minting short-lived npm tokens inside compromised CI workflows and shipping releases with valid provenance.


In May, it hit in repeated waves: an early one on May 12 (373 versions across 169 packages, heavy in TanStack), then a larger one on May 19 (639 versions across 323 packages) concentrated in the @antv ecosystem, including echarts-for-react at roughly 1.1M weekly downloads. The full campaign now spans 1,000+ malicious versions across 500+ packages, almost all on npm.


**For web3 teams, the exposure is direct.** The worm targets exactly the machines that hold RPC keys, deploy credentials, and signing secrets, and AI coding assistants that auto-update dependencies can pull a poisoned version into a build without anyone choosing it. Pin exact versions, review dependency PRs by hand, use[Socket dev](https://socket.dev/) or[Aikido](https://www.aikido.dev/) tooling to identify/block the supply chain attack, and if an affected package ran in your environment, rotate every credential reachable from that machine, not just the npm token.


Source |[Aikido](https://www.aikido.dev/blog/mini-shai-hulud-is-back-tanstack-compromised) ,[Socket](https://socket.dev/blog/antv-packages-compromised)


‍


### **Laravel-Lang: a backdoor aimed straight at wallets**


On May 23, Socket and Aikido disclosed a compromise of the community-maintained Laravel-Lang project: a remote-code-execution backdoor pushed across 700+ historical versions of packages, including laravel-lang/lang and laravel-lang/attributes. These are third-party localization packages, not the official Laravel framework, but because the malicious file sat in Composer's autoload, it ran automatically on essentially every request to an affected app.


**What makes this one matter for web3 is the payload:** a credential stealer with a dedicated crypto module. It scans for wallet files (Bitcoin, Ethereum, Monero), browser-extension data for MetaMask, Phantom, and Trust Wallet, and even desktop folders for plaintext seed.txt or recovery.txt files, alongside the usual cloud, CI/CD, and SSH secrets. If you ran an affected version, treat the host as compromised, not just exposed: rebuild from known-good images and rotate every reachable secret, wallet keys, and seed phrases first.


Source |[Socket](https://socket.dev/blog/laravel-lang-compromise) ,[Aikido](https://x.com/AikidoSecurity/status/2057958510445658144?s=20)


‍


### **The pattern across the month**


Two things ran through May:


1. **Attackers logged in, they didn't break in.** Stolen or abused credentials accounted for more than half of May's losses, the kind of operational and key-management failures that sit outside the smart contract itself.
2. **The supply chain is the front line.** Mini Shai-Hulud is the clearest example: the code and dependencies you don't write yourself are still your responsibility.


‍


### **Disclaimer**


This report aggregates publicly reported information as of the publication date and may be revised as investigations evolve and post-mortems are released. Recommendations are general guidance. Verify against primary sources before acting on any specific claim.


‍


### **About This Series**


Quantstamp publishes the Security Beat monthly. We've conducted 1,300+ audits and secured $500B+ in digital assets across 250+ clients, including Ethereum Foundation, Aave, Polymarket, Ethena, Visa, OpenSea, Maker, Curve, Compound, and Lido. If you'd like to chat about anything security or request an audit, check out quantstamp.com.


‍
