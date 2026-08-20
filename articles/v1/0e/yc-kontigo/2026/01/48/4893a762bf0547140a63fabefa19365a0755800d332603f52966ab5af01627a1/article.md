---
schema_version: "1.0.0"
document_id: "4893a762bf0547140a63fabefa19365a0755800d332603f52966ab5af01627a1"
company_key: "yc-kontigo"
company: "Kontigo"
source_id: "yc-kontigo-news-import-fc52ff9f0792"
canonical_url: "https://www.kontigo.com/en/article/security-incident-postmortem-unauthorized-account-access"
published_at: "2026-01-11T18:19:42.351+00:00"
first_seen_at: "2026-07-26T07:01:22.539657+00:00"
fetched_at: "2026-07-28T22:23:45.657833+00:00"
content_hash: "sha256:e064467ed826ef10caeaac746a748bfb15bdeb72e3004e9aa5e02e70feec9078"
---

# Security Incident Report

## January 5th, first attack


On January 5th, we discovered a deliberate and highly sophisticated attack targeting Kontigo, which impacted 1,005 users and resulted in the loss of 340,905.28 USDT. Within 30 minutes of detecting the malicious activity, we launched a full incident response and escalated across all key stakeholders. From that point on, we operated 24/7, taking dozens of calls per day with industry experts, infrastructure providers, ethical hackers, local authorities, and clients, fully recognizing the seriousness of the situation.


Once the attack was contained, we began issuing refunds immediately and completed the process within 24 hours.


### **How the attack occurred**


The attack required valid, minted authentication tokens (JWTs) issued by our Auth provider.


The attacker identified a legacy gateway in our Auth provider’s Apple OIDC authentication flow where the system did not properly validate/enforce the expected issuer. As a result, the attacker was able to use an attacker-controlled OIDC issuer to generate tokens that the Auth provider accepted as valid Apple tokens. This allowed them to gain access to user accounts and receive valid authentication JWTs.


Once the attacker had valid authentication tokens, they were able to generate quotes for wallet withdrawals and connect to the affected users’ wallets to execute those quotes.


On our side, certain backend tables in our Database provider did not have Row-Level Security (RLS) configured to restrict access at a granular level. This created some visibility of user records than would normally be permitted when those controls are enabled. These did not include private keys, access to private keys, or specifics on the user besides emails and Auth user IDs. Kontigo does not store, hold, or have access to private keys.


Logs revealed that our user information table in our Database provider, containing Auth user IDs, was accessed on December 28 and again on January 3. This unauthorized access allowed the attacker to gather Auth user IDs as well as email addresses, but not wallet access.


The Kontigo team worked with our infrastructure providers and independent auditors around the clock to patch the vulnerability. Within 12 hours of the initial attack, both the OIDC token issue in the Auth system (enabling access to user accounts) and the RLS access control issue (enabling read access to certain tables) had been patched. All users were then immediately refunded their lost funds.


## January 8th follow-on attack


On January 8th, the attackers drained 56,913 USDC from 258 of the same user wallets. In this second phase, the attacker did not need to mint new authentication tokens. Instead, they reused wallet session JWTs associated with our embedded wallet provider (Thirdweb) that were captured during the initial compromise.


Upon examining logs, we noticed a clear pattern: users connected to their wallets on Monday, but transactions were executed on Thursday with no subsequent user connection through our APIs. Thirdweb confirmed that, under default settings, the wallet session JWT expiration is 30 days. Based on this, we determined the attacker stored wallet session JWTs during the initial attack and reused them during the second attack.


We worked with Thirdweb to invalidate all active JWTs, and the expiration for all new Thirdweb JWTs has been reduced to 15 minutes. We’ve additionally implemented PIN restrictions on both wallet connection and transaction execution to prevent malicious access to Thirdweb’s APIs.


We refunded all users 100% of lost funds approximately 24 hours after the second attack. We aim to issue refunds and re-enable services as quickly as possible while ensuring the attack vector has been properly closed to prevent future abuse.


We were able to halt both attacks and identify its root cause thanks to the outstanding support of Andrew MacPherson, Privy’s Security Lead & part of Security Alliance (SEAL) 911 team.


### **Funds movement and laundering path**


The stolen funds were moved by the attackers from Kontigo to ChangeNOW, a cryptocurrency exchange based in Saint Vincent and the Grenadines. The attackers utilized infrastructure originating from Bulletproof Hosting (BHP), a service linked to known APT actors.


In addition, the SEAL 911 team at ZeroShadow helped us confirm that, based on activity from the threat actor (TA) address on Base (0x7347C7468Cef51053d395a6D1e0c771198c5014A), a total of 56,913 USDC was swapped and bridged via ChangeNOW, resulting in 123.07 XMR on the Monero network.


The funding Base address (0xa779f675dF0Ad1aBbD7f241313662a12592c39f0) that funded the TA address received its funds via Bridgers, originating from the TRON address (TAD5jaRKvGgKbSzLsqBKPaRLSRBiPuq1cF).


Funds on TRON appear to have originated from ChangeNOW; source deposit details related to the swap and bridge into the TRON address are still pending.


We have identified a series of IP addresses from which the attack was perpetrated, and we are collaborating with local authorities to identify and prosecute the hackers.


## Why does Kontigo utilize an external Auth and Database provider?


Our Auth provider is one of the biggest, fastest-growing, and most prominent authentication providers in the industry. They support Fortune 500 companies and much of the Silicon Valley startup ecosystem. Multiple global, well-known companies have scaled on top of their infrastructure and they have invested heavily in security.


Kontigo transitioned to this Auth + Database stack during our YC S24 batch. As fellow YC companies, we believe Kontigo can continue scaling with these providers as valuable strategic partners throughout our rapid growth.


## Next steps


To further strengthen user protection, Kontigo has implemented additional security layers on top of our authentication stack to ensure there is not a single point of failure for any of our users.


At the same time, Kontigo is conducting active security research to proactively map all potential threats, including likely next steps from hackers and crypto criminals. We have engaged Trail of Bits to conduct a rapid incident response review focused on the January 2026 security incidents. We are evaluating additional security work as part of our longer-term response. We’re committed to investing heavily in cybersecurity as we scale faster and safer.


Accordingly, we are also announcing the hire of our new CISO and CISO advisory team, which will soon be made public in a separate post.


## About Kontigo’s non-custodial infrastructure


The attack described above was not related in any form or capacity to the non-custodial nature of Kontigo’s wallets.


We use Thirdweb embedded (in-app) wallets and keep the private key abstracted away from the front-end. Concretely, Thirdweb generates and uses the key inside AWS Nitro Enclaves (a confidential computing “trusted execution environment”), so the key isn’t reconstructed on the client and signing happens inside the enclave.


### **Why we abstract private keys in the UI (and why that can be safer)**


Most Kontigo users aren’t crypto-native. Making “download/export your private key” a front-and-center flow creates real risk (loss, malware clipboard capture, screenshots, social engineering, mis-storage). Thirdweb even warns that revealing private keys can compromise assets. So we default to a safer, mainstream UX while keeping a user-controlled export path available.


### **Why this is becoming a “safe industry standard”**


This pattern, confidential computing (TEE) for key custody and signing, is increasingly used because it reduces attack surface versus handling secrets on end-user devices or general-purpose servers. AWS explicitly positions Nitro Enclaves for protecting and processing highly sensitive data (including financial data), with attestation and KMS integration to ensure only authorized enclave code can access sensitive material. AWS also publishes reference architectures for secure blockchain key management and transaction signing using Nitro Enclaves.


## Last but certainly not least:


Kontigo already serves over 1,300,000 daily active users and 1,200 companies, rapidly becoming the new go-to DeFi banking standard.


Our obsession with closing financial gaps at scale is irrepressible.


Jesus Castillo


**CEO & Co-founder**
