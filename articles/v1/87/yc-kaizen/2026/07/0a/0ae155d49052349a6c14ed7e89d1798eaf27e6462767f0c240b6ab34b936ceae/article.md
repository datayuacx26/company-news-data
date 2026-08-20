---
schema_version: "1.0.0"
document_id: "0ae155d49052349a6c14ed7e89d1798eaf27e6462767f0c240b6ab34b936ceae"
company_key: "yc-kaizen"
company: "Kaizen"
source_id: "yc-kaizen-news-import-1021911e948a"
canonical_url: "https://www.kaizenautomation.com/blog/credentialing-process"
published_at: null
first_seen_at: "2026-07-22T01:12:15.212824+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:f61ad1c14481fd40a480227afa92af5518a0f6da3b8af16c1b31887df7de6464"
---

# The Credentialing Process: Step-by-Step Guide for 2026

The average credentialing process runs 90 to 120 days, and during that window, a physician generating $2.3 million annually loses around **$122,000 in revenue** waiting for the process to complete.


## The credentialing process: Step by step


Each step of the credentialing process depends on **third parties, external databases, and approval windows** that no one controls. Here's what happens at each stage.


### Step 1: Application and document collection


The first step pulls from **more sources than most teams expect** : licenses, malpractice history, CAQH, board certifications, DEA registration, employment history, and professional references. What slows teams down is what arrives.


Documents show up **incomplete or outdated** , and you don't find out until a payer kicks the application back weeks later. The same provider data appears differently across sources (a name on a license won't always match the malpractice certificate), and **those discrepancies flag during verification** .


Employment gaps without explanation hold up the committee review.[80% of U.S. physicians now have a CAQH profile](https://www.caqh.org/hubfs/Credentialing%20Suite%20Brochure_2024.pdf) , and most major commercial payers pull from it as their primary credentialing source. **If that profile is stale or unattested** when you start, every commercial payer enrollment gets delayed before it begins.


### Step 2: Primary source verification (PSV)


PSV is where timelines stall. **Every credential goes back to the source:** medical school, residency program, certifying board, state licensing boards, DEA, malpractice carriers, and past employers. Most of these **don't offer a unified API** ; verification often means **web portals, faxes, or phone calls** .


You're submitting requests and waiting on **third parties who have no obligation to move fast.** Some licensing boards respond within days, while others take weeks. Teams that **check in during the first week** move faster than teams that wait a month.


### Step 3: OIG and exclusion screening


**Billing Medicare or Medicaid with an excluded provider** triggers claim denials, repayment demands, and compliance investigations. Run these checks before a single claim goes out.


That means checking the provider against:


- The OIG exclusions database (LEIE)
- The SAM.gov exclusions database
- State Medicaid exclusion lists


Before July 2025, most organizations ran exclusion checks every six months or at recredentialing.[The NCQA now requires](https://wpcdn.ncqa.org/www-prod/2025-HPA-Policy-Updates_7-28-25.pdf) **monthly exclusion monitoring** for all providers, for every credentialing file processed on or after July 1, 2025.


Miss a single monthly check and **your organization is out of compliance** . If your team still runs these manually, you just inherited a new recurring task that didn't exist last year.


### Step 4: Committee review and approval


After finishing PSV, the file goes to committee. Committees typically meet **monthly or quarterly** , so if your file arrives two days after the cutoff, you're waiting 30 more days.


They're reviewing **completeness, malpractice history, state board actions, and peer references** . One flag in any of those areas and the clock resets.


### Step 5: Privileging (for hospital-based providers)


Getting credentialed doesn't mean your provider can walk into a hospital and see patients. That requires **privileging: a separate application, criteria, and approval** at every facility they'll work in.


A provider joining a multi-hospital system with separate CMS Certification Numbers may need to complete privileging at each entity. The Joint Commission allows **organizations operating under one CCN to privilege a provider once** and apply it across locations, but if your facilities are separately certified, expect **separate paperwork and approval timelines** for each one.


### Step 6: Payer enrollment


Credentialing proves your provider is qualified. Payer enrollment is **the part that actually pays your bills** . After credentialing approval, submit enrollment applications to each insurance plan the provider will bill: **commercial payers** (United, Aetna, BCBS, Cigna, Humana) and **government payers** (Medicare, Medicaid).


What payer enrollment involves:


- Submit Medicare enrollment via Form CMS-855 through PECOS, the CMS portal; no third-party API access, and every change is keyed in by hand.
- Enroll in Medicaid separately for each applicable state, since there's no centralized submission process.
- Attest your CAQH ProView profile before submitting commercial payer applications, as most payers draw from it directly.
- Link the provider to the group practice and tax ID, execute the in-network contract, and configure EDI and ERA enrollment before claims can be processed.


Payer enrollment takes **60-120 days per payer** after credentialing approval, and most providers need to enroll with multiple plans. Process them one at a time, and your fully credentialed provider still can't bill a single claim for months.


### Step 7: Ongoing monitoring and recredentialing


Ongoing monitoring now requires:


- Monthly OIG/SAM exclusion checks, required by NCQA for all files processed on or after July 1, 2025
- License expiration tracking and renewal alerts
- Malpractice coverage renewal verification
- DEA and controlled substance certificate renewals
- Board certification maintenance tracking


**Recredentialing windows have also been shortened.** NCQA cut verification timelines from 180 days to **120 days for accredited organizations** , and from 120 days to **90 days for certified organizations** .


That's a **33% reduction in available time** , with monthly monitoring requirements added on top. Manual workflows now have to fit into a tighter window while handling additional monthly checks.


## Why the credentialing process takes so long


The **average credentialing timeline runs 90-120 days** . Some providers wait up to 180 days before seeing a single patient. During that window, a physician generating roughly $2.3 million annually can **lose around $122,000 in revenue** waiting for the process to complete.


Your organization eats that loss; the payer doesn't lose a dollar. **Most delays come from three operational bottlenecks your team controls:**


**1. Applications submitted with errors:** More than half of medical practices[reported credentialing-related claim denials on the rise](https://www.mgma.com/mgma-stat/more-than-half-of-practices-report-credentialing-related-denials-on-the-rise-in-2021) , with incomplete applications and documentation mismatches as the leading causes. Each error restarts the clock.


**2. Manual verification with no shortcuts:** Primary source verification means contacting medical schools, licensing boards, and certification bodies directly. There's no API for most of these. Someone on your team is literally picking up the phone.


**3. Sequential processing:** Teams hold payer enrollment until credentialing is complete. While PSV responses and committee reviews drag on, enrollment work hasn't started. Credentialing teams spend **20+ hours per application** chasing documents, verifying sources, and following up across portals.


## The real cost of running this process manually


Running credentialing by hand is expensive in ways that don't show up until your CFO starts asking questions.


Cost type What it actually costs


Staff time per application 20+ hours of credentialing specialist time per provider, per credentialing event


Specialist salary $42,000-$65,000/year for an in-house credentialing specialist (or $53,000-$85,000 fully loaded with benefits)


Revenue lost per physician Up to $122,000 during a 120-day credentialing delay


Monthly revenue leak $6,000-$8,000 per provider per month


Rework cost per denied claim $25-$181 to rework or appeal


Total US annual admin spend $83 billion on healthcare administrative transactions annually


For a digital health company onboarding 20 providers a year, a 90-day average credentialing timeline means **up to $122,000 in lost revenue per provider** while applications clear. Across 20 providers, that's **up to $2.4 million in delayed revenue** , much of which magnifies further if applications are sent back for errors or sit waiting for follow-up.


For a traditional practice, the math lands differently. You're paying experienced staff **a full salary to log into portals and chase paperwork,** which generates no revenue and pulls them off tasks that do.


## Where automation changes your credentialing process


Most credentialing software tracks status and stores documents. Someone on your team still has to do everything else. At[Kaizen](https://www.kaizenautomation.com/) , we run your **CAQH and payer portal workflows automatically** , moving providers from signed to billing without your team logging into a single portal for routine work.


We **continuously monitor application status** , catching stalled submissions before they result in denials. We handle exclusion checks, attestations, and data entry on schedule, so your **credentialing staff spends their time on exceptions** instead of execution.


> [Book a call](https://calendly.com/) to see how it works against your current volume.


## Frequently asked questions


### What is primary source verification in credentialing?


Primary source verification (PSV) is the process of **confirming a provider's credentials** directly with the organization that issued them: the medical school, licensing board, certification body, or past employer. You can't rely on documents the provider supplies; **each credential has to be confirmed at the source.**


### What is CAQH, and do all providers need it?


CAQH is the **centralized database that many commercial payers use** for credentialing and recredentialing data. Most providers working with commercial insurers need a **complete, attested CAQH ProView profile.**


### What is the difference between credentialing and payer enrollment?


The main difference between credentialing and payer enrollment is that credentialing verifies a **provider's qualifications** , while payer enrollment registers that provider with each **insurance plan so they can bill for services** .


### Can the credentialing process be automated?


Yes, parts of the credentialing process can be automated. That includes **portal-based tasks** like submitting applications, updating CAQH profiles, checking enrollment status, and running monthly exclusion monitoring. Those are the **repeatable browser workflows Kaizen is built to handle** . Review steps that require judgment still need a real person.
