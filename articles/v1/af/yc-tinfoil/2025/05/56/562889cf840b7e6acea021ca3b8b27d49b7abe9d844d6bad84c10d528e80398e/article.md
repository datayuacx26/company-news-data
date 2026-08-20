---
schema_version: "1.0.0"
document_id: "562889cf840b7e6acea021ca3b8b27d49b7abe9d844d6bad84c10d528e80398e"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-05-15-privacy"
published_at: "2025-05-15T12:23:29+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:d78aeaf61377e4bc5f6809dedb5fd298ee413b762c7d998a13e2fb72f330c31e"
---

# Got Privacy?

[← Back to Posts](https://tinfoil.sh/blog)


# Got Privacy?


May 15, 2025


•


9 min read


Sacha Servan-Schreiber


Updated Aug 15, 2025


# Introduction


We're seeing AI impact everything from creative writing to coding. Most of us feel the tectonic shift that AI has introduced both in our personal and professional lives, and with that, some of us also realize the magnitude of the data we send to these AI-powered applications. It's no secret that many companies are eager to use this data for profit. Aravind Srinivas, the CEO of Perplexity,[openly talks about this goal for his company](https://techcrunch.com/2025/04/24/perplexity-ceo-says-its-browser-will-track-everything-users-do-online-to-sell-hyper-personalized-ads/) .


But what if we want to use AI while keeping our data private? In this blog post, we explore the options we have today for private AI and see why secure hardware enclaves emerge as the desirable option when it comes to speed, convenience, and privacy. This is why at Tinfoil, we choose secure enclaves as the primitive to build private AI.


# The Privacy Challenge in AI


AI systems provide us with the most value when we share sensitive information. We need to trust them as much as our work and life partners (if not more). This creates an inherent privacy challenge: how can users benefit from AI assistance without compromising the confidentiality of their data? The key privacy risks in AI inference include:


1. **Data exposure** : Sensitive information might be accessed by unauthorized parties during processing or storage.
2. **Data persistence** : Our conversations and queries may be stored or logged for longer than promised or desired, making it impossible to know where our data ends up
3. **Secondary use** : Information shared during inference might be used for purposes beyond generating the immediate response (e.g., profiling and advertising)


Source:[TechCrunch — Sam Altman warns there’s no legal confidentiality when using ChatGPT as a therapist](https://techcrunch.com/2025/07/25/sam-altman-warns-theres-no-legal-confidentiality-when-using-chatgpt-as-a-therapist/)


# Current Options for Private AI


## 1. Policy-based ("pinky-promise") Privacy


Major AI providers like OpenAI and Anthropic offer privacy commitments through their terms of service, often including statements about data usage limitations.


**How it works** : These companies establish contractual agreements with specific privacy commitments about how they handle user inputs during chat and inference.


**Strengths** :


- Contractual commitments regarding specific data usage limitations
- Typically include defined data retention periods
- Enterprise offerings often feature enhanced privacy controls and guarantees


**Limitations:**


- Data typically processed in plaintext on provider infrastructure leaving it exposed to hackers, vulnerable to data leaks, and subject to future policy changes
- Limited technical mechanisms for users to verify data handling practices, we need to fully trust that these providers adhere to their promises


As such, policy-based approaches represent an important step in the privacy direction. However, they primarily rely on organizational controls and contractual agreements rather than technical mechanisms that can mathematically or physically prevent unauthorized data access.


Enforcing these policies often proves to be an impossible task for large organizations. For example, a[leaked 2021 internal Meta document](https://www.vice.com/en/article/facebook-doesnt-know-what-it-does-with-your-data-or-where-it-goes/) states:


> "We do not have an adequate level of control and explainability over how our systems use data, and thus we can't confidently make controlled policy changes or external commitments such as 'we will not use X data for Y purpose.'"


which highlights that we cannot rely on privacy policies to keep our data secure.


More recently, US District Judge Sidney Stein denied OpenAI's objections, noting that OpenAI's user agreement allows data retention when required by legal process, clearing for the[search of user's ChatGPT logs under court-negotiated terms](https://arstechnica.com/tech-policy/2025/07/nyt-to-start-searching-deleted-chatgpt-logs-after-beating-openai-in-court/) .


Source:[Ars Technica — NYT to start searching deleted ChatGPT logs after beating OpenAI in court](https://arstechnica.com/tech-policy/2025/07/nyt-to-start-searching-deleted-chatgpt-logs-after-beating-openai-in-court/)


## 2. Apple's Private Cloud Compute


Apple recently announced[Private Cloud Compute (PCC)](https://security.apple.com/blog/private-cloud-compute/) for its Apple Intelligence features, implementing a hardware-based approach to privacy.


**How it works** : PCC extends Apple's device security model to the cloud, using custom Apple silicon servers with secure enclaves and a hardened operating system.


**Strengths** :


- Hardware-level security protections
- End-to-end encryption from device to PCC nodes
- Data is only processed within secure enclaves and invisible to Apple


**Limitations** :


- Closed-source ecosystem with limited transparency
- Users must trust Apple's implementation
- Not available as a general-purpose solution for other AI applications


Apple's approach represents an important step in cloud privacy, leveraging their control over the entire hardware-software stack. However, its proprietary nature means users must still place significant trust in Apple's claims without the ability to fully verify the implementation.


## 3. PII Redaction


Companies like[Private AI](https://www.private-ai.com/) and[Redactor.ai](http://redactor.ai/) offer solutions to identify and remove personally identifiable information before it reaches AI systems.


Source:[PII Tools — Data Redaction: The Essential Guide](https://pii-tools.com/data-redaction-the-essential-guide/)


**How it works** : Automated systems scan text, documents, images, or audio to detect and redact sensitive information (names, addresses, financial details, etc.) before sending to AI services.


**Strengths** :


- Prevents sensitive identifiers from reaching AI providers
- Can be added as an additional preprocessing layer to all existing AI systems


**Limitations** :


- Depends on accurate identification of PII (risks missing some PII or contextual identifiers)
- Does not protect the overall content from the AI provider
- May reduce the quality and accuracy of the responses


PII redaction is valuable as a component of a privacy strategy but doesn't solve the fundamental issue of sharing data with third-party AI providers.


## 4. On-Prem Deployments


Running AI models locally, either at home or on their own infrastructure can represent the strongest form of privacy, limiting explicit exposure to any third parties.


**How it works** : Organizations deploy AI models on their own servers or infrastructure, eliminating the need to share data with external providers.


**Strengths** :


- Data never leaves organizational boundaries
- Complete control over data storage and retention
- No dependency on third-party privacy practices


**Limitations** :


- Generally limited to smaller, less capable models due to computational resource constraints (GPUs are expensive)
- Requires technical expertise to deploy, secure, and maintain
- Significant infrastructure investments for larger models
- Limited access to the latest AI capabilities


While local hosting offers strong privacy protections, it comes with significant tradeoffs in model capabilities, costs, and maintenance complexity. It can even introduce security vulnerabilities by relying on bespoke setups that may be more susceptible to hacking compared to standard cloud deployment options.


**Note on multi-tenancy** : Even on-prem, privacy risks reappear when considering that the system administrators have full access, or when multiple teams or customers share the same clusters, hosts, or GPUs. Co-tenancy can enable data leakage via misconfiguration, shared logs, and unauthorized access by the system administrators can violate privacy.


## 5. FHE-Based Solutions (Fully Homomorphic Encryption)


Startups like[Lattica.ai](http://lattica.ai/) are developing systems that use Fully Homomorphic Encryption to enable computation on encrypted data in the cloud.


Source:[NVIDIA — What Is Confidential Computing?](https://blogs.nvidia.com/blog/what-is-confidential-computing/)


**How it works** : FHE allows AI models to process data while it remains encrypted, providing the ability to use AI without ever exposing the raw data to the provider in any capacity.


**Strengths** :


- Data remains encrypted throughout processing
- Neither the cloud provider nor the model provider can see the unencrypted data
- Offers mathematical privacy guarantees rather than policy-based promises


**Limitations** :


- Significant computational overhead making it suitable only for tiny models
- Still in early commercial stages with very limited real-world adoption
- Cannot be used with large models due to massive performance constraints
- Vulnerable to attacks by[malicious inference servers](https://github.com/Hexens/awesome-fhe-attacks) , which requires significant additional computational overheads to mitigate effectively


FHE represents a promising direction but has traditionally faced practical challenges in performance that have limited widespread adoption. While some companies are working to make FHE more practical by optimizing for specific AI workloads, significant theoretical and architectural breakthroughs are still needed before FHE becomes a sufficiently practical tool for real-world applications.


## 6. The Secure Enclave Approach


Solutions like Tinfoil and services built on NVIDIA's confidential computing capabilities offer hardware-enforced isolation for AI workloads, similarly to Apple's Private Cloud Compute solution. See[our other blog post](https://tinfoil.sh/blog/2025-01-30-how-do-we-compare) or David Gobaud's[post comparing Tinfoil to Private Cloud Compute](https://blog.freedomtechhq.com/inside-apples-private-cloud-compute-can-confidential-ai-be-trusted-4279d494d9b3) .


**How it works** : These systems leverage hardware-based trusted execution environments (TEEs) that isolate memory and processing to prevent unauthorized access, even from administrators of the host machine.


**Privacy strengths** :


- Hardware-enforced isolation of computing resources
- Attestation mechanisms provide verification of the security environment
- Compatible with high-performance AI hardware (GPUs)


**Privacy limitations** :


- Requires trust in the hardware manufacturer (e.g., NVIDIA and AMD)
- Vulnerable to side-channel attacks, which we cover in-depth in our other[blog post about side-channels and our mitigation strategies](https://tinfoil.sh/blog/2025-05-15-side-channels)


NVIDIA's Hopper and Blackwell GPUs support confidential computing, enabling data privacy for AI workloads without performance overheads. This represents a state-of-the-art approach to practical AI with strong data confidentiality guarantees.


# The benefits of Tinfoil


Among these approaches, Tinfoil's use of secure enclave technology offers a notable solution for private AI inference. Here are the key aspects of why Tinfoil outperforms other solutions (summarized in Table 1):


1. **Hardware-Based Security Foundation** : Tinfoil leverages hardware-based TEEs (AMD SEV-SNP or Intel TDX) coupled with NVIDIA GPUs that support confidential computing, creating a secure processing environment for AI workloads.
2. **Comprehensive Memory Protection** : Tinfoil's approach includes memory encryption throughout the processing pipeline, providing protection against various types of access attempts during data processing.
3. **Verification Mechanisms** : The architecture allows for verification of security properties through attestation, helping organizations confirm the integrity of the secure environment where their data is processed.
4. **AI-Specific Design** : The platform is specifically engineered around AI applications, with an abstraction layer designed for the unique requirements of private inference use cases.
5. **Transparency Through Open Source** : Users can review open-source components to better understand the security implementation, enhancing confidence in the protection mechanisms.
6. **No Performance tradeoffs** : The approach is designed to maintain strong performance while adding security protections, making it suitable for production AI inference workloads.


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-05-15-side-channels)[Next Post](https://tinfoil.sh/blog/2025-08-05-gpt-oss-120b-privacy)
