---
schema_version: "1.0.0"
document_id: "5bf39bc9a6942b0976999dddb6ad0c2dcb82a86c2434e67e331ff3d77b914b77"
company_key: "yc-spruceid"
company: "SpruceID"
source_id: "yc-spruceid-rss-1607485977f6"
canonical_url: "https://blog.spruceid.com/digitalidentity-what-is-the-difference-between-remote-and-in-person-identity-proofing/"
published_at: "2026-05-15T22:37:37+00:00"
first_seen_at: "2026-07-24T02:02:18.818185+00:00"
fetched_at: "2026-07-28T21:55:53.408599+00:00"
content_hash: "sha256:7fc40af40057c2526d11a240c368ad3757bf9b1293e1df74825a993967282c4b"
---

# What Is the Difference Between Remote and In-Person Identity Proofing?

When a government program needs to verify that an applicant is who they claim to be, it faces a design decision that shapes the experience of every resident who enrolls: should that verification happen in person, or can it happen remotely?


The answer is not the same for every program. It depends on the level of assurance the program requires, the population it serves, and what barriers different approaches create for the people who need access most. Understanding the tradeoffs is foundational to designing a program that is both secure and genuinely accessible.


## What Identity Proofing Is


[Identity proofing](https://spruceid.com/learn/proofing?ref=blog.spruceid.com) is the process of verifying that a person exists in the real world and that the individual presenting credentials is that person. It is the step that comes before a credential is issued, and the quality of that step determines how much trust can be placed in the credential that follows.


Not all identity proofing is equivalent. The National Institute of Standards and Technology (NIST) defines three identity assurance levels in[SP 800-63](https://spruceid.com/learn/nist-guidelines?ref=blog.spruceid.com) , each corresponding to a different standard of evidence and verification. The method used (in person or remote) is one of the key factors that determines which assurance level a program can claim.


## In-Person Proofing


In-person proofing involves a trained agent examining physical identity documents and the applicant in the same location. The agent can inspect security features on the documents directly, compare the applicant's appearance to the photo, and apply judgment to inconsistencies that automated systems might not catch.


This is the basis for[IAL3](https://spruceid.com/learn/ial3?ref=blog.spruceid.com) , the highest identity assurance level under NIST SP 800-63. Programs that require IAL3 include those involving significant financial transactions, access to sensitive government data, or high-value benefits where fraud risk is elevated and the consequences of a mis-issued credential are severe.


The strength of in-person proofing is also its limitation. Requiring someone to appear at a physical location creates barriers that fall unevenly across the population. Rural residents may have no nearby enrollment office. People with disabilities may face mobility barriers. Working residents may not be able to take time away from work during office hours. A program that mandates in-person proofing as the only option can exclude a portion of the population it was designed to serve, not by intent, but by design.


## Remote Proofing


Remote proofing uses document capture,[liveness detection](https://spruceid.com/learn/liveness?ref=blog.spruceid.com) , and[facial matching](https://spruceid.com/learn/facial-matching?ref=blog.spruceid.com) to verify identity without requiring the applicant to appear in person. The applicant photographs their identity document, records a short video or selfie, and the system checks that the document is genuine, that the photo matches the applicant's live image, and that the image was captured from a real person at the time of submission.


This is the basis for[IAL2](https://spruceid.com/learn/ial2?ref=blog.spruceid.com) under NIST SP 800-63, and it is the approach used in most digital government enrollment flows today. Remote proofing is what allows a resident to enroll in a program from their phone, on their schedule, without visiting an office.


The accessibility benefits are significant. Remote proofing removes the geographic and logistical barriers that in-person requirements create. For programs serving large or dispersed populations, it is often the only approach that can achieve meaningful enrollment at scale.


Remote proofing also requires robust underlying technology. The document verification needs to catch forgeries that a human reviewer might not notice. The liveness detection needs to distinguish a live person from a photograph or synthetic image. The facial matching needs to be accurate across diverse populations. As[Why Liveness Checks and Facial Matching Are Not the Same Thing](https://blog.spruceid.com/digitalidentity-why-liveness-checks-and-facial-matching-are-not-the-same-thing-and-why-the-difference-matters/) explains, these are distinct functions, and both need to be present and well-implemented for remote proofing to deliver adequate assurance.


## The Equity Dimension


The choice between remote and in-person proofing is not just a security decision. It is an equity decision.


A program that requires in-person proofing can create access barriers that are not evenly distributed. The residents who face the most friction (distance from enrollment offices, inflexible work schedules, transportation limitations, disability) are often the same residents the program was designed to help. Designing for the most convenient case while leaving harder cases unsupported is a choice, even when it is not a deliberate one.


Remote proofing reduces those barriers. But it introduces a different kind of equity consideration: residents without reliable smartphone access, sufficient internet connectivity, or comfort with digital interfaces may find remote enrollment difficult in its own ways. Neither approach eliminates all barriers. The question is which barriers the program is willing to accept, for whom, and why.


## Matching the Approach to the Program


The right proofing approach depends on three things: the assurance level the program requires, the population it serves, and whether the friction introduced is proportionate to the risk being mitigated.


A high-value benefits program with significant fraud risk may warrant in-person proofing for initial enrollment, with remote options for lower-risk interactions afterward. A licensing program serving professionals who are geographically dispersed may be well-suited to remote proofing at IAL2, with in-person escalation available for cases that require additional verification. A resident services program with a broad population and lower individual transaction risk may be best served by remote proofing that prioritizes access.


Some programs use a hybrid approach: remote proofing for initial enrollment, with in-person verification available as an escalation path for applicants who cannot complete remote proofing or whose applications raise flags. This gives the program flexibility to serve a wider population while maintaining a higher-assurance option where needed.[What Is Identity Proofing and Why Does It Matter for Government Services?](https://blog.spruceid.com/digitalidentity-what-is-identity-proofing-and-why-does-it-matter-for-government-services/) covers the broader context of why proofing quality matters for everything that follows.


## The Data Dimension


Both approaches involve collecting identity data - but how much, and what happens to it afterward, varies by implementation.


A well-designed proofing process collects what the verification requires and nothing more. The document is checked, the match is confirmed, and the credential is issued. The underlying data (the document images, the live capture) does not need to be retained once verification is complete.[What Is Data Minimization and Why Does It Matter for Government Services?](https://blog.spruceid.com/zerotrust-what-is-data-minimization-and-why-does-it-matter-for-government-services/) makes the case for why retention decisions at the proofing stage are important.


## Designing for the Population You Serve


Identity proofing is where digital identity programs first encounter the real diversity of the population they serve. The choices made at this stage such as which method, which assurance level, which escalation paths, determine who can enroll easily, and who faces barriers.


Programs that design the proofing process with that diversity in mind, rather than defaulting to the technically simplest or least expensive option, are the ones most likely to achieve the enrollment rates and equity outcomes their program goals require.


SpruceID works with government programs to design identity proofing approaches that match assurance requirements to program risk, serve the full population, and protect resident data throughout the process. Reach out to us to learn more.


Building digital services that scale take the right foundation.


[Talk to our team](https://spruceid.com/contact-us?ref=blog.spruceid.com)


**About SpruceID** : SpruceID builds digital trust infrastructure for government. We help states and cities modernize identity, security, and service delivery — from digital wallets and SSO to fraud prevention and workflow optimization. Our standards-based technology and public-sector expertise ensure every project advances a more secure, interoperable, and citizen-centric digital future.


Share
