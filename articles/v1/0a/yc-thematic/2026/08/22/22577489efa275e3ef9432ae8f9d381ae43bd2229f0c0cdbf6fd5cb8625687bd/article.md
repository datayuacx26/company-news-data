---
schema_version: "1.0.0"
document_id: "22577489efa275e3ef9432ae8f9d381ae43bd2229f0c0cdbf6fd5cb8625687bd"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/sla-disaster-recovery-feedback-vendor"
published_at: null
first_seen_at: "2026-08-03T06:45:35.751002+00:00"
fetched_at: "2026-08-03T11:29:23.074207+00:00"
content_hash: "sha256:eebc90a850ae74c47263c85b58107886870b0f4fc4e35f3535a83be3f4d374bf"
---

# 10 SLA and Disaster Recovery Terms to Require From a Feedback Analytics Vendor

The security review is the part everyone remembers. Certifications get checked, a questionnaire comes back, InfoSec signs off, the deal moves. Then the platform goes down during the week your annual survey closes, and nobody ever asked what the vendor owes you when that happens.


Require ten things in writing before you sign: a service level agreement (SLA) scoped to a named component, a written definition of "down," capped exclusions, recovery objectives as numbers, evidence of tested restores, in-region recovery, disaster recovery for your theme taxonomy and not just your comments, a data freshness objective, defined incident communication, and a remedy that isn't only a service credit. The list below is built from National Institute of Standards and Technology (NIST), ISO, and AICPA source material rather than vendor marketing. The section near the end says plainly where Thematic itself does and doesn't meet these bars.


These are contract terms, not evaluation criteria. If you're still scoring platforms on analytical quality,[the nine requirements](https://getthematic.com/insights/customer-feedback-analytics-rfp-requirements) to score in a feedback analytics RFP come first. This is the redlines stage, after you've picked a winner.


## The list at a glance


1. **An availability commitment that names the component.** Dashboard, query API, and ingest pipeline are three different services.
2. **A written definition of "down," and the measurement window.** Monthly and annual measurement at the same percentage are different products.
3. **Exclusions that are capped.** Scheduled maintenance is the largest lever in any SLA.
4. **Recovery objectives as numbers.** And the maximum tolerable downtime they sit under.
5. **Evidence that restores are tested.** Not that backups exist.
6. **Recovery that stays in your region.** Failover can quietly cross a residency boundary.
7. **Protection for the taxonomy, not just the comments.** Rebuilding a theme model costs hours and human review.
8. **A freshness objective, not only an uptime objective.** A silent ingest failure looks like uptime.
9. **Incident notification and a post-incident report.** With a stated clock.
10. **A remedy beyond service credits.** Termination rights for chronic failure.


## 1. An availability commitment that names the component


A single uptime percentage covering "the service" tells you very little. A feedback analytics platform is at least three services with different failure consequences: the dashboard your executives open, the query API your data team calls, and the ingest pipeline that pulls last night's survey responses.


Ask which one the number covers. A vendor quoting one figure for all three is probably measuring only the dashboard, the easiest of the three to monitor. Get the scoping in writing at the account level, not as a deployment default that can change. Then convert the percentage into[permitted downtime](https://getthematic.com/insights/how-fast-ai-analyze-million-feedback) before you accept it. Each additional nine is a tenfold reduction in the minutes you're owed.


## 2. A written definition of "down," and the measurement window


Two vendors quoting the same percentage can be offering very different protection. Ask what counts as unavailable, who measures it, and over what window.


Degraded performance usually doesn't count. If queries take forty seconds instead of two but still return, most SLAs call that up. The vendor's own monitoring is normally authoritative, which you want to know before you rely on it. And annual measurement lets one long outage pass without breach, where the same percentage measured monthly caps every individual month. Monthly measurement is strictly better for you, and it costs a genuinely reliable vendor nothing.


## 3. Exclusions that are capped


Exclusions are where SLAs are really negotiated. Scheduled maintenance, force majeure, customer-caused outages, and third-party failures all typically sit outside the measured window.


Scheduled maintenance is the one that matters. If maintenance windows are excluded and uncapped, the commitment means whatever the vendor wants it to mean, because any planned window can run as long and as often as they choose. Require a cap on excluded maintenance hours per month, a minimum notice period, and windows that fall outside your business hours in every region you operate in.


## 4. Recovery time and recovery point objectives, as numbers


Recovery time objective (RTO) and recovery point objective (RPO) are the two numbers that describe what a disaster actually costs you.[NIST defines RTO](https://csrc.nist.gov/glossary/term/recovery_time_objective) as "the overall length of time an information system's components can be in the recovery phase before negatively impacting the organization's mission or mission/business processes." It defines RPO as "the point in time to which data must be recovered after an outage." In plain terms: how long until you're back, and how much data you lose.


Ask for both as numbers. Then ask the question that separates a real posture from a checkbox. NIST also defines maximum tolerable downtime, the outer bound an RTO has to sit under. A vendor with an RTO but no stated maximum tolerable downtime hasn't run a business impact analysis, and that analysis is precisely what a certified[ISO 22301](https://www.iso.org/standard/75106.html) business continuity program has to have completed.


Set your own tolerance first. UK Financial Conduct Authority[operational resilience rules](https://www.fca.org.uk/publications/policy-statements/ps21-3-building-operational-resilience) require regulated firms to identify their important business services, set their own impact tolerance, then test suppliers against it. Accepting the vendor's published RTO as your starting point is backwards.


## 5. Evidence that restores are tested


Every vendor takes backups. Far fewer can show you a restore.


Two control frameworks make the testing explicit. NIST SP 800-53 control CP-9(2) calls for restoration testing using a sample of backup information as part of contingency plan testing. The[SOC 2 Availability criterion A1.3](https://www.aicpa-cima.com/resources/download/get-description-criteria-for-your-organizations-soc-2-r-report) requires the vendor to test the recovery procedures supporting system recovery, and an untested plan draws an exception there. The evidence already sits inside the SOC 2 report, if the vendor has it.


So don't ask whether they have a disaster recovery plan. Ask for the date of the last successful restore test, what was restored, how long it took, and the section of the SOC 2 report where A1.3 was tested. That section tells you more than the cover page.


## 6. Recovery that stays in your region


If you picked a vendor partly for data residency, failover is where residency quietly breaks. A recovery that spins your data up in another region during an outage can be operationally sensible and contractually unacceptable at the same time, and it's rarely addressed in either the SLA or the data processing agreement.


Ask where backups are physically stored, where recovery infrastructure runs, and whether a regional failure means in-region recovery or cross-region failover. Then ask what the RTO becomes if recovery has to stay in-region, because that number is usually worse than the headline one. Thematic hosts on[AWS in three regions](https://getthematic.com/insights/customer-feedback-data-residency-eu) , US, EU-Frankfurt, and ANZ-Sydney, with an account active in exactly one region at a time. Any vendor with a similar footprint should be able to answer the same question.


## 7. Protection for the taxonomy, not just the comments


Nobody writes taxonomy protection into a generic SaaS SLA, and in feedback analytics it is one of the terms that matters most.


Your raw comments are replaceable. They live in your survey platform, your helpdesk, and your review sources, and you can re-ingest them. The theme taxonomy built on top of them is not: the categories your teams refined, the edits your analysts made, the mapping from themes to the metrics you report. Rebuild that from scratch and you're re-running analysis plus human review, then comparing this quarter against a taxonomy that no longer matches last quarter's.


Ask whether the taxonomy, theme definitions, model configuration, and dashboard definitions carry the same backup and recovery commitments as the underlying data. Ask for the RPO on that configuration separately from the RPO on comments. Most vendors have never been asked.


## 8. A freshness objective, not only an uptime objective


An uptime commitment measures whether the platform responds, not whether what it shows you is current.


A nightly ingest that fails silently produces a dashboard that's up, fast, fully available, and three days stale. Every SLA in the market would call that a perfect month. For a team making operational decisions off yesterday's feedback, that's worse than an hour of visible downtime, because at least downtime is obvious.


This isn't hypothetical. Atlassian processes 60,000 pieces of customer feedback every month and built a real-time pipeline on Thematic's API specifically so it could, in Mick Stapleton's words, "close loops where needed." A pipeline like that failing quietly is not a reporting inconvenience.


Require a data freshness objective: the maximum acceptable lag between a comment arriving at the source and appearing in the platform, with alerting when the lag is breached. Ask whether ingest failures alert you, or only the vendor.


## 9. Incident notification and a post-incident report


Most contracts say what the vendor must do during an incident and nothing about what they must tell you.


Require three clocks. First, initial notification: how fast you're told an incident is underway, and through what channel. A public status page is the low-effort version, so its absence tells you something. Second, updates at a stated cadence while the incident runs. Third, a written post-incident report within a defined number of business days, covering root cause, impact scope, and remediation.


Get support response targets in writing too, and treat them as separate from platform availability. Severity definitions from P1 to P4, an initial response target per severity, an escalation path with a named role, and coverage hours across your regions. A vendor can hit its uptime number and still take three days to answer a P1 ticket.


## 10. A remedy beyond service credits


Service credits are the standard remedy and close to meaningless on their own. They're commonly capped at a small percentage of one month's fees, claim-initiated so you forfeit them unless you file in time, and almost always named as your sole and exclusive remedy, which waives your right to pursue anything larger. A credit worth a fraction of one month's spend doesn't compensate a missed reporting cycle, and it gives the vendor no real financial reason to invest in resilience.


So don't ask for a bigger credit. Ask for a termination right triggered by chronic or sustained failure: a defined number of breaches in a rolling window, or any single outage beyond a stated duration, letting you exit without penalty and with your data returned. If your organization falls under the EU's[Digital Operational Resilience Act](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32022R2554) , this is closer to mandatory than optional. Article 30 requires documented exit strategies in contracts covering critical or important functions, so the provider has to keep delivering long enough for you to migrate.


## Where Thematic stands today


Thematic publishes a lot on security and very little on availability. Both halves belong here.


**What's published and verifiable.** Thematic holds[SOC 2 Type II certification](https://getthematic.com/insights/vendor-security-certifications) , audited annually by A-LIGN, with the scope covering the Security, Availability, and Confidentiality Trust Services Criteria and a reporting period closing 28 February each year. Availability being in scope matters for item 5, because A1.3 restore testing is examined there. Thematic encrypts data at rest with AES (FIPS-validated) and in transit with TLS 1.3. It runs annual third-party penetration testing on OWASP-based methodologies. Access logs are kept[immutable for at least 90 days](https://getthematic.com/insights/control-who-sees-customer-feedback) . Customer data is deleted within 30 days of termination using NIST 800-88 sanitization, with backups purged on their own rotation.


**What isn't published, and should be.** Thematic doesn't currently publish an availability commitment, a recovery time objective, a recovery point objective, or a public status page, and it doesn't list ISO 27001 or ISO 22301 certification. SLA and disaster recovery terms are handled in the contract rather than posted publicly. If you're evaluating Thematic, those are fair questions for the redlines, and you should put the same ones to every vendor on your shortlist.


One clarification that applies across the market, not just here. SOC 2 with Availability in scope and a stated uptime commitment are two separate asks. The Availability criterion tests whether a vendor met the commitments it made, and mandates no minimum percentage, so a vendor committing 95% and a vendor committing 99.99% can both pass. Availability is also elective, where Security is the only mandatory SOC 2 category. Confirm it's in scope rather than assuming the badge covers it.


## How to turn these into contract language


1. **Ask which document each term lives in.** Availability belongs in an SLA exhibit, recovery objectives in a disaster recovery addendum, data return in the master agreement. A vendor who can't say has none of them.
2. **Ask for the SOC 2 report, not the logo.** Check the categories in scope, the report period end date, and the A1.3 testing section.
3. **Ask when the report period ended.** If it closed more than a few months ago you'll be offered a bridge letter, and bridge letters come from the vendor's own management rather than the auditor. Treat one as a self-assertion.
4. **Ask what the cloud provider covers and what the vendor covers.** "We run on a major cloud" is a non-answer. Failover architecture is the vendor's choice, and their SLA is the only one you have privity to.
5. **Ask for the last restore test and the last incident post-mortem.** Under an NDA if needed. Willingness to share is itself the signal.


## The short version


Require an availability commitment scoped to a named component, a written definition of down, capped exclusions, RTO and RPO as numbers under a stated maximum tolerable downtime, evidence of tested restores, in-region recovery, disaster recovery coverage for your taxonomy as well as your comments, a freshness objective, defined incident communication, and a termination right for chronic failure. Thematic publishes SOC 2 Type II with Availability in scope and doesn't currently publish an availability commitment or recovery objectives. Ask any vendor, Thematic included, for the date of the last successful restore test. That answer takes one email and tells you more than the whole questionnaire.
