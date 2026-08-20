---
schema_version: "1.0.0"
document_id: "d2257982f14040a55ac6a88d4fa19fff29a9c76f2ac348a1605c945b7c275a3f"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/why-does-the-rise-of-agentic-ai-force-security-teams-to-become-research-aware/"
published_at: "2026-07-24T12:00:26+00:00"
first_seen_at: "2026-07-24T12:59:15.834950+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:cc8b6f3348a925e8e301cd666cd91e1f6ee5f691c8176e11d94e1537d38b06e8"
---

# Why does the rise of Agentic AI force security teams to become research-aware?

##


> Bruce Schneier : "security is a process, not a product".


It’s not enough to audit your application once, every new feature introduces new attack surfaces that must be taken into account when evaluating the security of the application as a whole. For example, a company rolling out a new Agentic AI process without adequate guardrails might expose themselves to MCP tool poisoning, an emerging class of attacks where malicious actors leverage the trust placed in a tool metadata to steer the state towards an exploitable state.


Given the incredible speed at which AI models and the surrounding ecosystem keep improving, such emerging attack vectors have become the norm, not the exception, as illustrated by the rate at which the[OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) **** and the surrounding[OWASP GenAI Security Project](https://genai.owasp.org/) are updated.


## AI and Pentest are “long games of whack-a-mole”


Since the real world never stops moving on, static benchmarks will inevitably drift away from the reality they are supposed to represent and become irrelevant. Even worse, as a benchmark stops evolving, it becomes training data for the next generation of models; this guarantees the benchmark score will improve, it does not guarantee that the system capability has improved in step. This can lead to **silent regressions, where the dashboards show ever-improving results while the underlying system has become compromised.**


A few weeks ago we shipped a new model in our pentesting pipeline. Benchmark scores went up, which is what we expected. However, our monitoring metrics also showed a significant, though benign-looking, uptick in accesses to GitHub.


To try and assess the relevance of this uptick, one of our researchers performed a detailed inspection of the logs (the importance of which we’ll discuss at length), and found one of the hardest failure modes to spot: the new and improved **model had become good enough to learn to cheat the benchmark,** and was using diffs to figure out where vulnerabilities were injected.


Cheating Agents are not new, and specification gaming has been studied for years. We had guardrails written for against the versions we knew about. None of them covered this one, and nothing in our monitoring was built to notice. Every metric we had said the system was improving. On 21 July,[OpenAI disclosed the same failure mode at a scale](https://openai.com/index/hugging-face-model-evaluation-security-incident/) that made the evening news. Two of its models, run against a public cybersecurity benchmark called ExploitGym, broke out of their test sandbox, reached the internet, and pulled the benchmark's solutions out of Hugging Face's production database. OpenAI's own account is that the models "went to extreme lengths to achieve a rather narrow testing goal." Hugging Face had detected the intrusion around 14 July and reported it to law enforcement, before either company knew what it was.


In both cases, a model optimized the measurement instead of the task, and in both cases the benchmark kept returning a number the whole time. This is why maintaining Agentic Pentesting systems and the[benchmarks to monitor](https://escape.tech/blog/modern-ai-powered-pentesting-tools-in-depth-benchmark/) requires the constant attention of a team of both Cybersecurity and AI experts. Such profiles are scarce, and few Application or Offensive Security teams manage enough surface area to justify internalizing the operational costs of such a team.


## Why does this require a research practice, not just more devs


Despite its inherent advantages in the short-term, the “build fast, fix later” mindset is not a good strategy for solving hard, long-horizon problems, since poorly planned experiments can yield ZERO info:


> Fisher: "To consult the statistician after an experiment is finished is often merely to ask him to conduct a post mortem examination. He can perhaps say what the experiment died of"


What it means is that the research team needs to assist during the specification part, if the project is already running it’s too late to properly instrument it.


[Grokking (OpenAI, 2022):](https://arxiv.org/abs/2201.02177) researchers observed “delayed generalization” where the model started performing correctly weeks after all monitoring metrics had stopped evolving (think of it as the AI equivalent of launching a cracking effort on a single password that could last years, with no guarantee of success). If those actions were initiated without prior reflection, they might still succeed through pure luck, but even more likely leave you no closer to your goal than you started from, but with significantly less budget left.


The role of a research practice is to leverage domain expertise to carefully weigh the risks and opportunities of looking for Grokking, before the production team takes over and actually runs the experiment. In Cybersecurity, that means delegating exploration (hunting for unknown threats) and exploitation (patching and monitoring known ones) to the teams that excel at each part.


Beyond individual tastes and abilities, the uncertainty inherent to research requires failures to be quick and cheap, often requiring product features to be temporarily removed to isolate possible sources of errors. Those requirements are incompatible with the ones of a live product : you cannot find a leak without cutting the water, but you cannot cut the water to an occupied building.


Despite their shared goal of improving the product, the two teams in practice need to operate on separate projects, sharing only the most relevant information to **avoid scope creep on either side.**


## The necessity and challenges of telemetry


As suggested by Fisher, the first step in designing a research program is to define what will be measured during the individual experiments, how it will be measured, and perhaps even more importantly **why it is being measured** : just because you can measure something does not mean you should optimize it, especially since as the adage of Goodhart’s Law says "when a measure becomes a target, it ceases to be a good measure".


This is not to say that one should trust their systems blindly: just because you did not check your uptime does not mean the site wasn’t down.


This leads to a compromise: log everything to make sure you don’t miss the important details, but don’t act on it unless absolutely necessary. Unfortunately, the SIEM bill always comes, and that’s when compromises become unavoidable : meaningless details have to be dropped in order to condense the relevant information. This is not a hypothetical scenario, it is a practical reality across all domains:[CERN builds custom silicon](https://www.theregister.com/offbeat/2026/03/22/cern-eggheads-burn-ai-into-silicon-to-stem-data-deluge/5219932) *to decide what's worth keeping out of 1PB/s of raw data; every SOC needs to triage thousands of daily alerts to decide which ones are worth investigating.*


> Borges’ “ *empire where cartography becomes so exact that only a map on the same scale as the empire itself will suffice”.*


It is imperative to identify which elements (cities, mountains, etc…) are actually relevant to the kind of map being built (a topographic map for flood prevention and roadmap for transport optimization have very different requirements).


Finding this essential complexity requires something the statistician cannot bring to the table: domain knowledge. This is where the expertise built from years of pentesting experience is necessary, to help the research team figure out what questions they want to answer and therefore what the research program will look like.


## If brute-force is not an option, what’s the right way to approach this?


In the previous cartography analogy, it was assumed cartographers had already built the map, but building it is not an easy task either.


*The difference between a brute-force strategy and a well-orchestrated multi-agent system, which both need to visit the exact same nodes, **is whether the system remembers which paths it already ruled out** . That's the difference between random fuzzing and a deliberate, methodical, coverage-guided pentest leveraging prior knowledge to build the most efficient path to a desired goal without wasting effort on doing the same thing over and over again.*


Even without wasting time retreading old ground, it is not always possible to explore all nodes due to budget constraints. In that case, **choosing which road to prioritize during the building of the map, or exploration, (be it of physical land or of a more abstract scientific landscape) is essential.**


This is precisely what the researchers behind “ *Decoupling Reconnaissance and Exploitation: Measuring the Capability Boundaries of LLM-Based Web Penetration Testing* "[(arXiv, 2026](https://arxiv.org/abs/2606.25332) ), investigated.


**Decoupling Reconnaissance and Exploitation**


They identified two main failure modes for Agentic Pentesting:


- either incorrect exploration, where an attack vector is never considered,
- or incorrect exploitation, where the vector has been considered but not successfully exploited.


Those two sources of error require completely different fixes, and without operating this decomposition the practitioner would have no way to decide where to invest its limited resources to optimize the final result.


In fact, the authors show that, across models and harnesses, agents reach 90% exploitation success with accurate context, but only ~50% recall on autonomous reconnaissance. Measuring those success rates independently allowed the researchers to determine that efforts should be spent improving exploration performance first, rather than improving the part of the system that already worked almost perfectly.


## Conclusion


While the task of building the perfect map of the land is daunting, there is one saving grace for the would-be traveling salesman: an up-to-date, high-quality map maintained by one group of cartographers can be used by all travelers to improve their journey.


So, when if you’re evaluating[agentic pentesting tools](https://escape.tech/blog/best-agentic-pentesting-tools/) this year, ask yourself whether a vendor has a true research practice or is riding on the hype train.


Dedication to the research process **means being your own harshest critic** , never settling for "good enough" while passively waiting for others to update your audit tools, and always building a deeper understanding of your own systems rather than placing blind trust in a promising and rapidly shifting technology.


All this work, hidden in the background, is the reason why when you hop on the call with us, you'll get the best possible understanding of what we provide and why we know we are doing the right thing.


If your Security team would rather spend its efforts on remediation than rebuilding this knowledge, we're here to help!


[Book a demo](https://escape.tech/book-a-demo)
