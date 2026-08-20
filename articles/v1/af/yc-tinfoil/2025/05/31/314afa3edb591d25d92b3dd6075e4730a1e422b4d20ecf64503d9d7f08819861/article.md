---
schema_version: "1.0.0"
document_id: "314afa3edb591d25d92b3dd6075e4730a1e422b4d20ecf64503d9d7f08819861"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-05-15-side-channels"
published_at: "2025-05-15T12:00:29+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:39cf561614e3040ac9021135ada3524aadab52824d76f934bae2a9c4c69e85ee"
---

# What About Side-Channels?

[← Back to Posts](https://tinfoil.sh/blog)


# What About Side-Channels?


May 15, 2025


•


8 min read


Jules Drean


# Introduction


I'm Jules, co-founder of Tinfoil and I'm excited to dive into one of my favorite topics: side-channels and secure hardware. In this blog post, we'll dive deeper into one of the most sophisticated attack vectors on secure hardware enclaves: architectural side channel attacks.


# What Are Side Channel Attacks?


Side channel attacks1 represent a class of security exploits that do not target implementation bugs in programs themselves, but rather the physical implementation of these systems. For instance, rather than attempting to break encryption directly, attackers would measure indirect information leakage — such as timing differences, power consumption, electromagnetic emissions, or cache access patterns — to infer the secret encryption key.


Side channels are very hard to fully mitigate, and they affect almost all known computer systems. They can breach isolation boundaries such as sandboxing, kernel isolation or virtual machine isolation. For example, when running code in the cloud, you are always at risk of a co-tenant using side channels to compromise the host machine kernel or extract information from other co-located users.


## Why are side channels ignored by the industry?


So if these attacks are so powerful, why are they not currently the main focus of cloud security? The reality is that these attacks are quite difficult to mount in practice. They are very sensitive to noise, require a high level of control over the environment, and exploit the specificity of the underlying hardware which sometimes makes them hard to generalize. That means that in practice, most cybersecurity breaches come from phishing, unpatched software, or exposed credentials — not from exploiting sophisticated side channel attacks.


## Why side channels matter


Should we not care about side-channels then? Absolutely not! These attacks might not be the low hanging fruit *for now,* but completely eliminating them requires deep architectural changes, sometimes all the way down to the hardware. That means mitigation efforts will take time to deploy and need to be properly synchronized over the entire industry. This is why side channels should be an important focus of security research. We need to better understand them and be able to address them in the long run. I have spent[my PhD on building defense mechanisms](https://drean.io/) , and I can tell you that we are still far from being able to fully address these attacks in the general case.


# What about secure enclaves?


As a demonstration of their impact, side channels have even been shown to break the highest level of isolation known on modern systems: enclave boundaries. That is a very impressive *tour de force* as enclaves were previously thought to be impenetrable. Indeed, most other classic attack vectors (aside from side-channels) are hindered once a sensitive computation is placed in an enclave. For instance, even compromising all privileged software on the host machine is not enough for an attacker to get their hands on protected data running in an enclave!2


What are the key takeaways from these results then? Well, not too surprisingly, one conclusion is that no system is perfectly secure and you should be suspicious of anyone telling you otherwise! However, this demonstrates that enclaves significantly raise the security bar, since these attacks require sophisticated compromises of the host machine — scenarios that would normally mean "game over" for security. An attacker must do more than just compromise the cloud provider, gain privileged access to the host, or obtain physical access to the machine. They must also successfully execute a microarchitectural side channel attack through the noise of a production system, all while avoiding detection!


# How does Tinfoil protect against side channel attacks?


Now on to the fun part (for me at least!). Old habits die hard: given the state of affairs, I cannot refrain from asking the exciting research question: "What simple measures can be taken to limit the impact of side channels on a system like Tinfoil?" Well, it turns out for our specific use case, we actually have a few interesting insights.


## GPUs will not go first


GPUs are not the best hardware to mount these types of attacks on; for instance, they lack speculation, a hardware-level performance optimization that plays an important role in the most efficient versions of these attacks. That does not mean they are exempt from side channels, but in practice, the kinds Tinfoil would be vulnerable to have not been demonstrated, and the CPU is a much easier target.


## Hardening the environment


First things first, our machines are completely private to Tinfoil and entirely dedicated to running our inference workloads. This means we only run our own code on these machines, and users are never allowed to execute arbitrary code. Good luck mounting side channels without control over the attack code! Additionally, we never share CPU cores across security domains — whether across time periods or through hyper-threading.


## Avoiding the SGX "kiss of death"


Due to Intel's desire for anonymity in their signature scheme, a specific configuration made it possible for attackers who extracted one key from an SGX enclave to extract all keys! We avoid this "kiss of death" issue since we want the opposite of anonymity for our enclaves: users should be able to verify that they are communicating with a genuine Tinfoil enclave. This is accomplished by simply adding a Tinfoil private key signature to any signature originating from an enclave.


## Microarchitectural Isolation of secrets


Without going into details, *microarchitectural* side channels exploit shared *microarchitectural* state. If your secrets are isolated on a different chip that doesn't share microarchitecture with your attacker, they're simply not accessible to these types of attacks. While not all secrets can be kept on a separate chip, some designs, like the AMD processors, store many secrets in the PSP secure co-processor. This means only application secrets explicitly present on the CPU can be exfiltrated through side channels. In our case, these are: (1) the client requests themselves, and (2) the keys used for the TLS connection between the client and the enclave.


## Leveraging statelessness


One beautiful thing about AI inference workloads is that they are intrinsically *stateless* . Requests arrive, get processed, output is sent back, and that's it! All state can be flushed and discarded. This is fantastic for mitigating side channels because with simple rate limiting, we can make it extremely difficult for attackers to collect enough samples (thousands are required) to extract anything meaningful from a request. In practice, this means our only Achilles' heel for side channels are the TLS keys I mentioned earlier.


## Forward secrecy


Here again, we can use similar principles: by rotating keys regularly, we can ensure that these are harder to extract, and that even in the case of a compromised key, it would only affect a limited set of user requests.


# What's next?


We already have several side-channel defenses implemented here at Tinfoil. An attacker would first need to compromise Tinfoil to have any chance of mounting a successful attack; and with everything we've put in place, that would be no easy feat! We could push things even further by reinforcing some of the points above. First, we could completely attest all code running on our machines, including the host. This would eliminate the possibility, even for us, to mount the vast majority of these attacks. Additionally, we're excited to provide formal guarantees on our computation statelessness in the future. This goal is achievable through formal verification methods and by using a programming language like Rust that provides strong memory safety guarantees. Finally, we could quantify information leakage in our environment using formal tools to evaluate the effectiveness of our countermeasures. While this is exciting research territory, especially for a side-channel nerd like me, my co-founder wisely suggested we might want to wait a few years before spawning a research branch at Tinfoil.


# Conclusion


Side channels are fascinating! While they're difficult to exploit in practice, they potentially affect every known system, and there's still no consensus on how to fully protect against them. This makes them an ideal topic of study for researchers like me. At Tinfoil, we're excited to have architected our system with side channels in mind from day one. Sure, this might seem a bit security-paranoid — but hey, we didn't choose our company name lightly!


---


## Footnotes


1.


Note that in this blog post, we will mostly focus on microarchitectural and physical side channels. Other types, working more at a system-level, exist as illustrated by[this great paper](https://arxiv.org/pdf/2403.09751) shared by our friends from[Casco](https://getcasco.com/) . However, their handling is a bit orthogonal and easier to address (you should still do it!).↩


2.


It should be noted that enclave systems have also been found vulnerable to good old bugs present in security-critical code. These are unavoidable as bugs are always present in any system, the game here is to reduce the number of lines of code that could contain bugs (i.e., the trusted code base or TCB), which is the game enclaves are trying to play. Unlike side channels though, these vulnerabilities can simply be patched (you should still patch them!).↩


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-05-10-ohttp)[Next Post](https://tinfoil.sh/blog/2025-05-15-privacy)
