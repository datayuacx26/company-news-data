---
schema_version: "1.0.0"
document_id: "a16b9ccde082b4fa566fca78de9241eb8c6a94941c3920921c8204f5d3e5773e"
company_key: "yc-spruceid"
company: "SpruceID"
source_id: "yc-spruceid-rss-1607485977f6"
canonical_url: "https://blog.spruceid.com/spruceid-at-the-2026-utah-data-governance-summit/"
published_at: "2026-05-27T21:24:59+00:00"
first_seen_at: "2026-07-24T02:02:18.818185+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:9418e5e9749950de5f9d9b4c445732638fc7bdfa16675140feef1694f0c5d8bf"
---

# SpruceID at the 2026 Utah Data Governance Summit

On May 21st, SpruceID joined state and local leaders, agency privacy teams, records managers, and policy makers at the[2026 Utah Data Governance Summit](https://www.uvu.edu/herbertinstitute/events/2026-events/index.html?ref=blog.spruceid.com) , hosted by the Gary R. Herbert Institute for Public Policy and the Utah Office of Data Privacy at Utah Valley University. The day's theme, set by Utah Chief Privacy Officer Christopher Bramwell in the opening keynote, was that data governance in Utah has moved beyond policy. The work now is implementation, turning law and frameworks into everyday operations of agencies, counties, and the programs residents actually use.


That framing is exactly where verifiable digital credentials belong. As an advocate for State-Endorsed Digital Identity (SEDI) and verifiable digital credential issuance, SpruceID was on the ground to share how a proposed shared-service model could carry governance, privacy, and local control into one of the highest-volume civic interactions residents have with their government.


## Our presentation: marriage licensing as a shared-service governance model


During the lunch session in the Grand Ballroom, SpruceID delivered a five-minute talk titled *"Marriage licensing as a shared-service governance model."* Marriage licensing is a familiar workflow, but it is deceptively complex: a single license touches identity, vital records, county authority, legal rights, and a long chain of downstream proof. It is a clean, human example of why governance should be built into the credential itself, rather than bolted on afterward.


Utah has 29 counties, and today each manages marriage licensing in its own way. In the shared services approach we proposed, counties would keep local control over fees, appearance rules, and process. What's missing today is a versioned reference layer that would allow the state to publish the shared parts, such as fields, notices, rules, templates, and credential schemas, once, so counties could adopt the baseline, extend locally, and override only where permitted.


## The problem, in plain terms


A marriage license is an official application signed by two applicants. Counties differ in the identity verification methods (in-person, hybrid, remote, kiosk), appearance rules, and how officiants return the record. Once issued, the marriage certificate becomes the downstream proof a resident has to carry into dozens of unrelated systems: name changes at the[Driver License Division](https://dld.utah.gov/?ref=blog.spruceid.com) and[Social Security](https://www.ssa.gov/?ref=blog.spruceid.com) , passport updates, banking, insurance, healthcare records, and even international use through the Lieutenant Governor's e-apostille process. A PDF emailed from a clerk's office is not, in itself, trusted proof. So the resident ends up shuttling paper from counter to counter, and each reviewer makes a subjective judgment about whether the document is valid.


## The shared-service model


The proposed model we presented separates statewide guidance from local execution:


- **State reference layer:** Versioned fields, notices, rules, templates, and credential schemas would serve as the public standard.
- **County implementations:** Traditional, hybrid, advanced remote, and clerk-assisted/kiosk modes, all able to run on the same backend.
- **Official application + governance evidence:** Every submission would be version-locked. Field set, notice text, and rules would be captured at submission. Where a county's rules diverge from the state's, the difference would be recorded. Counties would not be able to opt out of legally required changes.


This is the practical meaning of privacy-preserving by design *.* In this model, the governance framework would be built into the platform itself, creating alignment between policy intent and operational execution.


## Trust built into every transaction


In a shared-service model like the one we proposed, every field, notice, signature, decision, and credential would trace back to the same governance model. At input, the record would carry the notice shown, the legal authority, classification, record series, retention rule, and access/audit settings. Upon the decision, the license record would capture the clerk, the inputs, the policy version, and the timestamp together. After the decision, residents would get clear notice, correction paths, appeal rights, an evidence vault, and privacy-safe transparency.


AI can sit inside those guardrails. It can accelerate clerk review by flagging missing fields, surfacing inconsistencies, and routing exceptions, but the clerk would remain the decision-maker, and every action would tie back to a policy version and audit trail. That is the only way responsible automation works in a public-records context.


## Beyond the emailed PDF


The slide that got the most visible reaction in the room was the one that asked how many people have gone through a name change with the government after getting married - a lot of hands went up. The point landed because the pain is real: the marriage certificate can end up in property insurance, will and trust documents, the passport agency, the Social Security Administration, the DMV, banks and investment accounts, employers, tax records, TSA PreCheck, and sometimes foreign governments. A shared-service data model like the one we proposed could govern the license at issuance and naturally extend to the certificate as a verifiable digital credential, one that downstream systems can actually verify, rather than guess.


That is what SpruceID means by an interoperable trust framework: a model in which the same record could support DTS credential issuance, a name-change proof package, vital-records registration, and Lt. Gov e-apostille, without forcing residents to keep handing over paperwork.


## Citizen experience and clerk operations


The presentation closed on the two audiences that matter most. For residents, in the proposed model, county selection would drive the mode, but the public-facing flow would remain consistent, and notices, affidavits, restricted fields, and signatures would be versioned and stored as evidence.


For clerks, applications would become managed queue items moving through a clear pipeline: submitted, in review, ready, issued, completed, denied, with AI as decision support and the clerk as the decision-maker. The result would be faster service delivery, improved transparency, and an auditable privacy posture in the same workflow.


## Marriage licensing is the example, not the limit


Marriage licensing is a useful wedge because almost everyone in the room had felt the friction themselves. But the same pattern could apply to business licensing, permits, public records, and benefits intake, anywhere counties run the same essential service 29 different ways, while the state sets the standards.


The bigger question the summit asked, and that the afternoon's *"Is a shared services model needed?"* breakout dug into directly, is whether Utah is ready to build that shared layer - who funds it, who governs it, who's accountable when something goes wrong, and what it means for a small rural county without a dedicated data officer.


Those governance and funding considerations become easier to address when the stakeholders responsible for program budgets and implementation can see how a shared-services model could improve the citizen experience while helping reduce operational burdens for counties, particularly those with more limited technical resources.


## What's **** next


We are grateful to the Herbert Institute, the Utah Office of Data Privacy, and the partners and sponsors who put this summit together, and to the clerks, agency leaders, and elected officials, and more who took the time to listen to our talk.


If your agency, county, or program team is thinking about how verifiable digital credentials and shared-service governance could fit your work, marriage licensing or otherwise,[we’d love to hear from you](https://spruceid.com/contact-us?ref=blog.spruceid.com) .


Building digital services that scale take the right foundation.


[Talk to our team](https://spruceid.com/contact-us?ref=blog.spruceid.com)


**About SpruceID** : SpruceID builds digital trust infrastructure for government. We help states and cities modernize identity, security, and service delivery — from digital wallets and SSO to fraud prevention and workflow optimization. Our standards-based technology and public-sector expertise ensure every project advances a more secure, interoperable, and citizen-centric digital future.


Share
