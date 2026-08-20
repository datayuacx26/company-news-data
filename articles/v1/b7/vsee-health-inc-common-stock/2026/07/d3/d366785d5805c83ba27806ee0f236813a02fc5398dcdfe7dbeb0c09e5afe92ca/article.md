---
schema_version: "1.0.0"
document_id: "d366785d5805c83ba27806ee0f236813a02fc5398dcdfe7dbeb0c09e5afe92ca"
company_key: "vsee-health-inc-common-stock"
company: "VSee Health Inc."
source_id: "vsee-health-inc-common-stock-news-import-53c418ab6063"
canonical_url: "https://vsee.com/blog/fedramp-authorized-vs-in-process/"
published_at: "2026-07-30T22:43:29+00:00"
first_seen_at: "2026-08-01T00:40:21.274183+00:00"
fetched_at: "2026-08-01T00:40:22.411840+00:00"
content_hash: "sha256:80601efe08f55cea63efcf81af12b4cdc8d26d904db90742fd72d327ee56d9e8"
---

# FedRAMP EHR Status: What “Ready, In Process, Authorized, Moderate, and High Actually Mean

A FedRAMP status is really two separate pieces of information, glued together. One tells you how far along a platform is in getting approved. The other tells you what kind of data it’s allowed to touch. Read them apart, and any status — no matter how it’s worded — becomes easy to decode.


## **What “FedRAMP status” actually means**


When someone says a product is “FedRAMP Authorized” or “FedRAMP Moderate,” they’re actually answering two different questions at once:


- **Where is it in the approval process?** (Ready, In Process, or Authorized)
- **What data is it cleared to handle?** (Moderate or High)


Vendors often mash both into a single phrase — “FedRAMP Moderate In Process!” — and it can sound like a finished credential. It isn’t. Once you know to split the claim into its two parts, reading any FedRAMP status becomes significantlyless confusing.


## **Part 1: Where is it in the approval process?**


Every platform moves through the same three stages, in the same order:


1. **Ready —** An independent assessor has checked the platform’s security setup. It’s a real step, but no agency has approved it yet, and no risk has been accepted.


2. **In Process —** The vendor is actively working with an agency toward approval. This shows momentum, not clearance. Some products stay “In Process for years. Some never finish.


3. **Authorized —** The review is complete, and an agency has signed off. The platform holds an Authority to Operate (ATO). This is the only stage where a system can legally handle federal data.


Think of it as a lifecycle, not a label. A platform can only be at one stage at a time, and only the last stage actually means anything is allowed to operate.


## **Part 2: What data is it cleared to handle?**


This part has nothing to do with how far along a vendor is — it’s set by federal rules (


[FIPS 199](https://csrc.nist.gov/pubs/fips/199/final) ), based on how bad a breach would be:


- **Moderate** (~325 security controls) — covers data where a breach would cause serious harm. Most federal health data falls here.


- **High** (400+ controls) — covers data where a breach would be severe or even catastrophic. A lot of federal health information falls here too.


Think of it as a ceiling, not a score. A platform authorized at Moderate cannot take on High-level data, no matter how solid its authorization otherwise is. If your agency’s data is categorized as High, you need a High-authorized platform. No exceptions.


## **Putting the two parts together**


Once you can read both parts separately, combining them is simple. Ask one question about any vendor claim:


**“Authorized — at what level — by whom?”**


Only one combination actually clears a platform for federal use: Authorized, at the level your data requires. Everything short of that — Ready, In Process, or Authorized-but-at-the-wrong-level — is a work in progress, not a green light.


## **How to read a real FedRAMP Marketplace listing**


With the two parts in mind, checking a real listing takes about thirty seconds. Look up the vendor and check three fields, in this order:


- **Status** — Ready, In Process, or Authorized


- **Impact level** — Li-SaaS, Low, Moderate, or High


- **Authorizing agency** — who actually approved it


Then double-check you’re looking at the right product. Sometimes a listing covers a different edition of the software, or just the hosting layer underneath it — not the actual application.


One thing worth knowing: a platform can hold a real ATO without ever showing up on the Marketplace. That’s not automatically a red flag — it just means you’ll need to verify it a different way. More on that here: FedRAMP Marketplace listing vs. agency ATO →


## **Common mistakes when reading a status**


- **Treating “In Process” as good enough.** It signals effort, not clearance. No agency has accepted the risk yet.


- **Assuming a higher impact level is always better.** It’s not a quality score — it’s a match to your data’s sensitivity. What matters is whether it meets your requirement.


- **Stopping at the headline claim.** “FedRAMP Moderate In Process” reads like a credential but is really two half-finished facts stacked together.


- **Not checking who actually authorized it.** An ATO from one agency doesn’t automatically mean every agency will accept it — though OMB policy pushes agencies to lean on existing authorizations rather than starting over.


## **A quick heads-up: the labels are changing in 2026**


[FedRAMP is rolling out new terminology](https://www.fedramp.gov/2026/providers/updating/changes/#terminology-shifts) . “Authorized” is becoming “FedRAMP Certified,” and the old impact levels (Low, Moderate, High) are being replaced with class letters, A through D. The underlying security requirements haven’t changed — just the name you’ll see on listings and vendor sites going forward.


Here’s how the classes stack up, from lowest assurance to highest:


- **Class A —** the lightest tier, meant as an on-ramp for cloud providers entering the federal market. It asks for less upfront documentation and lighter ongoing reporting. It’s a starting point, not a destination — vendors are expected to move up to Class B, C, or D as they take on more federal work.


- **Class B and Class C —** middle tiers for services with progressively more federal use and more sensitive data, with reporting and monitoring requirements that scale up accordingly.


- **Class D —** the highest tier. It replaces what used to be called “High,” and it’s reserved for mission-critical services where a breach could cripple agency operations or cause catastrophic harm. It requires the largest investment in both obtaining and maintaining certification.


**What “FedRAMP 20x Class A” means:** as of today, FedRAMP has retired its old “Ready” status as the entry point for new vendors. “


[FedRAMP 20x](https://www.fedramp.gov/2026/reference/20x/) ” is the name of FedRAMP’s modernized certification program, and “Class A” is the new, lighter-weight starting certification within it — open to providers with an existing SOC 2 Type II audit who want a faster way into the federal market. It sits at the bottom of the new scale, roughly where “Ready” used to sit, not where “Authorized” did.


**What’s the highest certification for a**[FedRAMP EHR](https://vsee.com/fedramp) **?** Class D — the same tier previously called “High.” Right now, Class D is only available through FedRAMP’s traditional agency-sponsored path (Rev5); a Class D option within the newer 20x program is expected to be piloted in late 2026 and formally available in early 2027. For an EHR or telehealth platform handling sensitive federal health data, a Class D (High) authorization is the top of the scale — the same bar


[VSee’s ATO](https://vsee.com/fedramp) is assessed against.


-


## **FAQs**


### **What does “FedRAMP status” mean?**


It’s shorthand for two things at once: how far along a platform is in the approval process (Ready, In Process, Authorized), and what level of data it’s cleared to handle (Moderate or High).


### **What does “FedRAMP In Process” mean?**


It means a vendor is actively working toward approval but hasn’t gotten it yet. No agency has accepted the risk, so the platform isn’t cleared for federal data.


### **Can I use a platform that’s “In Process”?**


No. Only a completed authorization — at the impact level your data needs — clears a platform for federal use.


### **Is FedRAMP Ready the same as FedRAMP Authorized?**


No. Ready just means a platform passed an initial security check. It hasn’t gone through full agency review, so it isn’t cleared to handle federal data yet.


### **What does “FedRAMP Certified” mean?**


It’s FedRAMP’s newer name for what used to be called “Authorized.” Same requirement, new label. High-level (“Class D”) certification still means the toughest standard on the books.


### **How long does it take to go from In Process to Authorized?**


It varies a lot by agency and platform complexity — anywhere from several months to a couple of years. There’s no fixed timeline, which is exactly why “In Process” alone doesn’t tell you much.


### **Does an Authorized status ever expire?**


Authorized platforms have to go through continuous monitoring to keep their status current. It isn’t a one-time stamp — agencies expect ongoing proof the security posture holds up.


### **What does “FedRAMP High” mean?**


FedRAMP High is the most stringent impact level (400+ NIST 800-53 controls), reserved for the government’s most sensitive unclassified data, including health information used in federal programs.


### **What’s the difference between FedRAMP Moderate and High?**


It comes down to data sensitivity. Moderate covers serious-impact data (~325 controls). High covers the most sensitive data (400+ controls) — including a lot of federal health information. A Moderate platform can’t take on High-level work.


### **What is an Authority to Operate (ATO)?**


An ATO is the formal approval a federal agency issues before a system goes live, certifying its security risk has been assessed and accepted. A FedRAMP High ATO means it was assessed against the FedRAMP High baseline.


### **Which EHRs have a FedRAMP High ATO?**


Very few. VSee holds a FedRAMP High ATO issued by HHS ASPR; most others are Moderate, In Process, single-agency, or not FedRAMP.


See the comparison →


### **How do I verify a vendor’s FedRAMP status?**


Check the


[FedRAMP Marketplace](https://www.fedramp.gov/marketplace/) ; if a vendor isn’t listed, request its ATO letter, impact level, authorizing agency, and System Security Plan — and confirm it’s the platform’s own ATO, not just its hosting.


### **How long and how expensive is a FedRAMP authorization?**


Typically 12–18 months plus significant cost and staffing (several hundred thousand to a few million) — which is why deploying a platform that already holds a FedRAMP authorization removes the biggest time and risk from an agency’s project.


### **What is the FedRAMP authorization process?**


A platform is assessed against the FedRAMP control baseline by an accredited third-party assessor, and a federal agency reviews the results and issues an ATO. The full path follows the NIST Risk Management Framework:


1. Categorize the data’s impact level (Low / Moderate / High)


2. Secure a federal agency sponsor


3. Implement controls (typically on a FedRAMP-authorized cloud)


4. Document them in a System Security Plan (SSP)


5. Independent 3PAO (third-party assessment organization) assessment


6. Remediate findings (tracked in a POA&M or Plan of Action & Milestones)


7. Agency issues the ATO


8. Maintain it through continuous monitoring


Because this is long and demanding, deploying a platform that has already completed it removes most of that burden from your agency.


## **Where VSee stands**


VSee holds a completed authorization at the highest level: a FedRAMP High ATO issued by HHS ASPR.


[See the ATO details and request the security package →](https://vsee.com/fedramp)


### ***Sources***


- ** **[FedRAMP Marketplace](https://www.fedramp.gov/marketplace/)
- ** **[FedRAMP 20x](https://www.fedramp.gov/20x/)
- ** **[FIPS 199](https://csrc.nist.gov/pubs/fips/199/final)
- ** **[FedRAMP 2026 terminology updates](https://www.fedramp.gov/2026/definitions/)


### Share this:


- [Facebook](https://vsee.com/blog/fedramp-authorized-vs-in-process/?share=facebook)
- [Twitter](https://vsee.com/blog/fedramp-authorized-vs-in-process/?share=twitter)
- [LinkedIn](https://vsee.com/blog/fedramp-authorized-vs-in-process/?share=linkedin)
- Email
-


### Like this:


Like


Loading…


### *Related*
