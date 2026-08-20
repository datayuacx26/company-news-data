---
schema_version: "1.0.0"
document_id: "1d36af86df448ea4a54dd26d34f12a7f52c6f75b4ff50d071762f51427c146ff"
company_key: "yc-zerodev"
company: "ZeroDev"
source_id: "yc-zerodev-news-import-954b0c29e56a"
canonical_url: "https://www.zerodev.app/blogs/kernel-v1-v4-what-production-kept-and-what-it-left-behind"
published_at: null
first_seen_at: "2026-08-14T00:50:37.414550+00:00"
fetched_at: "2026-08-14T00:50:38.809185+00:00"
content_hash: "sha256:13a77936341c0d9578c763baef9271707d5516b7dc7957a79e24d5216990412f"
---

# Kernel v1 to v4: What Production Taught Us About Smart Accounts

*Part 2 of 3 in ERC-4337: A Retrospective, written by Taek Lee*


Across four versions, Kernel moved from extending an EOA-shaped account to replacing its root, testing modularity, and keeping what production justified.


ERC-4337 gave the ecosystem a shared place to experiment. Kernel records what happened when one account implementation followed real product demand through that experiment.At ZeroDev, I led the design and development of Kernel from v1 through v4 and worked on the paymasters, bundling infrastructure, SDKs, and developer-facing account stack around it. I focus here on the demands, defaults, and corrections that shaped the account across those versions.The sequence is easiest to understand as four questions:


- v1: How little can change while still improving the EOA experience?
- v2: Can the account outlive its credential?
- v3: How much authority and execution can become modular?
- v4: Which of those ideas deserve a stable foundation?


End users rarely selected an account architecture directly. Most evidence came from teams integrating Kernel and from the behavior their products produced. When I say a capability survived, I mean it repeatedly justified its integration and maintenance cost.


Kernel v1: Start With What People Know


Kernel v1 was EOA-rooted by design. Wallets and developers already knew how to reason about ECDSA ownership. If the root model felt unfamiliar, every capability above it would become harder to ship and explain.The account stayed deliberately small:


- Ownership: one EOA remained the unconditional root.
- Execution: the account supported calls and batching.
- Gas: a paymaster could sponsor the operation.
- Delegation: an owner-signed plugin authorization could remove repeated prompts for a bounded period or purpose.


That delegation was useful, but it was not durable account state. The owner authorized the plugin, validity window, and plugin data as part of the UserOperation. The account could open another action path without installing a persistent, general permission model.The product questions appeared immediately. Which contracts and functions could the delegated key call? How long would it remain valid? How much value could it move? Could a narrow-looking permission approve a third party that later drained the token?Convenience without durable constraints is not a complete security model.The ECDSA owner remained the structural limitation because it could still execute regardless of what the plugin required. Every added authority stayed subordinate to that unconstrained root.That became a product problem when teams asked for passkeys as the actual owner rather than as a convenience layer beside an unseen ECDSA key. A passkey is not the root if another credential can always override it.v1 taught me that compatibility matters more than elegance at the beginning. It also exposed a distinction that became central later: v1 preserved a familiar credential model, but it did not preserve the user’s existing address. The smart account still began with a new asset and identity history.


Kernel v2: The Account Outlives the Key


Kernel v2 moved beyond “EOA plus helpers.” The default validator became account state that could be selected and replaced, while additional validators and execution rules could remain installed for narrower paths.That enabled several ownership models:


- A passkey or WebAuthn validator could become the root.
- A two-factor or multisignature rule could become the root.
- A team could rotate signers without moving the account.
- An obsolete or compromised validation scheme could be replaced.


Once the account decides what counts as authorization, replaceable validation becomes a deeper abstraction than batching or sponsorship.


The account outlives the key.


That is also the foundation for recovery. With an EOA, key loss and account loss are the same event unless custody or wallet-side infrastructure adds recovery from outside the account. A smart account can represent recovery through another validator, a guardian set, a second device, a delay, or a team rule.Session keys also became persistent account state in v2 instead of being reconstructed inside each operation.Passkey ownership was the immediate demand. The broader lesson was that execution convenience gets attention, while replaceable validation gives the account longevity.


Kernel v3: The Wallet Becomes an Operating System


Kernel v3, initially built around EntryPoint v0.7, was the most ambitious version. It adopted[ERC-7579](https://eips.ethereum.org/EIPS/eip-7579) as a shared interoperability boundary for modular accounts.ERC-7579 defined four module categories: validators authorize, executors act on behalf of the account, fallback handlers extend its callable interface, and hooks apply checks around execution.Kernel then layered a more expressive permission model on top: one signer composed with one or more policies, scoped to particular execution paths. v3 also added WebAuthn and P256 signers, richer session keys, and support for evolving EntryPoint behavior.[v3.3](https://github.com/zerodevapp/kernel/releases/tag/v3.3) added EIP-7702 support.Once an account hosts validators, executors, hooks, policies, signers, upgrades, and fallback behavior, its security boundary moves to composition and the wallet starts looking like a small operating system:


- Which module may authorize this call?
- Which policy wins when several apply?
- Can a hook change state another module assumes is stable?
- Can two safe modules compose into authority neither author intended?
- Who audits the installed composition rather than only the pieces?


Late binding creates interoperability. It also makes the wallet, SDK, application, or user assembling the system responsible for the final authority graph.Kernel could scope authority by target, selector, value, token, time, and other conditions. A trading session could call one router, spend at most 500 USDC, and expire that evening.In the integrations I saw, however, teams usually chose the blanket sudo path rather than composing the narrow policy the account could enforce.


We shipped a scalpel. Most integrations used it as an on-switch.


That usage reflected the product around the permission engine. sudo was the shortest SDK path. Bounded-permission UX was immature. ZeroDev also disproportionately served applications that wanted the wallet to disappear rather than products building a permission-management interface.Teams rarely used that precision because the product had not made it feel necessary or easy.


Kernel v4: Converge


Kernel v4,[https://github.com/zerodevapp/kernel/pull/152](https://github.com/zerodevapp/kernel/pull/152) , kept the ERC-4337 and ERC-7579 model but rewrote the core around the ideas that had proved durable across v1-v3 and current integrations.v4 itself is not yet a long-running production result. It codifies those lessons into the foundation we expect to carry forward.v4 moved to EntryPoint v0.9, made EIP-7702 a first-class account variant, retained replaceable validators and signer-plus-policy permissions, added explicit root replacement and enable-mode installation, and separated three account forms: an upgradeable smart account, a deployment variant with an immutable ECDSA fallback signer, and a 7702-delegated EOA.v4 stopped treating each new ecosystem idea as a reason to reopen the account’s foundations.Convergence meant:


- Keep ERC-7579 as the module boundary rather than redesigning the taxonomy again.
- Keep replaceable root validation as a first-class account property.
- Keep permissions in the foundation while expecting products to expose narrower, opinionated forms.
- Treat EIP-7702 as its own account form rather than a compatibility bolt-on.
- Remove redundant helpers and split validators out of the core so the account itself carries less historical weight.


A clean implementation does not erase deployed v1–v3 accounts. They remain part of the support surface. Convergence stops adding a new architectural thesis to that burden with every release.


What Survived


Part 1 looked at ERC-4337 from the network level. Kernel’s history shows which account ideas were worth preserving even when they were not the largest volume driver.


- Replaceable root validation stayed. It lets an account survive a lost device, signer rotation, new authenticator, or future signature scheme without moving assets or identity.
- A narrow execution core stayed. Calls, batches, and account-defined validation belong in the common surface. Product behavior belongs above it.
- Modularity stayed as an implementation boundary. ERC-7579 helps accounts and modules interoperate. “Your wallet has modules” is rarely the user story.
- Delegated authority stayed for named workflows. Session keys and permissions are valuable when the scope is obvious, safe by default, and attached to repeated activity.
- EIP-7702 became a first-class path. The same account logic can now reach an existing EOA address rather than only a newly deployed smart-account address.


Sponsorship remained the clearest immediate product win. Replaceable validation remained the deepest account primitive. One removes friction now; the other preserves the account over years.


What Remained Outside the Default


Maximum programmability alone did not give users a reason to adopt an account. Capabilities stuck when a product could translate them into something concrete: no gas, fewer prompts, a passkey, recovery, team control, or a bounded session.General permission composition likewise remained outside the default integration path. I expected fine-grained delegated authority to become mainstream faster. The demand was real, but the packaging and timing were wrong.Several incentives had to align at once: the developer had to model authority correctly, the SDK had to make safe scope easier than sudo, the wallet had to explain the grant, and the application had to gain enough visible value to carry the work. We had not aligned all of them.The shortest SDK path effectively became the product.I would build fine-grained permissions again, but I would make safe scope the shortest path. I would package permissions around product concepts—trade on this venue, play this game, spend this allowance—rather than make contract selectors the primary interface. General policy composition would remain available for teams that genuinely need it.Capabilities that required ecosystem-wide coordination rarely became defaults before any one actor could show value.Cross-chain ownership is one example. A validator, module set, nonce, deployment, and recovery state are local to each chain. Keeping them globally consistent introduces proof, messaging, and liveness dependencies; leaving them independent permits authority to drift.Signature aggregation had a similar shape. Savings appeared only after enough accounts used the same scheme, while bundlers had to support a specialized validation and denial-of-service path before traffic justified it.[ERC-7766](https://eips.ethereum.org/EIPS/eip-7766) was withdrawn because aggregateable signature schemes had no production adoption in the contemporary AA market.By contrast, a sub-account is product-shaped. A parent owns the relationship, one application receives a child account, a limit defines the blast radius, and revocation has an obvious object. One team can ship the whole experience without waiting for a shared lane.


The Adoptability Test


Before treating an account capability as a product, I would now ask four questions:


- Is the user pain concrete and already visible?
- Can one actor solve it end to end?
- Can that actor capture enough value to carry the complexity?
- Can everyone else remain unchanged until the benefit is visible?


Gas sponsorship and embedded accounts passed because one product could carry each experience end to end. Replaceable validation passed more slowly because a wallet had to package it as passkey ownership, recovery, signer rotation, or team control.Fine-grained permissions required developers to model authority that wallets could not yet explain. Cross-chain ownership depended on coordination among chains, proof systems, wallets, and account implementations. Aggregation needed bundler support, compatible account schemes, mempool rules, and demand to arrive together.In practice, the ideas that shipped were the ones one team could explain and carry end to end.Kernel showed both that an account should outlive its key and that programmable authority still needs a product people can adopt.Part 3 asks how replaceable validation can reach an address that already matters.


## Related articles


[Developers 9 min read ERC-4337 : Final, Not Finished A builder’s retrospective on what ERC-4337 proved in production: less a path to mass smart-account migration, and more a shared rail for gas sponsorship, embedded onboarding, and programmable accounts. August 12, 2026](https://www.zerodev.app/blogs/erc-4337-final-not-finished)[Developers 9 min read How to Design Cross-Chain Wallet Funding Without Manual Bridging Learn how to design a cross-chain wallet deposit flow using one recognizable address, supported token routing, clear status updates, and recovery workflows. August 4, 2026](https://www.zerodev.app/blogs/how-to-design-cross-chain-wallet-funding-without-manual-bridging)[Developers 10 min read How Robinhood Chain Apps Can Simplify Wallet Funding With ZeroDev Smart Routing Address Robinhood Chain apps can simplify wallet funding with ZeroDev Smart Routing Address, giving users one deposit address that routes supported funds into the right smart account so they can fund once and keep moving. July 21, 2026](https://www.zerodev.app/blogs/how-robinhood-chain-apps-can-simplify-wallet-funding-with-zerodev-smart-routing-address)
