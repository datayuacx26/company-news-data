---
schema_version: "1.0.0"
document_id: "3692a6150df35ef467d749487d6e182f7ec9ed5674dbcd253fa44427455ebf7b"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-midjourney-medical-deepseek-vision-amd-security"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:f9e2b0e0c306e95124445c086c153917e1a4d75222408eb453fb77f5442c9332"
---

# Cosmic Rundown: Midjourney Medical, DeepSeek Vision, and AMD's Silent Security Change

## Midjourney Medical


Midjourney[announced a medical imaging product](https://www.midjourney.com/medical/blogpost) that generated significant discussion. The company known for AI image generation is now targeting healthcare applications. The[Hacker News thread](https://news.ycombinator.com/item?id=48579650) pulled in hundreds of comments, with developers debating the regulatory path, the technical challenges of medical imaging, and whether this signals a broader move by AI image companies into specialized verticals.


The timing is notable. Healthcare AI is one of the few sectors where "AI" branding might actually help rather than hurt, given the technical complexity involved.


## DeepSeek Adds Vision


DeepSeek[introduced vision capabilities](https://chat.deepseek.com/) to its model lineup. For developers building multimodal applications, this expands the options beyond the usual suspects. The[discussion](https://news.ycombinator.com/item?id=48581458) centered on benchmark comparisons and how the vision features stack up against GPT-4V and Claude's image understanding.


This comes as the US[held off on blacklisting DeepSeek](https://www.reuters.com/world/china/us-holds-off-blacklisting-chinas-deepseek-more-than-100-firms-deemed-security-2026-06-17/) despite adding over 100 other firms to security risk lists. The geopolitics of AI model access continues to be a factor developers need to track.


## AMD Removes Memory Encryption


Tom's Hardware reported that AMD[silently removed memory encryption from consumer Ryzen CPUs](https://www.tomshardware.com/pc-components/cpus/amd-silently-removes-memory-encryption-from-consumer-ryzen-cpus-leaving-users-unaware-that-they-may-be-vulnerable-security-feature-vanishes-after-newer-agesa-firmware-amd-engineers-go-radio-silent-when-pressed-about-the-change) . The feature disappeared after a firmware update, and AMD engineers reportedly went silent when asked about the change.


The[Hacker News discussion](https://news.ycombinator.com/item?id=48582320) raised concerns about security transparency. For developers running sensitive workloads on consumer hardware, this is a reminder to audit what security features your infrastructure actually has versus what the spec sheet promised.


## Quick Hits


**Microsoft Outlook performance** : New Outlook[takes 10 seconds to do what Outlook Classic does instantly](https://www.windowslatest.com/2026/06/15/microsofts-new-outlook-takes-10-seconds-to-do-what-outlook-classic-does-instantly-on-windows/) . The[thread](https://news.ycombinator.com/item?id=48584207) is full of developers sharing performance frustrations with modern rewrites of classic applications.


**Emacs 31 incoming** : A developer shared[the changes they're already using](https://www.rahuljuliato.com/posts/emacs-31-around-the-corner) from the upcoming release. Tree-sitter improvements and native compilation refinements are the highlights.[Discussion here](https://news.ycombinator.com/item?id=48584135) .


**GitHub malware** : A security researcher[found 10,000 GitHub repositories distributing Trojan malware](https://orchidfiles.com/github-repositories-distributing-malware/) . The[conversation](https://news.ycombinator.com/item?id=48583928) covers detection methods and whether GitHub's automated scanning catches these patterns.


**Swiss nuclear policy** : Switzerland's parliament[lifted the ban on new nuclear power plants](https://www.bluewin.ch/en/news/switzerland/parliament-lifts-ban-on-new-nuclear-power-plants-3257535.html) . Combined with the[TerraPower deal with Meta](https://neutronbytes.com/2026/01/09/terrapower-in-mega-deal-with-meta-for-eight-natrium-345-mw-advanced-nuclear-plants/) for eight advanced nuclear plants, the energy infrastructure conversation is shifting.


**Local AI tools** : A thoughtful post argues that[local Qwen isn't a worse Opus, it's a different tool](https://blog.alexellis.io/local-ai-is-not-opus/) . The[discussion](https://news.ycombinator.com/item?id=48580209) is worth reading if you're evaluating local versus cloud AI for your stack.


**Advanced compilers course** : Cornell published a[self-guided online course on advanced compilers](https://www.cs.cornell.edu/courses/cs6120/2025fa/self-guided/) . Free, rigorous, and covering optimization techniques that most CS programs skip.


## What This Means for Content Teams


The Midjourney Medical announcement is a signal. AI companies are moving from general-purpose tools to specialized verticals where domain expertise matters. For content teams, this suggests the tools you use today will look very different in 18 months.


The AMD security story is a reminder about infrastructure assumptions. If you're managing content that requires security compliance, verify your actual security posture rather than trusting documentation.


And the local AI discussion matters for anyone building AI into their content workflow. The right tool depends on your constraints: latency, privacy, cost, and capability all trade off differently between local and cloud models.


---


Building content infrastructure that adapts as the AI landscape shifts?[Cosmic's headless CMS](https://www.cosmicjs.com/) is model-agnostic by design. Your content layer stays stable while you swap models as better options emerge.


[Start free](https://app.cosmicjs.com/signup) or[talk to Tony](https://calendly.com/tonyspiro/cosmic-intro) about your architecture.
