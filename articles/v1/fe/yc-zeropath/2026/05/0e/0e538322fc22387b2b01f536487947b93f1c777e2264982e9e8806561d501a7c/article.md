---
schema_version: "1.0.0"
document_id: "0e538322fc22387b2b01f536487947b93f1c777e2264982e9e8806561d501a7c"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/introducing-zero"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-24T06:50:52.708520+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:c31927882c9b79f5db8d6429a5b7aa761559e7037e5fbd7152ceb1297e07d9bf"
---

# Zero: AI Assistant For AppSec

Security engineers spend a lot of time on work that doesn't require them.


Think about what a security team actually gets asked to do on a given week. Run a Security Champion program: figure out who fixed the most vulns, publicly praise them, send the report. Orchestrate a security sprint: split open issues into the top 10 by team, ping the security champion with the tickets, PRs, and assignees, send final reports to eng heads. Answer the question: "are we affected by this CVE?" These aren't hard problems. They're coordination problems. And they eat engineering time that should be going somewhere else.


We've been thinking about that gap for a long time. Today we're shipping Zero, our AI assistant for AppSec teams, and I want to walk through what it actually does rather than describe it in category language.


## The bug bounty triage demo


In the demo we've been running, a bug bounty report arrives over email. Full account takeover via unauthenticated security question migration brute force password reset. The security engineer receiving it has no idea which repo it's for, doesn't know the project, and has seen enough synthetic reports to be deeply skeptical.


He forwards it to a ZeroPath email address and goes back to work.


What happens next is the part worth paying attention to. Zero picks up the inbound email via webhook, triggers the bug bounty flow, and starts posting its progress in a Slack thread so engineers can monitor what it's doing. It analyzes the report against the codebase, determines validity, maps it to the affected repositories, drafts a fix, and if ZeroPath didn't already have a rule covering this class of issue, writes a natural language detection rule to catch similar ones in the future. Then it kicks off a remediation campaign, scanning the entire organization's repos for the same class of vulnerability.


The whole thing completes within about 10 minutes. As a side effect of the triage flow, it triggered 106 repository scans across the environment because those repos didn't have recent scans on file — including repos across different teams, different codebases, different versioning histories. It noted that some were mid-scan and would pick up the new detection rule automatically, and reported back.


It figured out that was the right thing to do.


## Building a workflow in plain English


The SCA response flow is where I'd point someone who's skeptical about whether this is actually agentic or just a fancier dashboard.


In the demo, the engineer types out what he wants in plain English: for every SCA issue deemed exploitable, coordinate a response, draft an upgrade PR, create a Linear ticket, assign it to the right developer based on recent commit history, and post in the primary Slack channel. If there's no response within an hour, send another message and DM both the engineer and the CISO. After two days with no patch, escalate to the CISO with the full trail: what happened, who was notified, what repos are affected, and the criticality assessment.


Zero builds that workflow out. While doing it, it flags two things: it doesn't have a CISO identity in memory and will ask for that at runtime, and Linear isn't connected yet. Rather than silently failing or building a broken workflow, it surfaces exactly what's missing and asks for help configuring it.


That's a small thing but it matters. A tool that fails silently is a tool you can't trust with anything important.


## The security SCA sprint


The third workflow is the one that resonates most with teams that have been running ZeroPath for a while. Think of it as a weekly security SCA sprint cycle: Zero finds the top critical and high severity findings across the environment, patches them in bulk, opens PRs, and notifies the right people – on a schedule, without anyone having to kick it off.


In the demo, the engineer types: "We have a number of SCA vulnerabilities open. Go find the top 5 critical or high, create a single PR that bulk fixes them, ping the relevant developers for each one, and post the PR in Slack for review."


Zero identifies 11 critical and high SCA findings, selects the top 5, determines they span two repositories, clones the repos, patches the vulnerabilities, and opens two separate PRs. Because it has memory and understands how the ZeroPath instance is configured, it includes the issue IDs in the PR descriptions automatically so they close on merge.


Run this on a schedule and your security debt stops accumulating.


## What's underneath it


Zero is only as reliable as the signals it acts on. An agent built on noisy findings doesn't save time, it just automates bad decisions at scale. So before we built Zero, we spent considerable time on the underlying platform.


The SAST V2 rebuild introduced a validation engine that uses synthetic vulnerability creation and filtering, reducing false positives by roughly 50%. Detection improved by going sync-to-source, meaning ZeroPath analyzes code in the context of how it actually executes rather than matching patterns against rule sets. The independent researcher Joshua covered this in depth earlier this year: running ZeroPath against curl alongside scan-build, clang-tidy, CodeSonar, Coverity, CodeQL, and OSS-Fuzz, all of which had already processed the codebase, ZeroPath surfaced 200+ additional real bugs. About 20% turned out to be false positives. Daniel Stenberg, curl's maintainer and a well-known skeptic of AI-generated bug reports, described some of the findings as "actually truly awesome."


Preconditions is another piece worth understanding. The consistent failure mode in SAST is surfacing a real vulnerability with wrong criticality because context is missing. You see an API route that allows arbitrary database writes and flag it as critical. But you don't know it sits behind a GraphQL authorization layer. Surface it as P0 and you lose trust with the developer immediately.


Preconditions make the uncertainty explicit. Instead of just reporting a finding, ZeroPath surfaces what has to be true for the criticality to hold: "the route is publicly exposed," "a valid account is required to reach this path," "this table contains PII." A security engineer can look at that list and immediately say which conditions don't hold in their environment. That feedback loop is what makes the scanner trustworthy enough to actually act on autonomously.


## The self-improvement piece


One thing Andrea Cappa, Security Lead at Aptos Labs, called out after evaluating Zero: the self-improvement loop. When false positives are flagged, Zero doesn't just log them. It correlates similar reports, refines detection rules using organizational knowledge, and routes those refinements back to the security team for approval. The system builds an increasingly precise model of your environment over time, your authorization patterns, your ownership structure, your SLA policies, without requiring additional configuration.


The alternative is a tool that's exactly as accurate on day 365 as it was on day 1, which means someone is manually tuning it forever or it slowly becomes less useful as the codebase evolves.


## Why we built this


Application security is a wicked problem. Every organization has a different way of measuring success, a different process to follow, different metrics, and different opinions on how to run an AppSec program. As a result, the actual tools they need differ from case to case. The primitives might remain the same, but the workflows they want to enable are different and arbitrarily complex.


Zero is built to support that. Security teams are expensive, experienced, and hard to hire. The work that actually requires them gets crowded out by coordination overhead. Zero is built to absorb that overhead.


Zero is available now. See it for yourself at[https://zeropath.com/demo](https://zeropath.com/demo) .


## See Zero in action
