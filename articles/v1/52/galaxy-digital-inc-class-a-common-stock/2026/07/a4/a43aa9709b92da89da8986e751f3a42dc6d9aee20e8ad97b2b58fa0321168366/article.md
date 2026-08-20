---
schema_version: "1.0.0"
document_id: "a43aa9709b92da89da8986e751f3a42dc6d9aee20e8ad97b2b58fa0321168366"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-3264694a8f1e"
canonical_url: "https://www.galaxy.com/insights/research/zcash-ironwood-upgrade"
published_at: null
first_seen_at: "2026-07-31T23:15:35.285855+00:00"
fetched_at: "2026-07-31T23:15:36.493107+00:00"
content_hash: "sha256:e675e13fe73b393e9e73c2e77e26477855db4eb0cbce0ed61219d71801df6dd4"
---

# Zcash’s Ironwood Upgrade Sharpens Chain’s Privacy Features

*This article originally appeared in Galaxy Research's weekly newsletter.*[Subscribe](https://www.galaxy.com/subscribe-to-research?utm_campaign=Weekly%20Research%20Brief%20-%20Email&utm_medium=email&utm_source=hs_email&_hsenc=p2ANqtz--YWOkviRZJsa_uYNPjl2N4ahqTpHeV86Rs9VQQeL9MWn1U09L_xNxjRhoRyGaaBrCPLXuX) *to get timely insights delivered to your inbox every Friday morning.*


**Zcash**[rolled out](https://x.com/zodl_co/status/2082106258740420648) **its Ironwood network upgrade Tuesday, permanently sealing the blockchain’s previous primary private transaction pool, called Orchard, so that it can no longer accept deposits or internal transfers, and opening a private pool named Ironwood.**


In Zcash, activity can occur in either transparent (public) addresses, similar to Bitcoin’s, or in shielded pools. Shielded pools use zero-knowledge cryptography to hide the amounts being transferred and the identities of the parties involved while still allowing the network to verify that transactions are valid. Orchard had been the network’s main shielded pool since its launch in May 2022 and held approximately 3.66 million ZEC (worth roughly $1.7 billion) at the moment of the upgrade. For a full overview of Zcash, refer to Galaxy’s[prior research here](https://www.galaxy.com/insights/research/zcash-price-zec-near-intents-zashi-wallet-privacy-zero-knowledge-proofs) .


Ironwood was developed after Shielded Labs researcher Taylor Hornby found a soundness vulnerability in Orchard’s zero-knowledge proof circuit on May 29. The flaw had existed since Orchard’s launch in 2022 and could theoretically have allowed the creation of counterfeit ZEC that would be undetectable on the public blockchain. An emergency upgrade in early June patched the[circuit](https://zkjargon.github.io/definitions/circuit.html) for new transactions. Ironwood goes further by creating a shielded pool that uses the corrected circuit and by restricting Orchard so that funds can leave only through a longstanding Zcash mechanism known as the turnstile. This is an accounting rule at the boundary of a shielded pool that ensures no more ZEC can exit than the amount that was verifiably deposited into it.


Users holding funds in Orchard must migrate them to the Ironwood pool voluntarily through updated wallets; the migration is not automatic. As of July 30, public trackers showed roughly 410,000–413,000 ZEC already in the Ironwood pool and about 3.29 million ZEC still in Orchard.


#### OUR TAKE:


A soundness flaw in a shielded pool is uniquely challenging. The cryptography that hides amounts and parties also hides any possible counterfeiting. You cannot see the flaw being used. You can only see, eventually, that the supply is wrong. Ironwood closes that gap with the turnstile by sealing the old pool and forcing every coin that wants to keep circulating through an accounting checkpoint. This means more value cannot leave the pool than went into it. Any counterfeit that might exist is now permanently quarantined.


The response by the Zcash developer community is impressive. Hornby found a four-year-old bug with AI-assisted tooling, the circuit was patched nearly immediately post discovery, and Ironwood deployed two months after disclosure with a formally verified pool and quantum-recoverable notes. Formal verification of counterfeiting risk in production is still rare.


The open variable now is migration speed. Roughly 11% of the old Orchard balance has already moved in the first two days, which is a solid start. The turnstile makes this a queue rather than an adoption metric. If the pool is short, the shortfall lands on whoever leaves last. That creates an incentive to move coins early. But Ironwood's anonymity set creates an incentive to wait, because early arrivals hide in a much smaller crowd. Until a critical mass of funds and daily activity sits in Ironwood, the effective private liquidity of the Zcash network remains split between two pools. Ultimately, if Orchard empties without the turnstile flagging any excess supply, that would prove the flaw was never used and put to rest the May supply question. Zcash in this scenario would emerge stronger thanks to new cryptographic assurances, independent supply verifiability, and a clearer long-term path to quantum recoverability. For allocators who want exposure to the privacy thesis, the residual risk would shift from "is the supply real?" to the more ordinary questions of adoption, liquidity, and execution.


Stepping back, the deeper question is why privacy is increasingly a non-negotiable feature across crypto. Two forces that usually talk past each other are converging. On one side is the classic cypherpunk view that privacy is an inherent right and that there is demand for digital bearer instruments with privacy baked into the base layer. Zcash is the market’s current favorite expression of that thesis. As the team put it, "Financial freedom requires financial privacy." On the other side is the institutional reality. Fully transparent public chains are simply unusable for serious finance. Counterparties will not run strategies, manage inventory, or settle size in the open. That is why privacy now sits at the center of Ethereum's Strawmap, Solana's infrastructure priorities, and nearly every major blockchain's roadmap. Privacy is no longer just a category within crypto. It is becoming a requirement of the base layer.


The uncomfortable implication for Zcash is that if privacy becomes a property of every chain, the premium for being the private chain goes away. Ironwood strengthens Zcash’s claim to that premium today by pairing strong cryptographic privacy with independent supply verification. Whether that positioning remains durable as the rest of the industry catches up is the real open question.


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
