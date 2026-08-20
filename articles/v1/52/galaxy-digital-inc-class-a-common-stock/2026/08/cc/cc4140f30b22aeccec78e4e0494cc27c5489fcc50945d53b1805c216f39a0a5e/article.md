---
schema_version: "1.0.0"
document_id: "cc4140f30b22aeccec78e4e0494cc27c5489fcc50945d53b1805c216f39a0a5e"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-3264694a8f1e"
canonical_url: "https://www.galaxy.com/insights/research/coldcard-exploit-abates-as-total-losses-climb-to-at-least-1700-btc"
published_at: null
first_seen_at: "2026-08-15T01:58:00.618900+00:00"
fetched_at: "2026-08-15T01:58:02.833093+00:00"
content_hash: "sha256:c92c149ca5494e942ea3f900cbf9ce5d9d287ec85c72f6ba270fa0b1973e3f48"
---

# Coldcard Exploit Abates as Total Losses Climb to (at Least) 1,700 BTC

*This article originally appeared in Galaxy Research's weekly newsletter.*[Subscribe](https://www.galaxy.com/subscribe-to-research?utm_campaign=Weekly%20Research%20Brief%20-%20Email&utm_medium=email&utm_source=hs_email&_hsenc=p2ANqtz--YWOkviRZJsa_uYNPjl2N4ahqTpHeV86Rs9VQQeL9MWn1U09L_xNxjRhoRyGaaBrCPLXuX) *to get timely insights delivered to your inbox every Friday morning.*


**New incidents of hackers exploiting vulnerabilities in the Coldcard hardware wallet have abated, though the tally of losses continues to climb as new victims come forward.**


Galaxy Research has now been in direct contact with 190 victims, whose reports have helped confirm with high confidence that 1778.84 BTC ($112.7m) has been stolen from more than 8,600 addresses. Notably, this number does not include several medium-confidence sets of possible thefts, such as the still unconfirmed “Wave 4.” If these candidate footprints and the suspected but unconfirmed Wave 4 were included, the loss would climb to 2,417.35 BTC ($153m).


For background, a Coldcard was a hardware wallet used mostly by ideological retail Bitcoiners for cold storage (storage of BTC without a centralized intermediary or custodian). On March 17, 2021, Coinkite (Coldcard’s manufacturer) upgraded the devices’ firmware to use a new random number generator for entropy insertion into the cryptographic key generation process. Due to a software bug, the implementation of the new RNG was faulty and the devices instead silently failed during key generation and fell back to a separate, catastrophically insufficient form of entropy. The resulting private keys created on the devices lacked sufficient entropy to defend against attackers who could use compute to regenerate the private keys. Attackers have been executing this attack since at least early morning July 30, 2026, systematically recreating Coldcard-generated seeds and sweeping the funds onchain.


Among the confirmed, high-confidence waves and footprints of attacker activity, none occurred after Aug. 6. Additional victims continue to report to Galaxy Research, helping us directly attribute losses and confirm additional footprints, but none have revealed confirmed attacker activity after Aug. 6. NOTE: the reason for the abatement in attack waves is likely because vulnerable users have migrated or most funds have already been drained. **If you still hold funds on a single-signature Coldcard wallet, you are advised to move your funds to new addresses.**


In addition to Waves 1, 2, and 3, with help from victim reports, Galaxy Research has identified at least 33 additional attacker footprints. We cannot confirm whether any of the waves or footprints are attributable to the same or different attackers, but we can say with high confidence that multiple attackers were active in the threat environment exploiting the Coldcard vulnerability.


The bulk of funds have remained largely unmoved since they were stolen from victims. Of the at least 1,778 BTC that has been stolen, 1,531 BTC remains in attacker-controlled addresses unmoved. ~246 BTC has been moved by attackers after the theft, with 65% flowing into Coinjoin mixing transactions and 35% continuing to move onwards onchain, sometimes through carefully constructed[peelchains](https://www.chainalysis.com/blog/common-blockchain-analysis-mistakes-cryptocurrency-investigations/) . A tiny amount can be traced, at least in part, to deposits into exchanges or interchain bridges.


Galaxy Research has provided lists of attacker addresses to crypto exchanges, compliance and investigative firms, and law enforcement agencies in hopes that they can be frozen if they appear at centralized intermediaries.


### **OUR TAKE**


This event has left the Bitcoin community reeling for a number of reasons. First, the nature of the attack is such that the victim cohort is comprised of some of the most philosophically aligned bitcoiners: long-term savers in BTC who believed in self-custody cold storage without an intermediary. These users did not lose their funds because they sent them to shady offshore exchanges, or bridged them to DeFi to chase a long tail of yield, or gambled their BTC for riskier assets. Second, while the loss just barely cracks the top 20 crypto thefts of all time (at $112m, it’s #20 just below Multichain’s July 2023 $130m heist and just above Harmony Horizon Bridge’s June 2022 $100m theft), the magnitude of the theft is unheard of for self-custody hardware wallets. Finally, Coldcard’s were widely promoted by Bitcoin maximalists as the most security-conscious hardware wallet, so its failure strikes at the core of the Bitcoin influencer intelligentsia’s multi-year educational and promotional efforts.


There is some evidence that the exploit has increased fear of and damaged the narrative about self-custody, at least for the near and medium terms. The count of small transfers (<1 BTC) flowing into exchanges reached a local high following the incident; inflows to exchanges[exceeded](https://x.com/SaniExp/status/2084234660414976062?s=20) 22k BTC in just the first four days after the attacks began; and as of Aug. 8, exchange balances stood at 3.683m BTC, an all-time high. Some traditional financial firms seemed to jump on the moment, with BlackRock[announcing](https://www.fxstreet.com/cryptocurrencies/news/blackrock-lowers-bitcoin-etf-in-kind-conversion-minimum-to-1m-202608110059) that the minimum to in-kind create IBIT ETF shares from native BTC had been lowered to $1m (from $25m).


But the event also highlighted the *safer* ways to do self-custody, with multisig emerging as a clear victor. Not one theft transaction stole funds from a multisig. One form of multisig that has found itself in the spotlight for self-custody, partly due to its redundancy but also its accessibility to less-technical users, is *collaborative multi-sig self-custody* , such as those services offered by Casa, Unchained, Nunchuk, Anchorwatch, Liana, and others.


“We’ve seen massive inbound in the two weeks since the Coldcard exploit was discovered,” said Casa CEO Nick Neuman. “New customer signups have surged and customers have moved substantial amounts of BTC into their Casa multisig vaults.”


Anchorwatch COO Becca Rubenfeld has seen similar growth in demand. “Anchorwatch saw an immediate uptick in new customer signups, as well as inflows from existing customers,” Rubenfeld told Galaxy Research. She noted that her company saw “its biggest single week of inflows since the company’s inception.”


“Framing this as a win for custodians and a loss for self-custody misses the point. The real risk is a single point of failure—whether that's a custodian, a hardware wallet manufacturer, or you,” Dhruv Bansal, co-founder of Unchained, told Galaxy Research. Unchained has seen “a meaningful surge in interested from longtime self-custody bitcoiners who want to distribute that risk without given control,” Bansal said.


In the wake of the attacks, Rob Hamilton (CEO of Anchorwatch), Calle (of the Cashu open source project), James O’Beirne, and ~25 others have been operating the Bitcoin Red Team, systematically sweeping crucial code repositories across the ecosystem for vulnerabilities and recommending patches. The renewed focus on code security is welcome but obviously more urgent in the age of AI. Our own research into the attackers suggests with high confidence that, even if they didn’t need to use it to discover the vulnerability (though they probably did), at least some of the attackers almost certainly utilized AI models without cyber safeguards (like the recently released, open-source Kimi K3 model) to carry out the attack. Hamilton told me on Galaxy Brains this week that “safety” policies from U.S. frontier labs have largely prevented the Red Team from utilizing frontier models to defend against the attacks, leaving him and his fellow security researchers to rely on the same Chinese open-source models as the attackers. It seems that, until now, the Bitcoin mantra of “don’t trust, verify” fell short – not nearly enough verification was being done. The Red Team is leading the charge to change that.


Bitcoin culture will evolve due to the Coldcard incident. The era of Bitcoin maximalist purity – one that included admonishment of those who do not self-custody, or who utilize hardware wallets that are not “Bitcoin only” (as Coldcard was), or that venerated non-technical influencer personalities above open-source developers with cypherpunk determinations – is very likely to come to an end in mainstream Bitcoin culture. To be clear, no one is to blame for this incident other than Coinkite, whose shoddy code introduced the bug and whose ostracization of the open source community contributed to its lack of discovery for five years – certainly not the victims, who did nothing wrong, and not the podcasters and influencers who promoted Coinkite hardware without having conducted deep security audits of the firmware themselves (an unreasonable standard, in my opinion).


But the Bitcoin social movement over the last six years, really starting during the Covid lockdowns, has reached a notable milestone, if not a pivot point or even bookend, as a result of the Coldcard exploit. The era of “contributing” to Bitcoin and converting people into bitcoiners solely through philosophical explanations, memetic platitudes, and, frankly, *s**tposting* , is over. Bitcoiners need to do more: audit more code, educate more clearly and effectively, and be less judgmental of newcomers who may not feel technologically capable of securing their own coins. The early evidence says this is already happening and that Bitcoin, and its community, will ultimately be stronger because of the incident. If the core tenets of self-custody, self-sovereignty, and transacting without intermediaries are to live on, this must happen. Because the centralized intermediaries are only growing bigger, stronger, and better at marketing their own versions of BTC exposure. **– Alex Thorn**


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
