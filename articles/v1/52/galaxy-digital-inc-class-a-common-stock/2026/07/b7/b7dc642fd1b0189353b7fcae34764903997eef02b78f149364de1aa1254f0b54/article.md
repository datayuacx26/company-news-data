---
schema_version: "1.0.0"
document_id: "b7dc642fd1b0189353b7fcae34764903997eef02b78f149364de1aa1254f0b54"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-3264694a8f1e"
canonical_url: "https://www.galaxy.com/insights/research/open-weights-ai-debate-tradexyz-oracle-liquidations-zcash-ironwood-upgrade"
published_at: null
first_seen_at: "2026-07-31T23:15:35.285855+00:00"
fetched_at: "2026-07-31T23:15:36.493107+00:00"
content_hash: "sha256:ec753bbbcb3a272cb20012eb7e5836fd4f480cf8c8f562bbc1ac444c35aabd10"
---

# Weekly Research Brief: AI's Civil War, a $60m Oracle Anomaly, and Zcash's Ironwood Upgrade

*Welcome to Galaxy Research's Weekly Research Brief.*[Subscribe](https://www.galaxy.com/subscribe-to-research?utm_campaign=Weekly%20Research%20Brief%20-%20Email&utm_medium=email&utm_source=hs_email&_hsenc=p2ANqtz--YWOkviRZJsa_uYNPjl2N4ahqTpHeV86Rs9VQQeL9MWn1U09L_xNxjRhoRyGaaBrCPLXuX) *to get this newsletter delivered to your inbox every Friday morning.*


*Got feedback on this newsletter? Email*[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection#daa8bfa9bfbba8b9b29abdbbb6bba2a3f4b9b5b7) *. We’d love to hear from you.*


## Open Letters, Open Weights, Openly Divided: AI Giants Stake Out Positions


**The AI industry has gone retro in its messaging, dueling over safety and governance in**[open](https://images.nvidia.com/pdf/Open-Weights-and-American-AI-Leadership.pdf) ****[letters](https://www.pacingthefrontier.com/) **, a medium employed by historical figures from**[Zola](https://jean-max-guieu.facultysite.georgetown.edu/other-interests/english-translation-of-emile-zolas-jaccuse) **to**[Martin Luther King Jr.](https://www.africa.upenn.edu/Articles_Gen/Letter_Birmingham.html)


Late last week, a cohort of American technology companies and investors including NVIDIA, Hugging Face, and a16z released[“Open Weights and American AI Leadership,”](https://images.nvidia.com/pdf/Open-Weights-and-American-AI-Leadership.pdf) urging against a national ban on Chinese AI models. Open-source and open-weight models are an important asset to “strengthen competition,” “expand access to the AI economy,” and make AI a “sustainable” business, the letter said. Open weights allow users to “match the…right job at the right cost” without “training one from scratch or paying frontier-model prices for everyday tasks.” One of the biggest proponents of an ecosystem underpinned by open-source models is Microsoft CEO Satya Nadella, one of the signers of the letter.[Nadella](https://x.com/satyanadella/article/2066182223213293753?lang=en) envisions a world where enterprise AI will shift away from raw model intelligence and toward converting company data into internal weights, evals, and reinforcement learning environments to create highly specialized models on top of open weights.


The letter acknowledges that open-weight models lack a central authority to control outputs but also notes open-source models serve as another line of defense for AI safety and security, as we saw this month where GLM 5.2 was used to[contain an AI-enabled intrusion](https://www.galaxy.com/insights/research/openai-hugging-face-hack) after closed-source models refused to help.


After Nvidia CEO Jensen Huang published it (in his[first-ever X post](https://x.com/JensenHuang/status/2080643682408321103) ) on July 24, the letter quickly gained traction and within a day the number of signatories doubled, picking up Google and even OpenAI along the way. As the hours went by, and more companies joined the list, one company remained conspicuous by its absence: Anthropic. The frontier lab broke its silence on Monday, releasing “[Our position on open-weights models](https://www.anthropic.com/news/position-open-weights-models) ” authored by founder and CEO Dario Amodei. In the essay, Amodei insisted that Anthropic's position was “never…for a ban on open-weights models.” Amodei outlined Anthropic’s concerns about Chinese access to American chips, industrial-scale distillation, and called for all “capable models, open and closed,” to go through “mandatory safety testing” before being released.


The following day,[Pacing the Frontier](https://www.pacingthefrontier.com/) , an employee petition signed by 1,200 frontier lab researchers, came out, calling for the U.S. government to support an international effort to "deliberately pace the frontier of automated AI development.” Similar to Amodei’s essay, the petition attempts to reframe the debate from closed vs. open models toward general AI safety and governance.


#### Our take


These letters signal a paradigm shift. Since the release of ChatGPT in 2022, frontier intelligence has been an expensive and highly sought-after asset. Chinese open-source models have, for the first time, turned inference into a commodity that anyone—benevolent or nefarious—has access to.


In the span of just five weeks, Chinese labs have closed much of the capability gap between open- and closed-source models. In June, Moonshot AI, founded by Carnegie Mellon PhD Yang Zhilin, released[Kimi K2.7](https://huggingface.co/moonshotai/Kimi-K2.7-Code) , a one-trillion parameter model optimized for long-horizon coding agents—think Claude Code and Codex-style workflows. Four days later, Z.ai released[GLM 5.2](https://z.ai/blog/glm-5.2) which reached the highest marks at the time of any open-weight model on[Artificial Analysis' Intelligence Index](https://artificialanalysis.ai/#intelligence) —the leading independent benchmark.


The model that caught the attention of leading technology companies was Moonshot AI’s[Kimi K3](https://www.kimi.com/blog/kimi-k3) : a 2.8 trillion parameter model that became the largest open-model release when weights shipped earlier this week. Today, Kimi K3 is fourth on the[Intelligence Index](https://artificialanalysis.ai/#intelligence) , a point above Opus 4.8 and two points shy of GPT-5.6 Sol. On some[benchmarks](https://artificialanalysis.ai/models/kimi-k3) , Kimi K3 edges out Anthropic’s top frontier models. Without the frontier labs’ markups, it costs less than a third as much to run.


For the first time, enterprises are[routing the bulk of their internal queries](https://x.com/brian_armstrong/status/2070670644577280109) to open-source models and reserving the powerful, frontier models for the most intellectually rigorous workloads. Although Kimi K3[was not released under an Apache 2.0 or MIT license](https://github.com/MoonshotAI/Kimi-K3/blob/main/LICENSE) , the majority of organizations can fine-tune, deploy, and commercially incorporate the model without paying Moonshot AI.


Chinese models have rapidly undercut the economics of American inference, raising serious questions for the frontier labs. Some have claimed Chinese open-source models are a form of “dumping”—when foreign countries undercut American markets below cost of goods until the domestic industry collapses.[Rumors](https://www.axios.com/2026/07/20/ai-us-china-open-source-kimi) arose that the White House and National Security Agency were considering cutting off or putting out an “advisory” note on Chinese AI labs to discourage U.S. companies from using their tech.


Each company may claim it's acting for the good of mankind, but it would be naive to ignore business interests shaping their stances, consciously or not. Anthropic, which didn't sign the Open Weights letter, and OpenAI, which did but still guards its closed frontier models, both stand to benefit from regulatory moats if open models face restrictions. Nvidia's interest runs the other way: as the leading chipmaker, it benefits from more people running more models, period, regardless of whose model or how open. And other signatories have their own motive for signing: a fighting chance to catch up to the frontier.


There are serious arguments on all sides of this debate, but we lean toward openness. We believe no single company or government should hold a monopoly (or duopoly or oligopoly) on deciding what is best for the future of frontier intelligence. As Meta CEO Mark Zuckerberg put it in a[recent piece](https://www.wsj.com/opinion/the-ai-future-is-for-everyone-a0c24e20?mod=hp_opin_pos_2) for *The Wall Street Journal* : "Historically, hoping that an absolute power will benevolently provide for humanity if sufficiently enlightened hasn't led to safe or positive outcomes."


Or, as Voltaire put it: “Doubt is uncomfortable. But certainty is absurd.”[– Taj Singh](https://x.com/taj_trader?s=21&t=1CjMtbiwlPDKUja707Vphw)


## How One Share in Seoul Broke Hyperliquid’s Largest Equity Market


**TradeXYZ, the largest deployer of HIP-3 perpetual futures markets on Hyperliquid, is**[reimbursing](https://x.com/tradexyz/status/2082260930751082821?s=20) **traders for liquidations resulting from an accurate but anomalous third-party price feed reading. Notably, the hiccup came from the traditional financial system, not crypto.**


At 8:00 a.m., Seoul time on July 28, a single share of chip manufacturer SK Hynix changed hands for 1,272,000 won (roughly $868) in the opening seconds of South Korea’s NextTrade pre-market session. That was 29.96% below the previous close of 1,816,000 won, right at the daily lower price limit. Buy orders arrived about two minutes later and the stock recovered to the 1.7 million won range. By then the print had already traveled 5,500 miles, onto an order book in a different asset class, and taken out roughly $60 million in leveraged long positions.


The market in question is xyz:SKHYNIX, a USDC-margined perpetual offering up to 10x leverage, deployed on Hyperliquid by TradeXYZ under the HIP-3 upgrade. HIP-3 has been live on mainnet since October 2025 and allows any team that stakes 500,000 HYPE (~$27 million at recent prices) to deploy its own perpetual futures markets on HyperCore, Hyperliquid’s onchain order book and matching layer. The deployer defines the market (oracle, leverage limits, margin mode, and contract spec). Hyperliquid supplies execution and settlement, splits the fees, and does not pick the price.


TradeXYZ, the perps arm of tokenization protocol Unit, was the first HIP-3 deployment and essentially has a monopoly on the category. It accounts for more than 90% of HIP-3 open interest and ~98% of builder-market volume. That matters at scale: HIP-3's share of total Hyperliquid perp volume has climbed from roughly 2% at the start of the year to around half today, with 30-day builder volume of ~$98 billion and network-wide HIP-3 open interest at ~$3.6 billion. SK Hynix is the top HIP-3 market in open interest, with $638 million as of July 30.


The contract does exactly what its documentation says: it tracks the U.S. dollar value of one SK Hynix common share. TradeXYZ splits Korean pricing into two regimes: an internal phase, where the oracle drifts off its own order book, and an external phase that begins the moment NXT opens at 8:00 a.m. Korea Standard Time. The anomalous print landed precisely at that handoff. Onchain records show the oracle component submitting an external price of $868.17 seconds after the switch.


TradeXYZ’s mark price is the median of three inputs: the oracle price, the oracle plus a 150-second exponential moving average of the book’s deviation from it, and the median of best bid, best ask, and last trade. That smoothing absorbed about 11 percentage points of a 30% corrupted input. It was not enough. The mark fell 18.7%, effectively instantaneously. Open interest in the contract dropped from $481 million to $331 million in a few minutes. Onchain analysts put liquidated notional amounts between $57 million and $80 million across roughly 960 long accounts; profitable shorts were auto deleveraged on the way through (similar to what happened during[10/10](https://www.galaxy.com/insights/research/cryptos-flash-crash-liquidation-binance-adl-auto-deleveraging) ), and a system backstop address absorbed 406 long positions before being liquidated itself.


TradeXYZ[published a response](https://x.com/tradexyz/status/2082260930751082821?s=20) on July 29, saying it would cover liquidation losses attributable to the anomaly, framed explicitly as a “one-time discretionary decision” rather than a standing policy, with eligibility criteria and distributions to follow. It also maintained that the oracle “worked as intended according to its specification” (which is true) and said it is accelerating work on price formation during tail events, including weighting its own order books, which it argues now carry meaningful depth relative to external sources.


SK Hynix was genuinely under pressure on AI-spending and Chinese memory competition fears. The stock closed the regular Seoul session down 14.65% at 1.55 million won. It reported Q2 results the next day and fell again, triggering circuit breakers on consecutive trading days for the first time in the history of the Korean market. A trader watching the tape that morning had every reason to believe the crash was real.


#### Our take


The oracle worked. The risk system didn’t. Traditional markets separated last trade, index price, and fair value for risk purposes decades ago, precisely so one local execution cannot decide the fate of a leveraged account. TradeXYZ had the architecture (a three-input median, EMA smoothing, discovery bounds) and it absorbed roughly 11 percentage points of a corrupted print before giving up. The protection was real. It was just calibrated for a reference market with more depth than the one it was actually pointed at. Correct price discovery is not the same thing as sound liquidation design, and a spec can be followed faithfully all the way into a bad outcome.


The failure mode itself is not a crypto problem. NexTrade is already patching it from the other side with a static volatility interruption in September. What onchain added was transmission speed and leverage on the receiving end. The print was real, the data providers were correct, and every module did its job. This is why “add more oracles” is the wrong fix. Every provider was reading the same NXT book and the median across them converges on the same $868.


The JELLYJELLY memecoin is a useful historical example on the accountability question. In March 2025, Hyperliquid’s own vault was underwater, validators froze the market and force-settled positions at a chosen price, and the protocol spent the next year answering for it. In the South Korea case, the core protocol didn’t touch anything. The deployer that chose the oracle absorbed the cost. What made traders whole is the generosity of the TradeXYZ team. Traders will be made whole because a company with a 90%-plus share of a category it spent nine months building decided its franchise was worth more than the check. TradeXYZ would have had a totally valid argument for paying nothing and didn’t make it.


It will be interesting to watch what happens next. TradeXYZ says it will weight its own order books more heavily against external feeds. Onchain venues mature by referencing the real-world market at the outset and then progressively becoming the reference. Equity perps are further along that curve than most people assume, and this incident shows it. The external (TradFi) feed was the fragile input, and the internal (onchain) book was the stable one.


Hyperliquid’s shared infrastructure is what makes this transition survivable. No builder here has to write a matching engine, a margin system, or a liquidation engine. TradeXYZ had exactly one hard problem to solve, and it got a $60 million tuition bill on the one thing it owned. Against a category doing $98 billion a month, that’s cheap.[- Will Owens](https://x.com/wowens)


## Zcash’s Ironwood Upgrade Sharpens Chain’s Privacy Features


**Zcash**[rolled out](https://x.com/zodl_co/status/2082106258740420648) **its Ironwood network upgrade Tuesday, permanently sealing the blockchain’s previous primary private transaction pool, called Orchard, so that it can no longer accept deposits or internal transfers, and opening a private pool named Ironwood.**


In Zcash, activity can occur in either transparent (public) addresses, similar to Bitcoin’s, or in shielded pools. Shielded pools use zero-knowledge cryptography to hide the amounts being transferred and the identities of the parties involved while still allowing the network to verify that transactions are valid. Orchard had been the network’s main shielded pool since its launch in May 2022 and held approximately 3.66 million ZEC (worth roughly $1.7 billion) at the moment of the upgrade. For a full overview of Zcash, refer to Galaxy’s[prior research here](https://www.galaxy.com/insights/research/zcash-price-zec-near-intents-zashi-wallet-privacy-zero-knowledge-proofs) .


Ironwood was developed after Shielded Labs researcher Taylor Hornby found a soundness vulnerability in Orchard’s zero-knowledge proof circuit on May 29. The flaw had existed since Orchard’s launch in 2022 and could theoretically have allowed the creation of counterfeit ZEC that would be undetectable on the public blockchain. An emergency upgrade in early June patched the[circuit](https://zkjargon.github.io/definitions/circuit.html) for new transactions. Ironwood goes further by creating a shielded pool that uses the corrected circuit and by restricting Orchard so that funds can leave only through a longstanding Zcash mechanism known as the turnstile. This is an accounting rule at the boundary of a shielded pool that ensures no more ZEC can exit than the amount that was verifiably deposited into it.


Users holding funds in Orchard must migrate them to the Ironwood pool voluntarily through updated wallets; the migration is not automatic. As of July 30, public trackers showed roughly 410,000–413,000 ZEC already in the Ironwood pool and about 3.29 million ZEC still in Orchard.


#### Our take


A soundness flaw in a shielded pool is uniquely challenging. The cryptography that hides amounts and parties also hides any possible counterfeiting. You cannot see the flaw being used. You can only see, eventually, that the supply is wrong. Ironwood closes that gap with the turnstile by sealing the old pool and forcing every coin that wants to keep circulating through an accounting checkpoint. This means more value cannot leave the pool than went into it. Any counterfeit that might exist is now permanently quarantined.


The response by the Zcash developer community is impressive. Hornby found a four-year-old bug with AI-assisted tooling, the circuit was patched nearly immediately post discovery, and Ironwood deployed two months after disclosure with a formally verified pool and quantum-recoverable notes. Formal verification of counterfeiting risk in production is still rare.


The open variable now is migration speed. Roughly 11% of the old Orchard balance has already moved in the first two days, which is a solid start. The turnstile makes this a queue rather than an adoption metric. If the pool is short, the shortfall lands on whoever leaves last. That creates an incentive to move coins early. But Ironwood's anonymity set creates an incentive to wait, because early arrivals hide in a much smaller crowd. Until a critical mass of funds and daily activity sits in Ironwood, the effective private liquidity of the Zcash network remains split between two pools. Ultimately, if Orchard empties without the turnstile flagging any excess supply, that would prove the flaw was never used and put to rest the May supply question. Zcash in this scenario would emerge stronger thanks to new cryptographic assurances, independent supply verifiability, and a clearer long-term path to quantum recoverability. For allocators who want exposure to the privacy thesis, the residual risk would shift from "is the supply real?" to the more ordinary questions of adoption, liquidity, and execution.


Stepping back, the deeper question is why privacy is increasingly a non-negotiable feature across crypto. Two forces that usually talk past each other are converging. On one side is the classic cypherpunk view that privacy is an inherent right and that there is demand for digital bearer instruments with privacy baked into the base layer. Zcash is the market’s current favorite expression of that thesis. As the team put it, "Financial freedom requires financial privacy." On the other side is the institutional reality. Fully transparent public chains are simply unusable for serious finance. Counterparties will not run strategies, manage inventory, or settle size in the open. That is why privacy now sits at the center of Ethereum's Strawmap, Solana's infrastructure priorities, and nearly every major blockchain's roadmap. Privacy is no longer just a category within crypto. It is becoming a requirement of the base layer.


The uncomfortable implication for Zcash is that if privacy becomes a property of every chain, the premium for being the private chain goes away. Ironwood strengthens Zcash’s claim to that premium today by pairing strong cryptographic privacy with independent supply verification. Whether that positioning remains durable as the rest of the industry catches up is the real open question.[-Lucas Tcheyan](https://x.com/Uptodatenow)


## Other News


-


🚨 Flaw in[Coldcard Mk3 hardware wallets](https://www.theblock.co/post/410235/coinkite-coldcard-mk3-warning) leads to[draining](https://atlas21.com/coldcard-bug-weak-seeds-generated-since-2021-funds-are-at-risk-move-them-now/) of 594 BTC ([~$38m](https://www.coindesk.com/tech/2026/07/31/major-bitcoin-wallet-flaw-drains-594-btc-in-25-minute-sweep) )


-


🏦 BNY launches transfer agency “[on a public blockchain](https://www.bny.com/corporate/global/en/about-us/newsroom/press-release/bny-launches-global-digital-transfer-agency-capabilities-extending-leadership-in-fund-servicing-to-digital-market.html) ,” doesn’t say which one


-


🐦‍🔥 Ionic, BTC miner and AI infra firm built from Celsius’ ashes,[debuts](https://www.theblock.co/post/409945/ionic-digital-jumps-22-nasdaq-debut-celsius-bitcoin-mining-assets-ai-infrastructure) on Nasdaq


-


🔬 AMD[signs](https://www.reuters.com/business/core-scientific-signs-ai-infrastructure-deal-with-amd-2026-07-28/) deal leasing AI data center capacity from Core Scientific...


-


🤠 ...as Meta and BlackRock[partner](https://www.theinformation.com/briefings/meta-blackrock-partner-14bn-el-paso-data-center) on $14bn El Paso data center...


-


🇰🇷 ...and Nvidia plans to invest $1b in South Korean[data center infrastructure](https://nvidianews.nvidia.com/news/naver-nvidia-and-brookfield-to-expand-koreas-national-ai-factory-infrastructure-buildout) ...


-


👷‍♂️ ... *and* Hyperscale Data[sells](https://www.theblock.co/post/410148/hyperscale-data-sells-100-bitcoin-to-fund-michigan-ai-data-center-build) 100 BTC to fund Michigan AI data center build


-


🕵️ Ondo[scraps](https://www.coindesk.com/business/2026/07/28/ondo-drops-tokenized-asset-blockchain-plans-for-private-high-speed-trading-network) plan to build own chain in favor of private, offchain trading network


-


🏀 Sports chain Fanatics buys CFTC-regulated firm, brings prediction market[in-house](https://www.fanaticsinc.com/press-releases/fanatics-to-acquire-exchange-and-clearinghouse-from-bgc-parties-to-partner-on-prediction-markets) ...


-


🔮 ... as Binance US plans to[seek CFTC license](https://www.theblock.co/post/410110/binance-us-cftc-license) for prediction market platform


## Charts of the Week: Solana Sweeteners


Solana perpetuals exchange Phoenix Trade recorded back-to-back record days for volume and open interest after announcing a rewards program that will pay traders $420,000 in USDC over the coming month.


The lesson is simple: incentives matter, not least of all on Solana, whose OI share of the perps market remained small in Q2 (despite a volume surge), as the Drift hack and the end of Jupiter’s annual airdrop removed enticements for trading. Even with the new incentives campaign, Phoenix reported only ~1,300 traders using the platform. That’s peanuts compared to leaders like Hyperliquid that report upwards of 70k daily users.


Stay tuned for more insights in Galaxy Research’s quarterly Solana checkup, coming soon.


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
