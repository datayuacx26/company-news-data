---
schema_version: "1.0.0"
document_id: "0be3831794c2a50d757ad414b8547ec30ac9747b96158f66da6c9c97a9de3397"
company_key: "yc-notabene"
company: "Notabene"
source_id: "yc-notabene-news-import-e4585f8c666e"
canonical_url: "https://notabene.id/post/the-ppsi-rule-and-the-future-of-stablecoin-payments"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-26T08:22:57.520792+00:00"
fetched_at: "2026-07-28T21:23:16.325685+00:00"
content_hash: "sha256:90dc6a0f2f55be71b1fb97bc1bdcde9eb9d136f37797b0fd662d0ec760ae9674"
---

# The PPSI Rule and the Future of Stablecoin Payments

For the first time in OFAC's history, U.S. regulators are codifying an explicit sanctions compliance program requirement in regulation. For the first time, FinCEN is treating stablecoin issuers as a distinct financial institution category under the Bank Secrecy Act rather than as money services businesses with a crypto wrapper.


The[joint proposed rule FinCEN and OFAC issued in April](https://notabene.id/post/what-genius-act-implementation-actually-requires-of-your-compliance-stack) implementing the Permitted Payment Stablecoin Issuer (PPSI) framework under the GENIUS Act is the most consequential digital asset rulemaking the U.S. has embarked on in a decade.


Notabene filed our response to the notice of proposed rulemaking (NPRM), answering eleven substantive questions across the AML/CFT and sanctions program sections.


Three positions anchor what we filed, and **the choices regulators make in finalizing this rule will shape the next decade of stablecoin payments** .


### Travel Rule codification has to be designed for cross-border use


The Travel Rule already applies to stablecoin transfers under existing 31 CFR 1010.410(e) and (f), as FinCEN's 2019 guidance made clear. Codifying it in a dedicated Part 1033 for PPSIs removes the ambiguity slowing U.S. implementation and gives institutions a clean foundation to build compliance programs on.


The codification has to be designed for international interoperability from day one. We recommended FinCEN endorse IVMS 101 as the data standard for transmittal-level identifying information, and require the messaging layer carrying it to operate on open and interoperable standards.


**Read more about our commitment to open standards**[here](https://notabene.id/post/call-for-open-travel-rule-standards)


We also asked FinCEN to align the U.S. threshold with the FATF Recommendation 16 as Travel Rule compliance is collaborative by design. One institution's ability to comply depends on its counterparty's ability to receive and process the data, and the U.S. threshold of $3,000 is now the highest among peer jurisdictions with active enforcement. The EU and UK, for example, enforce at zero threshold.


Threshold and data-set divergence is not abstract and it causes operational headaches. When a U.S. institution sends a stablecoin transfer to an EU counterparty with only the U.S. data set, the receiving EU CASP is required by the EU Transfer of Funds Regulation and the EBA Travel Rule Guidelines to detect the missing fields and decide whether to execute, request the missing data, reject, return, or suspend the transfer. When the EU CASP requests missing data from a non-EU sender, the EBA Guidelines give the sender a five working day window to supply it. The transfer is treated as suspended in the meantime. Repeated sub-standard transmissions can show up as a counterparty-risk flag on the EU side and trigger reporting to the EU CASP's competent authority under Article 17 of the TFR.


The result is that U.S. PPSIs and their global counterparties end up at a disadvantage relative to actors operating outside regulated channels, where no such friction applies. Threshold and data-set alignment with the FATF Recommendation 16 trajectory is what closes that gap.


### Block, freeze, and reject only work before settlement


Public blockchains have no built-in mechanism for evaluating a transaction before settlement. Once a transfer is initiated on-chain, it settles. A PPSI without pre-transaction authorization infrastructure is limited to reactive measures, freezing assets after they arrive rather than preventing transfers from occurring.


The GENIUS Act's block, freeze, and reject obligations under proposed 31 CFR 502.201(b)(3) are therefore architectural requirements, not just programmatic ones. Pre-transaction authorization is the mechanism that meets them.


Authorization-before-settlement models work through a private messaging layer operating alongside the blockchain. The two institutions party to a transfer exchange Travel Rule data, perform sanctions screening, assess counterparty risk, and make an explicit authorize-or-reject decision before on-chain settlement. The[Transaction Authorization Protocol (TAP)](https://tap.rsvp/) is one open standard implementation of this approach.


We asked OFAC to recognize pre-transaction authorization architectures as a compliant technical implementation, without mandating any specific protocol.


The recognition matters because pre-transaction authorization pairs with, rather than replaces, the freeze, burn, and reissue capabilities GENIUS Act Section 5(a)(2) grounds. Proactive authorization at the transaction level and reactive freeze at the asset level form a complete sanctions architecture no traditional financial institution has access to.


### The primary and secondary market distinction is operationally correct


FinCEN's proposed framework draws a clean line between activity where the PPSI is a direct party (primary market, where full programmatic obligations apply) and activity where the PPSI is observing smart contract operation as an issuer rather than as a transacting party (secondary market, where contract-layer interventions are narrowly scoped). We agree with how the line is drawn and think the rule should preserve it.


Any secondary market intervention authority outside lawful orders should be narrow, explicit, tied to defined triggers, and paired with an express liability safe harbor. Institutions holding the customer relationship and the Travel Rule channel are best positioned to enforce sanctions and AML controls in secondary market activity. The PPSI is best positioned to execute targeted contract-layer interventions when an objective trigger is met.


A narrow scope here protects the predictability making payment stablecoins viable as payment infrastructure. A broad or discretionary one shifts effective authorization control to an actor not party to the transfer and not participating in pre-transaction review. That breaks the architecture, and the institutions party to the transfer end up with no certainty about whether an authorized transfer will settle.


### What's at stake


This rulemaking will decide whether the U.S. ends up with a stablecoin payment system designed for cross-border interoperability or one engineered around U.S.-specific architectures the rest of the world has already moved past.


The full[Notabene submission can be found here,](https://notabene.id/reports/notabene-response-to-ppsi-aml-cft-and-sanctions-nprm) and is on[regulations.gov](https://www.regulations.gov/) under Docket FINCEN-2026-0100.


Additional analysis will follow over the coming weeks on OFAC sanctions architecture, the special standards of diligence framework for cross-border PPSI counterparty relationships, and the foreign payment stablecoin issuer equivalence question.


**Questions?** Contact our Regulatory & Compliance team[here.](https://notabene.id/solutions/regulatory-engagement)
