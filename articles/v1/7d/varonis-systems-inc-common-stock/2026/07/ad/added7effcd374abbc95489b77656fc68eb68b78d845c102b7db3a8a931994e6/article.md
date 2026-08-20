---
schema_version: "1.0.0"
document_id: "added7effcd374abbc95489b77656fc68eb68b78d845c102b7db3a8a931994e6"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-rss-915499d71e96"
canonical_url: "https://www.varonis.com/blog/ai-share-links"
published_at: "2026-07-29T17:03:24+00:00"
first_seen_at: "2026-07-29T18:55:04.772869+00:00"
fetched_at: "2026-07-29T18:55:06.469187+00:00"
content_hash: "sha256:b3528a44ece989b6e8d9be82ae06c011cfc211d81c4e8ead20a87f772ebdacd8"
---

# When AI Assistant Share Links Become Public Exposure

Every major AI assistant likely includes a "Share" button. The mental model users hold is a private hand-off, like a link you paste to a colleague. However, the mental model the web holds differs and is worth exploring.


## Why AI assistant share links are risky


A share link is an unauthenticated, permanent, crawlable HTTP resource sitting on a high-authority domain. The only things standing between the link and a search index is a header, a robots directive, and a vendor remembering to set both.


In corporate environments, the risk goes beyond "someone shared a link." The[blast radius](https://www.varonis.com/customer-stories/how-united-community-bank-reduces-their-blast-radius?hsLang=en) — how much damage is likely if a user is compromised — depends on what the user has access to before the prompt was written. Although the share link for enterprise licenses is limited, if an over-permissioned employee uses a personal AI account on a corporate device, a single shared conversation or artifact can accidentally carry sensitive files, customer context, code, tickets, or internal decisions into a public and durable surface.


The AI platform becomes the publishing layer, but the organization's permission model determines how much can leak through it.


## The surface


Share endpoints are almost universally a fixed path plus a high-entropy identifier. The entropy defeats brute force — a v4 UUID is not guessable — which is irrelevant. Enumeration never happens by guessing; it happens because a crawler was allowed to fetch the page once. After the fetch, the identifier is in an index, not a namespace.


Platform


Public share URL pattern


ChatGPT


chatgpt.com/share/<uuid v4>


(legacy: chat.openai.com/share/…)


Claude


claude.ai/share/<uuid v4>
claude.ai/public/artifacts/<uuid>


Grok


grok.com/share/<base64-prefix>_<uuid>


Gemini


g.co/gemini/share/<hex>


gemini.google.com/share/<hex>


DeepSeek


chat.deepseek.com/share/<token>


Perplexity


perplexity.ai/search/<slug>-<id>


perplexity.ai/page/<slug>-<id>


Meta


meta.ai/s/<id>


Manus


manus.im/share/<id>?replay=1


Kimi


kimi.com/share/<id>


Claude share links have been indexed by various search engines, a topic that has[been trending](https://x.com/om_patel5/status/2081494782396747779?s=46) in the security industry because of its potential impact, but also because of how it was disclosed.


The findings were first discussed publicly on Reddit, which accelerated attention on the issue and prompted broader discussion around responsible disclosure practices. Attention to share links was amplified by similar exposures found in ChatGPT in 2025, and later across other AI platforms, leading many to believe the issue had been addressed industry-wide.


While major search engines reacted quickly and removed the exposed content from indexes, remediation efforts varied among platforms, with some taking longer to fully resolve the exposure.


Nearly every vendor now ships the correct headers. Share pages return` <meta name="robots" content="noindex">` , most add` X-Robots-Tag: noindex` at the edge, none publish sitemaps for the share path, and several disallow it in robots.txt. Run the baseline dorks today and the counts are a fraction of what they were.


"Noindex" is a discoverability control, not an access control. The page still resolves, unauthenticated, for anyone holding the URL.


## The archives


The archives are where the story diverges from press coverage. The vendors negotiated with search engines, but almost none of them negotiated with archivists.


The Wayback Machine now excludes the bulk of the share paths for most platforms; requests were made, honored, and a large share of captures went dark. However, in some cases there are alternative domains for the same platforms that still work.


For example,` https://chatgpt.com/share/*` is excluded in the Wayback Machine, while` https://chat.openai.com/share/*` is not, and still shows the shared pages.


Look at Google: both the full` https://gemini.google.com/share/*` is excluded in the Wayback Machine, while the short link https://g.co/gemini/share/* is not excluded — though it is not delivering the content of the pages.


DeepSeek share pages are also still indexed and accessible via Google dorking, as mentioned in the table above.


Grok also has its shared chats indexed and accessible.


## The bottom line


The share button in AI assistants was designed for quick collaboration, but in practice, it can behave like a publish button. AI-shared links can remain live for years, appear in public archives, expose sensitive topics via URLs, and survive deindexing or revocation efforts.


For organizations, the risk is not limited to whether a specific platform currently allows public conversations or artifacts. The broader issue is shadow AI usage combined with over-permissioned access. When sensitive organizational data enters personal or unmanaged AI tools, the potential blast radius depends on what that identity could access in the first place. That visibility is rarely monitored — the opposite of what it should be.


Thank you to[Mark Vaitsman](https://www.linkedin.com/in/mark-vaitzman/) and[Dor Yardeni](https://www.linkedin.com/in/dor-yardeni/) for their contributions on the topic.
