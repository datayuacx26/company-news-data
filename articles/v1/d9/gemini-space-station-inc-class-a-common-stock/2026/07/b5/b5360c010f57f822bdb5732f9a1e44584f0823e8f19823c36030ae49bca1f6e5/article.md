---
schema_version: "1.0.0"
document_id: "b5360c010f57f822bdb5732f9a1e44584f0823e8f19823c36030ae49bca1f6e5"
company_key: "gemini-space-station-inc-class-a-common-stock"
company: "Gemini Space Station Inc."
source_id: "gemini-space-station-inc-class-a-common-stock-news-import-1c1a410de1c5"
canonical_url: "https://www.gemini.com/blog/why-gemini-wallet-looks-the-way-it-does"
published_at: null
first_seen_at: "2026-07-21T21:23:13.509460+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:0a7e1d66017592c435ef731f08b20bb82b79b927d7acf26efae6158dc4c3eb7d"
---

# Why Gemini Wallet Looks the Way It Does

The Gemini Wallet was built to upend the traditional wallet experience.


EOAs such as MetaMask, Ledger, and Trust Wallet expect users to safeguard a 24-word seed phrase, pay gas from an ETH balance, and recover nothing if a typo sends funds to /dev/null. That user experience terrifies newcomers and burdens power users with unnecessary risk. We knew there was a better way.


The first wave of embedded account-abstraction wallets delivered slick onboarding but at the cost of vendor lock-in. Custody often rested on the provider’s servers, modules were proprietary, and the user would lose sight of their funds after signing up for the 3rd dApp specific wallet.


Three industry milestones changed the calculus in 2024–2025:


-


Passkeys (WebAuthn) graduated from novelty to default on iOS 17, Android 14, and Chrome 122.


-


ERC-4337 left the lab and logged millions of user operations.


-


ERC-7579 introduced a vendor-neutral, upgrade-safe modular account kernel.


Those breakthroughs allowed us to design a wallet with Gemini-grade compliance while honoring the ecosystem’s commitment to permissionless innovation.


### Principles Before Architecture


We started the architecture memo with four non-negotiable principles. First, the wallet could never ask a user to store or export a seed phrase; if a phishing site demanded “recovery words” there would be nothing to steal. Second, it had to be both portable and embeddable; the same wallet should connect to Uniswap in a browser tab and drop seamlessly into a partner dApp with minimal code. Third, future-proofing mattered— we would deliver upgrades through audited modules, not through one-off forks that shatter compatibility. Finally, we vowed to build only on open standards. If we needed a feature, we would extend an EIP, not invent “Gemini-Only 101.” Those principles framed every decision that followed.


### Why Passkeys Won the Key Race


Key custody is the soul of any wallet, and we evaluated four approaches. Plain browser-stored private keys were dismissed immediately; one stray localStorage leak is all it takes to lose your crypto. ruin a life. Email-based “magic links” felt friendly, yet they would leave signing authority on a server Gemini could not control. Raw secure-enclave signatures provided genuine hardware isolation, but every phone vendor would expose a slightly different API surface and the SDK risked collapsing under conditional code.


The architecture was not conceived in a vacuum; it was born from scars. In 2019, I pasted a base64 private key into a startup SDK that logged requests server-side. My wallet was drained within an hour. In 2023 I watched a newcomer hand out his hex private key, wrongly believing a cold wallet prevented remote theft. Those episodes convinced us that if a secret can traverse the clipboard, eventually it will. Eliminating seed phrases was not a convenience decision—it was a mitigation strategy.


Passkeys solved all three painpoints at once. A passkey’s private half never leaves the secure enclave, its public half appears only during creation, and the FIDO standard abstracts away vendor differences. Even a user who wants to export a secret key cannot do so. An entire class of clipboard-based phishing disappears, and so does the horror story of a mentor at ETHDenver who once pasted his hex private key into a group chat and watched his funds vanish before dessert.


### The Core Stack: Passkey Validator, Nexus Kernel, Deterministic Factory


With key custody settled, we needed a smart-account core. We chose the Biconomy Nexus implementation of ERC-7579 because it exposes a lean upgrade surface, aligns closely with the canonical ERC-4337 EntryPoint, and arrives already gas-optimised and audited. On top of that,we built a deterministic factory: each wallet address is pre-computed with CREATE2, using the passkey’s public key as the salt. The benefit is immediate and three-fold:First, the user enjoys the same wallet address across every supported EVM L2 so they never wonder which chain owns which identity. Second, the address exists before deployment; an airdrop can target it even when the account still lives only as a hash. Third, compartmentalisation is trivial: a second passkey— say, a YubiKey— automatically produces a second wallet without cross-contamination.


### Opinionated UX, Un-Opinionated Plumbing


A wallet could ship with the bare kernel and let users piece together functionality, but most people would bounce at the first obstacle. Instead, we installed a small but powerful module set on day one. We collaborated with zerodev to build a WebAuthn validator that turns FIDO signatures into onchain isValidSignature passes; a paymaster sponsors gas so newcomers can perform their first trade without hunting gas tokens; Blockaid scanning runs every transaction through a risk engine before the user clicks “Sign”; an ENS sub-registrar assigns a you.gemini.eth name that doubles as a discovery anchor for future recovery. A guardian-style recovery module is next on the runway, maintained by a sister team.


Every module’s bytecode hash enters an ERC-7484 attestation registry (on which we collaborated with rhinestone) signed by a Gemini attester. If a malicious contract pretends to be an upgrade, the attestation fails and the install halts. The wallet is strict about provenance but relaxed about infrastructure: it trusts any bundler (we collaborated with at launch) whose user operation passes validation, and the paymaster lives in its own module, so swapping bundler providers or incorporating our own is a configuration file, not a redeploy.


### A Permissionless SDK—or Why We Refuse to Sell API Keys


Many account-abstraction providers lock their SDK behind API keys, turning developers into tenants. We chose the opposite path. The MIT-licensed @gemini-wallet/core speaks only to public RPC endpoints; no secret token is required. Builders who want extras—gas sponsorship or deep risk scanning—can point at keys.gemini.com, yet nothing breaks if they self-host or plug in a third-party service. Gemini Wallet shows up automatically in RainbowKit, Wagmi, AppKit, and anywhere that supports WalletConnect selectors. “Just works” was not a slogan; it was a release criterion.


Initially covering gas for every user operation is indeed expensive, but it forced us to design for scale first rather than extract rent later. Infra neutrality keeps us honest.


### Security and Compliance, Not Either-Or


Because Gemini’s exchange already carries SOC 2 attestations and a NYDFS trust license, the wallet stack had to inherit the same rigor. Trail of Bits completed a full audit of the contracts in July 2025 and the PDF is public.


### The Road Ahead


Launching a wallet is not the end of the story. We are also launching an innovative zk-proof based recovery module, giving users a path back from lost passkeys without re-introducing seed phrases. Configurable paymaster budgets follow in later this year so that dApps can sponsor fees without signing blank checks. We intend to offer integrations with custodial balances using the smart wallet as the bridge between exchange and DeFi.


### Try It Yourself


When August 15 arrives, open onchain.gemini.com in any modern browser, click Create with Passkey, authenticate with biometrics, and your wallet— including your own gemini.eth handle— will exist instantly across every supported L2. Gas is on us while you explore. Builders can head straight to for documentation, audits, and contribution guidelines.


### Closing Thoughts


The first generation of wallets made everyone their own bank— and their own security team. Gemini Wallet keeps the sovereignty while removing the home security system tax. Passkeys lock down the secrets, modular accounts future-proof the features, and infra neutrality keeps the ecosystem honest. It is the bridge from exchange-grade guardrails to the permissionless frontier— without seed phrases, without vendor lock-in, and without friction. See you onchain.


### Appendix: Reference Tables


**A1. Day-One Module Set**


Module Purpose Standard


WebAuthn Valdator Converts FIDO passkeys signatures into on-chain passes ERC-757


Gas Sponsor (Paymaster) Gemini covers fees during intro period ERC-4337


Transaction Simulation & Scan Real-time risk analysis before signing Blocked API


ENS Sub-Registrar Auto-assigns you.gemini.eth;aids future recovery ENS


Recovery Module Social recovery without seed phrases ERC-7579
