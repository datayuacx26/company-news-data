---
schema_version: "1.0.0"
document_id: "1c13317a6c522b43272d8918ff76e9592469e7f36242c764f4cb3c08f539a91d"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-3264694a8f1e"
canonical_url: "https://www.galaxy.com/insights/research/open-letters-open-weights-openly-divided-ai-giants-stake-out-positions"
published_at: null
first_seen_at: "2026-07-31T23:15:35.285855+00:00"
fetched_at: "2026-07-31T23:15:36.493107+00:00"
content_hash: "sha256:29c492bc3ddd3c0f1b027fe2e40dd59752ac12eca190b675a1fc6f5ca6775642"
---

# Open Letters, Open Weights, Openly Divided: AI Giants Stake Out Positions

*This article originally appeared in Galaxy Research's weekly newsletter.*[Subscribe](https://www.galaxy.com/subscribe-to-research?utm_campaign=Weekly%20Research%20Brief%20-%20Email&utm_medium=email&utm_source=hs_email&_hsenc=p2ANqtz--YWOkviRZJsa_uYNPjl2N4ahqTpHeV86Rs9VQQeL9MWn1U09L_xNxjRhoRyGaaBrCPLXuX) *to get timely insights delivered to your inbox every Friday morning.*


**The AI industry has gone retro in its messaging, dueling over safety and governance in**[open](https://images.nvidia.com/pdf/Open-Weights-and-American-AI-Leadership.pdf) ****[letters](https://www.pacingthefrontier.com/) **, a medium employed by historical figures from**[Zola](https://jean-max-guieu.facultysite.georgetown.edu/other-interests/english-translation-of-emile-zolas-jaccuse) **to**[Martin Luther King Jr.](https://www.africa.upenn.edu/Articles_Gen/Letter_Birmingham.html)


Late last week, a cohort of American technology companies and investors including NVIDIA, Hugging Face, and a16z released[“Open Weights and American AI Leadership,”](https://images.nvidia.com/pdf/Open-Weights-and-American-AI-Leadership.pdf) urging against a national ban on Chinese AI models. Open-source and open-weight models are an important asset to “strengthen competition,” “expand access to the AI economy,” and make AI a “sustainable” business, the letter said. Open weights allow users to “match the…right job at the right cost” without “training one from scratch or paying frontier-model prices for everyday tasks.” One of the biggest proponents of an ecosystem underpinned by open-source models is Microsoft CEO Satya Nadella, one of the signers of the letter.[Nadella](https://x.com/satyanadella/article/2066182223213293753?lang=en) envisions a world where enterprise AI will shift away from raw model intelligence and toward converting company data into internal weights, evals, and reinforcement learning environments to create highly specialized models on top of open weights.


The letter acknowledges that open-weight models lack a central authority to control outputs but also notes open-source models serve as another line of defense for AI safety and security, as we saw this month where GLM 5.2 was used to[contain an AI-enabled intrusion](https://www.galaxy.com/insights/research/openai-hugging-face-hack) after closed-source models refused to help.


After Nvidia CEO Jensen Huang published it (in his[first-ever X post](https://x.com/JensenHuang/status/2080643682408321103) ) on July 24, the letter quickly gained traction and within a day the number of signatories doubled, picking up Google and even OpenAI along the way. As the hours went by, and more companies joined the list, one company remained conspicuous by its absence: Anthropic. The frontier lab broke its silence on Monday, releasing “[Our position on open-weights models](https://www.anthropic.com/news/position-open-weights-models) ” authored by founder and CEO Dario Amodei. In the essay, Amodei insisted that Anthropic's position was “never…for a ban on open-weights models.” Amodei outlined Anthropic’s concerns about Chinese access to American chips, industrial-scale distillation, and called for all “capable models, open and closed,” to go through “mandatory safety testing” before being released.


The following day,[Pacing the Frontier](https://www.pacingthefrontier.com/) , an employee petition signed by 1,200 frontier lab researchers, came out, calling for the U.S. government to support an international effort to "deliberately pace the frontier of automated AI development.” Similar to Amodei’s essay, the petition attempts to reframe the debate from closed vs. open models toward general AI safety and governance.


#### Our Take


These letters signal a paradigm shift. Since the release of ChatGPT in 2022, frontier intelligence has been an expensive and highly sought-after asset. Chinese open-source models have, for the first time, turned inference into a commodity that anyone—benevolent or nefarious—has access to.


In the span of just five weeks, Chinese labs have closed much of the capability gap between open- and closed-source models. In June, Moonshot AI, founded by Carnegie Mellon PhD Yang Zhilin, released[Kimi K2.7](https://huggingface.co/moonshotai/Kimi-K2.7-Code) , a one-trillion parameter model optimized for long-horizon coding agents—think Claude Code and Codex-style workflows. Four days later, Z.ai released[GLM 5.2](https://z.ai/blog/glm-5.2) which reached the highest marks at the time of any open-weight model on[Artificial Analysis' Intelligence Index](https://artificialanalysis.ai/#intelligence) —the leading independent benchmark.


The model that caught the attention of leading technology companies was Moonshot AI’s[Kimi K3](https://www.kimi.com/blog/kimi-k3) : a 2.8 trillion parameter model that became the largest open-model release when weights shipped earlier this week. Today, Kimi K3 is fourth on the[Intelligence Index](https://artificialanalysis.ai/#intelligence) , a point above Opus 4.8 and two points shy of GPT-5.6 Sol. On some[benchmarks](https://artificialanalysis.ai/models/kimi-k3) , Kimi K3 edges out Anthropic’s top frontier models. Without the frontier labs’ markups, it costs less than a third as much to run.


For the first time, enterprises are[routing the bulk of their internal queries](https://x.com/brian_armstrong/status/2070670644577280109) to open-source models and reserving the powerful, frontier models for the most intellectually rigorous workloads. Although Kimi K3[was not released under an Apache 2.0 or MIT license](https://github.com/MoonshotAI/Kimi-K3/blob/main/LICENSE) , the majority of organizations can fine-tune, deploy, and commercially incorporate the model without paying Moonshot AI.


Chinese models have rapidly undercut the economics of American inference, raising serious questions for the frontier labs. Some have claimed Chinese open-source models are a form of “dumping”—when foreign countries undercut American markets below cost of goods until the domestic industry collapses.[Rumors](https://www.axios.com/2026/07/20/ai-us-china-open-source-kimi) arose that the White House and National Security Agency were considering cutting off or putting out an “advisory” note on Chinese AI labs to discourage U.S. companies from using their tech.


Each company may claim it's acting for the good of mankind, but it would be naive to ignore business interests shaping their stances, consciously or not. Anthropic, which didn't sign the Open Weights letter, and OpenAI, which did but still guards its closed frontier models, both stand to benefit from regulatory moats if open models face restrictions. Nvidia's interest runs the other way: as the leading chipmaker, it benefits from more people running more models, period, regardless of whose model or how open. And other signatories have their own motive for signing: a fighting chance to catch up to the frontier.


There are serious arguments on all sides of this debate, but we lean toward openness. We believe no single company or government should hold a monopoly (or duopoly or oligopoly) on deciding what is best for the future of frontier intelligence. As Meta CEO Mark Zuckerberg put it in a[recent piece](https://www.wsj.com/opinion/the-ai-future-is-for-everyone-a0c24e20?mod=hp_opin_pos_2) for *The Wall Street Journal* : "Historically, hoping that an absolute power will benevolently provide for humanity if sufficiently enlightened hasn't led to safe or positive outcomes."


Or, as Voltaire put it: “Doubt is uncomfortable. But certainty is absurd.”[– Taj Singh](https://www.linkedin.com/in/tajinders)


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
