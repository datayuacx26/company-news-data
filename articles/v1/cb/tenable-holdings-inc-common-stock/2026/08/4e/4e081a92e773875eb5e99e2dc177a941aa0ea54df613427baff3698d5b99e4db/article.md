---
schema_version: "1.0.0"
document_id: "4e081a92e773875eb5e99e2dc177a941aa0ea54df613427baff3698d5b99e4db"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/ai-code-security-with-claude-mythos-preview-inside-tenables-500-hours-of-testing-for-project"
published_at: "2026-08-06T12:00:00+00:00"
first_seen_at: "2026-08-06T19:55:35.430114+00:00"
fetched_at: "2026-08-06T19:55:36.412096+00:00"
content_hash: "sha256:b98820dc69351fbeab1de7a3c0c62a1a99501fb96c011305256d4267a5e2cf16"
---

# AI code security with Claude Mythos Preview: Inside Tenable’s 500+ hours of testing for Project Glasswing

We spent 500+ hours and 40 billion tokens testing Anthropic’s Claude Mythos Preview for Project Glasswing. The takeaway: frontier AI won't run your code security program, but used well, it can make one even stronger.


## **Key takeaways**


1. **Frontier AI dramatically scales security testing.** In one month, Tenable dedicated 11 security experts and more than 40 billion tokens testing Claude Mythos Preview across source code analysis, exploit creation, binary reverse engineering, threat modeling, and dynamic testing.


2. **Human expertise turns frontier AI findings into real risk reduction.** More findings don't automatically mean more risk. Mythos Preview surfaced a high volume of findings, but only a fraction proved to be true exposures once Tenable experts determined their reachability, exploitability, and whether existing controls already mitigated them.


3. **Source code access is the defender's asymmetric advantage.** Frontier AI is far more powerful when it can read the full source, and that visibility is something defenders have and outside attackers don't. Securing code repositories is more important than ever.


Ever since Anthropic introduced[Claude Mythos Preview](https://www.tenable.com/blog/claude-mythos-prepare-for-AI-cybersecurity-questions-from-your-board-of-directors) , the security community has been buzzing with equal amounts of excitement, anxiety, and healthy skepticism.


After well over[500 hours of rigorous testing](https://www.tenable.com/blog/testing-claude-mythos-preview-for-code-security-tenable) at the hands of Tenable security analysts, engineers, and researchers, we’re excited to share our assessment, even as we proceed with testing Claude Mythos 5 (more to come on that).


Here’s what Tenable learned from[our month in the trenches with Mythos Preview](https://www.tenable.com/blog/testing-claude-mythos-preview-for-code-security-tenable) , and what it means for your organization’s security posture. Our headline finding: used well, frontier AI earns a real place in a modern code security program. It won't run the program on its own, but paired with the right harness and expert oversight, it makes a strong program measurably stronger.


## How Tenable is testing Claude Mythos Preview


It’s important to clarify that we are not using[Claude Mythos Preview](https://www.tenable.com/cybersecurity-guide/principles/claude-mythos-preview) in any Tenable products. The same restriction applies to all[Project Glasswing](https://www.tenable.com/blog/anthropic-claude-mythos-tenable-joins-project-glasswing) participants: The model may be evaluated for research purposes, but it cannot be incorporated into commercial products.


**We evaluated the source code scanning, exploit creation, binary reverse engineering, threat model creation, and dynamic testing capabilities of Claude Mythos Preview.** **Tenable dedicated a team of experienced security engineers for the testing along with white-box source code analysis and a purpose-built testing harness (the set of prompts and tools that constrain and orchestrate a model across a multi-step task).**


Before evaluating Claude Mythos Preview, we built an independent testing harness. Our testing showed that the real power comes not from the model alone, but from the combination of the model, a purpose-built harness, rich context, and expert human oversight.


## Where Mythos and frontier AI fit alongside SAST, DAST, and SCA


To understand where a frontier model like Claude Mythos Preview fits into your software security posture, it helps to compare Mythos directly to your existing stack of deterministic tools, including static application security testing (SAST), dynamic application security testing (DAST), and software composition analysis (SCA).


Deterministic tools rely on hard-coded rules and produce the same exact output for any given input. Non-deterministic tools, like frontier AI models and generative AI systems, use statistical probabilities to predict outcomes. Consequently, non-deterministic systems can yield different results across identical inputs.


The bottom line: Traditional, deterministic code security tools remain the core of an effective, audit- and compliance-ready program. **Frontier AI models like Claude Mythos Preview provide additional capability for the creative, variance-tolerant layer where humans continue to review findings.** In other words, it’s a powerful new arrow in the quiver with the potential to make your whole security arsenal more effective.


**Table 1: Traditional security scanning tools vs. Claude Mythos Preview**


**Feature** **Traditional tools (SAST, DAST, SCA)** **Frontier AI models**


**Consistency** **Deterministic** : Produces identical, highly repeatable results every run. Perfect for enterprises with rigorous compliance requirements. **Non-deterministic** : Yielded as much as a 30% run-to-run variance in finding counts and severity rankings during Tenable’s testing.


**Operational cost** **Low** : Fast, cheap, and easily integrated into continuous[CI/CD pipelines](https://www.tenable.com/cybersecurity-guide/learn/ci-cd-workflow-integration) . **High** : Expensive to validate raw, noisy outputs; best used for periodic testing.


**Strength** Catching known vulnerabilities, patch-diffing, and maintaining compliance baselines. Creative, exploratory “offensive” testing, threat modeling, and contextual code analysis.


## Match the tool to the task: determinism for audit, frontier AI for exploration


Frontier AI models, including Claude Mythos Preview, are fundamentally non-deterministic. If you run the exact same target through the exact same model on different days, the model will yield different results.


In Tenable’s testing, we observed Mythos outputting different issue counts *and* different severity rankings, despite using the exact same prompt. That variance is an asset for creative, exploratory work with an expert reviewing output, but it's the wrong fit as the backbone of a consistent, repeatable, audit-grade security or compliance program. For this reason, deterministic tooling remains core, though it will need to adapt to AI and integrate it in a meaningful way.


That adaptation isn't free. Before you fold a frontier model into a security program, three constraints shape where and how it earns its place:


1. **The cost** - The economics only work when you're deliberate about where you deploy it. The tokens required to run the model as a continuous scanning tool would be cost-prohibitive — in our estimation, costs could reach $500,000 per full-time employee (FTE) per year, based on an estimated one-month token usage of roughly $41,700 per FTE.
2. **The noise** - In Tenable tests, model outputs (the findings) were noisy and required heavy validation. Consequently, if you’re going to use Claude Mythos Preview for source code scanning, you’ll want to feed the model’s high-quality findings into your existing static analysis and gating workflows. Without a validation pipeline working alongside the model, you’ll get buried in noise.
3. **The results** - Claude Mythos Preview produced different outputs even when our inputs were the same. This kind of inconsistent model behavior breaks pipelines that need to run predictably at scale. What’s more, compliance requirements demand reproducible, defensible evidence. A model that produces different results each run won’t stand up to auditors.


## The real costs of code security with frontier AI go beyond tokens


When calculating the total cost of ownership (TCO) for frontier AI as code security tool, many organizations make the mistake of focusing exclusively on API and token costs.


While token costs can quickly add up, they still represent just one factor in the TCO equation. Because frontier models generate a high volume of noisy findings, your TCO model should include the cost to validate the findings, which includes the cost of the experienced security engineers required to handle the manual validation.


**TCO is why frontier AI works best as a targeted, high-value layer rather than a day-to-day security scanning engine.** Run it against everything continuously and the costs quickly become prohibitive, quickly. Consider that just one of our security analysts used more than 10 billion tokens in a month of rigorous testing, which cost $41,700. Multiply that by 12 months, and you’ve got a nearly $500,000 annual run rate for just that one person.


## Frontier AI changes the scale of discovery, not the nature of source code flaws


Some organizations are bracing for a new class of “Mythos-level” bugs. Based on our experience with Mythos, that’s the wrong worry. The model finds the same kinds of source code flaws a pen tester would, such as broken logic and memory corruption; it just finds far more of them, at much greater speed, and if appropriately prompted, can create the exploit. **Claude Mythos hasn’t uncovered a new class of vulnerabilities; it’s amplified the speed and volume of findings.**


It’s important to remember that a potential finding is not the same as a validated exposure. A finding signals a *potential* issue. AI surfaces a large volume of findings, but[security teams require context](https://www.tenable.com/products/tenable-one) about potential flaws in the source code — whether they’re reachable, exploitable, and the extent to which any existing security controls mitigate them — to validate and remediate the findings that do indeed create true exposure.


> In a world of AI-driven vulnerability discovery, the context, triage, validation, and prioritization work become much more important. Once you subtract what your existing tools already catch, what others have reported, and what isn’t actually reachable, the list worth acting on gets shorter. Your capacity to validate and fix is the constraint now, not discovery.


## Source code access gives defenders a major advantage


The immense power of frontier models in code security relies on “white-box” source code analysis. Source code access gives internal security teams the ultimate asymmetric advantage against threat actors. The deep source code visibility that supercharges an internal defender’s automated pipeline is exactly what an outside, black-box attacker lacks.


Consequently, protecting your source code repository has never been more vital. If an attacker gains access to your source code, they gain the keys to run their own highly optimized AI discovery engines against you.


## The bottom line: frontier AI shifts your edge from finding flaws to proving what matters


Tenable testing confirmed Claude Mythos Preview can function as a highly capable pen-testing tool.


When combined with a purpose-built harness and the two things only a human defender has — source code access and the ability to validate findings live — Claude Mythos Preview can deliver powerful results. Give it a harness and an expert to drive it, and it becomes a genuine force multiplier; leave Mythos to run on its own and the output tends to be noisy, with many of the findings never making it to validation. Either way, frontier AI shifts the hard work from discovery to verification, prioritization, and remediation.


We continue to evaluate frontier AI models, including Mythos 5, to identify where they’ll serve customers best, and we look forward to sharing our ongoing findings in upcoming blogs.


## Learn more


- [Inside Tenable’s first 30 days with Claude Mythos Preview](https://www.tenable.com/blog/testing-claude-mythos-preview-for-code-security-tenable)
- [What is Claude Mythos Preview?](https://www.tenable.com/cybersecurity-guide/principles/claude-mythos-preview)
- [Claude Mythos: Prepare for your board’s cybersecurity questions about the latest model from Anthropic](https://www.tenable.com/blog/claude-mythos-prepare-for-AI-cybersecurity-questions-from-your-board-of-directors)
- [Tenable joins Anthropic’s Project Glasswing to advance AI-era cyber defense](https://www.tenable.com/blog/anthropic-claude-mythos-tenable-joins-project-glasswing)
