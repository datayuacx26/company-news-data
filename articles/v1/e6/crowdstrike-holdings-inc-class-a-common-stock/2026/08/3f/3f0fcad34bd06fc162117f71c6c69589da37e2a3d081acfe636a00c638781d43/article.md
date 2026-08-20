---
schema_version: "1.0.0"
document_id: "3f0fcad34bd06fc162117f71c6c69589da37e2a3d081acfe636a00c638781d43"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/expanding-cybersecurity-ai-benchmarks-beyond-vulnerability-discovery/"
published_at: null
first_seen_at: "2026-08-07T02:27:51.641607+00:00"
fetched_at: "2026-08-07T02:27:52.643680+00:00"
content_hash: "sha256:f58342068cbe600b5f840f5caae131d7cff395ea653e99df3fe8bb97a9df7de5"
---

# Expanding AI Benchmarks in Cybersecurity Beyond Vulnerability Discovery

The conversation about AI in cybersecurity has recently centered on capabilities like vulnerability discovery, exploit generation, and automated proof-of-concept development. It’s easy to see why: These tasks produce binary outcomes; a vulnerability either exists or it doesn't. That makes them useful for measuring model progress and demonstrating increasingly sophisticated cybersecurity capabilities.


Vulnerability discovery matters to defenders. According to the[Verizon 2026 Data Breach Investigations Report](https://www.verizon.com/business/resources/reports/dbir/#top-takeaways) , vulnerability exploitation is now the most common initial access vector, accounting for 31% of breaches in the reporting dataset. This is a meaningful and growing share of the problem and a strong reason to continue advancing AI capabilities in this area.


But it also means 69% of breaches begin through other paths. Credential abuse, phishing, social engineering, trusted relationships, and other forms of access remain central to the adversary playbook. A comprehensive evaluation framework should therefore measure not only whether AI can discover and exploit vulnerabilities, but also whether it can help defenders detect identity abuse, investigate suspicious activity, engineer effective detections, hunt for adversaries, and respond across the broader attack lifecycle.


We believe effective AI for defense must be evaluated against the operational reality of security teams: the range of techniques adversaries use to gain initial access, the work required across the kill chain, and defenders’ most time-consuming tasks. Here, we explore some of these use cases.


## Detecting Adversary Behavior After Initial Access


Once an adversary is inside, the defender’s work becomes more complex. Security teams must detect and triage suspicious activity across massive alert volumes, balancing signal and noise. Speed here determines whether the adversary is contained in minutes or operates on a network for a longer period of time.


When suspicious activity is found, the focus shifts to investigation, which requires significant effort and expertise. Analysts must reconstruct events across endpoints, identities, cloud environments, and other systems to determine what happened and how far the adversary has progressed. This is where attackers gain time: The longer an investigation takes, the greater the opportunity to move laterally, escalate privileges, and achieve their objectives.


Detection engineering powers these investigations by translating adversary behavior into durable detections, yet it remains a specialized and often under-resourced discipline. Effective defense also requires proactive threat hunting to find adversaries operating below existing detection thresholds. This depends on skilled analysts and accurate models of real-world behavior.


None of these disciplines produce clean binary outcomes or are easily benchmarked. However, they all depend on grounded, accurate models of real adversary behavior that public benchmarks lack.


## Evaluating AI Against Adversary Tradecraft


Proactive defense requires knowing exactly how adversaries operate: the tools they use, the sequence of actions they take, and the ways their activity appears in telemetry. Adversary emulation translates that knowledge into realistic activity, making it foundational to detection engineering, threat hunting, and security validation.


The critical factor here is being grounded in reality. AI red teaming based on hypothetical scenarios or synthetic data has limited value if it does not reflect the techniques, artifacts, and operational patterns observed in real intrusions.


A key question for AI is whether it can reproduce adversary behavior with enough fidelity to test detection coverage, expose engineering gaps, and improve defensive readiness. For most defenders, that is a harder and more consequential test of AI capability than vulnerability discovery alone.


Figure 1. ALTERED SPIDER profile from the[CrowdStrike Adversary Universe](https://www.crowdstrike.com/en-us/adversaries/)


Because CrowdStrike observes trillions events each day across a global customer base, our threat intelligence is sourced from real intrusions rather than synthetic scenarios. This operational visibility is the foundation for our adversary emulation, detection engineering, and AI evaluation based on how attackers operate.


Frontier labs can build highly capable models, but they lack the same access to operational threat intelligence generated across thousands of customer environments. This is the difference between a model that scores well on a public benchmark and one that is genuinely useful in a specific environment. Real telemetry closes that gap by showing which adversaries matter, how their behavior appears, and whether an AI system can help defenders detect and respond to it.


## Where Public AI Security Benchmarks Fall Short


Public benchmarks for AI security evaluation suffer from several compounding limitations. One is agenda capture: Benchmarks often reflect the research priorities of those who create them, rather than the operational effectiveness security teams need to achieve.


A second problem is saturation. As top-tier models begin clustering near the ceiling of public benchmarks, the tests become less useful for distinguishing meaningful differences in capability. When every leading model scores close to 100%, the benchmark becomes little more than a threshold test.


Contamination creates an additional challenge. Once benchmark content enters the broader data ecosystem, models can be trained directly or indirectly toward the test. Over time, a benchmark intended to measure general capability may instead measure how effectively a model has been optimized for a familiar evaluation.


The deeper fundamental issue is that public benchmarks do not evaluate whether AI can detect real adversary behavior in real telemetry, generate detection logic for specific lateral movement techniques, or improve the operational work defenders perform every day. Closing that gap requires more than better public benchmarks alone. It requires high-fidelity telemetry, real-world threat intelligence, and the operational context needed to evaluate AI against how adversaries behave.


## What Meaningful Security Evaluations Require


Evaluation frameworks designed for defenders must meet several requirements.


First, they must be task-relevant to the functions where security teams spend their time, including triage, investigation, remediation, and threat hunting.


Second, they must be grounded in telemetry. Evaluating AI against real adversary behavior in realistic environments produces results that are more predictive of operational performance than tests built around synthetic attack scenarios.


Finally, they need to be customer-specific. A financial institution faces a different threat profile than a healthcare provider or a critical infrastructure operator, and the benchmarks that matter for each are not the same. A single public leaderboard cannot serve all of them. The right framework must be adapted to the specific adversaries and techniques most relevant to a specific customer's environment, and account for the less common techniques and behaviors where advanced adversaries often operate.


CrowdStrike’s goal is to create an entirely different measurement framework that begins with the defender's operational reality rather than producing a better version of what is easiest to score today.


## Final Thoughts


AI for vulnerability discovery will continue to advance and generate significant attention. But for security teams, the more consequential question is whether AI can help detect, investigate, and disrupt adversaries after they gain access, regardless of the initial access mechanism.


Answering that question requires evaluation grounded in real threat intelligence, real telemetry, and the operational reality of defense. With visibility across a global security ecosystem and deep experience in how adversaries operate, CrowdStrike is positioned to define how AI should be measured against the outcomes defenders actually need.


At[Fal.Con 2026](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/) , we will show how our work is moving from principle to practice.


#### Additional Resources


- *[Download our guide](https://www.crowdstrike.com/en-us/resources/white-papers/five-steps-for-frontier-ai-security-readiness/) to explore the five steps for frontier AI security readiness.*
- *Visit our[Frontier AI Readiness and Resilience Service](https://www.crowdstrike.com/en-us/services/ai-security-services/frontier-ai-readiness-and-resilience/) page to learn about CrowdStrike’s approach to frontier AI and see how CrowdStrike Services can help.*
- *[Join us](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) at Fal.Con 2026 as we bring together cyber leaders from across the industry to help secure the AI revolution.*
