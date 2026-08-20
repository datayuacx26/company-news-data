---
schema_version: "1.0.0"
document_id: "f4eb585025620b90156eb9be01ce36cc4c960ad31310e4f19b4bb13fcce9e6c3"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/escape-joins-anthropics-cyber-verification-program/"
published_at: "2026-08-04T12:41:28+00:00"
first_seen_at: "2026-08-04T14:07:07.847578+00:00"
fetched_at: "2026-08-04T15:13:53.558824+00:00"
content_hash: "sha256:aaad14f6a3cf744dfd2d6f4524e0cc0326b3800fab03139a2613460de2a662fb"
---

# Escape joins Anthropic’s Cyber Verification Program to advance AI-powered offensive security

Good news doesn't arrive alone. This month we joined not one frontier AI program but two! Yesterday we shared that[Escape joined OpenAI's Trusted Access for Cyber](https://escape.tech/blog/escape-joins-openai-trusted-access-for-cyber-tac/) . Today we're a verified member of Anthropic's[Cyber Verification Program](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet) .


Both programs solve the same problem. The most useful security research is dual-use by nature. Ask a model to find a broken authorization check and prove it's exploitable, and you've just described an attack. It's the same one a defender runs to close the gap first. Frontier models are trained to hesitate there, and they should.


Anthropic draws the line in two places.


First, **prohibited use** covers activity that's almost always malicious with little to no legitimate defensive application, for example, mass data exfiltration, ransomware code development. These are blocked by default, with no route to adjustment.


Second, **high-risk dual-use** covers activity that has real defensive value, like vulnerability exploitation and offensive security tooling development. That's also blocked by default, but verified defenders can apply to adjust it for legitimate use cases through the Cyber Verification Program.


That second line is the one we live on. Offensive security tooling development isn't an edge case for Escape. It's the whole platform. With the exponential rise of AI capabilities, we want to empower security teams to discover vulnerabilities before the attackers do and help defenders focus on, well, defending their org. The program is what lets a verified team point frontier models at exactly that work, in the open.


*"Attackers already use AI to find and chain vulnerabilities faster than any human team can review them* ," says Antoine Carossio, CTO and fo-founder of Escape. " *Defending against that can't be point-in-time, it takes offensive security that runs continuously and reasons through an attack the way a real adversary would. Anthropic's Cyber Verification Program gives us verified access to those frontier capabilities for our research work at Escape, which is exactly how a small security team keeps pace with everything their engineering org ships* ."


The access is for our internal research and engineering teams, and it feeds Cascade, our[AI-powered pentesting product](https://escape.tech/product/ai-pentesting) . Cascade is a harness. It decides how a set of agents explores an application, what they carry between steps, and how a finding gets proven. We route that work across frontier and open-weight models and pick the best one for each sub-task, so no single provider powers the product end to end. Anthropic's models are strong where it counts for us, in long-horizon reasoning and agentic exploration. Stronger models underneath, and the harness reaches even further.


What comes out the other side stays concrete. Every finding ships with a working exploit and remediation code. Take the[Keycloak authorization flaw our research team disclosed last month, now CVE-2026-17059](https://escape.tech/blog/escape-research-pii-disclosure-keycloak-cve-2026-17059/) . We found it by asking every user-returning endpoint the same question and comparing the answers. A finding an engineer trusts is a finding that gets fixed.


Our mission hasn't moved. A five-person team should be able to defend a thousand-engineer org. Every verified defender with this kind of access makes the software all of us rely on a little harder to break. We're glad to be one of them, and what we find keeps going out in public.
