---
schema_version: "1.0.0"
document_id: "69e018b52f6b80b662e95c6323cb44e5e8ae260df036cb545db72120cff94977"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-b1ffd1321f3f"
canonical_url: "https://www.microsoft.com/en-us/microsoft-365/blog/2026/06/25/copilot-in-excel-built-for-the-era-of-frontier-finance/"
published_at: "2026-06-25T13:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.173713+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:bad4a8ae99d02ef28b4e6675fab18f9bb28471230b821d8582fe28b10ffc89de"
---

# Copilot in Excel: Built for the era of Frontier Finance

There’s a pattern to how new technology reaches the world. Wave after wave, from the PC to the relational database to the cloud, new technology reaches developers first, and finance is often next. For finance professionals, a better tool is an edge and the job is to model reality a little more precisely than you could yesterday. For decades, that tool has been Excel: where the quarter gets closed and the forecast gets argued line by line, where every number traces back to a source.


So when AI enters finance, it has to clear the same bar: showing its work, using trusted data and tracing every calculation. Plenty of AI tools claim to be built for finance; Microsoft 365 Copilot in Excel is proving it in practice.


Across Financial Planning and Analysis (FP&A), Accounting, Tax, Compliance, and Treasury, Microsoft Finance runs Copilot in Excel in real workflows, spending less time hunting for information and rebuilding analyses, and freeing teams to spend time applying judgement to decisions. They shape it as much as they use it, telling us where it falls short and pushing the product toward the standard their own work demands. By the time it reaches you, it has already been pressure-tested by a finance organization operating at the frontier.


Today, we’re introducing new features built for financial professionals to continue doing that with **skills** for repeatable workflows, **new financial connectors** for trusted data, and **improved capabilities for traceability** .


## **Built for the complexity of finance work**


Before new Copilot capabilities ship in Excel, we evaluate them across graded levels of task complexity and benchmarks that reflect the work finance teams do every day, ensuring it can deliver multi-step workflow with a trusted and verifiable result rather than just complete a single task. For more on how we test,[get an inside look](https://aka.ms/AA11qxnc) at our evaluation framework, benchmarks, and domain-specific approach for finance.


Meeting the high bar of professional finance work isn’t something we’ve done alone. We’ve also partnered with[Financial Modeling Institute (FMI),](http://www.fminstitute.com/) **** the global body that credentials the **** industry’s most demanding modelers. Its library of real-world financial modeling cases has become a foundational part of how we evaluate Copilot in Excel for finance work.


## **Tuned to your standards**


Today, we’re introducing **skills** that let teams define how Copilot should complete common processes such as building a DCF, closing the books, refreshing a monthly reporting model, or preparing a variance analysis. Instead of starting from scratch each time, a skill guides Copilot through the steps, applying the right structure and formatting, and helping produce an output that is easier to review, reuse, and trust.


A library of sample finance skills is now available, and you can build your own custom skills using an open-standard markdown file. Save a SKILL.md file in your OneDrive, and Copilot will pick it up to build a three-statement model or a board package using the process you define for it. Learn more on how to create, use, and[manage skills here](https://go.microsoft.com/fwlink/?LinkId=2369926) .


Developers and partners can soon build and deploy skills through Microsoft Marketplace and Microsoft 365 Admin Center. We’ve already begun working with a first wave of partners, including finance and ERP solutions like **LSEG** , **Ramp** , **Rogo, Samaya AI, Velixo, and Vena** .


Copilot can also adapt to how you like to work in Excel. With[Personalization](https://support.microsoft.com/en-us/excel/copilot/copilot-in-excel-personalization) **,** you can set preferences once and have Copilot apply them consistently, while[workbook rules](https://support.microsoft.com/en-us/excel/copilot/copilot-in-excel-rules) capture structure, naming, and formula conventions as a sheet in the workbook that follows the file.


## **Grounded in data you trust**


Copilot in Excel connects directly to the data financial professionals rely on, bringing market data, fundamentals, and research directly into the workbook so analysis starts from the latest sources instead of manual data pulls. In addition to the[LSEG and Moody’s](https://techcommunity.microsoft.com/blog/excelblog/introducing-federated-copilot-connectors-for-lseg-and-moodys-in-excel/4523360) connectors we released in May, today we’re expanding your options with more financial data connectors for bringing public and private market data into Copilot in Excel.


- **CB Insights** brings predictive intelligence on private companies and markets into Excel. Teams use it to source promising companies, evaluate emerging markets, and support strategy, M&A, and corporate development workflows.


- **Daloopa** provides audit-ready fundamentals sourced from SEC filings, investor presentations, press releases, and other public company materials. Analysts use it to update operating models, build comp tables, complex financial analysis, and reduce manual data entry from filings.


- **FactSet** connects Excel workflows to financial and alternative data used by investment professionals, financial institutions, and corporations. Teams use it for modeling, screening, market analysis, and research workflows that rely on trusted institutional data.


- **Morningstar** brings investment research and data into Excel, including analyst research, ratings, and portfolio analytics. Investment teams use it to evaluate holdings, compare funds, analyze portfolios, and support asset allocation decisions.


- **PitchBook** brings institutional-grade private capital market intelligence, including company profiles, deal histories, fund data, and analyst research, directly into Excel. Teams use it to build target lists, support diligence workflows, and screen investments using trusted private market data and expert analyst research.


- **S&P Global – Deterministic Retrieval,** developed by Kensho, provides structured, API-driven access to S&P Global data for LLMs and agent-based systems. Teams use it for company research, financial analysis, transcript intelligence, and multi-entity comparisons, with predictable, cited results and full control over orchestration and execution.


*Note: *Third-party connectors and data providers may require separate licensing or subscriptions from the respective provider. Learn more[here](https://support.microsoft.com/en-US/excel/copilot/copilot-in-excel-data-sources) .* FactSet is in preview and will be generally available in July.*


## **Unlocking finance workflows**


Combined with Work IQ for grounding in work context, these features unlock real scenarios finance teams work on every day. A few example prompts you can now try with Work IQ, skills and connectors *(Note: external data mentioned in the below prompts may require one or more connectors)* .


- **Close the books:** Compare last quarter’s actuals to plan using internal forecast reviews and planning decks. Identify the five largest revenue, expense, margin, and cash flow variances, explain likely drivers, and draft an executive-ready business review summary with @variance-analysis.


- **Update the forecast:** Roll forward the current forecast using the latest approved assumptions and budgets from my team. Incorporate market data, reconcile changes against the current operating plan, and summarize the key drivers behind the updated outlook with @model-update.


- **Build the valuation model:** Use @comps-analysis to build a DCF, comparable company analysis, and sensitivity model for this company. Pull financial fundamentals, analyst expectations, market benchmarks, and transaction multiples to explain key valuation drivers.


- **Find the next acquisition:** Use @deal-screening to help identify acquisition candidates that match our internal strategy documents and acquisition criteria. Combine company performance and market signals with funding history and investor activity to evaluate, rank, and recommend the strongest opportunities.


- **Analyze portfolio performance:** Use @portfolio-monitoring to assess this portfolio’s performance against investment objectives, and internal investment committee materials. Pull fund analytics and ratings alongside market performance and risk data to identify concentration risks and recommend portfolio adjustments.


- **Stay ahead of earnings:** Use @catalyst-calendar to analyze earnings results, estimate revisions, analyst expectations, and management commentary for the companies in this list. Pull consensus forecasts, transcript intelligence, and market data to identify sentiment shifts and summarize the developments that matter most to investors.


## **Controllable by design**


In finance, the answer alone isn’t enough. You need to know how you got there. Whether updating a forecast, refreshing a board reporting model, or reviewing changes before quarter close, finance teams need the same things they’ve always demanded from Excel: visibility into what changed, confidence in the methodology, and a clear trail.


That’s why you can now choose to[Plan with Copilot](https://support.microsoft.com/en-us/excel/copilot/get-started-with-copilot-in-excel) **** before taking action, outlining which ranges, worksheets, formulas, and assumptions it intends to update along with clarifying questions. Once changes are made, every edit remains traceable, with links back to affected cells and **changes are now** **attributed to Copilot** alongside the work of collaborators in the Show Changes pane.


The result is a Copilot that works more like a trusted analyst: proposing a path forward, explaining its approach, and making every change transparent and reviewable.


## **Availability**


*Personalization, workbook rules, pre-built skills, federated Copilot connectors, Plan with Copilot, and Copilot attribution in Show Changes are generally available for Microsoft 365 Copilot customers across Excel for Web, Windows, and Mac.*


*Custom skills are available today via the Insiders channel for Windows and Mac, and rolling out to general availability across Excel for Web, Windows, and Mac next month.*


*Partner-built skills are coming in Q3 2026. Learn more[here](https://aka.ms/ExcelSkills) about building and deploying skills.*


*The features described in this post are rolling out progressively to Microsoft 365 Copilot customers. Specific availability, supported regions, and licensing requirements may vary.*
