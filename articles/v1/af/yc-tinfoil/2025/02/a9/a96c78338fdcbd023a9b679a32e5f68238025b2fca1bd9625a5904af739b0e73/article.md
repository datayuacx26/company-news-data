---
schema_version: "1.0.0"
document_id: "a96c78338fdcbd023a9b679a32e5f68238025b2fca1bd9625a5904af739b0e73"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-02-03-running-private-deepseek"
published_at: "2025-02-02T12:23:29+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:4176b46063540621add926f54c268a5caf8b1d10cdcccb671ebacac35bca3355"
---

# Running Private DeepSeek R1 with Verifiable Security

[← Back to Posts](https://tinfoil.sh/blog)


# Running Private DeepSeek R1 with Verifiable Security


Feb 2, 2025


•


4 min read


Tinfoil Team


# Introduction


Today, we're thrilled to launch[Tinfoil Private Chat with DeepSeek R1](https://chat.tinfoil.sh/) , our first product designed to redefine trust in AI. We're replacing *blind faith* in inference providers with *verifiable privacy* . It's a particularly opportune time to do so: since R1 was released last week, we've seen an uptick in concerns around AI and privacy. Curiously, even[OpenAI employees](https://x.com/stevenheidel/status/1883695557736378785) are concerned about all the data users are sending to AI and cloud providers:


Source:[@stevenheidel on AI privacy and data sent to cloud providers](https://x.com/stevenheidel/status/1883695557736378785)


# "Do As I Say, Not As I Do"


The Chinese government might be spying on you,1 but so is[OpenAI](https://www.schneier.com/blog/archives/2024/02/microsoft-is-spying-on-users-of-its-ai-tools.html) ,2[your car](https://foundation.mozilla.org/en/privacynotincluded/articles/its-official-cars-are-the-worst-product-category-we-have-ever-reviewed-for-privacy/) ,[your AI wearable](https://rabbitu.de/articles/security-disclosure-1) , and the[US government](https://www.axios.com/2024/06/13/open-ai-security-nakasone-nsa) (maybe all at once). DeepSeek's servers have already had a[data breach](https://www.theverge.com/news/603163/deepseek-breach-ai-security-database-exposed) . The problem is that models like DeepSeek R1 are too large to run locally by most, and need to be run on large GPUs on the cloud. So to use the latest AI capabilities, you need to trust *many* third parties — all of which have the ability to see, collect, sell, and use your private data, even if they "pinky promise" to you that[they won't](https://www.ftc.gov/business-guidance/blog/2023/03/ftc-says-online-counseling-service-betterhelp-pushed-people-handing-over-health-information-broke) !


Or if you're a company with proprietary data trying to use AI applications, this leaves you with only a few unpleasant options: keep AI on-prem and sacrifice the enormous benefit of cloud deployments, or trust often unenforceable contracts — like Data Processing Agreements (DPAs) or access control policies offered by AI providers — while accepting the significant privacy and security risks that can arise even from accidental breaches. Or simply choose[not to use the technology](https://www.bloomberg.com/news/articles/2025-01-30/deepseek-s-ai-restricted-by-hundreds-of-companies-within-days) .


Just as using TLS to secure your internet connections ensures someone on the network can't see your financial details when you log into your bank's website, or using a secure messaging app like iMessage, Signal, or WhatsApp ensures your conversations remain private with end-to-end encryption, your interactions with AI chat assistants, AI financial assistants, or AI therapists should also be kept confidential. Your messages and private data must remain accessible only to you, never to the AI service providers or other snooping third parties.


# Replace trust with provable security


At Tinfoil, we're making it easy to deploy private AI applications with *verifiable* security guarantees — ensuring only end-users ever access their private data. Whether you're an individual who wants truly private chats with an LLM or an AI startup hoping to build trust with enterprise buyers, Tinfoil is building the tools to make it possible.


How it works, under the hood, is detailed in our other blogs, but at a high level, we are using hardware-enabled confidential computing technologies,[similar to](https://tinfoil.sh/blog/2025-01-30-how-do-we-compare) Apple Private Cloud Compute, but with additional layers of[hardware security](https://tinfoil.sh/blog/2025-01-10-tinfoil-enclaves-overview) ,[code transparency, and auditability](https://tinfoil.sh/blog/2025-01-13-how-tinfoil-builds-trust) .3


This gets you the highest levels of security and privacy that are currently available in cloud computing. Whether you're using[Tinfoil Chat](https://chat.tinfoil.sh/) , a[Tinfoil inference endpoint](https://tinfoil.sh/inference) , or an AI application deployed on the Tinfoil platform, we can guarantee (and you can verify) that no one — not the developer, not Tinfoil, not the cloud provider, nor any third party — ever has access to your data.


# What else is happening at Tinfoil?


The[private chat](https://tinfoil.sh/chat) and[inference endpoint](https://tinfoil.sh/inference) are just the tip of the iceberg. We're building an entire platform to deploy private, verifiable versions of AI applications: content moderation tools, secure code editors, medical and legal assistants, and more. Stay tuned for updates!


---


## Footnotes


1.


Recently,[large-scale attacks](https://www.wired.com/story/encryption-apps-chinese-telecom-hacking-hydra-russia-exxon/) on US telecommunications systems prompted US government officials to recommend that American citizens use end-to-end encrypted messaging apps.↩


2.


A standard enterprise contract with companies like OpenAI only specifies that they don't *train* on your data, not that they don't have access or analyze your data.↩


3.


This is critical to alleviate concerns regarding backdoors. As security researchers[like Matthew Green have highlighted](https://blog.cryptographyengineering.com/2025/01/17/lets-talk-about-ai-and-end-to-end-encryption/) , this can even be a concern for models running locally. *Complete* end-to-end transparency and *client-side* verification ensures that, if no such backdoor exists in our GitHub repository, then no such backdoors are present on the Tinfoil server you're connected to.↩


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-01-30-how-do-we-compare)[Next Post](https://tinfoil.sh/blog/2025-02-27-shrinking-complexity)
