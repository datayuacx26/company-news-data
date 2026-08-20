---
schema_version: "1.0.0"
document_id: "3a6ee7ba304f5dc1fbfa63e92546f72300cfdace1cd722accee3f0347197ee29"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2025-09-02-qwen3-coder-private"
published_at: "2025-09-02T12:00:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:d85617890df194f429286eda3c72df77ff8941b8716246b3ef1d188c4b5b11d8"
---

# Launching qwen3-coder-480B

[← Back to Posts](https://tinfoil.sh/blog)


# Launching qwen3-coder-480B


Sep 2, 2025


•


2 min read


Tinfoil Team


If you've been paying attention to OpenRouter's usage stats, you've probably noticed something interesting: Qwen3 Coder is now the third most popular model for programming, right after Claude Sonnet and Grok Code Fast.


Starting today, we’re really happy to announce that we’ve added support for Qwen3 480B Coder through our API — with the same cryptographically verifiable privacy guarantees as our other models!


Every time you paste code into an AI assistant, you're making a bet. The bet goes like this: the productivity gain from using this tool exceeds the risk of my code ending up... somewhere else. For personal projects, this is easy math. For your company’s core differentiation, that may be a tough sell.


The 2025 StackOverflow Developer survey highlights that data security and privacy concerns are the biggest issue that prevents developers from adopting tools:


Even if they want to use it, IT and security teams can make the situation difficult:


Along with Qwen3 Coder 480B, we’re also releasing a command line tool[tinfoil-qwen](https://github.com/tinfoilsh/qwen-code) , which is a forked version of` qwen-code` with Tinfoil client side verification integrated. This lets you verify with every request that the model is running on secure hardware enclaves and no one, not even Tinfoil, can see it. You no longer have to worry about your private keys or proprietary IP leaking and you can use coding agents on your most sensitive codebases.


## Getting Started


The CLI is forked from Qwen's official qwen-code with attestation verification added. To get started, run:


Then type` tinfoil-qwen` and start coding. More detailed instructions are in the README.md.


## Looking Ahead


We'd love to hear from you if you're navigating AI coding in sensitive environments. And as always, please don’t hesitate to reach out if you have any questions or feedback for us!


---


*The future isn't about choosing between privacy and capability. It's about making those false choices irrelevant through better infrastructure.*


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-08-22-openai-encrypted-chatgpt)[Next Post](https://tinfoil.sh/blog/2025-09-24-private-chat-backups-local-first)
