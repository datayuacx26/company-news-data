---
schema_version: "1.0.0"
document_id: "54cc2f0a593a9d8882ee82c603a87ea90c6d517bb3fb6a0a87c842a9a025582c"
company_key: "yc-multifactor"
company: "Multifactor"
source_id: "yc-multifactor-news-import-864f66d5ce56"
canonical_url: "https://multifactor.com/blog/keypo-multifactor-founders-reflection"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-22T05:04:58.540173+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:be22ecb00851b73f2275b13774c5dbf7f7c9d66cdc7abb1cf82d271455e3ab37"
---

# Keypo + Multifactor: A founder’s reflection on the acquisition

# Keypo + Multifactor: A founder’s reflection on the acquisition


2026-06-15


[Company](https://multifactor.com/blog/tags/company)[Keypo](https://multifactor.com/blog/tags/keypo)[Checkpoint](https://multifactor.com/blog/tags/checkpoint)


[DB Dave Blumenfeld Founding Engineer](https://multifactor.com/blog/authors/dave)


**TL/DR:** Earlier this month we announced that Keypo has joined Multifactor. The two companies are *uniquely* aligned: among the very few teams taking secret management for AI agents seriously, we’d arrived from two directions that fit together perfectly.


This is the story of that problem, the two halves we each solved, and what we're building now that we're one company.


## No one has figured out how to share an account with an agent


At their core, AI agents are LLMs wired up to tools that let them act on the outside world. Most of those tools touch sensitive information. A financial-advisor agent, for instance, needs to see your investment balances to do its job. But there's no clean way to grant it without also handing it the ability to move money out of your account. The credential that views your balance is the same credential that drains it.


And therein lies the tension: an agent is only as useful as the tools you give it, but the more access you grant, the more damage it can do. How do you hand an agent real capabilities without handing it the keys to start a fire?


When I started Keypo in 2024, no one had a good answer. Most developers were dropping credentials into a .env file and trusted software guardrails. The more sophisticated products borrowed from enterprise secret management: an encrypted vault with policies governing how and when an agent could pull a credential out of it. But both still hand the raw credential to the agent in the end, and the instant it holds the credential, there’s nothing constraining what it does with it.


These tools control who gets the secret, assuming that controlling access is the same as controlling use. For a human or deterministic code, that mostly holds. Agents are neither. They're part software, part improvised reasoner, capable of lasting damage in an instant. The dangerous part isn't who holds the credential; it's everything that happens after.


Account sharing for agents needs different primitives entirely: ones that let an agent use a credential without ever seeing it.


## How we approached it at Keypo


Trusted execution environments (TEEs), also called secure enclaves, are special-purpose hardware that does two things at once. First, it keeps the data processed inside it from leaking out. Second, it lets anyone outside verify exactly what code is running inside and that nothing has tampered with it. That verification happens cryptographically through a mechanism called attestation.


At Keypo, we used TEEs to bind a credential to a specific, attested program; change one line and the credential becomes unusable. Imagine a program running in a TEE that authenticates an agent, logs into an investment account and returns the balance. The investment account credentials can only be decrypted inside the TEE running this specific program, and the agent never sees them in plaintext.


## Why Keypo and Multifactor Need Each Other


While Keypo was making credentials usable without being seen, the other side of the coin is storing them when they're *not* in use. TEEs have no persistent disk, so credentials have to live somewhere between uses, encrypted, where a freshly launched TEE can fetch them. Before Multifactor, the only way to protect them at rest was password-based key derivation: encrypting your credentials under a master password, set by you, your password manager, or some mix of the two.


The hard truth is that a password-derived key is only as strong as the password. Most passwords are short, reused, and guessable, so an attacker who steals the encrypted vault can brute-force the master password offline until the whole thing cracks opens. A stronger password just moves the problem: one tough enough to resist that attack is too long to remember, so it ends up in a password manager, which is itself a custodian holding the one secret that unlocks everything.


The way out is genuinely new key derivation, which is what Vivek has spent 10+ years and 5 patents building. Multifactor's **Multi-Factor Key Derivation Function (MFKDF)** removes the single point of failure entirely. Rather than deriving the key from one master password, it derives it from several of the user's authentication factors at once: a password, an authenticator code, even biometrics, none of which can be guessed or stolen on its own. The key exists only in the instant the real user assembles it, and is never written down. No master password holds everything up, because there's no stored secret at all.


Put the two halves together and there's no single point of failure. A user's credentials sit in ordinary, untrusted storage as a blob only their live factors can unlock; when they hand a task to an agent, the credentials are reconstructed and passed straight into a scoped, attested enclave that uses them without ever exposing them. MFKDF guards the credential when it's idle, the enclave guards it the instant it's used, and at no point does a usable secret sit anywhere a thief or a rogue agent could reach it.


## What we're building together: Checkpoint


Since the acquisition closed in May, we've been heads-down building Checkpoint, the product where these two halves become one.


Checkpoint mediates every access to a shared account through a proxy that runs inside a TEE. It sits between the agent and the service the account lives on, intercepting requests and responses to enforce the access rules the account owner set. Because it works at the network layer, it needs no cooperation from the service on the other end. Any account that uses web authentication works, including ones protected by 2FA.


Checkpoint can do something password managers never could: give you control over how your accounts are used. Today, sharing an account is all or nothing. You hand over the password and you've handed over everything the account can do, forever, with no way to see how it gets used and no way to take it back short of changing the password. Checkpoint points at a different future. Access can be scoped down to exactly what's needed and nothing more, withdrawn the instant you want it gone, and seen in full while it's in use. Sharing stops being a thing you do once and lose control of, and becomes something you can actually govern.


Today, Checkpoint is a secure, programmable proxy. It terminates the encrypted connection from the user, inspects and rewrites the traffic based on rules set in advance, then re-encrypts it on its way to the destination. Below is a short video showing a connection to Checkpoint with a rule to replace every text instance of “Apple” with “Banana.” The same pattern will let us inject a user's credentials into an agent's session: the agent drives the account, Checkpoint supplies the secret in transit, and the credential is never exposed to the agent at all.


Video: A connection to Checkpoint with a rule that replaces every text instance of “Apple” with “Banana.”


The smarter agents become, the more we'll ask of them, and the more dangerous it becomes to hand them unfettered access to everything we own online. This new paradigm requires rethinking account management from the ground up with new cryptographic primitives. Multifactor and Keypo, together, will build this future.


— Dave


# We're redefining zero-trust — so you can protect your accounts with confidence.


Identity is your first and last line of defense, and the root cause of most application security breaches. Multifactor's provably secure zero-trust solutions cryptographically guarantee that only authorized users can access sensitive data, turning identity into your greatest asset in the fight against cyber threats. Learn more about our research, or reach out to explore working together.


[Get Started](https://multifactor.com/get-started)[Learn More](https://multifactor.com/company/sales)


## Related Posts


[Multifactor acquires Keypo to accelerate zero-trust security for AI agents](https://multifactor.com/blog/keypo-acquisition)


2026-06-01


[Company](https://multifactor.com/blog/tags/company)[Keypo](https://multifactor.com/blog/tags/keypo)


We're proud to share that Multifactor has acquired Keypo, Inc., a programmable encryption company. Keypo's founder, Dave Blumenfeld, is staying on as Multifactor's Founding Engineer.


[MT Multifactor Team](https://multifactor.com/blog/authors/team)


[We're Hiring](https://multifactor.com/blog/we-are-hiring)


2026-03-02


[Company](https://multifactor.com/blog/tags/company)[Research](https://multifactor.com/blog/tags/research)[Agentic](https://multifactor.com/blog/tags/agentic)


Multifactor is hiring. If you are a talented engineering leader or individual contributor who cares deeply about forging the future of authentication, authorization, and auditing for the agentic era, you should probably check out our open positions.


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)


[Multifactor Versus Password Managers: Securing Capabilities, Not Credentials](https://multifactor.com/blog/multifactor-versus-password-manager)


2026-01-31


Say goodbye to password managers and hello to Multifactor, the world’s first true Account Manager!


[VN Vivek Nair Founder & CEO](https://multifactor.com/blog/authors/vivek)
