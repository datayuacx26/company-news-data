---
schema_version: "1.0.0"
document_id: "1b52aedcec2d9c3818d696e0d176c2342cc4bd9b8dd2b851583d9fe7e9d5b055"
company_key: "yc-stackup"
company: "Stackup"
source_id: "yc-stackup-news-import-4adfae6a34ed"
canonical_url: "https://www.stackup.fi/resources/passkeys-webauthn-erc4337"
published_at: null
first_seen_at: "2026-07-27T05:25:45.190590+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:a9287b0a8fdf48b5c0e10ff3749b04077a30c67e13f1b0a421a0bdfe64559992"
---

# How to Use Passkeys on Ethereum

One of the promises of account abstraction is completely getting rid of private keys for users while maintaining strong security and self-custody. However, many smart accounts fall short of this vision because there are fewer libraries for implementing alternative authentication methods. This leads many app developers to rely on less user-friendly methods or third party MPC and social login services. These are generally a better user experience than giving a user a private key, but still falls short of the full vision. What if we could get rid of these dependencies altogether?


Enter passkeys. Passkeys represent a transformative step forward, offering a secure, user-friendly authentication alternative that leverages biometrics, PINs, or patterns, and eliminates the need for mnemonic phrases.


This guide explores how developers can implement passkey authentication in Ethereum applications, leveraging ERC-4337 to create a seamless, secure user experience without compromising on self-custody.


## Understanding Passkeys and Their Role in Blockchain Accessibility


The goal of account abstraction is to make blockchain applications more accessible to the general public, and passkeys are a great way to make web3 authentication as seamless as any other application or mobile app. By incorporating passkeys, we can make the authentication process as straightforward as using any conventional app, hiding the complexity of underlying technologies like face ID or fingerprint recognition from the user.


## Implementing Passkeys with the ERC-4337 Standard


To enable passkeys on Ethereum, the user’s account needs to be an ERC-4337 smart contract that can verify P256 signatures. This standard enables transactions without traditional private keys, a limitation on Ethereum and other EVM-compatible blockchains such as BnB Smart Chain, Optimism, and Aribtrum.


There are two general steps you need to take to enable passkeys on Ethereum.


1. **Client-Side Signing with Passkeys:** Use the WebAuthn API to generate and sign passkeys on the user's device. While creating local passkeys varies by device, the process typically follows a standard pattern. For more details on passkey generation, refer to resources like[Web.dev's great passkey implementation guide](https://web.dev/articles/passkey-registration) .
2. **Setting Up Smart Accounts for Passkeys:** Implement a smart account capable of verifying passkey signatures. These signatures are often referred to as P256 signatures or by the elliptic curve secp256r1. Implementing this involves modifying the validateUserOp function within your smart contract to include the P256 signature verification algorithm. For those seeking to minimize development time, consider using established smart account frameworks like Trust Wallet's Barz or exploring the[Daimo p256-verifier on GitHub](https://github.com/daimo-eth/p256-verifier) . An ongoing proposal, EIP-7212, suggests precompiling this contract to reduce gas costs associated with signature verification, but as of February 2024 is only live on Polygon.


## Creating Transactions with Passkeys


The first user operation of a new account always needs to include an initCode to deploy the account. The initCode is typically a hex concatenation of a smart account factory and an encoded call to that factory to create a new account owned by the passkey. You can see a simple implementation of this in[this GitHub repository](https://github.com/passkeys-4337/smart-wallet/blob/f3aa9fd44646fde0316fc810e21cc553a9ed73e0/front/src/libs/smart-wallet/service/userOps/builder.ts#L204) .


You can then create the user operation like normal, but when you get the signature from the user you will need to use the WebAuthn API to get a signature from the passkey. In ERC-4337, you will typically sign the hash of the user operation.


You can see a simple implementation of this in[this passkey demo](https://github.com/passkeys-4337/smart-wallet/blob/f3aa9fd44646fde0316fc810e21cc553a9ed73e0/front/src/libs/smart-wallet/service/userOps/builder.ts#L140) or in[this builder in the userop.js package](https://github.com/stackup-wallet/userop.js/blob/8e78819d6e93193ab67f2fb87f2b80906444fae4/src/preset/signers/barzSecp256r1.ts#L28) .


## Setting Up Smart Accounts To Use Passkeys


Now that you have a system of creating passkeys and signing messages with them, you’ll need a smart account that can verify p256 signatures. In ERC-4337 the verification of signatures occurs in the validateUserOp function. This is where you will include the p256 signature verification algorithm.


One implementation of this logic can be used from[this P256 signature solidity contract](https://github.com/daimo-eth/p256-verifier) , though there is a proposal,[RIP-7212](https://github.com/ethereum/RIPs/blob/master/RIPS/rip-7212.md) , to precompile this contract on the Ethereum blockchain to reduce the gas fees associated with this curve.


If you don’t want to write the entire smart contract from scratch yourself, you can also use one of a handful of smart account implementations that already exist. An implementation of a smart contract wallet that uses P256 signature passkeys is[Barz by Trust Wallet](https://github.com/stackup-wallet/userop.js/blob/main/src/preset/builder/barz.ts) . Their implementation has the advantage that it is audited and in production by one of the largest Ethereum wallets in the world, though it is not modular. ZeroDev's[Kernel passkey module](https://docs.zerodev.app/sdk/plugins/passkeys) will automatically use EIP-7212 to save gas fees (as much as 400,000 gas) if it exists on the network. Both are supported natively by the userop.js library.


You can also find a basic implementation of a simple account in[this nice open source project with a passkey SDK](https://github.com/passkeys-4337/smart-wallet) . This repository includes a very basic implementation of a web application with passkeys, and is a great starting point.


## Toward Better Web3 UX


Passkeys offer a revolutionary approach to user authentication on the Ethereum blockchain, aligning with the goals of account abstraction by eliminating traditional private keys. For developers and organizations looking to enhance the security and user experience of their blockchain applications, integrating passkeys with ERC-4337 provides a forward-looking solution.
