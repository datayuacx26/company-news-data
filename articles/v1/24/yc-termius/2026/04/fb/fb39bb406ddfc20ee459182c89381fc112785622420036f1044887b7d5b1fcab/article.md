---
schema_version: "1.0.0"
document_id: "fb39bb406ddfc20ee459182c89381fc112785622420036f1044887b7d5b1fcab"
company_key: "yc-termius"
company: "Termius"
source_id: "yc-termius-news-import-2bcf9b9228b4"
canonical_url: "https://termius.com/blog/post-quantum-cryptography"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-24T03:42:18.942730+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:6565b555d79ef8cb4379d8e82bb4655c14abed68823afea31453bb008470faf1"
---

# Post-quantum cryptography - Termius Blog

Post-quantum cryptography is a new generation of protection designed to resist future quantum computing attacks. Once quantum computers become a reality, they will break most of today's encryption. That moment is closer than you might think – and Termius is already prepared.


We added support for **ML-KEM** and **ML-DSA** , two post-quantum cryptographic algorithms that bring quantum-resistant protection to both SSH key exchange and authentication. They keep your SSH connections secure against the threats of future quantum computers.


**ML-KEM** and **ML-DSA** are defined by **NIST** (National Institute of Standards and Technology) in their standards **FIPS 203** and **FIPS 204** . Both algorithms are also recommended by **ENISA** (European Union Agency for Cybersecurity), making them officially approved standards by governments and cybersecurity authorities in both the US and Europe.


## Why it matters


Most modern encryption relies on mathematical problems that would take today's computers thousands or even millions of years to solve. However, quantum computers, once they become a reality, could break these algorithms in days or even hours.


For decades, quantum computers have been a theoretical concern and the threat felt comfortably distant. Most experts now estimate that a quantum computer could arrive within the next decade. In 2024, NIST finalized the first official post-quantum cryptographic standards and published a migration deadline – signaling that governments and standards bodies were no longer treating this as a future problem.


Quantum computers pose **two major** risks that need to be considered today, not in the distant future.


### "Harvest now, decrypt later"


Attackers can capture your encrypted SSH sessions traffic today and decrypt it once quantum computers become available. It's a long game, but a dangerous one – especially for the sensitive data.


### Impersonation attacks


Quantum computers would also be capable of breaking traditional signature algorithms like RSA, ECDSA, and Ed25519 used for authentication in SSH. That means an attacker could forge a server's identity to intercept your connection (man-in-the-middle), or impersonate you to any server and gain unauthorized access to your infrastructure.


## How Termius protects you


Termius has implemented two post-quantum algorithms, ML-KEM and ML-DSA, to secure your SSH session traffic and authentication.


### Protecting your sessions against harvest attacks


**ML-KEM** is a post-quantum key exchange algorithm that secures how your client and server establish a shared secret at the start of every SSH connection. Because this process is quantum-resistant, even if someone captures your encrypted traffic today, they won't be able to decrypt it – now or in the quantum era.


Termius supports:


-


**Hybrid mode** It combines post-quantum (ML-KEM) and classical (X25519) algorithms so that even if one is ever broken, the other still keeps your session secure. It is the recommended approach during the transition period. Post-quantum cryptography is still relatively new, and hybrid mode ensures you're protected even if an unexpected flaw in either algorithm is discovered in the future.


-


**Standalone ML-KEM** It is the fully post-quantum option – a clean, future-proof choice for environments where all systems already support the new standards.


ML-KEM is available in **OpenSSH 9.9** and is the default key exchange in **OpenSSH 10.0** . Termius automatically uses the strongest available security when you connect – you only need to keep OpenSSH up to date.


### Protecting your identity against impersonation


To address the impersonation threat, Termius supports ML-DSA keys – post-quantum secure keys for both host and user authentication. They come at three security levels:


-


**ML-DSA 44** – NIST security level 2


-


**ML-DSA 65** –NIST security level 3


-


**ML-DSA 87** – NIST security level 5


The risk here is less immediate than that of harvest attacks, and SSH servers haven't yet adopted ML-DSA, though the industry is moving in that direction. OpenSSH has committed to supporting post-quantum signatures in the near future, and Termius already has ML-DSA keys ready for when your servers catch up.


## Looking ahead


Post-quantum cryptography is no longer something you can ignore – it is becoming a compliance and risk management requirement.


Termius has already implemented post-quantum cryptography, ensuring your SSH sessions remain protected not just against today's threats but also against future attacks.


For more technical details, visit our[Documentation](https://docs.termius.com/security/post-quantum-cryptography)
