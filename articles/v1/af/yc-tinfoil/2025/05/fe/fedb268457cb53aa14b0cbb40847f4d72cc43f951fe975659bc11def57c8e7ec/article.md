---
schema_version: "1.0.0"
document_id: "fedb268457cb53aa14b0cbb40847f4d72cc43f951fe975659bc11def57c8e7ec"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-05-10-ohttp"
published_at: "2025-05-10T12:23:29+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:9d2b132b543443b869543e07ecf15e76e0b474e307cd31ec9443314f55104a16"
---

# Oblivious Is Not Always Private

[← Back to Posts](https://tinfoil.sh/blog)


# Oblivious Is Not Always Private


May 10, 2025


•


6 min read


Sacha Servan-Schreiber


# Introduction


As AI systems process more and more sensitive personal information, companies have started developing "private" AI offerings of various sorts. In this blog post, we unpack what privacy means in the context of AI applications. In particular, we will focus on why Oblivious HTTP (OHTTP) —[a new Internet protocol pioneered by Apple and Cloudflare for anonymity on the web](https://blog.cloudflare.com/stronger-than-a-promise-proving-oblivious-http-privacy-properties/) — is often not the right tool for AI privacy.


As demand for privacy in AI applications continues to grow, especially in enterprise contexts, we see different definitions of privacy emerge from AI providers.


For some AI providers, "privacy" merely means vague promises like "we don't train on your data"; while saying nothing about other ways they might use your data.


Others might claim privacy by using OHTTP, a protocol designed to decouple user identity from web requests. OHTTP is an excellent tool for identity protection and a critical component in many web-based systems. However, it's important to understand its specific design purpose, design context, and limitations when applied to AI systems.


In particular, OHTTP was designed for identity anonymization (hiding IP addresses) when performing web requests. It was **not** designed for applications where data privacy is needed. As such, it fails to provide meaningful privacy when it comes to AI applications, as we explain next.


# Understanding OHTTP: Protecting Who, Not What


OHTTP decouples user identity from web request content. It works by routing encrypted Internet requests and responses through a relay that sits between the client and its destination, separating who initiated the request from what was sent in the request. This provides *unlinkability* (i.e., anonymity) by hiding the user's IP address from the service provider.


## How OHTTP Works


OHTTP functions a bit like a VPN by hiding the user's identity from the destination server. However, unlike VPNs, where the VPN service knows both the client identity and the destination, OHTTP proxies the connection through two non-colluding entities called a Relay and a Gateway.


This ensures that the first entity (the Relay, which sees the client IP address) does not see the content of the request. Similarly, the second entity (the Gateway, which decrypts and processes the request) does not know the source of the connection.


## OHTTP provides identity unlinkability, not data privacy


While OHTTP prevents linking the user identity to requests, it is important to understand that **it only protects who sent the data, not the data itself** from the destination server. This works great for applications with fixed transactional requests that reveal information over time through metadata, such as DNS queries or telemetry reporting. In particular, OHTTP is designed for either accessing publicly available content (such as a DNS record) or sending generic data such as a crash report that does not contain personally-identifiable information (PII). However, when personal data is present in the request, all bets are off.


## OHTTP + AI ≠ Privacy


For AI applications processing emails, documents, or other content containing sensitive data or PII, this creates a big problem: Once decrypted at the destination, the actual content (including any PII) is fully exposed to the receiving server. If your data contains identifying information (like your name), OHTTP does not provide any meaningful privacy guarantees, since the PII is linked to your identity! It's analogous to wearing a hood and mask to protect your identity and then pinning your ID to your forehead.


# So is OHTTP Useful?


Yes! OHTTP is a critical component of a comprehensive Internet privacy solution. In many situations, the unlinkability property achieved by OHTTP is vital in preventing ISPs and other network observers from potentially learning sensitive information about you. For example, using OHTTP prevents ISPs from learning your browsing history or Apple from learning which applications you're running through crash reports and other telemetry sent from your devices. However, neither of these contexts involve sending over personal information that can be linked back to you.


For AI applications, we cannot assume that conversations and other queries are free of PII. Copy-pasting an email from your boss into ChatGPT? That contains PII and even potentially proprietary company information. In this case, ChatGPT can link the request back to you, regardless of whether you anonymized the connection through OHTTP. To have privacy in such cases, we need a stronger property: data privacy.


# Tinfoil Privacy: Protecting the Data Itself


Tinfoil provides actual *data* privacy through secure enclaves. Tinfoil protects code and data inside the enclave with privacy and integrity. Data privacy guarantees that only the inference logic ever has access to the data, and that all entities (including Tinfoil) outside the secure enclave environment never see your data.


## How Tinfoil protects your privacy


Secure enclaves provide an isolated, protected environment within the processor for sensitive operations. This isolation is enforced at the hardware level, ensuring neither the operating system nor other applications can access data being processed within the enclave. The secure enclave uses hardware-based memory encryption to isolate specific application code and data in memory. You can learn more about how Tinfoil uses secure hardware enclaves in[our other blog post](https://tinfoil.sh/blog/2025-01-10-tinfoil-enclaves-overview) where we provide a technical overview.


For AI applications processing personal data, secure enclaves **protect the data during use** . This means personal data remains *encrypted* even in memory during processing, allowing Tinfoil to run AI models in the cloud while maintaining full confidentiality of the data.


# Conclusion


As AI increasingly processes sensitive personal information, understanding the distinction between privacy technologies is crucial. OHTTP (and even VPNs) can create the illusion of privacy by hiding your IP address, but they are not designed to protect your data from the service provider. It is easy to shoot yourself in the foot by assuming that network-level anonymity also means your data stays private from the processor.


For users trusting AI systems with sensitive information — whether it's proprietary code, work emails, or personal documents — confidential computing or fully on-prem deployments are the only solutions that guarantee privacy regardless of the data you feed into the AI system. You can always use OHTTP in conjunction with Tinfoil to hide your IP address from us, but regardless of whether or not you use OHTTP, we *never* see your data.


As AI systems become more integrated into our professional and personal lives, the question isn't just "Does anyone know I used this service?" but rather "Can anyone see what I shared with this service?" For true privacy in AI applications deployed on the cloud, confidential computing needs to be part of the picture.


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-02-27-shrinking-complexity)[Next Post](https://tinfoil.sh/blog/2025-05-15-side-channels)
