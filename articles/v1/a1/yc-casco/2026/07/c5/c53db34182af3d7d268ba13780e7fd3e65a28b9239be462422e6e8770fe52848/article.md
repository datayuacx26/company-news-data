---
schema_version: "1.0.0"
document_id: "c53db34182af3d7d268ba13780e7fd3e65a28b9239be462422e6e8770fe52848"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/automatic-context-refinement-loops"
published_at: "2026-07-29T17:00:00+00:00"
first_seen_at: "2026-07-29T21:36:52.479365+00:00"
fetched_at: "2026-07-29T21:36:53.501082+00:00"
content_hash: "sha256:16b1d7b0d952616a07050607c4877cd6c46c80fe3e03b9ea079e8ee9433bb0f7"
---

# False Positives Are Context Failures.

# False Positives Are Context Failures.


Written by


[Rene Brandel](https://linkedin.com/in/renebrandel)


on


Wed Jul 29 2026


Casco's automatic context refinement loops turn every false positive into application context that prevents the next one. Casco's default false-positive rate is approximately 5% across all customers. With context tuning, it drops below 1%. Until now, that tuning happened in the background. Today we're exposing the whole loop as a user-facing setting, where you can see it, verify it, and tune it yourself.


## So where is this false positive coming from?


Scanners earned their reputation. They pattern-match: flag everything that looks like a vulnerability and let a human sort it out. They have no understanding of what your application is supposed to do, so the noise is structural. That's how “scanner” became shorthand for a thousand findings and thirty real ones.


Casco is not a scanner. It's an[agentic pentester](https://casco.com/supervised) . It explores your application, builds an understanding of how it works, and validates exploits before reporting anything. That's why the default false-positive rate is 5%.


However, the interesting part is that last 5%. It's almost never a detection error. It's a knowledge gap: that endpoint is a honeypot, that “exposed” data is a public API by design, or that admin route is gated by a proxy upstream. Things no pentester, human or agent, could know about your product on day one.


## How the refinement loop reduces false positives


Unlike a detection error, a knowledge gap only needs to be filled once. Capture the reasoning behind a dismissed finding as application context, and the same mistake can't happen twice.


That's the loop Casco's forward-deployed security engineers have been running behind the scenes. Every false positive became context, and the system understood the product a little better with each one.


Starting today, that loop runs in your dashboard instead of behind it:


**Every false positive feeds the context.** When a finding is marked as a false positive, its context and the reasoning behind it are added to **Settings → Application Context** automatically.


**Your engineers can read the reasoning.** No more wondering why a finding was dismissed. The context sits in plain text, in your settings, with the why attached.


**Your engineers can edit it.** Disagree with an entry? Change it. Know something the system can't know yet? Add it.


## Application context works before the finding exists


Here's what makes context different from every suppression rule and allowlist you've ever written.


A suppression rule reacts to a finding. The false positive has to fire first, someone triages it, dismisses it, and writes the rule. You pay the noise tax once per mistake, per tool, forever.


Application context is the knowledge of how your product is supposed to behave: which behaviors are by design, which environments hold seeded test data, and which protections exist in infrastructure a tester cannot see.


One entry, such as “the` /demo` environment is intentionally seeded with fake customer data,” prevents every future finding that would have tripped over it, across finding types, including ones that have never fired. And because your team can write context ahead of time, the false positives for next sprint's feature can be prevented before the first security test runs.


You're not cleaning up noise. You're making it impossible.


## Trust, but verify your security vendor


There's a quieter reason we built this, and it's about incentives.


Every security vendor claims low false-positive rates. Almost none show you the machinery that produces them. If the tuning is invisible, you can't distinguish “the system understands my application” from “someone is quietly suppressing findings to make the dashboard look clean.”


Exposing the context makes the claim auditable. You can read every piece of context, see the reasoning it came from, and check it against how your application actually works. If we get something wrong, it's in plain sight, and you can fix it in the settings page instead of a support thread.


## The bigger picture


Our mission at Casco is to make all software effortlessly secure. Autonomous security testing only works if the findings are worth trusting, and[as we wrote when we launched Email Triage](https://casco.com/blog/introducing-casco-email-triage) , time spent on noise is time not spent reducing risk.


Context refinement is how the trust compounds: every test, every dismissal, and every feature your team ships makes the next result more accurate.


The system learns your application. Now you can watch it learn, and teach it yourself.


Get year-round security with autonomous security testing. Get started at[https://casco.com](https://casco.com/) .


Quick answers


## Frequently asked questions


What to know before connecting Casco to your coding agent.


###


###


###


###


###


###


###
