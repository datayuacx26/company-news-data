---
schema_version: "1.0.0"
document_id: "3f1da53b405cfef9689352dcba7969372f7fcde79ca4d5ee7f75df151f376338"
company_key: "yc-spott"
company: "Spott"
source_id: "yc-spott-news-import-75f63b0df54a"
canonical_url: "https://spott.io/resources/off-limits-management-executive-search"
published_at: "2026-07-10T12:48:17.434+00:00"
first_seen_at: "2026-07-24T01:59:40.161266+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:009d0678b47cd12f2e483de631a2de203774e748c31dd48c73a412e7adf950b2"
---

# The Executive Search Firm's Guide to Off-Limits Management (Without the Excel Spreadsheets)

## TL;DR


Off-limits in executive search is the agreement that a search firm will not recruit executives out of a client's organization while working for that client, typically for the duration of the engagement plus one to two years. It sounds like a static list. It isn't. Off-limits changes per engagement, scope is negotiated per contract, protections expire, and private equity clients often extend hands-off treatment across an entire portfolio that changes every quarter. Many firms track it in Excel because most ATSs tag candidates instead of companies and can't make a rule cascade. This guide covers how off-limits actually works, why spreadsheets and generic ATSs break, and the architecture to evaluate any tool against: company-level flags, employment data that updates itself, per-engagement expiry, portfolio grouping, warnings at the moment of outreach, and an audit trail.


A few months ago I was on a call with a partner at a boutique US executive search firm. Sharp team, strong PE client base, a specialist ATS built for their industry. Then we got to conflict management, and the partner shared a screen: an Excel workbook, held together with VLOOKUP arrays, tracking every off-limits obligation the firm carried. Their ATS could not do it. So a spreadsheet, owned by one person, protected the firm's most valuable client relationships.


After enough of these conversations, I can tell you: that firm is the norm, not the exception. Off-limits management is a spreadsheet problem hiding inside a database problem, and almost nobody has written down how it should actually work. So here it is.


## What does off-limits mean in executive search?


An off-limits agreement (also called a hands-off agreement) is a commitment that a search firm will not approach a client's employees as candidates for other searches. When a client pays you a retainer to find their next CFO, they are also paying for protection: you don't get to turn around and poach their VP of Finance for someone else six months later.


The mechanics, in practice across the US retained search market:


- **Client protection covers the client's people, not just the placed candidate.** The moment an engagement starts, the client's employees come off your huntable market.
- **The protection period usually runs the length of the engagement plus one to two years.** Two years from the end of the engagement is the most widely cited convention; one year from the last placement is a common contractual floor. None of this is law. AESC guidance stresses agreeing off-limits terms explicitly with each client, so the period that binds you is the one in your contract.
- **Scope is negotiated per contract.** Sometimes it covers the whole company. Sometimes one division, one geography, or one function. Sometimes far more, which brings us to private equity.


## Why do PE and VC clients make off-limits so complicated?


Here is the famous version of the problem. You run one search for a PE-backed portfolio company. Many funds expect hands-off treatment across the entire portfolio, and often negotiate it into the agreement. How far that protection reaches varies by fund, but it can mean dozens of companies.


And the portfolio is not a fixed list. Funds buy and sell companies every quarter. A business the fund acquires in March enters your off-limits universe in March, mid-engagement, without anyone sending you a memo. An exit reopens executives who were untouchable yesterday. Multiply that across three or four PE clients and your off-limits universe is a living thing that changes faster than anyone's spreadsheet.


## Why is off-limits so hard to track?


Because it is dynamic in four different directions at once:


- **It changes per engagement, not per firm.** Off-limits attaches to contracts, not logos. The same company can be protected under one engagement and open under another.
- **Scope is negotiable.** Whole company, a division, a portfolio. Every contract draws the line differently, so a single "blocked" checkbox can't represent it.
- **It expires.** A protection that lapsed last quarter should reopen candidates automatically. In most firms it doesn't, because nobody remembers to update the sheet, so the huntable pool quietly shrinks below what the contracts actually require.
- **The blocklist grows mid-engagement.** New acquisitions, mergers, and renamed entities enter protection while the search is running.


A static list fails on all four. Which is exactly what both of the standard tools are.


## Why do most ATSs fail at off-limits?


Most generic ATSs, and even some platforms built for executive search, fall short here for four structural reasons:


1. **They tag candidates, not companies.** There is no company-level "hands off" that cascades down to people. You end up bulk-tagging 400 individuals and hoping you caught everyone.
2. **The rule doesn't cascade to new joiners.** Someone who joins the off-limits company after your bulk tag is invisible to the rule. Your database says fair game; your contract says otherwise. This is how breaches actually happen: not recklessness, stale data.
3. **No per-engagement scoping or expiry.** A tag is binary and permanent. It can't express "off-limits for the Series B fund's portfolio until March 2027, but only the healthcare assets."
4. **No audit trail.** When a client calls and asks "prove you didn't approach our people," a pile of tags with no history is not an answer. You need a record of what was protected, when, under which engagement, and what warnings fired.


## Why the Excel approach breaks


The VLOOKUP workbook fails for the same reasons, plus a few of its own:


- **Manual refresh.** Someone has to notice every acquisition, exit, and expiry, and edit the sheet. PE portfolios change faster than anyone's diligence.
- **No cascade when people move.** The sheet lists companies; your outreach targets people. Nothing connects the two when an executive changes jobs.
- **No expiry logic.** Lapsed protections linger forever, shrinking your huntable market beyond what you actually owe anyone.
- **One person owns it.** When that person is on vacation, every recruiter on the team is one InMail away from a breach.
- **It isn't in the workflow.** The check happens (or doesn't) before a call sheet is built, not at the moment a recruiter reaches out.


## How firms manage off-limits today, at a glance


Capability Excel + VLOOKUP Generic ATS tags What you actually need


Company-level rule A row in a sheet No, per-candidate tags Flag the company once, rule cascades


Covers new joiners No No, tags are frozen in time Yes, via auto-updating employment data


Per-engagement scope + expiry A "notes" column No Activate and expire per contract


PE portfolio grouping Manual list per fund No Tag the fund, inherit the portfolio


Warning at outreach No, check happens earlier (if at all) Sometimes, if the tag exists Yes, at the moment of contact


Audit trail File version history Minimal Full log: what, when, which engagement


## What does a real off-limits system look like?


Treat this as a vendor-neutral spec. Whatever tool you evaluate, including ours, hold it against these six requirements:


1. **Company-level off-limits flags that cascade to every current employee in real time.** The rule lives on the company (or the fund), and every person currently employed there inherits it automatically.
2. **Employment data that keeps itself up to date.** The cascade only works if the system knows who works where today. If profiles freeze at import, the rule protects a snapshot, not a client. It is the same failure mode that turns candidate databases into a[database graveyard](https://spott.io/resources/ats-database-graveyard) : data freshness is an off-limits feature, not just a sourcing feature.
3. **Activation and deactivation per engagement, with expiry dates.** Protections should attach to contracts, carry their negotiated scope, and lapse on schedule without a human remembering.
4. **Portfolio grouping.** Flag the fund, inherit every portfolio company, and pick up additions and exits as the portfolio changes.
5. **Warnings at the moment of outreach, not after.** The block should surface when a recruiter is about to call or message someone, inside the workflow, not in a report someone runs on Fridays.
6. **An audit log.** When a client asks you to prove compliance, you want a timestamped answer, not a reconstruction.


If you're evaluating platforms more broadly, our guide to the[best ATS for executive search firms](https://spott.io/resources/best-ats-for-executive-search-firms) covers where each major option stands on search-specific workflows, off-limits among them.


## The honest part: off-limits is a strategy problem too


Better tracking doesn't make the underlying tension go away. Executive search has a blocking problem: every client you sign shrinks the pool you can hunt in. Win every top company in a niche and you have nobody left to recruit from. Boutique firms have to manage that trade-off deliberately: which clients to take, how broadly to concede off-limits scope, and when a lucrative retainer costs more in blocked candidates than it pays in fees.


Good off-limits management doesn't answer that question for you, but it makes the trade-off visible. When you can see exactly how much of your market each contract locks up, you can negotiate scope with data instead of instinct. That is a business development conversation, not a hygiene one.


## Where Spott fits


I'll keep this part short, because the spec above matters more than the pitch.


Spott is an[AI-native ATS](https://spott.io/resources/what-is-ai-native-ats) built around exactly the primitives off-limits management needs: company-level relationships as first-class records, employment data that updates itself as people change jobs, and AI that captures the context in your notes, calls, and messages rather than waiting for someone to fill in a field. That is the architecture the six requirements above assume.


If you're running off-limits in a workbook today,[book a demo](https://spott.io/start) and bring your spreadsheet. Walking through a real off-limits setup is the fastest way to see whether the cascade works.


## The bottom line


Off-limits decides whether you keep your best clients, and much of the industry manages it with a spreadsheet one VLOOKUP away from a breach. The fix is architectural: rules that live on companies, employment data that stays current on its own, scope and expiry per engagement, portfolio inheritance, warnings at the moment of outreach, and a log you can show a client. Evaluate every tool against that list.


If you want to see it working on your own client list,[book an exec-search demo of Spott](https://spott.io/start) . Bring the workbook.
