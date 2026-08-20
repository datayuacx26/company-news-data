---
schema_version: "1.0.0"
document_id: "a727e16c1d2ab5fd42b90803b318ec7a1a036ef51de2078f9cc75254571f98fd"
company_key: "yc-substrate"
company: "Substrate"
source_id: "yc-substrate-news-import-ed25994f1cce"
canonical_url: "https://www.substrateai.com/blog/how-heading-health-eliminated-manual-ar-follow-up-with-ai-claim-status-agents"
published_at: "2026-03-01T00:00:00+00:00"
first_seen_at: "2026-07-24T02:38:43.425400+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:b95d42b1357baf3ea2ce0f7baed4f185efd314740b7c40ef3de93392f23cbcd0"
---

# How Heading Health Eliminated Manual AR Follow-Up with AI Claim Status Agents

## The Challenge


Every billing team in America starts their day the same way: logging into payer portals to check the status of outstanding claims. Heading Health’s billing team was no different.


For a behavioral health practice billing across Aetna, UHC, BCBS, Cigna, Tricare, and others, this consumed hours of staff time daily. Each payer has different portal requirements, different login credentials, and different ways of presenting claim information. A single biller might check five or six portals before lunch just to figure out what's happening with their outstanding AR.


The industry's default tool for claim status, the 276/277 EDI transaction is phenomenal but is just a start:


- **Every input must be perfect.** The payer ID, NPI, member ID format; get one of these wrong and you don't get a response. For practices where the rendering provider NPI and billing provider NPI are often different, this is a constant source of failed lookups.


- **Data quality is inconsistent.** Even when inputs are right, sometimes all you get back is acknowledgement that the payer knows about the claim.


- **Limited payer coverage.** Only about 500 payers support the 276/277 transaction. Compare that to roughly 1,200 payers for eligibility checks and 2,800 for claim submission.


The result is that failed claim statuses erode trust in automation. Billers manually status claims by calling payers and logging into portals, and can spend up to 15-20 minutes per claim on work that should take seconds.


> *"Substrate eliminated the daily portal-checking for our billing team. The agent follows up on no-response claims automatically, resolves an overwhelming amount of medical-records-pending claims, and lets my staff focus on denials and collections instead of status hunting. We’ve saved a significant amount of staff time every month.” ‍ Chimene Perez - Director of Operations, Heading Health*


‍


## Solution


Substrate's Claim Status Agent replaced Heading Health's manual AR follow-up process. When a claim is submitted for status lookup, the agent doesn't just try once and give up. It works through recovery strategies based on what's failed before, per payer, per practice, per state or for claims just like this one.


### Before and after


### Three operating principles


The agent operates on three principles that mirror how an experienced biller thinks, but at machine speed and scale:


**1. Try the smart thing first.** Before making any request, the agent checks what's worked for similar claims. If billing NPI succeeded for Cigna claims last week, it tries billing NPI first, not rendering NPI. If a particular payer ID worked for BCBS claims from the same CSV import, the agent uses that payer ID for sibling claims. This "Learned Strategies" system means the agent gets faster and more accurate with every claim it processes.


**2. When something fails, understand why and iterate.** Different errors mean different things. "Entity not found" might suggest an NPI or tax ID issue. "Subscriber not found" could mean the payer route is wrong and the agent should try a different clearinghouse ID. "Unable to respond" suggests the payer is down and the agent should retry later. The agent adapts its next attempt based on these signals rather than blindly retrying the same inputs.


**3. Learn from every success.** When a combination of NPI, payer ID, and demographics works, the agent stores that pattern. This knowledge compounds over time.


‍


> *“This is the first automation I’ve used that actually works like an experienced AR rep. It learns payer behavior, retries correctly, and gets claim statuses even when records are required. Our outstanding claims are getting resolved faster and our AR follow-up process is finally consistent.” Chimene Perez - Director of Operations, Heading Health*


‍


‍
