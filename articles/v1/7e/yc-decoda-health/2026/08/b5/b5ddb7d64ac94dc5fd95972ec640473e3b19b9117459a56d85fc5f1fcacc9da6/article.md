---
schema_version: "1.0.0"
document_id: "b5ddb7d64ac94dc5fd95972ec640473e3b19b9117459a56d85fc5f1fcacc9da6"
company_key: "yc-decoda-health"
company: "Decoda Health"
source_id: "yc-decoda-health-news-import-41de34412c0a"
canonical_url: "https://decodahealth.com/blog/ftc-health-data-rules-elective-care-practices"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-13T21:12:30.099457+00:00"
fetched_at: "2026-08-13T21:12:31.278782+00:00"
content_hash: "sha256:cb3774f93df5cdcb66f32a2e4662ff990260472875f0d7cf5256d7888b3bc61d"
---

# FTC Health Data Rules for Med Spas and Telehealth (2026)

## The FTC vs. Hims & Hers: What Actually Happened


On July 29, 2026, the FTC filed suit against Hims & Hers in the U.S. District Court for the Northern District of California, joined by the states of California and Utah. The complaint alleges the telehealth company promised discretion to roughly 2.5 million subscribers while third-party tracking pixels and SDKs on its site transmitted user interactions with health-related content to Meta, Snap, and other advertising platforms. A second set of claims concerns subscription billing and cancellation.


Three details matter for how you read this:


- **It is a lawsuit, not a settlement.** The FTC opened its probe in 2023, communicated findings to the company in April 2026, and entered settlement talks that did not resolve. No court has made any findings, and the allegations remain allegations.
- **The claims run on Section 5 of the FTC Act** — unfair or deceptive practices — along with the Restore Online Shoppers' Confidence Act for the billing conduct. The Health Breach Notification Rule is not the vehicle here.
- **The cost lands before any verdict does.** Hims disclosed a $15.0 million accrual for estimated probable losses as of March 31, 2026, then recorded $47.5 million in legal contingency charges in Q2, roughly $62.5 million across the first half of 2026.


That last point is the practical one. Defending a data-practices inquiry is expensive whether or not the agency ultimately prevails.


## This Is Not an Isolated Case


The FTC's 2023 action against GoodRx was the agency's first enforcement under the Health Breach Notification Rule, and it established the core idea: sharing health-related data with ad platforms can be actionable on its own terms, with no breach required. Since then the agency has worked the same theory across telehealth and consumer health apps, increasingly through Section 5 rather than the Rule itself. For elective-care owners, the through-line is simpler than the statutes: if your site collects health-related signals and hands them to advertisers while your privacy policy implies otherwise, the gap between those two facts is the exposure.


## Why Med Spas and Telehealth Clinics Are in the Crosshairs


[Med spas](https://decodahealth.com/who-we-serve/med-spa) and telehealth clinics sit at an uncomfortable intersection: they collect sensitive health data, and they spend heavily on digital advertising. That combination has drawn direct FTC scrutiny. These practices routinely feed patient data into ad networks through tracking pixels, use browsing behavior to retarget prospective patients, and share appointment or condition data with third-party vendors. They also make outcome claims in their advertising, which is a second, separate line of FTC exposure most owners never connect to the first. And they tend to run without dedicated compliance staff, so the person who installed the pixel is often the person who wrote the privacy policy — two decisions made months apart that a regulator will read side by side.


## What to Do Before the FTC Comes Calling


The enforcement pattern is clear. What follows is a compliance review any practice can run before a regulator runs it for you.


- Audit every third-party script on your website and patient-facing pages. If a pixel fires during booking or intake, document exactly what data it captures and where it goes.
- Settle your covered-entity status in writing before anything else, because it determines which of the rules below actually bind you.
- If you are a covered entity, confirm which vendors have signed Business Associate Agreements. Most ad-tech vendors will refuse, and a refusal is itself the answer about whether that data should be flowing to them.
- Compare your privacy policy to your actual data practices. The FTC targets aspirational language that doesn't match real behavior, and outright deception is far from the only trigger.
- Obtain written, HIPAA-compliant authorization for every patient photo or testimonial before it appears in any ad or post.
- Build a substantiation file for every outcome claim before the ad runs, not after a complaint arrives.
- Train your marketing team on FTC disclosure requirements. Staff-generated content and influencer posts both count, even when unpaid.


The goal is defensible marketing, not less marketing.


## How Tracking Pixels Become a Patient Data Problem


When a prospective patient visits your med spa website and browses a treatment page, the Meta Pixel or Google Tag embedded in your site captures that visit and ties it to their identity. If that person later books a consultation, you've effectively disclosed a health-seeking behavior to a third-party advertiser without their explicit consent.


Whether that is actionable, and under which law, is where practices get it wrong. In June 2024, a federal court in *American Hospital Association v. Becerra* vacated the part of HHS's tracking-technology guidance that treated an IP address plus a visit to an unauthenticated public webpage about a health condition as protected health information. HHS withdrew its appeal that August. So the simplest version of this theory — a pixel on your public treatment page automatically creates a HIPAA violation — is not the law. What the court left standing is everything else: tracking on logged-in patient portals, and other combinations of identifiable health information on public pages.


That is why the FTC's route matters more than the HIPAA one for most elective-care practices. Section 5 does not ask whether the data was PHI. It asks whether you told patients one thing and did another. For telehealth clinics running retargeting campaigns, that gap is usually invisible until someone reads the privacy policy against the network tab.


## First Answer This: Are You Even a HIPAA Covered Entity?


Most compliance advice written for med spas skips this question, and it decides everything that follows. A health care provider is a HIPAA covered entity only if it transmits health information electronically in connection with a HIPAA covered transaction — insurance claims, eligibility checks, and the like. A purely cash-pay med spa that never bills a payer may not be a covered entity at all.


Practices tend to guess wrong in both directions, and both errors are expensive:


- **If you are a covered entity** — you bill insurance for any part of your services — HIPAA governs, and the FTC Act applies on top of it. The[Health Breach Notification Rule](https://www.ftc.gov/business-guidance/resources/complying-ftcs-health-breach-notification-rule-0) does *not* apply to you. It was written for the businesses HIPAA misses, and it expressly excludes covered entities and their business associates acting as such.
- **If you are not a covered entity** — cash-pay only, no covered transactions — HIPAA may not reach you at all. That is not the relief it sounds like. It means the FTC Act is your primary federal exposure, plus state health-privacy law, and you lose the "we're HIPAA compliant" answer that owners reach for first.


The trap is assuming a HIPAA compliance program is a ceiling. Section 5 of the FTC Act prohibits unfair or deceptive practices whether or not the data is technically protected health information, and it applies to both groups above. A practice can be fully HIPAA compliant and still be sued for telling patients its site was discreet while a pixel said otherwise. That is the Hims theory in one sentence.


HIPAA


FTC Act, Section 5


**Who it covers**


Covered entities and their Business Associates (e.g., EHR vendors, billing services)


Essentially every business, covered entity or not. Being HIPAA compliant is not a defense


**What triggers a violation**


Unauthorized use or disclosure of Protected Health Information (PHI)


Saying one thing about your data practices and doing another; unfair practices causing substantial consumer injury


**Does pixel tracking apply?**


Partly. A 2024 ruling vacated the IP-address-plus-public-page theory; portals and other identifiable combinations still count


Yes, when your public promises do not match what the pixel actually sends. No breach required


**Key enforcement example**


Data breaches, missing BAAs, improper record access


[GoodRx (2023)](https://www.ftc.gov/news-events/news/press-releases/2023/02/ftc-enforcement-action-bar-goodrx-sharing-consumers-sensitive-health-info-advertising) , the FTC's first Health Breach Notification Rule action; *FTC v. Hims & Hers* (filed July 2026, unresolved)


**Remediation required**


HIPAA-specific: update BAAs, retrain staff, breach notification


Separate: change the practice or change the promise, plus penalties and possible parallel state claims


## FTC Advertising Rules Every Practice Must Know


The FTC's advertising rules hit med spas and telehealth clinics on two fronts: endorsements and health claims. Under the updated Endorsement Guides, every paid testimonial or influencer post must carry a clear disclosure, and before-and-after photos used in ads must reflect typical results, not outliers. Practices that let patients post glowing reviews in exchange for discounts are already in violation.


On health claims, the FTC requires that any stated benefit be substantiated with competent and reliable scientific evidence. Vague promises about "rejuvenation" or "optimal wellness" may seem harmless, but regulators have cited far subtler language in enforcement actions.


- Disclosures must be clear and conspicuous, placed where consumers will actually see them, not buried in captions or fine print.
- Typical-results standards apply to visuals too, meaning stock-style photos that imply outcomes your average patient won't achieve can trigger liability.
- Health claims require documented evidence on file before the ad runs, not after a complaint surfaces.


## The Rise of Coordinated Federal-State Enforcement


The Hims & Hers complaint was not solely a federal action. California and Utah joined as co-plaintiffs, bringing their own state consumer-protection claims alongside the FTC's. State attorneys general are active partners in health data enforcement now, not bystanders, and that changes the shape of the risk: one set of facts, several plaintiffs, different statutes, different remedies.


Be precise about what private plaintiffs can actually do, because this gets overstated constantly. California's CCPA has a private right of action, but a narrow one — it covers certain data breaches caused by failure to maintain reasonable security, not privacy violations generally; the rest is enforced by the state. Washington's My Health My Data Act is the broader consumer threat, because it is enforceable through the state's Consumer Protection Act, which does give individuals a path to sue.


For telehealth clinics operating across state lines, this compounds quickly. The same pixel firing on a booking page carries different statutory exposure in California versus Washington, and the FTC may be involved regardless. Practices in multi-state markets need to treat compliance as a layered obligation that follows their patients across state lines.


## Where a Practice Platform Helps, and Where It Doesn't


Worth being direct about this, because most vendor content in this category is not: your EMR cannot fix the problem this post describes. The pixel lives on your marketing website. Removing it, gating it behind consent, and rewriting your privacy policy to match are jobs for whoever owns that site. No practice management platform, Decoda included, reaches into your WordPress theme and takes the Meta Pixel out.


What a platform does change is how far patient data travels after the visit. The common pattern behind these cases is not one bad decision — it is a chain of syncs.[Patient records](https://decodahealth.com/products/patients) land in a CRM, the CRM feeds a marketing tool, the marketing tool builds an audience, and the audience goes to an ad network. Every hop is a place where health-related data can end up somewhere nobody intended.


Keeping[patient communications](https://decodahealth.com/products/communications) , records, and[membership billing](https://decodahealth.com/products/memberships) in one system removes hops from that chain, and the[AI scribe](https://decodahealth.com/products/scribe) keeps clinical documentation inside it rather than in a side tool someone integrated once and forgot about. That is a narrower claim than "we make you FTC compliant," and it is the honest one. The pixel audit is still yours to run.


## Final Thoughts on FTC Advertising and Data Compliance for Elective Care Practices


The FTC's recent actions aren't a warning that something might happen. They're a signal that it already is happening to practices that look a lot like yours. A pixel audit, a clear-eyed review of your privacy policy, and a clean process for patient consent go a long way.[Grab a quick call with us](https://calendly.com/d/cr5z-78z-8zq/decoda-ai-intro) if you want to see how Decoda supports compliant operations without adding friction to your day.


## Frequently Asked Questions


Does the FTC Health Breach Notification Rule apply to med spas and telehealth clinics that use Meta Pixel or Google Tag for retargeting?


Usually not, and the reason matters. The Rule expressly excludes HIPAA covered entities and their business associates acting as such; it exists to reach the consumer health businesses HIPAA misses, like apps and connected devices. If you bill insurance you are likely a covered entity, so the Rule is not your exposure. If you are cash-pay only you may not be a covered entity at all, which pulls you closer to the Rule's territory but still leaves Section 5 of the FTC Act as the primary federal risk. Note that the FTC's July 2026 complaint against Hims and Hers was brought under Section 5 and ROSCA, not under the Health Breach Notification Rule.


What should a med spa or telehealth clinic audit first to reduce FTC patient data advertising risk in 2026?


Start by settling in writing whether you are a HIPAA covered entity, because that determines which rules bind you. Then inventory every third-party script on your website and patient-facing booking pages, document exactly what each one captures and where that data goes, and read your privacy policy against that inventory. The gap between what the policy promises and what the scripts actually do is the specific thing Section 5 enforcement targets.


HIPAA compliance vs. the FTC Act for telehealth clinics: which one covers pixel tracking?


HIPAA's reach here is narrower than it was. In June 2024 a federal court in American Hospital Association v. Becerra vacated the portion of HHS guidance treating an IP address plus a visit to an unauthenticated public webpage as protected health information, and HHS withdrew its appeal that August. Tracking on logged-in patient portals and other identifiable combinations still count. The FTC Act operates on a different question entirely: not whether the data was PHI, but whether your public promises matched your actual practices.


Can a telehealth clinic get hit by both the FTC and a state attorney general over the same pixel on one booking page?


Yes, and the Hims and Hers case is the template. California and Utah joined the FTC's July 2026 complaint as co-plaintiffs, bringing their own state consumer-protection claims alongside the federal ones. One set of facts can support several plaintiffs under several statutes, each with its own remedies and its own defense costs.


What is the My Health My Data Act and does it affect med spas operating outside Washington State?


Washington's My Health My Data Act extends health data privacy protections to businesses that collect health information from Washington residents, regardless of where the business is located. It is the more serious consumer-litigation threat of the state laws in this area because it is enforceable through Washington's Consumer Protection Act, which gives individuals a path to sue directly. If your telehealth or med spa patients include Washington residents, it may reach your data practices.


Should I use a consent management tool or just update my privacy policy to cover pixel tracking?


Updating the policy alone will not protect you, and can make things worse by creating a second promise to fall short of. Section 5 targets the gap between stated practice and real behavior, so the durable fix is changing what the pixel does. A consent tool that blocks scripts from firing until a patient actively opts in is far more defensible than a revised disclosure buried in fine print.


What is the difference between a HIPAA Business Associate Agreement and consent for advertising vendors?


A BAA governs how a vendor handles protected health information under HIPAA, and it only matters if you are a covered entity. Most ad-tech vendors sit outside HIPAA's reach and will not sign one, which is itself a signal about whether that data should be flowing to them. Consent is a separate question that survives regardless of your HIPAA status: did the patient actually agree to this, and does your disclosure describe what really happens?


My staff is generating social content about patient results. How do I make sure those posts don't create FTC liability?


Any content implying a patient outcome, whether from staff, a paid influencer, or an unpaid patient, has to meet the FTC's typical-results standard and carry a clear disclosure where there is a material connection. Before-and-after photos must reflect what an average patient can expect rather than your best case, and the substantiation for any outcome claim needs to exist before the post goes live, not after a complaint arrives.


What documentation should a med spa keep on file to defend its advertising claims if the FTC opens an inquiry?


A substantiation file for every outcome claim, dated before the ad ran. Written authorization for every patient photo or testimonial used in marketing. Records showing paid or incentivized posts carried the required disclosures. Your script inventory and the dates you reviewed it. Regulators expect this evidence to already exist rather than to be assembled after a complaint surfaces, and the Hims timeline is instructive: the FTC opened its probe in 2023 and did not sue until 2026.


Would switching practice management software reduce my FTC advertising exposure?


Not for the pixel itself, and any vendor claiming otherwise is overselling. The tracking script lives on your marketing website, and removing or gating it is a job for whoever owns that site. What consolidating systems does change is how many places patient data travels to afterward: each sync from EMR to CRM to marketing tool to ad audience is a hop where health-related data can end up somewhere nobody intended. Fewer hops is a real risk reduction, but it is a different problem from the one on your booking page.
