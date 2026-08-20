---
schema_version: "1.0.0"
document_id: "3223a54c3e2e4fe561ae925a2274fe4d7b382f180c43cb3e8d65b91adf78e075"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-karpathy-anthropic-openbsd-npm-supply-chain"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T22:13:03.870884+00:00"
content_hash: "sha256:a6fbe560b48964955f4531effa0f13558410f42bcb660a56ddd2558f15c16d3a"
---

# Cosmic Rundown: Karpathy Joins Anthropic, OpenBSD 7.9, npm Supply Chain Attack

## Andrej Karpathy Joins Anthropic


The former OpenAI co-founder and Tesla AI leader[announced he's joining Anthropic](https://twitter.com/karpathy/status/2056753169888334312) . Karpathy built Tesla's Autopilot team and later returned to OpenAI before departing in 2024. His move to Anthropic signals continued talent consolidation among frontier AI labs.


This follows[Anthropic's acquisition of Stainless](https://www.anthropic.com/news/anthropic-acquires-stainless) announced yesterday. Stainless builds SDK generation tools that power client libraries for OpenAI, Cloudflare, and others. Anthropic is clearly investing in developer infrastructure alongside model capabilities.


## OpenBSD 7.9 Released


The[OpenBSD 7.9 release](https://www.openbsd.org/79.html) shipped with the usual focus on security hardening and clean code. OpenBSD continues to prioritize correctness over features, making it a reference implementation for security-conscious deployments.


For teams running critical infrastructure, OpenBSD's approach to minimal attack surface and proactive auditing remains relevant. The release includes updated drivers, improved hardware support, and continued refinement of the pledge and unveil sandboxing mechanisms.


## Mini Shai-Hulud Strikes Again: 314 npm Packages Compromised


A[supply chain attack compromised 314 npm packages](https://safedep.io/mini-shai-hulud-strikes-again-314-npm-packages-compromised/) , continuing a pattern of credential theft and malicious code injection in the JavaScript ecosystem. The attack vector exploited maintainer account takeovers to inject data exfiltration code.


This is the second major npm supply chain incident in recent months. Teams should audit their dependency trees, enable npm's audit features, and consider lockfile pinning. The attack reinforces why dependency management remains a security-critical practice.


## CISA Admin Leaked AWS GovCloud Keys on GitHub


Brian Krebs[reported](https://krebsonsecurity.com/2026/05/cisa-admin-leaked-aws-govcloud-keys-on-github/) that a CISA administrator accidentally committed AWS GovCloud credentials to a public GitHub repository. The keys were exposed for several hours before being rotated.


Secret scanning tools exist specifically to prevent this. GitHub's push protection, git-secrets, and pre-commit hooks can catch credentials before they reach remote repositories. The incident is a reminder that even security-focused organizations need automated guardrails.


## Simon Willison's LLM Recap


Simon Willison published[The last six months in LLMs in five minutes](https://simonwillison.net/2026/May/19/5-minute-llms/) , a rapid-fire summary of developments since late 2025. The post covers context window expansions, reasoning model improvements, and the proliferation of local inference options.


Willison's recaps are useful for teams trying to track which capabilities have actually shipped versus which remain speculative. The piece notes that tool use and structured output have become reliable enough for production workflows.


## Cursor Ships Composer 2.5


[Cursor's Composer 2.5 update](https://cursor.com/blog/composer-2-5) adds improved multi-file editing and better context management. Composer is Cursor's agent-style feature that can make changes across multiple files in a codebase.


The update reflects broader trends in AI coding tools: better context windows enable longer operations, but orchestration and file management remain the bottleneck. Cursor continues to iterate faster than most IDE-integrated AI tools.


## Gentoo Discloses Kernel Vulnerabilities


Gentoo published advisories for[Copy Fail, Dirty Frag, and Fragnesia kernel vulnerabilities](https://www.gentoo.org/news/2026/05/19/copy-fail-fragnesia-vulnerabilities.html) . These memory safety issues affect various kernel subsystems and require patching.


Linux kernel security continues to be a moving target. Teams running custom kernels or delayed update cycles should review their exposure.


## Quick Hits


**Virtual OS Museum** : Someone[built an interactive museum](https://virtualosmuseum.org/) with nearly every operating system you can think of. A useful reference for understanding how computing interfaces evolved.


**PhotoGIMP** : A[patch for GIMP 3](https://github.com/Diolinux/PhotoGIMP) that reorganizes the interface to match Photoshop conventions. Useful for teams migrating from Adobe tools.


**AI Radio Stations** : A team[let AIs run radio stations](https://andonlabs.com/blog/andon-fm) as an experiment in autonomous content generation. The results are uneven but demonstrate what's possible with current audio generation.


---


That's the Monday rundown. The Karpathy move and Stainless acquisition suggest Anthropic is building for a longer horizon than just model releases. Meanwhile, the npm compromise is another reminder that supply chain security deserves more attention than most teams give it.


**Building with a headless CMS?** Cosmic's[AI agents](https://www.cosmicjs.com/ai/agents) can help automate content workflows while you focus on shipping.[Start free](https://app.cosmicjs.com/signup) .
