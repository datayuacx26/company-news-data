---
schema_version: "1.0.0"
document_id: "6b83f13b5583ba30fa72cd5c7c00ca5d85a3f862bf43f1b9c37eb48936418c39"
company_key: "moody-s-corporation-common-stock"
company: "Moody's Corporation"
source_id: "moody-s-corporation-common-stock-news-import-112754bd8765"
canonical_url: "https://www.moodys.com/web/en/us/creditview/blog/prompting-series-financial-and-kpi-analysis.html"
published_at: "2025-07-31T16:14:00+00:00"
first_seen_at: "2026-07-22T04:52:53.039531+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:5d35609031e6f3c340928d82d759792628ca2ee747c74e86d738f2c591b2d550"
---

# Prompting Part 9: Financial & KPI Analysis

When analyzing a company’s financial performance, you need deep, actionable insights that highlight trends, uncover risks, and benchmark results. That’s where our financial and KPI analysis prompts come in — designed and tested by Moody’s specialists to deliver clarity, streamline reporting, and support confident, data-driven decisions.


In Part 9 of this series, we’ll showcase prompts that help you evaluate risks, track financial trends, and benchmark performance using our Advanced Query Workspace. Together, these prompts and this powerful AI platform can save you time, simplify complex workflows, and transform data into actionable insights with just a few clicks.


**Key benefits of Advanced Query Workspace**


When you need to go beyond just looking at key indicators — whether that means pulling additional financial data, running calculations, screening companies based on specific metrics, or retrieving key indicators for more than 10 companies (e.g., for an entire sector) — Advanced Query Workspace is your go-to tool.


### **Prompts that Deliver Results**


Explore these tested and validated prompts — designed to deliver clarity, context, and confidence in just a few clicks.


**Evaluate Risks and Generate Credit Insights**


*Advanced Query Workspace*


Prompt: Pull the last 3 years of Debt/Ebitda, Total Debt, Ebitda, Implied Rating, LT Rating, Notch difference, and outlook for \[ORG\] & peers.


Why it works: As a credit analyst, this prompt helps you evaluate a company’s ability to manage debt, monitor leverage ratios, and assess financial risk. It also supports you in preparing clear, data-driven reports for loan approvals. By analyzing and comparing implied credit ratings with long-term ratings, you can determine each company’s stability and future outlook.


Likewise, understanding the differences between Long Term, Through the Cycle (TTC) ratings and Point-in-Time (PIT) Implied ratings* can help you assess potential risks and predict possible rating changes. Analyzing financial data over multiple years and highlighting percentage differences makes it easier to quickly generate comprehensive credit reports.


* *An implied rating is derived from Moody's probability of default model (EDFX) and market data rather than directly assigned by a credit rating agency. These ratings are aligned with Moody's senior ratings scale and are directly comparable. Unlike traditional long-term (LT) ratings, implied ratings are based solely on the probability of default level, without incorporating the broader range of quantitative and qualitative factors used in analyst-driven ratings. However, because they are point-in-time (PIT), implied ratings can quickly respond to changes in company risks and shifts in the credit cycle. Additionally, they remain consistent and comparable across different regions and apply to both public and private companies.*


Integration into workflow: Use Advanced Query Workspace to understand what's driving leverage (an increase or decrease in debt or EBITDA) for multiple entities simultaneously.


****


**Analyze Performance and Benchmark Medians**


*Advanced Query Workspace*


Prompt: Calculate the median 2024 adjusted key indicators for the @\[SECTOR\] sector. Include in a column @\[ORG\] and their peers 2024 FY adjusted key indicators.


Alternatively...


Calculate the median 2024 adjusted key indicators for the @\[SECTOR\] sector. Then, identify entities whose debt/EBITDA is at least 10% above the sector median.


Why it works: As an equity analyst, this prompt helps you calculate sector medians for key indicators and compare individual companies to these benchmarks, making it easy to identify which ones are outperforming or underperforming.


You can efficiently screen companies based on their performance compared to sector medians, using specific percentages to make smarter buy or sell recommendations.


Integration into Workflow: This prompt streamlines your processes with automated year-over-year calculations and seamless data export to Excel, helping you save time on manual tasks and focus on meaningful analysis.


****


**Monitor Trends and Compare Financial Changes**


*Advanced Query Workspace*


Prompt: Pull @\[ORG\] Motor Company's adjusted financials over last 5 years and calculate # and % difference over each period.


Why it works: As a credit analyst or financial planning and analysis manager, you can use this prompt to quickly access five years of financial data with calculated percentage differences, making it easier to identify trends across leverage, debt, and EBITDA — and to develop effective long-term financial strategies.


Integration into Workflow: Effortlessly gather reported and adjusted numbers alongside year-over-year differences in one consolidated view. Integrating this prompt into your workflows streamlines the reporting process and delivers clear, actionable insights to senior management.


### **How is this generated?**


Responses within the Advanced Query Workspace will typically include a **"How is this generated?"** section. This is designed to give you an easy-to-understand explanation of how the data was pulled — essentially breaking down the SQL logic in plain language. Think of it as your built-in guide to help you check and validate the results. While it doesn’t access the actual content of reports, it serves as a helpful tool for understanding and verifying how the data was retrieved — essentially acting as a "data assistant."


### **Top Tips for Using Context to Improve AI Prompt Responses with Moody’s Research Assistant**


To get the most out of Moody’s Research Assistant for your financial analysis, crafting prompts with the right level of detail and clarity is key. By providing strong context, you can drive more specific, insightful responses that streamline your workflows. Below are some of our top tips for using context to improve the responses you get from your prompts:


- **Be Specific** : Clearly outline your objective and include relevant financial terms (e.g., EBITDA, leverage ratios, revenue growth) to make sure the AI assistant understands your focus.


For example, instead of asking for "key metrics," specify "the EBITDA margin trends for the past three years."


- **Provide Data Context** : Reference important details like timeframes, industries, geographic regions, or specific financial benchmarks.


For example, "Summarize the debt-to-equity ratio trends for U.S. retail companies from 2020 to 2023" gives the assistant a clearer scope to work within.


- **Structure Your Request** : Use clear, concise language and break down complex queries into smaller parts to make them more digestible.


For example, instead of asking, "Analyze profitability and risk," try splitting it into "Summarize net profit margins over the last five years" and "Highlight any significant changes in credit ratings."


- **State Desired Format** : Specify how you want the output structured (e.g., tables, bullet points, narratives) to save time and effort when integrating it into reports.


For instance, "Provide a summary in bullet points comparing revenue and operating margins year-over-year" or "Create a table showing quarterly KPI trends."


- **Refine and Iterate** : If the response isn’t perfect, tweak your prompt by adding more context or clarifying expectations.


For example, if results are too broad, you might write, "Focus on small-cap energy companies in North America" or "Exclude one-time adjustments in the financial metrics."


By following these tips, you can unlock the full potential of Moody’s Research Assistant, with faster, more exact analysis that delivers actionable insights to support strategic decision-making.


Stay tuned for our next installment, where we’ll explore prompts for peer comparison.


**About the author:**


Caroline Hedgcock is an Assistant Director of Customer Success at Moody’s, where she specializes in helping clients optimize their use of Moody’s Research Assistant. A leader in AI prompting strategies, Caroline works closely with financial institutions to streamline workflows, accelerate analysis, and unlock deeper insights leveraging Moody’s GenAI-powered Research Assistant.


With extensive expertise in applying advanced prompting techniques, Caroline ensures that clients harness the full capabilities of the Research Assistant to enhance decision-making and operational efficiency. Her approach bridges technical innovation with practical application, helping organizations unlock the benefits of embedding GenAI into their daily workflows.
