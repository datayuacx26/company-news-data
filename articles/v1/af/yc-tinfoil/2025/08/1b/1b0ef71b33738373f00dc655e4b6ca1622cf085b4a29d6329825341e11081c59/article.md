---
schema_version: "1.0.0"
document_id: "1b0ef71b33738373f00dc655e4b6ca1622cf085b4a29d6329825341e11081c59"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-08-05-gpt-oss-120b-privacy"
published_at: "2025-08-05T12:00:29+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:a8ba5a8b45df9371b809149b4d5552b55c09f8e43dfa7246add79c0b00d09dcc"
---

# Launching gpt-oss-120b on Tinfoil

[← Back to Posts](https://tinfoil.sh/blog)


# Launching gpt-oss-120b on Tinfoil


Aug 5, 2025


•


3 min read


Tinfoil Team


Updated Aug 15, 2025


# Introduction


Today we're excited to announce that we have added[gpt-oss-120b](https://openai.com/index/introducing-gpt-oss/) to Tinfoil, now available through our[private chat](https://chat.tinfoil.sh/) and[inference API](https://tinfoil.sh/models/gpt-oss-120b) .


# The Privacy Problem with AI


Just last week, OpenAI CEO Sam Altman[made headlines](https://techcrunch.com/2025/07/25/sam-altman-warns-theres-no-legal-confidentiality-when-using-chatgpt-as-a-therapist/) with a stark reminder about AI use and personal privacy: "There's no legal confidentiality for users' conversations with ChatGPT."


Source:[@TheChiefNerd on Sam Altman discussing ChatGPT privacy](https://x.com/TheChiefNerd/status/1948282239453729213)


This highlights a fundamental issue with current AI systems, and how people share their most personal information with tools that do not respect their privacy.


Unlike conversations with doctors, lawyers, or therapists, your AI interactions have no privilege protection. As Altman put it, "People talk about the most personal shit in their lives to ChatGPT." From therapy sessions to legal advice, our most personal data is being fed into AI systems. And yet, OpenAI would be legally required to expose those conversations in case of a lawsuit,[as was recently mandated in the New York Times lawsuit](https://arstechnica.com/tech-policy/2025/06/openai-says-court-forcing-it-to-save-all-chatgpt-logs-is-a-privacy-nightmare/) .


This creates a critical problem when handling sensitive information. Patent attorneys drafting confidential documents, M&A lawyers working on deals, doctors discussing patient cases, or anyone working with proprietary or personal data faces an impossible choice: embrace AI for its efficiency or protect confidentiality.


With Tinfoil, **we never have access to this kind of user data** and therefore cannot disclose it to anyone, even if we were legally required to.


# Open-Source Models: Too Big to Run Locally at Scale


OpenAI just released state-of-the-art open-source GPT models: gpt-oss-20b and gpt-oss-120b. People on X and Reddit are excited to run them locally for privacy reasons — because local-only deployments mean your data never leaves your computer. However, running these models locally requires 80GB of VRAM, making them very slow and impractical for most users and organizations on consumer-grade hardware.


# Tinfoil: Have Your Cake and Eat It Too


With Tinfoil, you get the privacy of running your model locally with the speed and scalability of the cloud. Nobody can see your data — not us, not the cloud provider, not anyone. **We replace trust with provable security.**


At Tinfoil, we're making it easy to deploy private AI applications *in the cloud* with *verifiable* privacy guarantees, ensuring that only end-users ever access their private data. Whether you're an individual who wants truly private chats with an LLM or an AI startup hoping to deploy AI with state-of-the-art security, Tinfoil is building the tools to make it possible.


# How It Works


Under the hood, we use confidential computing similar to[Apple Private Cloud Compute](https://security.apple.com/blog/private-cloud-compute/) but with additional layers of hardware security, code transparency, and auditability. This delivers the highest levels of security and privacy currently available in cloud computing without sacrificing speed and scalability.


Whether you're using[Tinfoil Chat](https://chat.tinfoil.sh/) , the[Tinfoil inference API](https://tinfoil.sh/models/gpt-oss-120b) , or an AI application deployed on the Tinfoil platform, we give you *verifiable* privacy (meaning you can verify for yourself that our privacy claims are enforced at the hardware level) and that no one — not the developer, not Tinfoil, nor any third party — can see or access your data.


# Getting Started


You can try out gpt-oss-120b right now in our private chat and via our inference API. Also feel free to reach out atcontact@tinfoil.sh — we're always interested in hearing about your experience and feature requests.


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-05-15-privacy)[Next Post](https://tinfoil.sh/blog/2025-08-22-openai-encrypted-chatgpt)
