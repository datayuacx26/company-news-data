---
schema_version: "1.0.0"
document_id: "9c2667f3641bd7963fe16ea9cb4a8d0744d4ce71f45179a225a24dcd307340e7"
company_key: "yc-superunit"
company: "Superunit"
source_id: "yc-superunit-news-import-57b6d5b65af6"
canonical_url: "https://www.superunit.com/blog/what-happens-when-the-work-number-cant-verify-your-applicant"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-24T02:52:52.663509+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:8f1ad6aa2e3ecb19aaab75ebc9e54e8a07b54d6631b040511fc577e595a81b6a"
---

# What Happens When The Work Number Can't Verify Your Applicant?

## Introduction


The Work Number fails more often than most HR and lending ops expect.[Equifax's employment database](https://theworknumber.com/) covers roughly 40% of U.S. employers, leaving a massive gap that catches teams off-guard when their go-to verification source returns "no record found."


Most background screening and mortgage operations treat TWN as their primary verification method — which makes sense, given its speed and automation. But when it fails, the majority of teams lack a defined next step beyond "figure it out manually." This ad hoc approach creates delays, compliance risks, and operational bottlenecks that compound with every failed lookup.


The solution isn't to abandon TWN but to build a structured waterfall that activates when it can't deliver. Smart operations map out the full sequence from database lookup to manual employer outreach, treating each stage as a defined process rather than a scramble. When TWN returns empty, your waterfall should already be running.


## Why The Work Number Can't Always Verify


The Work Number operates as a contributor database — it can only verify employment records that employers actively report to[Equifax](https://www.equifax.com/business/the-work-number/) . If a company doesn't submit payroll data to the system, no record exists to return. This isn't a technical glitch or verification failure; it's a fundamental coverage limitation.


Roughly[60% of employers are not in The Work Number database](https://www.equifax.com/business/the-work-number/) , creating predictable verification gaps across mortgage lending and background screening workflows. Small businesses, regional employers, and companies using local payroll providers rarely contribute to the system. Government agencies and non-profits also frequently operate outside TWN's coverage network.


Three distinct failure modes trigger different response strategies. A "no record found" result for a Fortune 500 company signals a potential fraud risk worth investigating. The same result for a 15-person construction firm is standard and requires immediate fallback to manual verification methods.


### No Record Found


The employer simply doesn't contribute payroll data to The Work Number. This affects most small and mid-size businesses, which represent the majority of American employers but a minority of TWN's database coverage. The result is not a fraud indicator — it's a coverage gap requiring immediate escalation to written or manual verification methods.


### Incomplete or Stale Record


The employer contributes to TWN but the returned data is partial, outdated, or missing the specific employment period you need verified. Large employers sometimes submit quarterly rather than real-time updates, creating lag periods where recent employment changes don't appear in search results.


### Not-Yet-Started Employment


New hires whose employment data hasn't been uploaded to The Work Number yet. Most employers submit payroll data 30-90 days after hire, making fresh employment relationships invisible to TWN searches even when the employer is a regular contributor.


## The Fallback Waterfall When TWN Comes Up Empty


When The Work Number fails, your next move determines whether verification completes in 24 hours or stalls for weeks. The fallback waterfall isn't random scrambling — it's a structured sequence where each stage has clear trigger conditions and exit criteria.


Stage 1 triggers when TWN returns "no record found" or incomplete data. Stage 2 activates when written requests go unanswered after 48-72 hours. Stage 3 handles edge cases where standard outreach methods fail entirely. Each transition point has measurable criteria, not subjective judgment calls.


### Step 1 — Written VOE / Direct Employer Contact


Your first fallback is the formal[written VOE](https://www.superunit.com/blog/written-voe-for-mortgage-lenders) request sent directly to the employer's HR department. This bypasses TWN entirely and goes straight to the source — the employer who holds the definitive employment record.


Use written VOE when TWN returns "no record found" or when the record exists but lacks critical details like employment dates or salary history. Written requests work particularly well with mid-size to large employers who have dedicated HR teams and established VOE procedures.


The written VOE produces a signed verification on official employer letterhead, which satisfies most underwriting and background screening requirements. Response time varies dramatically by employer — some HR departments turn around written requests within 24-48 hours, while others take 5-10 business days.


For mortgage applications, written VOE is standard practice when automated verification fails. The challenge is timing: you need the employer response before the loan closing date. For background screening, written VOE provides the documented trail that many compliance frameworks require, especially for roles involving financial responsibility or security clearances.


The weakness of written VOE is the passive wait. You send the request and hope the employer responds. When they don't — which happens in roughly 40% of cases — you move to active manual outreach.


### Step 1.5 — Borrower-Assisted Verification (Direct Payroll Connection)


Before escalating to active manual outreach, some lenders insert a borrower-assisted step: ask the applicant to connect their payroll account directly. Platforms like[Argyle](https://www.argyle.com/) and Pinwheel pull employment and income data straight from the employer's payroll system — bypassing TWN entirely and returning real-time records within minutes.


This works well when the employer uses a major payroll provider (ADP, Gusto, Paychex, Workday) and the borrower is willing to authenticate. Coverage is strong for mid-size and enterprise employers but thinner for small businesses, government employers, and anyone on a proprietary payroll system — which overlaps significantly with the cases where TWN already failed.


Use it as an opportunistic parallel step, not a guaranteed fallback. If the borrower can connect and the payroll provider is supported, you may resolve the verification before manual outreach is needed. If not, you move to Step 2.


### Step 2 — Manual Employer Outreach (Phone, Email, Fax), the Stage Superunit Automates


When a written VOE request goes unanswered, the waterfall advances to active multi-channel outreach. This is the highest-friction stage where most verification teams stall — but it's also where structured, parallel contact attempts can produce results in about one business day.


The key is simultaneous execution across phone, email, and fax rather than sequential attempts. Phone calls during business hours target the employer's HR department, payroll team, or the applicant's direct manager depending on company size. Email provides a paper trail and allows HR staff to respond outside business hours. Fax reaches employers who still operate on legacy systems or have compliance requirements around written documentation.


#### Target identification matters more than volume


Small employers (under 50 employees) often route verification requests directly to the hiring manager or owner. Mid-size companies typically have a dedicated HR contact who handles all employment verifications. Large corporations usually maintain a centralized verification hotline or third-party service, but manual outreach can still reach department-level contacts when the centralized system fails.


Contact information comes from the employer's website, public business directories, or the applicant themselves. The applicant can usually provide their direct manager's contact or the main HR number, which accelerates the process significantly.


When outreach is structured and executed in parallel across all three channels, response rates exceed 80% within one business day.[Superunit automates this entire manual outreach stage](https://www.superunit.com/) , handling the simultaneous phone, email, and fax attempts while maintaining full audit trails for compliance purposes.


Most verification teams avoid this stage because it requires dedicated staff time and follow-up coordination. But for[manual employment verification](https://www.superunit.com/blog/manual-employment-verification-for-background-screening) and[calling HR departments](https://www.superunit.com/blog/calling-hr-departments-for-employment-verifications) , AI automation transforms the highest-friction waterfall stage into a predictable, scalable process.


### Step 3 — Human Escalation


Human escalation kicks in when standard manual outreach fails completely. This is your last resort before marking the verification as impossible to complete.


Three scenarios trigger escalation: the employer remains unresponsive after multiple phone, email, and fax attempts spanning 3-5 business days; the contact information you have is demonstrably wrong (disconnected numbers, bounced emails, returned mail); or the employment situation requires human judgment that automated workflows can't handle.


The judgment scenarios include defunct companies that have closed but haven't been formally dissolved,[self-employed borrowers](https://www.consumerfinance.gov/ask-cfpb/how-do-i-verify-income-if-im-self-employed-en-684/) claiming W-2 employment, contractors misclassified as employees, or family businesses where the applicant works for relatives. These edge cases need experienced verification specialists who can navigate unusual employment structures and make defensible decisions about what constitutes valid verification.


At this stage, you're no longer following a standard process. Your verification team must document all previous attempts, research alternative verification methods specific to the situation, and often coordinate directly with underwriters to determine acceptable alternative documentation.


## What This Means for Mortgage Lenders Specifically


TWN failure hits mortgage lenders harder because you're racing against closing deadlines. When The Work Number returns "no record found" for a borrower who just started a new job or works for a small employer, you have maybe 7-10 business days to complete the verification waterfall before the loan falls through.


Small employers and new hires create the highest TWN failure rates in mortgage. The borrower who switched jobs two weeks before applying for a loan won't show up in TWN's database yet — their payroll data hasn't been uploaded or processed. Same goes for borrowers employed by local businesses, nonprofits, or any employer that doesn't contribute payroll data to Equifax.


Your written VOE and manual outreach stages must complete within the pre-closing window, which means parallel execution rather than sequential attempts. Send the written VOE immediately when TWN fails, then launch phone and email outreach to the employer's HR department simultaneously. Time is the constraint, not verification difficulty.[Verbal VOE completion](https://www.superunit.com/blog/verbal-voe-for-mortgage-process-compliance-and-turnaround) becomes the critical path item on your closing checklist.


## Frequently Asked Questions


**Q: What does "no record found" on The Work Number mean?**


"No record found" means the employer doesn't contribute payroll data to Equifax's database. This isn't a fraud signal or verification failure — it's a coverage gap. About 60% of employers are not TWN contributors, especially smaller companies, nonprofits, and government agencies.


**Q: How long does it take to verify employment manually if TWN fails?**


Written VOE requests typically take 3-7 business days if the employer responds. Manual outreach via phone, email, and fax can complete within 1 business day when executed simultaneously. Human escalation adds another 2-5 days depending on the complexity.


**Q: Can a lender close a loan if The Work Number can't verify employment?**


Yes, but you need alternative verification before closing. GSE guidelines accept written VOE, verbal VOE, or paystubs with bank statements. The key is completing the waterfall process within your pre-closing timeline.


**Q: What's the difference between a written VOE and a verbal VOE?**


Written VOE is a formal document on employer letterhead confirming employment details. Verbal VOE is phone verification directly with HR or payroll, documented in your loan file. Both satisfy mortgage compliance requirements.


**Q: Does The Work Number cover all employers?**


No. TWN only covers employers who actively contribute payroll data to Equifax. Large corporations typically participate, but most small-to-medium employers, government agencies, and nonprofits do not report to the system.


## Conclusion: Where Superunit Fits in the Fallback Waterfall


TWN failure isn't a dead end — it's a trigger. The teams that handle it well aren't doing anything heroic; they're just running a defined sequence instead of improvising. Borrower-assisted payroll connections catch some cases fast. Written VOE handles most of the rest. Manual outreach resolves the hard ones. Human escalation handles the edge cases that genuinely require judgment.


The stage that breaks most operations is Step 2 — the active multi-channel outreach to employers who haven't responded. That's where[Superunit](https://www.superunit.com/) operates: AI agents running simultaneous phone, email, and fax attempts at scale, with full audit trails, so verification teams aren't manually chasing employers one call at a time.
