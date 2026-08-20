---
schema_version: "1.0.0"
document_id: "edc7796bd94c6a9ee608b715c969add19d49bd764af374e55b14699f395d8317"
company_key: "tradeweb-markets-inc-class-a-common-stock"
company: "Tradeweb Markets Inc."
source_id: "tradeweb-markets-inc-class-a-common-stock-news-import-d4998d707856"
canonical_url: "https://www.tradeweb.com/newsroom/media-center/insights/blog/information-leakage-in-us-credit-evidence-from-all-to-all-rfqs/"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-24T04:29:24.716036+00:00"
fetched_at: "2026-07-28T22:13:17.137376+00:00"
content_hash: "sha256:d1100d88c9d968f75a537abfa02e2d5a559716cab362477708b458f4f64b341e"
---

# Information Leakage in US Credit: Evidence from All-to-All RFQs

May 2026


*By Filippo Caretti, Harveer Mahajan, Ted Husveth*


[Download the full whitepaper (PDF)](https://www.tradeweb.com/49a042/globalassets/newsroom/market-insights/2026/5.13.26-credit-whitepaper/information-leakage-in-us-credit-5.13.26b-final-1.pdf)


####


#### Abstract


In this paper, we estimate the relative market impact of two widely used trading protocols within the U.S. corporate bond market: All-to-All (A2A) request-for-quote (RFQ) trading and Voice (bilateral) processed trades. Specifically, we implement a generalized linear model (GLM) with a balanced weighting scheme to estimate the conditional difference in cumulative spread changes between RFQ acceptance and public reporting on TRACE. Our analysis leverages Tradeweb’s internal RFQ datasets alongside Ai-Price, a proprietary pricing algorithm that provides dense and consistent intraday price estimates. We focus exclusively on investment grade (IG) trades with notional values exceeding $1 million. The motivation for this analysis stems from Tradeweb’s central role in an increasingly electronic corporate bond market, where rising transparency has potentially amplified the cost of systemic inefficiencies, such as the broadcasting of trading intent. Our findings indicate that the A2A trading protocol, although proven to have better execution quality, is statistically associated with relatively greater information leakage compared to equivalent bilateral trades. This result supports the development of trading protocols such as SNAP+, which aim to preserve competitive pricing while reducing the diffusion of sensitive trading information inherent to one-to-many RFQ structures.


#### Introduction


Corporate bond markets have traditionally been characterized by an over-the-counter (OTC) market structure and limited transparency. Over time, regulatory initiatives, technological advancements, and new trading protocols have improved the timeliness, granularity and accessibility of transaction data. At the same time, credit market participants have become more sophisticated, though corporate bond markets still lag other asset classes in terms of automation and efficiency. As markets continue to evolve, so too do opportunities that arise from inefficiencies related to how trading intentions are revealed. This paper focuses on one of the central risks in increasingly electronic corporate bond markets: information leakage. We analyze how different trading protocols affect the visibility of clients’ trading interests to the broader market, and how that visibility can shape short-term price dynamics. We focus on two dominant protocols: All-to-All request-for-quotes (A2A RFQs) and bilateral trading or Voice Processing (Voice). A2A RFQs enable clients to broadcast a trading intent to an extensive set of potential counterparties, including anonymous buy-side participants. This approach offers deeper liquidity and more competitive pricing, often resulting in tighter bid-offer spreads. However, this broader dissemination of intent introduces a structural drawback: increased exposure may lead to information leakage, amplifying same-direction short-term price pressure. In contrast, Voice trading involves bilateral negotiations with a single dealer, limiting information spillover, but also narrowing the pool of liquidity providers, which may result in less competitive pricing and higher adverse selection risk. While prior research highlights the efficiency benefits of A2A RFQs, the central objective of this paper is only to quantify and compare the degree of information leakage between A2A RFQs and Voice trades. We argue that as electronification continues, the trade-off between transparency, competition, and information control will become increasingly important for both buy-side and sell-side participants. Our analysis contributes to this debate by providing empirical evidence on how disclosure through A2A protocols shapes short-term price dynamics, relative to more traditional Voice execution.
