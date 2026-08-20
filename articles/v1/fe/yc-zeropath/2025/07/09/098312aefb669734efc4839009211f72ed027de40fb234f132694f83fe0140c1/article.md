---
schema_version: "1.0.0"
document_id: "098312aefb669734efc4839009211f72ed027de40fb234f132694f83fe0140c1"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/idor-crisis-2025"
published_at: "2025-07-17T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:7a278572ed4ca163d61297a68eccefb06990e8d4d7caaeab6e23602471bbe6e9"
---

# Authorization Bugs Are Having Their SQL Injection Moment

Last week,[GitLab patched multiple critical authorization vulnerabilities](https://about.gitlab.com/releases/2025/07/09/patch-release-gitlab-18-1-2-released/) . Before that,[McDonald's leaked 64 million job applications](https://ian.sh/mcdonalds) through a basic IDOR.


Here's what's actually interesting: We've been using LLMs to analyze codebases for auth bugs since July 2024, and they're finding them everywhere. Not because these bugs are new - they've always been there. But because for the first time, we can find them programmatically.


Let me show you what we found.


## Why Authorization Bugs Have Always Been Different


Remember 2010? SQL injection was everywhere. Every pentest ended with a dumped database. Then frameworks evolved, parameterized queries became standard, and SQLi dropped from #1 to #3 in the OWASP Top 10.[Bobby Tables](https://xkcd.com/327/) became a meme, not the epidemic it once was.


Authorization bugs never got their Bobby Tables moment. They've been OWASP's #1 vulnerability for years because they resist the kind of fix that killed SQLi. You can't just parameterize them away.


[OWASP's data shows 94% of applications have broken access control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) . Unlike SQL injection, which dropped from #1 to #3, authorization bugs have stubbornly remained at the top.


Why? Because every authorization check is unique to your business logic:


```text
SQL Injection: Same fix everywhere (parameterized queries)
XSS: Same fix everywhere (output encoding)
Authorization: Different logic for every single endpoint
```


Since July 2024, we've been systematically testing how well AI can identify these logic flaws:


```text
Our Public Vulnerability Disclosures:
• Authorization/Access Control Flaws: 53% of critical vulnerabilities
• Traditional SAST Detection Rate: Near zero
• Time to Find Manually: 2 to 4 hours per endpoint
• Time to Find with LLMs: Minutes
```


## The Breaches Are Real


Authorization failures happen constantly:


**[GitLab, July 9, 2025](https://about.gitlab.com/releases/2025/07/09/patch-release-gitlab-18-1-2-released/) :** • Multiple authorization bypasses ([CVE-2025-4972](https://nvd.nist.gov/vuln/detail/CVE-2025-4972) ,[CVE-2025-6168](https://nvd.nist.gov/vuln/detail/CVE-2025-6168) ,[CVE-2025-3396](https://nvd.nist.gov/vuln/detail/CVE-2025-3396) ) • Users could bypass group-level restrictions on invitations and forking • High-severity 2FA bypass ([CVE-2025-0605](https://nvd.nist.gov/vuln/detail/CVE-2025-0605) ) • Stock impact: While some reports linked a stock decline to these vulnerabilities, correlation isn't causation


**[McDonald's, early July 2025](https://ian.sh/mcdonalds) :** • Job portal accepted` 123456:123456` as valid credentials •` PUT /api/lead/cem-xhr` endpoint exposed candidate data via` lead_id` parameter • Test applicant ID was ~64,185,742; decrementing revealed other applicants' PII • Result: 64 million job applications exposed


**[pcTattletale, 2024](https://www.malwarebytes.com/blog/news/2024/05/pctattletale-spyware-leaks-database-containing-victim-screenshots-gets-website-defaced) :** • Spyware company's own medicine •` /getScreen.php?device={id}` with no auth whatsoever • Result: 17TB of victim screenshots leaked


**[Assam Power Distribution, 2024](https://infosecwriteups.com/idor-account-takeover-how-i-secured-personal-information-pii-of-5-17m-electricity-consumers-a9db5e4999b9) :** • Endpoint:` /fetchConsumer?mobile_no={number}` • 5.17 million electricity customers exposed • Found by a security researcher on a lazy Sunday


Here's a sample of confirmed vulnerabilities we've reported after AI-assisted analysis (many more remain under responsible disclosure):


Date Project Vulnerability Impact


Jan 22, 2025 **SuperAGI** File download endpoint with no ownership check Any authenticated user could download ANY file in the system


Sep 2, 2024 **[RAGFlow](https://github.com/infiniflow/ragflow/security/advisories)** 8 separate IDOR vulnerabilities Delete anyone's conversations, access private knowledge bases, remove API keys


Sep 20, 2024 **[Monaco (Hulu)](https://github.com/hulu/monaco)** Unauthorized Redis access Access to ALL Redis clusters administered by Monaco


Sep 3, 2024 **[E2nest (Netflix)](https://github.com/Netflix/e2nest)** Path traversal in model loading Arbitrary file read via config manipulation


Oct 1, 2024 **[LogAI (Salesforce)](https://github.com/salesforce/logai)** Directory traversal via log paths Access to sensitive system files


Every single one of these boiled down to the same pattern:


```text
# What they wrote:
def     get_resource  (  id  )  :
return   Resource  .  query  .  get  (  id  )


# What they needed:
def     get_resource  (  id  ,   user  )  :
return   Resource  .  query  .  filter_by  (  id  =  id  ,   owner_id  =  user  .  id  )  .  first_or_404  (  )
```


One. Freaking. Line.


## The McDonald's Breach Was Embarrassingly Simple


The[McDonald's leak](https://ian.sh/mcdonalds) is a masterclass in how not to build an API:


1. **The Setup** : Job application portal, millions of users
2. **The Bug** :` PUT /api/lead/cem-xhr` with` lead_id` parameter, no ownership validation
3. **The Auth** : Hardcoded` 123456:123456` (yes, really)
4. **The Discovery** : Test applicant had ID ~64,185,742. Decrementing revealed other applicants' PII
5. **The Impact** : 64 million applications exposed


The exploit was trivial:


```text
for   lead_id   in     range  (  64185742  ,     0  ,     -  1  )  :
data   =   requests  .  put  (  "/api/lead/cem-xhr"  ,
json  =  {  "lead_id"  :   lead_id  }  ,
auth  =  (  "123456"  ,     "123456"  )  )
save_to_db  (  data  .  json  (  )  )
```


McDonald's probably ran security scans. They might have even had pentests. But traditional tools can't understand that` lead_id` should be bound to the authenticated user. An LLM would have caught this in seconds.


## What Actually Changed: LLMs Can Find These Bugs


Manual testing for auth bugs is tedious. Check endpoint, change ID, see if it works. Repeat hundreds of times.


Last year we tried something different. We fed a codebase to an LLM with this prompt: "Find places where user A can access user B's data."


It found 8 vulnerabilities in 20 minutes. All confirmed, all patched.


Traditional SAST tools can't find these. They pattern-match for SQL injection (user input + string concatenation) but they can't understand that` get_document(id)` needs ownership checks. That requires understanding intent.


LLMs understand intent. They read code like developers do. They see` download_file_by_id` and recognize it probably needs authorization. They trace through middleware and spot the missing checks.


[Our research demonstrates LLMs excel at finding these vulnerabilities](https://zeropath.com/blog/0day-discoveries) :


**SuperAGI** : File download endpoint took any file ID without checking ownership. The LLM identified it immediately by understanding that "download_file" functions require authorization.


**RAGFlow** : Eight auth bypasses in one codebase. The AI traced data flow across multiple files and found every single one.


**Monaco/Hulu** : AI determined Redis cluster access should be scoped to user permissions, even without explicit security annotations in the code.


The pattern was always the same: developers assumed the frontend would hide unauthorized options. They didn't.


## Why This Is About to Get Worse


Three things happening at once:


**1. Explosion of Attack Surface**
A 2005 web app might have had 20 endpoints. Modern apps expose hundreds or thousands. Each one needs correct authorization. The probability of mistakes has grown exponentially.


**2. Microservice Boundaries Multiply Risk**
Services trust each other by default. When Service A calls Service B, does B verify the request? Usually not. Each service boundary becomes a potential IDOR. A monolith might have had 5 trust boundaries; a microservice architecture might have 50.


**3. Discovery Is Now Automated**
What changes everything is that both defenders and attackers can now use AI to find these bugs systematically. Manual testing that took weeks now takes hours. The economics have shifted dramatically.


The McDonald's breach required someone to notice an endpoint and try changing IDs. But an AI can analyze every endpoint in your codebase, understand the access control requirements, and identify violations - all in the time it takes to run your test suite.


## How to Fix This


You can't fix auth bugs like SQL injection because they're logic bugs. But you can make them harder to introduce:


**Architecture patterns that work:** • Row-level security in your database • Centralized auth services like[Zanzibar](https://research.google/pubs/pub48190/) • Default-deny middleware • Use UUIDs instead of sequential IDs


**The one-liner everyone needs:**


```text
# Make this mandatory in your framework
@require_ownership      # Not optional
def     get_resource  (  id  ,   current_user  )  :
return   Resource  .  get_for_user  (  id  ,   current_user  )
```


The best defense? Run an LLM on your own code before someone else does.


## What Happens Next


Authorization bugs have been around forever. They're OWASP #1 for a reason.


What's different now is that anyone with API access can point an LLM at a codebase and find the same bugs that used to take security researchers days to discover manually.


The McDonald's breach happened because someone manually decremented an ID and found 64 million records. Imagine what happens when thousands of people start running LLMs against every API they can find.


The scale is about to change dramatically.


---


## The Numbers


**What we found:** • 53% of the critical vulns we reported were auth bugs • Traditional SAST tools found approximately zero of them • Manual testing: 2 to 4 hours per endpoint • LLM analysis: ~40 minutes for an entire codebase


**Industry stats:** • OWASP says 94% of apps have broken access control • It's been #1 for years, not getting better


Check out our[full analysis of finding 20+ zero-days with AI](https://zeropath.com/blog/0day-discoveries) for technical details.


The combination of auth bugs, AI discovery tools, and exponential API growth creates an unprecedented security challenge.
