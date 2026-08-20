---
schema_version: "1.0.0"
document_id: "a237d4c25cf58a238be9c71fc9c46fb49a11f568660c53b82c8b80cbb28b6fcf"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-3264694a8f1e"
canonical_url: "https://www.galaxy.com/insights/research/the-bitcoin-fork-that-lasted-two-blocks"
published_at: null
first_seen_at: "2026-08-15T01:58:00.618900+00:00"
fetched_at: "2026-08-15T01:58:02.833093+00:00"
content_hash: "sha256:94f143690fdf3506000bf9add18dd4b20a976e374c59161b04006f9698574824"
---

# The Bitcoin Fork That Lasted Two Blocks

*This article originally appeared in Galaxy Research's weekly newsletter.*[Subscribe](https://www.galaxy.com/subscribe-to-research?utm_campaign=Weekly%20Research%20Brief%20-%20Email&utm_medium=email&utm_source=hs_email&_hsenc=p2ANqtz--YWOkviRZJsa_uYNPjl2N4ahqTpHeV86Rs9VQQeL9MWn1U09L_xNxjRhoRyGaaBrCPLXuX) *to get timely insights delivered to your inbox every Friday morning.*


**The soft fork that was supposed to save Bitcoin from “spam"**[lasted two blocks](https://www.coindesk.com/tech/2026/08/09/controversial-bitcoin-fork-bip-110-mines-two-blocks-then-stops) **.**


BIP-110, formally the Reduced Data Temporary Softfork, was the brainchild of pseudonymous developer Dathon Ohm with input from Luke Dashjr, creator of the Bitcoin Knots node software, former chairman and CTO of the Ocean mining pool, and a polarizing figure in the Bitcoin community. It called for a one-year consensus-level restriction on arbitrary data in Bitcoin transactions. This includes Ordinals inscriptions (Bitcoin’s answer to NFTs), BRC-20 and Runes (both fungible token standards), and oversized[OP_RETURN](https://www.galaxy.com/insights/research/bitcoin-wallet-opreturn-salomon-brothers-abandoned-property-constructive-possession) . The proposal contained seven rules capping most outputs at 34 bytes, OP_RETURN at 83, data pushes at 256, plus restrictions on Taproot annexes and control blocks.


The activation design targeted 55% miner signaling (1,109 of 2,016 blocks). By comparison, Bitcoin’s SegWit upgrade in 2017 wanted 95% and Taproot in 2021 wanted 90%. If 55% wasn’t reached, a mandatory signaling window opened at block 961,632, where enforcing nodes would reject any block that didn’t signal, regardless of hashrate.


Miners never came close. In the last difficulty period before the window opened, 51 of 2,016 blocks signaled. That is 2.53% against a threshold its own authors had already cut nearly in half from the standard. According to[block data](https://bitcoinmagazine.com/bip-110-signaling-monitor) , every one of those 51 blocks came from Ocean.


Block 961,632 landed on Aug. 8 at 19:35:55 UTC. AntPool mined a non-signaling block that the network accepted, and BIP-110 nodes rejected it.[Roughnecks](https://x.com/Roughnecks110/status/2086300532709409140?s=20) , mining through Ocean’s DATUM system, produced the competing block.


The minority chain reached 961,633 and stopped. Roughnecks quit the next day and told other miners to do the same, then reversed on Aug. 10 and resumed, saying it would keep going until a “sensible POW change” arrived. The main Bitcoin chain is now several hundred blocks ahead. Strategy Executive Chairman[Michael Saylor commented](https://x.com/saylor/status/2086388338521497714?s=20) on the pitiful fork attempt, saying “Bitcoin worked exactly as designed” (by remaining unaffected in the face of the ill-fated fork attempt).


Ocean was the only pool signaling in favor of BIP-110. Days after the split, it disclosed that a[Stratum](https://bitcoinmagazine.com/glossary/stratum-v2) configuration error had routed some of its miners onto the BIP-110 chain (rather than the *real* Bitcoin chain) for roughly 18 hours. The pool restored its default endpoint and committed to roughly 0.3 BTC in direct rebates within 72 hours. Cryptographer and Bitcoin OG[Adam Back suggested](https://x.com/adam3us/status/2086648078845493425?s=20) the losses come out of Dashjr’s salary. Ocean’s reported hashrate has since fallen 96%, and miners have publicly called for leadership changes.


On Aug. 9, Bitcoin Improvement Proposal editor[Mark “Murch” Erhardt filed a motion](https://x.com/murchandamus/status/2086528140361515243?s=20) to the Bitcoin development mailing list recommending that fellow editor Luke Dashjr be removed as an editor. Erhardt cited Dashjr’s conflict of interest, minimal contribution, championing a contentious fork, and a coordination breakdown with the other editors.


The next day, it was done. Jon Atack merged the pull request (a one-line deletion) and longtime Bitcoin Core contributor Bryan Bishop confirmed the repository permissions were revoked. Bishop, Atack, Erhardt, Olaoluwa Osuntokun, and Ruben Somsen remain as BIP editors. Dashjr also[announced a sabbatical](https://x.com/LukeDashjr/status/2086919123599036926?s=20) from Ocean. He continues to[refer to the majority chain as “Bpedo.”](https://x.com/LukeDashjr/status/2087183652555817033?s=20) (One of his arguments for limiting arbitrary data was the alleged risk of child sexual abuse material being published onchain.)


Dathon Ohm’s response to the 2.53% tally was to accuse the large pools of collusion and announce work on a proof-of-work change to “fire the miners.” Dashjr backed it,[citing ASICBoost](https://x.com/LukeDashjr/status/2073499243638116722?s=20) when pressed on what was wrong with SHA-256d, the hash function at the core of Bitcoin. (ASICBoost is a technique for speeding up mining.)


### OUR TAKE


Ocean’s entire pitch was miner sovereignty. DATUM exists so operators build their own block templates locally instead of trusting pool operators’ block construction. That is a useful product solving a real centralization problem. Then a config error pointed unassigned hashrate at the chairman's fork for 18 hours without operator consent, and the pool ended up writing rebate checks. A movement that spent two years arguing pools are dangerous intermediaries proved its thesis at its own expense.


The soft fork itself was never a “neutral anti-spam patch” as its proponents claimed. It was a consensus-level attempt to invalidate fee-paying transactions a minority of the Bitcoin community disliked. On top of that, the threshold for acceptance was cut from 95% to 55% and a mandatory window was designed to split the network if even that low bar wasn’t cleared. Lowering the threshold manufactured a quixotic attempt at a chain that needs ([by Saylor’s math](https://x.com/saylor/status/2086388338521497714?s=20) ) 25 years to reach its first difficulty adjustment.


BIP 148 (whose activation in 2017 is celebrated as Bitcoin Independence Day) worked because exchanges, wallets, businesses, and holders were already behind it. Miners capitulated to an economic majority that had moved without them. A UASF (user-activated soft fork) is a mechanism for the economic majority to route around miners. It is *not* a mechanism for ~2.5% to route around everyone.


The PoW pivot makes this all worse for Ocean. Miners rejected BIP-110, and the response was to try to change the electorate. And ASIC resistance may not even be the point.[Per one of the Knots strategy channels](https://x.com/RobinSeyr/status/2086580715714658472?s=20) , the stated logic was to adopt an altcoin’s algorithm so ASICs would exist for the new chain on day one. Basically, cannibalize the hardware of an asset Bitcoiners would call valueless (they use an indelicate term for these; it rhymes with “bitcoin”). Bitcoin Gold ran this experiment in 2017 and nobody bothers citing it as a cautionary tale anymore.


*Luke Dashjr laying out proof-of-work design criteria in the Bitcoin Knots Discord channel. Source:*[Bitcoin.com News](https://news.bitcoin.com/crypto-news/bip-110-backers-plot-minority-chains-pow-reset-to-fire-bitcoin-miners/)


What to watch is whether Ocean’s 96% drawdown proves permanent. The pool building the most credible answer to Bitcoin mining centralization bet itself on a fork its chairman championed, and lost.


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
