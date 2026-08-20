---
schema_version: "1.0.0"
document_id: "e9c7d7a2682e677fe07d8431b5982c24f2d61846db97257b1ddb4d0e23a57154"
company_key: "yc-verbiflow"
company: "Verbiflow"
source_id: "yc-verbiflow-news-import-392b5e1f9020"
canonical_url: "https://verbiflow.com/blog/trust-atlas-yc-compliance-gaps"
published_at: "2026-06-01T00:00:00+00:00"
first_seen_at: "2026-07-22T18:39:30.762214+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:dfec822bf88e4a1aa01d5f084f47d5c86a480c00c243a1be6d0b2248ad9f2006"
---

# Trust Atlas: how we read every YC company's trust page to find compliance gaps

A customer selling SOC 2 / ISO 27001 tooling had the same problem every compliance vendor has: their best buyer is the company that needs the cert but doesn’t have it yet, and that company is invisible until you actually go check. So we built a pipeline that reads the trust and security pages of every company in a YC batch and tells you exactly which ones are still missing what.


~250 / batch


YC companies across the last few summer + winter batches


4 stages


seed → fetch → extract → score


22 cert types


SOC 2, ISO 27001, HIPAA, FedRAMP, PCI DSS, GDPR, and more


## Why this play exists


Compliance tooling has a sharp ICP. The buyer is a Series A / Series B startup that recently started selling into enterprise, hit their first SOC 2 question on a procurement questionnaire, and realized they don’t have one. They’re shopping this quarter whether they admit it or not.


The problem is the data. There’s no list of companies that don’t have SOC 2. Vendor databases don’t track it. The only honest source is the company’s own trust page, which is public. Read enough of them and the answer is right there.


## The pipeline


- **Seed the list.** Pull every company in the most recent YC batches (~250 each). Resolve their domain.
- **Find the trust page.** Search for “{company} trust security compliance”, score the results, and pick the best hit. Trust subdomains and /security paths get priority.
- **Extract certifications.** Fetch the page as markdown. Ask Claude (typed structured output) which certs are listed, which are “in progress,” and which evidence backs each claim.
- **Score the gap.** For each company, compute which of the 22 standard certs they have, which they’ve started, and which they’re missing entirely. Rank by “missing-cert priority” for the customer’s ICP.


The honest caveat


Some companies don’t have a trust page at all. If a startup hasn’t even put up a page saying which certs they’re working on, they almost certainly haven’t prioritized compliance. Those go to the top of the list.


## What the customer actually got


- A ranked list of YC companies, by cert gap, refreshed quarterly.
- The right person to email (head of engineering, head of security, founder) for each company.
- Per-company copy that referenced their actual posture. Not “do you need SOC 2?” but “noticed your trust page lists SOC 2 in-progress as of last quarter and your peers are landing it in 90 days now.”


Reply rate on the gap-ranked segment was multiples of what generic SOC-2 vendor cold email gets, because the email proved we’d actually read their trust page.


> If your enrichment can’t see the gap, your sequence reads like every other compliance pitch in their inbox.


## What this is a template for


Any “you need this thing and you don’t have it yet” pitch where the absence is detectable on a public page:


- Companies without a privacy policy that mentions GDPR.
- SaaS companies whose status page shows recent incidents but no public RCA.
- Companies hiring a security engineer but no head of security yet.
- Companies whose API docs are missing rate-limit guidance.


Same pipeline shape: find the cohort, fetch the public artifact, extract the signal, rank by gap. The play handles qualification so Verbiflow can focus on sending.
