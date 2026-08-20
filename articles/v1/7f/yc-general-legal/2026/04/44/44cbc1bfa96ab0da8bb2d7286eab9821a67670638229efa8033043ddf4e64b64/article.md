---
schema_version: "1.0.0"
document_id: "44cbc1bfa96ab0da8bb2d7286eab9821a67670638229efa8033043ddf4e64b64"
company_key: "yc-general-legal"
company: "General Legal"
source_id: "yc-general-legal-news-import-cc42147fa719"
canonical_url: "https://general.legal/blog/computing-on-data-without-seeing-it"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-29T04:44:54.246082+00:00"
fetched_at: "2026-07-29T04:44:55.619052+00:00"
content_hash: "sha256:e51fccbceecfd71ef4654bd051e869c38f5180758a48329cd06b5f5a2a6f9cce"
---

# The Frontier: Computing on Data Without Ever Seeing It

**Part 5 of our series on Privacy Enhancing Technologies**


The last three tools in this series sound like science fiction: computers that process data they’re never allowed to look at. They’re real, they’re in production today at companies you use daily, and they’re genuinely hard to build. This post explains each one in plain terms: what it does, where it’s actually used, where it falls short, and what it costs.


One thing all three share: they protect how data is handled during a computation. None of them can tell you whether the underlying decision being computed is fair, accurate, or correct. Keep that in mind as you read. It matters more than it sounds like it should.


# Secure Multiparty Computation (MPC)


### What is secure multiparty computation?


The simplest way to think about it: two or more organizations each have a private list. They want to know one specific thing about how their lists relate to each other, without showing each other the full list.


### Secure multiparty computation example


A concrete example: Bank A has a list of people it has flagged as suspicious. Bank B is about to open new customer accounts and wants to know: is anyone on my new-customer list also on Bank A’s flagged list? Without MPC, Bank A would have to hand Bank B its entire flagged list to check, meaning Bank B now sees thousands of people who have nothing to do with it. With MPC, the two banks run a computation where the only thing that comes out is which specific names appear on both lists. Nothing else about either list is revealed.


### What MPC does and doesn’t protect


What this does and doesn’t protect. This is the important part. MPC does not hide the answer to the question being asked. If you’re one of the matching names, both banks now know that. What it hides is everything else: all the other people on both lists who aren’t relevant to this particular check. So MPC reduces how much gets exposed, but for the person who actually matches, it changes nothing about the outcome. And if the original flag was wrong, biased, or unfair, MPC will faithfully and privately confirm that wrong flag. It has no opinion on whether the flag should have existed in the first place. MPC protects data confidentiality. It says nothing about fairness.


### Where MPC fits well


Where it fits well: a narrow, well-defined question between organizations that don’t trust each other with their full data. Fraud detection across banks, joint statistics across hospitals, government agencies comparing lists.


### Where MPC doesn’t fit


Where it doesn’t fit: open-ended “let’s explore each other’s data together” collaboration. MPC needs the exact question defined in advance.


### How expensive is MPC?


How expensive: high. Every participating organization needs to build and run the same protocol correctly. This is not something one company can switch on alone; it requires every party to invest.


## Fully Homomorphic Encryption (FHE)


### What is fully homomorphic encryption?


The simplest way to think about it: your data stays locked in an encrypted box the entire time, even while someone else is computing on it. The party doing the computing never has the key.


### Fully homomorphic encryption examples


Concrete examples already running today: when your phone checks whether a photo matches a landmark using a server-based index, the comparison can happen while your photo data stays encrypted the whole time. When a browser checks your password against a list of known breached passwords, the check can happen without the company ever seeing your actual password.


### Where FHE fits well


Where it fits well: narrow, repeated lookups: checking one thing against a big list, over and over, at scale. Password checks, biometric matching, this kind of one-question-at-a-time task.


### Where FHE doesn’t fit


Where it doesn’t fit: broad, open-ended data analysis. The math involved is enormously slower than normal computing, often somewhere around a thousand times slower, even after years of real speed improvements. Complex analysis that would take a computer one second normally could take much, much longer under FHE.


### How expensive is FHE?


How expensive: very high, and it’s a specialist skill. Very few engineers know how to build this correctly today.


## Trusted Execution Environments (TEE)


### What is a trusted execution environment?


The simplest way to think about it: a locked room inside the computer chip itself. Normal, unencrypted computation happens inside that room, but nobody outside it, not even the company running the computer, can see in.


### Trusted execution environment examples


Concrete examples already running today: every major cloud provider (Microsoft, Amazon, Google) now sells this as a standard product. Companies use it to run sensitive computations on a cloud server while keeping that data invisible even to the cloud company’s own staff and software. Apple uses the same underlying idea for some of its AI features: the computation happens on Apple’s servers, but is walled off from Apple’s own operators.


### Where TEEs fit well


Where it fits well: anywhere you need close-to-normal computer speed with strong isolation, and you’re willing to trust the chip manufacturer’s hardware. This is the fastest and most mature of the three, already commercially available and reasonably practical to use today.


### Where TEEs don’t fit


Where it doesn’t fit: situations where you cannot trust the hardware itself. This is the catch. TEEs are secure by design, but the “ *locked room* ” has been broken into repeatedly, using clever side-channel attacks that don’t break the lock directly but instead read secrets by watching how the room’s electricity or memory behaves. Even companies that use this technology heavily, like Apple, publicly say they don’t rely on it alone. They add extra protections on top, because the hardware guarantee by itself isn’t considered enough.


### How expensive are TEEs?


How expensive: lowest of the three. Available today from major cloud providers, without needing to invent new cryptography.


## MPC vs. FHE vs. TEE


**Technology** **Simplest Explanation** **Where It Fits Well** **Where It Doesn’t Fit** **Relative Cost**


Secure Multiparty Computation Two or more organizations answer a defined question using private datasets without sharing the full datasets. Narrow computations between organizations that do not want to reveal their complete data. Open-ended exploration where the required computation has not been defined in advance. High


Fully Homomorphic Encryption A third party computes on data while it remains encrypted. Narrow, repeated lookups and one-question-at-a-time computations. Broad or computationally complex analysis. Very high


Trusted Execution Environments Data is processed normally inside an isolated hardware environment. Workloads needing close-to-normal speed and strong hardware-backed isolation. Situations where the hardware manufacturer or hardware security cannot be trusted. Lowest of the three


## Coming **Up**


Last in this series:[A plain answer to the question that actually matters most.](https://general.legal/blog/which-privacy-enhancing-technology-to-use)


If you’re not a tech giant with unlimited engineering budget, which of these six tools should you actually be looking at?


*This post is for general informational purposes and does not constitute legal advice. Whether any of these technologies satisfies a specific regulatory standard depends on the implementation. General Legal’s attorneys can help you think that through before you rely on one as a compliance answer.*
