---
schema_version: "1.0.0"
document_id: "39b2d2eaf55a71526b71eb99ae27fd57d1ff08bfaf13331f6ae4a49ec3fc7cb2"
company_key: "international-business-machines-corporation-common-stock"
company: "International Business Machines Corporation"
source_id: "international-business-machines-corporation-common-stock-news-import-da8ebfcd6a45"
canonical_url: "https://research.ibm.com/blog/cyrography-abstraction-layer"
published_at: "2026-07-17T10:15:00+00:00"
first_seen_at: "2026-07-24T00:23:00.528191+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:2c3c324cd6abbb472d01e5554f67da79c15befe87bce186d7b9a34b96a414662"
---

# It’s time for cryptography to get its own abstraction layer

There was a time when developers had to understand how data was physically stored just to write or read a file. They worked with tracks and sectors and wrote large amounts of code to manage storage details. Filesystems changed that: They introduced a stable abstraction and removed that burden from developers.


Networking followed the same path. Developers once had to implement retransmissions and reliability themselves. TCP/IP, accessed through the sockets API, moved that complexity into a shared layer and made network programming significantly simpler.


The field of cryptography has not yet made that jump. Even today, developers still write and maintain hundreds of lines of security-sensitive code to perform what should be straightforward operations: sign, verify, encrypt, and decrypt. We’ve had crypto libraries and APIs for decades — but they[bake the algorithm choice](https://arxiv.org/abs/2606.13425) into the calling code. Each use embeds detailed choices that are hard to change later. Over time, this leads to fragile systems and expensive technical debt.


The answer is *cryptographic agility —* but making it real has remained elusive.


But IBM Research is working to change that. Recently, at the Eurocrypt 2026 workshop on *Migration and Agility in Cryptographic Systems* (MAgiCS), we presented a[principled approach](https://doi.org/10.1007/978-3-032-28946-9_9) to solving the cryptographic agility problem. The key insight is simple: those choices don’t belong in application code — they belong in a layer that can be governed and updated independently.


The[ideas](https://pkic.org/events/2025/pqc-conference-kuala-lumpur-my/WED_B_1200_navaneeth-rameshan_a-framework-for-cryptographic-agility_merged.pdf) were first shared with a broader audience at the PKI Consortium conference in Kuala Lumpur in November 2025, and the feedback was clear. Cryptographic transitions consistently stall because developers carry both the implementation burden and the operational complexity.


Since the initial presentation, the work has moved from concept toward practice. Alongside the research papers, we have begun a partial, work-in-progress reference implementation and supporting documentation. The goal is to enable the community to test, validate, and shape this approach early — and to finally introduce the kind of abstraction layer for cryptography that storage and networking have had for decades.


## Intent-based cryptography: from “how” to “what”


At its core, our idea is simple. Applications should not decide *how* cryptography is performed — they should only express *what* they need. The details of algorithms, parameters, and implementations move into a layer below, where they can be managed centrally and updated over time.


To make this a reality, we devised a new, intent-based API that introduces a vocabulary based on “scopes.” A scope represents a class of cryptographic intent, such as standard digital signatures or authenticated encryption. Within each scope, different algorithms can be used interchangeably because they share the same input and output structure. This allows systems to evolve without breaking application code. A scope says what is needed; policy decides which algorithm satisfies it — set centrally, not written into application code.


This shift fundamentally changes how cryptography is used. Developers no longer have to encode low-level decisions across hundreds of services. Instead, cryptographic behavior becomes an operational concern. Security teams can define which algorithms are allowed and how they are used. Systems then enforce these decisions consistently at runtime. This is the move SDN made for networks — separating the control plane that decides policy from the data plane that does the work. And because policy is decided and enforced in one place, auditors get a clear, reliable view of what was used and why.


Cryptographic systems must adapt continuously: keys are renewed, standards evolve, and much of the pressure comes not from new algorithms alone but from vulnerabilities, compliance requirements, and operational decisions. The ability to change implementations quickly — without touching application code — is therefore essential. An abstraction layer enables exactly that, treating cryptographic backends as interchangeable providers behind a single interface. The same operation can run on a software library, a hardware accelerator, a cloud service, or a trusted execution environment. Most importantly, it cleanly separates what is needed from how it is done, and where it runs.


## Why now: PQC as a once-in-a-generation opportunity


The transition to[post-quantum cryptography (PQC)](https://www.ibm.com/quantum/quantum-safe) makes this challenge both urgent and visible.


Moving to PQC is not just about swapping algorithms. In reality, many systems cannot adopt PQC without significant changes first. Dependencies need to be upgraded, interfaces evolve, protocols change, and applications must be re-tested end to end. Many systems will require updates, such as newer runtime environments (e.g., Java 27+), support of TLS 1.3, or integration with latest versions of dependencies (with possibly new interfaces). In short, PQC is forcing a large-scale rearchitecture across the industry.


This moment creates a rare opportunity. Since software and systems must be revisited, this is the right time to fix a long-standing architectural gap. Instead of repeating today’s model with new algorithms, organizations can introduce a proper abstraction layer and move to a fundamentally better state.


Treating PQC as a one-time migration will only recreate the same challenges in the future. Cryptography will continue to evolve, and systems need to be designed for that reality. Introducing a cryptographic abstraction layer is therefore a move the industry would not regret. It reduces developer burden, strengthens governance, and turns future changes from costly engineering efforts into controlled operational decisions.


## Try it, challenge it, help shape it


Filesystems and sockets once looked like unnecessary layers of indirection. A decade later, they became the obvious foundation of modern systems. Cryptography is ready for the same shift.


A specification only becomes meaningful when it is tested in practice. That is why, alongside the technical papers, we are releasing:


- The[API specification](https://github.com/agile-crypto/api)
- A reference implementation (partial and work in progress) — a[standalone server](https://github.com/agile-crypto/citius-server) and a[Go client SDK](https://github.com/agile-crypto/citius-go-sdk)


We invite developers, architects, and security teams to explore the work, challenge the assumptions, and help shape its evolution. Because the question is no longer whether cryptography will need to change again — it’s whether we are finally ready to make that change easy.


## References


- [Cryptographic Agility for Applications: An Assessment Framework and Principled API Design](https://doi.org/10.1007/978-3-032-28946-9_9) – paper at MAgiCS workshop co-located with Eurocrypt 2026
- A Framework for Cryptographic Agility – presentation at PKIC 2025 ([slides](https://pkic.org/events/2025/pqc-conference-kuala-lumpur-my/WED_B_1200_navaneeth-rameshan_a-framework-for-cryptographic-agility_merged.pdf) ,[video](https://www.youtube.com/watch?v=F3f0IrlIkMs) )
- [An Assessment Framework for Application-Level Cryptographic Agility](https://arxiv.org/abs/2606.13425)
- [Intent-Based Cryptographic API Design for Cryptographic Agility](https://arxiv.org/abs/2606.13445)
