---
schema_version: "1.0.0"
document_id: "7b5ec9a0c540a94a19042fbd8e5847699c144adbfa168efacca63b5258a9bc3e"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/agentic-ai-for-cyber-defenders-what-security-teams-built-at-black-hat-usa-2026"
published_at: "2026-08-07T12:00:00+00:00"
first_seen_at: "2026-08-08T00:58:09.597929+00:00"
fetched_at: "2026-08-08T00:58:10.395533+00:00"
content_hash: "sha256:1dfbbd20766d8c4a17c4ee38fd4358b99df55c76c99be30c6a61818b06d2f29f"
---

# Agentic AI for cyber defenders: What security teams built at Black Hat USA 2026

Agentic AI armed attackers first, but it also put real building power in defenders’ hands. Here’s what security practitioners built in two days at Black Hat USA 2026, and how the CyberAgents Exchange keeps that work compounding long after the event.


## **Key takeaways**


1. **Building defensive cybersecurity tooling no longer requires a developer.** Agentic tooling drove the cost of finding and exploiting a vulnerability down to 1990s levels; it also removed the engineering barrier that kept defenders from building the automation they’ve always wanted.


2. **The unglamorous work won the room: triage, reconciliation, toil.** Given two days and a requirement to publish, practitioners at SWARM developed agents for prioritization, cross-tool reconciliation, and the unglamorous toil they recognize from their own environments.


3. **All SWARM builds live on the**[CyberAgents Exchange](https://exchange.tenable.com/) **, source repos attached.** Every component built at SWARM is published open source with its source repository attached, so the next team facing the same problem starts from working code instead of a blank editor.


## Another security team already built the AI agent you need


You may not realize it, but somewhere out in the ether, there’s a security team facing the same challenge as you. The only difference is they just solved the problem with agentic AI. The problem is, you have no way to find out the solution even exists.


It could be an asset inventory that three systems describe in three different ways; a findings queue nobody has the hours to work; or a “critical” that a platform upgrade quietly neutralized six months ago, still sitting there waiting for someone to prove it. Somebody has already built the thing you keep meaning to build. Now it’s time we help you find it.


At Black Hat USA 2026, nearly 100 registrants had the opportunity to come together for 48 hours and solve both halves of that problem: identifying a key operational pain point and building the fix.


Problems like those got solved at SWARM, and the fixes are sitting on the[CyberAgents Exchange](https://exchange.tenable.com/) right now, open source, with their source repositories attached.


One team built the agent that works out which handful of fixes retires the most risk across thousands of findings. Another correlated two scanners to tell whether a flaw in the code is even reachable in the running application. A third made the case that a finding had already been mitigated, with evidence an auditor would accept. You can download and deploy any of them today.


## Agentic AI doesn’t just arm attackers


Black Hat’s keynote stage spent this year focused on one theme: the plummeting cost of cyber offense in the agentic AI era. The price for an attacker to find and exploit a vulnerability is at lows the industry hasn’t seen since the 1990s, when a working exploit meant weeks of expert reverse engineering. Now all it takes is an afternoon and a subscription.


The artisanal exploit isn’t rare anymore. True. But neither is the defender who can build.


The same agentic tooling that’s arming attackers puts real building power in everyone’s hands, and that half of the story got almost no airtime. **Security automation used to require the work and ongoing maintenance of skilled engineers. Now practitioners who understand the problem can build the fix.**


Inside the conference room at the Mandalay Bay where Tenable hosted our inaugural[SWARM](https://www.tenable.com/blog/black-hat-2026-swarm-event-build-AI-security-agents) event, the proof of that was on every table. The winning ranking engine ships as[a skill that runs on the Python standard library alone](https://exchange.tenable.com/skills/chokepoint-finder-skill/) — no packages, no install step, no build pipeline. Point it at the bundled demo estate and it answers “what should we fix first?” in seconds. That’s a deliverable a practitioner can produce and a colleague can run, and two days was enough.


The attacker-defender asymmetry doesn’t stem from a lack of talent or willingness. Offensive cyber capabilities compound because the tooling circulates: it’s built once, forked, passed on, or sold to the next threat actor to leverage in their attack.


Meanwhile, defenders continue to build in silos, with hundreds of teams solving the same fix for the same problem. Not because anyone wants to keep it to themselves, but because there’s never been an easy way to pass it along. Defensive cyber tools are built, then lost, then rebuilt somewhere else. That’s a distribution gap, and now defenders can close it themselves on the[CyberAgents Exchange](https://exchange.tenable.com/) .


## We gave defenders a mission and two days to build


Tenable hosted its first SWARM event, running alongside Black Hat, so practitioners could come together and build open-source agentic AI. Sponsored by AWS and with technical staff from Anthropic onsite for judging, SWARM came with a couple simple rules: build something practical that solves a real problem for your team, and publish it to the CyberAgents Exchange.


Then we got out of the way, and the magic immediately took shape.


### Practitioners built AI agents for Monday morning, not for the demo


The room skewed hard toward unglamorous work. Not autonomous red teams or self-healing networks, but the specific tasks that eat a practitioner’s week: reconciling asset inventories that three systems disagree about, triaging a findings queue nobody has time for, chasing down whether a finding is even real before someone spends a sprint on it.


One team built an agent that reads vendor advisories and mitigation notes, compares them against live findings, and recommends risk recasts — the kind of work a senior engineer does by hand, one CVE at a time, and never gets credit for. They built the thing that was annoying them last Tuesday.


### Defenders didn’t build robot analysts, they built plumbing


The most common thing in the room wasn’t a flashy autonomous agent. It was connective tissue: capabilities to normalize findings across scanners that describe the same asset three different ways, wrap the tools that teams already own so an agent can reach them, and package recurring analyst tasks as reusable skills instead of one-off scripts.


That tracks with how this technology actually gets adopted. Nobody rips out their stack to adopt agentic AI. They teach an agent to drive what’s already there, and the integration layer is where the real work sits.


## The agentic AI builds the judges put on the podium


Three teams took the podium. None of their work stayed in the room: every build had to ship to the CyberAgents Exchange as open source to be eligible to win, so the shortlist below is a set of tools you can read and run tonight. The judges — Tenable CSO Robert Huber, AWS Security Specialist SA Leader Chris Elmore, and a member of Anthropic’s technical staff — scored impact above everything else, and all three solve problems you probably recognize.


### First place: from thousands of findings to a handful of proven fixes


#### [Chokepoint Finder](https://exchange.tenable.com/playbooks/chokepoint-remediation-playbook/)


Built by Team Vauban; MIT licensed


Security teams don’t have a detection problem, they have a selection problem: a mid-size estate carries thousands of open findings and capacity for maybe 10 changes a week. Published as four agentic components—an[agent](https://exchange.tenable.com/agents/chokepoint-finder/) ,[MCP server](https://exchange.tenable.com/mcp-servers/chokepoint-finder-mcp/) ,[skill](https://exchange.tenable.com/skills/chokepoint-finder-skill/) , and[playbook](https://exchange.tenable.com/playbooks/chokepoint-remediation-playbook/) —Chokepoint Finder ranks fixes, not findings. It groups findings by the single action that resolves them (i.e., patches, base images, IAM roles, security groups, etc.) then solves for the shortest ordered list that mitigates the most weighted risk, leveraging its playbook to agentically inspect and rank fixes over eight distinct stages, with a human decision in the middle.


That discipline earned the top spot. The agent holds no write credentials, evidence it can’t read or can’t date resolves to a hold, and a re-scan has to prove the risk moved before the record closes. The team’s week stops being an unmanageable queue, collapsing its demo estate with 3,734 synthetic findings across 783 assets into seven concrete remediation actions. Each member of Team Vauban took home $2,000 in Anthropic credits, an AWS specialist certification voucher, and a gold-border SWARM patch.


### 2nd Place: determine if flaws in code are reachable


#### [ThreatCorraling](https://github.com/giraldomauricio/threatcorraling)


by Team ShellCodeandChill; MIT licensed


A team running two scanners has two lists and no shared view: the code scanner flags a flaw, the application scanner watches the running app, and nobody can say whether the flaw is reachable. ThreatCorraling correlates Checkmarx static analysis results against Tenable Web Application Scanning findings for the same scoped assets, so a team can tell which flagged flaws are actually exposed in production.


From there it maps what it finds to SOC 2 and ISO 27001 controls and generates remediation and regression-testing patterns specific to the team’s stack, rather than generic advice. The reconciliation spreadsheet becomes a ranked, control-mapped list. It ships as both halves of the same code: a local app, and an MCP server exposing the same tools to clients like Claude Code. Each member of Team ShellCodeandChill took home $1,500 in Anthropic credits, an AWS specialist certification voucher, and a silver-border SWARM patch.


### 3rd place: prove if findings were already mitigated


#### [Evidence-Backed Vulnerability Investigator](https://exchange.tenable.com/agents/evidence-backed-vulnerability-investigator/)


by Team ZeroSignal; MIT licensed


Arguing that a finding was already mitigated usually means an analyst has to rebuild the case from memory with no traceable record of why. The Evidence-Backed Vulnerability Investigator loads scanner findings, matches them against the vendor advisories a team already keeps on disk, and asks Claude for a disposition with detailed justification evidence.


The matching is deterministic, performed by an identifier and keyword scoring script so analysts can reliably see why advisories are returned, and only the human’s decision gets recorded. The disposition arrives with its evidence attached instead of as an assertion. Anyone who’s argued with an auditor about a “critical” upgrade that’s quietly fixed knows what that saves. Each member of Team ZeroSignal took home $1,000 in Anthropic credits, an AWS specialist certification voucher, and a bronze-border SWARM patch.


### Congratulations to the winners!


Congratulations to all three teams, and to everyone who shipped something in 48 hours and put their name on it in public. That takes more nerve than a demo. One SWARM participant walked out with an NVIDIA DGX Spark, drawn from raffle tickets every ceremony attendee earned just by being in the room.


## SWARM was two days, the CyberAgents Exchange lives on


Every component built at SWARM is now on the[CyberAgents Exchange](https://exchange.tenable.com/) , under open licenses, and that’s the whole point. The work gets done once. Take two days of work from a few dozen practitioners, then multiply it by a community publishing continuously instead of once at a hackathon, and you get the two things threat actors have always had that defenders haven’t: scale and distribution.


### What is the CyberAgents Exchange?


The CyberAgents Exchange is the industry’s[first open-source, vendor agnostic AI agent exchange](https://exchange.tenable.com/) , a cybersecurity-native registry for AI agents, skills, MCP servers, and multi-agent playbooks. It’s built around the objection practitioners actually have to agentic AI, and that objection was never automation. It’s opacity. You can’t responsibly hand production authority to something whose reasoning and provenance you can’t inspect. That isn’t blind distrust; it’s justifiable caution.


So every component links to its source repository. No bundled binaries. You can see who built it, when, and whether it’s been reviewed by Tenable, vetted by the community, or if it was freshly submitted. You set your own trust threshold before you run anything. The CyberAgents Exchange is free: there are no fees to list or use what’s there. SentinelOne and Recorded Future joined as founding members, which matters, because a registry carrying one vendor’s agents isn’t an exchange.


That’s the idea Tenable CTO Vlad Korsunsky keeps coming back to: “Security is a team sport,” he says. “We’re creating a collaborative ‘town square’ where cybersecurity practitioners can build, test, improve, and share the best in agentic defense.” Security has already learned to circulate threat intelligence, indicators of compromise, and detection rules. Agentic tooling is simply the next.


## **Your turn to build an AI agent**


Please[check out the CyberAgents Exchange](https://exchange.tenable.com/) for yourself!


Start by leveraging what other teams have already contributed. Find the component that maps to your own worst Tuesday scenario, read the source, inspect the code, and deploy it on your terms. Build if you feel inspired. If not, express your gratitude to builders, and strengthen our collective defense either way.


## **Learn more**


- Read the[announcement of the CyberAgents Exchange](https://www.tenable.com/press-releases/tenable-launches-industrys-first-open-source-ai-agent-exchange) , the industry’s first open-source AI agent exchange for cybersecurity
- See what[30 days with Claude Mythos Preview](https://www.tenable.com/blog/testing-claude-mythos-preview-for-code-security-tenable) taught Tenable’s own security team about proving exploitability instead of ranking suspicion
- Explore how[Tenable Hexa AI automates exposure remediation with agentic routines](https://www.tenable.com/blog/tenable-hexa-ai-automating-exposure-remediation-with-agentic-routines) , with a human in the loop
