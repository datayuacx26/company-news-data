---
schema_version: "1.0.0"
document_id: "67216307fbff08326e1c8c59a76ea51fb515a27853ab5c5d3136abd1002f67d4"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/introducing-auto-discovery-and-learning"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-24T03:05:55.735939+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:52f9f03f0801c0facd8e237df0bf4afad755aec113e266cd154f0cd143cfe256"
---

# Corgea Auto-Discovery and Learning

Yesterday we launched[Corgea Skill Scanning](https://corgea.com/blog/introducing-corgea-skill-scanning) , catching threats before your agents implement them. Today: what happens when code does exist, but your scanner treats it like it’s never seen it before.


Most security scanners have amnesia. Every scan starts from zero. They don’t know you use Django ORM everywhere. They don’t know your API gateway blocks SQL injection patterns. They don’t remember that three developers marked the same finding as a false positive last week.


Corgea does.


Today we’re launching two features that make Corgea dramatically more intelligent: **Auto-Discovery** and **Learning** .


## Auto-Discovery: Know your codebase before you scan it


When a project is first connected to Corgea, the platform doesn’t immediately start scanning. It studies first.


**1. Automatic discovery.** Corgea triggers a discovery pass that extracts the code and auto-detects frameworks, languages, and architecture: Django, Express, Flask, React, Spring Boot, and dozens more. It builds a map of what you’re actually running.


**2. Pattern reconnaissance.** An AI agent reads the actual codebase to discover the security controls your developers already built: authentication decorators, ORM usage, input validators, WAF configurations, middleware chains, sanitization routines. It doesn’t guess. It finds the real patterns in your files.


**3. Validation.** Discovered patterns are evaluated by an LLM to confirm they are legitimate, project-specific protections, not dead code, not comments, not test stubs. Low-quality findings are filtered out.


**4. Policy generation.** Validated patterns are converted into false-positive and fix policies tailored to your project. These are stored as corgea.yaml-compatible rules.


**5. Automatic application.** From the very next scan onward, these policies guide the security engine. When Corgea sees a potential SQL injection but your codebase uses Django ORM everywhere, it knows that’s a false positive. When it sees missing auth on an endpoint that has your custom` @api_key_required


`


decorator, it understands the protection is already there.


The result: your first scan is already tuned to your architecture. Not a generic ruleset that treats Django and raw PHP the same way.


## Learning: Your developers’ tribal knowledge becomes policy


Auto-Discovery handles the static picture. Learning handles what happens over time.


Every time a developer marks a vulnerability as a false positive, or explains why a fix is wrong, Corgea records it. But unlike tools that just log feedback and forget it, Corgea turns it into institutional knowledge.


**1. Capture.** Developers flag false positives in the UI or via PR comments, leaving reasoning like “Sanitized by Django’s ORM” or “Blocked by our API gateway.” Votes and comments are tracked per issue.


**2. Aggregate.** A daily job groups related feedback by project, CWE, and language. An LLM deduplicates similar comments and scores confidence based on how many developers reported the same pattern.


**3. Validate.** For high-confidence clusters, an agent reads the actual codebase to verify the developer’s claim. It hunts for the specific middleware, sanitization logic, or framework protections that justify the false positive. It never assumes a vote is true just because it got upvoted.


**4. Recommend.** The agent proposes a project-specific false-positive policy, written in plain English with concrete file references and code examples. No black-box tuning.


**5. Review and apply.** Proposed policies land in a Learnings tab for admin approval. Once approved, they are automatically injected into future scans, reducing noise for that exact project without affecting others.


The result: Corgea gets quieter and more accurate the more your team uses it. Your developers’ tribal knowledge becomes repeatable, verifiable policy, not lost in Slack threads.


## Why adaptive beats generic


Traditional SAST tools have one configuration: on or off. They produce the same results for a Django monolith, a microservices Go shop, and a legacy PHP application. The noise is the product.


Corgea’s approach is different. Auto-Discovery means the platform studies your actual defenses before making claims. Learning means it improves with every developer interaction. Together, they create a scanner that adapts to you, not the other way around.


## What this unlocks


- **Faster developer onboarding.** New team members see relevant findings, not a firehose.
- **Lower security team burnout.** Less triage. Fewer dismissed findings.
- **Better audit compliance.** Every policy decision is tracked and attributed.
- **Continuous improvement.** The platform gets better every week without any config changes.
