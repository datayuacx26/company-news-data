---
schema_version: "1.0.0"
document_id: "9e8e5e4ab499c816c2adb83200a5b8aedd7793d950780f288eb1969b6ca6054c"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/escape-joins-openai-trusted-access-for-cyber-tac/"
published_at: "2026-08-03T11:24:22+00:00"
first_seen_at: "2026-08-03T11:56:12.312504+00:00"
fetched_at: "2026-08-03T13:03:34.932163+00:00"
content_hash: "sha256:8d8edc8883aeb0f7132658f1e3297d9a0f37d5a67407b921038e66ce570e0d28"
---

# Escape joins OpenAI’s Trusted Access for Cyber (TAC) to advance AI-powered offensive security

We've been approved for OpenAI's Trusted Access for Cyber preview.


For years we've been building toward one idea: offensive security that runs continuously inside engineering, instead of arriving twice a year as a PDF nobody reads past the executive summary.[Business-logic DAST first](https://escape.tech/product/dast) , then application-layer attack surface management, then[Cascade, our AI pentesting agent.](https://escape.tech/product/ai-pentesting) Three products on one offensive security engineering platform, because a list of assets, a stream of findings and a pentest report are only worth much together.


Trusted Access for Cyber is the next piece. Our engineering and security teams now have verified access to OpenAI's models with less resistance on the dual-use security work that would normally trip a refusal.


All of it goes toward one number: the time between a vulnerability existing and your team holding both a working exploit and the fix. That window is where an adversary does their work. Today it runs to months. It should be hours.


Offensive security has become an AI workload. We've been building for that from the start, and TAC opens frontier models to the defenders doing the work and help turn insights into action faster.


## Reducing the time between discovery and remediation


Attackers use AI for discovery. Endpoints, business logic flaws, and the chains that turn two small gaps into one real breach. That work starts to get parallelized cheaply and rewards volume.


Defenders have mostly pointed AI at protection instead: threat modeling, alert triage, incident summaries, detection engineering. Meanwhile most scanners haven't kept up with frontier AI. So you get two clocks running at different speeds, and the distance between them is the number worth managing. Luckily, programs like OpenAI’s TAC give verified defenders a trusted route to apply frontier AI to cyber defense.


Time to proof is the distance between a logic-level vulnerability existing in your application and your team holding a working exploit and a fix for it. Not a severity score. We want to provide actual proof an engineer can reproduce.


For most organizations that distance runs to months, because it's set by the pentest calendar, the bug bounty queue, and whatever a scanner happens to cover, rather than by how long the vulnerability actually takes to find. In our[published benchmark](https://escape.tech/blog/modern-ai-powered-pentesting-tools-in-depth-benchmark/) on two live open-source applications, Cascade set up in minutes and reported in hours, at a 0% false-positive rate across both blackbox and[whitebox](https://escape.tech/blog/whitebox-pentesting-is-now-available-in-escapes-ai-pentesting-cascade/) runs. On Photoview it went from 12 validated findings blackbox to 28 with the repository attached.


Last week our research team disclosed[a PII disclosure in Keycloak, now CVE-2026-17059](https://escape.tech/blog/escape-research-pii-disclosure-keycloak-cve-2026-17059/) . The platform found it by asking every user-returning endpoint the same authorization question and comparing the answers, which is the kind of gap a code review reads straight past. Red Hat published the CVE six days after we reported it. Keycloak shipped the one-line fix four days later.


Every finding came with a working exploit and remediation code attached. That's the part that matters, because a finding an engineer doesn't trust is a finding that doesn't get fixed, and an unfixed finding reduces nobody's exposure.


## What it changes for us


The access is for our internal security research and engineering teams. What it improves is the research behind Cascade, and everything downstream that helps teams remediate exploitable vulnerabilities.


Cascade is a harness: it decides how a set of agents explores an application, what it carries between steps, which hypothesis is worth chasing, and how a finding gets proven. Escape routes work across a portfolio of frontier AI models and open-weight models. No single provider or model powers the product end-to-end. We treat models as interchangeable components and select the best one for each sub-task based on continuous internal benchmarks. **The harness is what we build, and with a stronger set of models underneath it reaches further.**


OpenAI makes the same observation in their announcement, that " *sophisticated harnesses elicit stronger and stronger capabilities* " with existing models. The two compound.


Our mission hasn't changed. Five people should be able to defend a thousand-person engineering org. Escape velocity was never about going faster, it's the point where you stop falling back, and programs like this bring our mission closer to its goal.


## What this means for security teams


A couple of things worth looking into:


**Annual pentest cadence is getting harder to defend.** Pentesting was annual or quaterly because expert hours were scarce and required large budgets. AI changed those economics. If your attack surface changes weekly and your testing is yearly, the delta between those two numbers is your actual risk position. **Our goal is to be helping customers manage risk as the threat landscape speeds up.** That takes visibility you can trust across everything you expose, findings proven well enough that engineers act on them without arguing, and remediation that doesn't stall when the next critical vulnerability lands.


**Verified access is becoming part of vendor due diligence.** Model providers are moving toward tiers based on proven identity and use case. Which frontier AI capabilities your security vendors can legitimately reach is now a fair question, alongside SOC 2 and data residency.


Discovery has been compounding on the attacker's side for a while now. What programs like OpenAI’s Trusted Access for Cyber do is let it compound on the defender's side too, in the open.


That's worth more than any single model release. Every verified defender who gets this kind of access makes the software all of us depend on a little harder to break. We're glad to be one of them, and what we find will keep going out in public.


##
