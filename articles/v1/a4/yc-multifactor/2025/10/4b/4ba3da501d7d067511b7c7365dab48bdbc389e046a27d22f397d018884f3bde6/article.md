---
schema_version: "1.0.0"
document_id: "4ba3da501d7d067511b7c7365dab48bdbc389e046a27d22f397d018884f3bde6"
company_key: "yc-multifactor"
company: "Multifactor"
source_id: "yc-multifactor-news-import-864f66d5ce56"
canonical_url: "https://multifactor.com/blog/multifactor-mfa"
published_at: "2025-10-14T00:00:00+00:00"
first_seen_at: "2026-07-22T05:04:58.540173+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:7b561da3322e291b41f15593cd8602333444287790b84caa2183ac16da6423ac"
---

# What is Multifactor? From Multi-Factor Authentication (MFA) to Secure Account Management

# What is Multifactor? From Multi-Factor Authentication (MFA) to Secure Account Management


2025-10-14


[MFA](https://multifactor.com/blog/tags/mfa)[MFKDF](https://multifactor.com/blog/tags/mfkdf)[Checkpoint](https://multifactor.com/blog/tags/checkpoint)[App](https://multifactor.com/blog/tags/app)


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


If you're searching for "Multifactor," you’re in the right spot. This article provides a comprehensive overview of[Multi-Factor Authentication (MFA)](https://en.wikipedia.org/wiki/Multi-factor_authentication) and its relationship to Multifactor, the company. We'll explore the core principles of modern web authentication, the threats it protects against, and how we at Multifactor are building off this foundation to pioneer the next evolution in digital identity security.


MFA, also known by its most common form, Two-Factor Authentication (2FA), is one of the most effective actions you can take to enhance your online security. It stands as a powerful defense against pervasive cyberattacks like phishing, credential stuffing, and identity theft. In fact, implementing MFA makes your accounts[99% less likely to be compromised](https://www.cisa.gov/topics/cybersecurity-best-practices/multifactor-authentication) .


But authentication is just the beginning. The real challenge is managing a complex digital life filled with countless credentials. Here, we'll introduce how Multifactor is leveraging the foundational concepts of MFA to provide security for a new era of managing and sharing online accounts and other secrets, powered by the[Multi-Factor Key Derivation Function 2 (MFKDF2)](https://mfkdf.com/) and[Checkpoint](https://multifactor.com/blog/introducing-checkpoint) .


---


**TL;DR:** Multi-Factor Authentication (MFA) is a security technique combining multiple forms of identification to verify a user's identity. Our company, Multifactor, and the Multifactor App leverage these same multi-factor principles to enable a new platform for account management that allows for truly secure account sharing without ever sharing underlying credentials.


---


## Why Passwords Are Not Enough


When you sign into a website, you're performing an act of authentication—proving you are who you say you are. For decades, this process was handled solely by a username and password, a method known as Single-Factor Authentication (SFA). But as cyber threats have grown in sophistication, it's become dangerously clear that this single layer of defense is broken; using SFA is generally considered[bad practice by the Cybersecurity Infrastructure Security Agency (CISA)](https://www.cisa.gov/stopransomware/bad-practices) .


Passwords have inherent weaknesses: they are hard for humans to remember, leading to widespread password reuse and the use of weak, easily guessable phrases. This has made them a prime target for attackers. Today, an[estimated 88% of all data breaches](https://www.verizon.com/business/resources/reports/dbir/) are the result of compromised, weak, or stolen credentials. This vulnerability has given rise to a host of common attack vectors:


- [Phishing](https://en.wikipedia.org/wiki/Phishing) **:** Deceptive emails or websites trick users into voluntarily revealing their credentials. Phishing remains the most common initial attack vector,[responsible for 17% of breaches](https://www.phishingbox.com/resources/phishing-facts) .
- [Brute-Force Attacks](https://en.wikipedia.org/wiki/Brute-force_attack) **:** Automated tools systematically guess password combinations. Modern GPUs can test billions of combinations per second, making short or simple passwords trivial to crack.
- [Credential Stuffing](https://en.wikipedia.org/wiki/Credential_stuffing) **:** Attackers use massive lists of usernames and passwords stolen from one data breach to attempt to log in to other services, exploiting the fact that roughly[65% of users reuse passwords across multiple sites](https://www.enzoic.com/blog/8-stats-on-password-reuse/) even though 91% claim they know better.
- [Man-in-the-Middle (MitM) Attacks](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) **:** Attackers intercept communication between a user and a website to steal credentials or session cookies.


To combat this, the industry has shifted to a layered defense strategy, making it more difficult for an unauthorized person to access a target like a computing device, network, or database.


## MFA vs. 2FA vs. 2SV


As security measures have evolved, the terminology can be confusing. Understanding the precise definitions is key to implementing effective protection.


- **Multi-Factor Authentication (MFA):** This is the broad, overarching term for any authentication process that requires a user to provide two or more distinct pieces of evidence, or "factors," to verify their identity. According to the U.S. National Institute of Standards and Technology (NIST), the core requirement is the use of "two or more different factors" from separate categories.
- **Two-Factor Authentication (2FA):** This is the most common implementation of MFA. As the name implies, 2FA is a specific subset that requires exactly two authentication factors. A classic real-world example is withdrawing cash from an ATM, which requires both a physical bank card (a possession factor) and a PIN (a knowledge factor). This leads to a simple but critical relationship: all 2FA is a form of MFA, but not all MFA is 2FA, as MFA can involve three or more factors.
- **Two-Step Verification (2SV):** This term introduces a subtle but vital distinction. While often used interchangeably with 2FA, 2SV refers to any process that requires two sequential verification steps, but these steps are not necessarily from *different factor categories* . For example, a system that asks for a password and then a secret answer to a security question is a two-step process. However, since both are "knowledge" factors, this setup does not meet the strict definition of true 2FA, which demands factors from different categories.


### The Core Factors of Authentication


For an authentication process to be considered true MFA, it must combine factors from at least two of the following independent categories :


1. **Knowledge Factor ("Something You Know"):** This is information that should be exclusive to the user.


- *Examples:* Passwords, Personal Identification Numbers (PINs), or answers to security questions.


2. **Possession Factor ("Something You Have"):** This requires a user to have a specific item in their possession.


- *Examples:* A smartphone receiving an SMS code or Push Notification, an authenticator app generating a One-Time Password (OTP), a physical hardware security key (like a[YubiKey](https://en.wikipedia.org/wiki/YubiKey) ), or a[smart card](https://en.wikipedia.org/wiki/Smart_card) .


3. **Inherence Factor ("Something You Are"):** This factor uses unique biological traits of the user, commonly known as[biometrics](https://en.wikipedia.org/wiki/Biometrics) .


- *Examples:* Fingerprint scanning, face recognition, voice patterns, or iris scans.


4. **Context Factors ("Somewhere You Are" or "Something You Do"):** Modern systems are increasingly adding contextual factors for more dynamic security.


- *Examples:* A user's geographic location, IP address, network connection, or even behavioral biometrics like keystroke dynamics can be used to assess risk.


## Common MFA Methods Explained


Implementing MFA isn't a single decision; it's a choice along a spectrum of security and convenience. Some methods are more secure than others.


- **SMS and Email Codes:** This is the most common method, where a one-time password (OTP) is sent via text or email. While better than nothing, it is the least secure form of MFA. It is highly vulnerable to[SIM swapping](https://en.wikipedia.org/wiki/SIM_swap_scam) , an attack where a criminal tricks a mobile carrier into transferring your phone number to their device, allowing them to intercept your codes. The[FBI reported that SIM swapping losses](https://www.ic3.gov/PSA/2022/PSA220208) surged from $12 million to $68 million in just three years.
- **Authenticator Apps:** Apps like Google Authenticator or Microsoft Authenticator generate a[Time-based One-Time Password (TOTP)](https://en.wikipedia.org/wiki/Time-based_one-time_password) that refreshes every 30-60 seconds. This is much more secure than SMS because the code is generated on your device and never transmitted over the insecure phone network, making it immune to SIM swapping.
- **Push Notifications:** Instead of a code, the service sends an "Approve" or "Deny" notification to your device. This is highly convenient but has a key vulnerability: MFA Fatigue. An attacker who has your password can bombard you with push notifications, hoping you'll accidentally approve one out of annoyance. Microsoft has observed approximately[6,000 MFA fatigue attempts per day](https://news.microsoft.com/en-cee/2023/10/12/microsoft-issued-annual-digital-defense-report-espionage-fuels-global-cyberattacks/) in 2023.
- **Biometrics:** Using a fingerprint or face scan is fast, convenient, and highly secure, as the "factor" is physically tied to you. However, it carries a unique risk: if a database of biometric data is breached, that data is compromised for life. You can't reset your fingerprint. Likewise, this is a factor that can also be lost and unable to be recovered.
- **Hardware Security Keys:** Physical keys that plug into your device (e.g., via USB) are considered the gold standard of MFA. They are a core part of the passwordless future.


## Passwordless, Zero Trust, and Compliance


The industry is rapidly evolving beyond simply adding a second factor to a password. The goal is to eliminate the password entirely and adopt a more holistic security philosophy.


- **The FIDO Alliance & Passwordless Future:** The[FIDO Alliance](https://fidoalliance.org/) , in partnership with the[W3C](https://www.w3.org/) , is creating open standards like[FIDO2](https://en.wikipedia.org/wiki/FIDO_Alliance#FIDO2) and[WebAuthn](https://en.wikipedia.org/wiki/WebAuthn) to enable truly passwordless authentication. This technology, often implemented with hardware security keys or device biometrics, uses public-key cryptography to create a login experience that is fundamentally resistant to phishing.
- **Zero-Trust Architecture (ZTA):** This is a modern security model built on the principle of["never trust, always verify"](https://www.nist.gov/blogs/taking-measure/zero-trust-cybersecurity-never-trust-always-verify) . Instead of assuming everything inside a corporate network is safe, ZTA assumes breach and requires continuous verification for every user, device, and access request. Strong MFA is the foundational pillar of any Zero-Trust strategy.
- **Compliance Mandates:** Strong authentication is now a legal and regulatory requirement in many industries.


- [NIST Special Publication 800-63-3](https://pages.nist.gov/800-63-3/) offers federal guidance on digital identity, defining what qualifies as high-assurance MFA.
- Regulations like[CCPA (California Consumer Privacy Act)](https://oag.ca.gov/privacy/ccpa) and[HIPAA (Health Insurance Portability and Accountability Act)](https://www.hhs.gov/hipaa/index.html) require robust[Identity and Access Management (IAM)](https://en.wikipedia.org/wiki/Identity_and_access_management) systems, with MFA as a central component.


## Applying Multi-Factor Principles to Secure Sharing


At Multifactor, we asked a different question: If we use multiple factors or passwordless techniques to secure a login, why can't we use them to create the most secure possible way to manage and share an account (or other secrets)?


Another glaring security hole in both personal and enterprise use cases is credential sharing. You write it on a sticky note, send it in a text, or use a password manager that wasn't designed for seamless, multi-user workflows. This is insecure, non-revocable, and breaks the entire "something you know" model. While password managers help users create strong, unique passwords, they often become a centralized honey pot for attackers and are typically only secured by a single master password themselves due to the encryption being keyed by a single master password.


Using the same principles as MFA, we built the Multi-Factor Key Derivation Function 2 (MFKDF2) to ensure that that encryption is as strong as the MFA itself.


### MFKDF2: Forging a Key, Not Just Opening a Lock


In simple terms, MFKDF2 is a process that takes multiple, distinct pieces of information (factors) and combines them to generate a single, powerful cryptographic key just like MFA takes these same factors and combines them to authenticate a login.


Think of it this way:


- Traditional **MFA** uses your factors to *unlock a door* (authenticate you).
- **MFKDF2** uses your factors to *forge a unique key* (derive an encryption key).


The Multifactor App uses MFKDF2 to allow you to grant access to an account *without ever revealing the password* . A user's device, their biometrics, and other factors are combined to create a temporary, single-use key that grants access. The master password remains safely encrypted and is never transmitted. This allows for passwordless access for the end-user and completely secure credential sharing for the account owner. In fact, this can combine the passwordless access alongside other factors to provide strictly greater security than a FIDO2/WebAuthn itself.


Unlike other mechanisms, MFKDF2 can also be made tougher to brute force over time. We’ve documented this in a[previous article](https://multifactor.com/blog/kdf-parameter-selection-part-1) and provided a[tool to determine KDF parameters](https://multifactor.com/blog/kdf-parameter-selection-part-2) as well.


#### MFCHF2: Making Server-Side Hashes Stronger


On the server-side, we use[MFCHF2](https://mfchf.com/) to protect your authentication credentials. Traditional password hashing functions (like[bcrypt](https://en.wikipedia.org/wiki/Bcrypt) or[Argon2](https://en.wikipedia.org/wiki/Argon2) ) are only as strong as the password they are protecting. If a user has a weak password, the hash is easier to crack after a breach.


MFCHF2 solves this by incorporating the entropy from your *multiple factors* directly into the credential hash stored on our servers. This provides asymmetric resistance to brute-force attacks. For an attacker, this makes cracking a stolen hash 10610^6


1


0


6


to 104810^{48}


1


0


48


times more difficult than with traditional methods, all with negligible performance impact for the legitimate user. This means that even if an attacker compromises our servers, they will still need to brute force the hash, which is significantly more difficult than simply using the password itself.


#### MFDPG2: Deterministic Password Generation


Another mode of operation is MFDPG2 which allows for the deterministic generation of passwords from multiple authentication factors. This means that passwords do not even need to be stored, they can be generated as needed. This is particularly useful for password managers, where the passwords can be generated as needed and never stored. If passwords are never stored, the risk of a breach is greatly reduced.


### Uniting Unbreakable Security with Superior User Experience


This combination of strong client-side and server-side cryptographic technology is the real reason why it's safe to have all your credentials in our app.


- **Client-Side Encryption (MFKDF2):** Your vault is encrypted on your device with a key that only you can generate using your unique factors.
- **Server-Side Protection (MFCHF2):** The authentication hash we store is exponentially more resistant to attack than any industry standard.
- **Deterministic Password Generation (MFDPG2):** Passwords can be generated as needed and never stored.


This architecture ensures true zero-trust security—not even we at Multifactor can access the contents of your vault.


Crucially, this approach also allows for the recoverability users expect. Unlike many password managers where losing your single master password means your data is lost forever, our multi-factor system provides secure recovery paths. By leveraging your other registered authentication methods, you can regain access to your account without compromising its security, delivering both best-in-class protection and a frictionless user experience. No master secrets are ever used by Multifactor and recovery can even be assisted by friends, family, or any other person you wish.


MFKDF2, MFCHF2, and MFDPG2 have all been discussed in a[previous article](https://multifactor.com/blog/introducing-mfkdf2-mfchf2-mfdpg2) in more detail


### Providing Shared Authentication


Our cryptographic foundation powers our[Checkpoint](https://multifactor.com/blog/introducing-checkpoint) technology, which addresses the inherent risks of account sharing. Instead of the insecure, "all-or-nothing" approach of handing over a password, Checkpoint allows you to grant temporary, revocable, and highly specific permissions to others without ever exposing your underlying credentials.


This technology layers a new level of security on the MFA foundation by eliminating the irrevocable nature of traditional sharing. With a single click, you can instantly terminate a shared session. Furthermore, you can define specific rules for that session, providing fine-grained control over what the other user can do. This capability is a natural extension of past authentication schemes; it transforms every shared session from a blanket grant of trust into a distinct, policy-enforced event.


At the same time, Checkpoint also allows for the underlying account to continue to use MFA itself and even update its MFA factors if needed all without affecting the shared session.


## End-to-End Security through Multifactor


The term "multifactor" represents a fundamental principle: security through layers. By combining different, independent factors, we create a whole that is far more secure than the sum of its parts. This is the core idea behind the Multi-Factor Authentication that protects your Google sign-in, and it's the same principle that drives our technology.


But we take it a step further. We believe the future isn't just about authenticating with multiple factors—it's about managing them. Our platform is built to be a single, unified vault for your entire digital identity: your passwords, your 2FA codes, your passkeys, and more. We've moved from Multi-Factor Authentication to comprehensive Multi-Factor Management.


So, by now, you probably get why we're called Multifactor. Our name is our mission: to unify all the factors of your digital life into a single, secure, and seamless system.


Ready to see how true multifactor technology can solve your account management and sharing problems?


[Learn more about the Multifactor App and our approach to security.](https://multifactor.com/)


---


## Frequently Asked Questions (FAQ) about Multi-Factor Authentication


**Q1: What is the main difference between MFA and 2FA?**


A1: 2FA (Two-Factor Authentication) is a specific type of MFA that always uses exactly two factors. MFA is the broader term and can include two, three, or more factors. All 2FA is MFA, but not all MFA is 2FA.


**Q2: Is SMS-based 2FA secure?**


A2: While better than no second factor, SMS is the least secure method of 2FA due to vulnerabilities like SIM swapping. Authenticator apps, push notifications, and hardware keys are significantly more secure options. You can use Multifactor as an all-in-one solution for MFA and password management so that you don't have to use a separate authenticator app.


**Q3: What is a hardware security key?**


A3: A hardware security key is a small physical device (often USB or NFC) that provides a possession factor. It uses[public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) to authenticate you and is one of the most secure forms of authentication, as it is resistant to phishing attacks.


**Q4: Is MFA 100% secure?**


A4: No security system is 100% foolproof. While MFA dramatically reduces risk, determined attackers can still find ways to bypass it through sophisticated methods like MFA fatigue attacks or man-in-the-middle phishing. However, it remains one of the most effective security measures available.


**Q5: How does Multifactor's MFKDF2 relate to MFA?**


A5: MFKDF2 uses the same core principle as MFA—combining multiple, independent factors. But instead of just authenticating a login, it uses those factors to generate a cryptographic key. This enables encryption on the server-side of your account, which is much more secure than traditional encryption which is keyed by a single master password.


[Explore the Multifactor App and see the next evolution of multifactor technology in action.](https://multifactor.com/)


# We're redefining zero-trust — so you can protect your accounts with confidence.


Identity is your first and last line of defense, and the root cause of most application security breaches. Multifactor's provably secure zero-trust solutions cryptographically guarantee that only authorized users can access sensitive data, turning identity into your greatest asset in the fight against cyber threats. Learn more about our research, or reach out to explore working together.


[Get Started](https://multifactor.com/get-started)[Learn More](https://multifactor.com/company/sales)


## Related Posts


[Keypo + Multifactor: A founder’s reflection on the acquisition](https://multifactor.com/blog/keypo-multifactor-founders-reflection)


2026-06-15


[Company](https://multifactor.com/blog/tags/company)[Keypo](https://multifactor.com/blog/tags/keypo)[Checkpoint](https://multifactor.com/blog/tags/checkpoint)


Earlier this month we announced that Keypo has joined Multifactor. This is the story of the problem, the two halves we each solved, and what we're building now that we're one company.


[DB Dave Blumenfeld Founding Engineer](https://multifactor.com/blog/authors/dave)


[Introducing Checkpoint: A Better Way to Share Online Accounts](https://multifactor.com/blog/introducing-checkpoint)


2025-07-07


[Research](https://multifactor.com/blog/tags/research)[Authorization](https://multifactor.com/blog/tags/authorization)[Auditing](https://multifactor.com/blog/tags/auditing)[Checkpoint](https://multifactor.com/blog/tags/checkpoint)


Checkpoint uses novel cryptographic techniques to enable easy revocable, non-repudiable, and fine-grained sharing of any online account resource.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


[How to Choose Parameters for KDFs like Argon2, Bcrypt, Scrypt, PBKDF2, Balloon Hashing, & More (Part 2 of 2)](https://multifactor.com/blog/kdf-parameter-selection-part-2)


2025-08-01


[Research](https://multifactor.com/blog/tags/research)[KDF](https://multifactor.com/blog/tags/kdf)[MFKDF](https://multifactor.com/blog/tags/mfkdf)


Explanation of our interactive tool and free API for selecting optimal parameters for hard KDFs like Argon2, Bcrypt, Scrypt, PBKDF2, and Balloon Hashing.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)
