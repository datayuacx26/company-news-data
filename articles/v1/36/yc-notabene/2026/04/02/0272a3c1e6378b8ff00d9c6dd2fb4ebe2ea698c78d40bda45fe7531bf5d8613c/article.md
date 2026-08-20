---
schema_version: "1.0.0"
document_id: "0272a3c1e6378b8ff00d9c6dd2fb4ebe2ea698c78d40bda45fe7531bf5d8613c"
company_key: "yc-notabene"
company: "Notabene"
source_id: "yc-notabene-news-import-e4585f8c666e"
canonical_url: "https://notabene.id/post/what-genius-act-implementation-actually-requires-of-your-compliance-stack"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-26T08:22:57.520792+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:819a2ea44d1d814c6c4093bbcd768e4a6fbcc2d4b0c68a8467c1f31fe4b0b963"
---

# What GENIUS Act Implementation Actually Requires of Your Compliance Stack

Between April 1 and April 8, 2026, three federal agencies put concrete proposals on the table for how the GENIUS Act will be implemented.


- Treasury issued its notice of proposed[rulemaking](https://www.federalregister.gov/documents/2026/04/03/2026-06489/genius-act-broad-based-principles-for-determining-whether-a-state-level-regulatory-regime-is) (NPRM) on how state-level stablecoin regimes get certified as substantially similar to the federal framework.
- The FDIC published a[NPRM](https://www.fdic.gov/news/financial-institution-letters/2026/notice-proposed-rulemaking-establish-genius-act) establishing requirements and standards applicable to FDIC-supervised permitted payment stablecoin issuers (PPSIs) and insured depository institutions (IDIs) that engage in payment stablecoin-related activities, covering reserves, redemption, custody, and risk management.
- FinCEN and OFAC[jointly proposed](https://www.fincen.gov/system/files/2026-04/PPSI-AMLCFT-NPRM.pdf) the AML/CFT and sanctions compliance program that every PPSI will need to build.


### **🗓️ April 1: Treasury defines the pathway to state-level regulation**


The GENIUS Act created a dual-pathway regime. Payment stablecoin issuers with consolidated outstanding issuance of $10 billion or less can opt into state-level regulation, provided the state regime is certified as "substantially similar" to the federal framework. Issuers above that threshold come under direct federal oversight.


Treasury's **April 1 NPRM** lays out the broad-based principles it will use to determine substantial similarity, building on the advance notice of proposed rulemaking Treasury issued the prior September.


Under GENIUS, states must meet or exceed the core prudential standards and requirements described in Section 4(a) of the Act. Treasury's proposal splits those Section 4(a) requirements into two buckets:


- **"Uniform requirements"** (such as reserve composition, BSA/sanctions compliance, and redemption obligations), where states have essentially no interpretive discretion and must mirror the federal standard.
- **"State-calibrated requirements"** (such as capital, liquidity, reserve asset diversification, and risk-management standards), where states retain meaningful design flexibility but must still produce outcomes "at least as stringent and protective" as the federal framework.


For provisions outside Section 4(a) — applications, licensing, supervision, and enforcement — states get more latitude, but must still deliver similar levels of authority, oversight, and holder protection. Examination authority, for example, cannot be limited to cases where the issuer consents, and stablecoin holders cannot be treated as general unsecured creditors in insolvency.


Treasury is also explicit that states may go beyond federal requirements, but only if the additional rules neither conflict with the Act nor erode the substantially-similar determination.


The comment period runs 60 days from Federal Register publication.


### **🗓️ April 7: FDIC sets the operating rules for PPSIs**


On April 7, 2026, the FDIC approved a **NPRM implementing GENIUS Act** requirements for FDIC-supervised PPSIs and insured depository institutions that engage in payment stablecoin-related activities, with comments due by June 9, 2026.


Among the most consequential provisions is proposed § 350.3(b)(4), which operationalizes Section 4(a)(11) of the GENIUS Act — the statutory prohibition on stablecoin yield. The rule would bar a PPSI from paying any holder of its payment stablecoin any form of interest or yield, whether in cash, tokens, or other consideration, solely in connection with the holding, use, or retention of the stablecoin.


Anticipating that issuers may seek to replicate yield indirectly through affiliates or commercial partners, the FDIC pairs the prohibition with a rebuttable presumption. An issuer is presumed to be in violation if it has an arrangement to pay consideration to an affiliate or related third party, **and that affiliate or related third party in turn pays interest or yield to holders of the issuer's stablecoin solely for holding, using, or retaining it** . The presumption also reaches cases in which the PPSI issues stablecoins on behalf of, or under the branding of, a related third party — in that scenario, the end holder is treated as a holder of the PPSI-issued stablecoin for purposes of the prohibition.


That second prong is aimed squarely at white-label and co-branded issuance arrangements, where the regulated issuer is one entity but the customer-facing brand and distribution relationship sit with another.


A PPSI may rebut the presumption only by submitting written materials that satisfy the FDIC the arrangement is neither prohibited nor structured to evade the prohibition. Taken together, the provision signals that the FDIC intends the no-yield rule to apply substantively, not just on the face of issuer–holder documentation.


### **🗓️ April 8: FinCEN and OFAC define the AML/CFT and sanctions rules**


On April 8, 2026, FinCEN and OFAC **jointly issued a proposed rule** implementing the GENIUS Act's directive to treat PPSIs as financial institutions under the Bank Secrecy Act and to require an effective sanctions compliance program. The rule puts PPSIs squarely under the full BSA playbook — a risk-based AML/CFT program, SAR filing (at a $5,000 threshold), Customer Due Diligence and beneficial ownership collection for legal entity customers, CTRs, Recordkeeping and Travel Rule, information sharing, enhanced due diligence for correspondent and private banking relationships, and special measures — plus two PPSI-specific obligations mandated by the GENIUS Act:


(1) technical capabilities to block, freeze, and reject impermissible transactions, and


(2) the capability to comply with "lawful orders" to seize, freeze, burn, or prevent the transfer of payment stablecoins.


On the sanctions side, OFAC proposes a new Part 502 codifying a five-element effective sanctions compliance program (senior management commitment, risk assessment, internal controls, testing and auditing, and training), modeled on OFAC's 2019 Compliance Framework and 2021 Virtual Currency Industry Guidance.


On Travel Rule specifically: FinCEN's 2019 interpretive guidance (FIN-2019-G001) had already clarified that transmittal orders involving convertible virtual currencies (CVCs) — a category that includes payment stablecoins — qualify as "transmittals of funds" under the Recordkeeping and Travel Rules, because a CVC transmittal order is an instruction to pay "a determinable amount of money." The underlying Travel Rule obligation therefore already applied. What is new is that FinCEN proposes amending the definition of "transmittal order" in 31 CFR 1010.100(eee) to expressly include payment stablecoin alongside "money", codifying the 2019 position and removing any remaining room for interpretation post-GENIUS. PPSIs are also being added to the list of institutions in § 1010.410(e)(6), giving them the same PPSI-to-PPSI and PPSI-to-bank exceptions that banks and broker-dealers enjoy.


The relevance of Travel Rule requirements extends well beyond Travel Rule compliance in itself. OFAC sanctions are a strict-liability regime — a PPSI can be held civilly liable for a violation even without knowledge that it was engaging in one — and the rule makes clear that PPSIs must account for identifying and blocking transactions that would violate U.S. sanctions, including stablecoins traded by blocked persons on the secondary market when a PPSI exercises possession or control (for example, through smart contracts). A risk-based OFAC program cannot function on wallet addresses alone; meaningful screening requires counterparty identity. The data PPSIs collect through Travel Rule controls is paramount to ensure the effectiveness of OFAC screening programs.


Finally, proposed § 1033.240(a) implements the GENIUS Act's directive that PPSIs maintain technical capabilities, policies, and procedures to block, freeze, and reject specific or impermissible transactions, and FinCEN expressly extends this obligation to secondary market activity, noting that this is where the majority of illicit stablecoin flows occur.


### **Conclusion**


Read together, these three proposals sketch the operational reality of PPSIs under the GENIUS Act. All three proposals follow the standard 60-day comment window from Federal Register publication, which puts the response deadlines in early June 2026. Overlaying this process is the GENIUS Act's own effective-date mechanic. The Act becomes effective on the **earlier** of (a) **January 18, 2027** , or (b) **120 days after the primary Federal payment stablecoin regulators issue final regulations implementing the Act** .


Notabene is actively preparing its response to the FinCEN and OFAC NPRM. Further insights to follow.
