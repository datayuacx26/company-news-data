---
schema_version: "1.0.0"
document_id: "7c3e705a84374b5afd9ca2e60a6aa1b0bd48fee74371f8f3de5e75cdd102a273"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-epic-lore-glm-52-ai-backlash"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:b7e3178c8faba0ecabec917342bd94591e658fa0804b396c25dd1e2c9203a3fa"
---

# Cosmic Rundown: Epic's Lore, GLM-5.2, and the AI Backlash

## Epic Games Announces Lore Version Control System


Epic Games has[announced Lore](https://lore.org/) , a new version control system designed for large-scale game development and media production.


The[Hacker News discussion](https://news.ycombinator.com/item?id=48571081) is digging into how Lore compares to Git LFS, Perforce, and other solutions for handling large binary assets. The core pitch: version control built from the ground up for projects where individual files can be gigabytes and teams span hundreds of contributors.


For content teams working with media-heavy projects, the pain points Lore addresses are familiar. Git was designed for source code. When your repository includes 4K textures, video assets, and compiled binaries, the standard tools start to break down.


---


## GLM-5.2 Takes Top Spot on Open Weights Benchmarks


Zhipu AI's[GLM-5.2 is now the leading open weights model](https://artificialanalysis.ai/articles/glm-5-2-is-the-new-leading-open-weights-model-on-the-artificial-analysis-intelligence-index) on the Artificial Analysis Intelligence Index.


The[discussion](https://news.ycombinator.com/item?id=48567759) covers benchmark methodology and whether GLM-5.2 is a practical replacement for Claude or GPT in production workflows. The[performance benchmarks](https://artificialanalysis.ai/models/glm-5-2) show competitive results across coding, reasoning, and general knowledge tasks.


This matters for teams building AI features into their products. Open weights models that perform at frontier levels give you options: run locally, deploy on your own infrastructure, or use a hosted API without vendor lock-in on the model layer.


---


## 60% of Consumers Say "AI" Is a Turnoff


A new WordPress VIP report found that[60% of US consumers say "AI" in brand messaging makes them less likely to engage](https://wpvip.com/future-of-the-web-2026/) .


The[Hacker News thread](https://news.ycombinator.com/item?id=48569278) is running hot with developers who have seen this firsthand. The word has been applied to everything from autocomplete to autonomous agents, and consumers have noticed.


The practical takeaway: describe what your product does, not what technology powers it. "Generate blog posts on a schedule" lands better than "AI-powered content generation."


---


## GrapheneOS Ported to Android 17


GrapheneOS has[been ported to Android 17](https://discuss.grapheneos.org/d/36469-grapheneos-has-been-ported-to-android-17-and-official-releases-are-coming-soon) , with official releases coming soon.


The[discussion](https://news.ycombinator.com/item?id=48561654) celebrates the project's continued development. In related news,[Volkswagen has started blocking GrapheneOS users](https://discuss.grapheneos.org/d/35949-volkswagen-app?page=3) from their mobile app, joining a growing list of apps that reject hardened Android installations.


For privacy-focused developers, the pattern is familiar: security features get flagged as "tampering" by apps that want to ensure they are running on stock, potentially less secure operating systems.


---


## Local Models Keep Improving


Vicki Boykis's post["Running local models is good now"](https://vickiboykis.com/2026/06/15/running-local-models-is-good-now/) continues to generate discussion, with the[thread](https://news.ycombinator.com/item?id=48555993) now at over 500 comments.


The thesis: local LLMs have crossed a usability threshold for daily development work. Combined with the SpaceX/Cursor acquisition news and ongoing model availability concerns, developers are actively evaluating what parts of their AI workflow they can run independently.


---


## Quick Hits


**New HTTP Query Method.**[RFC 10008](https://www.rfc-editor.org/info/rfc10008/) introduces a new HTTP Query method. The[discussion](https://news.ycombinator.com/item?id=48568502) covers how it differs from GET with query parameters and when you would use it.


**Image ransom scheme.** A developer documented how a service is[charging $5 to recover images](https://www.lutr.dev/want-your-images-back-sure-that-ll-be-5-dollars) after a migration. The[thread](https://news.ycombinator.com/item?id=48569954) is a reminder about data portability and owning your media infrastructure.


**US science funding in flux.** Scientific American reports that[US science is in chaos](https://www.scientificamerican.com/article/americas-compact-between-science-and-politics-is-broken/) , with researchers uncertain about grant funding and institutional support. The[discussion](https://news.ycombinator.com/item?id=48568058) covers the downstream effects on research labs and academic computing.


**DeepSeek avoids blacklist.** Reuters reports that the[US has held off blacklisting DeepSeek](https://www.reuters.com/world/china/us-holds-off-blacklisting-chinas-deepseek-more-than-100-firms-deemed-security-2026-06-17/) while adding over 100 other firms to the security risk list.


---


## What This Means for Content Infrastructure


The theme across these stories: developers want tools they control. Whether it is version control for large files, AI models they can run locally, or content infrastructure with portable data, the value of independence keeps increasing.


The AI backlash data from WordPress VIP reinforces something we have written about before:[shipping AI capabilities without leading with the label](https://www.cosmicjs.com/blog/why-we-dont-call-cosmic-an-ai-cms) is often the better approach. Describe the behavior, make it auditable, and let the technology be an implementation detail.


Cosmic is built on this principle. Your content lives in your bucket, accessible via REST API or TypeScript SDK. The AI features are real and central to the product, but the content infrastructure is the stable layer that survives model changes, policy shifts, and market consolidation.


---


*Sources:[Hacker News](https://news.ycombinator.com/) ,[Lore](https://lore.org/) ,[Artificial Analysis](https://artificialanalysis.ai/articles/glm-5-2-is-the-new-leading-open-weights-model-on-the-artificial-analysis-intelligence-index) ,[WordPress VIP](https://wpvip.com/future-of-the-web-2026/) ,[GrapheneOS](https://discuss.grapheneos.org/d/36469-grapheneos-has-been-ported-to-android-17-and-official-releases-are-coming-soon)*
