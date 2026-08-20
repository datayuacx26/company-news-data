---
schema_version: "1.0.0"
document_id: "9f107c7a8ca2331d7c5b9eb1cf31552c34b8949aa76a1fd5197b4dd06d3ace6b"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/alchemy-modular-account-how-audits-can-help-shape-standards-and-catalyze-mass-adoption"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:03eb6a6bc7d61b0921467315e98badf280a0ae5006bf4ada698bec7d2d413c31"
---

# Modular Account: How Audits Can Help Shape Standards And Catalyze Mass Adoption

Quantstamp recently conducted a[smart contract audit](https://certificate.quantstamp.com/full/alchemy-modular-account/2c13aab2-5d5f-4f45-a9ef-b890bdb12b97/index.html) for Alchemy’s[Modular Account](https://www.alchemy.com/blog/hello-modular-account) , a wallet implementation designed from the ground up for ERC-4337 and ERC-6900 compatibility including two plugins. The[ERC-6900 standard](https://eips.ethereum.org/EIPS/eip-6900) is an extension of the ERC-4337 account abstraction infrastructure, further standardizing smart contract accounts and so-called account plugins, which are smart contract interfaces that allow for composable logic within smart contract accounts.


Account abstraction is one of the most promising standards to onboard the first billion users to web3, already experiencing exponential growth. In Q4 2023, over 5,400,000 UserOperations were executed, a 194% increase compared to Q3 2023, according to[Alchemy’s report](https://www.alchemy.com/blog/smart-accounts-adoption-accelerated-in-q4-2023) (). Quantstamp has been committed to making Account Abstraction implementations as secure as possible with audits for the FUN Wallet, Blotco Wallet, and Alchemy’s[Light Account](https://accountkit.alchemy.com/smart-accounts/accounts/guides/light-account.html) . We believe that the ERC-6900 extension of the standard can become an additional catalyst for the ecosystem.


The audited codebase was almost fully compatible with v0.6 of the ERC-6900 standard. In the course of the audit process, some possible improvements to the underlying standard were discovered, both from a security and developer experience perspective, ultimately helping shape v0.7 of ERC-6900.


In this article, we will explain the reasoning behind the most significant changes to v0.7 of the standard. We will also discuss some security considerations for plugin developers.


This blog post assumes a sophisticated level of understanding of the standard and implementation details of the Modular Account, as it will focus on particular changes of a concept that is deeply intertwined with the inner workings of the standard. By that, we mean the concept of hooks.


## What Are Hooks?


Hooks are special functions that can be called as part of the initial validation of an invoked execution function or directly before or after the execution. The set of hooks of an execution function are defined by the installed plugin. Hooks can augment the flow of execution or validation of a User Operation or regular runtime transaction. The idea behind these hooks is to allow for more granular control of conditions around the actual execution functions.


‍


Caption: Two Modular Accounts with plugins installed. If they have the same set of plugins installed, each execution function of the two accounts will invoke the same set of validation and hooks.


However, this granularity does come with its own level of complexity that we will dive into more. Now that we know what they are, why are we so interested in them?


### Why Hooks Are Awesome


The modularity of hooks enables their reuse, all of which are configurable with plugin installations. Pre-execution hooks enable additional constraints that User Operation validation functions are not able to enforce due to ERC-4337’s validation rules (e.g. not being able to access most blockchain states or access storage slots outside of address-associated storage).


Hooks can also ensure that some execution flow only succeeds if some conditions defined in pre- and post-executions are met. Furthermore, the set of hooks remains fully configurable through installation and uninstallation of the hook-providing plugins, so that users can tighten or loosen constraints as desired. Hooks enable maximum flexibility without a contract upgrade being necessary.


### Examples of Hooks


Hooks are powerful. Let’s look at a couple of quick examples:


#### 1. Session Key


An entity can be granted a session key that enables them to withdraw a specific amount of funds from the modular account, as long as certain state in the blockchain is changed (i.e. equivalent amount is added to another wallet of the account owner or into a DeFi position accounted to the owner) as part of the execution of the withdrawal.


#### 2. Flash Loans


The modular account could have an execution function installed that enables flash loans. The execution function transfers the funds, performs the callback into the receiver contract, and asserts that the fees have been returned afterward.


#### 3. Access Controls


Through plugins providing additional hooks to the execution function, the account owner could add complex layers of access control. Not only do these execution hooks enable unrestricted blockchain state access compared to validation hooks, but they can be added or removed as additional requirements to the existing sets of hooks.


## Main Changes for v0.7 and Their Rationale


Below are a few security findings the Quantstamp auditing team discovered and contributed toward fixing alongside Alchemy smart contract engineers.


### 1. Disallow Using Dependencies for Hooks


One of the features of previous implementations of the ERC-6900 standard was the ability to have hook dependencies from other modules. In a sense, this would allow a plugin developer to reference a hook from another plugin that would be executed before or after any of the specified execution or validation functions. Hooks as dependencies are quite complex and have been a topic of discussion in past community calls. Furthermore, they require proper configuration with the intended dependency – however, this is outside of control for the original plugin developer.


This can lead to hooks from a plugin being called in a context that its original developer did not anticipate, such as state-changing validation hooks being called outside of validation or one of a pre- and post-exec hook pair being called without the other, which we pointed out in our report in **MSCA-8** and **MSCA-16** . For example, another plugin could install as a dependency the existing pre-execution hook from another plugin related to updating a session key’s gas usage in a completely unrelated execution flow. This unrelated execution flow could then tamper with the accounting of session keys, completely outside of control for the plugin developer.


This issue was present in both the Modular Account and the reference implementation of the standard. To tackle this concern, Alchemy[proposed a standard update to simplify hooks](https://github.com/erc6900/resources/issues/25) and this change ultimately landed in both ERC-6900 v0.7 and Modular Account.


Furthermore, with the removal of hooks as dependencies, the possibility of duplicate hooks is greatly diminished. Mainly, it would require that within a plugin’s manifest, it repeatedly applies its own hook to the same selector to achieve hook duplication, which seems very unlikely to be necessary. Therefore, the support of duplicate pre-hooks has been removed in the Modular Account and the change has been[forwarded](https://github.com/erc6900/reference-implementation/pull/31) after consultation with the community into v0.7 of the standard.


### 2. Cut Permitted Call Hooks and Injected Hooks


Furthermore, during the audit process, the concept of permitted call hooks and injected hooks were removed[via a standard proposal](https://github.com/erc6900/resources/issues/22) .


Installed plugins can make calls both to other execution functions of the underlying account (executeFromPlugin-flow) and also to permitted external contracts (executeFromPluginExternal-flow). Such behavior is defined in the manifest of the calling plugin and configured in the account during installation.


In version 0.6 of the standard, a plugin was able to provide individual execution hooks for both of these types of calls via so-called “permitted call hooks”, so essentially hooks that are tied to an execution function and the calling plugin. It was originally introduced to enable running of different hooks based on the calling plugin when plugins perform actions via the account.


This idea was further expanded upon with the concept of so-called “injected hooks”, which are essentially permitted call hooks defined outside of the manifest of the plugin that can be customized during each plugin installation, e.g. to enforce additional constraints for the permitted call hooks of the plugin.


Both in past community calls and during the audit process it became evident that these hook types unnecessarily inflate the complexity of the standard and consider too many edge cases that only become relevant for scenarios with a high amount of plugins installed. Both the behavior of permitted call hooks and injected hooks can be sufficiently replicated without being a specific part in the standard: Permitted call hooks can be replicated by the calling plugin encoding specific identifiers into the calldata that the hook-providing plugin accordingly implements switching logic to.


Injected hooks can simply be delegated to an additional plugin implementation that installs execution hooks that preemptively apply hooks to the execution function the plugin the user originally wanted to add additional constraint to. Logic switches can again be used to only apply the constraining hook for calls from the specific plugin.


Added complexity does not only add entry barriers but also adds additional risk for hidden vulnerabilities. For example, in **MSCA-9** , we found that plugins with injected hooks that execute functions defined by themselves would create a deadlock in an uninstallation process, making the user unable to uninstall it again. This is an interesting observation, as the underlying plugin can be completely harmless with a safely defined plugin manifest, but hooks injected to it can partially brick the account.


We definitely support the removal of these hook types, as their removal not only simplifies the life of plugin developers and anyone attempting to understand the standard but also reduces the account’s attack surface.


## Security Considerations Related To ERC-6900


We also want to use this opportunity to raise awareness of some security considerations for plugin developers, ERC-6900 implementations, and their users. While this list is far from exhaustive, it is hopefully useful for the ecosystem:


### 1. Batch Of External Calls Can Invalidate Pre-Execution Hook Assumptions


In case a pre-execution hook is validating some blockchain state before the execution of some batch of multiple external calls, it can be dangerous to assume that none of the calls within the batch invalidate assumptions made in the pre-execution hook.


For example, the pre-execution hook of the session key plugin used to iterate over the batch of calls and compare the calldata for token transfer calls with the blockchain state in the pre-execution state for tracking spend limits of keys. However, the hook failed to account for the fact that an allowance might have already been modified by prior calls in the batch, which ultimately would have enabled circumventing the spending limit, as we pointed out in **MSCA-1** . So, be careful with all blockchain state accesses within pre-execution hooks, especially in a batch-call setting.


### 2. Execution Hooks Can Only Validate Conditions at the Time of Their Own Execution


We briefly alluded to execution hooks having no awareness of other execution hooks being performed in the same execution setting. This reveals an important security consideration: an execution hook can only assure that at the time of its own execution, certain conditions are met, but this can not be generalized to the entire pre-execution context of potentially multiple pre-execution hooks.


For example, a pre-execution hook cannot be assured that the storage it performed validation upon does not get further updated in the following pre-execution hooks. Even an associated post-execution hook potentially repeating the validation cannot assure that the storage remained unmodified, as perhaps a prior post-execution hook has reset the state again.


This is a very nuanced issue that is however important for plugin developers, as users can potentially invalidate requirements set by the plugin developer through the installation of special plugins with storage tampering hook pairs. For more details, refer to **MSCA-7** of our report.


### 3. Plugins Are Trusted Components, and Malicious Plugins Can Cause Arbitrary Consequences


Furthermore, it should come at no surprise that plugins are trusted components of a modular account. The Modular Account can be thought of as a quite blank canvas, enabling almost arbitrary configuration through installed plugins. If a malicious plugin were to be installed, it could theoretically cause arbitrary consequences, including loss of funds.


We outlined some examples of consequences of plugin misconfiguration in **MSCA-10** . Worth mentioning is also **MSCA-5** , where a malicious plugin could theoretically add always reverting hooks to native execution selectors such as the ones for plugin uninstallation or account upgrades. So, the power of hooks on the one side is awesome, since it enables almost unbounded implementation freedom for plugin developers intending to extend the capabilities of a modular account, but also leaves room for numerous shenanigans.


### 4. Add Reverting Hooks So Other Plugins Can’t Enable a Plugin’s Execution in Undesired Contexts


However, even though plugins are trusted, plugin developers can add some protection measures to their plugins to avoid potential misuse of their plugin through additional plugin installations. A notable example of this is plugins intended for the context of user operations only, such as the session key plugin in Alchemy’s modular account implementation.


In **MSCA-6** , we identified that even though session keys are only intended for user operation context, the plugin manifest does not define a natively always reverting runtime validation function. This theoretically enables a malicious plugin to add an always passing runtime validation to those execution functions, circumventing all the parameter-validation that would have happened in the user operation validation context, essentially granting arbitrary access to the session key, as no matching signature has to be provided.


As a fix, the Alchemy team simply added an always reverting pre-runtime validation hook to the plugin manifest, fully disabling the successful passing of the runtime validation setting. Alchemy further noted that it will potentially consider a more general fix for this issue in future revisions of the standard. For now, if plugin developers implement a plugin that is only intended to operate within runtime or user operation context, it is advisable to add a reverting hook to the counterpart, so that other plugins may not enable the plugin’s execution in the undesired context.


## What’s Next?


We hope you enjoyed the look behind the curtain of the path to ERC-6900 v0.7. This has been a particularly special project for us; not only did we get an early glimpse into the future of Account Abstraction, but also enabled us to meaningfully contribute to an emerging standard. As auditors we usually get a holistic view of a finalized system, but having the opportunity to help flesh out a standard that will play a vital role in the space has been fascinating. We are proud to be a part of the journey.


Our full report can be viewed[here](https://certificate.quantstamp.com/full/alchemy-modular-account/2c13aab2-5d5f-4f45-a9ef-b890bdb12b97/index.html) **** along with the modular account repository[here](https://github.com/alchemyplatform/modular-account) **.** Given our excitement about Account Abstraction and the broader ERC-6900 ecosystem, Quantstamp is offering preferential audit priority and pricing for teams developing in the account abstraction ecosystem.


We also want to highlight what a pleasure it has been to work again with the Alchemy team. The team has been excellent to work alongside and the work they put in has been incredibly diligent and detailed. We are extremely excited to follow the journey of the modular account and ERC-6900 in general.


If you want to follow or contribute to the modular account ecosystem or ERC-6900 the bi-weekly community calls are a great place to start, as well as the following set of links!


‍


- [Twitter](https://twitter.com/erc6900)
- [Reference Implementation](https://github.com/erc6900/reference-implementation)
- [Alchemy’s Modular Account Implementation](https://github.com/alchemyplatform/modular-account/)
- [EIP](https://eips.ethereum.org/EIPS/eip-6900)
- [Telegram](https://t.me/modular_account_standards)
- [HackerOne Bug Bounty](https://hackerone.com/alchemyplatform)
