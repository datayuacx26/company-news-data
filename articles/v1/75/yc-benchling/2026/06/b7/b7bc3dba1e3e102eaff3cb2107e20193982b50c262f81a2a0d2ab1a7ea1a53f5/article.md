---
schema_version: "1.0.0"
document_id: "b7bc3dba1e3e102eaff3cb2107e20193982b50c262f81a2a0d2ab1a7ea1a53f5"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/introducing-chief-information-security-officer"
published_at: "2026-06-23T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:58c5fa0061deedd3ac4d6c73091ef02e9c08b77d3aa00ccd4f5d848c9331661c"
---

# How Benchling's new Chief Information Security Officer is thinking about security in the age of AI agents

*Chad Kalmes joined Benchling as CISO and CIO earlier this year. Most recently the CISO and CIO at Udemy, he's led AI-era security and technology transformations at Socure, Twilio, and PagerDuty. Chad is also an active investor in the next generation of security infrastructure companies.*


*Chad is not a biotech native, and in his first few months here, he has been asking questions that many insiders stopped asking. This conversation covers what breaks in security models when AI agents replace humans as the primary actors, why the research data layer is becoming the most contested real estate in science, and why containment has become the wrong mental model.*


**You've recommended**[The Coming Wave](https://the-coming-wave.com/) **as required reading. Mustafa Suleyman (cofounder of DeepMind and current CEO of Microsoft AI) argues that AI, synthetic biology, and robotics are converging faster than our governance frameworks can handle, and he puts a lot of weight on "containment." Does that framing hold up when you're actually building infrastructure?**


I think he is right about the convergence, but “containment" is the wrong word for what we actually need to do.


Containment traditionally implies a perimeter — some boundary inside which the dangerous thing is held. That mental model doesn't survive contact with AI, because the useful and the dangerous are woven into the same data, the same models, the same workflows. You can't contain the biosecurity risk of an AI reasoning over sequence data without also containing the science you're trying to accelerate. The abstraction breaks.


What works instead is contextual authorization with continuous visibility. The question stops being "can this agent touch this data" and becomes "under what conditions, for what purpose, with what reasoning chain, and with what human checkpoints?" Those conditions have to be encoded in the infrastructure — in the permissions model, the data lineage, the behavioral telemetry — not in a policy document nobody reads.


This is the part of the problem that made Benchling the right place to work on it. You can't retrofit contextual control into a system that wasn't designed for it. Structured, governed, traceable research data has to be in the foundation.


**You've spent 25 years in SaaS, fintech, and edtech. What's your honest first read on biotech as a security environment — where is it more mature than you expected, and where is it behind?**


Honestly, the thing that surprised me most is that life sciences has been managing the dual-use problem longer than most AI governance frameworks have existed. Biosafety levels, gain-of-function moratoriums, the DURC framework — these aren't theoretical constructs. They're hard-won institutional responses to a specific insight: that knowledge capable of enabling the most consequential breakthroughs is also, by definition, capable of enabling the most consequential harms. Biotech built governance infrastructure around that reality decades ago. That's the part of the maturity picture most outsiders miss.


Where I see the gap is in how the security function itself is positioned. A lot of programs are still wired primarily for regulatory compliance defensibility, not adversarial defense. Those are related problems, but they're not the same problem. An auditor and a bad actor are looking for very different things, and the controls that satisfy one don't necessarily stop the other.


Part of what's going on is historical. The tools life sciences built its data practices around — lab notebooks, siloed instruments, ELN workflows — were designed for the physical bench. They brought real structure and traceability to complex science, and they served an important purpose. But modern collaboration in biotech demands a different approach to data, and the compliance-centric posture that grew up around those tools is inheriting assumptions that don't match the current threat reality.


The other challenge is structural. Security in life sciences has historically reported to IT or compliance, which made sense when the main risk was regulatory exposure. That's shifting. Research data is now some of the most valuable data on the planet, and the industry is — rightly — starting to treat security as a strategic function rather than a support one. That transition is happening in real time, and it's further complicated by the fact that the data is increasingly AI-accessible. That blurs any notion of a "perimeter" and requires new risk models that most existing tooling wasn't designed for.


**What specifically breaks in the traditional security model when AI agents, not humans, are the actors?**


Almost every assumption traditional access control was built on needs to be reconsidered when AI agents are the actors.


RBAC, session management, anomaly detection — all of it assumes a human making decisions in real time, with bounded behavior, inferable intent, observable fatigue. Agents break those assumptions at once. The session boundary dissolves: an agent can persist context across days of autonomous operation, executing thousands of actions under a single authorization. You also have intent inversion: with a human you infer intent from behavior, but with an agent the behavior *is* the intent — and the reasoning that produced it is largely invisible to the controls we have today.


And the one I think is most underappreciated: agents are prompt-injectable at every surface they read. Documents, web pages, API responses, scientific literature — any of those can carry adversarial instructions that redirect the agent in ways no authorization model anticipated. Your perfectly-scoped permissions don't help if the agent has been told, by a sentence buried in a PDF it ingested, to do something its operator never asked for.


*So the question we used to ask was "who is doing this, and are they allowed to?" The question we now have to ask is closer to "what is being reasoned, over what data, toward what goal, and would I have stopped it if I'd seen the whole chain?"*


That's behavioral provenance, not access logging. It's a different category of problem.


It's also why I spend time as an investor in the companies building natively for this. The prior generation of security tools was built for a world where humans were the primary actors in every workflow. Those tools did their job. But when your primary actor is an agent executing thousands of operations autonomously, taking inputs from untrusted surfaces, the defensive requirements change. The companies I find most compelling aren't adding AI to an existing category — they looked at what security actually needs to do in five years and built toward that.


**Research data is some of the most competitively sensitive IP in the world. How does that change the threat model versus financial or identity data?**


Think about the asymmetry between a credit card breach and a research data breach. If an attacker steals card numbers, the harm can be bounded. You reissue, you notify, you absorb the fraud. Painful, but recoverable. If an attacker steals your lead compound data six months before you file, the harm may never be quantified, and it's permanent.


*You can't un-exfiltrate a protein structure.*


That irreversibility changes the math. Most enterprise security is built on an assumption that some breaches will happen and the job is fast detection and containment. That's still true, but for a certain tier of research data the balance has to shift much harder toward prevention, because a single successful exfiltration can represent years of work and hundreds of millions in capital. You plan for detection. You invest disproportionately in making sure certain categories of data never leave.


Two other dimensions are largely absent in other sectors. The regulatory framing around dual-use research of concern — DURC, biosafety frameworks, gain-of-function governance — signals that life sciences has actually been grappling with this problem longer than most. Those frameworks carry a lesson worth applying: the answer was never containment, it was principled access with structured accountability. The same logic applies to AI operating on this data. The other is the adversary profile. The top of the threat pyramid here isn't a financially motivated cybercriminal — it's a nation-state actor with multi-year patience, supply chain capability (e.g. the ability to compromise a vendor or partner and use that access as a way in), and very specific scientific objectives. You build differently for that adversary than for a ransomware crew.


**Every biopharma is wrestling with how to actually capture value from AI — not just adopt the tools, but rewire how the company works. You're leading a lot of that transformation at Benchling and you've done it before at other companies. Where do you see most companies stalling, and what's actually working?**


The technology adoption piece is rarely what stalls an AI transformation. The tools are advancing fast and they're increasingly accessible. What determines whether a company sees real returns is how quickly it can rewire the way people work, make decisions, and collaborate — and whether leadership treats that as a sustained investment or a one-time rollout.


The change management dimension is where most companies underinvest. Getting hundreds or thousands of employees to fundamentally rethink how they work requires ongoing enablement, direct engagement with teams, and visible executive commitment. A training session and a Slack announcement don't get you there.


I'd also push back on the urge to perfect a grand plan before moving. AI capabilities evolve faster than most companies' decision-making cycles. By the time you've aligned every function around a comprehensive program, the ground has shifted and you may be building toward yesterday's outcome. Get directional alignment, build momentum, and iterate. Speed of learning matters more than precision of planning.


The distinction I keep coming back to is between “AI-assisted” and “AI-native” as the end states. “AI-assisted” means people use tools when it's convenient, and in our experience that yields something like a 10–25% productivity lift for individuals. “AI-native” means every workflow, every process, every role is redesigned around what's now possible, with AI at the center and humans in the loop where it actually makes sense. That's a different order of magnitude in impact. Every company should be honest with itself about which one it's actually building toward in the near term.


**Most security conversations default to risk and restriction. Is there a version of this conversation where strong security actually helps science go faster?**


That's the version I want to have.


Picture a biotech that wants to share assay results with a CRO, co-develop a compound with an academic lab, and submit data to a regulator — all in the same quarter. Today, each of those interactions triggers a separate security review, a separate access provisioning process, and a separate set of controls that slow things down. The scientists involved experience security as the thing standing between them and the work. I don't blame them for that, it's an accurate description of how most of this has been built.


The conversation I actually want to have is about what trust makes possible. What becomes possible when you can extend meaningful data access to partners, collaborators, AI systems, and CROs without the anxiety that has historically made collaboration a security liability?


Zero-trust data access that actually works at the API and workflow level — not just at the network perimeter — means you can give a CRO access to exactly the assay results they need, with a full audit trail, without handing over the keys to your compound library. That's not restriction, that’s precision. Precision access, built on infrastructure that partners can actually trust and verify, turns into a real advantage — in deal-making, in regulatory submissions, and in the AI partnerships where someone has to be able to prove the integrity of their data.


Biotech is uniquely dependent on porous, multi-party collaboration. The industries that treated security as pure friction got worked around; parallel tools emerged, and science paid the price. Benchling's position — structured research data, governed at the infrastructure layer, designed for collaboration from the start — is what makes a different answer possible.


*Research data infrastructure is becoming the most contested real estate in science. Sitting at the intersection of security, data governance, and AI transformation at exactly this moment is the kind of opportunity you don't pass up.*
