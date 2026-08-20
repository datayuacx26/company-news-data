---
schema_version: "1.0.0"
document_id: "8ea57e4c7f391dda09211926e5e3d2b90c89b636ad5927c5c4a2bccd40d26915"
company_key: "backblaze-inc-class-a-common-stock"
company: "Backblaze Inc."
source_id: "backblaze-inc-class-a-common-stock-rss-a06767c1ff83"
canonical_url: "https://www.backblaze.com/blog/ai-can-now-execute-73-of-expert-level-cyberattacks-heres-what-it-leaders-need-to-do-now/"
published_at: "2026-07-07T16:37:14+00:00"
first_seen_at: "2026-07-20T23:18:53.525659+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:7b5ce856e9e19e761a762f290792cbf3094b098addd7e31e1c65e2636913529c"
---

# AI Can Now Execute 73% of Expert-Level Cyberattacks. Here’s What IT Leaders Need to Do.

On April 7, 2026, Anthropic announced a model so capable they refused to release it publicly.[Claude Mythos](https://www.anthropic.com/claude/mythos) , their most advanced frontier AI, was deemed too dangerous for open access because of one thing: it can hack.


Not in the vague, theoretical sense that security researchers worry about. In the “[completed 73% of expert-level capture-the-flag challenges that no AI could touch before April 2025](https://www.aisi.gov.uk/blog/our-evaluation-of-claude-mythos-previews-cyber-capabilities) ” sense. In the “[autonomously identified a 17-year-old remote code execution vulnerability in FreeBSD from scratch, without a human involved after the initial prompt](https://www.anthropic.com/research/mythos-preview) ” sense. In the “[found thousands of critical zero-days across every major operating system and web browser in a matter of weeks](https://www.anthropic.com/glasswing) ” sense.


Anthropic locked Claude Mythos behind Project Glasswing, a vetted partner program initially restricted to roughly 50 organizations—AWS, Microsoft, Google, Apple, Cisco, CrowdStrike, and others—to use the model for defensive work before adversaries could develop equivalent capability. By June, that program had expanded to more than 200 organizations across 15 countries, including operators of power grids, water systems, hospitals, and telecommunications infrastructure.


Then, on June 9, Anthropic released Fable 5—the first public version of a Mythos-class model—equipped with safeguards that reroute higher-risk queries to less-capable models. The same day, it released Claude Mythos 5 directly to vetted Glasswing partners. Later in June, after a brief US government export review, the Commerce Department confirmed that “appropriate safeguards are in place” and permitted Anthropic to redeploy Mythos 5 to trusted cyber defenders.


But here’s the part that should be on every IT leader’s radar: Anthropic itself now projects that other AI companies will have Mythos-class models within six to 12 months, and those companies may not ship with equivalent safeguards.


GPT-5.5, released three weeks later, didn’t wait. OpenAI shipped it with expanded cybersecurity capabilities and its own controlled-access program—also designed for defense, also eventually available to people with different intentions.


The AI arms race in cybersecurity isn’t coming. It’s here.


## **Ransomware 5.0 Doesn’t Need a Skilled Operator**


For most of its history, ransomware required a human being at the keyboard: someone doing reconnaissance, identifying targets, crafting phishing lures, moving laterally through a network. Skilled attackers commanded significant ransoms. Amateur operators made rookie mistakes.


That dynamic is collapsing.


Ransomware now appears in 48% of all breach chains, according to the[Verizon 2026 Data Breach Investigations Report](https://www.verizon.com/business/resources/Td15/reports/2026-dbir-data-breach-investigations-report.pdf) —up from 44% the year prior. Active ransomware groups jumped 49% year over year. Over 250 new operators entered the market in just the last six months, many of them low-skill actors using generative AI to craft personalized phishing campaigns 60% faster than was possible before. AI-assisted lateral movement was present in over 65% of recent cases.


The Verizon 2026 DBIR also marks a shift in how attackers get in the door: for the first time, exploiting unpatched software vulnerabilities has overtaken stolen credentials as the number one initial access vector, now responsible for 31% of breaches. That’s not a coincidence in a world where AI can scan codebases for exploitable flaws at machine speed.


[IBM’s 2026 X-Force Threat Index](https://www.ibm.com/reports/threat-intelligence) confirmed that “collapsing barriers to entry” are letting even low-volume operators run campaigns that overwhelm defenders. The average cost of a data breach in the US hit $10.22 million—an all-time record.


[Trend Micro’s 2026 security predictions](http://trendmicro.com/vinfo/us/security/research-and-analysis/predictions/the-ai-fication-of-cyberthreats-trend-micro-security-predictions-for-2026) describe what they call “Ransomware 5.0”: a model where AI handles reconnaissance, vulnerability scanning, lateral movement, and even ransom negotiation autonomously, without a human operator directing any of it.


If you’re still designing your security posture around slowing down a skilled human attacker, you’re fighting the last war.


## **The Thing Nobody Wants to Say Out Loud**


**Here’s where I’m going to say something a little uncomfortable: the cybersecurity industry has been selling you detection for years when what you actually needed was recovery.**


Detection is important. Don’t get me wrong. But detection-centric security assumes you catch the attack before it fully executes. In an era where AI compresses the attack timeline, exploit chains run at machine speed, and hundreds of new ransomware groups just showed up with AI-powered toolkits, detection alone isn’t a resilience strategy. It’s a bet.


The UK Government’s AI Security Institute tested Claude Mythos extensively and confirmed it cannot reliably execute attacks against organizations with well-hardened defenses. That’s genuinely good news. But it raises an obvious follow-up question: how many organizations actually have well-hardened defenses? A 2025 report found that over 45% of discovered security vulnerabilities in large organizations go unpatched after 12 months. Many critical infrastructure operators still run end-of-life software.


The honest answer is: most organizations are not that hardened. And even the ones that are will face a more capable threat next year than they face today.


This is why[immutable backups](https://www.backblaze.com/cloud-storage/solutions/object-lock) aren’t just a box to check; they’re the safeguard that functions even when everything else fails. If an attacker encrypts your production environment before detection fires, the question isn’t “how did that happen?” It’s “how fast can you recover?”


## **What Claude Mythos Actually Changes (And What It Doesn’t)**


It’s worth separating signal from noise here, because the coverage of Claude Mythos has ranged from measured to apocalyptic.


**What Mythos changes:** the technical barrier for sophisticated attacks. Vulnerabilities that previously required elite researchers to discover and weaponize can now be found and chained faster. Anthropic’s own red team found that Mythos could identify and exploit a previously unknown FreeBSD remote code execution vulnerability—fully autonomously, no human involved after the initial prompt. Across all Project Glasswing partners, Mythos has now surfaced more than 10,000 high- or critical-severity security flaws in production codebases. That means the window between vulnerability disclosure and active exploitation, already dangerously short, gets shorter. It also means less-skilled threat actors get access to capabilities that used to require significant expertise.


**What Mythos doesn’t change:** the fundamental anatomy of a ransomware attack. Attackers still need initial access. The Verizon 2026 DBIR confirms they’re still relying on unpatched software, stolen credentials, and phishing as entry points just finding and exploiting them faster. Once inside, they still need to move laterally, identify high-value data, and execute the encryption sequence. The Centre for Emerging Technology and Security at the Alan Turing Institute made this point clearly: more sophisticated ransomware attacks that rely on stolen credentials, social engineering, or already-compromised accounts are “far less likely to be affected” by Mythos-class models on either side.


That matters for how you defend. Hardening access controls, enforcing MFA, patching aggressively, segmenting your environment, and maintaining clean, immutable backups are not glamorous. They are not AI-powered. But they address the attack anatomy that AI tools, offensive or defensive, haven’t fundamentally changed.


## **The Recovery Imperative**


Strengthening cyber fundamentals, in practice, means one thing above all else: knowing that when something gets through, you can recover without paying a ransom.


That requires three things from your[backup and recovery posture](https://www.backblaze.com/cloud-storage/landing/ad/ransomware) :


1. **Immutability.** Backups that can’t be encrypted or deleted by ransomware, even by a compromised admin credential. This isn’t optional anymore. If your backups live in the same environment as your production data and share the same access credentials, they aren’t backups; they’re part of your blast radius.[Backblaze B2](https://www.backblaze.com/cloud-storage/solutions/backup-and-archive) Object Lock is S3-compatible, so if your team is already running Veeam, Commvault, MSP360, or Nutanix, you’re not replacing your backup stack. You’re giving it an immutable target that ransomware can’t touch.
2. **Air-gap or off-site isolation.** Object Lock, WORM storage, and geographically separate backup targets all put meaningful distance between your recovery point and an active attack. When AI tools can chain dozens of steps in a corporate network attack simulation autonomously, “isolated backups” means genuinely isolated, not just a separate folder. Version history matters here too: the ability to roll back to a known pre-attack state, not just the most recent snapshot, is what separates a clean recovery from discovering your restore point was already compromised.
3. **Recovery time that matches the threat.** AI-accelerated attacks mean recovery has to be fast. A backup strategy built around 72-hour RTOs made sense in a different threat environment. In 2026, breach costs approaching $10.22 million in the US, the question your leadership should be asking is: how long does it actually take us to restore from a clean state? Cold storage tiers that require hours of retrieval before a restore can even begin are a liability when the clock is running.[Backblaze B2](https://www.backblaze.com/cloud-storage/solutions/backup-and-archive) is hot storage: your data is available immediately after detection, with no retrieval queue to wait on.


## **A Practical Checklist for IT Leaders Right Now**


The Claude Mythos announcement, the Fable 5 public release, and GPT-5.5’s expanded cybersecurity capabilities are a forcing function. Not because Mythos-class capability is in attackers’ hands today, but because the direction of travel is confirmed, the timeline is compressed, and the question is no longer whether equivalent offensive tools will proliferate, only when.


A few things worth doing before that happens:


- **Audit your backup environment’s blast radius.** Can ransomware that has compromised your production environment also reach your backups? If yes, fix that first.
- **Test your recovery time.** Not just that backups exist, but how long an actual restore takes from your most recent clean snapshot. If you don’t know the number, you don’t have a recovery plan. You have a filing system. Backblaze gives you[3x your stored data in free egress each month](https://www.backblaze.com/cloud-storage/pricing) , which removes the cost barrier that causes most teams to skip DR testing entirely. Run the restore. Know the number.
- **Pressure-test your identity controls.** Credential abuse and phishing remain the dominant entry vectors. MFA, compromised credential monitoring, and least-privilege access aren’t new ideas, but they’re still the fastest path to closing the doors AI-powered attacks walk through.
- **Patch faster.** The Verizon 2026 DBIR found exploited vulnerabilities are now the leading breach entry point. The median time organizations take to fix a known flaw is 55 days. AI-assisted attackers don’t wait 55 days.
- **Layer your defenses, but anchor to recovery.** Perimeter protection, endpoint detection, vulnerability scanning: these all matter. But they’re all designed to catch something before it executes. Immutable backups are what you rely on when something executes anyway.
- **Revisit your RTO and RPO against today’s breach costs.** The math has changed. A $10.22 million average US breach cost changes the calculus on what it’s worth spending on faster, more resilient recovery infrastructure.


## **The Last Thing**


Anthropic made a decision that deserves credit: they looked at what Claude Mythos could do and chose not to hand it to the world on day one. Project Glasswing is a serious attempt to use the model’s capabilities on the right side of this fight, and the coordinated disclosure of thousands of vulnerabilities to the organizations responsible for patching them is meaningful defensive work.


But the history of powerful technology is not “we invented it and kept it safe.” It’s “we invented it, others reproduced it, and everyone had to adapt.” The 6-to-12-month window for equivalent capability to reach adversarial hands isn’t fearmongering; it’s Anthropic’s own forecast. Other AI companies are building toward the same capability threshold right now, and not all of them will ship with the same safeguards.


The organizations that come through this transition will be the ones that took recovery seriously before they needed it. Not because detection failed, but because recovery is the one safeguard that works regardless of what the attacker is running.


Backblaze B2 with Object Lock puts immutable, air-gapped backup storage within reach of organizations that can’t afford hyperscaler pricing (which, as it turns out, is most of them).[Start a free trial](https://www.backblaze.com/) or[talk to our team](https://www.backblaze.com/contact-sales/) about building a ransomware-resilient backup architecture before the threat landscape shifts again.
