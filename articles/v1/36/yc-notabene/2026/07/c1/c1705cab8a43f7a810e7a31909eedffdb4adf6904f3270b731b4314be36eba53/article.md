---
schema_version: "1.0.0"
document_id: "c1705cab8a43f7a810e7a31909eedffdb4adf6904f3270b731b4314be36eba53"
company_key: "yc-notabene"
company: "Notabene"
source_id: "yc-notabene-news-import-e4585f8c666e"
canonical_url: "https://notabene.id/post/genius-regulates-issuers-the-risk-is-in-the-network"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-26T08:22:57.520792+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:c9fc9a878db6a0af2a58bb989dffe2c510377ecb3a9828724bf4ecd1335f0f56"
---

# GENIUS Regulates Issuers. The Risk Is in the Network.

According to the Chainalysis[2026 Crypto Crime Report](https://www.chainalysis.com/reports/crypto-crime-2026/) , in 2025, illicit crypto addresses received over $154 billion which is a 162%increase year over year. Further, 84% of all illicit volume moved through stablecoins, which is up from 63% the year before. Sanctions evasion alone grew 694%.\[1\] The numbers should reframe how every compliance team in the United States is reading the GENIUS Act.


Now read the GENIUS Act and ask yourself: does this law address where the risk lives?


### The Architecture Is Pointed at the Wrong Thing


The GENIUS Act is a serious piece of legislation. The first federal statute to define payment stablecoins as a distinct asset class. Reserve requirements. Redemption mechanics. Issuer governance. Bank-level AML obligations for permitted issuers. All necessary.


But here is the thing nobody is saying out loud. The entire architecture of GENIUS is built around regulating the issuer. The act defines who is permitted to issue. Who must hold reserves. Who must attest monthly. Issuer, issuer, issuer.


The problem is the risk does not live with the issuer.


Risk in stablecoin payments lives in the network. Wallets. Exchanges. Intermediaries. Cross-chain flows. Counterparty institutions in jurisdictions with weak or nonexistent supervision.


Regulate a stablecoin issuer perfectly and the 84% still flows right through. Require pristine reserves, monthly attestations, full BSA compliance at the issuance point. None of these addresses what happens to the tokens once they leave the issuer's custody.


## The Rulemaking to Read First


One piece of GENIUS implementation is focused on the network reality. The[FinCEN/OFAC joint NPRM](https://www.federalregister.gov/documents/2026/04/10/2026-06963/permitted-payment-stablecoin-issuer-anti-money-launderingcountering-the-financing-of-terrorism#h-125) , released for public comment in April with a June 9 deadline, amends 31 CFR 1010.100(eee). \[2\] For the first time, the U.S. is codifying the Travel Rule for digital assets into federal statute.


This matters more than the headline rulemakings most teams are tracking. The Travel Rule is the only mitigating control we have before a transaction settles. No correspondent bank holding the message in a queue. No three-day window for sanctions review. The compliance work has to happen before the transaction broadcasts.


The Travel Rule pairs two BSA obligations the U.S. has had on the books since the mid-1990s for wire transfers. Recordkeeping for what you hold. Information exchange for what you pass to the next institution in the chain.[FinCEN](https://www.fincen.gov/resources/statutes-regulations/guidance/application-fincens-regulations-certain-business-models) extended these obligations to crypto in 2019. \[3\] The April NPRM is the codification.


The international picture has moved further. According to[FATF](https://www.fatf-gafi.org/en/publications/Fatfrecommendations/targeted-update-virtual-assets-vasps-2025.html) , 87% of material jurisdictions have either implemented Travel Rule for crypto or are in the process. \[4\] The[UK](https://www.legislation.gov.uk/uksi/2022/860/made) has been live since September 2023. \[5\] The[EU](https://eur-lex.europa.eu/eli/reg/2023/1113/oj/eng) went live under the Transfer of Funds Regulation on December 30, 2024. \[6\] The U.S. is now closing the implementation gap.


## What Winners Are Doing Differently


Most firms approach a new regulatory regime by reading the rule, writing a memo, running the memo through legal, building a compliance program around the memo. Six months later they realize they have a policy and no operational capability.


The teams winning this approach the work differently. They start with their transaction flows. They map their counterparty universe. They identify decision points where data has to be collected, screened, exchanged, or held. They architect for configurability because GENIUS is one regime among several.


Address screening alone is no longer sufficient. Identifying a counterparty institution at the time of every transfer is no longer optional. Real-time pre-transaction authorization is what counterparties and examiners now expect. The firms reading GENIUS as a checkbox on issuer reserves are going to be surprised when their counterparty diligence questionnaires double in length next quarter.


## The Read


The act addresses issuer accountability, which is necessary. But the risk lives in the network. Miss the network framing and you have a beautifully regulated issuer base with bad actors flowing 84% of their illicit volume right through the rails.


From where we sit at Notabene, supporting Travel Rule and transaction authorization across more than 2,000 institutions in 100 plus jurisdictions, the firms moving fastest right now are treating GENIUS as a network problem from day one. They are not waiting for final rules. They are architecting for the operational reality the NPRMs already describe.


Bad compliance is a barrier to growth. Good compliance is infrastructure for growth. Compliance is now a feature of the platform, not a wrapper around the platform.


The winners here won't be the most compliant. They'll be the most operationally adaptable.


‍
