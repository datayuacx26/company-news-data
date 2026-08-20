---
schema_version: "1.0.0"
document_id: "72a1541d150725f755a88078ae8304db81c2269c18e64ec12ff588888aae44ec"
company_key: "yc-zerodev"
company: "ZeroDev"
source_id: "yc-zerodev-news-import-954b0c29e56a"
canonical_url: "https://www.zerodev.app/blogs/the-eoa-transition-an-in-place-path-to-native-aa"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T18:12:26.240405+00:00"
fetched_at: "2026-08-18T18:12:27.471842+00:00"
content_hash: "sha256:eedcc066af301e04afca39110c04278692c8563e96ef69673ed23f545ac94b1e"
---

# The EOA Transition: An In-Place Path to Native AA

*Part 3 of 3 in ERC-4337: A Retrospective, written by Taek Lee.*


ERC-4337 proved that programmable accounts can work, and Kernel showed that an account can outlive its key. An existing EOA still needs to change its root authorization without changing the address where its assets, approvals, and history already live.


ERC-4337 gave Ethereum a production sandbox above consensus. Kernel used that sandbox to test replaceable validation, delegation, permissions, modularity, and different account forms.Ethereum’s existing accounts need a better authorization model without first becoming new accounts. The migration unit should be the authorization model, not the address.


Why Relocation Did Not Scale


The original consumer account-abstraction story assumed that a sufficiently capable smart account would pull users away from EOAs. Recovery, passkeys, batching, sponsorship, session keys, and replaceable validation would justify creating a new address and moving into it.That forecast combined three different markets:


- App and embedded wallets can create an account during onboarding. There is no established identity to preserve, so the product can select the account model, sponsor gas, and own the signing experience end to end.
- Consumer main wallets begin from incumbency. A new address competes with an account that already holds assets, approvals, history, counterparties, governance identity, and habits.
- Organizational accounts solve an unavoidable coordination problem. Teams and treasuries need shared control, signer rotation, and explicit authorization rules that a single-key EOA cannot represent.


Each market faced a different adoption problem. App accounts could win by starting from zero. Organizational accounts could win because coordination already hurt. A consumer smart account had to be better enough to justify relocating an existing crypto identity.“Your wallet is programmable” was an abstract benefit attached to a concrete migration cost.Standalone smart accounts remain the right starting point for many greenfield products and organizations. Expecting the same relocation model to move Ethereum’s installed consumer base was the mistake.


EOAs Are the Migration Surface


An established account is where assets sit, approvals live, counterparties send funds, applications cache assumptions, tax and accounting tools track activity, and identity accumulates over time.EOAs also impose very few operational requirements:


- No deployment: the account exists anywhere its key is recognized.
- Stable identity: the same address remains meaningful across applications and chains.
- Universal compatibility: wallets, explorers, bridges, indexers, and contracts already understand its ordinary transaction path.
- Predictable limitations: the account cannot express much, but the ecosystem understands the little it can express.


Those properties make EOAs the most practical migration surface for a better account model. Native account abstraction must reach this installed base and replace the permanent secp256k1 root.Kernel taught us that an account should outlive its key.[EIP-7702](https://eips.ethereum.org/EIPS/eip-7702) showed that an address can outlive the code model attached to it. Native AA should combine those lessons by keeping the account where it is while allowing the authorization recognized at its root to change.


What the Non-Native Path Taught


ERC-4337 improved the user experience by moving complexity from users into wallets, SDKs, and infrastructure.Wallets and SDKs absorbed account deployment, validators, signing modes, gas estimation, sponsorship, and version compatibility. Infrastructure providers absorbed bundling, simulation, mempool policy, abuse prevention, pricing, and error attribution. Auditors inherited a larger authority surface across validators, modules, hooks, policies, and upgrades.The paths differ in their steady-state requirements:


- EOA steady state: build and sign a transaction, broadcast it, and wait for inclusion.
- ERC-4337 self-funded steady state: build and sign a UserOperation, pass bundler simulation and acceptance, submit an enclosing transaction, validate through the EntryPoint, and execute through the account.
- ERC-4337 sponsored or first-use flow: add sponsor approval and, when needed, counterfactual deployment, factory logic, and initialization.


Each extra layer creates another place where gas estimation, latency, compatibility, or error attribution can fail.The extra surface bought Ethereum the ability to test account abstraction without a fork. Native AA should preserve the capabilities that justified their cost while retiring mediation that existed only because the base protocol did not understand programmable authorization.


Where EIP-7702 Stops


EIP-7702 lets an EOA authorize persistent delegation to code while retaining the same address. Batching, sponsorship, contract-style execution, and delegated behavior can therefore reach an established account without first moving assets or recreating identity.EIP-7702 changes the migration shape:


- Continuity is preserved. Assets, approvals, history, and counterparties remain attached to the address.
- Upgrade-in-place becomes credible. Programmable behavior can reach the installed base without a relocation event.
- The root problem becomes explicit. The original secp256k1 key still authorizes future delegation changes and can replace or clear the delegated code.


Delegated code may implement passkeys, multisig, recovery, policies, or future validation schemes for actions routed through it. It cannot make Ethereum forget the ECDSA authority that created the delegation.EIP-7702 demonstrates a credible and likely long-lived in-place upgrade path while leaving the authorization transition incomplete. A final account model must allow permanent removal of the old root.


Root Replacement Has Two Layers


The desired end state keeps the same address, assets, approvals, and history while replacing validation and removing every residual authority of the old key.Removing the old key has two layers.


1. Transaction authorization must change at the protocol level. The new scheme must become authoritative for top-level transactions, delegation changes, and every route that could restore the old credential.
2. Message authorization must change at the application level. Contracts and applications that equate an ECDSA recovery result with account authority must learn how to follow the account’s current validation scheme.


Transaction authorization and message authorization are separate migration surfaces.The ecrecover precompile makes the application layer difficult. It will still recover the same address from a valid signature by the historical key. A contract that treats that result as authorization will not automatically follow a protocol-level root change.[ERC-1271](https://eips.ethereum.org/EIPS/eip-1271) gives contract accounts an interface for defining whether a signature is valid. Many existing consumers bypass that interface. Some assume a 64- or 65-byte ECDSA signature, pre-screen v and low-s values, or call ecrecover without attempting account-defined validation.Those applications need an explicit compatibility and migration path. The system must also distinguish signatures that should stop working from historical signatures that were intentionally durable.A root transition is incomplete if the protocol rejects the old key but applications still accept it as current authority. The reverse is also incomplete if applications recognize a new validator while the old key can still retake the account.The rollout is cross-chain as well. The same address may matter on many L1s and L2s, but authorization state and protocol support are local. A transition that completes on one network can leave the old root active on another.


Authority Must Be Legible


An ordinary transaction approval usually asks whether the user wants to perform an action now. A smart-account approval may also install a validator, create a session, change recovery, replace the root, or grant authority that survives the current action.Consider a session key.The low-level artifact may communicate:


- Sign message: 0x9af2c1b7e0d4...e83a


The user needs to understand:


- Which application receives authority.
- Which calls it may make.
- Whether the 500 USDC limit is cumulative or per transaction.
- When the grant expires and how it can be revoked.
- Whether it can install modules, replace the root, or widen its own scope.


Those descriptions may refer to the same authorization. One trains the user to approve bytes. The other explains the authority being created.The draft[ERC-7739](https://eips.ethereum.org/EIPS/eip-7739) is designed to improve domain separation while preserving readable typed-signature content for smart accounts. It does not define what a permission, session key, module installation, recovery change, or root replacement means to a user.A wallet must still explain where delegated authority lives, what it can do, whether it can renew itself, and what revocation removes.Smart accounts can be safer through replaceable validation, recovery, limits, passkeys, guardians, and delays. They can also move risk into validators, modules, upgrades, external dependencies, and composition.“Smart accounts are safer” is too broad. Safety depends on whether authority is constrained, reviewable, and recoverable.


What Native AA Should Preserve


Native account abstraction only needs the ERC-4337 components that production proved valuable. ERC-4337 reconstructed an account-aware transaction path above the protocol because that was the available way to experiment safely. Once the protocol understands programmable authorization, it can remove unnecessary coordination layers.I would expect six properties:


1. Preserve continuity and replace the root. An established account should keep its address and state while changing the authorization scheme recognized for top-level actions. The old path must be permanently removable.
2. Keep the account as the caller. Validation should establish consent and then let the account call outward as itself. Applications should continue to see the account address as msg.sender.
3. Separate authorization, payment, and execution. Clients, receipts, explorers, and wallets should be able to identify who authorized, who paid, which account executed, and where a failure occurred.
4. Keep the common execution shape narrow. The shared envelope should cover proven needs—validate, fund, execute, and batch—rather than enshrine every modular experiment.
5. Let the ordinary path use ordinary infrastructure. Specialized operators may still add sponsorship, privacy, aggregation, or latency guarantees, but a native account transaction should not require a mandatory parallel mempool or coordinator.
6. Expose enough structure for clear signing. The protocol need not dictate wallet copy, but it should make execution, delegation, recovery, and root replacement machine-readable enough for wallets to explain.


Kernel v2 taught us to let the account outlive the key. Kernel v3 taught us that maximum modularity creates an interpretation and security burden of its own. Native AA should enshrine the v2 lesson without importing the entire v3 surface into consensus.


What I Would Carry Forward


ERC-4337 succeeded as a shared experimentation and execution layer. It made gas abstraction operational, supported embedded accounts, produced infrastructure and security knowledge, and showed that replaceable validation has lasting value.The consumer migration many of us forecast did not happen. Users kept the address around which their assets, approvals, history, and counterparties had accumulated; that behavior says little about whether they prefer a permanent ECDSA root.If I were starting again, I would still build sponsorship and replaceable validation. I would package session keys around safe, legible scopes; treat modularity as infrastructure rather than the user story; push clear signing earlier; and separate app-wallet adoption from main-wallet transition from day one.Established accounts need a native-AA path that replaces their root authorization in place. Wallets should make safe scopes the default and clearly explain any broader authority.I am preparing a follow-up article focused specifically on native account abstraction: what belongs in the protocol, what should remain in wallets and account implementations, how root replacement could work, and how applications can migrate without importing the entire ERC-4337 sandbox into consensus.


## Related articles


[Developers 11 min read Kernel v1–v4: What Production Kept and What It Left Behind Across four versions, Kernel evolved from an EOA-shaped smart account into a modular foundation for replaceable validation, permissions, and EIP-7702. Its history reveals which account abstraction capabilities became durable products, and which still need better packaging. August 13, 2026](https://www.zerodev.app/blogs/kernel-v1-v4-what-production-kept-and-what-it-left-behind)[Developers 9 min read ERC-4337 : Final, Not Finished A builder’s retrospective on what ERC-4337 proved in production: less a path to mass smart-account migration, and more a shared rail for gas sponsorship, embedded onboarding, and programmable accounts. August 12, 2026](https://www.zerodev.app/blogs/erc-4337-final-not-finished)[Developers 9 min read How to Design Cross-Chain Wallet Funding Without Manual Bridging Learn how to design a cross-chain wallet deposit flow using one recognizable address, supported token routing, clear status updates, and recovery workflows. August 4, 2026](https://www.zerodev.app/blogs/how-to-design-cross-chain-wallet-funding-without-manual-bridging)
