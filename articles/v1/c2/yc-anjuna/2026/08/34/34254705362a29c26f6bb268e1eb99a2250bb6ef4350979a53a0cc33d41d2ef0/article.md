---
schema_version: "1.0.0"
document_id: "34254705362a29c26f6bb268e1eb99a2250bb6ef4350979a53a0cc33d41d2ef0"
company_key: "yc-anjuna"
company: "Anjuna"
source_id: "yc-anjuna-news-import-d3424cd26fcb"
canonical_url: "https://www.anjuna.io/blog/attested-tls-flaw-explained"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T09:31:43.768016+00:00"
fetched_at: "2026-08-05T09:31:44.540566+00:00"
content_hash: "sha256:0b58a6609886b72a9860b3753ad5b6eb8f41834b59d951183e05316a095a65b7"
---

# The Attested TLS Flaw Is Real — and It's Not What the Headline Says

A new peer-reviewed attack on attested TLS is real, carries a CVE, and deserves attention. It does not mean confidential computing is broken. It means one specific way of binding attestation to a network connection needs to be replaced with a better one — and Anjuna's architecture already reflects the fix.


Researchers at TU Dresden recently published[formal-verification work](https://dl.acm.org/doi/10.1145/3779208.3785387) , presented at AsiaCCS 2026 and ESORICS 2026 and assigned **CVE-2026-33697** , showing that several widely-discussed attested TLS protocols are vulnerable to diversion and relay attacks. The coverage that followed carried headlines suggesting confidential computing's "core trust mechanism is broken" and that "the fix may not exist."


The research is excellent and the vulnerability is real. But the headline conflates two very different things. What the researchers broke is a particular protocol construction — not the hardware, and not remote attestation itself. Understanding exactly where the flaw lives is the difference between abandoning a sound security architecture and simply building it correctly.


## What the researchers actually found


The team, led by Muhammad Usama Sardar, used the formal-verification tool ProVerif to examine *intra-handshake attestation* : schemes where a Trusted Execution Environment (TEE) generates its attestation evidence during the TLS handshake itself. They tested seven different ways of cryptographically binding that evidence to the connection. None of them prevented relay attacks.


The attack is elegant. A client asks for proof that it's talking to a genuine, unmodified enclave. An attacker relays that challenge to a real enclave, collects the legitimate evidence, and presents it as its own — all while the client ends up encrypting its actual traffic to a different machine. The evidence checks out. The software integrity is genuine. But the connection has been silently diverted to a server the client never intended to reach.


The root cause is a timing problem, not a hardware problem. Intra-handshake attestation generates its evidence *before* the session key that encrypts application data exists. By the time that key is available to bind against, the evidence has already been sent. The proof and the channel it's supposed to protect are never cryptographically joined.


## What the researchers did not find


Here is the part the headlines omit. The research explicitly assumes, and does not challenge, that the hardware root of trust is sound. As Sardar put it, in confidential computing you have to trust the hardware manufacturer anyway; there is no way around that, and it is not where the flaw lies.


So this is not a break in:


- **Confidential computing hardware.** Intel TDX, AMD SEV-SNP, and Arm CCA enclaves still do what they claim: isolate and protect code and data in use, with the manufacturer as the root of trust.
- **Remote attestation itself.** The ability of an enclave to produce signed, verifiable evidence of its identity and integrity is intact. The evidence is genuine and the flaw is that a specific protocol fails to bind that genuine evidence to the specific connection carrying the data.


The flaw is narrower and more precise than "confidential computing is broken." It is this: when you generate attestation evidence inside the TLS handshake, you cannot reliably tie it to the encrypted channel that follows. That is a real and important finding. It is not an indictment of the foundation.


## The more secure approach: attest first, then establish the channel


The most reassuring part of the research is that the mitigation direction it points to is the one Anjuna already considers best practice. Sardar's own conclusion is that post-handshake attestation — completing attestation after the session key exists, so evidence can be bound to the actual channel — is the path to the strongest binding guarantee. In other words: stop trying to prove trust *during* the handshake, and instead establish trust first, then let it govern the session.


The most robust way to build on that principle is to invert the order entirely. Rather than attesting inside a TLS handshake and hoping the binding holds, use remote attestation as the gate for provisioning a shared secret into the enclaves themselves:


1. **Attest the enclave first.** Verify the hardware evidence and the measured software identity of each TEE independently. And do this before any application session exists so there is no dependence on a TLS handshake to carry the proof.
2. **Release a secret only to enclaves that pass.** Use that successful attestation to load a shared secret into the verified enclave. A machine that cannot produce valid evidence never receives the secret.
3. **Establish the private session from the shared secret.** With a secret that provably exists only inside attested enclaves, the two endpoints can stand up a mutually authenticated private channel. Because possession of the key is itself proof of successful attestation, a relayed or diverted endpoint has nothing to authenticate with. The diversion attack has no purchase: an impostor machine may be running identical software, but it was never handed the secret, because it could not attest.


This is the crucial difference. Intra-handshake attested TLS tries to bind a proof to a connection and gets the timing wrong. The **attest-then-provision** pattern binds a secret to an attested enclave, and derives the connection from the secret. Location diversion stops being possible, because the thing that authenticates the channel is something only a genuinely attested enclave could ever hold.


## The takeaway for security leaders


If your architecture depends on intra-handshake attested TLS, the TU Dresden research is a genuine call to action. Four questions worth putting to your architecture team:


- **When does attestation happen** ?


- Is it inside the TLS handshake, or before any session exists?


- **What binds the evidence to the channel?**


- If the answer is "nothing cryptographic," you are exposed to relay and diversion.


- **Could a machine that never passed attestation obtain the key material that authenticates the session?**


- If yes, valid evidence isn't buying you what you think it is.


- **Is the evidence valid in general, or valid for *this* connection?**


- A signature proves who wrote the letter. It doesn't prove who's standing at your door holding it.


‍


The correct response is not to lose confidence in confidential computing or remote attestation. Both are doing exactly what they were designed to do.


The lesson is one confidential computing has taught before: the hardware and attestation primitives are strong, and the security of a system rests on composing them correctly. When trust is established before the channel is established, the class of attack these researchers found simply doesn't apply.


Want to see the attest-then-provision pattern in practice?[Start a free trial of Anjuna Seaglass](https://www.anjuna.io/anjuna-free-trial) and put your architecture to the test.
