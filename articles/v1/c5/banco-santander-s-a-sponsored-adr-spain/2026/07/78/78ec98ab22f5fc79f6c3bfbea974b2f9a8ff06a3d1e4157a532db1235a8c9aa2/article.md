---
schema_version: "1.0.0"
document_id: "78ec98ab22f5fc79f6c3bfbea974b2f9a8ff06a3d1e4157a532db1235a8c9aa2"
company_key: "banco-santander-s-a-sponsored-adr-spain"
company: "Banco Santander S.A. Sponsored ADR (Spain)"
source_id: "banco-santander-s-a-sponsored-adr-spain-rss-14b90327b079"
canonical_url: "https://www.santander.com/en/stories/when-speed-meets-quality-how-we-make-ai-products-measurable-at-santander"
published_at: "2026-07-30T12:30:00+00:00"
first_seen_at: "2026-07-30T13:52:37.352460+00:00"
fetched_at: "2026-07-30T13:52:38.135997+00:00"
content_hash: "sha256:e512910573a59364f7f9c854ef470d9cd53457f657bca4453341b312072180de"
---

# When speed meets quality: How we make AI products measurable at Santander

[Communications Santander](https://www.santander.com/en/press-room/topics-of-interest)


CEST


[Linkedin](https://www.linkedin.com/company/banco-santander)


[Instagram](https://www.instagram.com/santander.global/)


[X](https://x.com/bancosantander)


Artificial intelligence (AI) is changing how software is built. A working assistant, agent or first version of a feature can now be produced in hours rather than weeks. As Santander expands the use of AI across software engineering, customer service, operations and risk, that speed creates a new challenge: proving that AI products are reliable, safe and fair, and that they stay that way as they evolve.


The process, and the challenge, began with a Responsible AI question. Before putting a conversational product in front of customers, teams need to know that the system would respect its guardrails, would treat customers fairly, and would not answer in a way that was biased, unsafe or simply wrong. Those reviews grew out of Santander's Responsible AI practice, where teams were already stress-testing conversational and agent-based systems for the risks ordinary testing tends to miss: biased or inconsistent responses, unsafe answers, and manipulation that only appears after several turns. The demand was so consistent that the manual review had to become something bigger: a bank-wide capability that checks those Responsible AI concerns automatically, and keeps checking them across a product's whole life rather than once before release.


"We started from a Responsible AI question — is this system safe and fair, and how would we know? — and realised the honest answer had to be measurable," says Andrés Herranz, tech lead of this initiative within the Data & AI Science team. Herranz explains that "we built one common way to evaluate AI products across performance, behaviour, safety and fairness. The point isn't to pass a test before launch. It's to keep proving, over the whole life of a product, that it still does what it should."


##


Integrated need


This is the idea at the centre of the work: Responsible AI is not a separate checklist bolted onto quality, it is part of what quality means. A product that shows bias, blocks a legitimate request, or can be talked into the wrong answer is not a high-quality product, however fast or fluent it looks. So, safety and fairness are scored in the same place as accuracy and performance, in a single view of quality. If a product falls short on Responsible AI, it cannot be considered good.


The need for this comes from the nature of AI itself. Traditional software is deterministic: the same input gives the same result unless the code changes. Products built on large language models (LLMs) and agents behave differently. Their outputs can depend on wording, context, the sources they retrieve, the orchestration around them and updates to the underlying models. A product can work well at launch and then drift because a model was updated or a data source changed.


This is why evaluation is treated as part of development rather than a gate at the end of it. Regression testing still matters, but confirming a system has not got worse is not the same as showing it is good. Building AI products is increasingly a loop — generate an output, assess it, improve it, evaluate again — and the framework is built for that loop.


It starts from the product itself. Rather than applying a generic test set, it reads what the system is meant to do and generates targeted evaluation cases from its own requirements and policies. These evaluations include its Responsible AI obligations, so fairness, safety and guardrail compliance are tested with the same rigour as functional correctness. It then probes behaviour beyond those cases, to find weaknesses no one thought to anticipate, while a curated set of expected behaviours gives a stable reference point so regressions show up when prompts, models or data change. The same logic runs before and after deployment: an end-to-end quality check ahead of release, and continuous monitoring in production for drift, degradation and emerging vulnerabilities — one consistent definition of quality, Responsible AI included, across a product's life.


Underneath the technology, the ambition is cultural. Engineers should be able to move fast while understanding the impact of each change. Product teams should prioritise evidence. And Responsible AI and governance teams should rely on the same metrics as the people shipping the product — so safety and fairness are measured in the same breath as performance, not bolted on afterwards.


For the customer, none of this is visible, and that is the point. People do not ask for ‘Responsible AI’; they ask for a product that answers correctly, holds a normal conversation, and treats them fairly. Good quality disappears into a good experience — and so does good Responsible AI. When it works, no one notices it; they simply trust the product. As AI becomes a larger part of how the bank operates, that trust cannot be something we assume is there. It has to be observable, measurable and continuously improved. That is what this capability is for: not simply better testing, but a more responsible, more reliable way of building and scaling AI products.
