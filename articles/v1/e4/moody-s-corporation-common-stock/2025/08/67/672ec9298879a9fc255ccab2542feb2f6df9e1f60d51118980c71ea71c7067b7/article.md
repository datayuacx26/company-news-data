---
schema_version: "1.0.0"
document_id: "672ec9298879a9fc255ccab2542feb2f6df9e1f60d51118980c71ea71c7067b7"
company_key: "moody-s-corporation-common-stock"
company: "Moody's Corporation"
source_id: "moody-s-corporation-common-stock-news-import-112754bd8765"
canonical_url: "https://www.moodys.com/web/en/us/creditview/blog/prompting-series-portfolio-monitoring.html"
published_at: "2025-08-28T16:14:00+00:00"
first_seen_at: "2026-07-22T04:52:53.039531+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:2de96ad4921802572fa6df73abebcf4d4111585af0d91f03b33665522b579887"
---

# Prompting Part 11: Portfolio Monitoring

To manage a portfolio effectively, you need the right tools and insights at your fingertips. Using Moody’s Research Assistant with our carefully crafted prompts, you can streamline portfolio monitoring, uncover key trends, and make data-driven decisions with confidence.


In this installment of our *Summer Prompting Series* , we’ll walk you through how to set up a portfolio on moodys.com and introduce a compilation of advanced prompts designed to simplify complex analyses. Whether you’re tracking credit quality, identifying high-growth companies, or organizing credit opinions, these prompts are tailored to save you time and deliver actionable insights.


### **Setting up a portfolio on moodys.com**


Before we dive in, let’s go over how to set up a portfolio on moodys.com.


Step 1: From the moodys.com homepage, navigate to the person/profile icon and click on "Monitoring". There, you'll be able to see the portfolios you have already created and create new ones.


Step 2: To create a new portfolio, click on "New Portfolio" and specify the type of portfolio:


a) Research Assistant currently supports Corporates, Financial Institutions, and Sovereign portfolios.


b) Add entities individually with identifiers (name, ISIN, LEI Moody's Org ID, Orbis ID, Stock Symbol) or upload a list of entities using our template file.


For more detailed instructions, visit our blog on **[how to get started with CreditView’s new monitoring experience](https://www.moodys.com/web/en/us/creditview/blog/how-to-get-started-with-CreditView-s-new-monitoring-experience.html)** .


**About Monitoring**


- **Portfolio limit:** Each portfolio can include up to 500 entities.
- **Alerts:** After creating a portfolio, set up custom alerts by clicking on the bell icon.
- **Monitoring features:** Access content for your selected issuers, including rating activity, ratings, outlooks, the latest Moody’s research and news, financial data, and more.
- **Integration with Advanced Query:** Portfolios created in “Monitoring” are integrated into the Advanced Query (AQ) workspace, which supports detailed analysis of your chosen issuers. For instance, when querying sectors, entities, or regions, use the @ symbol to reference portfolios.


Note: Portfolios are only query-able in Advanced Query and cannot be accessed via Chat.


### **Prompts to propel results forward**


Explore these tested and validated prompts — designed to deliver clarity, context, and confidence in just a few clicks.


**Effortlessly Access Organized Credit Opinions**


Advanced Query


Prompt: *Show me the last 5 credit opinions for each company in @\[PORTFOLIO\]. Organize by entity and most recent to oldest.*


Why it works: This prompt simplifies the process of gathering recent and historical credit opinions for each company in your portfolio. By organizing the data by entity and timeline, Research Assistant eliminates the hassle of navigating multiple pages or manually extracting documents. All relevant credit assessments are available in one convenient place, giving you easy access to up-to-date information.


Designed for professionals who need quick, organized access to multiple credit opinions across various companies, this prompt creates a centralized hub for credit assessments. It saves you time and effort, so you can focus on deeper analysis and smarter decision-making rather than tedious data collection.


**Spot High-Growth Companies in Your Portfolio**


Advanced Query


Prompt: *What companies in @\[PORTFOLIO\] had revenue YOY growth >20% in either 2023 or 2024. Include a column for revenue growth.*


Why it works: Similar to the examples we explored in "[Prompting with precision: Financial screening for credit analysts](https://www.moodys.com/web/en/us/creditview/blog/prompting-series-financial-screening-for-credit-analysts.html) ", Research Assistant also allows you to screen entities within a portfolio based on financials, ratings, and outlooks, and firmographic data.


You can uncover which companies in your portfolio are showing significant year-over-year revenue growth, helping you evaluate top-performing investments and understand what’s driving your portfolio’s success.


****


**Monitor Credit Quality Trends in Your Portfolio**


Advanced Query


Prompt: *Calculate the upgrades to downgrades ratio for companies in @\[PORTFOLIO\] for each of the last 3 years.*


Why it works: Tracking the upgrades-to-downgrades ratio for companies in your portfolio gives you valuable insights into how their credit quality has changed over time. By monitoring this trend, you can spot signs of improving financial health or potential risks tied to credit deterioration, helping you make more informed decisions.


****


**Top Tips for Portfolio Monitoring in AI Prompts with Moody’s Research Assistant**


When using AI-powered tools like Moody’s Research Assistant, well-crafted prompts can make a huge difference in how effectively you track portfolio performance, risk exposure, and emerging opportunities. Generic requests may return broad results, but specific, structured prompts deliver targeted insights you can use instantly.


Here are some practical ways to fine-tune your portfolio monitoring questions for maximum impact:


- **Specify the Type of Risk to Monitor**


Clearly define the risk category (credit risk, market risk, liquidity risk, etc.), so that the AI focuses on the most relevant factors.


For example: *"Analyze liquidity risk trends across my portfolio’s investment-grade bonds over the last 12 months."*


- **Use Comparative Queries to Spot Patterns**


Frame prompts to compare performance across sectors, geographies, or benchmarks to identify trends, outperformers, or underperformers.


For example: *"Compare default probabilities in my portfolio to industry benchmarks for the transportation and energy sectors."*


- **Incorporate Hypothetical Scenario Analysis**


Request insights based on potential market shifts or macroeconomic events to prepare for contingencies.


For example: *"Assess how a 75bps interest rate increase would impact the credit quality of my portfolio holdings in emerging markets."*


- **Focus on Key Metrics or Thresholds**


Highlight specific metrics like credit ratings, leverage ratios, or ESG scores, and set thresholds for flagging risks.


For example: *"Identify portfolio assets with credit ratings below BBB or ESG scores below 75."*


- **Request Sector-Specific Insights**


Zoom in on particular industries or sectors to address unique challenges or opportunities within those spaces.


For example: *"Analyze the risk exposure of renewable energy companies in Europe facing regulatory changes in carbon emissions reporting."*


- **Ask for Diversification and Concentration Analysis**


Monitor portfolio diversification to identify areas of over-concentration that might increase risk exposure.


For example: *"Evaluate sector diversification in my portfolio and highlight any areas with over 30% exposure to a single sector."*


- **Request Visual Summaries for Clarity**


Ask for charts, graphs, or summaries to visualize data and simplify complex risk or performance metrics.


For example: *"Provide a bar chart summarizing credit upgrades by sector for my portfolio over the last quarter."*


By strategically crafting prompts along these lines, you can use Moody’s Research Assistant to gain sharper, more actionable insights, so that your portfolio is optimized for performance and prepared for emerging risks.


**About the author:**


Caroline Hedgcock is an Assistant Director of Customer Success at Moody’s, where she specializes in helping clients optimize their use of Moody’s Research Assistant. A leader in AI prompting strategies, Caroline works closely with financial institutions to streamline workflows, accelerate analysis, and unlock deeper insights leveraging Moody’s GenAI-powered Research Assistant.


With extensive expertise in applying advanced prompting techniques, Caroline ensures that clients harness the full capabilities of the Research Assistant to enhance decision-making and operational efficiency. Her approach bridges technical innovation with practical application, helping organizations unlock the benefits of embedding GenAI into their daily workflows.
