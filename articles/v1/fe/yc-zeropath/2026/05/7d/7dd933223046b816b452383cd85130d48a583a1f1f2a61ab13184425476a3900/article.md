---
schema_version: "1.0.0"
document_id: "7dd933223046b816b452383cd85130d48a583a1f1f2a61ab13184425476a3900"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/zeropath-outperforms-mythos-in-real-world-test"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-24T06:50:52.708520+00:00"
fetched_at: "2026-07-28T22:08:49.880649+00:00"
content_hash: "sha256:06d9ab64a6966ef5ad7ba578ceb2db9a0535cd58b6c9b73d242a00e5e36679c6"
---

# ZeroPath Outperforms Mythos In Real World Test

At the end of 2025, Security Researcher Joshua Rogers used ZeroPath and other AI-powered SAST scanners to analyze curl. The project fixed nearly 170 unique issues because of his work, and its maintainer Daniel Stenberg published a[blog](https://daniel.haxx.se/blog/2025/10/10/a-new-breed-of-analyzers/) about how the experience changed his mind about AI-powered vulnerability reports.


Recently, Anthropic used its Mythos-powered vulnerability scanner Glasswing to take another look at curl. The net result according to Daniel Stenberg[was just 1 new low severity vulnerability](https://daniel.haxx.se/blog/2026/05/11/mythos-finds-a-curl-vulnerability/) .


This is hopeful news for those of us who have been worried what Mythos' release will mean for appsec and vulnerability management… while the model is undoubtedly impressive, existing products are already delivering comparable results. This is not to minimize the challenges maintainers face keeping up with the the torrent of vulnerability reports they've been dealing with – they are real and serious – but the world has not ended so far, and datapoints like curl suggest that the Mythos alone is unlikely to make the problem orders of magnitude worse, except potentially by encouraging more people to scan their code using modern SAST tools.


This is not to say that Mythos won't be impressive… from what we've seen, it likely does deliver a substantial incremental bump in raw vuln-finding capability… but when it comes to discovering flaws reliably and exhaustively at scale, the harness around the model is really a bigger part of the story than the model itself.


We touched on this in a[n earlier post](https://zeropath.com/blog/benchmarking-opus-4-6-vuln-detection) where we put Opus 4.6 through its paces detecting real CVEs in single C functions using a fairly naive single shot strategy mirroring what you might do in a coding agent or chat bot: showing it the sample and asking it if there were any vulns in it. The model found around 28.5% of the vulns in the dataset – impressive since every one of these made it past human review and into production – but it got these numbers with a massive false positive rate, and extremely unstable and inconsistent results run over run.


At ZeroPath, we're intimately familiar with these sort of quirks because of the work we've done to build a complex harness around commodity LLMs to mitigate them, in order to produce results that are:


- Stable run over run
- Low in false positives
- Exhaustive and auditible (it's the vulns from your entire codebase not a random selection)


When Mythos becomes public, we intend to try integrating it into our stack. When we do, it won't be surprising if it improves performance… but the curl comparison highlights that the model alone is the wrong thing to focus on. ZeroPath used 6 month old models in a harness we've spent years perfecting to achieve the same results as Mythos in a more naive configuration.


The "vulnpocalpyse" is already here. Current frontier models with strong harnesses are already leading to tens of thousands of vulnerability reports. Keeping up with this torrent has been stretching open source maintainers, but the world hasn't ended. Mythos' release doesn't change that, it just calls attention to something that's already happening.
