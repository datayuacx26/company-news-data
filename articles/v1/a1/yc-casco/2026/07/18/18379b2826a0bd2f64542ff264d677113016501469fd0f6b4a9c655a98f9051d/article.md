---
schema_version: "1.0.0"
document_id: "18379b2826a0bd2f64542ff264d677113016501469fd0f6b4a9c655a98f9051d"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/introducing-casco-email-triage"
published_at: "2026-07-28T17:00:00+00:00"
first_seen_at: "2026-07-28T19:16:00.594663+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:1530101b4729cd933d9d7de0974e3c4034e9c4045c0fb3c2ef57fae988344a5a"
---

# Security Slop Killed curl's Bug Bounty. Now It's Coming for Your Inbox.

# Security Slop Killed curl's Bug Bounty. Now It's Coming for Your Inbox.


Written by


[Rene Brandel](https://linkedin.com/in/renebrandel)


on


Tue Jul 28 2026


Casco Email Triage is an AI agent that validates inbound vulnerability reports for you. Forward any report totriage@agent.casco.com


and the agent breaks it into individual risks, matches each one to the right application, safely runs the exploit, and tells you whether it's real. Valid findings reach your security engineers. Noise gets debunked, with the reasoning attached.


The reason it exists starts with curl.


## There is a security slop problem


Ask the curl project. For years, roughly 15% of the vulnerability reports curl received turned out to be confirmed vulnerabilities. Then AI-generated reports flooded in, and by 2025 about one in five submissions was outright slop. The confirmed rate collapsed to under 5%.


In January 2026, after more than six years and[87 confirmed vulnerabilities](https://daniel.haxx.se/blog/2026/01/26/the-end-of-the-curl-bug-bounty/) , curl shut its bug bounty down. This month, the maintainers temporarily paused vulnerability intake for a[“summer of bliss”](https://daniel.haxx.se/blog/2026/06/15/curl-summer-of-bliss/) : no reports during July, with submissions reopening on August 3, so the team can catch its breath.


Daniel Stenberg, curl's founder, warned that this problem starts with curl, but it does not end there.


Security teams inside companies are living the same story. From a[thread on reducing false positives](https://www.reddit.com/r/devsecops/comments/1uv8o7s/how_do_you_reduce_false_positives_and_duplicate/) in r/devsecops:


> “The false positive rate is the part that's grinding us down.”
>
>
> — u/Budget_Note4222


And from the same thread:


> “At scale it feels like we're spending more time analyzing findings than reducing real risk.”
>
>
> — u/Perfect_Process_8647


That second comment is pretty much the TL;DR of the problem. Analyzing findings is not reducing risk. It's the tax your security engineers pay before they are allowed to reduce risk.


## Outsourced triage doesn't know a feature from a flaw


The easy answer would be to put a triage layer in front of your inbox: a bug bounty platform, a managed service, a severity rubric. But third-party triagers don't know your application. They can't tell whether “public projects' chats are visible” is a vulnerability or a feature, because they don't know what your product is supposed to do.


Earlier this year, a valid broken object level authorization report against Lovable was[closed by bug bounty triagers without escalation](https://thenextweb.com/news/lovable-vibe-coding-security-crisis-exposed) , because the exposed data looked like intended behavior to someone outside the company. The vulnerability stayed open for 48 days. We covered what happened in a[video breakdown](https://www.linkedin.com/feed/update/urn:li:activity:7454907303935381504/) at the time.


So if you take security seriously, you can't fully delegate triage. You read every report yourself. Which means the flood lands on your security engineers, and the people you hired to solve strategic security problems spend their days disproving fiction.


## Casco's one-email fix


One email address:


```text
triage@agent.casco.com
```


Forward a vulnerability report there, or point your intake at it, and the agent goes to work:


**Splits the report into individual risks.** One rambling submission often contains three claims. Each needs to be evaluated on its own.


**Matches each risk to the right application.** The agent knows your applications through Casco, so “the login page” resolves to an actual asset, not a guess.


**Runs the exploit, safely.** This is the part that matters. The agent doesn't score the report on how plausible it sounds. It attempts the exploit itself, with safety guardrails in place, against the application in question.


**Delivers a verdict.** If the finding is valid, your security engineer gets a notification with the confirmed vulnerability. If it's invalid or noise, you get the reasoning behind it, so nothing is silently dropped.


The difference between this and a filter is validation. Instead of guessing the validity of a finding, Casco's Email Triage agent confirms it.


See the[Casco Email Triage feature page](https://casco.com/features/email-triage) for the full product walkthrough.


## What this means for you, the security engineer


**Your team gets its time back.** The reports that reach a human are the ones worth their attention. The rest arrive pre-debunked, with reasoning you can spot-check instead of work you have to redo.


**Signal separates from noise on evidence, not vibes.** A report that sounds sophisticated but doesn't reproduce is noise. A clumsy report describing a real exploit is signal. Running the exploit is the only test that can't be fooled by well-written slop.


**The triage layer actually knows your business.** Unlike an outsourced triager, the agent has your application context through Casco's continuous penetration testing. It can tell intended behavior from broken access control because it knows what the application is supposed to do.


## The bigger picture


Our mission at Casco is to make all software effortlessly secure. Autonomous security testing finds vulnerabilities year-round. The[Casco MCP server](https://casco.com/blog/introducing-the-casco-mcp-server) brings validated findings to the agent that fixes them. Email Triage handles the findings that come to you and holds them to the same bar: validated, in context, or explained away with reasoning.


The AI security slop wave pushed curl to take a month-long break from vulnerability reports. We are aiming to make sure your inbox never needs the same reset.


Get year-round security with autonomous security testing. Get started at[https://casco.com](https://casco.com/) .


Quick answers


## Frequently asked questions


What to know before connecting Casco to your coding agent.


###


###


###


###


###
