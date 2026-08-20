---
schema_version: "1.0.0"
document_id: "c56115ef2fc3d1c4fac00720ca4b7b1d62429f32514761f0f698c3594353dcd7"
company_key: "yc-zerodev"
company: "ZeroDev"
source_id: "yc-zerodev-news-import-954b0c29e56a"
canonical_url: "https://www.zerodev.app/blogs/build-once-deploy-everywhere-introducing-the-zerodev-omni-sdk"
published_at: null
first_seen_at: "2026-07-24T08:02:55.252814+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:23057a593c2c7ac7a2176e8dcc76b559e3aca33b9e0aae8de3e5f38ba84c3af9"
---

# Build Once, Deploy Everywhere: Introducing the ZeroDev Omni SDK

Most web3 developers are tired of the "JavaScript Tax." If you want to build a high-performance mobile app or a scalable backend service, you’re usually forced to choose between wrapping a clunky webview or rewriting complex account abstraction logic from scratch in a dozen different languages. It’s a massive drag on builder velocity.


At ZeroDev, we believe your infrastructure shouldn't dictate your language stack. Today, we’re releasing the alpha of the Omni SDK: a high-performance, cross-platform ERC-4337 smart account SDK designed to move as fast as you do.


One Core, Every Language: Why We Chose Zig


Engineering for scale means eliminating technical friction at the root. Instead of building six different SDKs that will eventually drift apart in features and security, we built one core in Zig.


Why Zig? Because it provides the safety of a modern language with the performance of C. By using a Zig core with a C FFI (Foreign Function Interface) layer, we’ve created a single source of truth for Kernel logic, UserOperations, and EntryPoint interactions. This means whether you are calling ZeroDev from a Swift-based iOS app, a Go backend, or a Rust microservice, you are getting the exact same hardened, optimized performance.


Native Mobile AA: No More Webview Hacks


The current state of mobile web3 is, frankly, full of friction. Developers shouldn't have to "juggle" infrastructure just to get a gasless transaction to work on an iPhone.


The Omni SDK starts with a native Swift binding. This isn't a wrapper; it's a native experience designed for the first five minutes of a user's journey. It’s built to keep your users from churning by making smart accounts feel inescapably normal.


- Swift (iOS + macOS): Available now via SPM (Swift Package Manager).
- Coming Soon: Kotlin (Android), Go, Rust, and Python support.


Technical Deep Dive: Getting Started


The Omni SDK is engineered for immediate clarity. Here is how you can move from "zero to userop" across different environments.


Swift (iOS + macOS)


You don't need to know Zig to use this. Just add the package and start building.


Swift


` import ZeroDevAA`


` // Initialize your context with ZeroDev middleware for gasless flow`


` let ctx = try Context(projectID: projectID, chainID: 11155111, gasMiddleware: .zeroDev)`


` let signer = try Signer.local(privateKey: pk)`


` let account = try ctx.newAccount(signer: signer, version: .v3_3)`


` // Send a transaction without worrying about the underlying infra`


` let hash = try await account.sendUserOp(calls: \[Call(target: addr)\])`


` let receipt = try await account.waitForUserOperationReceipt(useropHash: hash)`


Go Backend Implementation


For those building high-throughput backend services, the Go binding offers a clean, efficient interface.


Go


` ctx, _ := aa.NewContext(projectID, "", "", 11155111, aa.GasZeroDev, aa.PaymasterZeroDev)`


` defer ctx.Close()`


` signer, _ := aa.LocalSigner(privateKey)`


` defer signer.Close()`


` account, _ := ctx.NewAccount(signer, aa.KernelV3_3, 0)`


` defer account.Close()`


` hash, _ := account.SendUserOp(\[\]aa.Call{{Target: recipientAddr}})`


Rust


If your roadmap requires the absolute peak of safety and performance, our Rust bindings (with automatic Drop support) are ready for the boardroom.


Rust


` let ctx = Context::new(project_id, "", "", 11155111, GasMiddleware::ZeroDev, PaymasterMiddleware::ZeroDev)?;`


` let signer = Signer.local(&private_key)?;`


` let account = ctx.new_account(&signer, KernelVersion::V3_3, 0)?;`


` let hash = account.send_user_op(&\[Call { target: addr, value: \[0u8; 32\], calldata: vec!\[\] }\])?;`


Flexible Signers: Async is the Future


A major pain point in mobile development is integrating with modern wallet providers like Privy or WalletConnect. These signers are inherently asynchronous.


The Omni SDK handles this gracefully with Signer.async(impl). We’ve eliminated the need for developers to write complex boilerplate to bridge async signing events into the synchronous flow of a UserOperation. Whether you're using HSMs, MPC wallets, or local keys, the interface remains unified.


Architecture at a Glance


The repository is structured to maximize transparency and extensibility:


- src/core/: The "engine room"—AA primitives, Kernel, and Bundler logic.
- transport/: Pluggable HTTP/JSON-RPC clients.
- bindings/: Language-specific layers (Swift, Go, Rust, Kotlin, Python).


FAQ: What You Need to Know


What is the ZeroDev Omni SDK?


The ZeroDev Omni SDK is a high-performance, cross-platform ERC-4337 smart account SDK written in Zig. It allows developers to integrate advanced account abstraction features—like gas sponsorship and flexible signing—across Swift, Go, Rust, Kotlin, and Python using a single, unified core.


Is the Omni SDK production-ready?


We are currently in Alpha. It is perfect for teams looking to accelerate their mobile and backend roadmaps and get a head start on native integrations. If you are building for the next million users, this is your foundation.


Does it support gasless transactions?


Yes. The SDK has built-in support for ZeroDev gas and paymaster middleware, allowing you to eliminate gas friction for your users entirely.


Can I use my own signers?


Absolutely. We support local private keys, JSON-RPC, and custom SignerProtocol implementations for wallets like Privy or hardware-backed solutions.


Transform Technical Friction into Business Flow


The era of fragmented web3 development is ending. You shouldn't have to be a Zig expert or a C engineer to build high-performance smart accounts. With the Omni SDK, you can focus on your product, not the infrastructure.


Ready to build what you couldn't before?


[Reach out to ZeroDev](https://zerodev.app/book-a-demo) for a walkthrough on how to fit the Omni SDK into your product roadmap.


## Related articles


[Announcements 8 min read Introducing ZeroDev Wallet ZeroDev Wallet completes the ZeroDev platform, giving teams one place to build embedded wallet onboarding, smart account execution, gasless transactions, recovery, permissions, and automation. July 7, 2026](https://www.zerodev.app/blogs/introducing-zerodev-wallet)[Announcements 2 min read ZeroDev Is Live on Robinhood Chain ZeroDev is live on Robinhood Chain, giving developers account abstraction infrastructure for building better onchain apps from launch. July 1, 2026](https://www.zerodev.app/blogs/zerodev-is-live-on-robinhood-chain)[Announcements 4 min read Solana Gas Sponsorship: ZeroDev Brings Seamless UX to the SVM Eliminate gas friction on Solana. Use ZeroDev’s Solana paymaster to sponsor transactions for Phantom, multisigs, and PDAs. Build for velocity and scale. ZeroDev’s Solana Gas Sponsorship allows Solana-based applications to sponsor transaction fees for users. It works across traditional wallets like Phantom, multisigs, and Program Derived Addresses (PDAs). By removing the requirement for users to hold SOL for gas, developers can eliminate onboarding friction and significantly improve user retention. April 14, 2026](https://www.zerodev.app/blogs/solana-gas-sponsorship-zerodev-brings-seamless-ux-to-the-svm)
