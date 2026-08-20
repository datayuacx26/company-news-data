---
schema_version: "1.0.0"
document_id: "4db46e99041c764fd4e3142f456e3ef64c31788784a0b7766723cfa0bf1c4e0d"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/aptos-labs-rust-ai-application-security"
published_at: "2026-03-05T00:00:00+00:00"
first_seen_at: "2026-07-24T06:50:52.708520+00:00"
fetched_at: "2026-07-28T22:18:58.238043+00:00"
content_hash: "sha256:761215781a2eed13b9d43c9c949ff8de4a7dcf9daee9ff44c212b0f4687a82e8"
---

# How Aptos Labs Scales Application Security Across 1M+ Lines of Rust with AI-Powered SAST

## Aptos Labs: Layer 1 Blockchain for the Real World


[Aptos Labs](https://aptoslabs.com/) builds a next-generation layer 1 blockchain designed for real-world usage. The platform is fast, inexpensive, reliable, and capable of supporting enterprise-grade applications. The team owns the entire stack: validators, SDKs, execution environment, and supporting infrastructure. That breadth creates both high development velocity and significant security surface area.


---


## The Challenge: Keeping Up with Engineering Velocity


Andrea and his security team at Aptos Labs protect everything from infrastructure code and application logic to cloud environments, endpoints, and smart contracts. Engineering teams ship code constantly, and the security team simply cannot manually review every change.


Before evaluating AI-powered static analysis tools, the team had largely avoided commercial SAST platforms. Every product they tested failed in the same ways: rigid rule systems that could not express subtle bugs in blockchain logic, too much noise, insufficient support for Rust, and high maintenance overhead for rule sets that still missed critical issues.


- Engineering teams pushing code faster than security can review
- Rust codebases with over a million lines of code
- Traditional static analysis tools unable to detect complex business logic vulnerabilities
- No existing automated security testing tool flexible enough to justify the investment


Andrea describes what the team needed as an "automation baseline": something that automates foundational detection work so the security team can spend their time on the difficult, high-value tasks that require human creativity. The goal was to shift left on security without slowing developers down. As he put it, "Security changes constantly. You need a platform that lets you put energy in the right place."


---


## Why Aptos Labs Chose ZeroPath


Aptos discovered ZeroPath through an internal comparison shared by a CISO friend. Andrea was running evaluations against four or five tools at once, mixing large enterprise vendors like Semgrep, Snyk, and Aikido with smaller AI-native players.


During testing, ZeroPath surfaced a subtle replay-related issue in a vendor library. Andrea was surprised because this was not something a simple Semgrep rule could catch, especially given how the library's behavior was documented. He described the finding as looking "like something a person spent real time discovering."


What kept the team engaged beyond that initial finding was the combination of detection quality and tuning flexibility. Andrea prefers a tool that surfaces more findings, even with some noise, as long as he can tune it down. "It's my job to figure out what is good and what is not. I just want a really good tool that has the capacity to throw me the good balls." ZeroPath's enterprise direction and willingness to build deep integrations specific to the Aptos stack, including a custom[Wiz integration](https://docs.zeropath.com/integrations/wiz) , was a major factor in the decision.


---


## The Solution: AI-Powered SAST in the Aptos Security Workflow


### How Andrea's team hunts for variants at scale


ZeroPath runs continuous static analysis on Aptos's most critical codebases, including a Rust repository with more than one million lines of code. When the security team discovers an issue manually, they convert it into a[reusable ZeroPath rule](https://zeropath.com/products/policy-engine) . The team has already created fifteen custom rules, many targeting application-specific logic like replay protection and transaction validation edge cases. Andrea describes the workflow as: "We can express an issue once and ZeroPath goes hunting for variants across the codebase."


### Developers fix issues before merge


ZeroPath provides real-time findings directly inside[GitHub pull requests](https://zeropath.com/products/pr-reviews) . Developers see security issues in the context of their work and resolve them before merging, without opening a separate platform or waiting for a security review. This shift-left approach means the security team no longer needs to manually review every PR, and the Slack back-and-forth that used to dominate the workflow has dropped significantly. Andrea noted that AI coding bots also pick up ZeroPath's PR comments, adding additional context that developers find useful. "Developers live in GitHub. ZeroPath meets them there."


### Three-click triage


When Andrea reviews a finding in the ZeroPath platform, distribution takes three clicks: read through it, validate, and send to Linear. Developers pick up from there. Time spent per finding dropped from roughly an hour to about twenty minutes, and the distribution step itself is nearly instantaneous.


### From first call to scanning production code in under two days


Many Enterprise SAST tools take months to fully integrate and get production-ready. With ZeroPath, Aptos Labs was able to get production-ready in under two days, including configuration of custom integrations. Andrea emphasized that ZeroPath's hands-on support during onboarding was a real differentiator, especially for a team with demanding requirements like[Wiz integration](https://docs.zeropath.com/integrations/wiz) . "No one else would have integrated with Wiz the way ZeroPath is doing. They really care about the customer and what they're doing."


---


## The Results: Saving 20+ Hours Per Week


With ZeroPath deployed across Aptos's critical codebases, Andrea's team now operates at a fundamentally different scale. For new code, issues are caught in the pull request before they ever reach the main branch.


Issues caught earlier in the development lifecycle


For new code, vulnerabilities are caught at PR time before merge. Existing issues are surfaced quickly across critical repositories.


20-32 hours saved per week


ZeroPath saves 5-8 hours per person per week across the security team. Time per finding dropped from ~60 minutes to ~20 minutes, with distribution taking three clicks.


Security is no longer the bottleneck


Automated PR scanning lets developers ask questions and fix issues immediately. The security team can support a large engineering organization without becoming a bottleneck.


15 custom rules for application-specific logic


The team converts manually discovered issues into reusable rules that hunt for variants across the entire codebase, covering patterns no off-the-shelf scanner can express.


Enterprise-scale Rust scanned without compromising depth


ZeroPath easily handles enterprise-scale codebases, running deep AI-powered analysis across Aptos's largest Rust repositories.


When asked what would happen if ZeroPath disappeared, Andrea was direct: "We would have to divide our attention much more between finding and remediating vulnerabilities, which would negatively impact productivity." And from the budget perspective: "Incidents cost us more than buying a solution that prevents them."


---


## About ZeroPath


ZeroPath is one of the first AI-native application security platforms that detects, explains, and helps fix real vulnerabilities, including business logic bugs, with precision and developer-friendly workflows. Leading engineering teams use ZeroPath to scale application security coverage without slowing development or increasing headcount. ZeroPath combines SAST, SCA, secrets scanning, and IaC scanning in a single platform.


---


## Frequently asked questions


How long does it take to set up ZeroPath?


At Aptos Labs, the security team went from first call to scanning production code in under two days, including configuration of custom integrations like[Wiz](https://docs.zeropath.com/integrations/wiz) . Most teams are scanning within five minutes of connecting their GitHub. ZeroPath's onboarding includes hands-on support from the team, and a 24x7 dedicated Slack channel stays active long after initial setup is complete.


Can ZeroPath detect business logic vulnerabilities in blockchain software?


Yes. Business logic bugs in blockchain software like replay attacks, transaction validation bypasses, improper authorization flows look like normal, syntactically correct code. There is no regex that catches "this validation is missing replay protection." ZeroPath's AI-powered analysis understands code intent, not just patterns, which is why it surfaced a subtle replay-related vulnerability at Aptos Labs that rule-based scanners like Semgrep, Checkmarx, and Snyk missed. The team now converts these findings into[custom rules](https://zeropath.com/products/policy-engine) that hunt for variants across the codebase.


Does ZeroPath support Rust static analysis?


Yes. ZeroPath handles enterprise Rust codebases at scale. Along with Rust, ZeroPath supports many[legacy languages and frameworks](https://zeropath.com/docs/scanning/sast-overview) which many enterprise SAST tools cannot.


How much does ZeroPath cost?


ZeroPath starts at $1,000/month base plus $60/developer/month on the Team plan; book a demo to see it on your own codebase. ZeroPath counts only developers actively using the platform, not everyone who has ever committed to a repository. Enterprise pricing with volume discounts, on-premises deployment, and custom compliance reports is available on request. As Andrea at Aptos Labs put it: "Incidents cost us more than buying a solution that prevents them."
