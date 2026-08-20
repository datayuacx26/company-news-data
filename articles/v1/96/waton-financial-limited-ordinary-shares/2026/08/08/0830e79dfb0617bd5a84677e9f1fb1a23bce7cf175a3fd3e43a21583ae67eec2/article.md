---
schema_version: "1.0.0"
document_id: "0830e79dfb0617bd5a84677e9f1fb1a23bce7cf175a3fd3e43a21583ae67eec2"
company_key: "waton-financial-limited-ordinary-shares"
company: "Waton Financial Limited"
source_id: "waton-financial-limited-ordinary-shares-news-import-1e307eb61c1b"
canonical_url: "https://www.globenewswire.com/news-release/2026/08/10/3341768/0/en/waton-financial-highlights-alphaschema-research-progress-in-structured-ai-assisted-trading-signal-discovery.html"
published_at: "2026-08-10T12:00:00+00:00"
first_seen_at: "2026-08-10T19:12:53.493170+00:00"
fetched_at: "2026-08-10T19:12:55.278084+00:00"
content_hash: "sha256:6dd91a4d268dc0c7ce2c644cfd757c9cd83830a2da1624477b8824d3802807b1"
---

# Waton Financial Highlights AlphaSchema Research Progress in Structured AI-Assisted Trading Signal Discovery

HONG KONG, Aug. 10, 2026 (GLOBE NEWSWIRE) -- Waton Financial Limited (NASDAQ: WTF) ("Waton" or the "Company") today highlighted recent progress associated with AlphaSchema, a research framework for structured, large-language-model-assisted discovery of candidate quantitative trading signals. The work is described in a publicly available arXiv preprint and also disclosed by a public source-code repository covering the core research workflow.


The research reflects Waton’s long-term approach to AI trading, which approach is to turn model-generated ideas into explicit, reviewable hypotheses before implementation, and then test those hypotheses under defined data, validation and evaluation rules. Waton views the preprint as an early research milestone—not a product launch and not evidence of live trading performance.


The preprint was titled “AlphaSchema: Exploring the Space of Trading Semantics for LLM-Based Alpha Mining” and was authored by Jingyang Yi, Jian Yang, Yifei Jin, Yuqi Li and Jian Li. The authors’ listed affiliations include X-Tech, the Xtech-PandaAI-Waton Joint Lab, Monash University, PandaAI and the Institute for Interdisciplinary Information Sciences at Tsinghua University. These affiliations identify the authors’ research associations and do not imply institutional endorsement of Waton or its products.


Separately, in March 2026, Waton Securities International Limited ("WSI"), a Hong Kong subsidiary of Waton, announced a strategic cooperation agreement with Pandaai Quantum Global Pte. Limited and X-Tech (Beijing) Co., Ltd. to establish an AI and Fintech Joint Lab. Waton regards research collaboration, product development and regulated operating capabilities as complementary but distinct workstreams.


**Preprint:**[AlphaSchema on arXiv](https://www.globenewswire.com/Tracker?data=lPB6hYfkKF1wfnZNFG_cTi5zGHDw4mNRHBfUZdIiCWxq_zXQR3N07xoXxzzecS3mmza51HUV6rpX2vsTqIC04zDZVT1jOnPOyzrqGOR46oI=)


**Public source-code repository:**[AlphaSchema on GitHub](https://www.globenewswire.com/Tracker?data=lPB6hYfkKF1wfnZNFG_cTsJuUSDtxK2cKn3Ln5TjA3P5FG9HFLJeSbglIcPP0nXh2pdiN8VFEoBRTbpw_OItiGK7scby0KKZYzyz5itdIzSDX_p8_ZENPH7mbwQ6VTzP)


**Prior company announcement:**[March 2026 strategic cooperation announcement](https://www.globenewswire.com/Tracker?data=WtUfSNLfDWzPEtEzFVA8EoKt35vXr1LGi0VYHveELDBE7MqQzDKsWm6Al_SR2S9YuZsRCvNj-cZEJ2cwnTBfwpSpQEIXuQAxS45dWs40mkGFZXUHc3cvt0xyDuPaBZtNZBFifpHhepIIdaavO2QEOA==)


Making Candidate Ideas Explicit Before Code


The authors begin from a practical research problem: many LLM-based factor-mining approaches ask a model to invent an idea and implement it in code at the same time, making it harder to inspect what is being tested and why. AlphaSchema, instead, treats a semantic plan as the primary object of a search and delays code generation until a plan has been selected.


Each candidate plan is represented through five fields:


- **Event —** the market phenomenon being observed
- **Context —** the conditions under which that phenomenon occurs
- **Qualities —** optional confirmation, consistency or filtering criteria
- **Direction —** the expected trading interpretation, such as continuation or reversal
- **Output —** the numerical form of the resulting candidate signal


The search process combines broad exploration with surrogate-guided selection and local mutation of promising plans. Selected plans are translated into executable research code and checked against data contracts and leakage rules before evaluation. The workflow records the plan, generates code, validates status and results, and supports traceability and iteration.


This separation matters to Waton because practical AI-assisted trading research requires more than model output. It also requires reproducible data pipelines, evaluation discipline, risk controls, human review and governed paths from research to any potential production use.


What the Preprint Reports


According to the authors, the main experiments use historical Chinese equity data for the CSI 300 universe, with 2016–2020 data used for training, 2021–2022 data used for validation, and 2023–2025 data used as a held-out historical test period. The paper reports five independent discovery runs and comparisons with representative machine-learning methods, deep sequence models, factor-mining libraries and agent systems under its stated protocol.


Under that historical backtesting protocol, the authors report the strongest results on certain predictive and portfolio metrics and competitive results on others. They also report additional CSI 500 experiments using factors mined separately for that universe. These outcomes have not been independently validated, are based on a limited number of runs over a single historical test period, do not cover live execution or real-account results, and should not be read as proof of future returns or of results in other markets or periods.


The public repository includes the core factor-mining workflow, schema library, prompts, configuration examples, tests and a lightweight demo. Market data, private experiment histories and API credentials are not included; users must provide their own data, model endpoint and configuration. The repository makes important parts of the method inspectable, but it is not presented as a fully packaged production or research environment.


Why This Matters to Waton’s Long-Term AI Strategy


Waton views AI trading as a long-term challenge spanning research, data engineering, model engineering, risk controls, human oversight and regulated operations. The value of AlphaSchema for Waton lies less in any single backtest than in the design choice to make trading hypotheses explicit before code—making assumptions easier to inspect, test, reject and refine.


*“Our confidence in this strategy does not rest on a single backtest. It comes from sustained execution across research, validation, product workflows and regulated operations. AlphaSchema is an early research milestone: it structures candidate trading hypotheses before code generation and makes the research process easier to inspect and iterate. We are encouraged by the direction while remaining clear that historical testing is not live performance and that any production use would require further validation, controls and review.”* **— Zhou Kai, Chairman of the Board and Chief Technology Officer of Waton Financial Limited**


Within this broader roadmap, research collaboration, MoTA (Manager of Trading Agents, the Company’s native investment-team workbench) product development and WSI’s regulated operating infrastructure remain complementary but distinct. AlphaSchema is a distinct research framework and is not being announced as a current MoTA feature or a live trading system. Waton intends to evaluate whether insights from this work may inform future AI-assisted workflows, subject to further technical validation, product governance and applicable regulatory requirements.


Potential next research priorities include evaluation across market regimes and asset universes; sensitivity to data definitions, costs and model choices; reproducibility; robustness and failure analysis; and clearer human-oversight procedures. These are research objectives, not commitments to commercial deployment or a timetable.


Important Research Context


AlphaSchema is described in a preprint that has not completed peer review. All metrics discussed in the paper or this release are historical and hypothetical. They do not reflect the results of actual trading or any customer account and are subject to limitations, including data quality and revision, market impact, liquidity, transaction costs, implementation delay, model choice and changing market conditions. There can be no assurance that the results can be independently replicated or that the approach will prove commercially viable.


Nothing in this release is investment advice, research advice, or a recommendation regarding any security or trading strategy, and it should not be relied upon in making any investment decision. This press release does not constitute an offer to sell, or the solicitation of an offer to buy, any security. Waton does not warrant or guarantee the accuracy, completeness or reliability of any data, methodology or results described in the preprint or this press release. No assurance is given that AlphaSchema will be commercialized, integrated into MoTA or another product, achieve any particular outcome, or receive any required regulatory approval, and any such development remains subject to further validation, risk and compliance review, and applicable law.


About Waton Financial Limited


Waton Financial Limited is a holding company registered in the British Virgin Islands, conducting business primarily through its wholly owned subsidiaries in Hong Kong. Waton offers comprehensive financial services, including securities brokerage, asset management and software licensing to retail and institutional investors. Driven by technology and a customer-centric philosophy, Waton is committed to delivering innovative and reliable financial solutions. For more information, visit[www.wtf.us](https://www.globenewswire.com/Tracker?data=ClIlD9zC-ij4bQLpOIkCrPC981WFtK6VLF05oOu3NXqZp3NS53fT2ttuNgFEhonVnWYzuBQ2wlx58TkadiNi1g==) .


Forward-Looking Statements


Certain statements in this press release constitute "forward-looking statements" within the meaning of U.S. federal securities laws, including, but not limited to, statements regarding Waton’s AI strategy, research priorities, continued investment in research and development, the potential relevance or future application of AlphaSchema-related insights, the possibility or timing of any integration into MoTA or another product, the possibility or timing of any required regulatory approval, future AI-assisted workflows, and other plans, outcomes, results, estimates, objectives, expectations or timelines. These statements are based on the Company’s current expectations and projections about future events and involve known and unknown risks and uncertainties. Investors can identify forward-looking statements by words such as "believe," "plan," "expect," "intend," "should," "seek," "estimate," "will," "target" and similar expressions, although not all forward-looking statements contain these identifying words.


Actual results may differ materially from those expressed or implied due to numerous factors, including the reliance of any reported results on historical, backtested and hypothetical data; the risk that results are not replicated in out-of-sample periods, other markets or asset universes; the fact that the preprint has not completed peer review and may be revised or corrected; intellectual property, licensing and data-rights risks associated with collaborative research and publicly released source code; technology and model limitations; data and cybersecurity risks; regulatory developments and requirements applicable to the Company’s research, product development and operating activities in the jurisdictions in which it operates; commercialization and integration challenges ; market, economic and competitive conditions; and the other risks described in the Company’s filings with the U.S. Securities and Exchange Commission. Except as required by law, the Company undertakes no obligation to update any forward-looking statement. Investors are encouraged to review the Company’s registration statements and other SEC filings at[the SEC’s EDGAR website](https://www.globenewswire.com/Tracker?data=nyZdKmRyeMn-uxK_up8ho0z5xqOuIY0CS66zqe3AuiPimyH6JzQopUtCGY3RPi93EDFngFwLMp6QQermoVpzs5GGcLjHO5gPCkT8we-lg_CooB72moSaKJQw2ysja2EzuZQDbVwSg3WpvxSauksrlA==) .


Contacts


**Media Inquiries:**[pr@waton.com](https://www.globenewswire.com/Tracker?data=OZ3xihkr458DZeJI2NTO8ucZTebgPUqYEORLmTaq_PtfQpLinphFk56tu63giJX4GS-EPgLE4jPXFrS5PzlVvg==) **| Investor Relations:**[ir@waton.com](https://www.globenewswire.com/Tracker?data=cgxtPX0-1kqX1NtuV1IBCOueQRcPRPaDeGdIdu_zpLPAc46cuCpKQCRMt7Pti-NP8NIUBOgrJ1c4HzeB-bM5Zg==)
