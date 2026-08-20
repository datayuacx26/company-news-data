---
schema_version: "1.0.0"
document_id: "3824dbbb5a187a8235cfb82b952de52e5eaeb7d8676f89002613fc3b3aa8e537"
company_key: "yc-great-question"
company: "Great Question"
source_id: "yc-great-question-news-import-f3444748def0"
canonical_url: "https://greatquestion.co/blog/protecting-pii-in-research-ops"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-23T10:44:58.935973+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:c7b16f90892468aa00cd2991a1cb685b99837e5df891dee65a8fb06a3e4a5083"
---

# How to protect PII in your research operations

**TL;DR:** PII is any bit of information that, dropped into Google, points back to a real person. In research operations it hides across your panel, your repository, your calendar, and every tool in between. The job is to know five things about it: what it is, where it lives, who can see it, when to remove it, and why you are holding it at all. The fewer tools that touch it, the easier all five get, which is a big part of why teams consolidate their stack onto one governed platform.


**Go deeper with two Great Question guides:**
→[Governance & security guide](https://greatquestion.co/features/security) — how role-based access, obfuscated PII, audit trails, and SOC 2 / GDPR / HIPAA fit together.
→[The ultimate guide to MCP for UX researchers](https://greatquestion.co/resources/download-mcp-guide-see-our-ultimate-guide-to-mcp-for-ux-researchers) — connect research to AI without handing over your participant data.


*Last updated: July 2026 to reflect current best practices, tools, and compliance.*


If you run research, you already know a good participant panel is the thing everything else stands on. Finding customers, getting them to opt in, managing their participation, not over-contacting them. Underneath all of it is a quieter responsibility: protecting the[privacy and security](https://greatquestion.co/features/security) of their data.


Kasey Canlas[put this well](https://www.linkedin.com/in/kaseycanlas/) in a ReOps community talk on PII. As research ops professionals, controlling access to participant data is our job, not IT's, not legal's, ours. And to control it, you have to answer five questions about every piece of it. What it is. Where it lives. Who has access. When it should be anonymized or deleted. And why you have it in the first place.


Here is how to work through each one.


## What is PII?


PII stands for personally identifiable information. The legal definitions run long, so here is Kasey's version, which is much easier to remember: if someone can take a few bits of information about a person, type them into Google, and surface that person's contact details, it is PII. Names, emails, phone numbers, and often job title plus employer when combined. It skips the legalese and gets the point across.


## Where PII hides in research operations


For[research ops teams](https://greatquestion.co/blog/ux-research-operations-101) , participant PII lives in more places than you would guess. Your panelist database. Your repository. Your recruiting emails. Even your calendar, where a participant's full name sits on every invite.


Now multiply that by the number of tools you use. If recruiting, scheduling, incentives, and analysis each run on a separate platform, PII gets copied into every one of them, and the list of people with access grows each time. A stack of a dozen tools is not just a workflow problem, it is a governance problem, because you cannot protect data you cannot see. This is one of the less glamorous reasons teams consolidate onto a single platform: fewer places for PII to live means fewer places to defend.


## Who should actually see participant PII


Most teams massively overestimate how much access people need to participant PII, and almost nobody needs as much as they have.


A product manager weighing feature A against feature B does not need the email of the person who preferred A. Title and company, maybe. Name and email, no. A designer reviewing feedback on a new flow does not need anyone's phone number, just their role and company size. The insight rarely depends on knowing who the person is.


The hard part is enforcing that when your tools were not built for it. In Great Question you set what counts as PII based on each user's role and strip it out of the panel, the repository, and everywhere else it would otherwise show up. People search the repo and pull the insight without ever seeing the identity behind it. If you are recruiting from a synced CRM, you can also set field-level visibility on the way in, so PII you do not want in research never lands there in the first place. For studies where identity should never be exposed at all,[concealed identity studies](https://greatquestion.co/features/concealed-identity-studies) keep participants anonymous end to end.


## When to anonymize or delete PII


Recency beats history in research. There is little reason to hold detailed records for someone who has not answered your last three invitations over six months. You set the parameters for keep-versus-delete, and if you are recruiting regularly, pruning disengaged contacts barely registers as work.


When you are deciding what to anonymize and when, four questions help:


- What participant PII is actually sitting in your systems right now?
- Who is accessing it, and for what reason?
- If you did nothing, how might this data leak out of those systems?
- Would you be comfortable if this were your own PII?


That last one tends to settle most debates.


## Why you have PII in the first place


The best question in Kasey's talk was the most obvious one: why. Researchers ask it all day about everything else, so turn it on your own data collection. Do you need a last name? Do you need an email for a quant survey, or would industry and company size do? Every field you request is a field you then have to store, secure, govern, and eventually delete. Ask why you are collecting it, and cut anything that cannot justify itself.


## The new wrinkle: PII and AI


When Kasey first gave this talk, governing PII mostly meant locking down databases and calendars. Now research data flows into AI too. Synthesis tools, AI-moderated interviews, and MCP connections that let an assistant query across every session you have ever run. That is a genuine unlock for speed, and it raises the stakes on the five W's, because "why do you have this data" now includes "and what is it being fed into."


The principle does not change, the surface area does. You still want the minimum PII necessary, scoped to the right people, removed when it is stale. You just want those controls to hold when an AI is reading the repository, not only when a person is. That means keeping governance and AI in the same system rather than exporting transcripts into a separate tool and losing your permissions at the door. If you are working through what responsible AI in research looks like, our[ultimate guide to MCP for UX researchers](https://greatquestion.co/resources/download-mcp-guide-see-our-ultimate-guide-to-mcp-for-ux-researchers) walks through how to connect your research to AI without handing over the keys to your participant data.


## Putting it into practice


Kasey shared[a template](https://docs.google.com/spreadsheets/d/1J1QepP6EBgdfaL0kU67Ugkv_HsPF42ry/edit#gid=635252615) you can clone to document your org's five W's for PII. Fill it in, then decide where you will anonymize data and for which groups. Tell your stakeholders what changed and why, so the new rules stick.


Then automate as much of it as you can. Manual PII hygiene does not survive contact with a busy quarter. Great Question was built with governance as a feature, not a bolt-on, because a good research experience starts with consent and ends with deleted data. That includes consent management, over-contact and over-poll safeguards, configurable user roles, and audit trails that log every time anyone, on your team or ours, touches participant data. On the compliance side, we are SOC 2 Type II,[GDPR compliant](https://greatquestion.co/features/security) from the ground up, and[HIPAA](https://greatquestion.co/features/hipaa) supported for healthcare research. It is also worth mapping your own obligations beyond GDPR: US state privacy laws like CCPA and CPRA, and HIPAA if you touch health data.


Protecting participant data is not the exciting part of research. But it is the part that lets everything else keep running, and doing it well is what earns you the next round of participants.


**See how governed research works in one place.**[Book a demo →](https://greatquestion.co/about/demo)
