---
schema_version: "1.0.0"
document_id: "8d0c66fbd209284870911b19d12f4755893a7031bb8a55c4310bce7b9096080b"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-08-22-openai-encrypted-chatgpt"
published_at: "2025-08-22T12:00:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:6d4060910e245ac00f633e94f8ca41998bf2d7d33dda9c712889fd1018c33c20"
---

# Encrypted ChatGPT?

[← Back to Posts](https://tinfoil.sh/blog)


# Encrypted ChatGPT?


Aug 22, 2025


•


6 min read


Sacha Servan-Schreiber


# Introduction


Recently, Sam Altman said that OpenAI is considering a version of ChatGPT with "encryption" to preserve users' privacy. This comes after a court order forced OpenAI to hand over user conversations (including deleted chats), and people are increasingly getting concerned about their privacy when using AI chatbots.


Source:[Axios — OpenAI weighs encryption for ChatGPT temporary chats](https://www.axios.com/2025/08/18/altman-openai-chatgpt-encrypted-chats)


From the article:


> "We're, like, very serious about it," the OpenAI CEO said during a[dinner with reporters](https://www.axios.com/2025/08/15/sam-altman-gpt5-launch-chatgpt-future) last week. But, he added, "We don't have a timeline to ship something."


Details are sparse, so let's dive into what such a system might look like from a technical perspective. As we'll see, it may not be just technical challenges that would prevent OpenAI from building something like this, but rather the fact that they'd need to become an open-source company and live up to their name.


Specifically, a private version of ChatGPT would require open-sourcing their (likely proprietary) infrastructure to auditors and users in order to let third-parties verify the privacy claims; is that something OpenAI would be willing to do to provide chat privacy?


## Understanding "Encryption" for Chats


In order to understand the technical details, we first need to understand what encryption looks like when it comes to AI.


> Encrypted messaging keeps providers from reading content unless an endpoint holds the keys. With chatbots, the provider is often an endpoint, complicating true end-to-end encryption.


Let's see why this is a problem and what solutions we have to provide the highest level of privacy for AI inference, comparable to end-to-end encryption services like Signal and WhatsApp.


## Internet Encryption


The first thing to note is that all conversations in ChatGPT are *already* encrypted when traveling from your computer or phone to the OpenAI servers. This encryption happens because[chatgpt.com](http://chatgpt.com/) , as do most websites on the Internet, uses TLS to encrypt all network communications from your computer to the OpenAI servers. This type of encryption prevents a hacker on the same network as you from snooping on your conversations. However, because the encryption only happens at the network layer, OpenAI still has access to your conversations, which doesn't protect your privacy.


> "In this case, OpenAI would be a party to the conversation. Encrypting the data while it is in transit isn't enough to keep OpenAI from having sensitive information available to share with law enforcement."


To prevent OpenAI from seeing your conversations, it should simply *never get access to the secret decryption key* .


## Hiding the Key: Processing Without Access


To allow OpenAI to "process" your conversations and provide you with the AI output, we need to evaluate the AI model without OpenAI gaining access to the decryption key for the request. While this might sound a little paradoxical, doing so *is possible* using one of two technologies available today: (1)[fully homomorphic encryption](https://en.wikipedia.org/wiki/Homomorphic_encryption) or (2)[secure hardware enclaves](https://en.wikipedia.org/wiki/Trusted_execution_environment) .


Homomorphic encryption is a powerful type of encryption that enables computation over encrypted data without ever needing to access the decryption key. Unfortunately, this type of encryption typically adds several orders of magnitude in performance overheads relative to baseline (non-private) processing, making it a non-starter for AI workloads. Imagine waiting hours, even days, to get your response back!


Meanwhile, secure hardware enclaves isolate all processing to a "hardware vault" that nobody can see into, even the operators of the hardware themselves. Turns out, hardware enclaves are a great place to hide an encryption key!


So why doesn't OpenAI just deploy this today? This is where we identify a significant challenge for *closed-source* AI providers like OpenAI and Anthropic.


## Does Open Source Have a Privacy Advantage?


An important (yet often overlooked) problem that emerges when thinking about privacy is *verifying* that your privacy is protected. While you can sign a legal contract with OpenAI to "not log or train on your data", this doesn't prevent them from accessing it — and indeed there is no way of stopping compelled access by courts or law enforcement even when the company has the best intentions. Therefore, it's helpful to think of the status quo as providing "pinky-promise" or "best effort" privacy guarantees, since there are no technological enforcements available to end users to ensure their privacy.


To get meaningfully better guarantees, it becomes important for users to be able to *verify* that the code was evaluated correctly by the provider. As a concrete example, ChatGPT users would need a way to verify that OpenAI set up the secure hardware correctly and evaluated the right inference code when serving your request. Without such verification, a court order or rogue employee could "turn off" the privacy protections with the end user being none the wiser.


## How to Ensure *Verifiable* Privacy


To provide such verifiability, OpenAI would need to, at minimum, open-source their inference infrastructure code. Only then would end users and external auditors be able to verify that OpenAI has no secret backdoors and that the encryption is enforced at the hardware level. This verifiability challenge was also highlighted by Apple when building their[Private Cloud Compute](https://security.apple.com/blog/private-cloud-compute/) platform, and is something we've spent a great deal of time thinking about at Tinfoil.


Interestingly, this is where open-source infrastructure has a big advantage — by having everything be public *by default* it becomes easier to provide end users with verifiability. This verifiability is also the foundation for Tinfoil, and why we open-source all the inference and security infrastructure. When it comes to privacy, what matters isn't the language in the privacy policy that nobody reads, but rather the transparency of the codebase and *technical* mechanisms guaranteeing privacy.


Will OpenAI or Anthropic be willing to open source their code to provide private chats? In my opinion, that is the biggest blocker to the closed-source AI labs actually building private AI, and I’m curious to see how this evolves in the near future.


In the meantime, you can check out[Tinfoil Chat](https://chat.tinfoil.sh/) and[verify the security claims for yourself](https://blog.freedomtechhq.com/inside-apples-private-cloud-compute-can-confidential-ai-be-trusted-4279d494d9b3?gi=4682d865721c) .


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-08-05-gpt-oss-120b-privacy)[Next Post](https://tinfoil.sh/blog/2025-09-02-qwen3-coder-private)
