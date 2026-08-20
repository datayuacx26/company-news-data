---
schema_version: "1.0.0"
document_id: "7288c0419339b94392f7da4afe021c76e5bef9337dbad9622b58f513940f001a"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-01-30-how-do-we-compare"
published_at: "2025-01-30T10:43:28+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:6daae687f799bb60ef4acc34808a713f8a7898eee957eac15e699407920e1656"
---

# How Does Tinfoil Compare to Apple Private Cloud Compute?

[← Back to Posts](https://tinfoil.sh/blog)


# How Does Tinfoil Compare to Apple Private Cloud Compute?


Jan 30, 2025


•


9 min read


Jules Drean


# Introduction


In a[previous blog post](https://tinfoil.sh/blog/2025-01-10-tinfoil-enclaves-overview) , we covered how Tinfoil uses secure hardware enclaves to protect all data at the hardware level. We also explained how Tinfoil enables the deployment of private AI applications with verifiable privacy guarantees. However, the many technical details and moving parts can make it difficult to understand how Tinfoil differs from existing solutions for private AI.


Secure hardware enclaves (also known as trusted execution environments) are a central building block of Tinfoil. Such technology for low-level hardware isolation has been around for a while, and many different types of abstractions have been proposed to isolate software.


The first secure‑enclave platforms such as Intel Software Guard Extensions (SGX) provided a bare‑metal environment for isolated processes. This design keeps the trusted computing base (TCB) as small as possible, reducing the attack surface, but it also makes it difficult to run unmodified binaries that expect a full operating system beneath them.


In contrast, confidential computing and trusted‑VM solutions isolate entire virtual machines, allowing off‑the‑shelf software to run inside secure hardware. Platforms can layer additional software infrastructure on top of the underlying hardware mechanisms in order to expose different abstractions to the isolated software. For example, a minimal runtime like[Gramine](https://github.com/gramineproject/gramine) (which implements the required OS interfaces inside the enclave) can be added on top of SGX to support arbitrary C binaries.


This creates an interesting design space, with platforms such as[Azure Confidential Computing](https://learn.microsoft.com/en-us/azure/confidential-computing/overview) ,[AWS Nitro Enclaves](https://aws.amazon.com/ec2/nitro/nitro-enclaves/) , or the recent[Apple Private Cloud Compute](https://www.apple.com/newsroom/2024/06/apple-extends-its-privacy-leadership-with-new-updates-across-its-platforms/) . Each solution occupies a different point in this space along two dimensions: security guarantees and level of abstraction.


So back to the question of “How is Tinfoil different?” The short answer is that we focus on a specific application, verifiably private AI, which makes it possible to get the best of both worlds. We can build on the strongest hardware-based isolation primitives, while making it easy to use via a clear abstraction tailored specifically to AI.


# Security: Reducing Trust


Trust can be evaluated in multiple ways: the number of trusted parties, the size of the trusted code base, or the types of attacks considered. This means that different solutions are not strictly ordered. Nevertheless, we can roughly compare them to one another.


To make this discussion easier to follow, let’s first pop up one level and consider the setting that Tinfoil operates in. Tinfoil is a cloud computing setup, which means we have some *guest* software (a function, a process, or an entire virtual machine) that is running on top of a *host* machine. This host machine includes all the privileged software to orchestrate, isolate, and manage the guest software.


### A Brief History of Secure Enclaves


Secure hardware enclaves have a long history that stretches back two decades, with several academic proposals1 and the development of the Trusted Platform Module (TPM). The overarching goal has remained the same: to guarantee the privacy and integrity of a piece of software, even in the presence of an **attacker controlling the host** . In this malicious‑host *threat model* , the adversary controls the entire privileged software stack: operating system, hypervisor, firmware, and the physical machine itself. This is an *extremely* strong threat model unmatched by any other isolation primitives. For instance, process-isolation or virtualization protects a workload from other processes or VMs but not from the host. Sandboxes or classic containers add extra isolation to protect the *host* from untrusted guests. Secure enclaves are a step up from this: they also protect the guest from a malicious host.2


### Hypervisor-Based Secure Enclaves


At first, processors did not contain hardware mechanisms to completely isolate guest software. Instead, the first generation of secure enclaves relied on a small but trusted hypervisor (not controlled by the host) to enforce isolation between the host and guests.3 This[light hypervisor](https://support.apple.com/guide/security/operating-system-integrity-sec8b776536b/web#secd022396fb) is only expected to perform checks on security-critical operations and delegates all complex logic (e.g., memory-page management) to a full-fledged host-controlled operating system. This is the underlying architecture used by Apple Private Cloud Compute and Amazon Nitro Enclaves.


An important assumption here is that the mini-hypervisor is trusted. Apple and AWS likely have the resources to tailor and harden these key pieces of software. Nevertheless, at the time of writing, while Apple offers the option to inspect corresponding binaries, both projects remain *closed source.*4


Despite this lack of code transparency, the[Nitro hypervisor](https://docs.aws.amazon.com/whitepapers/latest/security-design-of-aws-nitro-system/the-components-of-the-nitro-system.html#the-nitro-hypervisor) has a pristine history when it comes to security. The AWS team is reputed for leveraging a modular architecture which enables formal verification at scale. Among other things, the Nitro hypervisor also enforces isolation between EC2 instances and the rest of the AWS infrastructure.


Nevertheless, in these existing solutions, the guest memory is *not* encrypted; Apple and AWS's trust assumptions suffer from closed ecosystems where trust cannot be easily distributed, and despite Apple leveraging some remote attestation mechanism, code transparency is limited: there is no way to verify that the running binary matches published open‑source code.


In contrast to these existing solutions, Tinfoil provides client-side verifiability *against open source code* and encrypts all memory at all times.


### Memory-Encrypted Secure Enclaves


Starting in 2015 with Intel SGX, we saw the introduction of isolation mechanisms fully implemented in hardware which make it possible to reduce the amount of trusted code. These hardware mechanisms add an extra layer of security by *encrypting* the guest memory, so it is not accessible to the (now untrusted) hypervisor, nor to a physical attacker that could access the DRAM.5 This protects against entirely new classes of attacks. While no system is ever perfectly secure,6 hardware-based secure enclaves provide the strongest level of guest isolation and security available today.7


### GPU-Powered Secure Enclaves


NVIDIA's latest GPUs, the Hopper and Blackwell architectures, are equipped with confidential computing capabilities. This means they can be configured to extend a CPU-only enclave with computational resources needed to run the largest AI models with negligible performance overhead. GPU memory is encrypted, as is the connection with the CPU secure enclave.8


At Tinfoil, our platform uses a combination of hardware-based secure enclaves (AMD SEV-SNP and Intel TDX) and NVIDIA GPUs with confidential computing enabled. It removes any trust assumption in the hypervisor and encrypts guest memory.


# Usability: Finding the Right Abstraction


> *So why aren't secure enclaves used everywhere?*


As is often the case in complex systems, the exposed abstraction is more important than the underlying technology.9


As we already mentioned, early hardware enclaves like SGX only offered a bare metal environment for developers. No syscalls, no runtime, no libc! This made SGX difficult to use, and despite efforts to improve developers' experience through better OS support,10 it failed to achieve widespread adoption. Other abstractions saw more success, such as trusted VMs or confidential computing as provided by Azure and other major clouds. Here the abstraction matches normal VMs, giving customers SSH access to a guest VM with an extra layer of protection against the host machine and the cloud provider. Nevertheless, when looking at deploying private AI applications where only the end-user should ever have access to their data, this abstraction is somewhat of a mismatch. The application provider has access to the guest, which makes it difficult for them to guarantee they cannot access their users' data. These platforms also often rely on[attestation](https://cloud.google.com/confidential-computing/docs/attestation)[services](https://learn.microsoft.com/en-us/azure/attestation/overview) hosted by the cloud provider. These closed source services add back trust assumptions on the cloud providers, making it more difficult to tell a simple security story.


### Private-AI as a Service


Tinfoil introduces a purpose‑built abstraction for deploying **verifiably secure AI applications** . Developers only have to: choose the AI model they want to run, and add a small amount of code that connects the model to generic, privacy‑preserving services (memory, inference/evaluation, telemetry, etc.).


Then they can deploy their AI service with the guarantee that the Tinfoil endpoints are private, stateless, and that only the end-user will ever have access to their personal data. We believe this service model is to AI applications what function-as-a-service has been to cloud computing: a native abstraction that directly fits the use case and simplifies many complex problems, starting, in our case, with user privacy.


# How is Tinfoil different?


In summary, we’ve developed an entire platform and AI stack to make it easier than ever to use secure hardware enclaves with the “right” abstraction to deploy private AI applications. We do so by leveraging hardware-based mechanisms available in modern GPUs and CPUs that guarantee the strongest levels of guest isolation available in the cloud. Beyond isolation, we use a series of techniques to reduce trust and make it possible for the end-users to *verify* these security guarantees on the fly.11


---


## Footnotes


1.


Early academic proposals include[XOM](https://dl.acm.org/doi/pdf/10.1145/356989.357005) or[Aegis](https://people.csail.mit.edu/devadas/pubs/aegis-istr-august6-2005.pdf) . The[TPM](https://trustedcomputinggroup.org/resource/trusted-platform-module-tpm-summary/) was the first widely available piece of hardware that would commoditize trusted computing.↩


2.


Secure enclaves also provide *code-integrity* guarantees in addition to isolation, but for the rest of this blog post, we’ll primarily focus on the isolation guarantees.↩


3.


This isolation between trust domains is enforced using separate page tables (hence separate memory spaces) and other[virtualization mechanisms](https://en.wikipedia.org/wiki/Virtualization) sometimes implemented in hardware for performance purposes.↩


4.


To the best of our knowledge, Apple publishes *some* hashes and binaries but provides no source code for them, with the exception of[some small security modules](https://github.com/apple/security-pcc) .↩


5.


In general, physical attacks are among the hardest to mount (they require physical access after all) with the exception of[Cold-boot](https://en.wikipedia.org/wiki/Cold_boot_attack) attacks. They make it possible for an attacker to read out the content of the DRAM by cooling it using a freeze spray and plugging it onto a different machine under the attacker’s control.↩


6.


Some advanced microarchitectural attacks are still possible (as on any other cloud platform), but these do not represent the main privacy concern for running AI workloads in the cloud. We will detail microarchitectural side channels and their implications in a future blog post.↩


7.


Cryptographic solutions such as multiparty-computation or fully homomorphic encryption offer better security based on mathematical assumptions. Nevertheless, they incur significant (10^4 - 10^10) performance overhead, making them impractical for large AI workloads.↩


8.


[Remote attestation](https://docs.nvidia.com/attestation/index.html#overview) also makes it possible to confirm the authenticity and correct operation of the NVIDIA GPU.↩


9.


In the late 60’s / early 70’s, computer hardware and systems were growing in complexity and delivering more power and capacities than ever before. Nevertheless, the lack of correct abstraction to these systems led to a[Software Crisis](https://en.wikipedia.org/wiki/Software_crisis) . This was solved by the[invention of object-oriented programming](https://en.wikipedia.org/wiki/Barbara_Liskov) , which gave a clear abstraction to tie data and programs.↩


10.


[Gramine](https://gramine.readthedocs.io/en/stable/) , previously Graphene or LibOS aimed at making off-the-shelf applications run inside SGX.↩


11.


See our detailed[blog post](https://tinfoil.sh/blog/2025-01-13-how-tinfoil-builds-trust) on these mechanisms and how we manage trust.↩


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-01-13-how-tinfoil-builds-trust)[Next Post](https://tinfoil.sh/blog/2025-02-03-running-private-deepseek)
